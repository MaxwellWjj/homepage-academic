---
title: "A Case for Low Bitwidth Floating Point Arithmetic on FPGA for Transformer Based DNN Inference"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- Mo Song
- Jingmin Zhao
- Hayden So

# Author notes (optional)
# author_notes:

date: "2024-03-29T00:00:00Z"
# doi: "10.1109/ICFPT59805.2023.00032"

# Schedule page publish date (NOT publication's date).
publishDate: "2023-03-29T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In 2024 IEEE International Parallel and Distributed Processing Symposium Workshops (IPDPSW)
publication_short: In *IPDPSW 2024*

abstract: Modern Transformer-based deep neural networks are increasingly reliance on an accelerator's ability to perform the model's non-linear operations accurately and efficiently. Unfortunately, while conventional low bitwidth fixed-point arithmetic are superior in terms of power, performance, and area consumption for low-precision linear operations, they are not well-suited to perform non-linear operations that require large dynamic range and high precision. In this work, we present a case for using a mix of low-bitwidth block floating-point and single precision floating-point operations to address the needs of both the linear and non-linear layers of Transformer-based DNNs. To support both datatypes in hardware, an 8-bit block floating point (bfp8) processing array that can be reconfigured to implement 32-bit floating point (fp32) computation during run time is proposed. With the support of both datatypes, pre-trained Transformer models in fp32 can now be deployed without the need for quantization-aware retraining. The proposed bfp8 systolic array has been implemented efficiently on an AMD Alveo U280 FPGA, consuming only marginally more hardware resources than an int8 equivalence. It attains 2.052 TOPS throughput for the linear operations in bfp8 mode, which is equivalent to over 95% of the theoretical maximum 8-bit throughput of the target platform, while it achieves 33.88 GFLOPS throughput when operating in fp32 mode. By demonstrating the hardware efficiency of low-precision floating-point operations on FPGAs, this work provides an attractive tradeoff direction in the vast design space of accuracy, speed, power, and time-to-market for full-stack Transformer models acceleration.

# Summary. An optional shortened abstract.
summary: In this paper, we have presented a case for using low-bitwidth floating-point arithmetic for Transformer-based DNNs inference. We demonstrate that low-bitwidth floating-point (bfp8) matrix multiplication can be implemented effectively in hardware with a marginal increase over an 8-bit integer equivalence while attaining processing throughput close to the platform maximum. In addition, we show that such an array can be effectively reconfigured during run-time into a programmable fp32 vector processing unit that can be programmed to implement any non-linear functions required by future Transformer-based DNN models. With efficient support of both datatypes, the proposed design eliminates the need to quantize and retrain Transformer models, which is increasingly challenging due to its size. We argue that mixed-precision floating-point appears to be a promising datatype that provides a favorable balance between model accuracy and hardware performance for Transformer-based DNN acceleration. Currently, an automatic compilation framework that provides full stack acceleration of Transformer models is underway. The vector processing unit is also being optimized to improve non-linear function performance.

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---