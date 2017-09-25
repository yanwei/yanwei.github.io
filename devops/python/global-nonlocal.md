# Python3中`global`和`nonlocal`的区别
```json
{"Author":"yanwei", "At":"Shanghai", "LastUpdate":"2017-09-25"}
```

Python3中引入了`nonlocal`关键字，很多人搞不清楚它和`global`的区别。

先看一个网上能找到的例子：

```python
def scope_test():
    def do_local():
        spam = "local spam"
    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"
    def do_global():
        global spam
        spam = "global spam"
    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```

运行结果如下：

```
After local assignment: test spam
After nonlocal assignment: nonlocal spam
After global assignment: nonlocal spam
In global scope: global spam
```

看到这里就不理解了，为什么调用`do_global()`之后，`spam`的值还是`nonlocal spam`呢？

---

实际上百度搜到的大多数中文博客，都没有解释清楚`global`和`nonlocal`的区别。

```python
global_value = 0  # 这是定义在模块内的全局变量

def scope_test():
    nonlocal_value = 1  # 这才是相对于内部函数而言的外层变量
    def do_nonlocal():
        nonlocal nonlocal_value  # 引用外层变量，而不是全局变量
        nonlocal_value = 2
        
    def do_global():
        global global_value  # 引用全局变量，而不是外层变量
        global_value = 3

```

现在再回去看最前面的例子，就能理解了。实际上`do_global()`函数中的`global spam`并不是引用了外层函数的`spam`变量，而是定义了一个全新的`global`变量。

我们再次验证一下。注释掉`scope_test`函数中的`do_global`调用，

```python
def scope_test():
    def do_local():
        spam = "local spam"
    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"
    def do_global():
        global spam
        spam = "global spam"
    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    # do_global()  #  把这一行注释掉
    # print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)  # 这一行就会出错
```

```
Traceback (most recent call last):
  File "test.py", line 23, in <module>
    print("In global scope:", spam)
NameError: name 'spam' is not defined
```

因为不调用`do_global`，`global`的`spam`变量就不会被定义，直接在全局作用域访问`spam`当然就会出错咯。

---

参考1: [这篇英文博客](https://www.smallsurething.com/a-quick-guide-to-nonlocal-in-python-3/)解释的比较清楚<br/>
参考2: https://www.techforgeek.info/global_nonlocal.html
