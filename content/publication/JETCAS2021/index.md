---
title: "An Energy-efficient Deep Belief Network Processor Based on Heterogeneous Multi-core Architecture with Transposable Memory and On-chip Learning"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- Xuan Huang
- Le Yang
- Jipeng Wang
- Bingqiang Liu
- Ziyuan Wen
- Juhui Li
- Guoyi Yu
- Kwen-Siong Chong
- Chao Wang

# Author notes (optional)
author_notes:
- "Equal contribution"
- "Equal contribution"

date: "2021-09-27T00:00:00Z"
doi: "10.1109/JETCAS.2021.3114396"

# Schedule page publish date (NOT publication's date).
publishDate: "2017-10-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: In *IEEE Journal on Emerging and Selected Topics in Circuits and Systems*
publication_short: In *IEEE JETCAS*

abstract: With the growing interest of edge computing in the Internet of Things (IoT), Deep Neural Network (DNN) hardware processors/accelerators face challenges of low energy consumption, low latency, and data privacy issues. This paper proposes an energy-efficient processor design based on Deep Belief Network (DBN), which is one of the most suitable DNN models for on-chip learning. In this study, a thorough algorithm-architecture-circuit design optimization method is used for efficient design. The characteristics of data reuse and data sparsity in the DBN learning algorithm inspires this study to propose a heterogeneous multi-core architecture with local learning. In addition, novel circuits of transposable weight memory and sparse address generator are proposed to reduce weight memory access and exploit neuron state sparsity, respectively, for maximizing the energy efficiency. The DBN processor is implemented and thoroughly evaluated on Xilinx Zynq FPGA. Implementation results confirm that the proposed DBN processor has excellent energy efficiency of 45.0 pJ per neuron-weight update, which has been improved by 74% against the conventional design.

# Summary. An optional shortened abstract.
summary: This paper presents an energy-efficient DBN processor based on heterogeneous multi-core architecture with transposable weight memory and on-chip local learning. In the future, we will focus on ASIC implementation of the proposed DBN processor in a GALS architecture with a 7T/8T SRAM-based transposable memory design to solve the bottlenecks and further improve throughput and energy efficiency.

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: True

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/MaxwellWjj/DBN-Processor'
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
