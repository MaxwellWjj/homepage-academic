---
title: "MSD: Mixing Signed Digit Representations for Hardware-efficient DNN Acceleration on FPGA with Heterogeneous Resources"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- Jiajun Zhou
- Yizhao Gao
- Yuhao Ding
- Ngai Wong
- Hayden So

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
publication: In the 31st IEEE International Symposium On Field-Programmable Custom Computing Machines
publication_short: In *FCCM 2023*

abstract: While quantizing deep neural networks (DNNs) to 8-bit fixed point representations has become the de facto technique in modern inference accelerator designs, the quest to further improve hardware efficiency by reducing the bitwidth remains a major challenge due to (i) the significant loss in accuracy and (ii) the need for specialized hardware to operate on these ultra-low bitwidth data that are not readily available in commodity devices.
By employing a mix of a novel restricted signed digit (RSD) representation that utilizes limited number of effectual bits and the conventional 2's complement representation of weights, a hybrid approach that employs both the fine-grained configurable logic resources and coarse-grained signal processing blocks in modern FPGAs is presented.
Depending on the availability of fine-grained and coarse-grained resources, the proposed framework encodes a subset of weights with RSD to allow highly efficient bit-serial multiply-accumulate implementation using LUT resources.
Furthermore, the number of effectual bits used in RSD is optimized to match the bit-serial hardware latency to the bit-parallel operation on the coarse-grained resources to ensure the highest run time utilization of all on-chip resources.
Experiments show that the proposed mixed signed digit (MSD) framework can achieve a 1.52$\times$ speedup on the ResNet-18 model over the state-of-the-art, and a remarkable 4.78\% higher accuracy on MobileNet-V2.

# Summary. An optional shortened abstract.
summary: This work has proposed MSD, an FPGA-tailored and heterogeneous DNN acceleration framework to utilize both LUTs and DSPs as computation resources and to exploit bit-sparsity. The RSD data representation enables MSD to fine-tune and encode the DNN weights into a bit-sparsity-aware format, making the bit-serial computation on LUTs more efficient. Furthermore, we adopt a latency-driven search algorithm into MSD, which can search for the optimal schedule, the number of EB, and the workload split ratio for each layer, based on a latency constraint. Evaluation results on various DNN models and edge FPGA devices demonstrate that MSD achieves 1.52 $\times$ speedup and 1.36 $\times$ higher throughput compared with the state-of-the-art on ResNet-18 model, and 4.78\% higher accuracy on MobileNet-V2. In the future, we will explore more efficient scheduling methods for workload splitting in the heterogeneous architecture and EB selection in the bit-serial computation, and exploit FPGA-layout-tailored hardware design to further enhance the hardware clock frequency.

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/CASR-HKU/MSD-FCCM23'
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

The final version of pdf file and the conference poster/video will be uploaded in the future.
