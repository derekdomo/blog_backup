---
title: Golang note(1)
date: 2017-03-01 22:40:05
tags: [新知识, Golang]
categories: [Language]
---
一般的struct只有data，在golang里，可以定义方法，通过以下的方法，可以把一个method和一个struct联系在一起。

```
func (接受者 接受类型) method_name() return_type {
...
...
}
```

举个例子

```
type rect struct {
    width, height int
}

func (r rect) area_v1() int {
    return r.width * r.height
}
```
这里还有一种定义方法:

```
func (r *rect) area_v2() int {
    return r.width * r.height
}
```
讲道理，调用方法应该是：

```
r1 := Rectangle{4, 3}
r1.area_v1()
(&r1).area_v2()
```
但是其实也可以这样子:

```
r1 := Rectangle{4, 3}
r1.area_v2()
(&r1).area_v1()
```
而且结果也是一样的。这是因为golang自动做了一些类型转换。但是一般是比较推荐用指针作为接受类型，这样子可以直接修改值。

