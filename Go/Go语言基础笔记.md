# Go 语言基础笔记

## 基本语法

### 1.package

Go程序由package构成，每个.go文件都需要声明package，且一个文件夹下所有.go文件都应该属于同一个package。一般package名和文件所在文件夹名字一致，不一致时以声明的package名字为准。如：

`package lib // C:\Users\admin\go\src\hello\lib\hello_lib.go`

Go的执行程序由一个 main package 创建，该package拥有一个无参数无返回值的main函数。当需要使用其他package的可导入标识符时，使用import导入。可导入标识符同时满足 条件1：首字母大写，条件2：the identifier is declared in the package block or it is a field name or method name。

```go
package main

import (
    "fmt" // 可以用分号 ; 结尾，也可以不用，Go编译器会自动添加
    . "math" // 导入 math package的全部可导出标识符，如下面用到的 Pi
    t "time" // 设置别名
)
func main(){
    fmt.Println(Pi,t.Now())
}
```

### 2.变量

变量可以通过以下方式声明和赋值：

```go
var v1 int // 声明一个int类型的变量 v1，类型在变量名后面。声明变量后，每个变量会初始化为声明类型的零值，这里 v1 初始化为0。
v1 = 1 // 给 v1 赋值 1
var ( // 声明多个变量
    v2 int = 2 // 声明并赋值
    v3 = 3 // 由 Go 编译器判断变量类型
    v4,v5 int = 4,5 //声明多个相同类型变量，可以只保留最后一个类型声明
)
v6 := 6 // 短变量声明，只能在函数内部使用
```

### 3.常量

iota 是常量生成器，从0开始，每赋值一次，加 1。常量声明结束后，iota 重置为0。

```go
const World="世界"
const (
    c1 int = 1
    c2 = 2
)
const (
    Sunday = iota // 0
    Monday  // 1,省略了后面的赋值，等价于 Monday = iota
    Tuesday
    Wednesday
    Thursday
    Friday
    Partyday
    numberOfDays  // 7
)
const ( // iota 重置为 0
    c3 = iota *2 // 0
    c4 = iota *2 // 2
    c5 = iota *2 // 4
)
```


## 数据类型

### String

字符串有两种表示方式，反引号 " ` "括起来的Raw string 和 双引号括起来的interpreted string。注意：单引号括起来的是一个确定Unicode码点的整数值 ：
> A rune literal represents a rune constant, an integer value identifying a Unicode code point.

```go
const s1 = "\n" // 转义成 换行符
const s2 = `\n` // 字符串 "\n"
const rune1 = '\n' // 整数 10
```

### Array

数组需要声明长度和元素类型

```go
arr1 := [2]int{} // [0 0]
arr2 := [2][2]int{} // [[0 0] [0 0]],等价于  [2]([2]int{})
arr3 := [2]int{1,2} // [1 2]
```

### Slice

```go
arr := [4]int{1,2,3,4} // [1 2 3 4]
var slice1 []int // [],slice1为nil,长度 len(slice1) 为0,容量 cap(slice1) 为0, 切片的零值是 nil。
slice2 := []int{} // [],注意，虽然表示形式一样，但 slice2 和 slice1 不相同，slice2!=nil。 切片只能和 nil 比较，切片和切片不能比较，即 slice1==slice2 这个写法会报错。
slice3 := arr[0:3] // [1 2 3]，长度为 3，容量为 4 （底层数组仍为 [1 2 3 4]）
slice4 := arr[1:2] // [2]，长度为 1，容量为 3 （底层数组为 [2 3 4]）
slice5 := make([]int,5,10) // [0 0 0 0 0], 长度为 5，容量为 10
slice5 = append(slice5,1) // [0 0 0 0 0 1],长度为 6，容量为 10

// [[_ _ _] [_ _ _] [_ _ _]]， 切片的切片，长度为 3，容量为 3
board := [][]string{
    []string{"_", "_", "_"},
    []string{"_", "_", "_"},
    []string{"_", "_", "_"},
}
```

### Struct

```go
type T1 int
type Vertex struct {
    X,Y int // 字段名不能相同
    T1 // 只有类型没有字段名的字段 称为 嵌入字段
}
v := Vertex{1,2,3} // {1 2 3}
x=v.X // 3
```

### map

```go
var m1 =map[string]int{"one":1} // map[one:1]
m2 := make(map[string]int,2) // map[],初始空间为 2 个元素的映射
m2["two"] = 2 // map[two:2]
v,ok := m2["one"] // v,ok 为 0，false
v,ok = m2["two"] // v,ok 为 2，true
```

### interface

```go
package main

import (
    "fmt"
    "math"
)

type Locker interface{
    Lock()
    Abser  // 接口可以嵌入其他接口类型，等价于把 Abser的所有方法都嵌入进来，注意方法名不能重复
}

type Abser interface {
    Abs() float64
}

func main() {
    var a Abser
    f := MyFloat(-math.Sqrt2)
    v := Vertex{3, 4}

    a = f  
    fmt.Println(a.Abs()) // 1.4142135623730951
    a = &v
    fmt.Println(a.Abs()) // 5


    a = v // v 是值类型 Vertex,没实现 abs方法，不能赋值给接口 a ,所以这句会报错。

}

type MyFloat float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

type Vertex struct {
    X, Y float64
}

func (v *Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

### chan

```go
ch := make(chan int, 2) // 元素类型为 int,缓存大小为 2
ch <- 1
value,ok := <- ch // value,ok 为 1，true。信道缓存为空时，发送数据会引发 error。
close(ch) //关闭信道
value,ok = <- ch // value,ok 为 0，false,表示信道已关闭
ch <- 3 // 发送数据给已关闭的信道会引发 panic

c := make(chan int) // 无缓存的信道，则只有当发送者和接收者都做好准备时通信才会成功。
go func(){c<-1}()
fmt.Println(<-c)
```

## 流控制语句

### switch

```go
//表达式选择
x := 1
switch {
case x < 0:
    fmt.Println("小于0")
case x > 0:
    fmt.Println("大于0")
    fallthrough  //强制执行下一子句
default: fmt.Println(x)


// 类型选择,不能使用 fallthrough
var x interface{} = 'a' // 参数类型必须 interface
switch x.(type){
case string:
    fmt.Println("String")
case int:
    fmt.Println("int")
default: fmt.Println(x)

}
```

### select

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
    x, y := 0, 1
    for {
        select {
        case c <- x: //阻塞，直到有其他地方从 c 接收数据，把 x 发送给 c。
            x, y = y, x+y
        case <-quit:
            fmt.Println("quit")
            return
        }
    }
}

func main() {
    c := make(chan int)
    quit := make(chan int)
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Println(<-c) //阻塞，直到可以从 c 接收数据
        }
        quit <- 0
    }()
    fibonacci(c, quit)
}
```
