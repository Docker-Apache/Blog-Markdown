---
title: 第2章 Java基本句法
date: 2021-03-27 13:09:05
tags:
  - 数组
  - 引用类型
category:
  - Java
---

## 2.9 引用类型

**五种引用类型：数组、类、接口、枚举、注解**

- 接口是一种类似于类的引用类型，其中定义了方法签名，但一般没有实现方法的方法主体
- 不过，从 Java 8 开始，接口可以使用关键字 `default` 指明其中的方法是可选的。接口必须包含可选方法的实现
- 所有实现这个接口的类，如果没有实现可选的方法，则使用接口中默认的实现
- 枚举是特殊的类，注解是特殊的接口

### 2.9.1 引用类型与基本类型的比较

- 八种基本类型由 Java 语言定义，程序员不能定义新基本类型。引用类型由用户定义，因此有无限多个
- 基本类型表示单个值。引用类型是聚合类型，可以保存零个或多个基本值或对象。假设 Point 类存储两个 double 类型的值，char[] 和 Point[] 数组类型是聚合类型，因为它们保存一些 char 类型的基本值和 Point 对象
- **「基本类型使用深复制」**。把基本值赋给变量或传入方法时，会重新申请一块内存，在哪申请内存取决于变量类型。具体而言，局部变量在栈中，全局变量和静态变量都在全局（静态）存储区
- **「引用类型使用浅复制」**。把对象赋给变量或传入方法时，不会重新申请一块内存，而是把这个内存的引用存储在变量中或传入方法。即只有一个对象的副本，但是这个对象的引用有两个副本

### 2.9.2 处理对象和引用副本

> Java 无法使用任何方式处理引用，即不能对地址进行操作（引用可以看作 C 语言中的指针或地址）

**数组可以通过 clone() 或 System.arraycopy() 方法实现深复制**

```java
// 声明数组的两种方法
int[] srcNums = new int[]{2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
int[] srcNums = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};

// 浅复制
int[] dstNums = srcNums;
dstNums[0] = 100;   // srcNums[0] = 100

// 深复制
int[] dstNums = new int[srcNums.length];
System.arraycopy(srcNums, 0, dstNums, 0, srcNums.length);
dstNums[0] = 100;   // srcNums[0] = 2

int[] dstNums = srcNums.clone();
dstNums[0] = 100;   // srcNums[0] = 2
```

### 2.9.3 比较对象

#### "==" 运算符比较基本值和引用类型对象

- 比较基本值时，只判断两个值是否一样。即每一位的值都完全相同
- 比较引用类型的对象时，比较的是引用而不是对象。即 `==` 判断两个引用是否指向同一个对象，而不判断两个对象的内容是否相等

```java
String letter = "o";
String s = "hello";
String t = "hell" + letter; // s 和 t 保存的文本完全一样

if (s == t) {               // 两个引用指向的不是同一个对象，为假
    System.out.println("equal");
}
if (s.equals(t)) {           // 两个对象的内容相等，为真
    System.out.println("equal");
}
```

#### 引用相同与对象相等

> 使用 “相同”（identical）表示引用相同（指向同一个对象），使用 “相等”（equal）表示对象的内容相等

- 若想判断两个不同的对象是否相等，可以在一个对象上调用 `equals()` 方法，然后把另一个对象传入这个方法
- 所有对象都从 `Object` 类继承了 `equals()` 方法，但是默认的实现方式是使用 `==` 判断引用是否相同，而不判断对象内容是否相等。想比较对象是否相等的类可以自定义 `equals()` 方法
- 数组没有自定义，但 `String` 类自定义了。因此，数组调用的 equals() 方法与 `==` 运算符的作用一样
- 比较数组是否相等可以使用 `java.util.Arrays.equals()` 方法

```java
int[] nums = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
int[] nums2 = nums;
int[] nums3 = nums.clone();

if (nums.equals(nums2)) {   // 两个引用指向同一个对象，为真
    System.out.println("equal");
}
if (nums.equals(nums3)) {   // 两个引用指向的不是同一个对象，为假
    System.out.println("equal");
}
if (java.util.Arrays.equals(nums, nums3)) {   // 两个对象的内容相等，为真
    System.out.println("equal");
}
```

## 自动装包（装包和拆包转换）

- 包装类：Boolean、Byte、Short、Character、Integer、Long、Float、Double，每个实例只保存一个基本值
- 包装类一般在把基本值存储在集合中时使用，例如 java.util.List

```java
List<Integer> numbers = new ArrayList<>();  // List 接口
Integer i = 1;          // 自动装包 int -> Integer
numbers.add(i);
numbers.add(2);         // 自动装包 int -> Integer
int j = numbers.get(0); // 自动拆包 Integer -> int
```

```java
Integer i = 0;      // 把 int 类型字面量 0 装包到 Integer 对象中
Number n = 0.0f;    // 把 float 类型字面量装包到 Float 对象中，然后放大转换成 Number类型
Integer i = 1;      // 这是装包转换
int j = i;          // i在这里拆包
i++;                // 拆包 i，递增，再装包
Integer k = i + 2;  // 拆包 i，再装包两数之和
i = null;
j = i;              // null 只能赋给引用类型，抛出 NullPointerException 异常
```
