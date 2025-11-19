---
title: Dialect, TableGen and Customized Operation 
subtitle: 

# Summary for listings and search engines
summary: Operation is the foundation

# Link this post with a project
projects: []

# Date published
date: "2025-11-19T00:00:00Z"

# Date updated
lastmod: "2025-11-19T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**MLIR**](https://llvm.org/devmtg/2020-09/slides/MLIR_Tutorial.pdf)'
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- admin

tags:
- Notes

categories:
- Notes
---

## Dialect

MLIR 被设计成完全可扩展的基础框架，没有封闭的属性集、操作和类型。MLIR 通过[Dialect, https://mlir.llvm.org/docs/LangRef/#dialects](https://mlir.llvm.org/docs/LangRef/#dialects)的概念来支持这种可扩展性。Dialect在一个特定的namespace下为抽象提供了分组机制。在Dialect里面，Operation是抽象和计算的核心单元，在许多方面与 LLVM 指定类似。具有特定于应用程序的语义，并且可以用于表示 LLVM 中的所有核心的 IR 结构：指令、globals（类似function）和模块。在这个博客中，将从dialect中的operation讲起，从tablegen文件到具体实现，通过一个自定义dialect **Jiajun**来进行展开笔记。

让我们先看一段MLIR代码：

``` java
module {
  func.func @main() -> tensor<2x2xf32> {
    // 使用jiajun.constant创建常量张量
    %c1 = arith.constant dense<[[1.0, 2.0], [3.0, 4.0]]> : tensor<2x2xf32>
    %c2 = arith.constant dense<[[5.0, 6.0], [7.0, 8.0]]> : tensor<2x2xf32>
    %c3 = arith.constant dense<2.0> : tensor<2x2xf32>

    // 使用jiajun.add进行张量加法
    %sum = jiajun.add %c1, %c2 : tensor<2x2xf32> -> tensor<2x2xf32>
    
    // 使用jiajun.mul进行张量乘法（带缩放因子）
    %result = jiajun.mul %sum, %c3 scale 1.5 : tensor<2x2xf32> -> tensor<2x2xf32>

    // 返回最终结果
    return %result : tensor<2x2xf32>
  }
}
```

可以先不用关注具体的语法，而只对dialect有个概念。这里的`arith`和`jiajun`就是两种不同的dialect。可以看出，在一个MLIR程序中，**可以存在不同的Dialect**，这符合MLIR对于dialect赋予的意义，即不用level的dialect可以用于不同层次的优化。`Dialect.Operation`就是对operation的调用方法，例如`jiajun.add`意为调用Jiajun Dialect中的add operation进行运算。

## TableGen：C++代码生成

TableGen是LLVM生态系统中的一个重要工具，它在MLIR中扮演着关键角色。TableGen的核心价值在于自动化代码生成，通过声明式的方式描述操作、类型和属性，然后自动生成大量的样板代码。这种设计带来了几个显著优势：

**减少重复代码**：手动编写每个操作的验证器、打印机、解析器等会产生大量重复代码，TableGen可以自动生成这些基础设施代码。

**保证一致性**：通过集中定义操作规范，确保不同操作之间的接口和行为保持一致。

**易于维护**：当需要修改操作接口时，只需在TableGen定义中修改一处，所有相关代码都会自动更新。

**类型安全**：TableGen在编译时检查类型约束，减少运行时错误。

对于硬件加速器开发者来说，TableGen特别有价值，因为它允许快速定义硬件特定的操作和类型，而无需担心底层基础设施的实现细节。而本章博客最重要的基本概念Dialect，就将在tablegen文件（.td）中进行定义和声明。注意，对Dialect和Operation的定义大多是通过TabelGen规范构造的，通过TableGen驱动MLIR的Operation定义也被称作ODS（ Operation Definition Specification）。本章会通过一些tablegen代码示例来讲述其语法和作用，而更详细的ODS用法总结将在之后的博客找时间总结。

## 定义Jiajun Dialect

首先创建一个基础的Dialect定义。在MLIR项目中，Dialect是相关操作、类型和属性的逻辑分组。我们的Jiajun Dialect将专门为AI加速器设计。

在`JiajunDialect.td`文件中定义Dialect：
``` java
 include "mlir/IR/OpBase.td"
 include "mlir/IR/DialectBase.td"

 def Jiajun_Dialect : Dialect {
   let name = "jiajun";
   let summary = "Jiajun AI Accelerator Dialect";
   let description = "This dialect contains operations for the Jiajun AI accelerator";
   let cppNamespace = "::jiajun";
   let useDefaultAttributePrinterParser = 1;
   let useDefaultTypePrinterParser = 1;
 }
```

这个定义包含了Dialect的基本信息：名称、描述和C++命名空间。`useDefaultAttributePrinterParser`和`useDefaultTypePrinterParser`启用了默认的打印和解析功能。在接下来的几个博客中，我会进一步展开讲这些信息，本章只会简单描述大体风格。

## 定义基础操作：mul和add

接下来定义两个基础操作：乘法和加法。在`JiajunOps.td`文件中：
```java
include "JiajunDialect.td"
include "mlir/IR/OpBase.td"

class Jiajun_Op<string mnemonic, list<Trait> traits = []> :
    Op<Jiajun_Dialect, mnemonic, traits>;

def Jiajun_MulOp : Jiajun_Op<"mul"> {
    let summary = "Multiplication operation for Jiajun accelerator";
    let description = "This operation performs element-wise multiplication on two tensors";

    let arguments = (ins 
        TensorOf<[F32]>:$lhs,
        TensorOf<[F32]>:$rhs
    );

    let results = (outs TensorOf<[F32]>:$result);

    let assemblyFormat = "$lhs `,` $rhs attr-dict `:` type($lhs) `->` type($result)";
}

def Jiajun_AddOp : Jiajun_Op<"add"> {
    let summary = "Addition operation for Jiajun accelerator";
    let description = "This operation performs element-wise addition on two tensors";

    let arguments = (ins 
        TensorOf<[F32]>:$lhs,
        TensorOf<[F32]>:$rhs
    );

    let results = (outs TensorOf<[F32]>:$result);

    let assemblyFormat = "$lhs `,` $rhs attr-dict `:` type($lhs) `->` type($result)";
}
```

让我们详细解析这些定义：

**Operation Base Class 操作基类**：`Jiajun_Op`是一个辅助基类，所有Jiajun Dialect的操作都继承自它，确保一致的命名和traits设置。这里的*mnemonic*（助记符）`mul`可以简单理解为后续继承这个基类产生的具体operation的名字。例如在之前的MLIR代码中，**我们使用的是`jiajun.mul`而不是`jiajun.Jiajun_Op`**。

**参数定义**：`arguments`部分定义了操作的输入。这里使用`TensorOf<[F32]>` **Constraint(约束)**，表示只接受浮点32位张量类型。`$lhs`和`$rhs`是参数的符号名称。之后的ODS用法总结博客会更详细地讲约束语法。

**结果定义**：`results`部分定义了操作的输出。同样约束为浮点32位张量。

**汇编格式**：`assemblyFormat`定义了操作在文本格式中的表示方式。这个格式字符串指导MLIR如何**Print(打印)和Parse(解析)**操作，这两个方法也是后续博客会详细展开的，比较重要。

## Builder

定义了Operation之后，我们怎么构建实例呢？ 每一个Operation，都会基于Operation的参数和Operation的返回值自动生成一些builers，即TableGen可以自动生成默认的builder。例如，如果我们有一个operation：

```java
def MyOp : ... {
    let arguments = (ins
        I32:$i32_operand,
        F32:$f32_operand,
        ...,

        I32Attr:$i32_attr,
        F32Attr:$f32_attr,
        ...
    );

    let results = (outs
        I32:$i32_result,
        F32:$f32_result,
        ...
    );
}
```

下面的builders会通过tablegen自动产生：
```cpp
// All result-types/operands/attributes have one aggregate parameter.
static void build(OpBuilder &odsBuilder, OperationState &odsState,
                  ArrayRef<Type> resultTypes,
                  ValueRange operands,
                  ArrayRef<NamedAttribute> attributes);

// Each result-type/operand/attribute has a separate parameter. The parameters
// for attributes are of mlir::Attribute types.
static void build(OpBuilder &odsBuilder, OperationState &odsState,
                  Type i32_result, Type f32_result, ...,
                  Value i32_operand, Value f32_operand, ...,
                  IntegerAttr i32_attr, FloatAttr f32_attr, ...);

// Each result-type/operand/attribute has a separate parameter. The parameters
// for attributes are raw values unwrapped with mlir::Attribute instances.
// (Note that this builder will not always be generated. See the following
// explanation for more details.)
static void build(OpBuilder &odsBuilder, OperationState &odsState,
                  Type i32_result, Type f32_result, ...,
                  Value i32_operand, Value f32_operand, ...,
                  APInt i32_attr, StringRef f32_attr, ...);

// Each operand/attribute has a separate parameter but result type is aggregate.
static void build(OpBuilder &odsBuilder, OperationState &odsState,
                  ArrayRef<Type> resultTypes,
                  Value i32_operand, Value f32_operand, ...,
                  IntegerAttr i32_attr, FloatAttr f32_attr, ...);

// All operands/attributes have aggregate parameters.
// Generated if return type can be inferred.
static void build(OpBuilder &odsBuilder, OperationState &odsState,
                  ValueRange operands, ArrayRef<NamedAttribute> attributes);

// (And manually specified builders depending on the specific op.)
```

上面的代码注释翻译已经解释了这些builder的不同之处。并且可能还存在一些其它的builder，请参考(https://mlir.llvm.org/docs/OpDefinitions/#run-mlir-tblgen-to-see-the-generated-content) 这里的文档进行查看。这里感谢[GiantPandaLLM](https://mp.weixin.qq.com/s?__biz=MzA4MjY4NTk0NQ==&mid=2247499828&idx=1&sn=d7ed4550bcf76e2267ac5a1eb9d5154e&chksm=9f837aa2a8f4f3b4bb63531c1ce3f545caedef3b2b3c4b5efc1dc87b50ee91211c02e7d6195f&scene=178&cur_album_id=2099721001268740096&search_click_id=#rd)的教程提供了这个例子。

但对于复杂场景，我们需要自定义builder。这在AI加速器开发中特别重要，因为硬件操作通常有特定的参数要求。让我们为乘法操作添加自定义builder：
``` java
def Jiajun_MulOp : Jiajun_Op<"mul"> {
    let summary = "Multiplication operation for Jiajun accelerator";
    let description = "This operation performs element-wise multiplication on two tensors";

    let arguments = (ins 
        TensorOf<[F32]>:$lhs,
        TensorOf<[F32]>:$rhs,
        OptionalAttr<F32Attr>:$scale
    );

    let results = (outs TensorOf<[F32]>:$result);

    let assemblyFormat = ...

    let builders = [
        OpBuilder<(ins "Value":$lhs, "Value":$rhs)>,
        OpBuilder<(ins "Value":$lhs, "Value":$rhs, "float":$scale)>
    ];
}
```

这里添加了一个可选的缩放因子属性`$scale`，即，我们定义的这个乘法操作可以在`$lhs`和`$rhs`相乘之后再通过这个缩放因子进行进一步的乘法。由此，定义了两个不同builder：
- 第一个builder只接受两个输入值
- 第二个builder额外接受一个浮点数缩放因子

如果读者对C++熟悉的话，可以把它联想理解为C++ class中不同的**constructor(构造函数)**，且由于基类不会定义builder（均在子类的具体operation里声明+实现），这个方法也不存在重写(Override)与重载(Overload)。

可见，如果我们用tablegen自动生成的build()方法，将无法使用这个缩放因子进行初始化。读者可能会问这里的`OptionalAttr<F32Attr>`是指什么样的attribute，博客将在之后详细展开对于attribute（属性）的理解。

这样定义的operation，由tablegen生成代码后会产生这样的声明：

```cpp
class Jiajun_MulOp : /*...*/ {
  /*...*/
    static void build(::mlir::OpBuilder &builder, ::mlir::OperationState &state, ::mlir::Value lhs,  ::mlir::Value rhs, float scale);
    static void build(::mlir::OpBuilder &builder, ::mlir::OperationState &state, ::mlir::Value lhs,  ::mlir::Value rhs);
};
```

这里的两个重要参数是Opbuilder和OperationState:

- OpBuilder &builder: MLIR构建器，提供上下文和创建方法。
- OperationState &state: 操作构建状态，收集所有构建信息（operands操作数、Attribute属性，Type类型等）。关于属性和类型会在后续的博客中进一步展开介绍。

如果我们只要简单的builder，那这两个参数可以不用深入研究，下文会提到自定义*复杂*构建方式，就需要理解这两个参数到底是什么意义。

在C++代码中，这些builder可以这样使用：

```cpp
OpBuilder builder(ctx);
// 使用第一个builder
Value result1 = builder.create<Jiajun::MulOp>(loc, lhs, rhs);

// 使用第二个builder（带缩放因子）
Value result2 = builder.create<Jiajun::MulOp>(loc, lhs, rhs, 2.5f);
```

`builder.create`方法会自动调用operation类里的build()方法，加入OpBuilder和OperationState。这样，我们就在C++中构造了Jiajun dialect下的MulOp这个operation。

## builder的具体定义：Tablegen 还是 C++？

细心的读者会发现，在上文我们定义声明的`OpBuilder<(ins "Value":$lhs, "Value":$rhs, "float":$scale)>`并没有具体的定义实现，如果我们在TableGen中使用了OpBuilder，那么MLIR会自动生成一个build()定义。那么，如果我们显式地要求自定义实现，该如何做呢？一般来说，实现具体的builder有两种方式，一种是放在tablegen里用ODS语法实现，另一种则是在cpp文件里实现。

### 1> C++实现

如果我们默认它只是*简单*创建了一个operation，然后把lhs，rhs和scale（attribute）作为参数传入，并推断一下结果类型，那么我们可以显式地写出这样的C++代码完成build()实现：

```cpp
void Jiajun_MulOp::build(::mlir::OpBuilder &builder, ::mlir::OperationState &state,
                        ::mlir::Value lhs, ::mlir::Value rhs, float scale) {
  // 添加操作数
  state.addOperands(lhs);
  state.addOperands(rhs);
  
  // 添加缩放因子属性
  auto scaleAttr = builder.getF32FloatAttr(scale);
  state.addAttribute("scale", scaleAttr);
  
  // 推断结果类型与输入操作数类型相同
  state.addTypes(lhs.getType());
}
```

回顾一下OperationState，它的意义在于在一个operation中操作构建状态，收集所有构建信息（operands操作数、Attribute属性，Type类型等）。

### 2> Tablegen实现

当然，如此简单的build我们甚至可以直接用ODS语法，让tablegen帮我们自动生成：

``` java
    ...
    OpBuilder<(ins "Value":$lhs, "Value":$rhs, "FloatAttr":$scale),
              [{ 
                // 推断结果类型与输入相同
                $_state.addTypes(lhs.getType());
              }]>
    ...
```

在ODS语法中，[{...}]内的语句表示C++内容，即我们可以在tablegen文件中通过[{...}]代码块来实现简单的C++的语句。$_builder和$_state这两个特殊参数等效于builder和state。ins部分中的参数可以被直接使用，比如val。builer的c++代码实现会通过替换ODS中的特殊变量来完成，要保证builder ODS实现的其他部分是有效的C++结构。如果在这个代码块中要使用别的C++文件定义的函数或者方法，那么也需要在tablegen的开头include对应的头文件。

虽然对代码大小没有限制，**但我们鼓励只在ODS中内联较短定义的builder，而将定义较长（或者逻辑较复杂）的builder的定义放在C++文件中。**

## 使用extraClassDeclaration添加自定义方法

`extraClassDeclaration`允许在生成的C++类中添加自定义方法和成员。这对于实现硬件特定的功能非常有用。

例如，为加法操作添加自定义方法：
```java
def Jiajun_AddOp : Jiajun_Op<"add"> {
    let summary = "Addition operation for Jiajun accelerator";
    let description = "This operation performs element-wise addition on two tensors";

    let arguments = (ins 
        TensorOf<[F32]>:$lhs,
        TensorOf<[F32]>:$rhs,
        OptionalAttr<BoolAttr>:$in_place
    );

    let results = (outs TensorOf<[F32]>:$result);

    let assemblyFormat = "$lhs `,` $rhs (`in_place` $in_place^)? attr-dict `:` type($lhs) `->` type($result)";

    let extraClassDeclaration = [{
        /// 检查是否可以原地执行操作
        bool canExecuteInPlace() {
            return getInPlace().has_value() && getInPlace().value();
        }
        
        /// 获取操作的硬件指令编码
        std::vector<uint8_t> getHardwareEncoding() {
        // 硬件特定的编码逻辑
            return {0x01, 0x02, 0x03}; // 示例编码
        }
        
        /// 估算操作在目标硬件上的执行周期
        unsigned estimateCycles() {
            auto shape = getLhs().getType().cast<ShapedType>().getShape();
            unsigned elements = 1;
            for (auto dim : shape) elements *= dim;
            return elements / 16 + 10; // 简化的估算模型
        }
    }];
}
```
这些自定义方法为硬件加速器开发提供了重要功能：

**执行模式检查**：`canExecuteInPlace`检查操作是否可以在原地执行，这对于内存受限的加速器很重要。

**硬件编码**：`getHardwareEncoding`生成硬件特定的指令编码。

**性能估算**：`estimateCycles`提供操作的执行周期估算，用于调度和优化。

请注意，在`extraClassDeclaration`中自定义的方法会放在这个operation类里作为声明供其他C++代码使用，且tablegen中该`extraClassDeclaration`的其他部分也可以直接调用它们来使用。和build实现相同，虽然对代码大小没有限制，**但我们鼓励只在ODS中内联较短定义的extraClassDeclaration，而将定义较长（或者逻辑较复杂）方法的定义放在C++文件中。**

## Dialect Operation的实现与注册（在C++中）

正如前文所说，TableGen生成的是声明，我们还需要提供部分实现。在`JiajunDialect.cpp`中：
```cpp
#include "Jiajun/JiajunDialect.h"
#include "Jiajun/JiajunOps.h"

using namespace mlir;
using namespace jiajun;

#include "Jiajun/JiajunDialect.cpp.inc"
#include "Jiajun/JiajunOps.cpp.inc"

void JiajunDialect::initialize() {
    addOperations<
        #define GET_OP_LIST
        #include "Jiajun/JiajunOps.cpp.inc"
>();
}

void Jiajun_MulOp::build(::mlir::OpBuilder &builder, ::mlir::OperationState &state,
                    ::mlir::Value lhs, ::mlir::Value rhs, float scale) {
    // 添加操作数
    state.addOperands(lhs);
    state.addOperands(rhs);

    // 添加缩放因子属性
    auto scaleAttr = builder.getF32FloatAttr(scale);
    state.addAttribute("scale", scaleAttr);

    // 推断结果类型与输入操作数类型相同
    state.addTypes(lhs.getType());
}

...

```
这里的build实现已经在前面的章节说明。

对于复杂的操作验证，可以在C++文件中添加验证(verify)逻辑：
```cpp
LogicalResult Jiajun::MulOp::verify() {
    auto lhsType = getLhs().getType().cast<ShapedType>();
    auto rhsType = getRhs().getType().cast<ShapedType>();

    if (lhsType.getShape() != rhsType.getShape()) {
        return emitOpError("operand shapes must match");
    }

    if (getScale().has_value()) {
        float scale = getScale().value();
        if (scale <= 0.0f) {
        return emitOpError("scale factor must be positive");
        }
    }

    return success();
}
```
注意，实现验证的前提是在tablegen的定义中要添加`let hasVerifier = 1;`信息，例如：

``` java
def Jiajun_MulOp : Jiajun_Op<"mul"> {
    let summary = "Multiplication operation for Jiajun accelerator";
    let description = "This operation performs element-wise multiplication on two tensors";

    let arguments = (ins 
        TensorOf<[F32]>:$lhs,
        TensorOf<[F32]>:$rhs,
        OptionalAttr<F32Attr>:$scale
    );

    let results = (outs TensorOf<[F32]>:$result);

    let hasVerifier = 1;

    let assemblyFormat = ...

    let builders = [
        OpBuilder<(ins "Value":$lhs, "Value":$rhs)>,
        OpBuilder<(ins "Value":$lhs, "Value":$rhs, "float":$scale)>
    ];
}
```

## 使用示例

定义完操作后，可以在MLIR转换中使用它们，就像这篇博客开头展示的那样。
```java
module {
    func.func @test_jiajun_ops(%arg0: tensor<128x128xf32>, %arg1: tensor<128x128xf32>) -> tensor<128x128xf32> {
        %0 = jiajun.add %arg0, %arg1 : tensor<128x128xf32> -> tensor<128x128xf32>
        %1 = jiajun.mul %0, %arg1 scale 2.5 : tensor<128x128xf32> -> tensor<128x128xf32>
        return %1 : tensor<128x128xf32>
    }
}
```

## 总结

通过TableGen定义自定义Dialect和Operation，硬件开发者可以快速构建针对特定AI加速器的MLIR基础设施。这种方法的优势在于：

**开发效率**：自动生成大量样板代码，专注于硬件特定逻辑。

**可维护性**：集中定义操作规范，修改时自动更新所有相关代码。

**类型安全**：编译时类型检查减少运行时错误。

**可扩展性**：通过自定义builder和extraClassDeclaration支持复杂的硬件特定需求。

读者也许注意到我在这篇博客跳过了一个很重要的概念：**Attribute属性**。下一个博客我将详细介绍它，以及它所对应的另一个更重要的概念，**Constraint约束**。此外，基于operation，在后续的博客中，我们将探讨如何为这些自定义操作实现优化Pass，包括Canonicalizer模式匹配和硬件特定的优化转换。

## 6. Reference

- [MLIR Tutorial](https://mlir.llvm.org/docs/Tutorials/)
- [Toy Tutorial](https://mlir.llvm.org/docs/Tutorials/Toy/)