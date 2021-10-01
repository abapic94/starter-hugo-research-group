---
title: "Proving the conjecture of O’Donnell in certain cases and disproving its general validity"
authors:
- sadmir
- admin

date: "2021-01-31T00:00:00Z"
doi: "https://doi.org/10.1016/j.dam.2020.11.005"

# Schedule page publish date (NOT publication's date).
publishDate: "2021-10-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: "Discrete Applied Mathematics"
publication_short: " "

abstract: For a function $f:\\{−1,1\\}^n\\to \\{−1,1\\}$ the relationship between the sum of its linear Fourier coefficients $\\hat{f}(i)$ (defined by $\\hat{f}(i)≔\\frac{1}{2^n}\\sum_{x\in \\{−1,1\\}^n} f(x)x_i$ for $i=1,2,\\ldots,n$ and $x=(x_1,\\ldots,x_n)$) and its degree $d$ is a problem in theoretical computer science related to social choice. In that regard, in 2012, O’Donnell conjectured that $\sum_{i=1}^n \\hat{f}(i)\\leq d\\binom{d-1}{\\lfloor \\frac{d-1}{2} \\rfloor}2^{1-d}$. In 2020, Wang (Wang, 2020) proved that the conjecture is equivalent to a conjecture about cryptographic resilient Boolean functions and proved that the conjecture is true for every $n$, when $d=1$ and $d=n−1$. In this paper, we contribute theoretically to this problem by showing that if the conjecture is true for some specific relatively small value $n_0$, which depends on $d$, then it is valid for any $n\\geq n_0$. In particular, we show that the conjecture is correct for every $n$, when $d=2$ and $d=3$. However, despite these promising results, we present a counterexample when $d=4$, and so the conjecture is not true in general.

# Summary. An optional shortened abstract.
summary: 

tags:
- Boolean functions
- O’Donnell’s conjecture
- Resiliency
- Walsh transform

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
