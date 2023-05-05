<!--
 * @NOTE: pyside6学习技巧
 * @Author: gu lei
 * @Date: 2023-04-19 23:47:02
 * @LastEditTime: 2023-05-05 13:48:06
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

## 将pandas显示在tableview中

```python
class PandasModel(QAbstractTableModel):
    def __init__(self, data):
        QAbstractTableModel.__init__(self)
        self._data = data

    def rowCount(self, parent=None):
        return self._data.shape[0]

    def columnCount(self, parent=None):
        return self._data.shape[1]

    def data(self, index, role=Qt.DisplayRole):
        if index.isValid():
            if role == Qt.DisplayRole:
                return str(self._data.iloc[index.row(), index.column()])
        return None

    def headerData(self, section, orientation, role=Qt.DisplayRole):
        if orientation == Qt.Horizontal and role == Qt.DisplayRole:
            return str(self._data.columns[section])
        return None

    def mimeData(self, indexes):
        mimedata = QMimeData()
        buffer = QBuffer()
        buffer.open(QIODevice.ReadWrite)
        self._data.to_parquet(buffer)
        mimedata.setData('application/octet-stream', buffer.data())
        return mimedata
```

>将这个类放入到逻辑py文件中，接着在下面代码中进行引用

```python
table_model = PandasModel(dataframe)
self.ui.tableView.setModel(table_model)
```

