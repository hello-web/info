其他图形用户界面（GUI）包
*************************

Python 可用的主要跨平台（Windows，Mac OS X，类Unix）GUI 工具：

参见:

  PyGObject
     PyGObject 使用 GObject 提供针对 C 库的内省绑定。 GTK+ 3 可视化部
     件集就是此类函数库中的一个。 GTK+ 附带的部件比 Tkinter 所提供的更
     多。 请在线参阅 Python GTK+ 3 教程。

  PyGTK
     PyGTK 提供了对较旧版本的库 GTK+ 2 的绑定。 它使用面向对象接口，比
     C 库的抽象层级略高。 此外也有对 GNOME 的绑定。 请参阅在线 教程。

  PyQt
     PyQt 是一个针对 Qt 工具集通过 **sip** 包装的绑定。 Qt 是一个庞大
     的 C++ GUI 应用开发框架，同时适用于 Unix, Windows 和 Mac OS X。
     **sip** 是一个用于为 C++ 库生成 Python 类绑定的库，它是针对
     Python 特别设计的。

  PySide
     PySide 是一个较新的针对 Qt 工具集的绑定，由 Nokia 提供。 与 PyQt
     相比，它的许可方案对非开源应用更为友好。

  wxPython
     wxPython 是一个针对 Python 的跨平台 GUI 工具集，它基于热门的
     wxWidgets (原名 wxWindows) C++ 工具集进行构建。 它为 Windows, Mac
     OS X 和 Unix 系统上的应用提供了原生的外观效果，在可能的情况下尽量
     使用各平台的原生可视化部件。 （在类 Unix 系统上使用 GTK+）。 除了
     包含庞大的可视化部件集，wxPython 还提供了许多类用于在线文档和上下
     文感知帮助、打印、HTML 视图、低层级设备上下文绘图、拖放操作、系统
     剪贴板访问、基于 XML 的资源格式等等，并且包含一个不断增长的用户贡
     献模块库。

PyGTK, PyQt 和 wxPython 都拥有比 Tkinter 更现代的外观效果和更多的可视
化部件。 此外还存在许多其他适用于 Python 的 GUI 工具集，既有跨平台的，
也有特定平台专属的。 请参阅 Python Wiki 中的 GUI 编程 页面查看更完整的
列表，以及不同 GUI 工具集对比文档的链接。
