---
title: "TATAA: Programmable Mixed-Precision Transformer Acceleration with a Transformable Arithmetic Architecture"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- Mo Song
- Jingmin Zhao
- Yizhao Gao
- Jia Li
- Hayden So

# Author notes (optional)
author_notes:
- "Equal contribution"
- "Equal contribution"

date: "2025-03-14T00:00:00Z"
doi: "https://doi.org/10.1145/3714416"

# Schedule page publish date (NOT publication's date).
publishDate: "2025-03-14T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: ACM Transactions on Reconfigurable Technology and Systems, Volume 18, Issue 1
publication_short: In *ACM TRETS*

abstract: Modern transformer-based deep neural networks present unique technical challenges for effective acceleration in real-world applications. Apart from the vast amount of linear operations needed due to their sizes, modern transformer models are increasingly reliance on precise non-linear computations that make traditional low-bitwidth quantization methods and fixed-dataflow matrix accelerators ineffective for end-to-end acceleration. To address this need to accelerate both linear and non-linear operations in a unified and programmable framework, this article introduces TATAA. TATAA employs 8-bit integer (int8) arithmetic for quantized linear layer operations through post-training quantization, while it relies on bfloat16 floating-point arithmetic to approximate non-linear layers of a transformer model. TATAA hardware features a transformable arithmetic architecture that supports both formats during runtime with minimal overhead, enabling it to switch between a systolic array mode for int8 matrix multiplications and a SIMD mode for vectorized bfloat16 operations. An end-to-end compiler is presented to enable flexible mapping from emerging transformer models to the proposed hardware. Experimental results indicate that our mixed-precision design incurs only 0.14% to 1.16% accuracy drop when compared with the pre-trained single-precision transformer models across a range of vision, language, and generative text applications. Our prototype implementation on the Alveo U280 FPGA currently achieves 2,935.2 GOPS throughput on linear layers and a maximum of 189.5 GFLOPS for non-linear operations, outperforming related works by up to 1.45x in end-to-end throughput and 2.29x in DSP efficiency, while achieving 2.19x higher power efficiency than modern NVIDIA RTX4090 GPU.

# Summary. An optional shortened abstract.
summary: In this work, we have presented TATAA, a programmable accelerators on FPGA for transformer models by using a novel transformable arithmetic architecture. Using TATAA, we demonstrate that both low-bitwidth integer (int8) and floating-point (bfloat16) operations can be implemented efficiently using the same underlying processing array hardware. By transforming the array from systolic mode for int8 MatMul to SIMD-mode for vectorized bfloat16 operations, we show that end-to-end acceleration of modern transformer models including both linear and non-linear functions can be achieved with state-of-the-art performance and efficiency. In the future, we plan to explore more general FPGA implementations of TATAA with more devices support (i.e., with or without HBM) and to enhance the flexibility of our compilation framework to accelerate future transformer models as they are being rapidly developed.

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/CASR-HKU/TATAA'
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
