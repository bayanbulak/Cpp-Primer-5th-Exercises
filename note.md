## 第 1 章 开始

### 1.1 编写一个简单的 C++ 程序

#### 1.1.1 编译、运行程序

### 1.2 初识输入输出

**一个使用 IO 库的程序**

```c++
#include <iostream>

int main() {
    std::cout << "Enter two numbers: " << std::endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The sum of " << v1 << " and " <<  v2 << " is " << v1 + v2 << std::endl;
    return 0;
}
```



**向流写入数据**

* main() 函数的第一条语句执行了一个**表达式** (expression)

* `<<` 为**输出运算符**。接收两个运算对象：左侧的运算对象必须是一个 ostream 对象，右侧的运算对象是要打印的值。

* 表达式等价于

  ```c++
  (std::cout << "Enter two numbers: ") << std::endl;
  ```

  也等价于：

  ```c++
  std::cout << "Enter two numbers: ";
  std::cout << std::endl;
  ```

* 一个运算符给用户打印一条消息。这个消息是一个**字符串字面值常量** (String literal)。

* 第二个运算符打印 endl, 这是一个被称为操纵符 (manipulator) 的特殊值。写入 endl 的效果是结束当前行，并将与设备关联的缓冲区 (buffer) 中的内容刷到设备中。缓冲刷新操作可以保证到目前为止程序所产生的所有输出都真正写入输出流中，而不是仅停留在内存中等待写入流。**怎么理解？**

  > :warning: 程序员常常在调试时添加打印语句。这类语句应该保证"一直"刷新流。否则，如果程序崩溃，输出可能还停留在缓冲区中，从而导致关于程序崩溃为止的错误推断。

* `>>`  与 `<<` 都是从左到右，详细见如下：

  > :bulb: [C++ 内置运算符、优先级和关联性](https://learn.microsoft.com/zh-cn/cpp/cpp/cpp-built-in-operators-precedence-and-associativity?view=msvc-170)



**使用标准库中的名字**

前缀 `std::` 指出名字 cout 和 endl 是定义在名为 **std** 的**命名空间** (namespace)中的。

命名空间的作用：可以帮助我们避免不经意的名字定义冲突，以及使用库中相同名字导致的冲突。

标准库定义的所有名字都在命名空间 std 中。

命名空间使用标准库的副作用：当使用标准库中的一个名字时，必须显式说明我们想使用来自命名空间 std 中的名字。



**从流读取数据**

* 与输出运算符类似，输入运算符返回其左侧运算对象作为其计算结果。



**完成程序**



**1.2 节练习**

练习1.6: 解释下面程序是否合法。

```c++
std::cout << "The sum of " << v1;
					<< " and " << v2;
					<< " is " << v1 + v2 << std::endl;
```

如果程序是合法的，它输出什么？如果程序不合法，原因何在？应该如何修正？

The program is not legal. The operator `<<` is a member of `std::cout`, thus cannot be called without the object `std::cout`. Prepend the expression with `std::cout`.



### 1.3 注释简介

“错误的注释比完全没有注释更糟糕，因为他会误导读者”。

**C++ 中注释的种类**

C++有两种注释，单行注释和界定符对注释。



**注释界定符不能嵌套**



**1.3 节练习**

练习 1.7：编译一个包含不正确的嵌套注释的程序，观察编译器返回的错误信息。

```c++
#include <iostream>

/*
 * Test wrong explanation.*/
 */
int main() {
    return 0;
}
```

the result is: 

```
====================[ Build | 1_7 | Debug ]=====================================
/Applications/CLion.app/Contents/bin/cmake/mac/aarch64/bin/cmake --build /Users/john/CLionProjects/1.7/cmake-build-debug --target 1_7 -j 12
[1/2] Building CXX object CMakeFiles/1_7.dir/main.cpp.o
FAILED: CMakeFiles/1_7.dir/main.cpp.o 
/Library/Developer/CommandLineTools/usr/bin/c++   -g -std=gnu++2b -arch arm64 -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.2.sdk -fcolor-diagnostics -MD -MT CMakeFiles/1_7.dir/main.cpp.o -MF CMakeFiles/1_7.dir/main.cpp.o.d -o CMakeFiles/1_7.dir/main.cpp.o -c /Users/john/CLionProjects/1.7/main.cpp
/Users/john/CLionProjects/1.7/main.cpp:5:3: error: expected unqualified-id
 */
  ^
1 error generated.
ninja: build stopped: subcommand failed.
```



练习 1.8：指出下列哪些输出语句时合法的(如果有的话)：

```c++
std::cout << /* "*/" /* "/*" */; // OK
```



### 1.4 控制流

#### 1.4.1 while 语句



#### 1.4.2 for 语句

1.4.2 节练习

练习1.14: 对比 for 循环和 while 循环，两种形式的优缺点各是什么？

`for` has a scope where you can define temporary variables and used inside loop.

`while` is simple and apporiate for situations where the loop time is unknown before the loop.



#### 1.4.3 读取数量不定的输入数据

```c++
#include <isstream>
int main() {
  int sum = 0, value = 0;
  // 读取数据直到遇到文件尾，计算所有读入的值的和
  while(std::cin >> value) {
    sum += value; // 等价于 sum = sum + value   
  }
  std::cout << "Sum is: " << sum << std::endl;
  return 0;
}
```

该 while 循环会一直执行直到遇到文件结束符或输入错误。

> :bulb: **从键盘输入文件结束符**
>
> 当从键盘向程序输入数据时，对于如何指出文件结束，不同操作系统有不同的约定。在 Windows 系统中，输入文件结束符的方法是敲 `Ctrl` + `Z`, 然后按`Enter` 或 Return键。在 UNIX 系统中，包括 Mac OS X 系统中，文件结束符输入是用 `Ctrl` + `D`。
