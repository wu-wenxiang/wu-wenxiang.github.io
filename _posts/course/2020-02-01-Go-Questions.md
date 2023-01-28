---
layout:         post
title:          Go-逆向学习问题总结
category:       course
description:    逆向法学习 Golang，关于 Golang 需要能回答出的问题
---


## 问题分类

- 核心知识
    - **基础**：开始，开发环境搭建，程序结构和流程，基本数据，复合数据类型，函数
    - **进阶**：方法，接口，goroutine 和通道，并发，包和工具，测试，反射，低级编程
- 常见应用
    - **Web 开发**：Micro / Go-Micro
    - **K8S 开发**：ETCD / Containerd / K8S / CRD

---------------------------------------
---------------------------------------


## 核心知识

### 基础

1. [开始] **Go 在编程语言世界中的处于什么样的位置**？[TIOBE](https://tiobe.com/tiobe-index/)，10-20名，2 次入选年度语言

1. [开始] Go 源于什么需求而被创造？在软件规模日趋复杂时，兼顾代码**易用性、健壮性和性能**。C++（ MySQL / MongoDB ）未能平衡代码易用性和健壮性，Java（ Hadoop ）不够高性能，其它为高并发而生的语言：Erlang（ RabbitMQ ），Scala（ Apache Spark ），Rust

1. [开始] **Go 有什么样的特质和设计哲学**？简单（ 类 C 非 OOP，接口抽象数据灵活 ）、可靠（ 静态类型 & 垃圾回收 ）、高效（ 高并发 ）

1. [开始] Go 继承和发展了哪些语言？C（ 表达式语法，控制流语句，基本数据类型，按值调用的形参传递和指针 ），Pascal / Oberon-2（ 包、导入、声明、方法声明的语法 ），CSP / Alef （ 并发 ），APL（ iota ），Scheme（ 嵌套函数 ），全新（ Slice / defer ），垃圾回收

1. [开始] **Go 适用于哪些场景**？网络服务器 & 客户端工具，可以替换无类型脚本语言，因为兼顾了表达力和安全性。

1. [开始] **和其它高级语言相比，Go 有哪些特性**？ GC，包系统，一等公民函数，词法作用域，系统调用接口，默认用 UTF-8 编码的不可变字符串

1. [开始] **和其它高级语言相比，Go 没有哪些特性**？ 隐式数据类型强制转换（ 浮点数赋值给整数不可以，`int(3.65)`不可以，`float64(3.65)`可以，但`int(float64(3.65))`不行，因为`constant 1.9 truncated to integer`，参考后面的**无类型常量**，但是赋值给变量之后就行`x=float64(3.65);int(x)`；也不可以直接运算，比如 `int(3) + float64(3)`，要显式转换成相同类型，并且转换数值过大导致溢出时行为不保证 ），构造和析构函数，运算符重载，形参默认值，继承，**范型**，**异常**，宏，函数注解，线程局部变量存储（ 依赖编译器判断是否放在线程栈上 & GC ），切片语法糖，map / filter / reduce

1. [开始] Go 的语法兼容性如何？保证向后（下）兼容（ 旧版本 Go 写的程序，能用新版本的编译器和库 ）

1. [开始] Go 有什么缺点？[go-is-not-good](https://github.com/ksimka/go-is-not-good)

1. [开始] **有哪些优秀的 Go 学习资源可以参考**？ 最好的是 [Learning Go Go 语言学习指南：惯例模式与编程实践](https://book.douban.com/subject/35902219/)，其它还有：[TGPL](https://book.douban.com/subject/27044219/)（ [源码](https://github.com/adonovan/gopl.io/) ），[Learning basic Golang in one day](https://github.com/go-training/training)，[Training Meterials](https://github.com/golang/go/wiki/Training)，[官方文档](https://golang.org/doc/#references)，[标准库中文版](https://books.studygolang.com/The-Golang-Standard-Library-by-Example/)

1. [开始] **优秀的三方类库**：[golang.org/x](https://godoc.org/-/subrepo)，[pkg.go.dev](https://pkg.go.dev/)，[gofunk](https://godoc.org/github.com/thoas/go-funk)

1. [开始] 有哪些框架是 Go 语言开发的？容器（ Docker / Kubernetes ），数据库（ TiDB / InfluxDB / ETCD ），消息系统（ NSQ ），缓存系统（ GroupCache ），Web 框架（ go-restful / Beego / Gorilla / Micro / Go-Micro ）

1. [环境] 如何搭建 VSCode 开发和调试环境？[Go in VSCode](https://code.visualstudio.com/docs/languages/go)，[Code Navigation](https://code.visualstudio.com/docs/editor/editingevolved)，[Debugging Go code using VS Code](https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code)

1. [环境] 如何用 Delve 进行调试？[Get Started](https://github.com/go-delve/delve/blob/master/Documentation/cli/getting_started.md)，[Delve Documentation](https://github.com/go-delve/delve/tree/master/Documentation)，[Using the Go Delve Debugger from the command line](https://www.jamessturtevant.com/posts/Using-the-Go-Delve-Debugger-from-the-command-line/)，[Debugging Go with VS Code and Delve](https://flaviocopes.com/go-debugging-vscode-delve/)

1. [环境] 如何搭建 Goland 开发和调试环境？[Debugging code](https://www.jetbrains.com/help/go/debugging-code.html)，[Debugging containerized Go applications](https://blog.jetbrains.com/go/2018/04/30/debugging-containerized-go-applications/)

1. [环境] 代码格式整理？gofmt（`gofmt -d <dir>`，预览格式化代码信息，`gofmt -w <dir>`，格式化代码） / golint（直接敲此命令）

1. [环境] VSCode 怎么调试 golang 代码？Run and Debug，或者用单元测试 `import "testing"`，然后 run test / debug test。如果要运行带 main 函数的 go 文件，可以 OUTPUT 下来菜单里选 go，configure json 选 Launch file，在 Debug Console 里可以看到标准输出

1. [流程] **Go 语言中函数、变量、常量、类型、语句标签、包的命名规则是什么**？类 C、字母 => Unicode，Case

1. [流程] 如下预声明的常量、类型、函数是什么意思，[iota](https://golang.org/ref/spec#Iota) / [rune](https://golang.org/pkg/builtin/#rune) / [cap](https://golang.org/pkg/builtin/#cap) / [imag](https://golang.org/pkg/builtin/#imag)？iota 是希腊字母艾欧他，对应拉丁字母 i，表示下标，结合隐性重复最后一个非空的表达式的语法糖，用于递增场景（ 常量生成器 ）。rune 就是 int32，用于 unicode。

1. [流程] 变量的有效性如何判断？函数内、包内、流程语句，流程代码块内部

1. [流程] **包内有效变量的可导出性如何决定**？。首字母的大小写

1. [流程] 变量命名潜规则？优先短名，作用域越大命名越长且越有意义，驼峰规则，首字母缩写单词用相同大小写

1. [流程] **各类程序实体如何声明？var / const / type / func**

1. [流程] **短变量声明用于哪里**？函数内。函数内一般使用短变量声明（ Python 3.8 中引入的海象运算符 := ），var 一般用于包级别，或者函数内的零值情况。

1. [流程] 短声明最少声明一个新变量（ 不需要声明所有在左边的变量 ），否则要改成赋值等号

1. [流程] **数字 / 布尔 / 字符串 / 接口和引用类型（ slice / 指针 / map / 通道 / 函数 ）的零值（ 变量默认值 ）分别是**？

1. [流程] 变量初始化在何时发生？包级别在 main 函数开始前，局部变量在函数执行期间（ 声明时 ）

1. [流程] 多重赋值语法（ 类似 Python 的元组赋值 ）求公约数？`for y!=0 { x, y = y, x%y }`

1. [流程] 指针变量何时相等？指向同一变量或者都为 nil

1. [流程] **new(T) 实际做了什么事**？实例化一个 T 类型的变量，初始为 T 类型的零值，返回其地址（ *T 类型 ）。`return new(T)` == `{ var dummy T;  return & dummy }`

1. [流程] 每次 new 都会创建一个不同实例么？一般是，但有特例（ 变量类型不携带信息且为零值有相同地址，比如 struct{}，[0]int ），1.13 版本中，这个特例也改掉了（ `a, b := new(struct{}), new(struct{}); &a != &b` ）。

1. [流程] **包级别变量的生命周期是**？整个程序的执行时间。

1. [流程] **局部级别变量的生命周期是**？一直生存直至 GC。如果处于性能考虑，要减少局部变量逃逸，从而减少 GC（ 编译器会在栈上分配内存 ）。

1. [流程] **赋值操作符的写法，递增递减的写法**？+=，++ / -- 后置

1. [流程] **哪些内置函数和方法返回 err**？**map 查询**（`v, ok := map[key]`），**类型断言**（将接口类型的值(x)，装换成类型(T)，x 必须是接口类型，int 不行。T 可以是非接口类型，如果想断言合法，则 T 应该实现 x 的接口，T 也可以是接口，则 x 的动态类型也应该实现接口 T。`type I interface {m()}; var y I; s:=y.(string)` 非法：string 没有实现接口 I (missing method m)，类型断言如果非法，运行时就会出现错误，为了避免这种错误，可以使用一下语法：`v,ok:=x.(T)`），**非阻塞通道接收**（data := <-ch，阻塞接受不了。`data, ok := <-ch`，非阻塞，ok 表示是否接收到值）

1. [流程] **丢弃返回值如何操作**？_

1. [流程] 有哪些隐性赋值？实参到形参，返回值接收，复合类型的字面量表达式（ a := []strings{"A", "B"} ），map，通道

1. [流程] 什么是可赋值性？赋值只有在值对于变量类型是可赋值的时才合法。

1. [流程] **可赋值性规则有哪些**？。类型精确匹配，nil 可以被复制给任何接口变量或者引用类型，常量有更灵活的可赋值性规则以避免显式转换。

1. [流程] 相等性比较的条件？a == b / a != b 合法的前提时 b = a 合法（ 第一个操作数相对第二个类型是可赋值的 ）

1. [流程] **类型转换的使用场景**？双向，单向。具有相同的底层类型（ 不能用算数表达式进行比较和合并 ），或者指向相同底层类型变量的未命名指针类型，可以**双向**转换。string(65) == "A"，但 int("A") 不成立，这就是**单向**转换。如果 x 对于类型 T 是可赋值的，允许类型转换但无必要，因为会隐式转换。

1. [流程] 类型转换产生新的实例么？对于值类型，就是值复制，不存在实例对象的概念。对于 string => []byte 会分配副本，struct 也是类似，`type A struct { id int }; type B A; a := A{1}; b := B(a); &(a.id) != &(b.id)`，就是成员对象复制。

1. [流程] **可以对表达式返回值取地址么**？不可以

1. [流程] 类型转换可能在运行时失败么？不会，静态类型在编译时检查类型转换是否能正常完成。

1. [流程] **相当于 Python 中的 `__str__` 方法如何实现**？`func (a T) String() string { return fmt.Sprintf('<T: %g>', a) }`

1. [流程] **何时会调用到 String()**？%v（%v 相应值的默认格式。`Printf("%v", people) // {zhangsan}`，%+v 打印结构体时，会添加字段名 `Printf("%+v", people) // {Name:zhangsan}` %#v 相应值的Go语法表示，`Printf("%#v", people) // main.Human{Name:"zhangsan"}`，%T 相应值的类型的Go语法表示`Printf("%T", people) // main.Human`），%s，fmt.Println。注意 %g 不会调到这个函数（%g 表示自动保持精度+简洁）。上面的 %#v 对应的是 GoString，默认是带 package.Type{...}

1. [流程] **包的文档注释写在哪里**？开头一句话总结性描述 & 每个包只有一个文件需要描述 & 一般每个包有独立的 doc.go

1. [流程] **怎么使用包里的标识符**？包名.大写字母开头的标识符

1. [流程] **import 的语法**？单行 import "fmt" => 字符串，`import ( "fmt";"math")` 换行，每行一个，这个类似 const 单行和多行语法; 非标准库依赖 $GOPATH "绝对"路径或者 go mod 相对路径（ 默认短名是包名 ）

1. [流程] **类似 Python 中的 import as 的语法怎么写**？`import test "math"`

1. [流程] 什么是包初始化函数？func init() {}，包中变量会在编译时依据依赖关系自动确定先后初始化

1. [流程] **for 有几种形式**？3 种。Condition：`[Condition]`，ForClause：`[ InitStmt ] ";" [ Condition ] ";" [ PostStmt ]`，RangeClause：`[ ExpressionList "=" | IdentifierList ":=" ] "range" Expression`

1. [流程] **for range 的使用**？`for i := range new([5]byte) { fmt.Println(i) } `等同于` for i, _ := range new([5]byte) { fmt.Println(i) }[流程]`，单个 i 是 index，不是 item value。`for _, _ := range s` 等同于 `for range s`。

1. [流程] range后面支持的数据类型有哪些？共5个，分别是数组，数组指针（`x:=[2]int{1, 2}`，下面两种写法完全等价：`for i := range x`和`for i := range &x`），slice，字符串，map 和 channel

1. [流程] 纯概念，作用域（ 编译时属性，声明在程序文本中出现的区域 ），变量的生命周期（ 运行时概念 ），语法块（ 大括号里的语句序列 ），词法块（ 推广语法块到没有显式包含在大括号里的声明代码 ）。

1. [流程] **在一个作用域内，x := x + 1，这里的两个 x 是同一个变量么**？不是！

1. [流程] **怎么理解 for / if 的作用域**？分隐式块和显式块。隐式块包含：声明 + 条件 + 后置语句（ for 循环 ）+ 显式块

1. [流程] **怎么理解 if / else if / else**？单纯的嵌套（ 与之对应的，也是作用域嵌套 ）

1. [流程] 怎么理解 switch 的作用域？条件对应一个块，每个 case 语句体对应一个块。

1. [流程] switch 使用的时候有什么注意事项？switch 每一个 case 都默认带 break，有 fallthrough 相当于允许继续下一个判断，但是否继续要看下一个判断里有没有 fallthrough。switch 里有一种特殊的写法，叫**类型分支**，类似类型断言，element.(type)，表示 element 的类型，用于实现特设多态（ad hoc polymorphism）。另一种多态就是 OO 里的子类型多态（subtype polymorphism）

1. [流程] if / for 和 err 怎么搭？判断条件中有 err，则常规代码写在 else 里。

1. [流程] init 和 err 怎么搭？在 init 函数中，var err 私有化，然后用多重赋值，而不是短声明

1. [IO] 标准输入、标准输出、标准错误？os.Stdout，os.Stdin，`for input.Scan { fmt.Println(input.Text()) }`，`in := bufio.NewReader(os.stdin)`，`in := bufio.NewScanner(os.stdin)`，`input.Split(bufio.ScanWords)`，`input.Scan()`

1. [基本] **Go 数据类型的分类**？4 大类。basic（ number 整数、浮点数、复数 / string / boolean ），aggregate（ array / struct ），reference（ pointer / slice / map / function / channel ），interface

1. [基本] **如何确认数据类型**？%T，`reflect.TypeOf(42).String()  // int`

1. [基本] 复合类型数据有几种？哪几种长度固定？哪几种可以动态增长？长度固定：数组 & 结构体。动态数据结构：slice / map

1. [基本] make 怎么创建 slice / map / chan？slice：`make([]int, len, [cap]) == make([]int, cap)[:len]`，map：`make(map[string]int)`，chan：`make(chan string)`

1. [整数] 整数类型有几种？固定大小：uint8-32 / int8-32 / rune(int32) / byte，不固定：int / uint / uintptr（ 仅用于底层编程 ）

1. [整数] 如何实现乘方运算？math.Pow(float64, float64)

1. [整数] 三元运算符？没有，只能 `var a int; if true { a = 5 } else { a = 6 }`

1. [整数] 有哪些一元运算符，哪个是按位取反运算符？`+  /  -  ^ !`，一元 ^ 表示逐位取反，! 逻辑取反

1. [整数] 有哪些二元运算符？`*  /  %  <<  >>  &  &^`，`+  -  ｜ ^`，`==  !=  <  <=  >  >=`，`&&`，`||`。`&&` 表示逻辑乘，优先级高，`||` 表示逻辑和，`&` 和 `|` 同理。

1. [整数] **如何将数字转换成字符串**？`fmt.Sprintf("%b %o %x %d", ...)`，`strconv.FormatInt(42, 17)`，`strconv. Itoa()`

1. [整数] **如何将字符串转换成数字**？[strconv](https://golang.org/pkg/strconv/)，`strconv.ParseInt("42", 8, 64)`，`strconv. Atoi()`，fmt.Scanf 不够灵活

1. [整数] 为什么数组长度不可能为负数，还是采用有符号整型？for 循环--避免溢出

1. [整数] 左移、右移的处理逻辑和注意点？`x << n => x * 2^n`，`x >> n => x / 2^n` 向下取整，n 必须无符号。有符号右移高位填充符号位，因此位模式处理右移需要无符号整型

1. [整数] 无符号整数用于什么场景？位运算 & 特定算术运算，一般不用于表示非负值

1. [整数] fmt.Printf 怎么用 index，# 是什么意思？`"%#[1]d"`，`fmt.Printf("%#[2]d", 1, 2)` 打印出 2，**#0 的话是 badindex**

1. [整数] **单引号括起来的单个文本符号表示什么**？字符。rune(int32) / byte(int8)，`%d %c %q`，%c 表示相应 Unicode 码点所表示的字符`Printf("%c", 0x4E2D)` => 中，%q 表示单引号围绕的字符字面值，由Go语法安全地转义`Printf("%q", 0x4E2D)` => '中'

1. [整数] **ASCII 和字符转换**？`str(62)`，`%c %d`，`[]rune("hello"), []byte("hello")`。`a, b := []rune("我hello"), []byte("我hello");fmt.Printf("%+v%+v", a, b)` => `[25105 104 101 108 108 111][230 136 145 104 101 108 108 111]`

1. [整数] 浮点数转换成整数的逻辑是什么？丢失小数部分。`x:=1.9;int(x)`，向上取整`math.Ceil(1.9)`，向下取整`math.Floor(1.9)`，四舍五入`math.Round(1.9)`

1. [整数] 参考：[随机数](https://zhuanlan.zhihu.com/p/94684495) math/rand // 伪随机，crypto/rand // 真随机，性能相差十倍。伪随机每次需要设置 seed，seed 相同则随机数相同，所以常用时间戳。三方：随机整数 [funk.RandomInt(0, 100) // will be between 0 and 100](https://github.com/thoas/go-funk#funkrandomint)，随机字符串 [funk.RandomString(4) // will be a string of 4 random characters](https://github.com/thoas/go-funk#funkrandomstring)

1. [浮点] Go 支持支持几种浮点数类型？算术特性遵从什么标准？float32 / float64，IEEE 754 ( `a=0.1; a+0.2 != 0.3` )，注，字面常量这里相等的 `0.1+0.2==0.3`，参考后面的**无类型常量**。

1. [浮点] float32 和 float64 的精准度如何？32 有效数字大约 6 位，64 大约 15 位。**绝大多数情况用 64**，避免运算误差迅速累计放大。

1. [浮点] **%g, %f, %e 有和区别**？%g 自动保持精度+简洁，%f 无指数，%e 有指数。都可以通过谓词 %8.3f 掌握宽度和精度。

1. [浮点] 如何表示最大的 64 位浮点数？正负无穷大（ 超出许可值，非零除以零 ）？数学上无意义的运算结果（ 0/0，sqrt(-1) ）？math.MaxFloat64，math.Inf(64)，-math.Inf(64)，math.NaN()。`func div0(f float64) float64 {return f/0};fmt.Printf("%+v %+v %+v\n", math.MaxFloat64, div0(1), div0(0))` => `1.7976931348623157e+308 +Inf NaN`

1. [浮点] 如何将 NaN 作为信号值进行判断？math.IsNaN。NaN 的比较总为 false，但 != 除外，因为它与 == 的值总是反的

1. [浮点] 如果一个函数的返回值是浮点型且有可能出错，怎么处理？单独报错。return ret, err

1. [复数] 复数的类型？complex64 / complex128，二者分别由 float32 和 float64 构成。real(complex128)，imag(complex128) 取实部和虚部

1. [布尔] **布尔值和数值的转换**？无法隐式转换。`i := 0; if b { i = 1 }`，`b = i!=0`

1. [布尔] 布尔值如何转换成字符串？%t

1. [字符] **字符串转义**？`\d \b \f \n \r \t \v \' \" \\`，参考 [golang fmt 格式占位符](https://studygolang.com/articles/2644)

1. [字符] **原始字符串怎么写，用于何处**？反引号，可以换行。用于正则表达式，路径，HTML/JSON 模版，多行的命令行提示信息。

1. [字符] 长行字符串如何表示？a := "xxx" +，换行，tab，"yyy" +，换行，tab，"zzz"

1. [字符] **字符串遍历**？`for _, i := range "hello" {}`，这里 %T 看，会发现 i 是 int32，**按 UTF-8 隐式解码**。%x 可以看到是 3 个字节。

1. [字符] **字符串与字节 slice 如何互换**？[]byte(s)，分配内存，拷贝填入 s 含有的字节（ 这一步可能会被编译器优化掉 ），并生成一个 slice 引用，指向整个数组。

1. [字符] **如何令字符串索引能支持 Unicode**？[]rune 转换成字符串？`[]rune("he我llo")[2]`，`string([]rune("hello"))`

1. [字符] Unicode 和 UTF-8 什么关系？UTF-8 以字节为单位对 Unicode codepoint 做变长编码，由 Go 语言创建者 Ken Thompson & Rob Pike 发明。Unicode 定长编码是 UTF-32，也叫 UCS-4。`'世' == '\u4e16' == '\U00004e16'`。UTF-8 本身不会嵌入 NUL 0 值，便于某些程序语言用 NUL 标记字符串结尾。

1. [字符] **字符串怎么存储的？可变么**？不可变字节序列，文本字符串被解读成 **UTF-8 编码的 Unicode 码点序列**。类似这样存储：`struct { data pointer; len int }`

1. [字符] Golang 对 UTF-8 的支持情况如何？Go 的源文件**总是**以 UTF-8 编码，Go 操作文本字符串**优先**用 UTF-8 编码。

1. [字符] **怎么对 UTF-8 进行编解码**？unicode/utf8，`for _, i := range "1世a" { buf := make([]byte, 3); n := utf8.EncodeRune(buf, i); fmt.Println(buf[:n])}`，`DecodeRuneInString(s[i:])`，或者用：`golang.org/x/text/encoding/simplifiedchinese`、`golang.org/x/text/transform`，`reader := transform.NewReader(bytes.NewReader(s), simplifiedchinese.GBK.NewDecoder()); d, e := ioutil.ReadAll(reader)`，`reader := transform.NewReader(bytes.NewReader(s), simplifiedchinese.GBK.NewEncoder()); d, e := ioutil.ReadAll(reader)`

1. [字符] **根据字符串索引对应的字符**？s[i] => 字符。`s := "hello"; s[1]` 是 byte 还是 rune，这里 %T 看，会发现是 int8，即使里面有中文。`len("我") == 3`。

1. [字符] **如何获取字符串长度**？字节长度 len(s)，字符长度 utf8.RuneCountInString(s)

1. [字符] **字符串 slice 运算符的操作逻辑**？s[i:j] / s[:j] / s[:i] / s[:]，子串生成操作，左闭右开，产生新的字符串对象，但底层不重新分配内存，因为字符串不可变，所以可以安全地共用底层。[import "index/suffixarray"](https://golang.org/pkg/index/suffixarray/)

1. [字符] **怎么理解字符串加法 +=**，比如 `s += "hello"`？产生一个新的字符串实例并赋值给 s。底层应该也不是直接加而是分配全新的内存空间，因为对同一个底层，多个字符串都可以 += 不同的新串。

1. [字符] **怎么实现序列反转**？[funk.Reverse([]int{0, 1, 2, 3, 4})](https://github.com/thoas/go-funk)，[源码：transform.go](https://github.com/thoas/go-funk/blob/master/transform.go#L279)，`for i,j:=0,len(s)-1; i<j; i,j:=i+1,j-1 { s[i],s[j] = s[j],s[i] }`

1. [字符] **怎么区分单个文字符号是字母还是数字？怎么转换大小写**？`unicode.IsDigit(i)`, `unicode.IsUpper(i) || unicode.IsLower(i)`，`unicode.ToUpper(i)`，`unicode.ToLower(i)`

1. [字符] **有哪些字符串处理库函数**？strings.HasPrefix `len(s) >= len(prefix) && s[0:len(prefix)] == prefix`，Contains，ToUpper，Index，Field，Split，Join 与 bytes ( 处理 slice ) 里的对应。`strings.TrimSpace(" \t\n Hello, Gophers \n\t\r\n")` => Hello, Gophers

1. [字符] 处理 URL 和文件路径？path，path/filepath

1. [字符] **如何高速处理字符串**？bytes.Buffer / WriteRune / fmt.Fprint

1. [字符] 如何实现旋转滚动效果？```for _, r := range `-\|/` {fmt.Printf("\r%c", r); time.Sleep(delay)}```

1. [常量] **常量的基本语法**？const pi [float64] = 3.1415926，或者加括号，多行表示，const 可以结合 iota 完成递增逻辑。如果没有赋值，会复用前一行的表达式及其类型。参考 [iota: Golang 中优雅的常量](https://segmentfault.com/a/1190000000656284)

1. [常量] 常量的类型？常量本质上都属于基本类型（ 包括 type 具名的基本类型 ）：布尔 / 字符串 / 数字

1. [常量] 常量计算在编译时还是运行时完成？不确定，但对于常量操作数，所有数学、逻辑、比较运算的结果还是常量。常量的类型转换结果和一些内置函数的返回值，例如 len / cap / real / imag / complex / unsafe.Sizeof 同样是常量。

1. [常量] 常量表达式用于何处？数组类型声明长度时。

1. [常量] **什么是无类型常量？为什么需要无类型常量**？math.Pi 或者其它字面值 bool / int / char / float / complex / string。如果 math.Pi 一开始就属于 float64，则 math.Pi 相关的常量运算精度会下降。因此，在编译阶段，编译器会保持高精度，可以认为精度至少达到了是 256 位，因此 0.1+0.2 == 0.3。

1. [数组] 数组一般用于什么场景？长度固定（ 不需要增加 ）的场景，比如 Sum256 等。很少用到。

1. [数组] **怎么定义和初始化数组**？var a [3]int，可以用数组字面量来初始化数组 var q [3]int = [3]int{1, 2, 3}，或者 q := [...]int{1, 2, 3}，... 只能用于数组字面量，不可以 var [...]int = [...]int{1, 2}，数组长度在编译时确定。**长度也是数组类型的一部分**，不可以 var [3]int = [2]int{1,2}。数组长度必须是常量表达式。

1. [数组] **数组初始化完成后还可以批量重新赋值么**？可以。`a := [...]int{1, 2, 3, 4}; a = [...]int{5, 2, 3, 4}`，后面一句长度必须为 4，3 和 5 都不行，因为前面一句已经确定了 a 的类型是 `[4]int`。

1. [数组] **怎么遍历数组**？`for i, v := range a {...}`，`for i := range a {...}`，i 是 index

1. [数组] **对数组字面量，如何做部分赋值（ 剩余为 0 值 ）**？r := [...]int{99:-1}，len(r) == 100，`a := [...]int{1: 4, 18: -1}` => [0 4 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 -1]

1. [数组] **数组作为形参是值传递还是引用传递**？值传递。如果希望引用传递，可以用数组指针 ptr *[32]byte

1. [数组] 如何比较两个数组？精确比较 == / !=，摘要比较 crypto/sha256，sha256.Sum256([]byte("x"))

1. [切片] 怎么比较 []byte？slice 不能直接比较（ 唯一合法是和 nil 比较 ），可以用 bytes.Equal，或者string([]bytes)进行比较

1. [切片] **Slice 对象有哪些属性？存储模型是怎样的**？指针（ 指向底层数组的第一个可以被 slice 访问的元素 ）；长度（ slice 元素个数，小于容量）；容量（ slice 起始元素到底层数组的最后一个元素 ）。

1. [切片] Slice 操作符 s[i:j] 会创建的 slice 么？会分配内存创建不同的底层数组么？`0<=i<=j<=cap(s)`，会创建新的 Slice，一般会共用底层数组。字符串切片返回字符串，slice 切片返回 slice，array 切片返回 slice。

1. [切片] 如果 slice 的引用超过了 cap 会如何？超过了 len 呢？超过 cap 会宕机，超过 len 但不超过 cap，正常返回。

1. [切片] 为什么 slice 不能直接判断相等？1. slice 可能会包含自身 2. slice 底层数组可能随时改变（ map 的键在散列表的生命周期中必须保持不变，所以 slice 不可以用作 map 的键 ）

1. [切片] **怎么判断一个 slice 是否为空**？长度为 0 的 slice 和值为 nil 的 slice，len(s) 都 == 0？接口对外应该不区分二者，呈相同处理方式。nil：`var s []int`，`s = nil`，`s = []int(nil)`；非 nil 但 len = 0：`s = []int{}，s = make([]int, 3)[3:]`。`a, b := []int{}, []int(nil); fmt.Println(len(a), a, a == nil, len(b), b, b == nil)` => `0 [] false 0 [] true`

1. [切片] **append 的用法和底层实现是怎样的**？slice = append(slice, item)，需要扩容时会生成新的底层数组（ 一般直接扩一倍 ）。只要可能改变 slice 的长度、容量，或者指向不同地层数组，都需要更新 slice 变量，重新赋值回去。`stringSlice := make([]string, 5, 10); stringSlice = append(stringSlice, "5", "6"); stringSlice = append(stringSlice, []string{"7", "8"}...)`

1. [切片] **slice copy 的用法和底层实现是怎样的**？n = copy(dst, src)，不会产生一个新的底层数组，src 和 dst 可能重合（ 因此 copy 函数执行后，src 可能变化 ）。n = min(dst, src)

1. [切片] 什么场景下会重用 slice 底层数组？过滤部分元素。

1. [切片] **如何用 slice 实现一个栈**？`s = append(s, v)` 作为 push，`s = s[:len(s)-1]` 作为 pop。**注意，这里要判断下长度，下标越界会报错的** `[]int(nil)[0:]` 不会报错，但 endindex 写 1 就会报错了，也不能写 -1

1. [切片] 怎么实现 remove 一个 slice 元素？`copy(s[i:], s[i+1:]); return s[:len(s)-1]`，若不需要维持顺序，可以 `s[i] = s[len(s)-1]; return s[:len(s)-1]`

1. [切片] 是否可以获取 array / slice 元素的地址？可以

1. [切片] 如何给字符串 slice 排序？sort.Strings(s)

1. [切片] 怎么判断一个元素在 slice 中？[funk.Contains](https://github.com/thoas/go-funk#funkcontains)，IndexOf，LastIndexOf

1. [切片] 切片运算：洗牌，Sum，Reverse 怎么操作？[funk.Shuffle](https://github.com/thoas/go-funk#funkshuffle)，[funk.Sum](https://github.com/thoas/go-funk#funkshuffle)，[funk.Reverse](https://github.com/thoas/go-funk#funkreverse)

1. [切片] 怎么间隔取值？先 [funk.Chunk](https://github.com/thoas/go-funk#funkchunk)，再用 Map 取高维列表第一个值。或者直接用 Filter 做整除判断

1. [切片] 怎么拍平多维列表？[funk.FlattenDeep](https://github.com/thoas/go-funk#funkflattendeep)

1. [字典] **Map 对象有哪些属性？底层实现是怎样的**？map[K]V，是无序集合，键值必须唯一（ 必须可以比较 ）。K 必须类型相同，V 也必须类型相同。浮点型可以比较，但请不要。如果较真一下，`a := map[float64]string{3.65: "hello"}` 是可以的。

1. [字典] Map 怎么比较是否相等？自己写循环。和 slice 一样，map 不能直接比较，唯一合法的比较是和 nil 比较。

1. [字典] **Map 对象怎么声明和初始化**？`ages := make(map[string]int)` == `map[string]int{}`，也可以直接通过 map 字面量进行初始化 `ages = map[string]int{ "alice":31, "charlie":14}`

1. [字典] **Map 怎么增加元素，删除元素**？`ages["green"] = 15`，`delete(ages, "green")`

1. [字典] Map 查找元素时，找不到键值如何返回？返回零值。类似 DefaultDict，因此可以直接 `ages["bob"]++`

1. [字典] 是否可以获取 map 元素的地址？不是变量，不可以获取地址。&ages["bob"] 编译失败。不可以获取地址的原因是随着元素增加，已有元素可能会被散列到新的存储位置，这样可能使获取的地址无效。slice 也可能底层数据扩展，但原来的地址总还是在的，不会无效。

1. [字典] **如何遍历一个 map**？`for name := range(ages)`；`for name, age := range(ages)`，迭代顺序不固定。

1. [字典] map 的零值 nil 有什么注意事项？不可以为零值中设置元素，必须先初始化。`var ages map[string]int; ages = map[string]int{}; ages["hello"] = 1`

1. [字典] **当一个键不存在的时候，map 取索引会返回零值，如何能知道键值是否存在**？`if ages,ok:=ages["bob"];!ok { not exist... }`

1. [字典] 怎么实现集合？只用 map 的键。但没有集合运算内置库函数。可以用三方的：交集 [funk.Intersect](https://github.com/thoas/go-funk#funkintersect)，差集 [funk.Difference](https://github.com/thoas/go-funk#funkdifference)，子集 [funk.Subset](https://github.com/thoas/go-funk#funksubset)，并集 [funk.Uniq](https://github.com/thoas/go-funk#funkuniq)

1. [字典] 当希望键是 slice 时，怎么办？添加 hash 核函数 k，对 s1 == s2 时，k(s1) 才会 == k(s2)

1. [字典] 字典操作 keys / values 怎么做？[funk.Keys](https://github.com/thoas/go-funk#funkkeys)，map 或者 struct 都可以。[funk.Values](https://github.com/thoas/go-funk#funkvalues)

1. [结构] **结构体的定义和使用**？`type xx struct { ID int }; var y xx; fmt.Println(y.ID)`

1. [结构] **结构体指针的用法**？字段的指针：`&y.ID`，结构体指针：`y.ID == (&y).ID`，有一个诡异的语法，`func test() astruct {}` 返回结构体，可以 print test().ID，但不能对字段直接赋值 test().ID = 4，可是如果 func 返回的是 *astruct，就可以这么写。**方法类型是结构体还是结构体指针，涉及到方法能否改变实例本身**，结构体方法 return struct 并且方法类型也是 struct（不是引用）才能写连等

1. [结构] **结构体字面量赋值**？x := Point{1,2}，**当 2 和 } 不在一行时，2 后面必须加上逗号**。

1. [结构] **如何实现递归结构体，比如二叉树**？聚合类型不可以包含自己，但能定义一个同类型的指针成员变量。`type tree struct { value int; left,right *tree}`

1. [结构] 生成二叉树？二叉树遍历？递归。

1. [结构] 空结构体 struct{}{} 有什么用？没有长度，不携带任何信息。尽量不用。

1. [结构] 成员变量的顺序对结构体来说有意义么？有，顺序不同，就是不同的结构体。

1. [结构] **结构变量的初始化方法**？序列初始化，（部分）字典初始化。二者不可混用，并且二者的字段命名都要符合包导出规则。`x := p.T{A:1, C:3}`，`x := p.T{1, 3}`，跨包用时就不能用小写

1. [结构] 结构体在作为函数的参数和返回值时，是值传递还是引用？值传递。

1. [结构] **常用的创建一个 struct 并获取其地址的语法是**？`pp := &Point(1,2)`，不用 `pp := new(Point); *pp = Point(1,2)`

1. [结构] 结构体是否可以比较？可以用 `==` / `!=`，可以用作 map 的 key

1. [结构] **什么是结构体嵌套？有什么用**？便于直接取得成员的成员的值，类似继承语法（ 只要成员的成员开头大写，不管成员如何，快捷方式写法可以导出。w.circle.X 不可以导出，但 w.X 可以导出 ）。

1. [结构] **什么是匿名成员？匿名成员有什么用**？成员没有名字，只有类型。用于实现继承。参考 [Nameless fields in Go structs?](https://stackoverflow.com/questions/28014591/nameless-fields-in-go-structs)，匿名成员可以用 T，也可以用 *T，参考 [Embedding when to use pointer](https://stackoverflow.com/questions/27733854/embedding-when-to-use-pointer/27733969#27733969)：An embedded type must be specified as a type name T or as a pointer to a non-interface type name *T, and T itself may not be a pointer type.

1. [结构] "%#v" 有什么用？输出结构体时包含成员变量的名字

1. [模板] 通过什么库支持 JSON / ASN.1 / XML？encoding/json，encoding/xml，encoding/asn1

1. [模板] JSON 本质是什么？JavaScript 值的 Unicode 编码，包括字符串（ 双引号扩起来的 Unicode 代码点序列，\uhhh转义得到 UTF-16 编码 ）、数字（ 十进制或者科学记数法 ）、布尔（ true/false ）、数组和对象。

1. [模板] 什么是结构体的成员标签（ field tag ），有什么用？\`json:"color, omitempty"\`

1. [模板] 结构体如何转换成 JSON？json.Marshal / json.MarshalIndent，**首字母大写的字段才可以转，基于反射**

1. [模板] JSON 如何转换成结构体？json.unmarshal，注意 unmarshal 忽略大小写

1. [模板] json.Marshal 和 json.Decoder 有何区别？整体解码和流式解码。json.NewDecoder(resp.Body).Decode(&result)

1. [模板] 文本模板和 HTML 模板用什么标准库处理？text/template（ 会对 HTML/JS/CSS/URL 自动转义 ），html/template

1. [模板] 模板语法是怎样的？变量 {{.VAR}}，循环 {{range .Items}} {{end}}，判断，过滤器 {{.CreateAt | daysAgo}}。func daysAgo (t time.Time) int {}

1. [模板] 如何使用模板的链式调用？template.New("report").Funcs(template.FuncMap("daysAgo", daysAgo)).Parse(templ)

1. [模板] 如何先解析检查，再执行模板转换？var report = template.Must(template.New...); report.Excute(os.Stdout, result)

1. [函数] 变长形参怎么写？`func aFun(x int, y ...int) {}`

1. [函数] slice 中的每个元素作为实参怎么写？`aFun(x, runes...)`

1. [函数] 高阶函数 [github.com/thoas/go-funk](https://github.com/thoas/go-funk)

1. [函数] **函数的类型（函数签名）包含哪些元素**？形参列表和返回值列表。形参列表和返回值列表相同，可以认为这两个函数的类型或者签名相同。形参和返回值的名字不回影响到函数类型。

1. [函数] 函数的形参可以有默认值么？不能。

1. [函数] 函数可以接受命名传参么？不能。

1. [函数] 什么是匿名函数？命名函数只能在包级别的作用域进行声明。函数字面量可以在表达式中指定函数变量。函数字面量像函数声明，但func 关键字后面没有函数的名称。它是一个表达式（可以直接调用），它的值就是匿名函数。**函数变量称之为闭包**。

1. [方法] 方法和函数有什么区别？`func (s* Struct) funName() (ret) { ... }`，s.funName()

1. [函数] 函数传参是值传递还是引用传递？值传递，比如数组，结构体，都是值复制。除非值本身是引用，比如指针，slice，map 等。

1. [异常] **错误处理要注意什么**？`if err: = fun(); err != nil { 错误处理 }`，然后正常处理逻辑不要放在 else，而是放在外层作用域。

1. [异常] **延迟函数是什么**？有什么用？延迟函数是类似 finally 的存在。一个普通的函数或者方法调用时，在前面加上 defer 就是延迟函数 `defer test()`。defer 一般用于成对的操作，比如打开文件和关闭，加缩和解锁。一般在成功获得资源（比如打开文件）之后，立即 `defer fun( # 释放资源 )`，如果放在 return 或者 panic 后面，defer 就无法执行了。defer 会在当前函数 / 方法的 return 语句之后执行，是退出当前函数 / 方法的最后一步操作。执行时可以更新函数的结果变量。比如声明时结果变量是 output int，return 4，def 里可以 output = 8，`defer func() {output = 8}()`，这是用闭包获取外层函数变量。然后返回就是 8。

1. [异常] 宕机和恢复怎么处理？0/x ，x==0 时就会发生 panic，也可以通过 panic(bailout{}) 抛宕机异常。**若递归内层函数发生 panic，递归外层函数的 defer 被调用过的还是会生效的**。recover 会写在 defer 里，若 panic 发生，recover 会返回 panic 的参数；反之，若 panic 未发生，recover 执行时返回 nil。**一般写法是 switch(p) 里先 case nil，然后 case 预期宕机值，比如 bailout{}，最后 default，default 里继续 panic(p)**。有些情况下没有恢复动作，比如内层耗尽导致 Go 运行时发生严重错误，会直接终止进程。
### 进阶

1. [接口] **sort.Interface 怎么使用**？实现 Len() int / Less(i, j int) bool / Swap(i, j int) 三个方法（i/j 是下标），即可用 sort.Sort 实现原地排序

1. [包] go mod 从什么版本开始支持？1.11/1.12 可以用 GO111MODULE=on 打开，1.13 默认打开

1. [包]  go mod 的参考资料？官方：[Using Go Modules](https://blog.golang.org/using-go-modules)，非官方：[Introduction to Go Modules](https://roberto.selbach.ca/intro-to-go-modules/)，[常见问题](https://colobu.com/2018/08/27/learn-go-module/)，比如翻墙：` replace golang.org/x/text v0.3.0 => github.com/golang/text v0.3.0`，download & why

1. [包]  **go mod 的基本使用步骤**？`go mod init example.com/hello` 初始化，`go list -m all` 列举模块，`go get golang.org/x/text` 拉取最近的 tagged 版本，`go get rsc.io/sampler@v1.3.1`，`go doc rsc.io/quote/v3` 文档，`go mod tidy` 去掉无用的依赖。参考 [How to Write Go Code](https://golang.org/doc/code.html#Library)

1. [包]  怎么把项目迁移到 go mod？[Migrating to Go Modules](https://blog.golang.org/migrating-to-go-modules)

1. [包]  如何发布一个 go mod？[Publishing Go Modules](https://blog.golang.org/publishing-go-modules)

1. [包]  项目的大版本变更或者多版本一起维护时，go mod 应如何处理？[Go Modules: v2 and Beyond](https://blog.golang.org/v2-go-modules)

1. [并发] **goroutine 的一般用法是**？`go f()` 新建一个调用 f 的 goroutine，与主协程并发，不等待。goroutine 类似 daemon 线程（主 goroutine 结束时，所有 goroutine 都暴力终结），但数量级有很大差异（百万 goroutine）。goroutine 的栈默认很小，只有几 k，又可以扩大到几 G。 goroutine 不能被杀死，只能通信告知令其自杀。

1. [并发] **goroutine 类似线程/进程中 Join 操作是**？在 goroutine 中写 `c <- true`，在主 goroutine 中 `<- c` 取值。或者，主 goroutine 中 `wg.Add(1)` & `go func() { defer wg.Done() ... }`

1. [并发] **管道的一般用法是**？管道，可以多个值，也可以给多个 goroutine 用，但 close 只能一次。`for v := range c {...}` range 会在 c close 且取完之前持续阻塞，如果 c 忘记关闭了，会容易引起 deadlock。

1. [并发] **什么是有缓冲和无缓冲通道**？make(chan int) 和 make(chan int, 0) 容量为 0，表示无缓冲通道。make (chan int, 3)，返回容量为 3 的缓冲通道（**缓冲通道实现了一个计数信号量，计数上限为 1 的信号量称为二进制信号量 binary semaphore**）。无缓冲通道上发送操作会阻塞，直至另一个 goroutine 上在对应的通道上执行接收操作，此时值传送完，两个 goroutine 都可以继续。反之，如果接收操作先执行，接收方 goroutine 将阻塞，直到另一个 goroutine 在同一个通道上发送一个值。使用无缓冲通道导致发送和接收方 goroutine 同步化。

1. [并发] 通道关闭后，后续针对这个通道的发送和接收操作会有怎样的结果？后续的发送操作会导致应用崩溃，后续的接收操作能正常。

1. [并发] **怎么判断一个通道是否关闭**？x, ok := <- ch，ok 为 true 表示接收成功，false 表示当前的接收操作在一个关闭并且读完的通道上。利用这个特性，可以 for { ... } 读完通道上所有的值，然后 close(ch)，range 适用于这种场景（close 并读完才会结束 range 循环）。

1. [并发] 何时应该关闭通道？关闭每一个通道不是必须的，**只有在通知接收方 goroutine，所有数据都已经发完，才需要关闭通道**。通道可以通过垃圾回收器根据它是否可以访问来决定是否回收它，而不是根据它是否关闭。每一个文件需要 close，而通道未必。

1. [并发] 什么是单向通道，有什么用？ chan<- int 是一个只能发送的通道，<-chan int 是一个只能接收的通道。一般用于形参（实参 chan int 会隐式转换），用于防止 goroutine 中误用，类似只读形参效果。

1. [并发] select 多路复用是什么？有什么用？类似 switch 语法，switch 换成 select，case 里 x := <- ch 或者 ch <- i。select 会一直等待，直到有一个 case 可以执行；如果多个情况都满足，select 随机选择一个，以保证每个通道有相同的机会被选中。

1. [并发] select 怎么实现非阻塞通信？select 可以有一个默认的情况，default，用来指定没有其它通信发生时可以立即执行的操作。然后可以实现非阻塞 & 轮询

1. [并发] 通道的零值是什么？有什么用？通道的零值是 nil。在 nil 通道上发送和接收会永远阻塞。在 select 对 nil 通道的 case 永远不会被选中，可以利用非阻塞 select 执行超时处理或者取消。

1. [并发] 互斥锁和读写互斥锁各自用于什么场合？sync.Mutex 是普通互斥锁，仅在绝大部分 goroutine 都在获取读锁并且锁竞争比较激烈时（即，goroutine 一般都需要等待后才能获得锁），sync.RWMutex 才有优势。竞争不激烈时，RWMutex 比 Mutex 慢，因为 RWMutex 需要更复杂的内部簿记工作。

1. [并发] 如何理解 goroutine 只保证在单个 goroutine 内的串行一致。`var x, y int; go func() { x=1; fmt.Print(y)}(); go func() { y=1; fmt.Print(x)}();` 可能输出 `y:0 x:0` 和 `x:0 y:0`

1. [系统] 怎么获取环境变量？`os.Getenv("GOLANG_PROJECT")`

1. [系统] 怎么获取 CPU 信息？runtime.NumCPU() 可以获取逻辑 CPU。runtime.GOMAXPROCS(n) 能设置实际并发 goroutine 数量并返回之前设置，n<1 时不设置只返回之前的设置。goroutine 访问堆变量会遇到类似线程的竞争问题（GOMAXPROCS=1 时若带阻塞也一样有问题），访问栈变量就没事

1. [系统] 怎么 Sleep？time.Sleep(time.Millisecond * time.Duration(funk.RandomInt(500, 1000))) 不能乘 int，需要类型转换成 time.Duration，但可以乘无类型直接量 1

1. [系统] Server 端 TCP 程序实现？`listener = net.Listen("tcp", "localhost:8000"); for { conn = listener.Accept(); go handleConn(conn)}` handleConn 里要先 defer conn.Close()
