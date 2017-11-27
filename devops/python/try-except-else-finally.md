# Python中的 try / except / else / finally 语句

```json
{"Author":"yanwei", "At":"Shanghai", "LastUpdate":"2017-09-27"}
```

与其他语言相同，在python中，try/except语句主要是用于处理程序正常执行过程中出现的一些异常情况，如语法错误（python作为脚本语言没有编译的环节，在执行过程中对语法进行检测，出错后发出异常消息）、数据除零错误、从未定义的变量上取值等；而try/finally语句则主要用于在无论是否发生异常情况，都需要执行一些清理工作的场合，如在通信过程中，无论通信是否发生错误，都需要在通信完成或者发生错误时关闭网络连接。尽管try/except和try/finally的作用不同，但是在编程实践中通常可以把它们组合在一起使用try/except/else/finally的形式来实现稳定性和灵活性更好的设计。

默认情况下，在程序段的执行过程中，如果没有提供try/except的处理，脚本文件执行过程中所产生的异常消息会自动发送给程序调用端，如python shell，而python shell对异常消息的默认处理则是终止程序的执行并打印具体的出错信息。这也是在python shell中执行程序错误后所出现的出错打印信息的由来。

```python
try:
    # Normal execution block
except A:
    # Exception A handle
except B:
    # Exception B handle
except:
    # Other exception handle
else:
    # if no exception, get here
finally:
    print("finally")
```
说明：
正常执行的程序在try下面的Normal execution block执行块中执行，在执行过程中如果发生了异常，则中断当前在Normal execution block中的执行跳转到对应的异常处理块中开始执行；

python从第一个except X处开始查找，如果找到了对应的exception类型则进入其提供的exception handle中进行处理，如果没有找到则直接进入except块处进行处理。except块是可选项，如果没有提供，该exception将会被提交给python进行默认处理，处理方式则是终止应用程序并打印提示信息；

如果在Normal execution block执行块中执行过程中没有发生任何异常，则在执行完Normal execution block后会进入else执行块中（如果存在的话）执行。
无论是否发生了异常，只要提供了finally语句，以上try/except/else/finally代码块执行的最后一步总是执行finally所对应的代码块。

需要注意的是：

1. 在上面所示的完整语句中try/except/else/finally所出现的顺序必须是try-->except X-->except-->else-->finally，即所有的except必须在else和finally之前，else（如果有的话）必须在finally之前，而except X必须在except之前。否则会出现语法错误。
2. 对于上面所展示的try/except完整格式而言，else和finally都是可选的，而不是必须的，但是如果存在的话else必须在finally之前，finally（如果存在的话）必须在整个语句的最后位置。
3. 在上面的完整语句中，else语句的存在必须以except X或者except语句为前提，如果在没有except语句的try block中使用else语句会引发语法错误。也就是说else不能与try/finally配合使用。
4. except的使用要非常小心，慎用。由于使用了except语句，语法错误会被掩盖掉。因此在使用try/except是最好还是要非常清楚的知道Normal execution block中有可能出现的异常类型以进行针对性的处理。