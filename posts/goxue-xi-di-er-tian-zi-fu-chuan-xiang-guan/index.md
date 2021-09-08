<!--
.. title: Go学习第二天-字符串相关
.. slug: goxue-xi-di-er-tian-zi-fu-chuan-xiang-guan
.. date: 2021-09-08 21:06:45 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Go学习第二天-字符串相关

如果有不确定的地方, 可以在a tour of go上做做小实验

## byte和rune

> Golang has integer types called byte and rune that are aliases for uint8 and int32 data types, repectively.

所以byte和rune本质上就是uint8和uint32, byte用于ASCII编码, rune用于Unicode编码

除此之外, 在golang中, 没有char类型, 用于byte和rune表示char类型

值得注意, golang中的int和int32与int64并不是一个类型, [int vs int32 vs 34](https://golang.org/ref/spec#Numeric_types), 因此下面的代码并不合法
```
strconv.ParseInt("123", 10, 32) // ok
var a int = 10
strconv.ParseInt("123", a, 32) // error, cannot use a (type int32) as type int in argument to strconv.ParseInt
```

> In Go, Characters are expressed by enclosing them in single quotes like this: 'a'.

在Go中, 用单引号来实现char的literal

默认情况下, char用rune来存储, 如下: 
```golang
var myLetter = 'a' // Type inferred as rune
```

我们也可以显示声明一个byte存储的char, 如下: 
```golang
var myLetter byte = 'a'
```

在上述表述中, char不是一个具体的类型

## 字符串

在golang中, 字符串并不是byte数组或rune数组, 因为byte数组和rune数组都是可变，但字符串不可变(和除了C++以外的其他语言一样), 

除此之外, 还有一个值得注意的是, byte数组和rune数组用fmt.Println()打印出来都是数字, 说明他们本质上仍然是uint8数组和uint32数组, 如果要打印出可读的字符串, 还是需要转成string

如下:
```go 
package main

import (
    "fmt"
) 

func main() {
    a := "张明阳是傻逼"
    fmt.Println(a[0:3]) // => 张
    fmt.Println([]rune(a)[0:3]) // => [24352 26126 38451]
    fmt.Println(string([]rune(a))[0:3]) // => 张明阳
}
```

## bytes slice over string?

由于string不可变, 因此在刷题时并不实用, 更实用的方法是通过bytes数组进行字符数据存储和操作, 只在需要显示时才将其转化为string

## strings包

string是go的内置类型, 而strings是针对string的包, 包含了处理string的诸多方法, 在此我们只列举部分常用函数

```
strings.Compare(s1, s2 string) int // 字符串比较
strings.Join([]string, string) // 拼接多个字符串
strings.Split(s, str string) // 切割字符串
strings.Fields(s strings) // 按空白字符切割字符串
strings.Repeat(s string, n int) string // 重复字符串
strings.Replace("张明阳是傻逼", "傻逼", "纯弱智", -1) // 替换字符串, -1表示替换所有, 如果大于0表示替换几个 // => 张明阳是纯弱智
strings.Contains(s, substr string) // 是否存在子串
strings.ContainsAny(s, chars string) // chars中的字符是否在s中出现过
strings.Index(s string, str string) // 返回str在s中第一次出现的位置, 如果不存在返回-1, str为空返回0
```
