---
title: PySide6如何绘制无框窗口
published: 2024-09-02
description: '用PySide6绘制无框窗口'
tags: ["PySide6", "Python", "Qt"]
image: 'guide/alxy.webp'
category: 'PySide6'
draft: false 
lang: ''
pinned: false
author: Mikuas
---

### 自定义标题栏
为了还原窗口的最大,小化,关闭拖拽功能,我们需要实现一个标题栏`TitleBar`
```python
from PySide6.QtWidgets import QWidget

class TitleBar(QWidget):
    def __init__(self):
        super().__init__()


```

### 实现无框窗口
```python
import sys
from PySide6.QtWidgets import QApplication, QWidget, QVBoxLayout, QHBoxLayout
from PySide6.QtGui import QColor, QPainter
from PySide6.QtCore import Qt

class Window(QWidget):
    def __init__(self):
        super().__init__()
        
        # 设置无边框
        self.setWindowFlags(Qt.WindowType.FramelessWindowHint)
        # 透明背景
        self.setAttribute(Qt.WidgetAttribute.WA_TranslucentBackground)
        
        # 弹出的无框窗口
        self.setWindowFlags(
            Qt.WindowType.FramelessWindowHint |
            Qt.WindowType.Popup |
            Qt.WindowType.NoDropShadowWindowHint # 不加边框会有阴影
        )
        
        
    def paintEvent(self, event):
        """ 自定义绘制背景 """
        painter = QPainter(self)
        painter.setPen(Qt.NoPen)
        painter.setBrush(QColor(245, 245, 245))
        
        painter.drawRoundedRect(self.rect(), 8, 8)

```