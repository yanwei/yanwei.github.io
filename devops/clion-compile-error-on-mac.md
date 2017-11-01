# Mac上升级XCode后CLion项目编译失败

```json
{"Author":"yanwei", "LastUpdate":"2017-11-01"}
```

## 问题描述

升级到XCode 9.1后，原有的CLion C/C++项目编译失败，提示：
`fatal error: 'stdio.h' file not found`

## 解决步骤

* 建立一个新工程，编译正常。说明环境OK，问题出在原有的项目编译选项上。
* 研究了一下CLion工程的文件结构，在cmake-build-debug目录下发现一个文件`CMakeCache.txt`，删除这个文件后，点击鼠标右键，选择“Reload CMake Project”，重新编译，一切正常。

## 问题分析

* XCode升级后，C/C++的的头文件路径发生了变化；
* 但是CLion的CMakeCache.txt文件中保存的还是老版本的路径；
* 删除后重建，问题解决。