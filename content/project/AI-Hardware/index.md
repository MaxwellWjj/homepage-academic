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
url_pdf: ""
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

During my undergraduate research, I have studied many advanced algorithms and design tricks. These foundations help me write my first top-tier conference paper **An Energy-efficient Multi-core Restricted Boltzmann Machine Processor with On-chip Bio-plausible Learning and Reconfigurable Sparsity**. Please refer to my publications for more details.

Generally, an AI hardware system needs highly optimized Processing Elements (PEs) for computation, domain-specific architecture and an efficient compiler for scheduling data mapping and movement. In architecture level, the key optimization point is the connection and dataflow of PEs with hierarchical architecture. For a certain multi-level hardware architecture, there are thousands to millions of data mapping solutions and data moving strategies. Therefore, how to find the best optimized mapping and data flow solution for different goals like power and latency should be the task of compilers. The AI model is represented as loops, memory addresses, and computational functions by compilers. Compilation optimizes tasks to efficiently run on each PE of hardware. Although AI hardware has been deeply studied in the academic field, there are still some problems and bottlenecks that need to be solved, which are my objectives during PhD research.

Problem 1: PE-level optimization for supporting both training and inference. Training process is a crucial ingredient for user adaptation at the edge-device as well as transfer learning with domain-specific data. Many circuit-level works have been proposed to support both training and inference in DNN models such as and. However, transposed memory is proved to be the most important point because the weights matrix should be transposed to join training and inference computation, while most of the previous works ignored that.

Problem 2: PE-level optimization for sparsity in AI models. The small values that almost do not affect results can be replaced by zeros, which is called sparsity. Sparsity gives the opportunity to implement efficient hardware. If zero values can be skipped in computation, it is potential to achieve significantly high energy saving and improve faster computation speed. Therefore, it is still promising to develop more efficient Processing Elements to support sparsity optimization.

Problem 3: Varieties of AI models cannot be efficiently accelerated in a fixed architecture. There are various AI models that need to be accelerated in specific hardware especially in DNNs used in edge devices. In those models, data dimension in layers can diminish, as presented in. For hardware designers, widely varying DNN layer shapes is challenging because an architecture may fit a type of convolutional layer, but it will face utilization problems when applied for other layers because of different topology. To solve the problem, high-level analysis of different layers, even different DNN models, is important to design architectures for different models.

Problem 4: Architecture should be abstract to find the best topology and parameters. Previous works that focus on AI hardware are mostly based on fixed architecture or constrained by hardware resource. However, those implementations lose flexibility of architecture-level due to their fixed layout without discussing whether the parameters are optimized. Some proposals focusing on compilers for AI hardware analyze a broad space of software mappings of a workload onto an architecture, but the relationship between mappings and hardware dataﬂows is not elucidated. The best solution for high-level tools is to make hardware abstract and use mathematical tools to get the best optimized result.

Problem 5: Architecture-compiler co-design and dynamic reschedule. Compiler is responsible for finding the schedule of operation tasks in AI hardware. Previous works mainly focus on data reuse and dependency to get the schedule. But they did not analyze data movement with architecture. For example, intermediate results may not be moved to off-chip before the next layer computation starts. Besides, computation latency is irregular because it depends on the sparsity pattern, leading to decrement of hardware utilization, which needs to be rescheduled dynamically. Therefore, Architecture-compiler co-design is a promising objective.
