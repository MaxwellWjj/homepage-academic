---
title: "Efficient Design of Spiking Neural Network with STDP Learning Based on Fast CORDIC"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- Yi Zhan
- Zixuan Peng
- Xinglong Ji
- Guoyi Yu
- Rong Zhao
- Chao Wang

# Author notes (optional)
author_notes:
- "Equal contribution"
- "Equal contribution"

date: "2021-06-01T00:00:00Z"
doi: "10.1109/TCSI.2021.3061766"

# Schedule page publish date (NOT publication's date).
publishDate: "2021-03-02T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: In IEEE Transactions on Circuits and Systems I - Regular Papers
publication_short: In *TCAS-I*

abstract: In emerging Spiking Neural Network (SNN) based neuromorphic hardware design, energy efficiency and on-line learning are attractive advantages mainly contributed by bio-inspired local learning with nonlinear dynamics and at the cost of associated hardware complexity. This paper presents a novel SNN design employing fast COordinate Rotation DIgital Computer (CORDIC) algorithm to achieve fast spike timing–dependent plasticity (STDP) learning with high hardware efficiency. In this study, a system design and evaluation method of CORDIC-based SNN is proposed for finding optimal CORDIC type and precision, from theoretical CORDIC-level error to application-level learning performance. From the proposed design and evaluation method, a reconfigurable SNN design based on fast-convergence CORDIC is designed to achieve high classification accuracy on MNIST, fast on-line learning and good energy efficiency. By utilizing SNN’s fault tolerance and time-division-multiplexing (TDM) strategy, the reconfigurable SNN design employs 8-bit fast-convergence CORDIC and TDM-based hardware accelerator for high efficiency. FPGA implementation results confirm that the proposed fast-convergence CORDIC SNN design outperforms the state-of-the-art CORDIC method by 38.5%-45.3% in terms of learning speed and energy efficiency, with the STDP learning of 30.2 ns/SOP, energy efficiency of 176.6 pJ/SOP, processing speed of 6.1 ms/image, and on-line learning convergence of 21.4 s (time to reach the final accuracy, on average), on MNIST benchmark.

# Summary. An optional shortened abstract.
summary: This paper presents a novel CORDIC based Spiking Neural Network (SNN) design with on-line STDP learning and high hardware efficiency. A system design and evaluation method of CORDIC SNN is proposed to evaluate the hardware efficiency of SNN based on different CORDIC algorithm types and bit-width precisions.

tags: [AI Hardware, Neuromorphic Computing]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: 'https://github.com/MaxwellWjj/FPGA_Spiking_NN'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: 'Slides-Storyline_TCAS-I_CORDIC_SNN.pptx'
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

This paper was my first journal article, which inspired me to further study AI accelerators. Please note that the "code" is RTL-level code. The software simulation code is here: [Software Simulation Codes](https://github.com/MaxwellWjj/ULIIC_SNN_library).
