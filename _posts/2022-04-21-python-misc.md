---
title: "Python: Miscellaneous"
date: 2022-04-21T14:30:00+08:00
#last_modified_at: 2022-05-11T17:30:00+08:00
categories:
  - Blog
tags:
  - Python
---

## 1. Anaconda/Miniconda

### 1.1) Create new environment
```bash
conda create -n py39 python=3.9
```

### 1.2) Add kernel to Jupyter
```bash
source activate py39
python -m ipykernel install --name py39
source deactivate
```

### 1.3) Install mamba
```bash
conda install mamba -n base -c conda-forge
```

### 1.4) Add existing `R` installation to Jupyter

- Check `R` home directory: 

	```white
	R.home("bin")
	```

- In Command Prompt (Windows):

	```bash
	cd 'C:\Program Files\R\R-4.0.0\bin\x64\'
	conda activate my_env
	R.exe
	install.packages("IRkernel")
	IRkernel::installspec()
	q()
	```

---


## 2. Matplotlib

- Import:

	```python
	import matplotlib.pyplot as plt
	%matplotlib inline
	plt.rcParams.update({'text.usetex': True, 'figure.dpi':150, 'savefig.dpi':300})
	```

- Use subset of colormap:

	```python
	cmap = matplotlib.colors.ListedColormap(plt.cm.tab10.colors[:2])
	```

---

## 3. Pytables

- Close all opened files:

	```python
	tables.file._open_files.close_all()
	```

## 4. Jupyter

- Use full width:

	```python
	from IPython.display import display, HTML
	display(HTML("<style>.container { width:100% !important; }</style>"))
	```

## 4. HTTP Server (Python 3)

```bash
python -m http.server
python -m http.server 8000 --bind 127.0.0.1 --directory /tmp/
```

