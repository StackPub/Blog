## Go语言基本语法

### 变量
#### 声明
```go
var 变量名 变量类型

//批量声明
var (
    a int
    b string
    c [] float32
    d func() bool
    e struct{
        x int
        y string
    }
)
```
#### 初始化

1. 初始化的标准格式 ```var 变量名 变量类型 = 表达式```

2. 编译器自动推断类型的初始化格式 ```var 变量名 = 表达式```

3. 短变量初始化模式 ```变量名 := 表达式```

```go
var a int = 10
var b = 10
c := 10
```
短变量```:=```模式只能用于函数体内，多个短变量声明和赋值中，至少有一个新声明的变量出现在左侧，那么即便其他变量可能是重复声明，编译器也不会报错。
```go
var a = 100
a, b := 100, 200
```

```go
var a int = 10
var b int = 20
a, b = b, a
```

#### 匿名变量
```go
func GetData() (int , int) {
    return 10, 20
}
a, _ = GetData()
_, b = GetData()
```

### 数据类型
基本数据类型: 整型，浮点型，复数型，布尔型，字符串，字符。

复合数据类型: 数组，切片，映射，函数，结构体，通道，接口，指针。

#### 打印格式

### 类型转换

## 流程控制

|||
|:-:|:-:|
|if/else||
|switch||
|select|select会随机执行一个case语句，如果没有case可执行将会被阻塞，直到case可运行|
|for循环||
|break||
|continue||
|goto||


switch语句还可以被用于type switch(类型转换)来判断interface变量中实际存储的变量类型。
```go
switch x.(type){
    case type:
        statement(s);
    case type:
        statement(s);
    default:    //可选
        statement(s);
}
```

```go
package main
import "fmt"
func main(){
    var x interface()
    switch i := x.(type) {
        case nil:
            fmt.Printf("x的类型:%T")
        case int:
            fmt.Printf("x的类型是int")
        case float64:
            fmt.Printf("x的类型是float64")
        case func(int) float64:
            fmt.Printf("x的类型是func(int)")
        case bool, string:
            fmt.Printf("x的类型是bool或string")
        default:
            fmt.Printf("未知类型")
    }
}
```

for循环的range格式对string,slice,map,channel等进行迭代循环。array,slice,string返回索引和值；map返回键和值；channel只返回通道内的值。
```go
for key, value := range oldMap {
    newMap[key] = value
}
```
```go
package main
import "fmt"
func main(){
    str := "12345678abc"
    for i, value := range str {
        fmt.Printf("第 %d 位的ascii值=%的， 字符串是%c \n",i, value, value)
    }
}
```

### 函数与指针
```go
func 函数名 (参数列表) (返回参数列表) {
    //函数体
    return value1, value2...
}
```
go语言支持匿名函数，即在需要使用函数的时候再定义函数。匿名函数没有函数名，只有函数体，函数可以作为一种类型被赋值给变量，匿名函数通常以变量的方式被传递。
```go
func(参数列表) (返回参数列表){
    //函数体
}

func main(){
    func(data int){
        fmt.Println("hello", data)
    }(100)
}
```

在Go语言中，函数也是一种类型，通过type来定义一个自定义类型。
```go
package main
import "fmt"

type processFunc func(int) bool

func main(){
    slice := []int{1,2,3,4,5,7}
    fmt.Println("slice = ", slice)
    odd := filter(slice,isOdd)
    fmt.Println("奇数元素: ", odd)
    even := filter(slice,isEven)
    fmt.Println("偶数元素: ", even)
}

func isEven(integer int) bool {
    if integer & 2 == 0 {
        return true
    }
    return false
}

func isOdd(integer int) bool {
    if integer % 2 == 0 {
        return false
    }
    return true
}

func filter(slice []int, f processFunc) []int {
    var result []int
    for _, value := range slice {
        if f(value) {
            result = append(result, value)
        }
    }
    return result
}
```

#### 闭包
闭包是由函数与其他相关的引用环境组合而成的实体。函数+引用环境=闭包。函数是编译器静态的概念，而闭包是运行期动态的概念。
```go
package main
import "fmt"
func main(){
    pos := adder()
    for i := 0; i < 10; i++ {
        fmt.Printf("i = %d \t", i)
        fmt.Println(pos(i))
    }
    fmt.Println("--------------------------------")
    for i := 0; i < 10; i++ {
        fmt.Printf("i = %d \t", i)
        fmt.Println(pos(i))
    }
}

func adder() func(int) int {
    sum := 0
    return func(x int) int {
        fmt.Printf("sum1=%d \t", sum)
        sum += x
        fmt.Printf("sum2=%d \t", sum)
        return sum
    }
}
```

### 指针
Go语言指针不能运算，对指针进行运算会报错。```p++```
```go
var a int = 120
var ip *int
ip = &a

var ptr [3] *string
var **int
```

## 内置容器
```go
var 变量名 [数组长度] 数据类型

var nums = [5]int{1,2,3,4,5}
var nums1 = [...]int{1,2,3,4,5}

len(nums) //获取数组长度
```
### 切片
切片没有自己的任何数据，它是底层数组的一个引用。
```go
var identifier []type
var slice1 []type = make([]type, len)
slice1 := make([]type, len)

s := [] int{1,2,3}
arr := [5]int{1,2,3,4,5}
s := arr{:}

cap(s) //容量
a := int(len(numbers)/2)
numbers = append(numbers[:a], numbers[a+1:]...)
```
### map
```go
var 变量名 map[key类型]value类型

变量名 := make(map[key类型]value类型)
delete(map, "key")
```

## 内置常用包
### string字符串处理

### strconv

### regexp正则表达式

### time

### 随机数

## 面向对象
### 结构体
```go
type 类型名 struct {
    成员属性1 类型1
    成员属性2 类型2
    成员属性3 类型3
    ....
}
```

**语法糖**: 指计算机语言中添加某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用。
```go
type Emp struct {
    name string
    age int8
    sex byte
}
emp1 := new(Emp)
(*emp1).name = "David"
(*emp1).age = 30
(*emp1).sex = 1
//语法糖
(*emp1).name = "David2"
(*emp1).age = 31
(*emp1).sex = 0
```

结构体是值类型，是深拷贝，为新的对象分配来内存，引用类型是浅拷贝，只复制来对象的指针。

#### 匿名结构体和匿名字段
```go
变量名 := struct{
    //成员属性
}(//初始化成员属性)

type user struct {
    string
    byte
    int8
    float64
}//同一个类型只能有一个匿名字段
```

```go
package main
import (
    "math"
    "fmt"
)

func main() {
    //匿名函数
    res := func (a,b float64) float64{
        return math.Pow(a,b)
    }(2, 3)
    //匿名结构体
    addr := struct {
        province, city string
    }{"aaa", "bbb"}
}
```

### 方法
方法有接受者，函数无接受者。方法的接受者不是指针的话，实际获取的是一个拷贝。
```go
func (接受者变量 接受者类型) 方法名(参数列表) (返回值列表) {
    //方法体
}
```
#### 方法的继承
```go
package main
import "fmt"
type Human struct{
    name, phone string
    age int
}

type Student struct{
    Human
    school string
}

type Employee struct{
    Human
    company string
}

func main() {
    s1 := Student{Human{"aaa","123456",13},"bbbb"}
    el := Employee{Human{"ccc","654321",35},"dddd"}
    s1.SayHi()
    el.SayHi()
}

func (h *Human) SayHi() {
    fmt.Printf("hey~,my name is %s, %d years old, my assoclead is : %s\n", h.name, h.age, h.Phone)
}
```

#### 方法的重写
```go
package main
import "fmt"
type Human struct{
    name, phone string
    age int
}

type Student struct{
    Human
    school string
}

type Employee struct{
    Human
    company string
}

func main() {
    s1 := Student{Human{"aaa","123456",13},"bbbb"}
    el := Employee{Human{"ccc","654321",35},"dddd"}
    s1.SayHi()
    el.SayHi()
}

func (h *Human) SayHi() {
    fmt.Printf("hey~,my name is %s, %d years old, my assoclead is : %s\n", h.name, h.age, h.Phone)
}

func (s *Student) SayHi() {
    fmt.Printf("hey~,my name is %s, %d years old, my assoclead is : %s, study in %s\n", s.name, s.age, s.Phone, s.school)
}

func (e *Employee) SayHi() {
    fmt.Printf("hey~,my name is %s, %d years old, my assoclead is : %s, work in %s\n", e.name, e.age, e.Phone, e.company)
}
```
### 接口
go语言接口是一组方法签名。接口指定了类型应该具有的方法，类型决定了如何实现这些方法。

```go
type 接口名 interface{
    方法1([参数列表]) [返回值]
    方法2([参数列表]) [返回值]
    ...
    方法n([参数列表]) [返回值]
}


func (变量名 结构体类型)  方法1([参数列表]) [返回值] {
    //方法体
}

func (变量名 结构体类型)  方法2([参数列表]) [返回值] {
    //方法体
}

...

func (变量名 结构体类型)  方法n([参数列表]) [返回值] {
    //方法体
}
```

### duck typing
duck typing是描述事物的外部行为而非内部结构。使用duck typing的编程语言被归类为动态类型语言或者解释型语言。duck typing语言中，不要求一个类显示地声明它实现了某个接口。

```go
package main
import "fmt"
type ISayHello interface {
    SayHello() string
}

type Duck struct {
    name string
}

type Person struct {
    name string
}

func (d Duck) SayHello() string {
    return d.name + " ga ga ga!"
}

func (p Person) SayHello() string{
    return p.name + " hello!"
}

func main() {
    duck := Duck{"yaya"}
    person "= Person{"world"}


    var i ISayHello
    i = duck

    i = person
}
```
一个函数如果接受接口类型作为参数，那么实际上它可以传入该接口的任意一个实现类的对象作为参数。

#### 多态
go语言中的多态性是在接口的帮助下实现的--定义接口类型，创建实现该接口的结构体对象。
```go

```

#### 空接口
空接口中没有任何方法。任意类型都可以实现该接口。常用与:

1. println的参数就是空接口。
2. 定义一个map:key是string，value是任意类型。
3. 定义一个切片，存储任意类型的数据。

#### 接口对象转型
```go
instance, ok := 接口对象.(实际类型)

接口对象.(type)
```

## 异常处理
### error

### defer


### panic和recover机制

## 文件i/o操作

## 网络编程

## 数据库编程

## 并发编程

## 密码学算法

## beego框架

## gin框架