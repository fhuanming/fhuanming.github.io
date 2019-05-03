---
title: Python? Unicode? UTF-8?
date: 2019-04-18 22:00:00
tags: Python Unicode UTF-8 ASCII
---

一直以来Python的Unicode问题我就没有彻底弄明白，这次趁着工作上的机会，将这个问题彻底梳理一下。

很感谢[The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)这篇Blog, 给了我很大的帮助。

<!-- more -->

# 首先从Unicode和UTF-8的概念谈起

很久之前，通用的字符编码标准是ASCII码。ASCII码用了一个Byte表示了Control Character和Printable Character. 可是Printable Character只包含了英语字符。这给非英语地区的使用带来了麻烦。Unicode的诞生便是要解决这个问题。

Unicode想用一个通用的码表来表示地球上的绝大多数字符。Unicode引入的一个特别的概念： Code Point. 在ASCII码或者其他编码方式中，字符都是直接映射到一串比特，而Unicode的做法是将字符映射到Code Point. 比如说“文”的Code Point就是`U+6587`。 那么Code Point是如何保存在磁盘上的呢？这里面就涉及到了Unicode的不同的encoding方法。最早的encoding就是将`U+6587`直接存在硬盘上，也就是用两个Bytes分别存储`0x65`和`0x87`。但是对于纯英文文件，很少有字符是在`U+00FF`之上的。这样子带来的问题是文件里面会有一半的`0x00`。于是UTF-8就诞生了。在UTF-8中，0～127的Code Point存储在一个Byte里面。128以及之后的存储在2，3到6个Bytes中。比如“文”(`U+6587`)在UTF-8中对应的编码就是`\xe6\x96\x87`，即三个Bytes. 所以说Unicode或者UTF-8用两个Bytes的说法是错误的。除了UTF-8，Unicode还有很多其他的encoding方式。

# Python与Unicode

说完Unicode以及UTF-8，我们再来看看在Python是怎么处理他们的。Python 2 和Python 3 在对待字符串的方式上有很大改变，我们来分别讨论。

## Python 2

如果没有改变的话，python 2 默认的编码方式是ASCII码。让我们用实验验证一下。

### 实验1

创建文件`unicode_test.py`， 内容如下：

```python
a = 'text:中文'
print(a)

ua = u'text:中文'
print(ua)
```

保存完后，看一下文件的编码方式：

```shell
$ file -I ./unicode_test.py
./unicode_test.py: text/plain; charset=utf-8
```

可以发现是UTF-8编码。然后运行该文件：

```shell
$ python ./unicode_test.py 

  File "./unicode_test.py", line 1

SyntaxError: Non-ASCII character '\xe4' in file ./unicode_test.py on line 1, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

可以发现尽管文件是以UTF-8存储的，但是Python的解释器仍然按照ASCII码去分析文件，导致了上面的错误。我们需要做的是按照上面的错误提示里的网页提到的，在Python的文件的开头加上`# coding=utf-8`来告诉Python这个文件是以UTF-8编码保存的。

```python
# coding=utf-8
a = 'text:中文'
print(a)

ua = u'text:中文'
print(ua)
```

当你再次运行该文件时，会发现运行成功了，Python打印出了两行一样的文字`text:中文`。那么`'some_str'`和`u'some_str'`的差别在什么地方呢，让我们做实验2.

### 实验2

将文件改写成下面的样子：

```python
# "coding=utf-8"
a = 'text:中文'
print(a)
print('%r' % a)
print(type(a))

ua = u'text:中文'
print(ua)
print('%r' % ua)
print(type(ua))
```

保存后，运行该文件：

```shell
$ python ./unicode_test.py
text:中文
'text:\xe4\xb8\xad\xe6\x96\x87'
<type 'str'>
text:中文
u'text:\u4e2d\u6587'
<type 'unicode'>
```

可见

1. `'some_str'`是 `<type 'str'>`， 而`u'some_str'`是`<type 'unicode'>`.
1. `<type 'str'>`还保留着UTF-8的Raw Byte， 而`<type 'unicode'>`已经将其解码成了Code Point.

在`<type 'str'>`和`<type 'unicode'>`之间，我们可以手动做如下转换。

```python
a.decode('utf8') == ua # True
ua.encode('utf8') == a # True
```

## Python 3

而到了Python 3，情况变不一样了。Python 3 默认是UTF-8编码，所以代码里面的str可以包含任意Unicode.

### 实验3

我们再来做一个实验，这次去掉上面代码中的`# "coding=utf-8"`.

```python
a = 'text:中文'
print(a)
print('%r' % a)
print(type(a))

ua = u'text:中文'
print(ua)
print('%r' % ua)
print(type(ua))
```

然后用`python3`去运行：

```shell
$ python3 ./unicode_test.py 
text:中文
'text:中文'
<class 'str'>
text:中文
'text:中文'
<class 'str'>
```

你会发现，首先Python 3 的解释器并没有报错。其次无论是`'some_str'` 还是`u'some_str'`，在赋值完成后，都被当作是`<class 'str'>`来对待。也就是说从概念上来说，Python 3 的`<class 'str'>`等于Python 2 的`<type 'unicode'>`. Python 3 引入了`bytes`, 在概念上 Python 3 的`<class 'bytes'>`等于Python 2 的 `<type 'str'>`.让我们在做最后一个实验验证这一点。

### 实验4

```python
a = 'text:中文'
print(a)
print('%r' % a)
print(type(a))

ua = u'text:中文'
print(ua)
print('%r' % ua)
print(type(ua))

ba = ua.encode('utf-8')
print(ba)
print('%r' % ba)
print(type(ba))
```

```shell
$ python3 ./unicode_test.py 
text:中文
'text:中文'
<class 'str'>
text:中文
'text:中文'
<class 'str'>
b'text:\xe4\xb8\xad\xe6\x96\x87'
b'text:\xe4\xb8\xad\xe6\x96\x87'
<class 'bytes'>
```

## Python的Encode和Decode的方向

Unicode --> encode --> Bytes

Unicode <-- decode <-- Bytes

## Best Practice of Handling str/unicode in Python 2

在遇到到str后，马上将其转换成Unicode (by calling `.decode('utf-8')`). 然后始终处理Unicode而不是str. 当你最后要将其写入文件的时候在将其转化为str (by calling `.encode('utf-8')`). 在Python 3 中可以省略这些。

# 相关资料
最后的最后，放几篇相关的，帮助我理解这个问题的Blog:

* [The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)

* [Unicode strings in Python: A basic tutorial](http://www.pgbovine.net/unicode-python.htm)

* [Strings, Bytes, and Unicode in Python 2 and 3](https://timothybramlett.com/Strings_Bytes_and_Unicode_in_Python_2_and_3.html)