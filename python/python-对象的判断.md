Python中的对象包含三要素：id、type、value

其中id用来唯一标识一个对象，type标识对象的类型，value是对象的值

is 判断的是a对象是否就是b对象，是通过id来判断的

== 判断的是a对象的值是否和b对象的值相等，是通过value来判断的

如下代码或许可以帮助你理解。

    >>> a = 1
    >>> b = 1.0
    >>> a is b
    False
    >>> a == b
    True
    >>> id(a)
    12777000
    >>> id(b)
    14986000
    >>> a = 1
    >>> b = 1
    >>> a is b
    True
    >>> a == b
    True
    >>> id(a)
    12777000
    >>> id(b)
    12777000
