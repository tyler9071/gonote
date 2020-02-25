


# **golang 笔记**

收集于网络



# 开发标准

## 文件命名

文件命名一律采用小写，不用驼峰式，尽量见名思义，看见文件名就可以知道这个文件下的大概内容。
其中测试文件以_test.go结尾，除测试文件外，命名不出现_。

例子：

> stringutil.go， stringutil_test.go

## package

包名用小写,使用短命名,尽量和标准库不要冲突。
包名统一使用单数形式。

## 变量

变量命名一般采用驼峰式，当遇到特有名词（缩写或简称，如DNS）的时候，特有名词根据是否私有全部大写或小写。



```go
var A int //如果在全局使用首字母大写则表示

var a int //全局定义可以在package中任意调用
a:=int
var a string = "Runoob"

var (  // 这种因式分解关键字的写法一般用于声明全局变量
    a int
    b bool
)

//在func 中的变量只能在当前函数中使用
func main(){
    g, h := 123, "hello"
    println(x, y, a, b, c, d, e, f, g, h)
}
```



例子：

> 小驼峰:apiClient
>
> 大驼峰:URLString

## 常量

同变量规则，力求语义表达完整清楚，不要嫌名字长。
如果模块复杂，为避免混淆，可按功能统一定义在`package`下的一个文件中。

```go
1.
	const LENGTH int = 10
   const WIDTH int = 5  

2.
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)
```





## 字符串格式化

时常用动词及功能

```go
动  词		功  能
%v			按值的本来值输出
%+v			在 %v 基础上，对结构体字段名和值进行展开
%#v			输出 Go 语言语法格式的值
%T			输出 Go 语言语法格式的类型和值
%c  		字符
%s  		字符串 
%%			输出 % 本体
%b			整型以二进制方式显示
%o			整型以八进制方式显示
%d			整型以十进制方式显示
%x			整型以十六进制方式显示
%X			整型以十六进制、字母大写方式显示
%U			Unicode 字符
%f			浮点数
%p			指针，十六进制方式显示
```





## 接口

单个函数的接口名以 `er` 为后缀

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

两个函数的接口名综合两个函数名，如:

```go
type WriteFlusher interface {
    Write([]byte) (int, error)
    Flush() error
}
```

三个以上函数的接口名类似于结构体名，如:

```go
type Car interface {
    Start() 
    Stop()
    Drive()
}
```

## 结构体

结构体名应该是名词或名词短语，如`Account`,`Book`，避免使用`Manager`这样的。
如果该数据结构需要序列化，如`json`， 则首字母大写， 包括里面的字段。

## 方法

方法名应该是动词或动词短语，采用驼峰式。将功能及必要的参数体现在名字中， 不要嫌长， 如updateById，getUserInfo.

如果是结构体方法，那么 Receiver 的名称应该缩写，一般使用一个或者两个字符作为 Receiver 的名称。如果 Receiver 是指针， 那么统一使用p。 如：

```go
func (f foo) method() {
    ...
}
func (p *foo) method() {
    ...
}
```

对于Receiver命名应该统一， 要么都使用值， 要么都用指针。

# 注释

每个包都应该有一个包注释，位于 package 之前。如果同一个包有多个文件，只需要在一个文件中编写即可；如果你想在每个文件中的头部加上注释，需要在版权注释和 Package前面加一个空行，否则版权注释会作为Package的注释。如：

```go
// Copyright 2009 The Go Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.
package net
```

每个以大写字母开头（即可以导出）的方法应该有注释，且以该函数名开头。如：

```go
// Get 会响应对应路由转发过来的 get 请求
func (c *Controller) Get() {
    ...
}
```

大写字母开头的方法以为着是可供调用的公共方法，如果你的方法想只在本包内掉用，请以小写字母开发。如:

```go
func (c *Controller) curl() {
    ...
}
```

注释应该用一个完整的句子，注释的第一个单词应该是要注释的指示符，以便在 godoc 中容易查找。

注释应该以一个句点 . 结束。

# README

每个文件夹下都应该有一个README文件，该文件是对当前目录下所有文件的一个概述，和主要方法描述。并给出一些相应的链接地址，包含代码所在地、引用文档所在地、API文档所在地，以以太坊的README文档为例

基本在所有语言当中，关键字都是不允许用于自定义的，在Golang中有25个关键字，图示如下：





![d](C:\Users\lok\OneDrive\golang 笔记 .assets\5b45c15a00010d5d10240211.jpg)







下面我们逐个解析这25个关键字。



## package && import (引入包)

包是函数和数据的集合。用 package 关键字定义一个包，用 import 关键字引入一个包，文件名不需要和包名一致。包名的约定是使用小写字符。Go 包可以由多个文件组成，但是使用相同的 package 这一行。

```go
package main
```



当函数或者变量等首字母为大写时，会被导出，可在外部直接调用。

包名是导入的默认名称。可以通过在导入语句指定其他名称来覆盖默认名称

```go
import bar "bytes"  
```



## func （定义函数）

使用关键字 func 定义函数

```go
func test(a, b int) int {   
    return a + b 
}  
```

其中，有返回值的函数，必须有明确的终止语句，否则会引发编译错误。

函数是可变参的，变参的本质上是slice，只能有一个，且必须是最后一个，如

```go
func test(s string, n ...int) string {   
    var x int   
    for _, i := range n {     
        x += i   
    }   return fmt.Sprintf(s, x) 
}  
```

Golang 函数支持多返回值。这个特性在 Go 语言中经常被用到，例如用来同时返回一个函数的结果和错误信息。

```go
func vals() (int, int) {   
    return 3, 7 
}  
```

### return(返回值) 

defer用于资源的释放，会在函数返回之前进行调用。一般采用如下模式：

```go
func test() {   
    f, err := os.Open(filename)   
    if err != nil {       
        panic(err)   
    }   
    defer f.Close() 
}  
```

如果有多个defer表达式，调用顺序类似于栈，越后面的defer表达式越先被调用，即后入先出的规则。

```go
func test() { 
    defer fmt.Println(1) 
    defer fmt.Println(2) 
}  
```

输出结果为

```go
2 1  
```

为了更深刻理解 defer 和 return 下面我们先来看几个例子。

例1：

```go
func f() (result int) {   
    defer func() {     
        result++   
    }()   
    return 0 
}  
```

例2：

```go
func f() (r int) {   
    t := 5   
    defer func() {     t = t + 5   
                 }()   
    return t 
}  
```

例3：

```go
func f() (r int) {   
    defer func(r int) {     
        r = r + 5   
    }(r)   
    return 1 
}  
```

函数返回的过程是这样的：先给返回值赋值，然后调用defer表达式，最后才是返回到调用函数中。

defer表达式可能会在设置函数返回值之后，在返回到调用函数之前，修改返回值，使最终的函数返回值与你想象的不一致。

其实使用defer时，用一个简单的转换规则改写一下，就不会迷糊了。改写规则是将return语句拆成两句写，return xxx会被改写成:

```go
返回值 = xxx 调用defer函数 空的return  
```

下面我们针对上面的三个例子分析，先看例1，它可以改写成这样：

```go
func f() (result int) {   
    result = 0  //return语句不是一条原子调用，return xxx其实是赋值＋ret指令   
    func() { 
    //defer被插入到return之前执行，也就是赋返回值和ret指令之间     
    result++   
}()   return 
}  
```

所以这个返回值是1。

例2，它可以改写成这样：

```go
func f() (r int) {   
    t := 5   
    r = t //赋值指令   
    func() {        
        //defer被插入到赋值与返回之间执行，这个例子中返回值r没被修改过     
        t = t + 5   
    }   return        //空的return指令 }  
```

所以这个返回值是5。

例3，它可以改写成这样：

```go
func f() (r int) {   
    r = 1  //给返回值赋值   
    func(r int) { 
        //这里改的r是传值传进去的r，不会改变要返回的那个r值     
        r = r + 5   
    }(r)   return //空的return }  
```

所以这个返回值是1。

## var(定义变量) && const（定义常量）

使用var关键字是Go最基本的定义变量方式，有时也会使用到 := 来定义变量。

定义变量

```go
var name string  
```

定义变量并初始化值

```go
var name string = "keywords"  
```

同时初始化多个同类型变量

```go
var name1, name2, name3 string = "name1", "name2", "name3"  
```

同时初始化多个不同类型变量

```go
var (   name string = "keywords",   count int = 2 )  
```

也可省略变量类型

```go
var name1, name2, name3 = "name1", "name2", "name3"  
```

使用 := 这个符号取代var和type,这种形式叫做简短声明。不过它有一个限制，那就是它只能用在函数内部；在函数外部使用则会无法编译通过，所以一般用var方式来定义全局变量。

```go
name1, name2, name3 := "name1", "name2", "name3"  
```

const 用来声明一个常量，const 语句可以出现在任何 var 语句可以出现的地方，声明常量方式与 var 相同，这里就不在赘述了。但是需要注意的是，const 声明的是常量，一旦创建，不可赋值修改。

## 容器：***数组Array、切片Slice、字典Map***

### []数组

Go 语言中数组的定义有两种方式 
1. 使用 `var` 来定义 语法如下: 

  ```go
  var name[length]Type
  ```

  定义数组需要定义其 名称,长度,数组里面保存的数据类型 

  例如: 

  ```go
  var index [10] int 
  var name [20] string 
  ……… 
  ```

2. 如果在函数内部定义数组并且给数组初始化某些值则可以如下的方式来定义 

  ```go
  arrys := [6]int{1, 2, 3, 4, 5, 6}
  ```

数组的默认元素
默认情况下数组定义时没初始化，则数组都会有默认值，如果是`int` 类型的数组, 里面的默认值都是 0 ,如果是 `string`类型的数组 则默认是 `” ” `
例如 下面的数组: 

```go
var index [10] int 
```

默认初始化为

```
 [0,0,0,0,0,0,0,0,0,0]
```

获取数组的长度
数组的长度可以使用len 函数来获取 
例如： 

```go
a1 :=[6] int {1,2,3,4,5,6} 
length := len(a1)
```





### slice(切片)



切片的定义

切片不存值，它就像一个框，去底层数组框值。直接指向数组的内存地址

切片：指针、长度、容量

切片的扩容策略：

```
1. 如果申请的容量大于原来的2倍，那就直接扩容至新申请的容量
2. 如果小于1024， 那么就直接两倍
3. 如果大于1024，就按照1.25倍去扩容
4. 具体存储的值类型不同，扩容策略也有一定的不同。
```

`slice`和`数组`有着紧密的联系 
例如 数组 `a := [6] int {1,2,3,4,5,6}` 我们使用使用切片的方式获取第1 – 3 个元素 
`a1 := a[0:2]` 
这种方式时 `a1` 就已经是`slice` 类型的 可以对其进行 `append` 操作 
`a2 := append(a1, 20) `
`数组`是不支持`append`的，但是`slice` 支持 
数组支持使用 `==` 进行比较两个数组是否相等 但是`slice` 不支持



### map（字典）

map 是 Go 内置关联数据类型（在一些其他的语言中称为哈希 或者字典 ）。

创建一个空 map

```go
m := make(map[string]int)  
```

设置键值对对map赋值

```go
m["k"] = 7  
```

使用 name[key] 来获取一个键的值

```go
v := m["k"] fmt.Println("v: ", v)  
```

当对一个 map 调用内建的 len 时，返回的是键值对数目

```go
fmt.Println("len:", len(m))  
```

内建的 delete 可以从一个 map 中移除键值对

```go
delete(m, "k")  
```

当从一个 map 中取值时，可选的第二返回值指示这个键是在这个 map 中。这可以用来消除键不存在和键有零值，像 0 或者 "" 而产生的歧义。

```go
val, ok := m["k"] fmt.Println("val:", val)  
```

```go
package main

import "fmt"

func main() {
	// 创建一个字典可以使用内置函数make
	// "make(map[键类型]值类型)"
	m := make(map[string]int)
	// 使用经典的"name[key]=value"来为键设置值
	m["k1"] = 7
	m["k2"] = 13
	// 用Println输出字典，会输出所有的键值对
	fmt.Println("map:", m)
	// 获取一个键的值 "name[key]".
	v1 := m["k1"]
	fmt.Println("v1: ", v1)
	// 内置函数返回字典的元素个数
	fmt.Println("len:", len(m))
	// 内置函数delete从字典删除一个键对应的值
	delete(m, "k2")
	fmt.Println("map:", m)
	// 根据键来获取值有一个可选的返回值，这个返回值表示字典中是否
	// 存在该键，如果存在为true，返回对应值，否则为false，返回零值
	// 有的时候需要根据这个返回值来区分返回结果到底是存在的值还是零值
	// 比如字典不存在键x对应的整型值，返回零值就是0，但是恰好字典中有
	// 键y对应的值为0，这个时候需要那个可选返回值来判断是否零值。
	_, ok := m["k2"]
	fmt.Println("ok:", ok)
	// 你可以用 ":=" 同时定义和初始化一个字典
	n := map[string]int{"foo": 1, "bar": 2}
	fmt.Println("map:", n)
}
```

输出:

```go
map: map[k1:7 k2:13]
v1: 7
len: 2
map: map[k1:7]
ok: false
map: map[foo:1 bar:2]
```





### type(定义)

type是go语法里的重要而且常用的关键字，其主要作用就是用来定义类型。

#### 定义结构体

```go
type Person struct {   
    name string 
}  
```

 **类型等价定义，相当于类型重命名**

```go
type name string func main() {   
    var myname name = "golang" //其实就是字符串类型   
    fmt.Println(myname) 
}  
```

#### 定义接口

```go
type Person interface {   
    Run() 
}  
```

### struct (结构体)

Go 的结构体 是各个字段字段的类型的集合。是值类型，赋值和传参会赋值全部内容。

#### struct的基本用法

```go
type Person struct { 
    Name string 
    Age  int 
} 
func main() { 
    p := Person{ Name: "ck", Age:  20, } 
    p.Age = 25 fmt.Println(p) 
}  
```

顺序初始化必须包含全部字段。否则会报错

```go
type Person struct { 
    Name string 
    Age  int 
} 
func main() {
    p1 := Person{"ck", 20} 
    p2 := Person{"ck"
} // Error: too few values in struct initializer }  
```

支持匿名结构，可用作结构成员或定义变量

```go
type Person struct { 
    Name string 
    Attr struct{     age int   } 
}  
```

支持 "=="、"!=" 相等操作符，可用作 map 键类型。

```go
type Person struct { 
    Name string 
} 
m := map[Person]int{   
    Person{"ck"}: 10, 
}  
```

struct 支持嵌入式结构，可以像普通字段那样访问匿名字段成员，如下

```go
type Person struct { Name string } 
type User struct { Person Age int } 
func main() { 
    u := User{ Person: Person{ Name: "ck", }, Age: 22, } 
    fmt.Println(u.Name) // ck 
}  
```

当被嵌入结构中的某个字段与当前struct中已存在的字段同名时，编译器从外向内逐级查找所有层次的匿名字段，直到发现目标或者报错。

```go
type Person struct { Age int } 
type User struct { Person Age int } 
func main() { 
    u := User{ Person: Person{ Age: 20, }, Age: 22, } 
    fmt.Println(u.Age) // 22 
}  
```

如果想访问被嵌入结构Person中的Age

```go
fmt.Println(u.Person.Age) // 20  
```

#### [嵌套结构体](https://www.runoob.com/go/go-structures.html)





#### 结构体与JSON

1.结构体内部的是的字段需要大写,不然json的包不能访问

2.json需要把值转换成byte,



JSON:是一种跨语言的数据格式.多用于在不同语言间传递数据

```go
// 1.序列化:   把Go语言中的结构体变量 --> json格式的字符串
// 2.反序列化: json格式的字符串   --> Go语言中能够识别的结构体变量

type person struct {
	Name string `json:"name" db:"name" ini:"name"`  //json输出定义
	Age  int    `json:"age"`
}

func main() {
	p1 := person{
		Name: "周林",
		Age:  9000,
	}
	// 序列化
	b, err := json.Marshal(p1)
	if err != nil {
		fmt.Printf("marshal failed, err:%v", err)
		return
	}
	fmt.Printf("%v\n", string(b))
	// 反序列化
	str := `{"name":"理想","age":18}`
	var p2 person
	json.Unmarshal([]byte(str), &p2) // 传指针是为了能在json.Unmarshal内部修改p2的值
	fmt.Printf("%#v\n", p2)
}
```



### interface

首先 interface 是一种类型，从它的定义可以看出来用了 type 关键字，更准确的说 interface 是一种具有一组方法的类型，这些方法定义了 interface 的行为。

如果一个类型实现了一个 interface 中所有方法，我们说类型实现了该 interface，所以所有类型都实现了 empty interface，因为任何一种类型至少实现了 0 个方法。go 没有显式的关键字用来实现 interface，只需要实现 interface 包含的方法即可。

#### 接口定义与基本操作

```go
type Dog interface {   Category() } 
type Ha struct {   Name string } 
func (h Ha) Category() {
    fmt.Println("狗子") 
} 
func main() {
    h := Ha{"二哈"}   
    h.Category()   
    test(h) 
} 
func test(a Dog) {   
    fmt.Println("成功调用啦") 
} // 输出结果为：狗子 成功调用啦  
```

上述代码中可以看到，对于 test 函数接收的参数类型为 Dog 这个类型，我们传入的是 Ha 类型的h，该函数正常运行并输出了结果，说明 Ha 类型已经成功实现了 Dog 。

#### 嵌入结构

当我们需要使用复杂结构关系的时候，我们就会需要用到嵌入接口。接下来，我们将上述例子修改一下，如下所示

```go
type Dog interface {   
    Animal 
} 
type Animal interface {   
    Category() 
} 
type Ha struct {   
    Name string 
} 
func (h Ha) Category() {   
    fmt.Println("狗子") 
} 
func main() {   
    h := Ha{"二哈"}   
    h.Category()   
    test(h) 
} 
func test(a Dog) {   
    fmt.Println("成功调用啦") 
} // 输出结果为：狗子 成功调用啦  
```

可以看到，程序同样正常运行，这也就证明了我们成功是实现了嵌入接口。

#### 类型断言

一个 interface 被多种类型实现时，有时候我们需要区分 interface 的变量究竟存储哪种类型的值。

go 可以使用 comma, ok 的形式做区分 value, ok := em.(T)：em 是 interface 类型的变量，T代表要断言的类型，value 是 interface 变量存储的值，ok 是 bool 类型表示是否为该断言的类型 T。。

```go
type Dog interface { 
    Animal 
} 
type Animal interface { 
    Category() 
} 
type Ha struct { 
    Name string 
} 
func (h Ha) Category() { 
    fmt.Println("狗子") 
} 
func main() { 
    h := Ha{"二哈"} 
    h.Category() test(h) 
} 
func test(a Dog) { 
    if v, ok := a.(Ha); ok { 
        fmt.Println(v.Name) 
    } 
} // 输出结果为：狗子 二哈  
```

如果需要区分多种类型，可以使用 switch 断言，更简单直接，这种断言方式只能在 switch 语句中使用。

```go
type Dog interface { 
    Animal 
} 
type Animal interface { 
    Category() 
} 
type Ha struct { 
    Name string 
} 
func (h Ha) Category() { 
    fmt.Println("狗子") 
} 
func main() { 
    h := Ha{"二哈"} 
    h.Category() 
    test(h) 
} 
func test(a Dog) { 
    switch v := a.(type) { 
        case Ha: fmt.Println(v.Name) 
    default: fmt.Println("暂未匹配到该类型") 
        } 
} // 输出结果为：狗子 二哈  
```

#### 空接口

空接口 interface{} 没有任何方法签名，也就意味着任何类型都实现了空接口。其作用类似于面向对象语言中的根对象 Object 。

```go
func Print(v interface{}) {   
    fmt.Println(v) 
} 
func main() {   
    Print(1)   
    Print("Hello, World") 
} // 输出结果为：1  Hello, World  
```

既然空的 interface 可以接受任何类型的参数，那么一个 interface{}类型的 slice 是不是就可以接受任何类型的 slice ?

```go
func printAll(vals []interface{}) { 
    //1 
    for _, val := range vals { 
        fmt.Println(val) 
    } 
} 
func main(){ 
    names := []string{"stanley", "david", "oscar"} 
    printAll(names) 
}  
```

执行之后竟然会报 cannot use names (type []string) as type []interface {} in argument to printAll 错误，why？

这个错误说明 go 没有帮助我们自动把 slice 转换成 interface{} 类型的 slice，所以出错了。go 不会对 类型是interface{} 的 slice 进行转换 。

但是我们可以手动进行转换来达到我们的目的。

```go
var dataSlice []int = foo() 
var interfaceSlice []
interface{} = make([]interface{}, len(dataSlice)) 
for i, d := range dataSlice { 
    interfaceSlice[i] = d 
}  
```

#### 有个坑需要注意

如果是按 `pointer` 调用，go 会自动进行转换，因为有了指针总是能得到指针指向的值是什么，如果是 `value` 调用，go 将无从得知 `value` 的原始值是什么，因为 `value` 是份拷贝。go 会把指针进行隐式转换得到 `value`，但反过来则不行。

### 数组&切片&字典(增,删,改)

#### make 切片

对于`make` `slice`而言，有两个概念需要搞清楚：长度跟容量。

容量表示底层数组的大小，长度是你可以使用的大小。
容量的用处在哪？在与当你用 `append`扩展长度时，如果新的长度小于容量，不会更换底层数组，否则，go 会新申请一个底层数组，拷贝这边的值过去，把原来的数组丢掉。也就是说，容量的用途是：在数据拷贝和内存申请的消耗与内存占用之间提供一个权衡。
而长度，则是为了帮助你限制切片可用成员的数量，提供边界查询的。所以用 `make` 申请好空间后，需要注意不要越界【越 `len` 】

```go
package main

import "fmt"

func main() {
    slice1 := make([]int, 5)
    slice2 := make([]int, 5, 10)
    fmt.Println(slice1, len(slice1), cap(slice1))
    fmt.Println(slice2, len(slice1), cap(slice2))

    map1 := make(map[string]int)
    map2 := make(map[string]int, 5)

    fmt.Println(map1, len(map1))
    fmt.Println(map2, len(map2))
}

```

输出:

```
[0 0 0 0 0] 5 5
[0 0 0 0 0] 5 10
map[] 0
map[] 0
```





#### new (新建)

一样也找不到的具体的原型函数，只在`src/builtin/builtin.go`中有`func new(Type) *Type`。根据前面查 `make` 的具体原型的经验，我猜这个这是` new` 的函数格式，具体调用应该是类似于 `func newint() *int` 这种函数，总之应该还是由编译器链接完成的。 
但是我们用 `new` 分配的空间，函数返回的是一个指向新分配的零值的指针。函数格式如下：

```go
func new(Type) *Type
```

例子:

```go
	new1 := new([2]int)
    fmt.Println(new1)
    new1[0] = 1
    new1[1] = 2
    fmt.Println(new1)
```

输出：

```
&[0 0]
&[1 2]
```



#### len(长度)&cap(容量)



##### 函数 len 格式：

`func len(v Type) int`

- 如果 `v `是数组：返回的是数组的元素个数
- 如果` v` 是个指向数组的指针：返回的是`*v `的元素个数
- 如果` v` 是 `slice` 或者 `map` ：返回 `v` 的元素个数
- 如果` v `是 `channel：the number of elements queued (unread) in the channel buffer`；因还不清楚 channel 是啥，就网上直接搬过来

##### 函数 cap 的格式:

`func cap(v Type) int`

- 数组：返回的是数组的元素个数，同` len(v)`
- 指向数组的指针：返回的是 *v 的元素个数，同 `len(v)`
- `slice`：返回的是 `slice` 最大容量，`>=len(v)`
- `channel： the channel buffer capacity, in units of element`

#### append (追加切片元素)

`append` 将元素追加到切片 `slice` 的末尾，若它有足够的容量，其目标就会重新切片以容纳新的元素。否则，就会分配一个新的基本数组。`append` 返回更新后的切片，因此必须存储追加后的结果。

```go
slice = append(slice, elem1, elem2)
slice = append(slice, anotherSlice…)
```

第一种用法中，第一个参数为` slice` ,后面可以添加多个参数。 
如果是将两个` slice `拼接在一起，则需要使用第二种用法，在第二个` slice `的名称后面加三个点，而且这时候 `append `只支持两个参数，不支持任意个数的参数。 
个人感觉方法2用得多些 

例子1【采用方法1】：

```go
slice1 := make([]int, 5, 10)
slice2 = append(slice2, 5, 6, 7, 8, 9)
fmt.Println(slice2, len(slice2), cap(slice2))
slice2 = append(slice2, 10) 
fmt.Println(slice2, len(slice2), cap(slice2)) 
```

输出：

```
[0 1 2 3 4 5 6 7 8 9] 10 10
[0 1 2 3 4 5 6 7 8 9 10] 11 20//注意容量变为了20
```



例子2【采用方法1】：

```go
	d := []string{"Welcome", "for", "Hangzhou, ", "Have", "a", "good", "journey!"}
    insertSlice := []string{"It", "is", "a", "big", "city, "}
    insertSliceIndex := 3
    d = append(d[:insertSliceIndex], append(insertSlice, d[insertSliceIndex:]...)...)
    fmt.Println(d)

```

输出：

```
[Welcome for Hangzhou,  It is a big city,  Have a good journey!]
```



#### copy (复制)

`copy` 可以将后面的 第2个切片的元素赋值`copy` 到第一个切片中

例子:

```go
package main;
 
import "fmt"
 
func  test () {
   s1 := []int{1,2,3,4,5}
   s2 := make([]int, 10)
   fmt.Println(s2);
   copy(s2, s1)
   fmt.Println(s2);
}
func  main () {
   test()
}
```

输出:

```
[0 0 0 0 0 0 0 0 0 0]
[1 2 3 4 5 0 0 0 0 0]
```

`copy` 不会新建新的内存空间，由它原来的切片长度决定



#### delete(删除)

内建函数 delete 按照指定的键将元素从映射中删除。若 m 为 nil 或无此元素，delete 不进行操作。 
函数结构：

```go
func delete(m map[Type]Type1, key Type)
```



例子:

```go
    map1 := make(map[string]int)
    map1["one"] = 1
    map1["two"] = 2
    map1["three"] = 3
    map1["four"] = 4

    fmt.Println(map1, len(map1))
    delete(map1, "two")
    fmt.Println(map1, len(map1))

```

输出:

```go
map[three:3 four:4 one:1 two:2] 4
map[four:4 one:1 three:3] 3
//map 是无序的,每次打印出来的 map 都会不一样,所以它不能通过 index 获取,而必须通过 key 获取
```



## 流程控制

### if else(如果,否则)

golang 中 if 要注意的点是

- 可省略条件表达式的括号。
- 支持初始化语句，可定义代码块局部变量。
- 代码块左大括号必须在条件表达式尾部。
- 不支持三元操作符 "a > b ? a : b"

```go
if num := 9; num < 0 {   
    fmt.Println(num, "is negative") 
} else 
if num < 10 {   
    fmt.Println(num, "has 1 digit") 
} else {   
    fmt.Println(num, "has multiple digits") 
}  
```

### switch case default(条件,结果)

```go
switch sExpr {   
    case expr1:       
    some instructions   
    case expr2:       
    some other instructions   
    case expr3:       
    some other instructions   
    default:       
    other code 
}  
```

sExpr和expr1、expr2、expr3的类型必须一致。Go的switch非常灵活，表达式不必是常量或整数，执行的过程从上至下，直到找到匹配项；而如果switch没有表达式，它会匹配true。 Go里面switch默认相当于每个case最后带有break，匹配成功后不会自动向下执行其他case，而是跳出整个switch

#### fallthrough(向下执行)

在switch中，使用fallthrough可以强制执行后面的case代码。

```go
switch sExpr {   
    case false:     
    fmt.Println("The integer was <= 4")     
    fallthrough   
    case true:     
    fmt.Println("The integer was <= 5")     
    fallthrough   
    case false:     
    fmt.Println("The integer was <= 6")     
    fallthrough   
    case true:     
    fmt.Println("The integer was <= 7")   
    case false:     
    fmt.Println("The integer was <= 8")     
    fallthrough   
    default:     
    fmt.Println("default case")   
}  
```

输出

```go
The integer was <= 5 
The integer was <= 6 
The integer was <= 7  
```



### for(循环)

`for` 是 `Go` 中唯一的循环结构。这里有 `for `循环的三个基本使用方式。

最常用的方式，带单个循环条件

```go
for i := 1 for i <= 3 {   
    fmt.Println(i)   
    i = i + 1 
}  
```

经典的初始化/条件/后续形式 for 循环

```go
for j := 7; j <= 9; j++ {   
    fmt.Println(j) 
}  
```

`for 值; 条件; 值++`

`for j := 7; j <= 9; j++{内容}`

不带条件的 for 循环将一直执行，直到在循环体内使用了 break 或者 return 来跳出循环。

```go
for {   
    fmt.Println("loop")   
    break 
}  
```

#### return(返回变量)

#### break(跳出)

break是跳出本次循环，

#### continue(跳过执行)

continue是跳过该次循环，继续下次循环。

#### goto(转到)

跳转到指定循环位置

#### range(遍历)

Go 语言可以使用 for range 遍历数组、切片、字符串、map 及通道（channel）。通过 for range 遍历的返回值有一定的规律：

- 数组、切片、字符串返回索引和值。
- map 返回键和值。
- 通道（channel）只返回通道内的值。

##### 遍历数组、切片——获得索引和元素

```go
for key, value := range []int{1, 2, 3, 4} {
    fmt.Printf("key:%d  value:%d\n", key, value)
}
```

Go 语言和其他语言类似，可以通过 for range 的组合，对字符串进行遍历，遍历时，key 和 value 分别代表字符串的索引（base0）和字符串中的每一个字符。

下面这段代码展示了如何遍历字符串：

```go
var str = "hello 你好"
for key, value := range str {
    fmt.Printf("key:%d value:0x%x\n", key, value)
}
```



代码输出如下:

```go
key:0 value:0x68
key:1 value:0x65
key:2 value:0x6c
key:3 value:0x6c
key:4 value:0x6f
key:5 value:0x20
key:6 value:0x4f60
key:9 value:0x597d
```

代码中的 v 变量，实际类型是 rune，实际上就是 int32，以十六进制打印出来就是字符的编码。



##### 遍历map——获得map的键和值

对于 map 类型来说，for range 遍历时，key 和 value 分别代表 map 的索引键 key 和索引对应的值，一般被称为 map 的键值对，因为它们总是一对一对的出现。下面的代码演示了如何遍历 map。

```go
m := map[string]int{
    "hello": 100,
    "world": 200,
}
for key, value := range m {
    fmt.Println(key, value)
}
```

**注意** 对 map 遍历时，遍历输出的键值是无序的，如果需要有序的键值对输出，需要对结果进行排序。

##### 遍历通道（channel）——接收通道数据

for range 可以遍历通道（channel），但是通道在遍历时，只输出一个值，即管道内的类型对应的数据

```go
c := make(chan int)
go func() {
    c <- 1
    c <- 2
    c <- 3
    close(c)
}()
for v := range c {
    fmt.Println(v)
}
```



##### 在遍历中选择希望获得的变量

在使用 for range 循环遍历某个对象时，一般不会同时需要 key 或者 value，这个时候可以采用一些技巧，让代码变得更简单。下面将前面的例子修改一下，参考下面的代码示例：

```go
m := map[string]int{
    "hello": 100,
    "world": 200,
}
for _, value := range m {
    fmt.Println(value)
}
```



在例子中将 key 变成了下画线，那么这里的下画线就是匿名变量。什么是匿名变量？

- 可以理解为一种占位符。
- 本身这种变量不会进行空间分配，也不会占用一个变量的名字。
- 在 for range 可以对 key 使用匿名变量，也可以对 value 使用匿名变量。

### 错误处理

在其他语言里，宕机往往以异常的形式存在，底层抛出异常，上层逻辑通过 `try`/`catch` 机制捕获异常，没有被捕获的严重异常会导致宕机

`go`语言追求简洁，优雅，`Go`语言不支持传统的 `try`…`catch`…`finally `这种异常

`Go`语言的设计者们认为，将异常与控制结构混在一起会很容易使得代码变得混乱

`Go`语言，可以使用多值返回来返回错误。不要用异常代替错误，更不要用来控制流程。在极个别的情况下，才使用`Go`中引入的`Exception`处理：`defer`, `panic`,` recover`

#### panic&recover

试验代码:

```go

package main
import (
	"fmt"
	"runtime/debug"
)
 
// 异常处理函数1
func panicDeal1() {
	fmt.Println("panicDeal1,begin")
	if err := recover(); err != nil {
		fmt.Println("err1:", err)          // 打印出异常
        //(由于panicDeal2()中的recover函数已经捕获了异常,所以这里捕获不到异常,不会得到执行)
		fmt.Println(string(debug.Stack())) // 打印出堆栈信息
	}
	fmt.Println("panicDeal1,end")
}
 
// 异常处理函数2
func panicDeal2() {
	fmt.Println("panicDeal2,begin")
	if err := recover(); err != nil {
		fmt.Println("err2", err)           // 打印出异常
		fmt.Println(string(debug.Stack())) // 打印出堆栈
	}
	fmt.Println("panicDeal2,end")
}
 
func test() {
	fmt.Println("1111")
 
	// 必须先声明defer,否则不能捕获panic异常
	defer panicDeal1()
 
	// 触发panic时,逆序执行,也就是先执行 panicDeal2(),再执行 panicDeal1()
	defer panicDeal2()
 
	fmt.Println("2222")
 
	// 空指针赋值,产生崩溃
	var p *int
	*p = 1
 
	// 这里的代码得不到执行
	fmt.Println("3333")
}
 
func main() {
	test()
}
```

输出:

```
1111
2222
panicDeal2,begin
err2 runtime error: invalid memory address or nil pointer dereference
goroutine 1 [running]:
runtime/debug.Stack(0x4ede80, 0xc000006018, 0xc00008bd40)
        c:/go/src/runtime/debug/stack.go:24 +0xa4
main.panicDeal2()
        D:/gopath/src/xuexi/04.cuowu/cuowutest.go:24 +0x18b
panic(0x4b68c0, 0x570a80)
        c:/go/src/runtime/panic.go:679 +0x1c0
main.test()
        D:/gopath/src/xuexi/04.cuowu/cuowutest.go:43 +0x157
main.main()
        D:/gopath/src/xuexi/04.cuowu/cuowutest.go:50 +0x27

panicDeal2,end
panicDeal1,begin
panicDeal1,end
```



##### panic

函数中遇到`panic`语句，会立即终止当前函数的执行，在`panic`所在函数内如果存在要执行的`defer`函数列表，按照`defer`的逆序执行

##### recover

`recover`函数的返回值报告协程是否正在遭遇`panic`

有异常时，`recover()`只能调用一次，后面再次调用则捕获不到任何异常

通常办法：`go`中可以抛出一个`panic`的异常，然后在`defer`中通过`recover`捕获这个异常，然后正常处理，从而恢复正常代码的执行

#### defer(延时执行)

`defer`确实是在`return`之前调用的。但表现形式上却可能不像。本质原因是`return` xxx语句并不是一条原子指令，`defer`被插入到了赋值 与 ret之间，因此可能有机会改变最终的返回值。

`goroutine`的控制结构中，有一张表记录`defer`，调用`runtime.deferproc`时会将需要`defer`的表达式记录在表中，而在调用`runtime.deferreturn`的时候，则会依次从`defer`表中出栈并执行。

```go
defer function_name() 
```

##### **简单讲defer**

在`defer`所在函数执行完所有的代码之后，会自动执行defer的这个函数。
使用场景
主要用于资源需要释放的场景。比如打开一个文件，最后总是要关闭的。而在打开和关闭之间，会有诸多的处理，可能会有诸多的`if-else`、根据不同的情况需要提前返回。在传统语言中，`return`之前都需要一一调用`close()`。

而`Go`的`defer`就将事情变得简单了，`open()`之后，直接就用`defer`“注册”一个`close()`。

### time

golang默认初始时间`2006-01-02 15:04:05.000`

回收`runtime.Caller()`

#### 时间类型

* `time.Time`  : `time.Now()`
* 时间戳：
  * `time.Now().Unix()` ：1970.1.1到现在的秒数
  * `time.Now().UnixNano()` :1970.1.1到现在的纳秒数

#### 时间间隔类型

* `time.Duration`:  时间间隔类型，
  * `time.Second`

```go
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)
```

### 时间操作

时间对象+/-一个时间间隔对象

```go
// now + 24小时
fmt.Println(now.Add(24 * time.Hour))
// Sub 两个时间相减
nextYear, err := time.Parse("2006-01-02 15:04:05", "2019-08-04 12:25:00")
if err != nil {
	fmt.Printf("parse time failed, err:%v\n", err)
	return
}
now = now.UTC()
d := nextYear.Sub(now)
```

after(后来)/before(过去)

#### 时间格式化

2006-01-02 15:04:05.000

#### 定时器

```go
timer := time.Tick(time.Second)
for t := range timer {
	fmt.Println(t) // 1秒钟执行一次
}
```

#### 解析字符串格式的时间(时区)

```go
// 按照指定格式取解析一个字符串格式的时间
time.Parse("2006-01-02 15:04:05", "2019-08-04 14:41:50")
// 按照东八区的时区和格式取解析一个字符串格式的时间
// 根据字符串加载时区
loc, err := time.LoadLocation("Asia/Shanghai")
if err != nil {
	fmt.Printf("load loc failed, err:%v\n", err)
	return
}
// 按照指定时区解析时间
timeObj, err := time.ParseInLocation("2006-01-02 15:04:05", "2019-08-04 14:41:50", loc)
if err != nil {
	fmt.Printf("parse time failed, err:%v\n", err)
	return
}
fmt.Println(timeObj)
```



### math/rand随机数生成

```go
func f() {
	rand.Seed(time.Now().UnixNano()) // 保证每次执行的时候都有点不一样
	for i := 0; i < 5; i++ {
		r1 := rand.Int()    // int64
		r2 := rand.Intn(10) // 0<= x < 10
		fmt.Println(r1, r2)
	}
}
```

### 



### 反射

接口类型的变量底层是分为两部分:动态类型和动态值.

反射的应用:`json`等数据解析\ORM等工具...

#### 反射的两个方法:

* `reflect.TypeOf()`
* `reflect.ValueOf()`



# 并发





## goroutine

我们在使用Go语言进行开发时，一般会使用goroutine来处理并发任务。那么大家有没有考虑过goroutine的实现机制是什么样的？很多同学会把goroutine与线程等同起来，但是实际上并不是这样的。在这边文章中，我们将介绍以下内容：

### 什么是goroutine？

Goroutine与线程的区别Goroutine是如何调度的

![img](C:\Users\lok\OneDrive\golang 笔记 .assets\u=904179418,1211108315&fm=173&app=49&f=JPEG.jfif)

Goroutine是建立在线程之上的轻量级的抽象。它允许我们以非常低的代价在同一个地址空间中并行地执行多个函数或者方法。相比于线程，它的创建和销毁的代价要小很多，并且它的调度是独立于线程的。在golang中创建一个goroutine非常简单，使用“go”关键字即可：

```go
package mainimport ( "fmt" "time")func learning() { 
    fmt.Println("My first goroutine")
}
func main() { 
    go learning() /* we are using time sleep so that the main program does not terminate before the execution of goroutine.*/ 
    time.Sleep(1 * time.Second) 
    fmt.Println("main function")
}
```





这段代码的输出是这样的：

```go
My first goroutinemain function
```

如果把Sleep去掉的话，输出就会变成：

```go
main function
```

这是因为，和线程一样，golang的主函数（其实也跑在一个goroutine中）并不会等待其它goroutine结束。如果主goroutine结束了，所有其它goroutine都将结束。

###  **Goroutine与线程的区别**

许多人认为goroutine比线程运行得更快，这是一个误解。Goroutine并不会更快，它只是增加了更多的并发性。当一个goroutine被阻塞（比如等待IO），golang的scheduler会调度其它可以执行的goroutine运行。与线程相比，它有以下几个优点：

### **内存消耗更少：**

Goroutine所需要的内存通常只有2kb，而线程则需要1Mb（500倍）。

### **创建与销毁的开销更小**

由于线程创建时需要向操作系统申请资源，并且在销毁时将资源归还，因此它的创建和销毁的开销比较大。相比之下，goroutine的创建和销毁是由go语言在运行时自己管理的，因此开销更低。

![img](C:\Users\lok\OneDrive\golang 笔记 .assets\u=1034150745,2083205045&fm=173&app=49&f=JPEG.jfif)



###  切换开销更小

这是goroutine于线程的主要区别，也是golang能够实现高并发的主要原因。线程的调度方式是抢占式的，如果一个线程的执行时间超过了分配给它的时间片，就会被其它可执行的线程抢占。在线程切换的过程中需要保存/恢复所有的寄存器信息，比如16个通用寄存器，PC（Program Counter），SP（Stack Pointer），段寄存器等等。

而goroutine的调度是协同式的，它不会直接地与操作系统内核打交道。当goroutine进行切换的时候，之后很少量的寄存器需要保存和恢复（PC和SP）。因此gouroutine的切换效率更高。

### **Goroutine的调度**

真如前面提到的，goroutine的调度方式是协同式的。在协同式调度中，没有时间片的概念。为了并行执行goroutine，调度器会在以下几个时间点对其进行切换：

Channel接受或者发送会造成阻塞的消息当一个新的goroutine被创建时可以造成阻塞的系统调用，如文件和网络操作垃圾回收下面让我们来看一下调度器具体是如何工作的。Golang调度器中有三个概念

Processor（P）OSThread（M）Goroutines（G）在一个Go程序中，可用的线程数是通过GOMAXPROCS来设置的，默认值是可用的CPU核数。我们可以用runtime包动态改变这个值。OSThread调度在processor上，goroutines调度在OSThreads上，如下图所示

![img](C:\Users\lok\OneDrive\golang 笔记 .assets\u=3681409504,995593685&fm=173&app=49&f=JPEG.jfif)

Golang的调度器可以利用多processor资源，在任意时刻，M个goroutine需要被调度到N个OS threads上，同时这些threads运行在至多GOMAXPROCS个processor上（N <= GOMAXPROCS）。Go scheduler将可运行的goroutines分配到多个运行在一个或多个processor上的OS threads上。

每个processor有一个本地goroutine队列。同时有一个全局的goroutine队列。每个OSThread都会被分配给一个processor。最多只能有GOMAXPROCS个processor，每个processor同时只能执行一个OSThread。Scheculer可以根据需要创建OSThread。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=936486653,538614756&fm=173&app=49&f=JPEG?w=640&h=307&s=CCA63472131A44625A5455CA0000E0B2)

在每一轮调度中，scheduler找到一个可以运行的goroutine并执行直到其被阻塞。

Search in the local queueif not found Try to steal from other Ps' local queue //see Fig 3 if not found Search in the global queue Also periodically it searches in the global queue (every ~ 1/70)

由此可见，操作系统的一个线程下可以并发执行上千个goroutine，每个goroutine所占用的资源和切换开销都很小，因此，goroutine是golang适合高并发场景的重要原因。



###  语法:go

轻松开启高并发，一直都是golang语言引以为豪的功能点。golang通过goroutine实现高并发，而开启goroutine的钥匙正是go关键字。开启并发执行的语法格式是：

```
go funcName()  


```

### 计数&等待(sync.WaitGroup)

```go
sync.WaitGroup

var wg sync.WaitGroup //等待组

func hello(i int) {
	defer wg.Done() // goroutine结束就登记-1
	fmt.Println("Hello Goroutine!", i)
}
func main() {

	for i := 0; i < 10; i++ {
		wg.Add(1) // 启动一个goroutine就登记+1
		go hello(i)
	}
	wg.Wait() // 等待所有登记的goroutine都结束
}
```

### sync.WaitGroup

在代码中生硬的使用`time.Sleep`肯定是不合适的，Go语言中可以使用`sync.WaitGroup`来实现并发任务的同步。`sync.WaitGroup`有以下几个方法：

|             方法名              |        功能         |
| :-----------------------------: | :-----------------: |
| (wg * WaitGroup) Add(delta int) |    计数器+delta     |
|     (wg *WaitGroup) Done()      |      计数器-1       |
|     (wg *WaitGroup) Wait()      | 阻塞直到计数器变为0 |

`sync.WaitGroup`内部维护着一个计数器，计数器的值可以增加和减少。例如当我们启动了N 个并发任务时，就将计数器值增加N。每个任务完成时通过调用Done()方法将计数器减1。通过调用Wait()来等待并发任务执行完，当计数器值为0时，表示所有并发任务已经完成。

我们利用`sync.WaitGroup`将上面的代码优化一下：

```go
var wg sync.WaitGroup

func hello() {
	defer wg.Done()
	fmt.Println("Hello Goroutine!")
}
func main() {
	wg.Add(1)
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
	wg.Wait()
}
```

需要注意`sync.WaitGroup`是一个结构体，传递的时候要传递指针。



### once.Do(唯一运行)

```go
once.Do(func(){close(ch)})//确保这个操作只运行一次
```

### sync.Once

说在前面的话：这是一个进阶知识点。

在编程的很多场景下我们需要确保某些操作在高并发的场景下只执行一次，例如只加载一次配置文件、只关闭一次通道等。

Go语言中的`sync`包中提供了一个针对只执行一次场景的解决方案–`sync.Once`。

`sync.Once`只有一个`Do`方法，其签名如下：

```go
func (o *Once) Do(f func()) {}
```

*备注：如果要执行的函数`f`需要传递参数就需要搭配闭包来使用。*

### worker pool（goroutine池）

在工作中我们通常会使用可以指定启动的goroutine数量–`worker pool`模式，控制`goroutine`的数量，防止`goroutine`泄漏和暴涨。

一个简易的`work pool`示例代码如下：

```go
func worker(id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Printf("worker:%d start job:%d\n", id, j)
		time.Sleep(time.Second)
		fmt.Printf("worker:%d end job:%d\n", id, j)
		results <- j * 2
	}
}


func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)
	// 开启3个goroutine
	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}
	// 5个任务
	for j := 1; j <= 5; j++ {
		jobs <- j
	}
	close(jobs)
	// 输出结果
	for a := 1; a <= 5; a++ {
		<-results
	}
}
```

### `goroutine`什么时候结束?

goroutine 对应的函数结束了，goroutine结束了。

`main`函数执行完了，由`main`函数创建的那些`goroutine`都结束了。



## chan

channel[通道]是golang的一种重要特性，正是因为channel的存在才使得golang不同于其它语言。channel使得并发编程变得简单容易有趣。

一个channel可以理解为一个先进先出的消息队列。如下图所示:





![d](C:\Users\lok\OneDrive\golang 笔记 .assets\5b45c15b0001a23005090120.jpg)





### 创建channel有以下几种 方式，

```go
var ch chan string; 
// nil 
channel ch := make(chan string); 
// zero 
channel ch := make(chan string, 10); 
// buffered channel  
```

channel里面的value buffer的容量也就是channel的容量。channel的容量为零表示这是一个阻塞型通道，非零表示缓冲型通道[非阻塞型通道]。

但是，这里有个坑，当channel的容量为0时，for循环一次开10个goroutine打印输出，此时理论上应该是顺序输出的，但是确实无序输出的，这是因为现在的 Go 默认就是启用的多核，不像以前版本还需要手动设置使用多核。

### 通道的操作

 操作符号:`<-` 

1. 发送 : `ch1 <- 1`
2. 接收: ` <- ch1`
3. 关闭:`close()`

### 赋值chennel

```go
ch <- 10  //把10放到chennel
x := <- ch // 从ch中接收值并赋值给变量x
<-ch       // 从ch中接收值，忽略结果
close(ch)  //通过调用内置的close函数来关闭通道
```

### 无缓冲的通道

无缓冲的通道又称为阻塞的通道。我们来看一下下面的代码：

```go
func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}
```

上面这段代码能够通过编译，但是执行的时候会出现以下错误：

```bash
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.main()
        .../src/github.com/Q1mi/studygo/day06/channel02/main.go:8 +0x54
```

为什么会出现`deadlock`错误呢？

因为我们使用`ch := make(chan int)`创建的是无缓冲的通道，无缓冲的通道只有在有人接收值的时候才能发送值。就像你住的小区没有快递柜和代收点，快递员给你打电话必须要把这个物品送到你的手中，简单来说就是无缓冲的通道必须有接收才能发送。

上面的代码会阻塞在`ch <- 10`这一行代码形成死锁，那如何解决这个问题呢？

一种方法是启用一个`goroutine`去接收值，例如：

```go
func recv(c chan int) {
	ret := <-c
	fmt.Println("接收成功", ret)
}
func main() {
	ch := make(chan int)
	go recv(ch) // 启用goroutine从通道接收值
	ch <- 10
	fmt.Println("发送成功")
}
```

无缓冲通道上的发送操作会阻塞，直到另一个`goroutine`在该通道上执行接收操作，这时值才能发送成功，两个`goroutine`将继续执行。相反，如果接收操作先执行，接收方的goroutine将阻塞，直到另一个`goroutine`在该通道上发送一个值。

使用无缓冲通道进行通信将导致发送和接收的`goroutine`同步化。因此，无缓冲通道也被称为`同步通道`。

### 有缓冲的通道

解决上面问题的方法还有一种就是使用有缓冲区的通道。我们可以在使用make函数初始化通道的时候为其指定通道的容量，例如：

```go
func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}
```

只要通道的容量大于零，那么该通道就是有缓冲的通道，通道的容量表示通道中能存放元素的数量。就像你小区的快递柜只有那么个多格子，格子满了就装不下了，就阻塞了，等到别人取走一个快递员就能往里面放一个。

我们可以使用内置的`len`函数获取通道内元素的数量，使用`cap`函数获取通道的容量，虽然我们很少会这么做。

### for range从通道循环取值

当向通道中发送完数据时，我们可以通过`close`函数来关闭通道。

当通道被关闭时，再往该通道发送值会引发panic，从该通道里接收的值一直都是类型零值。那如何判断一个通道是否被关闭了呢？

我们来看下面这个例子：

```go
// channel 练习
func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)
	// 开启goroutine将0~100的数发送到ch1中
	go func() {
		for i := 0; i < 100; i++ {
			ch1 <- i
		}
		close(ch1)
	}()
	// 开启goroutine从ch1中接收值，并将该值的平方发送到ch2中
	go func() {
		for {
			i, ok := <-ch1 // 通道关闭后再取值ok=false
			if !ok {
				break
			}
			ch2 <- i * i
		}
		close(ch2)
	}()
	// 在主goroutine中从ch2中接收值打印
	for i := range ch2 { // 通道关闭后会退出for range循环
		fmt.Println(i)
	}
}
```

从上面的例子中我们看到有两种方式在接收值的时候判断该通道是否被关闭，不过我们通常使用的是`for range`的方式。使用`for range`遍历通道，当通道被关闭的时候就会退出`for range`。

### 单向通道

有的时候我们会将通道作为参数在多个任务函数间传递，很多时候我们在不同的任务函数中使用通道都会对其进行限制，比如限制通道在函数中只能发送或只能接收。

Go语言中提供了**单向通道**来处理这种情况。例如，我们把上面的例子改造如下：

```go
func counter(out chan<- int) {
	for i := 0; i < 100; i++ {
		out <- i
	}
	close(out)
}

func squarer(out chan<- int, in <-chan int) {
	for i := range in {
		out <- i * i
	}
	close(out)
}
func printer(in <-chan int) {
	for i := range in {
		fmt.Println(i)
	}
}

func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)
	go counter(ch1)
	go squarer(ch2, ch1)
	printer(ch2)
}
```

其中，

- `chan<- int`是一个只写单向通道（只能对其写入int类型值），可以对其执行发送操作但是不能执行接收操作；
- `<-chan int`是一个只读单向通道（只能从其读取int类型值），可以对其执行接收操作但是不能执行发送操作。

在函数传参及任何赋值操作中可以将双向通道转换为单向通道，但反过来是不可以的。

### 通道总结

`channel`常见的异常总结，如下图：![channel异常总结](https://www.liwenzhou.com/images/Go/concurrence/channel01.png)

关闭已经关闭的`channel`也会引发`panic`。

已经关闭的`channel`也取值

## select

Go的select关键字可以让你同时等待多个通道操作，将协程（goroutine），通道（channel）和select结合起来构成了Go的一个强大特性。

```go
package main 
import "time" 
import "fmt" 
func main() {   
    // 本例中，我们从两个通道中选择   
    c1 := make(chan string)   
    c2 := make(chan string)   
    // 为了模拟并行协程的阻塞操作，我们让每个通道在一段时间后再写入一个值   
    go func() {     time.Sleep(time.Second * 1)     
               c1 <- "one"   
              }()   
    go func() {     time.Sleep(time.Second * 2)     
               c2 <- "two"   }()   
    // 我们使用select来等待这两个通道的值，然后输出   
    for i := 0; i < 2; i++ {     
        select {     
            case msg1 := <-c1:       
            fmt.Println("received", msg1)     
            case msg2 := <-c2:       
            fmt.Println("received", msg2)     
        }   
    } 
}  
```

输出结果

```go
received one received two  
```

如我们所期望的，程序输出了正确的值。对于select语句而言，它不断地检测通道是否有值过来，一旦发现有值过来，立刻获取输出。



## select多路复用

在某些场景下我们需要同时从多个通道接收数据。通道在接收数据时，如果没有数据可以接收将会发生阻塞。你也许会写出如下代码使用遍历的方式来实现：

```go
for{
    // 尝试从ch1接收值
    data, ok := <-ch1
    // 尝试从ch2接收值
    data, ok := <-ch2
    …
}
```

这种方式虽然可以实现从多个通道接收值的需求，但是运行性能会差很多。为了应对这种场景，Go内置了`select`关键字，可以同时响应多个通道的操作。

`select`的使用类似于switch语句，它有一系列case分支和一个默认的分支。每个case会对应一个通道的通信（接收或发送）过程。`select`会一直等待，直到某个`case`的通信操作完成时，就会执行`case`分支对应的语句。具体格式如下：

```go
select{
    case <-ch1:
        ...
    case data := <-ch2:
        ...
    case ch3<-data:
        ...
    default:
        默认操作
}
```

举个小例子来演示下`select`的使用：

```go
func main() {
	ch := make(chan int, 1)
	for i := 0; i < 10; i++ {
		select {
		case x := <-ch:
			fmt.Println(x)
		case ch <- i:
		}
	}
}
```

使用`select`语句能提高代码的可读性。

- 可处理一个或多个channel的发送/接收操作。
- 如果多个`case`同时满足，`select`会随机选择一个。
- 对于没有`case`的`select{}`会一直等待，可用于阻塞main函数。

## 并发安全&锁

有时候在Go代码中可能会存在多个`goroutine`同时操作一个资源（临界区），这种情况会发生`竞态问题`（数据竞态）。类比现实生活中的例子有十字路口被各个方向的的汽车竞争；还有火车上的卫生间被车厢里的人竞争。

举个例子：

```go
var x int64
var wg sync.WaitGroup

func add() {
	for i := 0; i < 5000; i++ {
		x = x + 1
	}
	wg.Done()
}
func main() {
	wg.Add(2)
	go add()
	go add()
	wg.Wait()
	fmt.Println(x)
}
```

上面的代码中我们开启了两个`goroutine`去累加变量x的值，这两个`goroutine`在访问和修改`x`变量的时候就会存在数据竞争，导致最后的结果与期待的不符。

### 互斥锁

互斥锁是一种常用的控制共享资源访问的方法，它能够保证同时只有一个`goroutine`可以访问共享资源。Go语言中使用`sync`包的`Mutex`类型来实现互斥锁。 使用互斥锁来修复上面代码的问题：

```go
var x int64
var wg sync.WaitGroup
var lock sync.Mutex

func add() {
	for i := 0; i < 5000; i++ {
		lock.Lock() // 加锁
		x = x + 1
		lock.Unlock() // 解锁
	}
	wg.Done()
}
func main() {
	wg.Add(2)
	go add()
	go add()
	wg.Wait()
	fmt.Println(x)
}
```

使用互斥锁能够保证同一时间有且只有一个`goroutine`进入临界区，其他的`goroutine`则在等待锁；当互斥锁释放后，等待的`goroutine`才可以获取锁进入临界区，多个`goroutine`同时等待一个锁时，唤醒的策略是随机的。

### 读写互斥锁

互斥锁是完全互斥的，但是有很多实际的场景下是读多写少的，当我们并发的去读取一个资源不涉及资源修改的时候是没有必要加锁的，这种场景下使用读写锁是更好的一种选择。读写锁在Go语言中使用`sync`包中的`RWMutex`类型。

读写锁分为两种：读锁和写锁。当一个goroutine获取读锁之后，其他的`goroutine`如果是获取读锁会继续获得锁，如果是获取写锁就会等待；当一个`goroutine`获取写锁之后，其他的`goroutine`无论是获取读锁还是写锁都会等待。

读写锁示例：

```go
var (
	x      int64
	wg     sync.WaitGroup
	lock   sync.Mutex
	rwlock sync.RWMutex
)

func write() {
	// lock.Lock()   // 加互斥锁
	rwlock.Lock() // 加写锁
	x = x + 1
	time.Sleep(10 * time.Millisecond) // 假设读操作耗时10毫秒
	rwlock.Unlock()                   // 解写锁
	// lock.Unlock()                     // 解互斥锁
	wg.Done()
}

func read() {
	// lock.Lock()                  // 加互斥锁
	rwlock.RLock()               // 加读锁
	time.Sleep(time.Millisecond) // 假设读操作耗时1毫秒
	rwlock.RUnlock()             // 解读锁
	// lock.Unlock()                // 解互斥锁
	wg.Done()
}

func main() {
	start := time.Now()
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go write()
	}

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go read()
	}

	wg.Wait()
	end := time.Now()
	fmt.Println(end.Sub(start))
}
```

需要注意的是读写锁非常适合读多写少的场景，如果读和写的操作差别不大，读写锁的优势就发挥不出来。