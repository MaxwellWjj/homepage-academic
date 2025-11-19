---
title: MLIR Core Concepts, A Practical Introduction
subtitle: 

# Summary for listings and search engines
summary: MLIR basic concepts.

# Link this post with a project
projects: []

# Date published
date: "2025-11-18T00:00:00Z"

# Date updated
lastmod: "2025-11-18T00:00:00Z"

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

## MLIR核心概念

IR（中间表示，Intermediate Representation）是一种中介数据格式，用于简化模型在不同框架之间的转换。在深度学习领域，任何能够表示计算图结构的数据格式均可视为一种IR，例如ONNX、TorchScript、TVM Relay等。由于本项目主要围绕ONNX-MLIR展开，后续内容将聚焦于ONNX这一框架。

ONNX是由微软提出的一种IR，它定义了一套标准化的算子格式。无论使用PyTorch、TensorFlow还是OneFlow等深度学习框架，均可将计算图转换为ONNX格式进行存储。各类部署框架只需支持ONNX模型格式，即可实现跨框架模型的便捷部署，从而显著降低了不同框架间模型互转的复杂性。

然而，ONNX在设计时未充分考虑不同框架中算子功能与实现方式的差异。由于难以覆盖所有框架及版本的算子实现，ONNX目前已累积了十余个版本的算子定义，给用户带来较大困扰。可以类比的是，IR在计算图中的作用类似于计算机体系结构中的指令集，而指令集的频繁变动通常是难以接受的。此外，ONNX虽然提供了如If之类的控制流算子，但支持仍较为有限。从广义上看，只要一种中介数据格式能够表示由算子和数据构成的深度学习模型，即可被视为IR。

当前，深度学习领域存在众多IR，很难由单一IR统一其他表示，这种“百花齐放”的局面也带来了一些挑战。以TensorFlow Graph为例，它可以直接转换为TensorRT IR、nGraph IR、CoreML IR或TensorFlow Lite IR进行部署；也可以先转换为XLA HLO，通过XLA编译器完成图级优化，再将优化后的XLA HLO交由后端进行硬件相关的优化和代码生成。在此过程中，主要存在以下问题：

- 第一，IR数量过多，而开源社区需要维护多套不同的IR。每种IR都具备各自的图优化Pass，尽管某些Pass功能相似，却无法直接跨IR复用。如果假设有10种通用的图优化Pass，为每一种IR都实现一遍，工作量将十分巨大。

- 第二，当出现新的IR时，若希望将其他IR中的优化Pass迁移过来，常因语法表示的差异难以直接复用，只能借鉴其思路，迁移成本较高。此外，为某个IR新增Pass也具有一定挑战。例如，为ONNX添加图优化Pass不仅需要深入理解相关机制，甚至可能需要系统学习ONNX的源码。

- 第三，在上述流程中，优化后的XLA HLO直接输入至XLA后端，转化为LLVM IR并最终生成代码，这一跨度较大。举例来说，从编写基础的三重循环矩阵乘法，到使用汇编语言实现高度优化的版本，中间缺乏循序渐进的过渡，导致初学者难以参与。即便理清了如TVM中Codegen的调用流程，要独立实现完整的代码生成过程仍然具有较高难度。

为解决上述问题，MLIR（多级中间表示）应运而生。MLIR是由LLVM团队开发和维护的编译器基础设施，强调工具链的可重用性与可扩展性。具体而言：

针对前两个问题，MLIR引入Dialect机制作为统一的表示方式。各种IR可以学习并转换为基于Dialect的MLIR形式，从而被置于同一命名空间下。每个IR在MLIR中可定义其对应的产生式和操作，从而促进优化Pass在不同IR间的复用。

针对IR之间跨度大的问题，MLIR通过多级Dialect抽象实现渐进式 lowering。例如，llvm dialect对应LLVM IR的抽象，tensor dialect则封装了张量操作。源程序可经过多轮 lowering，逐步转换为更低级的表示，最终生成目标代码。这一过程类似于分阶段学习，降低了直接从高级IR跨至低级表示的难度。例如，若不熟悉LLVM IR之后的层次，可将该部分交由LLVM处理，开发者仅需参与前端的Dialect实现。对于从ONNX起步的AI编译流程，甚至可以直接基于ONNX IR进行自定义扩展与设计。

## 0. 为什么在AI Accelerator中使用MLIR？

当前网络资源中几乎所有的教程和博客都以**Software**的视角来看待MLIR以及对应的编译链，在此我想简单地从**硬件开发者**的视角来谈谈为什么在AI accelerator里我们需要一个标准化的多级IR编译链（即MLIR），因为我本人就是做硬件设计和architecture起步的，在compiler这个领域对下游的理解要深于上游。从硬件设计者的角度出发，将一个复杂多变的深度学习模型映射到AI Accelerator的runtime是一个极具挑战性的任务。这涉及到多个层次的优化空间，每个空间都需要精细的设计和权衡。

### 面临的优化挑战

1. **存储受限下的数据切分**
   - 片上存储空间有限，需要智能地切割data tile以适应存储约束
   - 示例：对于卷积运算，需要确定最优的tile大小来平衡计算效率和存储使用

2. **数据流设计**
   - 在确定tile切分后，需要设计对应的dataflow来提升运行效率
   - 关键考虑：数据重用模式、减少片外存储通信开销、数据依赖关系管理

3. **多核架构的负载分配**
   - 对于多核硬件，需要合理分配workload以最小化同步开销
   - 涉及任务划分、数据分布和通信模式优化

4. **流水线架构的算子融合**
   - 在流水线式dataflow architecture中，通过算子融合优化计算和通信
   - 需要考虑数据流动模式、缓冲区管理和并行度挖掘

### 传统方法的局限性

如果针对每个优化空间手动编写映射方案，会面临以下问题：

- **复杂性爆炸**：组合多种优化策略导致设计空间巨大
- **标准化困难**：每个加速器都有独特的实现，难以形成统一标准
- **最优解难寻**：手动调优难以探索整个设计空间，可能错过更优解

### MLIR的多级IR解决方案

MLIR通过多级中间表示完美解决了上述挑战：
```
  深度学习模型
     |
     v
  High-Level Dialect (计算图优化，例如：ONNX)
     |
     v
  Mid-Level Dialect (数据流优化，例如：Linalg)
     |
     v
  Low-Level Dialect (硬件特定优化，例如：LLVM-IR)
     |
     v
  硬件代码
```

1. **高层优化**（如图级别）
   - 使用ONNX等dialect进行算子融合、常量折叠
   - 示例：将Conv+ReLU融合为单个操作

2. **中层优化**（如数据流级别）
   - 使用Linalg等dialect进行循环变换、tile切分
   - 示例：确定最优的矩阵乘tile大小

3. **低层优化**（如硬件特定级别）
   - 使用LLVM或自定义dialect进行指令调度、寄存器分配
   - 示例：生成特定加速器的微码

如此一来，我们可以看到MLIR在AI accelerator系统可以帮助进行：

- **标准化接口**：每个dialect提供清晰的抽象边界
- **可组合性**：优化pass可以独立开发和重用
- **可扩展性**：新的硬件特性可以通过新dialect快速支持
- **设计空间探索**：自动化工具可以系统性地搜索最优配置

通过MLIR的多级IR方法，硬件设计者能够以系统化、可维护的方式解决复杂的映射问题，同时保持设计的灵活性和性能。

## 1. MLIR的基本组成

MLIR（Multi-Level Intermediate Representation）是一个可重用的编译器基础设施，它的核心设计理念是通过分层和模块化的方式来表示不同抽象级别的中间表示。

### 1.1 IR（Intermediate Representation）

IR是编译器的中间表示，MLIR中的几个关键特性：

- **SSA形式**：所有值都是静态单赋值形式
- **三地址代码**：操作通常是`result = op operand1, operand2`的形式
- **区域（Regions）**：包含基本块（Blocks）的容器
- **基本块（Blocks）**：包含操作的有序序列

示例IR结构：
``` java
// 这是一个MLIR函数的例子
func.func @example(%arg0: i32) -> i32 {
    %0 = arith.constant 42 : i32
    %1 = arith.addi %arg0, %0 : i32
    return %1 : i32
}
```

### 1.2 Dialect

Dialect是MLIR的核心组织单元，它将相关的操作、类型和属性分组：

- **模块化设计**：每个domain-specific语言或抽象级别可以有自己的dialect
- **可扩展性**：可以轻松添加新的操作和类型
- **互操作性**：不同dialect的操作可以在同一模块中共存

## 2. 核心概念详解

### 2.1 Operation（操作）

Operation是MLIR中的基本执行单元，包含：

- **操作名称**：如`arith.addi`
- **操作数**：输入值
- **结果**：输出值
- **属性**：编译时常量参数
- **区域**：包含子操作（用于结构化操作）

TableGen定义示例：
``` java
// 在MyDialect.td文件中
def My_AddOp : My_Op<"add"> {
    let summary = "addition operation";
    let arguments = (ins My_Type:$lhs, My_Type:$rhs);
    let results = (outs My_Type:$result);
    let assemblyFormat = "$lhs , $rhs attr-dict : type($lhs)";
}
```

### 2.2 Attribute（属性）

Attribute是编译时常量数据，用于参数化操作：

- **类型安全**：每个属性都有特定类型
- **不可变性**：在编译期间固定不变
- **多样性**：支持整数、浮点数、字符串、数组等类型

属性使用示例：
``` java
%result = my_dialect.custom_op %input {
    factor = 42.0 : f32,
    name = "example",
    dimensions = [1, 2, 3]
} : (tensor<f32>) -> tensor<f32>
```

### 2.3 Constraint（约束）

约束用于验证操作和类型的正确性：

- **类型约束**：限制操作数/结果的类型
- **属性约束**：验证属性值的有效性
- **操作数数量约束**：确保正确数量的操作数

约束定义示例：
``` java
// 类型约束
def My_TensorType : Type<My_Dialect, "Tensor"> {
    let parameters = (ins "int64_t":$rank, "ArrayRef<int64_t>":$shape);
    let verifier = [{
        return success(shape.size() == rank);
    }];
}
// 在操作中使用约束
def My_MatmulOp : My_Op<"matmul"> {
    let arguments = (ins My_TensorType:$A, My_TensorType:$B);
    let constraints = [
        TypesMatchWith<"A and B must have compatible shapes", "A", "B",
        [{$_self.getRank() == $_other.getRank()}]>
    ];
}
```

### 2.4 Trait（特质）

特质描述操作的语义和行为特性：

- **不可变性**：如`Pure`、`ReadNone`
- **通信性**：如`Commutative`、`Associative`
- **区域特性**：如`SingleBlock`、`RecursiveMemoryEffects`

Trait使用示例：
``` java
def My_AddOp : My_Op<"add"> {
    let arguments = (ins My_Type:$lhs, My_Type:$rhs);
    let results = (outs My_Type:$result);
    // 添加特质
    let traits = [
        Commutative,
        Pure,
        TypesMatchWith<"lhs and rhs types must match", "lhs", "rhs",
        [{$_self == $_other}]>
    ];
}
```

但是我们一般不这么做，而是直接在自定义的operation参数里声明它的特质，例如MLIR官方的toy example中，直接将Pure这个Trait传入：
``` java
def ConstantOp : Toy_Op<"constant", [Pure]> {
  // Provide a summary and description for this operation. This can be used to
  // auto-generate documentation of the operations within our dialect.
  let summary = "constant";
  ...
```

### 2.5 Interface（接口）

接口提供操作的多态行为，类似于C++中的抽象基类：

- **类型接口（Type Interface）**：为类型定义通用操作
- **操作接口（Operation Interface）**：为操作定义通用操作
- **属性接口（Attribute Interface）**：为属性定义通用操作

接口作为MLIR中非常重要的核心概念之一，广泛应用在Dialect之间的转换（即pass）中。

接口定义示例：
``` java
// 在MyInterfaces.td文件中
def My_InferTypeOpInterface : OpInterface<"InferTypeOpInterface"> {
    let description = "Interface for operations that can infer their return types";
    let methods = [
        InterfaceMethod<
            "Infer and set the output types",
            "void", "inferTypes", (ins)
        >
    ];
}

// 在操作中实现接口
def My_AddOp : My_Op<"add"> {
    // ... 其他定义
    let extraClassDeclaration = [{
    void inferTypes() {
    // 类型推断逻辑
        getResult().setType(getLhs().getType());
    }
    }];
}

```

### 2.6 Pass（我也不知道怎么翻译）

Pass是MLIR转换和优化的基本单元：

- **Opertion Pass**：在单个操作上运行
- **Function Pass**：在单个函数上运行
- **Module Pass**：在整个模块上运行

Pass定义示例：
``` cpp
// 在C++文件中
struct MyOptimizationPass : public PassWrapper<MyOptimizationPass, OperationPass<ModuleOp>> {
    void runOnOperation() override {
    ModuleOp module = getOperation();
    // 优化逻辑
}
};

// 注册Pass
void registerMyOptimizationPass() {
    PassRegistration<MyOptimizationPass>();
}

```

在后续的笔记和博客中，我会更详细地展开介绍每个概念的具体实现，以及它们在tablegen文件中的语法和coding技巧。

## 3. 自定义Dialect实现

### 3.1 Dialect定义（TableGen）

在`MyDialect.td`中：
``` java
// 包含必要的MLIR基础定义
include "mlir/IR/OpBase.td"
include "mlir/IR/OpAsmInterface.td"

// 定义Dialect
def My_Dialect : Dialect {
    let name = "my_dialect";
    let summary = "A custom dialect for demonstration";
    let description = "This dialect contains custom operations and types";
    let cppNamespace = "::my_dialect";
    let useDefaultAttributePrinterParser = 1;
    let useDefaultTypePrinterParser = 1;
}

```

### 3.2 Operation定义（TableGen）

在`MyOps.td`中：
``` java
// 包含dialect定义
include "MyDialect.td"

// 定义操作基类
class My_Op<string mnemonic, list<Trait> traits = []> :
Op<My_Dialect, mnemonic, traits>;

// 具体操作定义
def My_CustomOp : My_Op<"custom", [Pure]> {
    let summary = "A custom operation";
    let description = "This operation performs a custom computation";

    let arguments = (ins
    F32Tensor:$input,
    F32Attr:$scale
);

let results = (outs F32Tensor:$output);

let assemblyFormat = "$input , $scale attr-dict : type($input) -> type($output)";

// 自定义builder
let builders = [
    OpBuilder<(ins "Value":$input, "float":$scale)>
];

// 验证器
let hasVerifier = 1;
}

```

### 3.3 C++ Implementation

在`MyDialect.cpp`中：
``` cpp
#include "MyDialect/MyDialect.h"
#include "MyDialect/MyOps.h"

using namespace mlir;
using namespace my_dialect;

#include "MyDialect/MyDialect.cpp.inc"
#include "MyDialect/MyOps.cpp.inc"

void MyDialect::initialize() {
    addOperations<
    #define GET_OP_LIST
    #include "MyDialect/MyOps.cpp.inc"

    ...
    addTypes<
    #define GET_TYPEDEF_LIST
    #include "MyDialect/MyTypes.cpp.inc"
    ...
}

// 自定义操作的C++实现
LogicalResult My_CustomOp::verify() {
    // 验证逻辑
    if (failed(verifySomeCondition(getInput())))
    return emitOpError("input validation failed");
    return success();
}

```

### 3.4 Type和Attribute定义

在`MyTypes.td`中：
``` java
// 自定义类型定义
def My_CustomType : TypeDef<My_Dialect, "CustomType"> {
    let parameters = (ins "int64_t":$size);
    let mnemonic = "custom";

    let assemblyFormat = "< $size >";

    let summary = "A custom tensor type";
}

// 自定义属性定义
def My_CustomAttr : AttrDef<My_Dialect, "CustomAttr"> {
    let parameters = (ins "int64_t":$value);
    let mnemonic = "custom";

    let assemblyFormat = "< $value >";
}

```

## 4. 构建和使用

### 4.1 CMake配置
在CMakeLists.txt中:
``` cmake
mlir_tablegen(MyOps.h.inc -gen-op-decls)
mlir_tablegen(MyOps.cpp.inc -gen-op-defs)
mlir_tablegen(MyDialect.h.inc -gen-dialect-decls)
mlir_tablegen(MyDialect.cpp.inc -gen-dialect-defs)

add_library(MyDialect
    MyDialect.cpp
    MyOps.cpp
    MyTypes.cpp
)

target_link_libraries(MyDialect
    MLIRIR
    MLIRSupport
)

```

### 4.2 在转换中使用
// 在Pass中使用自定义操作
``` cpp
void MyOptimizationPass::runOnOperation() {
    getOperation()->walk([](My_CustomOp op) {
        // 对自定义操作进行优化
        if (auto constant = op.getInput().getDefiningOparith::ConstantOp()) {
            // 常量折叠逻辑
            ...
        }
    });
}
```

## 5. 总结

在接下来的笔记博客中，我会更详细地展示MLIR lowering每个阶段如何针对自定义的Dialect, Attribute, pass等部件进行。这些概念为构建基于MLIR的编译器前端和中间端优化提供了坚实的基础。在后续文章中，我们将深入探讨TableGen语法、模式重写和更复杂的转换技术。

## 6. Reference

- [MLIR Tutorial](https://mlir.llvm.org/docs/Tutorials/)
- [Toy Tutorial](https://mlir.llvm.org/docs/Tutorials/Toy/)
- [【从零开始学深度学习编译器】十一，初识MLIR](https://mp.weixin.qq.com/s/4pD00N9HnPiIYUOGSnSuIw)