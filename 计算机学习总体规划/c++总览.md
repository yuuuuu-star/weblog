# 学习顺序

- 1：完成翁恺 C++
    
    主要建立类、对象、生命周期、继承、多态等概念框架。
    
- 2：现代 C++ 知识清单
    
    优先补前面清单中的 01–04 模块，配合 PTA 和自己设计的小程序，不要求一开始全部精通。
    
- 2：浙大数据结构 + PTA
    
- 3：代码随想录 → 灵神 → LeetCode
    
    主要按算法专题递进，不重复刷三套相同题目。
    
- 
# 现代 C++ 知识点清单

## 01 · 指针、引用与类型系统
- [ ] 指针、数组与指针运算
- [x] 引用、const、const 指针
- [ ] 左值、右值、左值引用与右值引用
- [ ] auto、decltype、类型推导
- [ ] nullptr、类型转换、作用域与生命周期

## 02 · 现代语法基础
- [ ] 统一初始化与初始化列表
- [ ] 范围 for、结构化绑定
- [ ] Lambda 表达式与捕获列表
- [ ] constexpr、enum class
- [ ] 函数重载、默认参数、命名空间

## 03 · STL 容器与算法
- [ ] string、string_view 基础
- [ ] vector、array、deque
- [ ] map、unordered_map、set、unordered_set
- [ ] stack、queue、priority_queue
- [ ] 迭代器、sort、find、lower_bound
- [ ] Lambda 与 STL 算法配合

## 04 · 面向对象与资源管理
- [ ] 构造、析构、拷贝构造和赋值
- [ ] RAII 与异常安全基础
- [ ] Rule of Three / Five / Zero
- [ ] unique_ptr、shared_ptr、weak_ptr
- [ ] std::move、移动构造与移动赋值

## 05 · 泛型编程
- [ ] 函数模板与类模板
- [ ] 模板参数推导
- [ ] 泛型函数与标准库容器
- [ ] 类型别名、using、可变参数模板入门

## 06 · 工程基础
- [ ] 头文件、源文件、编译与链接
- [ ] ODR、链接错误与包含保护
- [ ] Git、GDB 调试与断点
- [ ] CMake 基础
- [ ] 单元测试、编译警告和 Sanitizer

## 07 · 现代 C++ 进阶
- [ ] 完美转发与 std::forward
- [ ] std::optional、std::variant
- [ ] C++20 Concepts 与 Ranges
- [ ] 协程、并发基础
- [ ] 模板元编程与性能优化



# 学习笔记
# C++ 面向对象基础：类、声明与定义、头文件

## 一、类（Class）与对象（Object）

### 1. 类的基本组成

类是一种用户自定义类型，用于组织数据和操作数据的函数。

一个类通常包含：

- **成员变量（Field / Data Member）**：描述对象的状态。
    
- **成员函数（Member Function / Method）**：定义对象可以执行的操作。
    

```
class Student {
private:
    int age;

public:
    void setAge(int a);
    int getAge();
};
```

`Student` 是类，`age` 是成员变量，`setAge()` 和 `getAge()` 是成员函数。

### 2. 类和对象的关系

```
Student a;
Student b;
```

- `Student`：类，是一种类型。
    
- `a`、`b`：对象（Object），也是 `Student` 类的实例（Instance）。
    
- 不同对象拥有各自的非静态成员变量。
    
- 普通成员函数的代码通常由同一个类的不同对象共享，通过 `this` 指针区分当前操作的对象。
    

**注意：** 这里讨论的是非静态成员。`static` 成员变量属于类，不是每个对象各有一份。

---

## 二、声明（Declaration）与定义（Definition）

### 1. 两者的区别

**声明**：告诉编译器某个名字代表什么、具有什么类型或接口。

**定义**：提供编译器或链接器所需的完整定义，例如函数体、变量实体或类的完整结构。

> 核心规则：定义通常也是声明，但声明不一定是定义。

### 2. 函数的声明与定义

```
void f();  // 函数声明（函数原型）

void f() { // 函数定义
    // 函数体
}
```

函数声明提供函数名、返回类型和参数类型；函数定义则提供实际执行的代码。

### 3. 变量的声明与定义

```
extern int global; // 声明
int global;        // 定义
int number = 10;   // 定义并显式初始化
```

上述代码以命名空间作用域为前提。

`extern int global;` 表示该变量的定义位于其他地方。

注意：`int global;` 即使没有写初始化值，在这种情况下仍然是定义。

同一程序中，通常应只为一个普通的、具有外部链接的全局变量提供一处定义。

### 4. 类的声明与定义

```
class Student;  // 类的前向声明

class Student { // 类的完整定义
    int age;
};
```

**重要：类的完整定义通常写在** `**.h**` **中。**

因此不能简单地说「头文件只放声明，不放定义」。头文件通常放类的完整定义，但可以只放普通成员函数的声明，把函数体放到 `.cpp`。

---

## 三、头文件（.h）与源文件（.cpp）

### 1. 为什么分离文件？

C++ 项目通常把类的接口与具体实现分开，以提高代码的可维护性和复用性。

|文件|主要作用|常见内容|
|---|---|---|
|`.h`|描述接口与类型|类定义、函数声明、`extern` 变量声明|
|`.cpp`|提供具体实现|成员函数定义、普通全局变量定义|
|`main.cpp`|程序入口|`main()` 函数、对象创建与调用|

### 2. 完整示例

**Student.h**

```
#ifndef STUDENT_H
#define STUDENT_H

class Student {
private:
    int age;

public:
    void setAge(int a);
    int getAge();
};

#endif
```

**Student.cpp**

```
#include "Student.h"

void Student::setAge(int a) {
    age = a;
}

int Student::getAge() {
    return age;
}
```

**main.cpp**

```
#include <iostream>
#include "Student.h"

int main() {
    Student s;
    s.setAge(18);

    std::cout << s.getAge() << '\n';
    return 0;
}
```

### 3. `::` 作用域解析运算符

```
void Student::setAge(int a)
```

`Student::` 表示后面的 `setAge` 属于 `Student` 类。

类外定义成员函数时，需要使用类名限定该函数。

单独写 `::f()` 则表示从全局命名空间查找 `f`。

---

## 四、编译单元（Translation Unit）与 #include

### 1. 什么是编译单元？

C++ 通常以每个 `.cpp` 为起点，分别编译。

准确地说，**一个翻译单元是一个源文件经过预处理之后得到的内容**，包括展开的头文件内容。

基本过程：

```
main.cpp
   ↓
预处理（展开 #include 等）
   ↓
编译
   ↓
main.o
   ↓
链接其他目标文件
   ↓
可执行程序
```

因此，包含某个 `.h` 并不意味着对应的 `.cpp` 自动参与编译。

这也是多文件项目中可能出现 `undefined reference` 的原因。

### 2. #include 的作用

`#include` 是预处理指令，作用是在预处理阶段引入指定文件的内容。

```
#include "Student.h"
#include <iostream>
```

两种写法通常有不同的搜索规则：

- `"Student.h"`：通常优先搜索当前文件附近或项目指定的头文件路径。
    
- `<iostream>`：通常通过编译器配置的系统或标准库头文件路径寻找。
    

具体搜索顺序由编译器及其配置决定。

**实践原则：**

- 实现某个类的 `.cpp`，应该包含对应的 `.h`。
    
- 使用该类的其他 `.cpp`，也应该包含其声明所在的 `.h`。
    
- 头文件自身依赖的类型，也应有适当的声明或包含。
    

---

## 五、头文件保护（Header Guard）

### 1. 为什么需要头文件保护？

由于 `#include` 会引入头文件内容，同一个翻译单元可能通过多条路径重复包含一个头文件。

如果同一个类在同一翻译单元内被重复定义，就会产生编译错误。

### 2. 标准写法

```
#ifndef STUDENT_H
#define STUDENT_H

class Student {
    // ...
};

#endif // STUDENT_H
```

逐行解释：

- `#ifndef STUDENT_H`：如果这个宏没有定义。
    
- `#define STUDENT_H`：定义该宏，标记头文件已被处理。
    
- `#endif`：结束条件编译。
    

**宏名称可以自行命名，但同一组指令中必须一致，并且应避免与其他宏重名。**

另一种常见写法：

```
#pragma once
```

它更加简洁，主流 C++ 编译器普遍支持，但并不是 ISO C++ 标准规定的预处理指令。

注意：Header Guard 主要解决单个翻译单元中的重复包含，不能消除多个 `.cpp` 中普通函数或全局变量重复定义造成的链接问题。

---

## 六、成员函数与 this 指针

### 1. 成员函数如何找到当前对象？

例如：

```
class A {
public:
    int value;

    void f() {
        value++;
    }
};

A a;
a.value = 10;
a.f();
```

这里的 `f()` 为什么知道应该修改对象 `a` 的 `value`？

因为 C++ 的非静态成员函数中存在一个特殊指针：`**this**`。

上面的函数可以显式写成：

```
void f() {
    this->value++;
}
```

在 `a.f()` 的调用过程中，`this` 指向对象 `a`。

### 2. this 的本质

- `this` 是指向当前对象的指针。
    
- 在 `A` 的普通非静态成员函数中，`this` 的类型是 `A*`。
    
- 在 `const` 成员函数中，`this` 的类型是 `const A*`。
    
- 静态成员函数没有 `this` 指针。
    

### 3. 从底层理解成员函数调用

可以概念性地理解成：

```
a.f();
```

类似于把对象地址传给一个处理该对象的函数：

```
f(&a);
```

但这只是帮助理解的一种**概念模型**，不是可以直接替换的 C++ 语法。

具体的参数传递方式由编译器及调用约定决定。

---

## 七、成员变量、参数与局部变量

|类型|作用域或所属关系|特点|
|---|---|---|
|非静态成员变量（Field）|类作用域，属于具体对象|每个对象拥有自己的成员|
|参数（Paramet|

## 构造与析构

1.保证成员变量被初始化 -- constructor
```c++
//构造函数 函数与类同名 没有返回类型
//构造函数在创建对象时就会被自动调用
class X { 
	int i；
public:
	X(int i);
};
X::X(int i)
{
	i++;
}
X obj1(114514);
```

```c++
class X {
public:
    int num;
    X(int i);
};

X::X(int i)
{
    i++;
    num = i;
    cout <<"传入的i值为："<<i<<endl;
}
X obj1(114514);
int main(){
    cout <<"现在访问obj得到的num为："<<obj1.num<<endl;
    return 0;
}
```

2.析构函数
```c++
//析构函数 当这个对象脱离作用域时调用 
//不能有参数
class Y{
public:
	int num;
	~Y();
}
Y::Y(){
}
```
## 对象初始化

1.内存在一进入某对大括号就会分配，但是初始化直到初始化那一行才会开始
数组：int a[10] ={1,2,3,4,5};  int b[] ={3,6,9}; 
  sizeof c / sizeof * c
嵌套：
  ![[Pasted image 20261027160707.png|314]]

```c++
//初始化多个对象
class Student {
public:
    int age;
    double score;
    char grade;

    Student(int a = 18, double s, char g)
        : age(a), score(s), grade(g) {}
};
```
2.default constructor 没有参数的初始化
## 动态内存分配：new delete

1.new 类型：new int; new int[10]; 
如果 new 自定义的类 会先分配空间，再调用构造函数，最后返回分配对象的地址

2.delete类型  delete 销毁对象并收回空间
int * psome = new int[10];
delete[ ] psome;
可以delete空指针

## 访问限制

1.public
2.private
3.protect
4.friend
struct X;
struct Y{
	void f(X*);
};
struct X{
private:
int i;
public:

}
class（首选）通常是私有的，struct默认是公有的
![[Pasted image 20261027184500.png]]