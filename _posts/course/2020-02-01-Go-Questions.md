---
layout:         post
title:          Go-逆向学习问题总结
category:       course
description:    逆向法学习 Golang，关于 Golang 需要能回答出的问题
---


## 问题分类
- 核心知识
	- **基础**: 开始，开发环境搭建，程序结构和流程，基本数据，复合数据类型，函数
	- **进阶**: 方法，接口，goroutine 和通道，并发，包和工具，测试，反射，低级编程
- 常见应用
	- **Web开发**: Micro / Go-Micro

---------------------------------------
---------------------------------------


## 核心知识

### 基础

1. [开始] **Go 在编程语言世界中的处于什么样的位置？**[TIOBE](https://tiobe.com/tiobe-index/)，10-20名，2 次入选年度语言

1. [开始] Go 源于什么需求而被创造？在软件规模日趋复杂时，兼顾代码易用性、健壮性和性能。C++（ MySQL / MongoDB ）未能平衡代码易用性和健壮性，Java（ Hadoop ），为高并发而生的语言：Erlang（ RabbitMQ ），Scala（ Apache Spark ），Rust，Go

1. [开始] **Go 有什么样的特质和设计哲学？**简单（ 类 C 非 OOP，接口抽象数据灵活 ）、可靠（ 静态类型 & 垃圾回收 ）、高效（ 高并发 ）

1. [开始] Go 继承和发展了哪些语言？C（ 表达式语法，控制流语句，基本数据类型，按值调用的形参传递和指针 ），Pascal / Oberon-2（ 包、导入、声明、方法声明的语法 ），CSP / Alef （ 并发 ），APL（ iota ），Scheme（ 嵌套函数 ），全新（ Slice / defer ），垃圾回收

1. [开始] **Go 适用于哪些场景？**网络服务器 & 客户端工具，可以替换无类型脚本语言，因为兼顾了表达力和安全性。

1. [开始] **和其它高级语言相比，Go 有哪些特性？**GC，包系统，一等公民函数，词法作用域，系统调用接口，默认用 UTF-8 编码的不可变字符串

1. [开始] **和其它高级语言相比，Go 没有哪些特性？**隐式数据类型强制转换（ 浮点数赋值给整数不可以，也不可以直接运算，比如 `int(3) + float64(3)`，要显式转换成相同类型，并且转换数值过大导致溢出时行为不保证 ），构造和析构函数，运算符重载，形参默认值，继承，**范型**，**异常**，宏，函数注解，线程局部变量存储（ 依赖编译器判断是否放在线程栈上 & GC ），切片语法糖，map / filter / reduce

1. [开始] Go 的语法兼容性如何？保证向后（下）兼容（ 旧版本 Go 写的程序，能用新版本的编译器和库 ）

1. [开始] Go 有什么缺点？[go-is-not-good](https://github.com/ksimka/go-is-not-good)

1. [开始] **有哪些优秀的 Go 学习资源可以参考？** [TGPL](https://book.douban.com/subject/27044219/)（ [源码](https://github.com/adonovan/gopl.io/) ），[Learning basic Golang in one day](https://github.com/go-training/training)，[Training Meterials](https://github.com/golang/go/wiki/Training)，[官方文档](https://golang.org/doc/#references)，[标准库中文版](https://books.studygolang.com/The-Golang-Standard-Library-by-Example/)

1. [开始] **优秀的三方类库**：[golang.org/x](https://godoc.org/-/subrepo)，[pkg.go.dev](https://pkg.go.dev/)，[gofunk](https://godoc.org/github.com/thoas/go-funk)

1. [开始] 有哪些框架是 Go 语言开发的？容器（ Docker / Kubernetes ），数据库（ TiDB / InfluxDB / ETCD ），消息系统（ NSQ ），缓存系统（ GroupCache ），Web框架（ Beego / Gorilla / Micro / Go-Micro ）

1. [环境] 如何搭建 VSCode 开发和调试环境？[Go in VSCode](https://code.visualstudio.com/docs/languages/go)，[Code Navigation](https://code.visualstudio.com/docs/editor/editingevolved)，[Debugging Go code using VS Code](https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code)

1. [环境] 如何用 Delve 进行调试？[Get Started](https://github.com/go-delve/delve/blob/master/Documentation/cli/getting_started.md)，[Delve Documentation](https://github.com/go-delve/delve/tree/master/Documentation)，[Using the Go Delve Debugger from the command line](https://www.jamessturtevant.com/posts/Using-the-Go-Delve-Debugger-from-the-command-line/)，[Debugging Go with VS Code and Delve](https://flaviocopes.com/go-debugging-vscode-delve/)

1. [环境] 如果搭建 Goland 开发和调试环境？[Debugging code](https://www.jetbrains.com/help/go/debugging-code.html)，[Debugging containerized Go applications](https://blog.jetbrains.com/go/2018/04/30/debugging-containerized-go-applications/)

1. [流程] **Go 语言中函数、变量、常量、类型、语句标签、包的命名规则是什么？**类 C、字母 => Unicode，Case

1. [流程] 如下预声明的常量、类型、函数是什么意思，[iota](https://golang.org/ref/spec#Iota) / [rune](https://golang.org/pkg/builtin/#rune) / [cap](https://golang.org/pkg/builtin/#cap) / [imag](https://golang.org/pkg/builtin/#imag)？iota 是希腊字母艾欧他，对应拉丁字母 i，表示下标，结合隐性重复最后一个非空的表达式的语法糖，用于递增场景（ 常量生成器 ）。rune 就是 int32，用于 unicode。

1. [流程] 变量的有效性如何判断？函数内、包内、流程语句，流程代码块内部

1. [流程] **包内有效变量的可导出性如何决定？**。首字母的大小写

1. [流程] 变量命名潜规则？优先短名，作用域越大命名越长且越有意义，驼峰规则，首字母缩写单词用相同大小写

1. [流程] **各类程序实体如何声明？var / const / type / func**

1. [流程] **短变量声明用于哪里？**函数内。函数内一般使用短变量声明（ Python 3.8 中引入的海象运算符 := ），var 一般用于包级别，或者函数内的零值情况。

1. [流程] 短声明最少声明一个新变量（ 不需要声明所有在左边的变量 ），否则要改成赋值等号

1. [流程] **数字 / 布尔 / 字符串 / 接口和引用类型（ slice / 指针 / map / 通道 / 函数 ）的零值（ 变量默认值 ）分别是？**

1. [流程] 变量初始化在何时发生？包级别在 main 函数开始前，局部变量在函数执行期间（ 声明时 ）

1. [流程] 多重赋值语法（ 类似 Python 的元组赋值 ）求公约数？`for y!=0 { x, y = y, x%y }`

1. [流程] 指针变量何时相等？指向同一变量或者都为 nil

1. [流程] **new(T) 实际做了什么事？**实例化一个 T 类型的变量，初始为 T 类型的零值，返回其地址（ *T 类型 ）。`return new(T)` == `{ var dummy T;  return & dummy }`

1. [流程] 每次 new 都会创建一个不同实例么？一般是，但有特例（ 变量类型不携带信息且为零值有相同地址，比如 struct{}，[0]int ），1.13 版本中，这个特例也改掉了（ `a, b := new(struct{}), new(struct{}); &a != &b` ）。

1. [流程] **包级别变量的生命周期是？**整个程序的执行时间。

1. [流程] **局部级别变量的生命周期是？**一直生存直至 GC。如果处于性能考虑，要减少局部变量逃逸，从而减少 GC（ 编译器会在栈上分配内存 ）。

1. [流程] **赋值操作符的写法，递增递减的写法**？+=，++ / -- 后置

1. [流程] **哪些内置函数和方法返回 err**？map 查询，类型断言，通道接收

1. [流程] **丢弃返回值如何操作**？_

1. [流程] 有哪些隐性赋值？实参到形参，返回值接收，复合类型的字面量表达式（ a := []strings{"A", "B"} ），map，通道

1. [流程] 什么是可赋值性？赋值只有在值对于变量类型是可赋值的时才合法。

1. [流程] **可赋值性规则有哪些**？。类型精确匹配，nil 可以被复制给任何接口变量或者引用类型，常量有更灵活的可赋值性规则以避免显式转换。

1. [流程] 相等性比较的条件？a == b / a != b 合法的前提时 b = a 合法（ 第一个操作数相对第二个类型是可赋值的 ）

1. [流程] **类型转换的使用场景？**双向，单向。具有相同的底层类型（ 不能用算数表达式进行比较和合并 ），或者指向相同底层类型变量的未命名指针类型，可以**双向**转换。string(65) == "A"，但 int("A") 不成立，这就是**单向**转换。如果 x 对于类型 T 是可赋值的，允许类型转换但无必要，因为会隐式转换。

1. [流程] 类型转换产生新的实例么？对于值类型，就是值复制，不存在实例对象的概念。对于 string => []byte 会分配副本，struct 也是类似，`type A struct { id int }; type B A; a := A{1}; b := B(a); &(a.id) != &(b.id)`，就是成员对象复制。

1. [流程] **可以对表达式返回值取地址么？**不可以

1. [流程] 浮点数转换成整数的逻辑是什么？丢失小数部分。

1. [流程] 类型转换可能在运行时失败么？不会，静态类型在编译时检查类型转换是否能正常完成。

1. [流程] **相当于 Python 中的 `__str__` 方法如何实现？**`func (a T) String() string { return fmt.Sprintf('<T: %g>', a) }`

1. [流程] **何时会调用到 String()？**%v，%s，fmt.Println。注意 %g 不会调到这个函数

1. [流程] **包的文档注释写在哪里？**开头一句话总结性描述 & 每个包只有一个文件需要描述 & 一般每个包有独立的 doc.go

1. [流程] **怎么使用包里的标识符？**包名.大写字母开头的标识符

1. [流程] **import 的语法？**单行 import "fmt" => 字符串，`import ( "fmt";"math")` 换行，每行一个，这个类似 const 单行和多行语法; 非标准库依赖 $GOPATH "绝对"路径或者 go mod 相对路径（ 默认短名是包名 ）

1. [流程] **类似 Python 中的 import as 的语法怎么写？**`import test "math"`

1. [流程] 什么是包初始化函数？func init() {}，包中变量会在编译时依据依赖关系自动确定先后初始化

1. [流程] **for 有几种形式？**3 种。Condition：`[Condition]`，ForClause：`[ InitStmt ] ";" [ Condition ] ";" [ PostStmt ]`，RangeClause：`[ ExpressionList "=" | IdentifierList ":=" ] "range" Expression`

1. [流程] **for range 的使用？**`for i := range new([5]byte) { fmt.Println(i) } `等同于` for i, _ := range new([5]byte) { fmt.Println(i) }[流程]`，单个 i 是 index，不是 item value。`for _, _ := range s` 等同于 `for range s`。

1. [流程] range后面支持的数据类型有哪些？共5个，分别是数组，数组指针，slice，字符串，map 和 channel

1. [流程] 纯概念，作用域（ 编译时属性，声明在程序文本中出现的区域 ），变量的生命周期（ 运行时概念 ），语法块（ 大括号里的语句序列 ），词法块（ 推广语法块到没有显式包含在大括号里的声明代码 ）。

1. [流程] **在一个作用域内，x := x + 1，这里的两个 x 是同一个变量么？**不是！

1. [流程] **怎么理解 for / if 的作用域？**分隐式块和显式块。隐式块包含：声明 + 条件 + 后置语句（ for 循环 ）+ 显式块

1. [流程] **怎么理解 if / else if / else？**单纯的嵌套（ 与之对应的，也是作用域嵌套 ）

1. [流程] 怎么理解 switch 的作用域？条件对应一个块，每个 case 语句体对应一个块。

1. [流程] if / for 和 err 怎么搭？判断条件中有 err，则常规代码写在 else 里。

1. [流程] init 和 err 怎么搭？在 init 函数中，var err 私有化，然后用多重赋值，而不是短声明

1. [IO] 标准输入、标准输出、标准错误？os.Stdout，os.Stdin，`for input.Scan { fmt.Println(input.Text()) }`，`in := bufio.NewReader(os.stdin)`，`in := bufio.NewScanner(os.stdin)`，`input.Split(bufio.ScanWords)`，`input.Scan()`

1. [基本] **Go 数据类型的分类？**4 大类。basic（ number 整数、浮点数、复数 / string / boolean ），aggregate（ array / struct ），reference（ pointer / slice / map / function / channel ），interface

1. [基本] **如何确认数据类型？**%T，reflect.TypeOf(42).String()

1. [基本] 复合类型数据有几种？哪几种长度固定？哪几种可以动态增长？长度固定：数组 & 结构体。动态数据结构：slice / map

1. [基本] make 怎么创建 slice / map / chan？slice：`make([]int, len, [cap]) == make([]int, cap)[:len]`，map：`make(map[string]int)`，chan：`make(chan string)`

1. [整数] 整数类型有几种？固定大小：uint8-32 / int8-32 / rune / byte，不固定：int / uint / uintptr（ 仅用于底层编程 ）

1. [整数] 如何实现乘方运算？math.Pow(float64, float64)

1. [整数] 三元运算符？没有，只能 `var a int; if true { a = 5 } else { a = 6 }`

1. [整数] 有哪些一元运算符，哪个是按位取反运算符？`+  /  -  ^ !`，一元 ^ 表示逐位取反，! 逻辑取反

1. [整数] 有哪些二元运算符？`*  /  %  <<  >>  &  &^`，`+  -  ｜ ^`，`==  !=  <  <=  >  >=`，`&&`，`||`。&& 表示逻辑乘，优先级高，|| 表示逻辑和，& 和 | 同理。

1. [整数] **如何将数字转换成字符串？**`fmt.Sprintf("%b %o %x %d", ...)`，`strconv.FormatInt(42, 17)`，`strconv. Itoa()`

1. [整数] **如何将字符串转换成数字？**[strconv](https://golang.org/pkg/strconv/)，`strconv.ParseInt("42", 8, 64)`，`strconv. Atoi()`，fmt.Scanf 不够灵活

1. [整数] 为什么数组长度不可能为负数，还是采用有符号整型？for 循环--避免溢出

1. [整数] 左移、右移的处理逻辑和注意点？x << n => x * 2^n，x >> n => x / 2^n 向下取整，n 必须无符号。有符号右移高位填充符号位，因此位模式处理右移需要无符号整型

1. [整数] 无符号整数用于什么场景？位运算 & 特定算术运算，一般不用于表示非负值

1. [整数] fmt.Printf 怎么用 index，# 是什么意思？`"%#[1]d"`

1. [整数] **单引号括起来的单个文本符号表示什么？**字符。rune / byte，`%d %c %q`

1. [整数] **ASCII 和字符转换？**`str(62)`，`%c %d`，`[]rune("hello"), []byte("hello")`。

1. [浮点] Go 支持支持几种浮点数类型？算术特性遵从什么标准？float32 / float64，IEEE 754 ( `a=0.1; a+0.2 != 0.3` )，注，字面常量这里相等的 `0.1+0.2==0.3`，应该是编译器直接优化了。

1. [浮点] float32 和 float64 的精准度如何？32 有效数字大约 6 位，64 大约 15 位。绝大多数情况用 64，避免运算误差迅速累计放大。

1. [浮点] **%g, %f, %e 有和区别？**%g 自动保持精度+简洁，%f 无指数，%e 有指数。都可以通过谓词 %8.3f 掌握宽度和精度。

1. [浮点] 如何表示最大的 64 位浮点数？正负无穷大（ 超出许可值，非零除以零 ）？数学上无意义的运算结果（ 0/0，sqrt(-1) ）？math.MaxFloat64，math.Inf(64)，-math.Inf(64)，math.NaN()

1. [浮点] 如何将 NaN 作为信号值进行判断？math.IsNaN。NaN 的比较总为 false，但 != 除外，因为它与 == 的值总是反的

1. [浮点] 如果一个函数的返回值是浮点型且有可能出错，怎么处理？单独报错。return ret, err

1. [复数] 复数的类型？complex64 / complex128，二者分别由 float32 和 float64 构成。real(complex128)，imag(complex128) 取实部和虚部

1. [布尔] **布尔值和数值的转换？**无法隐式转换。`i := 0; if b { i = 1 }`，`b = i!=0`

1. [布尔] 布尔值如何转换成字符串？%t

1. [字符] **字符串转义？**`\a \b \f \n \r \t \v \' \" \\`

1. [字符] **原始字符串怎么写，用于何处？**反引号，可以换行。用于正则表达式，路径，HTML/JSON 模版，多行的命令行提示信息。

1. [字符] 长行字符串如何表示？a := "xxx" +，换行，tab，"yyy" +，换行，tab，"zzz"

1. [字符] **字符串遍历？**`for _, i := range "hello" {}`，这里 %T 看，会发现 i 是 int32，**按 UTF-8 隐式解码**。%x 可以看到是 3 个字节。

1. [字符] **字符串与字节 slice 如何互换？**[]byte(s)，分配内存，拷贝填入 s 含有的字节（ 这一步可能会被编译器优化掉 ），并生成一个 slice 引用，指向整个数组。

1. [字符] **如何令字符串索引能支持 Unicode？**[]rune 转换成字符串？`[]rune("he我llo")[2]`，`string([]rune("hello"))`

1. [字符] Unicode 和 UTF-8 什么关系？UTF-8 以字节为单位对 Unicode codepoint 做变长编码，由 Go 语言创建者 Ken Thompson & Rob Pike 发明。Unicode 定长编码是 UTF-32，也叫 UCS-4。`'世' == '\u4e16' == '\U00004e16'`。UTF-8 本身不会嵌入 NUL 0 值，便于某些程序语言用 NUL 标记字符串结尾。

1. [字符] **字符串怎么存储的？可变么？**不可变字节序列，文本字符串被解读成 **UTF-8 编码的 Unicode 码点序列**。类似这样存储：`struct { data pointer; len int }`

1. [字符] **根据字符串索引对应的字符？**s[i] => 字符。`s := "hello"; s[1]` 是 byte 还是 rune，这里 %T 看，会发现是 int8，即使里面有中文。`len("我") == 3`。

1. [字符] **字符串 slice 运算符的操作逻辑？**s[i:j] / s[:j] / s[:i] / s[:]，子串生成操作，左闭右开，产生新的字符串对象，但底层不重新分配内存，因为字符串不可变，所以可以安全地共用底层。[import "index/suffixarray"](https://golang.org/pkg/index/suffixarray/)

1. [字符] **怎么理解字符串加法 +=**，比如 `s += "hello"`？产生一个新的字符串实例并赋值给 s。底层应该也不是直接加而是分配全新的内存空间，因为对同一个底层，多个字符串都可以 += 不同的新串。

1. [字符] Golang 对 UTF-8 的支持情况如何？Go 的源文件**总是**以 UTF-8 编码，Go 操作文本字符串**优先**用 UTF-8 编码。

1. [字符] **怎么对 UTF-8 进行编解码？**unicode/utf8，`for _, i := range "1世a" { buf := make([]byte, 3); n := utf8.EncodeRune(buf, i); fmt.Println(buf[:n])}`，`DecodeRuneInString(s[i:])`，或者用：`golang.org/x/text/encoding/simplifiedchinese`、`golang.org/x/text/transform`，`reader := transform.NewReader(bytes.NewReader(s), simplifiedchinese.GBK.NewDecoder()); d, e := ioutil.ReadAll(reader)`，`reader := transform.NewReader(bytes.NewReader(s), simplifiedchinese.GBK.NewEncoder()); d, e := ioutil.ReadAll(reader)`

1. [字符] **怎么实现序列反转？**[funk.Reverse([]int{0, 1, 2, 3, 4})](https://github.com/thoas/go-funk)，[源码：transform.go](https://github.com/thoas/go-funk/blob/master/transform.go#L279)，`for i,j:=0,len(s)-1; i<j; i,j:=i+1,j-1 { s[i],s[j] = s[j],s[i] }`

1. [字符] **怎么区分单个文字符号是字母还是数字？怎么转换大小写？**`unicode.IsDigit(i)`, `unicode.IsUpper(i) || unicode.IsLower(i)`，`unicode.ToUpper(i)`，`unicode.ToLower(i)`

1. [字符] **有哪些字符串处理库函数？**strings.HasPrefix `len(s) >= len(prefix) && s[0:len(prefix)] == prefix`，Contains, ToUpper, Index, Field, Join，与 bytes ( 处理 slice ) 里的对应。

1. [字符] **如何获取字符串长度？**字节长度 len(s)，字符长度 utf8.RuneCountInString(s)

1. [字符] 处理 URL 和文件路径？path，path/filepath

1. [字符] **如何高速处理字符串？**bytes.Buffer / WriteRune / fmt.Fprint

1. [常量] **常量的基本语法？**const pi [float64] = 3.1415926，或者加括号，多行表示，这里可以结合 iota 完成递增逻辑。如果没有赋值，会复用前一行的表达式及其类型。

1. [常量] 常量的类型？常量本质上都属于基本类型（ 包括 type 具名的基本类型 ）：布尔 / 字符串 / 数字

1. [常量] 常量计算在编译时还是运行时完成？不确定，但对于常量操作数，所有数学、逻辑、比较运算的结果还是常量。常量的类型转换结果和一些内置函数的返回值，例如 len / cap / real / imag / complex / unsafe.Sizeof 同样是常量。

1. [常量] 常量表达式用于何处？数组类型声明长度时。

1. [常量] **什么是无类型常量？为什么需要无类型常量？**math.Pi 或者其它字面值 bool / int / char / float / complex / string。如果 math.Pi 一开始就属于 float64，则 math.Pi 相关的常量运算精度会下降。因此，在编译阶段，编译器会保持高精度，可以认为精度至少达到了是 256 位，因此 0.1+0.2 == 0.3。

1. [数组] 数组一般用于什么场景？长度固定（ 不需要增加 ）的场景，比如 Sum256 等。很少用到。

1. [数组] **怎么定义和初始化数组？**var a [3]int，可以用数组字面量来初始化数组 var q [3]int = [3]int{1, 2, 3}，或者 q := [...]int{1, 2, 3}，... 只能用于数组字面量，不可以 var [...]int = [...]int{1, 2}，数组长度在编译时确定。**长度也是数组类型的一部分**，不可以 var [3]int = [2]int{1,2}。数组长度必须是常量表达式。

1. [数组] **数组初始化完成后还可以批量重新赋值么？**可以。`a := [...]int{1, 2, 3, 4}; a = [...]int{5, 2, 3, 4}`

1. [数组] **怎么遍历数组？**

1. [数组] **对数组字面量，如何做部分赋值（ 剩余为 0 值 ）？**r := [...]int{99:-1}，len(r) == 100

1. [数组] 如何比较两个数组？精确比较 == / !=，摘要比较 crypto/sha256，sha256.Sum256([]byte("x"))

1. [数组] **数组作为形参是值传递还是引用传递？**值传递。如果希望引用传递，可以用数组指针 ptr *[32]byte

1. [切片] **Slice 对象有哪些属性？存储模型是怎样的？**指针（ 指向底层数组的第一个可以被 slice 访问的元素 ）；长度（ slice 元素个数，小于容量）；容量（ slice 起始元素到底层数组的最后一个元素 ）。

1. [切片] Slice 操作符 s[i:j] 会创建的 slice 么？会分配内存创建不同的底层数组么？`0<=i<=j<=cap(s)`，会创建新的 Slice，一般会共用底层数组。字符串切片返回字符串，slice 切片返回 slice，array 切片返回 slice。

1. [切片] 如果 slice 的引用超过了 cap 会如何？超过了 len 呢？超过 cap 会宕机，超过 len 但不超过 cap，正常返回。

1. [切片] 怎么比较 []byte？slice 不能直接比较（ 唯一合法是和 nil 比较 ），可以用 bytes.Equal，或者string([]bytes)进行比较

1. [切片] 为什么 slice 不能直接判断相等？1. slice 可能会包含自身 2. slice 底层数组可能随时改变（ map 的键在散列表的生命周期中必须保持不变，所以 slice 不可以用作 map 的键 ）

1. [切片] **怎么判断一个 slice 是否为空？**长度为 0 的 slice 和值为 nil 的 slice，len(s) 都 == 0？接口对外应该不区分二者，呈相同处理方式。nil：`var s []int`，`s = nil`，`s = []int(nil)`；非 nil 但 len = 0：`s = []int{}，s = make([]int, 3)[3:]`。

1. [切片] **append 的用法和底层实现是怎样的？**slice = append(slice, item)，需要扩容时会生成新的底层数组（ 一般直接扩一倍 ）。只要可能改变 slice 的长度、容量，或者指向不同地层数组，都需要更新 slice 变量，重新赋值回去。

1. [切片] **slice copy 的用法和底层实现是怎样的？**n = copy(dst, src)，不会产生一个新的底层数组，src 和 dst 可能重合（ 因此 copy 函数执行后，src 可能变化 ）。n = min(dst, src)

1. [切片] 什么场景下会重用 slice 底层数组？过滤部分元素。

1. [切片] **如何用 slice 实现一个栈？**`s = append(s, v)` 作为 push，`s = s[:len(s)-1]` 作为 pop。

1. [切片] 怎么实现 remove 一个 slice 元素？`copy(s[i:], s[i+1:]); return s[:len(s)-1]`，若不需要维持顺序，可以 `s[i] = s[len(s)-1]; return s[:len(s)-1]`

1. [切片] 是否可以获取 array / slice 元素的地址？可以

1. [切片] 如何给字符串 slice 排序？sort.Strings(s)

1. [字典] **Map 对象有哪些属性？底层实现是怎样的？**map[K]V，是无序集合，键值必须唯一（ 必须可以比较 ）。K 必须类型相同，V 也必须类型相同。浮点型可以比较，但请不要，没有较真过能不能作为键值。

1. [字典] Map 怎么比较是否相等？自己写循环。和 slice 一样，map 不能直接比较，唯一合法的比较是和 nil 比较。

1. [字典] **Map 对象怎么声明和初始化？**`ages := make(map[string]int)` == `map[string]int{}`，也可以直接通过 map 字面量进行初始化 `ages = map[string]int{ "alice":31, "charlie":14}`

1. [字典] **Map 怎么增加元素，删除元素？**`ages["green"] = 15`，`delete(ages, "green")`

1. [字典] Map 查找元素时，找不到键值如何返回？返回零值。类似 DefaultDict，因此可以直接 `ages["bob"]++`

1. [字典] 是否可以获取 map 元素的地址？不是变量，不可以获取地址。&ages["bob"] 编译失败。不可以获取地址的原因是随着元素增加，已有元素可能会被散列到新的存储位置，这样可能使获取的地址无效。slice 也可能底层数据扩展，但原来的地址总还是在的，不会无效。

1. [字典] **如何遍历一个 map ？**`for name := range(ages)`；`for name, age := range(ages)`，迭代顺序不固定。

1. [字典] map 的零值 nil 有什么注意事项？不可以为零值中设置元素，必须先初始化。`var ages map[string]int; ages = map[string]int{}; ages["hello"] = 1`

1. [字典] **当一个键不存在的时候，map 取索引会返回零值，如何能知道键值是否存在？**`if ages,ok:=ages["bob"];!ok { not exist... }`

1. [字典] 怎么实现集合？只用 map 的键。但没有集合运算库函数。

1. [字典] 当希望键是 slice 时，怎么办？添加 hash 核函数 k，对 s1 == s2 时，k(s1) 才会 == k(s2)

1. [结构] **结构体的定义和使用？**`type xx struct { ID int }; var y xx; fmt.Println(y.ID)`

1. [结构] **结构体指针的用法？**字段的指针：`&y.ID`，结构体指针：`y.ID == (&y).ID`，有一个诡异的语法，`func test() astruct {}` 返回结构体，可以 print test().ID，但不能对字段直接赋值 test().ID = 4，可是如果 func 返回的是 *astruct，就可以这么写。

1. [结构] **结构体字面量赋值？**x := Point{1,2}，**当 2 和 } 不在一行时，2 后面必须加上逗号**。

1. [结构] **如何实现递归结构体，比如二叉树？**聚合类型不可以包含自己，但能定义一个同类型的指针成员变量。`type tree struct { value int; left,right *tree}`

1. [结构] 生成二叉树？二叉树遍历？递归。

1. [结构] 空结构体 struct{}{} 有什么用？没有长度，不携带任何信息。尽量不用。

1. [结构] 成员变量的顺序对结构体来说有意义么？有，顺序不同，就是不同的结构体。

1. [结构] **结构变量的初始化方法？**序列初始化，（部分）字典初始化。二者不可混用，并且二者的字段命名都要符合包导出规则。`x := p.T{A:1, C:3}`，`x := p.T{1, 3}`，跨包用时就不能用小写

1. [结构] 结构体在作为函数的参数和返回值时，是值传递还是引用？值传递。

1. [结构] **常用的创建一个 struct 并获取其地址的语法是？**pp := &Point(1,2)，不用 pp := new(Point); *pp = Point(1,2)

1. [结构] 结构体是否可以比较？可以用 == / !=，可以用作 map 的 key

1. [结构] **什么是结构体嵌套？有什么用？**便于直接取得成员的成员的值，类似继承语法（ 只要成员的成员开头大写，不管成员如何，快捷方式写法可以导出。w.circle.X 不可以导出，但 w.X 可以导出 ）。

1. [结构] **什么是匿名成员？**成员没有名字，只有类型

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

### 进阶

1. [方法] 

1. [方法] 

1. [方法] 

1. [方法] 

1. [方法] 

1. [方法] 
