# ES6利用解构赋值（Destructuring Assignment）交换两个变量的值

<link rel="stylesheet" href="https://yanwei.github.io/auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2018-11-21"}
```

## 解构赋值

JavaScript ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。利用这个特性，可以很方便的交换两个变量的值。

```javascript
var x = 1;
var y = 2;

[x, y] = [y, x];
console.log(x, y);
```

可以看到，输出结果为`2 1`，x，y的值完美的实现了交换。

## 一点疑问

但是，等等，了解Python的同学一定很熟悉下面这段代码：

```python
x = 1
y = 2

x, y = y, x
print(x, y)
```

那么，上面的JavaScript代码如果写成下面这样，会发生什么情况呢？

```javascript
var x = 1;
var y = 2;

x, y = y, x;
console.log(x, y);
```

可以看到，输出结果还是`1 2`。x，y的值既没有发生变化，运行时也没有报错。这是为什么呢？

## 现象解释

解释这个现象需要回到JavaScript的语法基础。我们再看一个例子：

```javascript
var x, y;

x, y = 1, 2;
console.log(x, y);
```

输出结果为`undefined 1`。很显然，x没有被赋值，y被赋值成了1。这是因为，`x, y = 1, 2`实际上是用逗号`,`连接的三个表达式：expr1:`x`, expr2:`y = 1`, expr3:`2`

> 逗号操作符：对它的每个操作数求值（从左到右），并返回最后一个操作数的值。

> 语法：expr1, expr2, expr3...

表达式`x`什么都没有做，表达式`y = 1`对y赋值了1，表达式`2`什么也没有做。最后的结果是，x仍然是undefined，y是1。

同样，`x, y = y, x`中，x的值没有变化，y赋值给y自己，所以最后依然是x等于1，y等于2。

这就是为什么解构赋值必须基于数组或对象的原因了。

## 回到Python语法

最后解释一下Python为什么可以用`x, y = y, x`的方式交换变量的值。

这是因为，Python的语法规定，`x, y`实际上是一个元组（Tuple），`x, y = y, x`等同于`(x, y) = (y, x)`，交换赋值得以顺利进行。同样，在Python语法中，`return x, y`也是隐含表示返回一个元组，等同于`return (x, y)`。

```python
def foo():
    return 1, 2

x, y = foo()
```

等同于：

```python
def foo():
    return (1, 2)

(x, y) = foo()
```

> 看似最常见的逗号操作符，带来的语法影响却是深远的。学习一门语言，不能满足于代码可以运行，还是必须回到最基本的原理。