---
title: "Linux: Miscellaneous"
date: 2022-04-22T13:45:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Android
  - Oneplus
---

## 1. Terminal
Reset terminal driver:
```
echo -e \\033c
```

Reset scrollback:
```
echo -e "\033[3J"
```

## 2. Desktop
Reset desktop:
```
sudo systemctl restart display-manager
```
