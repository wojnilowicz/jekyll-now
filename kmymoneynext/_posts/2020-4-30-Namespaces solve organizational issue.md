---
layout: post
title: Namespaces solve organizational issue
---

Development work described [here]({% post_url kmymoneynext/2020-4-29-Qt Quick and Qt Widgets interchangeably %}), has led to multiple [modules](https://en.wikipedia.org/wiki/Modular_programming) carrying out the same function. A need has arisen, to name the modules in a way readily discernible to the human eye.

A simplified [directory structure](https://en.wikipedia.org/wiki/Directory_structure) (as a [tree](https://en.wikipedia.org/wiki/Tree_(command)) view) of a storage manager and its two storage plugins is shown below.
<div id="codesInRow" markdown="1">
```
.
├── manager
│   └── frontend
│       ├── quick
│       │   └── storagemanageropenquick.h
│       ├── widget
│       │   └── storagemanageropenwidget.h
│       └── storagemanageropen.h
├── sql
│   ├── backend
│   │   └── sqlstoragebackend.h
│   └── frontend
│       ├── quick
│       │   └── sqlstorageopenquick.h
│       ├── widget
│       │   └── sqlstorageopenwidget.h
│       └── sqlstorageopen.h
└── xml
    ├── backend
    │   └── xmlstoragebackend.h
    └── frontend
        ├── quick
        │   └── xmlstorageopenquick.h
        ├── widget
        │   └── xmlstorageopenwidget.h
        └── xmlstorageopen.h
```
</div>

It can be seen that the file names:
1. are relatively long and thus not readily discernible,
2. contain repetitive prefixes and suffixes.

Naming convention, used above, is explained below.

![](/images/2020-04-30/filename-convention.png)

The naming convention incorporates all the parts to avoid potential [name collisions](https://en.wikipedia.org/wiki/Name_collision). It's used for [class](https://en.wikipedia.org/wiki/C%2B%2B_classes) names (obligatory) and file names (voluntarily) in the same way. This approach is considered useful in a sense, that it's easy to find a class name by a file name, and in reverse. This usefulness is limited to short names though.

----

As a remedy, it has been decided to use [namespaces](https://en.cppreference.com/w/cpp/language/namespace). They have taken prefixes and suffixes, incorporated earlier in class names, on themselves. As a result, classes for opening can be referred by a short **Open** identifier, as shown below.

<div id="codesInRow" markdown="1">
```c++
namespace Storage {
namespace Xml {
namespace Widget {
Open::method-1();
Open::method-2();
...
Open::method-n();
}
}
}
```
```c++
namespace Storage {
namespace Sql {
namespace Widget {
Open::method-1();
Open::method-2();
...
Open::method-n();
}
}
}
```
</div>

Instead by a full identifier only, as shown below.

<div id="codesInRow" markdown="1">
```c++
XmlStorageOpenWidget::method-1();
XmlStorageOpenWidget::method-2();
...
XmlStorageOpenWidget::method-n();
```
```c++
SqlStorageOpenWidget::method-1();
SqlStorageOpenWidget::method-2();
...
SqlStorageOpenWidget::method-n();
```
</div>

Content of xml/frontend/widget directory from the application's code, before and after applying namespaces, is shown below for comparison.

![](/images/2020-04-30/xml-widget-open-directory-structure.png)

As one can see, the content on the right side is more readily discernible.

As for the numbers:
1. 122 from 186 files in the storage manager benefit from this design;
2. 17 unique names have been reused instead of prefixed, or suffixed for uniqueness.