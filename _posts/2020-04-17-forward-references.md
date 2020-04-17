---
title: Python Forward References
date: 2020-04-17 17:00:00
tags: Python
---

Python3中加入了对 `Type` 的支持，但是在下面的例子中：

```python
class Node:

    def __init__(self, val: int, left_child: Node=None, right_child: Node=None):
        pass
```

Python会抱怨`Node is not defined.`.出现这种情况的原因是，我们在Node的定义还没有完全结束的时候引用了它。解决的方法是使用`string`来标注这个type:

```python
class Node:

    def __init__(self, val: int, left_child: 'Node'=None, right_child: 'Node'=None):
        pass
```

这种方法叫做 [Forward References](https://www.python.org/dev/peps/pep-0484/#forward-references).
