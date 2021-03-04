---
title: "An Energy-efficient Multi-core Restricted Boltzmann Machine Processor with On-chip Bio-plausible Learning and Reconfigurable Sparsity"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- Xuan Huang
- Le Yang
- Liang Wang
- Jipeng Wang
- Zuozhu Liu
- Kwen-Siong Chong
- Shaowei Lin
- Chao Wang

# Author notes (optional)
# author_notes:
# - "Equal contribution"
# - "Equal contribution"

date: "2020-07-31T00:00:00Z"
doi: "10.1109/A-SSCC48613.2020.9336135"

# Schedule page publish date (NOT publication's date).
publishDate: "2020-11-09T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In 2020 IEEE Asian Solid-State Circuits Conference
publication_short: In *A-SSCC 2020*

abstract: This paper proposes an energy-efficient multi-core processor design of restricted Boltzmann machine (RBM) with on-chip learning and reconfigurable sparsity. Inspired by bio-plausible variational probability flow (VPF) algorithm, our design significantly reduces the on-chip learning time and associated computation/energy as compared to conventional methods. The multi-core design with reconfigurable sparse weight connections further efficiently and flexibly reduces the required computation time and energy. FPGA implementation shows that the proposed design achieves 63.14 pJ per NW (neural weight) and 9.77 GNWs/s (neural weight update per second) at 128 MHz, which outperforms the baseline design by 44.0% and 24.3%, respectively.

# Summary. An optional shortened abstract.
summary: In this paper, a multi-core RBM processor design with on-chip learning and reconfigurable sparsity is proposed to reduce energy consumption and improve processing throughput. The FPGA implementation results show that the proposed RBM design achieves 44.0% energy saving and 24.3% speed improvement in RBM training operation against the baseline CD-based RBM design. In the future, we will focus on ASIC implementation of our proposed RBM processor to further improve the energy efficiency and minimize the hardware cost.

tags: [AI Accelerators]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/MaxwellWjj/RBM_Processor'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: 'slides.pptx'
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

Supplementary notes can be added here, including [code, math, and images](https://wowchemy.com/docs/writing-markdown-latex/).
