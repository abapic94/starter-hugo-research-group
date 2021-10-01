---
title: "Characterization of Basic 5-Value Spectrum Functions Through Walsh-Hadamard Transform"
authors:
- samir
- Peter Horak
- admin

date: "2021-02-01T00:00:00Z"
doi: "https://doi.org/10.1109/TIT.2020.3044059"

# Schedule page publish date (NOT publication's date).
publishDate: "2021-10-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: "IEEE Transactions on Information Theory"
publication_short: " "

abstract: The first and the third authors recently introduced a spectral construction of plateaued and of 5-value spectrum functions. In particular, the design of the latter class requires a specification of integers $\\{W(u)\\colon u \\in {F}_2^n\\}$, where $W(u) \\in \\{0, \\pm 2n+s_1/2, \\pm 2n+s_2/2\\}$, so that the sequence $\\{W(u)\\colon u \\in \\mathbb{F}^n_2\\}$ is a valid spectrum of a Boolean function (recovered using the inverse Walsh transform). Technically, this is done by allocating a suitable Walsh support $S = S[1] \\cup S[2] \\subset \\mathbb{F}^n_2$, where $S[i]$ corresponds to those $u \\in \\mathbb{F}^n_2 $ for which $W(u) = \\pm 2n+s_i/2$. In addition, two dual functions $g[i]\\colon S[i]  \\mathbb{F}_2^n$ (with $\\# S[i] = 2^{\\lambda_i}$) are employed to specify the signs through $W(u) = 2n+s_i/2(-1)_{g[i]}(u)$ for $u \\in S[i]$ whereas $W(u) = 0$ for $u \\notin S$. In this work, two closely related problems are considered. Firstly, the specification of plateaued functions (duals) $g[i]$, which additionally satisfy the so-called totally disjoint spectra property, is fully characterized (so that $W(u)$ is a spectrum of a Boolean function) when the Walsh support $S$ is given as a union of two disjoint affine subspaces $S[i]$. Especially, when plateaued dual functions $g[i]$ themselves have affine Walsh supports, an efficient spectral design that utilizes arbitrary bent functions (as duals of $g[i]$) on the corresponding ambient spaces is given. The problem of specifying affine inequivalent 5-value spectra functions is also addressed and an efficient construction method that ensures the inequivalence property is derived (sufficient condition being a selection of affine inequivalent duals). In the second part of this work, we investigate duals of plateaued functions with affine Walsh supports. For a given such plateaued function, we show that different orderings of its Walsh support which are employing the Sylvester-Hadamard recursion actually induce bent duals which are affine equivalent.

# Summary. An optional shortened abstract.
summary: 

tags:
  - Bent functions
  - plateaued functions
  - 5-value spectrum functions
  - lexicographic ordering
  - spectral construction methods


featured: false

#links:
# - name: ""
#url: https://link.springer.com/article/10.1007%2Fs12095-021-00491-1
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
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/jdD8gXaTZsc)'
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
slides:
---
