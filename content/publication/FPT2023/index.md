---
title: "SqueezeBlock: A Transparent Weight Compression Scheme for Deep Neural Networks"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- Mo Song
- admin
- Yuhao Ding
- Hayden So

# Author notes (optional)
# author_notes:

date: "2024-02-01T00:00:00Z"
doi: "10.1109/ICFPT59805.2023.00032"

# Schedule page publish date (NOT publication's date).
publishDate: "2023-02-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In 2023 International Conference on Field Programmable Technology
publication_short: In *ICFPT 2023*

abstract: Modern Deep Neural Networks (DNNs) are no-torious for their large memory footprint, which impacts not only the storage capacity requirement in resource-constrained embedded systems, but also the performance of an inference machine due to data movement. In this work, we demonstrate a transparent weight compression scheme, called SqueezeBlock, which effectively reduces the memory footprint of DNN models with only minimal impact on their accuracy without the need for retraining. SqueezeBlock employs three steps, namely, clustering, quantization, and block encoding, to compress weights of DNN models and relies on automatic design space exploration to derive the optimal encoding configuration. Custom hardware decoders can be generated automatically for seamless integration with the memory subsystem. Experiments on a range of DNNs show that SqueezeBlock can effectively compress the original fp32 weights by up to 4.88× to 6 bit per weight with the loss of accuracy kept within 0.92% across tested models.

# Summary. An optional shortened abstract.
summary: In this work, we present SqueezeBlock, a transparent weight compression scheme to effectively reduce the memory footprint of DNN models while maintaining a low accuracy degradation without requiring network retraining. SqueezeBlock uses a cluster-aided FP8 quantization method to preprocess weights that facilitates a subsequent block-based weight encoding scheme to achieve a good compression ratio and accuracy loss tradeoff. In addition, the proposed design space exploration framework allowed us to identify optimal encoding parameter configurations for different DNN models. A hardware decoder customized to the specific encoding could then be generated automatically to decode the weights during run time. Experiments showed that SqueezeBlock was able to retain the most significant information from the original model despite the large compression ratio, which may not be immediately apparent, as demonstrated by the ViT model compared to other vision models. In the future, we plan to expand the proposed scheme to compress and decompress activations during run time on hardware, which can improve both DNN inference and training time.

tags: [AI Hardware]

# Display this page in the Featured widget?
featured: false

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