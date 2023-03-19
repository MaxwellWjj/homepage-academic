---
title: "Model-Platform Optimized Deep Neural Network Accelerator Generation through Mixed-integer Geometric Programming"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- Yuhao Ding
- admin
- Yizhao Gao
- Maolin Wang
- Hayden So

# Author notes (optional)
author_notes:
- "Equal contribution"
- "Equal contribution"

date: "2023-03-18T00:00:00Z"
doi: "10.1109/A-SSCC48613.2020.9336135"

# Schedule page publish date (NOT publication's date).
publishDate: "2023-03-18T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In the 31st IEEE International Symposium On Field-Programmable Custom Computing Machines
publication_short: In *FCCM 2023*

abstract: Although there are distinct power-performance advantages in customizing an accelerator for a specific combination of FPGA platform and neural network model, developing such highly customized accelerators is a challenging task due to the massive design space spans from the range of network models to be accelerated, the target platform's compute capability, and its memory capacity and performance characteristics. To address this architectural customization problem, an automatic design space exploration (DSE) framework using a mixed-integer geometric programming (MIGP) approach is presented. Given the set of DNN models to be accelerated and a generic description of the target platform's compute and memory capabilities as input, the proposed framework automatically customizes an architectural template for the platform-model combination and produces the associated I/O schedule to maximize its end-to-end performance. By formulating DNN inference as a multi-level loop tiling problem, the proposed framework first customizes an accelerator template that consists of a parameterizable array architecture with SIMD execution cores and a customizable memory hierarchy using a MIGP to maximize the expected resource utilization. Subsequently, a second MIGP is used to schedule memory and compute operations as tiles to improve on-chip data reuse and memory bandwidth utilization. Experimental results from a wide range of neural network models and FPGA platform combinations show that the proposed scheme is able to produce accelerators with performance comparable to the state-of-the-art. The proposed DSE framework and the resulting hardware/software generator are available as an open-source package called AGNA with the hope that it may facilitate vendor-agnostic DNN accelerator development from the research community in the future.

# Summary. An optional shortened abstract.
summary: In this paper, we have presented AGNA, an open-source deep neural network (DNN) accelerator hardware and software generator for FPGA platforms.  AGNA relies on the two proposed MIGP formulations and their relaxed solutions to perform DSE in customizing a generic accelerator template for the given network model-platform combination.  Through extensive experiments using many combinations of DNN models and platforms, we have demonstrated AGNA's capability to produce DNN accelerators with performance comparable to the state-of-the-art in research and vendor-provided frameworks.  Importantly, although the accelerators produced currently may not be the fastest in all model-platform combinations, AGNA is vendor-agnostic and is designed to be easily extensible, making it suitable for real-world deployments and for serving as a basis for future research.  In the future, we plan to improve AGNA with advanced scheduling capability to work with multi-bank memories, as well to improve its performance through low-level hardware optimizations.  We also plan to explore novel network model quantization and pruning techniques by leveraging the processing architecture and scheduling capabilities of AGNA.

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/CASR-HKU/AGNA-FCCM2023'
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

The final version of pdf file and the conference poster/video will be uploaded in the future. Doi is not the real one now.
