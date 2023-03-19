---
title: "DyBit: Dynamic Bit-Precision Numbers for Efficient Quantized Neural Network Inference"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- Jiajun Zhou
- admin
- Yizhao Gao
- Yuhao Ding
- Chaofan Tao
- Boyu Li
- Fengbin Tu
- Kwang-Ting Cheng
- Hayden So
- Ngai Wong

# Author notes (optional)
author_notes:
- "Equal contribution"
- "Equal contribution"

date: "2023-03-18T00:00:00Z"
# doi: "10.1109/A-SSCC48613.2020.9336135"

# Schedule page publish date (NOT publication's date).
publishDate: "2023-05-11T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In the Design Automation Conference 2023
publication_short: In *DAC 2023*

abstract: To accelerate the inference of deep neural networks (DNNs), quantization with low-bitwidth numbers is actively researched. A prominent challenge is to quantize the DNN models into low-bitwidth numbers without significant accuracy degradation, especially at very low bitwidths ($<$ 8 bits). This work targets an adaptive data representation with variable-length encoding called DyBit. DyBit can dynamically adjust the precision and range of separate bit-field to be adapted to the DNN weights/activations distribution. We also propose a hardware-aware quantization framework with a mixed-precision accelerator to trade-off the inference accuracy and speedup. Experimental results demonstrate that the inference accuracy via DyBit is 1.97\% higher than the state-of-the-art at 4-bit quantization, and the proposed framework can achieve up to 8.1$\times$ speedup compared with the original model.

# Summary. An optional shortened abstract.
summary: This paper has proposed a novel hardware-aware quantization framework, with a fused mixed-precision accelerator, to efficiently support a distribution-adaptive data representation named DyBit. The variable-length bit-fields enable DyBit to adapt to the tensor distribution in DNNs. Evaluation results show that DyBit-based quantization at very low bitwidths ($<$8bits) consistently achieves higher accuracy than competing methods. Moreover, the proposed end-to-end framework can effectively search for the optimal solution under various constraints, thus achieving a trade-off between accuracy and hardware speedup. Experiments on various DNN models under different quantization constraints demonstrate that the framework can quantize DNN models to achieve $2.5 \sim 8.1\times$ speedup. 

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: false

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/lloo099/Unified_Quantization'
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

This paper has been accepted as a poster in DAC 2023. We are submitting it to TCAD.
