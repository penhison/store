# java 刷题快速指南

本文用于本人笔记，记录如何从c++做题快速切换到java做题。

## java快速入门

java的基本教程参照[Java教程 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744)。

### java 数据类型

java类型分为基本类型和引用类型两类，没有指针。

- 基本类型和包装类型

  基本类型是值语义，基本类型字长与机器无关，每个基本类型有对应的包装类型，为引用语义，列表如下：

  | 基本类型 | 包装类型  | 字长（byte） | 字面量示例                 |
  | -------- | --------- | ------------ | -------------------------- |
  | byte     | Byte      | 1            | 无                         |
  | short    | Short     | 2            | 无                         |
  | int      | Integer   | 4            | 123, 0x78, 0173, 0b1111011 |
  | long     | Long      | 8            | 123L                       |
  | float    | Float     | 4            | 1.23f, 1.23F               |
  | double   | Double    | 8            | 1.23, 0.123e1, 123e-2      |
  | char     | Character | 2            | 'a', '\u1234'              |
  | boolean  | Boolean   | not defined  | true, false                |

  注意到java内码使用UTF-16编码，所以char字长是2字节。

  java 规范未定义boolean的字长，但是一般实现为boolean变量字长是4，boolean数组每个元素字长是1。

  java 整型字面量默认是int，浮点型默认是double，float和long字面量需要加后缀。

  java 基本类型在栈中分配没有默认值，使用未赋值的变量会报变量未初始化的异常，在堆中分配有默认值0，（boolean为false)。

  （注：以下讨论基本类型里boolean除外）

  java 基本类型运算时会进行自动类型提升，将结果自动提升到能够容纳下运算结果的类型，但是要注意byte, short, char会无条件提升到int。

  类型提升规则：`byte, short, char -> int -> long -> float -> double`

  赋值时向上赋值是容许的，向下赋值会报类型不兼容的异常，此时可能需要强制类型转换。

  例子：

  ```java
  byte b1 = 1;
  byte b2 = 2;
  byte b3 = b1 + b2; // Type mismatch: cannot convert from int to byte
  byte b4 = b1 + 1; // Type mismatch: cannot convert from int to byte
  byte b5 = (byte)(b1 + 1); // convert int to byte, ok
  double d1 = b1 + b2 // int -> double, ok
  ```

  java的三元运算符`?:`返回值类型符合类型提升规则，返回值类型为`:`两边的操作数的类型提升结果，但是要注意不带后缀的整数字面量类型（即int型字面量）优先级小于变量类型的优先级，例：

  ```
  byte b = 1, short s = 2, int i = 4, long ll = 8;
  float f = 4.0f, double d = 8.0;
  check is a boolean expression
  check ? b : 10 // byte
  check ? b : 10L // long
  check ? b : i // int
  check ? i : d // double
  check ? f : ll // float
  ```

  java 没有原生的无符号类型，但是基本类型的运算结果带符号和不带符号两个版本在二进制上一致，所以可以直接将使用对应的带符号类型表示无符号类型，取值时使用对应的二进制掩码获取类型提升后的整数，例：

  ```java
  byte b = -1;
  int i = b & 0x0ff;
  ```

  java 每个基本类型都有对应的包装类型（引用类型），基本类型和包装类型可以自动拆箱装箱（即自动类型转换）。

  基本类型和包装类型转换示例：

  ```java
  int i = 1;
  Integer iBox = new Integer(1) // 被标记为废弃，不要使用
  Integer iBox1 = Integer.valueOf(i); // 装箱
  Integer iBox2 = i; // 自动装箱
  int i1 = iBox.intValue(); // 拆箱
  int i2 = iBox; // 自动拆箱
  ```

  每个包装类都有类常量`MAX_VALUE`, `MIN_VALUE`。

  java 不支持运算符重载。

  java基本类型运算可以使用运算符，但引用类型不可以（`==`除外）。

  `==`是通用的运算符，基本类型使用值语义比较是否相等，引用类型比较引用是否相同（即是否指向同一对象）

  java基本类型比较使用运算符`<, <=, ==, >=, >`，包装类型为引用语义，原则上不能使用运算符比较大小，但是引用类型使用运算符不会报错，原因是引用类型会自动拆箱为基本类型再比较。

  包装类型比较使用compareTo函数，小于返回`-1`, 等于返回`0`, 大于返回`1`, 示例：

  ```java
  Integer i1 = Integer.valueOf(1);
  Integer i2 = Integer.valueOf(2);
  i1.compareTo(i2); // -1
  ```

  整数包装类型默认在`[-128, 127]`内会有缓存，范围内的整数使用valueOf生成的包装类型会直接返回缓存里的对象，可以在java的启动参数中更改缓存的大小。

- 引用类型

  java 除了基本类型外都是引用类型。

  java 引用类型在栈中分配同样没有默认值，使用未赋值的变量会报变量未初始化的异常，在堆中分配有默认值null。

  

  

