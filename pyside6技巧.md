<!--
 * @NOTE: pyside6技巧笔记
 * @Author: gu lei
 * @Date: 2023-04-19 23:47:02
 * @LastEditTime: 2023-04-21 09:55:01
 * @LastEditors: gu lei
-->

# PySide6

## 拖拽文件

### 将文件拖拽进QListWidget

```python
class MyListWidget(QListWidget):

    def dragEnterEvent(self, event: QDragEnterEvent) -> None:
        """（从外部或内部控件）拖拽进入后触发的事件"""
        if event.mimeData().hasUrls():
            event.accept()
        else:
            event.ignore()

    def dragMoveEvent(self, event: QDragMoveEvent) -> None:
        """拖拽移动过程中触发的事件"""
        event.accept()

    def dropEvent(self, event: QDropEvent) -> None:
        """拖拽结束以后触发的事件"""
        urls = event.mimeData().urls()
        self.addItems([i.toLocalFile() for i in urls])
        event.accept()
```

> 三个函数缺一不可。

### 将文件拖拽进QLineEdit

```python
class MyLineEdite(QLineEdit):

    def dragEnterEvent(self, event: QDragEnterEvent) -> None:
        event.accept()

    def dropEvent(self, event: QDropEvent) -> None:
        urls = event.mimeData().urls()[0]
        self.setText(urls.toLocalFile())
        event.accept()
```

> 其中 `dragEnterEvent`是必须函数。`dropEvent`可以实现更多的功能。
