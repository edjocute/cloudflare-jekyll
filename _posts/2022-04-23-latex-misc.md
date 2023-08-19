---
title: "Space saving tricks for LaTeX"
date: 2022-04-23T18:00:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Writing
  - Latex
  - Science
toc: true
usemathjax: true
---


## 1. Use [savetrees](http://www.ctan.org/tex-archive/macros/latex/contrib/savetrees/savetrees.pdf)

```white
\usepackage{savetrees}
```

## 2. Change lengths for floats [(reference)](https://tex.stackexchange.com/questions/23313/how-can-i-reduce-padding-after-figure)

1. Caption package

	```white
	\usepackage{caption}
	\captionsetup{aboveskip=0pt}
	\captionsetup{belowskip=0pt}
	```

2. Use setlength: `\setlength{\abovecaptionskip}{0pt}`

	```white
	\floatsep
	\textfloatsep
	\intextsep
	\dbltextfloatsep
	\dblfloatsep
	\abovecaptionskip
	\belowcaptionskip
	```

3. Use `\vspace{}`


## 3. Change other lengths with `\setlength` [(reference)](http://www-h.eng.cam.ac.uk/help/tpl/textprocessing/squeeze.html)

1. Paragraphs

	```white
	\parindent
	\parskip
	```

2. Maths

	```white
	\abovedisplayskip
	\belowdisplayskip
	\arraycolsep
	```

3. Lists

	```white
	\topsep
	\partopsep
	\itemsep
	```

## 4. Increase penalties [(reference)](https://tex.stackexchange.com/questions/51263/what-are-penalties-and-which-ones-are-defined)

- Default values:

	```white
	\linepenalty=10
	\hyphenpenalty=50
	\exhyphenpenalty=50
	\interlinepenalty=0
	\widowpenalty=150
	\brokenpenalty=100
	```

## 5. Reduce space around section headings with `titlesec` package [(reference)](https://robjhyndman.com/hyndsight/squeezing-space-with-latex/)

	```white
	\usepackage[compact]{titlesec}
	\titlespacing{\section}{0pt}{2ex}{1ex}
	\titlespacing{\subsection}{0pt}{1ex}{0ex}
	\titlespacing{\subsubsection}{0pt}{0.5ex}{0ex}
	```



## 6. Other tricks [here](https://gist.github.com/yig/81b4c993ea13252edc81)
