# pip更新全部packages

<link rel="stylesheet" type="text/css" href="../../auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2017-11-27"}
```

## 问题描述
pip可以用`pip install --upgrade xxx`更新指定的package，但是并没有提供一个更新全部的命令。要是有类似`--upgrade all`或`--upgrade *`这样的方式该有多好。

## 解决方案
幸好我们有万能的Python，3行代码即可搞定。

### 创建一个py文件

```
$ vi pip-upgrade-all.py
```

### 输入如下代码

```python
import os

for line in os.popen('pip list').readlines():
    os.system('pip install --upgrade ' + line.split(' ')[0])
```

### 需要时执行如下命令即可

```
$ python pip-upgrade-all.py
```
