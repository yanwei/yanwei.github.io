# pip3更新全部packages

<link rel="stylesheet" type="text/css" href="../../auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2018-05-03"}
```

## 问题描述
在[上一篇文章](pip-upgrade-all.md)中描述了用Python脚本解决pip3更新全部package的问题。前几天pip3更新了10.0版本，`pip3 list`的输出格式发生了变化，导致原来的脚本无法正常运行了。

## 解决方案
用`pip3 list --outdated --format json`获取需要更新的package列表，并调用`pip3 install -U`进行更新。

### 创建一个py文件

```
$ vi pip3-upgrade-all.py
```

### 输入如下代码

```python
import json
import os

package_list = json.loads(os.popen('pip3 list --outdated --format json').readlines()[0])

for package in package_list:
    os.system('pip3 install -U' + package['name'])
```

### 需要时执行如下命令即可

```
$ python pip3-upgrade-all.py
```
