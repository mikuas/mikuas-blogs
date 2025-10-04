---
title: PySide6如何检测窗口是否可以在屏幕完全显示
published: 2025-05-11
description: 检测窗口是否完整显示在屏幕内
image: ''
category: 'PySide6'
tags: ["PySide6", "Python", "Qt"]
draft: false 
lang: ''
pinned: false
author: Mikuas
---

```python
from PySide6.QtWidgets import QApplication
from PySide6.QtCore import QRect

# 通过Rect检测
rect = self.frameGeometry()

# 通过Point检测
rect = QRect(self.pos(), self.size())

# 获取screen对象
screen = QApplication.screenAt(rect.center())
if not screen:
    screen = QApplication.primaryScreen()
available = screen.availableGeometry()
  
avaRect = available.contains(rect)

result = [
    max(0, available.left() - rect.left()),     # left
    max(0, available.top() - rect.top()),       # top
    max(0, available.right() - rect.right()),   # right
    max(0, available.bottom() - rect.bottom())  # bottom
]

```