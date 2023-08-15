---
title: "decrypt pdf"
date: 2022-12-15T12:00:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Linux
toc: false
usemathjax: false
copybutton: true
---

### Remove password from PDF using Linux/WSL

Install pdftk

```
sudo dnf install pdftk
```
```
sudo apt-get install pdftk
```

Then run
```
pdftk file.pdf output output.pdf do_ask
```

Enter password when prompted.