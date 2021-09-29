---
title: Energy-efficient AI Hardware
summary: Develop domain-specific AI hardware systems for power-limited and real-time applications.
tags:
- AI Hardware
date: "2020-01-27T00:00:00Z"

image:
  focal_point: Smart

links:
url_code: "https://github.com/MaxwellWjj/DBN_Processor"
url_pdf: "media/presentation_ai_hardware.pdf"
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

Artificial Intelligent (AI) has been widely studied due to its high performance in various applications such as classification, translation and recognition. With the help of AI hardware platforms such as GPU, the inference speed of AI models achieves faster than CPU. However, in many power-limited applications such as edge computing, the power-hungry AI platforms face challenges in power consumption. Therefore, developing the energy-efficient AI hardware systems is very important. I'm very interested in this direction and my PhD research is about it.

Generally, an AI hardware system needs highly optimized Processing Elements (PEs) for computation, domain-specific architecture and an efficient compiler for scheduling data mapping and movement. In architecture level, the key optimization point is the connection and dataflow of PEs with hierarchical architecture. For a certain multi-level hardware architecture, there are thousands to millions of data mapping solutions and data moving strategies. Therefore, how to find the best optimized mapping and data flow solution for different goals like power and latency should be the task of compilers. The AI model is represented as loops, memory addresses, and computational functions by compilers. Compilation optimizes tasks to efficiently run on each PE of hardware. Although AI hardware has been deeply studied in the academic field, there are still some problems and bottlenecks that need to be solved, which are my objectives during PhD research.

- Problem 1: PE-level optimization for supporting both training and inference. 

- Problem 2: PE-level optimization for sparsity in AI models. 

- Problem 3: Varieties of AI models cannot be efficiently accelerated in a fixed architecture.

- Problem 4: Architecture should be abstract to find the best topology and parameters.

- Problem 5: Architecture-compiler co-design and dynamic reschedule.

Currently we are working on accelerator generation, which aims to solve the problem 5. We prepare to summarize into a paper and submit it to FCCM'22 Conference. Hope everything goes well!