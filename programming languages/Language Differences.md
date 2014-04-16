# Differences in Programming Languages
------

##语言基础 Basic

###可访问性 Accessibility(Public/Private/Protected)

- Obj-C中，在.h文件中定义的所有变量和函数都是public的，在.m文件中定义的所有变量和函数都是private的。有点隐式定义(implicit)的感觉.
- C++、Java、C#中，都是通过public、private等关键字显式(explicit)定义

###布尔值 BOOL

Obj-C中的BOOL是在c中通过typedef定义的，值分别为`YES`和`NO`。

###方法/函数 function

####定义

除了Obj-C，其他语言的函数定义都是如下形式

```
返回值类型 函数名(参数1类型 参数1名称,参数2类型 参数2名称...)
```

例如

```
void SayHello();
```

而Obj-C中，定义函数则是如下形式

```
- (返回值类型) 函数名:(参数1类型)参数1名称;
- (int) Sqrt:(int) value;
- (int) Add:(int)a To:(int) b;

```
上面的第二个函数名可以理解为 **Add:** a **To:** b

####类型
- 实例方法
实例方法（instance methods）通过类的实例调用。

```
[instanceofClass instanceMethods]
```
- 类方法
类方法（class methods）通过类名可直接调用。相当于C#、Java中类的静态方法。

```
[ClassName classMethods]
```

###对类成员的访问

- java、C#中，对类所有成员（包括method,property,field）的访问方式为`.`符。
- Obj-C中，只有对`@property`才能使用`.`符进行访问。方法的调用方式为`[]`。形式如下

```
[value1 Add:value2]
[函数的拥有者 函数名:参数]
```

###字符串 String

- C#中，`@"string content"` 中的`@`可让系统讲引号中的字符按照原样保存。

```
string a = "string/t";   // /t 被转义为tab
string b = @"string/t";	  // b中存储内容为string/t 	
```

- Obj-C中，`@"string"`则是直接将字符串内容存入`NSString`对象中，而不带@的字符串的实际意义为

```
const char * s = "string"
```
####格式化输出字符串

与C#、java相同，Obj-C提供类似`string.format`的方法。在Obj-C中，同样功能的方法如下

```
NSString *result = [NSString stringWithFormat:@"格式化字符串",元素1,元素2];
```

###指针 Pointers

- Java、C#的对象管理机制与C++中`shared_ptr`类似，其内部维护了该对象的被引用次数，只有当该对象不再被引用时，GC就能回收该对象在Heap上所占的内存空间。
- Obj-C中，通过`(strong)`和`(weak)`来表示当前指针的‘类型’，某个对象能被释放的条件是：所有对该对象的`(strong)`类型引用全部被置为`nil`。

###对象的存储位置 Stack & Heap

- C、C++、Java、C# 中，new出来的对象的实例存储在Heap上，其他变量存储在Stack上。
- Obj-C中，所有对象均存在于Heap之上，因此在Obj-C中，不能这样定义一个对象`NSString content`。

###属性 Property

- C#中，可通过以下方式定义一个Property

```
private int variable;
public int Variable
{
	get{return variable;}
	set{variable = value;}
}
```

目前，C#新增了自动属性特性，只要以`public int Variable;`定义，Variable就自动成为一个属性。在获取或设置属性时无需额外处理时，使用自动属性可大幅减少代码量。

- Obj-C中，通过以下方式定义一个Property

`in .h file`

```
@property (strong) NSString * content;
```

这类似C#中的自动属性。在Obj-C内部，是通过如下代码实现的该属性的getter和setter方法。

`in .m file`

```
@synthesize contents = _contents;
- (NSString *) contents
{
	return _contents;
}
- (void)setContents:(NSString*) contents
{
	_contents = contents;
}
```

上述代码通过@synthesize定义了一个content的同义词，由于其在.m文件中定义，因此是个private类型的变量。Obj-C为所有property都隐式定义了一个以`_`为前缀的同名的同义词，例如在上面的程序中，可以不定义@synthesize而直接使用`_content`变量。**例外的是，如果同时对某property同时定义了getter和setter，则必须显式定义`_content`。**

当然，若程序中需要在获取或设置该值时进行额外处理，则仍需手工完成属性的get和set方法。

在Obj-C中，可以在定义属性时指定getter和setter的名称，无需手工重新在.m文件中实现这两个方法。在手工指定getter/setter的名称之后，也可以在.m文件中重新实现其getter/setter方法。

`in .h file`

```
@property (getter=isChosen) BOOL chosen;
```

`in .m file`

```
@synthesize chosen = _chosen;
- (BOOL) isChosen
{
	return _chosen;
}
``` 

- 特殊用法

`in .h file`

```
@property (readonly) NSUInteger variable;
```

`in .m file`

```
@property (readwrite) NSUInteger variable;
```

注意上面代码要求.h和.m文件中属性名必须一致，这样该属性公开的部分为只读模式，而该属性私有部分则可读可写。私有属性与公开属性一样，Obj-C也为其提供了隐式的getter和setter方法。