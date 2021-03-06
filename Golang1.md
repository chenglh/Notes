#### 第三天课程

##### 3.1 复习

**运算符**

~~~go
算术运算符：+、-、*、/

逻辑运算符：&& || !

位运算符：>> << | ^ &

赋值运算符： = 、+= 、-= 、 ++ 、--

比较运算符： <、<=、!=、>、>=
~~~

**数组**

~~~go
var ages  [30]int	//元素的个数、类型
var names [30]string

var aa = []int{1,13,50}
bb := aa
fmt.Printf("aa:%T\tbb:%T\n", aa, bb)  //aa:[]int        bb:[]int
fmt.Printf("aa:%p\tbb:%p\n", aa, bb)  //aa:0xc00009e140 bb:0xc00009e140

bb[1] = 2
fmt.Println(aa,bb)  //[1 2 50] [1 2 50]  数组赋值是地址引用
~~~



数组是值类型

~~~go
func main() {
	x := [3]int{1, 2, 3}
	y := x //把x的值拷贝了一份给y
	y[1] = 200 //修改了的是副本y，并不影响x

	fmt.Println(x)
	f1(x)
	fmt.Println(x)
}

func f1(a [3]int)  {
	a[1] = 100
}

func main() {
	var a1 = [...][2]int{ //多维数组只有最外层可以使用...
		[2]int{1,2},
		[2]int{3,4},
		[2]int{5,6},
	}
	fmt.Println(a1)
}
~~~





**切片**

~~~go
//切片(Slice) 方法1：声明+初始化
var s1 []int  //没有分配内存，== nil
fmt.Println(s1) 	 // []
fmt.Println(s1==nil) //true
s1 = []int{1, 2, 3}

//方法2：make初始化，分配内存
s2 := make([]bool, 2, 4) //类型  长度  容量
fmt.Println(s2)		//[false false]
fmt.Println(s2==nil) //false 已经分配内存有内存地址 != nil
~~~

切片本质

切片：指针、长度、容量

<img src="./Golang.assets/image-20200718115108169.png" alt="image-20200718115108169" style="float:left;" />

切片的拷贝

~~~go
s1 := []int{1, 2, 3}
s2 := s1
fmt.Println(s2) // [1 2 3]
s2[1] = 200
fmt.Println(s2) // [1 200 3]
fmt.Println(s1) // [1 200 3]
~~~

切片不存值，相当于一个框，去底层数组去框值

//切片只是切了一部分数据，但还是同一个内存地址；切片不存值，指向同一个底层数组

 ~~~

 ~~~

切片的扩容策略：

1、如果申请的容量大于原来的2倍，那就直接扩容到新申请的容量

2、如果小于1024，那么直接两倍

3、如果大于1024，就按照1.25倍去扩容

4、具体存储的值类型不同，扩容策略也有差别

~~~go
var s1 []int  // nill
s1 = append(s1, 1) //append函数追加，会自动初始化切片。
fmt.Println(s) // [1]
~~~

copy()

~~~go
s1 := []int{1, 2, 3}
s2 := s1  //赋值
var s3 []int  //nil 所以打印出来的是 []
// var s3 = make([]int, 3)
copy(s3, s1)  //拷贝，注意copy不会自动扩容，make时需要设置长度
fmt.Println(s2) // [1 2 3]
s2[1] = 200
fmt.Println(s2) // [1 200 3]
fmt.Println(s1) // [1 200 3]
fmt.Println(s3) // []
~~~



**指针**

& 和 *

~~~go
addr := "广东"
addrP := &addr
fmt.Println(addrP
fmt.Println(*addrP)
~~~

**map**

map存储的是键值对

~~~go
var m1 map[string]int
fmt.Println(m1 == nil)  //true

//声明变量且申请内存地址
var m1 = make(map[string]int, 10) //算一下存的长度
m1["chenglh"] = 100  //如果key不存在，返回的是对应类型的零值
fmt.Println(m1)

v,ok := m1["chenglh"]
if ok {
    //
} else {
    //没有对应key
}

delete(m1, "chenglh") //如果key不存在，什么都不干
~~~

##### 3.2 作业

~~~go
//1、判断字符串中汉字的数量
//统计汉字字数
s1 := "Hello沙河有沙有河"
var count int
for _, v := range s1{
    if unicode.Is(unicode.Han, v) {
        count++
    }
}
fmt.Println(count)


//2、统计英文单词出现的次数
s  := "how do you do"
m1 := make(map[string]int, 10)
s2 := strings.Split(s, " ")
for _,w := range s2 {
    if _,ok := m1[w]; ok {
        m1[w]++
    } else {
        m1[w] = 1
    }
}
for key, value := range m1 {
    fmt.Println(key,value)
}

//3、回文判断
//上海自来水来自海上
//山西落雁塔雁落西山
//黄山落叶松叶落山黄
ss := "a山西落雁塔雁落西山a"
//把字符串中的字符放到一个[]rune切片中
r := make([]rune, 0, len(ss)) //容量长一点没关系
for _,c := range ss{
    r = append(r, c)
}
fmt.Println("[]rune", r) //转成ASCII码了
for i := 0; i < len(r)/2; i++ {
    // ss[i]  ss[len(ss)-1-i]
    if r[i] != r[len(r)-1-i] {
        fmt.Println("不是回文 ")
        return
    }
}
fmt.Println("是回文")
~~~





#### 第四天课程

##### 4.1 复习

**函数**

函数的定义

~~~go
func 函数名(参数1,参数2...) 返回值 {
   //函数体
}
~~~



**函数进阶**

> 高阶函数(函数可以作为参数，也可以作为返回值)

~~~go
func main() {
    f1(f2, "chenglh")
    ret := zl() //返回的是一个函数体
    sum := ret(10, 20) //调用函数体
}

func f1(f func(string), name string) {
    f(name)
}

func f2(name string) {
    fmt.Println("Hello，",name)
}

func zl() func(int, int) int {
    return func (x,y int) int{
        return x+y
    }//返回的是一个函数类型
}
~~~



> 闭包

定义：函数和其外部变量的引用

~~~go
func ys(name string){
    fmt.Println("Hello，",name)
}
func low(f func()) { //类型不匹配，ys()函数传不进去
    f()
}

//闭包
func bibao(f func(string),name string) func() {
    return func() {
        f(name)
    }
}

func main() {
    fc := bibao(yx, "myname")
    low(fc)
}
~~~



> defer：延迟调用，多用于处理资源释放

> 内置函数

panic 和 recover

~~~go
func f1() {
    defer func() {
        err := recover()//收集当前的错误
        fmt.Println("松手去爱...")
        fmt.Println(err)
    }()
	panic("犯错") //程序崩溃了，后面不打印了
    fmt.Println("f1")
}

func f2() {
    fmt.Println("f2")
}

func main() {
    f1()
    f2()
}
~~~



##### 4.2 作业



##### 4.3 struct

Go语言中没有“类”的概念，也不支持“类”的继承等面向对象的概念。
Go语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性。
结构体是**值类型**，赋值的时候都是拷贝。



###### 4.3.1 自定义类型

~~~go
//将myInt定义为int类型
type myInt int
~~~



###### 4.3.2 类型别名

 类型别名是在 go1.9以后版本新增

~~~go
//定义格式
type TypeAlias = Type
~~~



如系统已定义的别名

~~~go
type byte = uint8
type rune = int32
~~~



###### 4.3.3 类型定义和别名区别

~~~go
//自定义类型
type newInt int

//类型别名
type myInt = int

func main()  {
	var a newInt = 10
	var b myInt  = 20

	fmt.Printf("a的值：%v 对应类型：%T\n", a, a) //a的值：10 对应类型：main.newInt
	fmt.Printf("b的值：%v 对应类型：%T\n", b, b) //b的值：20 对应类型：int
}

//a的类型是main.newInt，表示main包下定义的newInt类型
//b的类型是int
/*myInt类型只会在代码中存在，编译完成时并不会有myInt类型*/
~~~



###### 4.3.4 结构体定义

> 结构体也是一种类型，可以通过 type 和 struct 来定义结构体

~~~go
type 类型名 struct {
	字段名 字段类型
	字段名 字段类型
	…
}
~~~



> 正常定义

~~~go
type person struct {
	name string
	city string
	age  int8
}
~~~



> 简写写法

~~~go
type person1 struct {
	name,city string //相同类型写在一行
	age int8
}
~~~



###### 4.3.5 结构体实例化

只有当结构体实例化时，才会真正地分配内存。

~~~go
var 实例名称  结构体类型
~~~



定义人的结构体：

~~~go
type Person struct {
	Name string
	Age  int8
	Sex  string
}
~~~



> 实例化方法一：var 变量名 结构体名

~~~go
func main() {
    //var 变量名 类型
	var p1 Person
	p1.Name = "chenglh"
	p1.Age  = 20
	p1.Sex  = "男"
	fmt.Printf("p1的值：%v p1详：%#v p1类型：%T\n", p1, p1, p1)
	//p1的值：{chenglh 20 男} p1详：main.Person{Name:"chenglh", Age:20, Sex:"男"} p1类型：main.Person
}
~~~



> 实例化方法二：new实例化，得到指针地址

~~~go
func main() {
    //创建指针类型结构体
	var p2  = new(Person)
	p2.Name = "fanlp" //等同于 (*p2).Name = "fanlp" //语法糖。
	p2.Age  = 20
	p2.Sex  = "女"
	fmt.Printf("p2的值：%v p2详：%#v p2类型：%T\n", p2, p2, p2)
	//p2的值：&{fanlp 20 女} p2详：&main.Person{Name:"fanlp", Age:20, Sex:"女"} p2类型：*main.Person
}
~~~



> 实例化方法三：&结构体，得到地地址

~~~go
func main() {
    //取结构体的地址实例化
	var p3  = &Person{}//其成员变量都是对应其类型的零值。
	p3.Name = "chenghl"
	p3.Sex  = "男"
	p3.Age  = 20
	fmt.Printf("p3的值：%v p3详：%#v p3类型：%T\n", p3, p3, p3)
	//p3的值：&{chenghl 2 男} p3详：&main.Person{Name:"chenghl", Age:20, Sex:"男"} p3类型：*main.Person
}
~~~



> 实例化方法四：使用键值对的方式来实例化结构体

~~~go
func main() {
    //键值对初始化
	var p4 = Person{
		Name: "zhangsan",
		Sex	: "man",
		Age	: 22,
	}
	fmt.Printf("p4的值：%v p4详：%#v p4类型：%T\n", p4, p4, p4)
	//p4的值：{zhangsan 22 man} p4详：main.Person{Name:"zhangsan", Age:22, Sex:"man"} p4类型：main.Person
}
~~~



> 实例化方法五：

~~~go
func main(） {
	var p5 = &Person{
		Name: "zhangsan",
		Sex	: "man",
		Age	: 22,
	}
	fmt.Printf("p5的值：%v p5详：%#v p5类型：%T\n", p5, p5, p5)
	//p5的值：&{zhangsan 22 man} p5详：&main.Person{Name:"zhangsan", Age:22, Sex:"man"} p5类型：*main.Person
}
~~~



> 实例化方法六：

~~~go
func main() {
	//使用值的列表初始化
	var p6 = Person{
		"李子",
		20,
		"女",
	}
	fmt.Printf("p6的值：%v p6详：%#v p6类型：%T\n", p6, p6, p6)
	//p6的值：{李子 20 女} p6详：main.Person{Name:"李子", Age:20, Sex:"女"} p6类型：main.Person
}
~~~

**总结归纳为两种方法：1、创建指针类型结构体，2、取结构体的地址实例化**



###### 4.3.6 匿名结构体

在定义一些临时数据结构等场景下还可以使用匿名结构体。

~~~go
func main() {
    var user struct{Name string; Age int} //临时使用的结构体
    user.Name = "chenglh"
    user.Age = 18
    fmt.Printf("%#v\n", user)
}
~~~



###### 4.3.7 结构体内存布局

**结构体占用一块连续的内存**

~~~go
type test struct {
	a int8  //8bit -> 1byte
	b int8
	c int8
	d int8
	//e string  会出现内存对齐(内存高级管理)
}

func main()  {
	n := test {
		1, 2, 3, 4,
	}

	fmt.Printf("n.a %p\n", &(n.a))
	fmt.Printf("n.b %p\n", &(n.b))
	fmt.Printf("n.c %p\n", &(n.c))
	fmt.Printf("n.d %p\n", &(n.d))
}
~~~

输出结果：[连续的内存地址]

~~~go
n.a 0xc00000a0d8
n.b 0xc00000a0d9
n.c 0xc00000a0da
n.d 0xc00000a0db
~~~



###### 4.3.8 空结构体

空结构体是不占用空间的。

~~~go
var v struct{}
fmt.Println(unsafe.Sizeof(v))  // 0
~~~



**面试题**

~~~go
func main() {
	m := make(map[string]*student)
	stus := []student{
		{name: "小王子", age: 18},
		{name: "娜扎", age: 23},
		{name: "大王八", age: 9000},
	}

	for _, stu := range stus {
		m[stu.name] = &stu
	}
	for k, v := range m {
		fmt.Println(k, "=>", v.name)
	}
}

//https://studygolang.com/articles/9701
~~~



###### 4.3.9 构造函数

自定义实现构造函数，如果结构体比较复杂的话，值拷贝性能开销会比较大，所以该构造函数返回的是结构体指针类型。

```go
func newPerson(name, city string, age int8) *person {
	return &person{
		name: name,
		city: city,
		age:  age,
	}
}
```



**调用构造函数**

~~~go
p := newPerson("张三", "广州", 20)
fmt.Printf("%#v\n", p) //&main.person{name:"张三", city:"广州", age:20}
~~~



###### 4.3.10 结构体是值类型

如果判断结构体是值类型？

~~~go
type Student struct {
	Name string
	Age  int
}

/**
 * 值类型  : 改变变量副本的时候，不会改变变量本身的值(数组，基本数据类型，结构体)
 * 引用类型: 改变变量副本的时候，会改变变量本身的值(切片，map)
 */
func main() {
	var student = Student{
		Name: "chenglh",
		Age: 20,
	}
	fmt.Println(student) //{chenglh 20}

	s1 := student
	s1.Age = 25
	fmt.Printf("student：%#v, s1：%#v\n", student, s1)
    //student：main.Student{Name:"chenglh", Age:20}, s1：main.Student{Name:"chenglh", Age:25}
}
~~~

以上测试到的两个值是不同的内存地址，是值类型，但是如果实例化的是指针类型，s1 = student 则是指向同一内存地址。



###### 4.3.11 方法与接收者

**给类型（结构体，自定义类型）定义方法。**

~~~go
func (接收者 接收者类型) 方法名(参数列表)(返回参数) {
	//方法体
}
~~~

接收者：一般写法是使用接收者类型的首字母

接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。

- 非指针类型：表示**不修改**结构体的内容
- 指针类型   ：表示**修改**结构体中的内容

举个栗子：

```go
//Person 结构体
type Person struct {
	name string
	age  int8
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
}
```

方法与函数的区别是，**函数不属于任何类型，方法属于特定的类型**。



> 指针类型接收者

~~~go
// SetAge 设置p的年龄
// 使用指针接收者
func (p *Person) SetAge (newAge int8) {
	p.age = newAge
}
~~~

调用该方法：

~~~go
func main() {
	p1 := NewPerson("小王子", 25)
	fmt.Println(p1.age) // 25
	p1.SetAge(30)
	fmt.Println(p1.age) // 30
}
~~~



> 值类型接收者

当方法作用于值类型接收者时，Go语言会在代码运行时**将接收者的值复制一份**。在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是**针对副本**，**无法修改接收者变量本身**。

~~~go
// SetAge2 设置p的年龄
// 使用值接收者
func (p Person) SetAge2(newAge int8) {
	p.age = newAge
}

func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
	fmt.Println(p1.age) // 25
	p1.SetAge2(30) // (*p1).SetAge2(30)，只修改副本，修改不成功
	fmt.Println(p1.age) // 25
}
~~~



**什么时候使用指针类型接收者**

1. 需要修改接收者中的值
2. 接收者是拷贝代价比较大的大对象
3. 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。



例子一：

~~~go
//方法与接收者

type Student struct {
		Name string
		Sex  string
		Age  int8
}

//打印学生基本信息
func (s Student) getInfo() {
		fmt.Println("姓名：",s.Name)
		fmt.Println("性别：",s.Sex)
		fmt.Println("年龄：",s.Age)
}

//修改学生基本信息
func (s *Student) setInfo(name string, sex string, age int8) {
		s.Name = name
		s.Sex  = sex
		s.Age  = age
}

func main()  {
		var student Student
		student.Name = "张三"
		student.Age  = 22
		student.Sex  = "男"
		student.getInfo()

		student.setInfo("chenglh", "man", 20)
		student.getInfo()
}
~~~

**注意，因为结构体是值类型，所以我们修改的时候，需要传入的指针**



例子二：

~~~go
//构造函数实例化

type Person struct {
		Name string
		Sex  string
		Age  int
}

/**
 * 构造函数：约定成俗用new开头
 * 如果字段少时可以返回结构体对象
 * 但是如果字段多的时侯，返回结构体指针
 */
//func newPerson(name string, sex string, age int) Person {
//	return Person{
//		Name: name,
//		Sex: sex,
//		Age: age,
//	}
//}

//当结构体比较大的时候尽量使用结构体指针，减少程序的开销
func newPerson(name string, sex string, age int) *Person {
		return &Person{
				Name: name,
				Sex: sex,
				Age: age,
		}
}

func main()  {
		var p = newPerson("程辉", "男", 25)
		fmt.Println(p)
}
~~~



> 什么时候使用指针接收者

1. 需要修改接收者中的值
2. 接收者是拷贝代价比较大的大对象
3. 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。









###### 4.3.12 结构体匿名字段

结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为**匿名字段**。

~~~go
type Person struct {
	string
	int
}
func main()  {
	var p1 = Person {
		"chenglh",
		14,
	}
	fmt.Printf("%#v\n", p1) //main.Person{string:"chengl", int:14}
	fmt.Println("姓名：",p1.string)
	fmt.Println("年龄：",p1.int)
}
~~~

**注意：**这里匿名字段的说法并不代表没有字段名，而是**默认会采用类型名作为字段名**，结构体要求字段名称必须**唯一**，因此一个结构体中同种类型的匿名字段只能有一个。



###### 4.3.13 嵌套结构体

结构体的字段类型可以是：**基本数据类型，也可以是切片、Map 以及结构体**等。

~~~go
//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//Student 用户结构体
type Student struct {
	Name    string
	Gender  string
	Address Address //嵌套地址结构体
}

func main()  {
	var student = Student{
		Name: "chenglh",
		Gender: "男",
		Addr: Address{
			Province: "广东省",
			City	: "广州市",
			Dist	: "天河区",
		},
	}
	fmt.Printf("%#v\n",student)
	fmt.Println("姓名：",student.Name) //通过属性去读取值
	fmt.Println("地址：", student.Addr.Province,student.Addr.City,student.Addr.Dist)
	/**
	main.Student{Name:"chenglh",Gender:"男",Addr:main.Address{Province:"广东省",City:"广州市",Dist:"天河区"}}
	姓名： chenglh
	地址： 广东省 广州市 天河区
	*/
}
~~~



> 嵌套匿名结字段

上面Student结构体中嵌套的`Address`结构体也可以采用匿名字段的方式，例如：

~~~go
//嵌套匿名字段
type Address struct {
	Province string
	City	 string
}

type User struct {
	Name 	string
	Gender 	string
	Address	//匿名字段
}

func main()  {
	var user User
	user.Name 	= "平平"
	user.Gender = "女"
    //匿名字段可以两种方式”赋值“
	user.Address.Province = "广东"
	user.City = "广州" //当访问结构体成员时会先在结构体中查找该字段，找不到再去嵌套的匿名字段中查找。

	fmt.Printf("%#v\n",user)
	//main.User{Name:"平平", Gender:"女", Address:main.Address{Province:"广东", City:"广州"}}
}
~~~



> 嵌套结构体的匿名字段名冲突

1、主结构体与匿名结构体的冲突

~~~go
type Address struct {
	Province string
	City	 string
}

type User struct {
	Name 	string
	Gender 	string
	City	string //与匿名地址结构体中冲突
	Address	//匿名字段
}

func main()  {
	var user User
	user.Name 	= "平平"
	user.Gender = "女"
	user.Address.Province = "广东"
	user.City = "广州"  //优先是赋值给主结构体，匿名结构体默认零值，一级级往上找属性名。
	//user.Address.City = "xxx"

	fmt.Printf("%#v\n",user)
    //main.User{Name:"平平", Gender:"女", City:"广州", Address:main.Address{Province:"广东", City:""}}
}
~~~

2、匿名结构体与匿名结构体冲突

~~~go
type Address struct {
	Province string
	City	 string
}

type Live struct {
	Province string
	City	 string
}
type User struct {
	Name 	string
	Gender 	string
	Address	//匿名字段
	Live	//匿名字段
}

func main()  {
	var user User
	user.Name 	= "平平"
	user.Gender = "女"
	user.Address.Province = "广东"
	//user.City = "广州" //这里会飘红线，编译出错
	user.Address.City = "广州" //需要指定是哪个结构体的字段

	fmt.Printf("%#v\n",user)
}
~~~



**当访问结构体成员时会先在结构体中查找该字段，找不到再去嵌套的匿名字段中查找。嵌套的结构体从上到下查找的优先级**

但是在实际项目中最好不要这么使用，增加代码阅读复杂度。

嵌套结构体内部可能存在相同的字段名。在这种情况下为了避免歧义需要通过指定具体的内嵌结构体字段名。

~~~go
user.City			//最外层结构体
user.Address.City	//Address结构体
user.Live.City		//Live 结构体
~~~



###### 4.3.14 结构体继承

一个结构体继承另一个结构体。

~~~go
//结构体的继承

//Animal结构体
type Animal struct {
	Name string
}
func (a Animal) run() {
	fmt.Printf("%v 在跑~\n", a.Name)
}

//Dog结构体
type Dog struct {
	feet int8
	*Animal
}
func (d Dog) wang() {
	fmt.Printf("%v 在叫~\n", d.Name)
}

func main() {
	var dog = Dog{
		feet: 4,
		Animal:&Animal { //注意嵌套的是结构体指针
			Name: "小汪财",
		},
	}
	dog.run()	//小汪财 在跑~
	dog.wang()	//小汪财 在叫~
}
~~~



###### 4.3.15 结构体字段可见性

结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）。

函数或变量，结构体及属性首字符大写，即可在其他包中访问，表示公有属性。



###### 4.3.16 结构体与JSON序列化

Golang中的序列化和反序列化主要通过"encoding/json"包中的 **json.Marshal()** 和 **json.Unmarshal()**

~~~go
//需求：把班级中的学生序列化与反序列化

//Class 班级
type Class struct {
	Title string
	Student []*Student
}

//Student 学生
type Student struct {
	Id     int
	Gender string
	Name   string
	No  string
}

func writeLog(str string) {
	file,err := os.OpenFile("log.txt",os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0666)
	if err != nil {
		fmt.Println("open file failed, err:", err)
		return
	}
	defer file.Close()

	file.Write([]byte(str))
}

func main() {
	var c = &Class{
		Title: "一年级一班",
		Student: make([]*Student, 0, 200),
	}
	//往班级中添加学生
	for iNum := 1; iNum <= 45; iNum++ {
		stu := &Student{
			Id: iNum,
			No: fmt.Sprintf("No%02d", iNum),
			Name: fmt.Sprintf("stu%02d",iNum),
			Gender: "男",
		}
		c.Student = append(c.Student, stu)
	}
	//json序列化
	data ,err := json.Marshal(c)
	if err != nil {
		fmt.Println("json marshal failed")
		return
	}
    //在控制台界面输出的字符串有点问题，就直接写文件了
	writeLog(string(data))

	//反序列化
	str := `{"Title":"一年级一班","Student":[{"Id":1,"Gender":"男","Name":"stu01","No":"No01"},{"Id":2,"Gender":"男","Name":"stu02","No":"No02"},{"Id":3,"Gender":"男","Name":"stu03","No":"No03"},{"Id":4,"Gender":"男","Name":"stu04","No":"No04"},{"Id":5,"Gender":"男","Name":"stu05","No":"No05"}]}`
	c1 := &Class{}
	err = json.Unmarshal([]byte(str), c1)
	if err != nil {
		fmt.Println("json unmarshal failed!")
		return
	}
	fmt.Printf("%v\n", c1)
	//【这里得到的结果是指针类型，还没有解决办法】
}
~~~

**因为要传入外包格式化，结构体中变量首字母需要大写，如果是小写，会被过滤丢弃掉**

**常见的格式化有：json:"name" db:"name" ini:"name"**



###### 4.3.17 任意类型添加方法

在Go语言中，接收者的类型可以是任何类型，不仅仅是结构体，任何类型都可以拥有方法。 

举个例子，我们基于内置的`int`类型使用type关键字可以定义新的自定义类型，然后为我们的自定义类型添加方法。

~~~go
//MyInt 将int定义为自定义MyInt类型
type MyInt int

//SayHello 为MyInt添加一个SayHello的方法
func (m MyInt) SayHello() {
	fmt.Println("Hello, 我是一个int。")
}
func main() {
	var m1 MyInt
	m1.SayHello() //Hello, 我是一个int。
	m1 = 100
	fmt.Printf("%#v  %T\n", m1, m1) //100  main.MyInt
}
~~~



###### 4.3.18 其他知识点

结构体的字段类型可以是：基本数据类型，也可以是切片、Map 以及结构体；

如果结构体的字段类似是：指针、slice、和 map 的零值都是nil，即还没有分配空间；

如果需要使用这样的字段，需要先make，才能使用。

~~~go
/**
结构体的字段类型可以是：基本数据类型，也可以是切片、Map 以及结构体；
如果结构体的字段类似是：指针、slice、和 map 的零值都是nil，即还没有分配空间；
如果需要使用这样的字段，需要先make，才能使用。
*/

type Person struct {
	Name   string
	age    int
	hobby  []string
	Detail map[string]string
}

func main()  {
	var person Person
	person.Name = "chenglh"
	person.age  = 21

	//切片申请空间
	person.hobby = make([]string, 3, 3)
	person.hobby[0] = "学习"
	person.hobby[1] = "运动"
	person.hobby[2] = "看报"

	//map申请空间
	person.Detail = make(map[string]string)
	person.Detail["address"] = "广东省广州市天河区"
	person.Detail["phone"]   = "13678917765"

	fmt.Printf("%#v\n", person)
	// main.Person{Name:"chenglh", age:21, 
	// hobby:[]string{"学习", "运动", "看报"}, 
	// Detail:map[string]string{"address":"广东省广州市天河区", "phone":"13678917765"}}
}
~~~



