---
title: 如何在PySide6实现选择文件,文件夹功能
published: 2025-10-15
description: '通过PySide6选择文件或文件夹'
image: ''
tags: ["PySide6", "Python", "Qt", "File"]
category: '网络安全'
draft: false 
lang: ''
pinned: false
author: Mikuas
---

### 

* 选择文件夹
```python
from PySide6.QtWidgets import QFileDialog

# 打开文件夹选择对话框
folder = QFileDialog.getExistingDirectory(
    parent=None,
    caption="Title",
    dir=""          # 起始路径
)
""" 选择了返回文件夹路径, 取消返回"" """

```

* 选择单个文件
```python
from PySide6.QtWidgets import QFileDialog
file = QFileDialog.getOpenFileName(
    parent=None,
    caption="",
    dir="",
    filter="(*.*);;文本文件 (*.txt);;图片 (*.png *.jpg)"      # 过滤器
)[0]

```

* 选择多个文件
```python
from PySide6.QtWidgets import QFileDialog
files = QFileDialog.getOpenFileNames(
    parent=None,
    caption="",
    dir="",
    filter="(*.*);;文本文件 (*.txt);;图片 (*.png *.jpg)"
)[0]

```

* 选择保存路径
```python
from PySide6.QtWidgets import QFileDialog
path = QFileDialog.getSaveFileName(
    parent=None,
    caption="",
    dir="",
    filter="文本文件 (*.txt)"
)[0]

```