---
title: 第3章 Java面向对象编程
date: 2021-03-28 22:47:39
tags:
  - 对象
category:
  - Java
---

## 3.1 类简介

### 3.1.3 定义类的句法

- `class` 关键字前面可以放修饰符关键字或注解
- 如果类扩展其他类，类名后面要加上 `extends` 关键字和要扩展的类名
- 如果类实现一个或多个接口，类名或 `extends` 子句之后要加上 `implements` 关键字和用逗号分隔的接口名

```java
public class Integer extends Number implements Serializable, Comparable {
    // 这里是类的成员
}
```

**其他修饰符关键字**

**abstract**

- `abstract` 修饰的类未完全实现，不能实例化
- 只要类中有 `abstract` 修饰的方法，这个类就必须使用 `abstract` 声明。抽象类在 3.6 节介绍。

**final**

- `final` 修饰符指明这个类无法被扩展（不能有子类）
- 类不能同时声明为 `abstract` 和 `final`

**strictfp**

- 如果类声明为 `strictfp`，那么其中所有的方法都声明为 `strictfp`。这个修饰符极少使用

## 3.2 字段和方法

字段和方法有两种不同的类型：类成员和实例成员

- 类成员（静态成员）：关联在类自身上。包括使用 `static` 字段修饰的类字段（全局变量）和类方法（全局方法）。类外调用时，类名.类成员
- 实例成员：关联在类的单个实例（对象）上。包括未使用 `static` 字段修饰的实例字段和实例方法

**注意**

- 类方法只能使用类字段和类方法，实例方法可以使用类字段、类方法、实例字段和实例方法
- 类方法不能使用实例字段或实例方法，因为类方法不关联在类的实例身上，即没有 `this` 引用指向当前对象
- 实例字段和实例方法必须通过 `this` 引用调用

```java
public class Solution {
    public static final int X = 1;  // 类字段
    public int y = 2;               // 实例字段

    // 类方法
    public static void print_X() {
        System.out.println(X);      // 类方法调用类字段
        int tmp_X = get_X();        // 类方法调用类方法
    }

    public static int get_X() {
          return X;                 // 类方法调用类字段
    }

    // 实例方法 ---- 隐藏一个指向调用对象的 this 引用
    public void print_y() {
        System.out.println(X);      // 实例方法调用类字段，也可以 this.X
        int tmp_X = get_X();        // 实例方法调用类方法，也可以 this.get_X()

        System.out.println(y);      // 实例方法调用实例字段，也可以 this.y
        int tmp_y = get_y();        // 实例方法调用实例方法，也可以 this.get_y()


    }

    public int get_y() {            // 实例方法调用实例字段
        return y;                   // 也可以是 this.y
    }
};
```

## 3.3 创建和初始化对象

**this 的特殊用法**

在一个构造方法中可以使用 `this` 关键字调用同一个类中的另一个构造方法

- 如果一些构造方法共用大量的初始化代码，这种技术是有用的，因为能避免代码重复
- 如果构造方法执行很多初始化操作，在这种复杂的情况下，这种技术十分有用
- **使用 this() 时有个重大的限制：** 只能出现在构造方法的第一个语句中

```java
public class Solution {
    public static final int X = 1;  // 类字段
    public int y = 2;               // 实例字段

    // 构造方法 ---- 创建对象时初始化
    public Solution(int y) {        // Solution obj = new Solution(10)
        this.y = y;                 // 实例字段 y = 10
    }
    public Callback() {             // Solution obj = new Solution()
        this(3);                    // 实例字段 y = 3
    }
}
```

## 3.4 子类和继承

### 3.4.1 扩展类

**超类 Circle**

```java
public class Circle {
    public static final double PI = 3.14159;
    public double r;

    public Circle(double r) {
        this.r = r;
    }

    public Circle() {
        this(1.0);
    }

    public static double radiansToDegrees(double rad) {
        return rad * 180 / PI;
    }

    public double area() {
        return PI * r * r;
    }

    public double circumference() {
        return 2 * PI * r;
    }
}
```

**子类 PlaneCircle**

```java
public class PlaneCircle extends Circle {
    private double cx = 0.0;
    private double cy = 0.0;

    public PlaneCircle(double r, double x, double y) {
        super(r);       // 调用超类的构造函数：Circle(double r);
        this.cx = x;
        this.cy = y;
    }

    public double getCentreX() {
        return cx;
    }

    public double getCentreY() {
        return cy;
    }

    public static double radiansToDegrees(double rad) {
        return rad;
    }

    // 这个方法使用了继承的实例字段 r
    public boolean isInside(double x, double y) {
        double dx = x - cx;
        double dy = y - cy;
        double dist = Math.sqrt(dx * dx + dy * dy);
        return dist < r;
    }
}
```

**final 类**

- 如果声明类时使用了 `final` 修饰符，那么这个类无法作为超类或子类
- 子类不能重写超类的 `final` 方法

**子类对象与超类对象的赋值**

- 把子类对象赋值给超类对象时无需强制转换
- 把超类对象赋值给子类对象时需强制转换

```java
PlaneCircle pc = new PlaneCircle(1.0, 0.0, 0.0);
Circle c = pc;
PlaneCircle pc2 = (PlaneCircle) c;
boolean origininside = ((PlaneCircle) c).isInside(0.0, 0.0);
```

### 3.4.2 超类、对象和类层次结构

- `Object` 类是 Java 中唯一一个没有超类的类
- 所有 Java 类都从 `Object` 类中继承方法

### 3.4.3 子类的构造方法

**super 关键字**

在子类的构造方法中使用 `super()` 调用超类的构造方法与使用 `this()` 调用构造方法有同样的限制

- 只能在构造方法中使用 `super()`
- 必须在构造方法的第一个语句中调用超类的构造方法，甚至要放在局部变量声明之前

### 3.4.4 构造方法链和默认构造方法

如果构造方法的第一个语句没有使用 `this()` 或 `super()` 显式调用另一个构造方法，javac 编译器会插入 `super()`。即调用超类的构造方法，而且不传入参数

**以 `PlaneCircle` 类为例，创建实例时会经过以下过程：**

- 首先，调用 `PlaneCircle` 类的构造方法
- 这个构造方法显示调用了 `super(r)`，即调用 `Circle` 类的第一个构造方法 `Circle(double r)`
- `Circle(double r)` 构造方法会隐式调用 `super()`，即调用 `Circle` 的超类 `Object` 的构造方法。`Object` 只有一个构造方法
- 此时，到达层次结构的顶端了，接下来开始运行构造方法
- 首先运行 `Object` 构造方法的主体
- 返回后，再运行 `Circle(double r)` 构造方法的主体
- 最后，对 `super(r)` 的调用返回后，接着执行 `PlaneCircle()` 构造方法中余下的语句

### 3.4.5 遮盖超类的字段

假如有三个类 A、B 和 C，它们都定义了一个名为 x 的字段，而且 C 是 B 的子类，B 是 A 的子类。那么，在 C 类的方法中可以按照下面的方式引用这些不同的字段：

```java
x               // C 类的 x 字段
this.x          // C 类的 x 字段
super.x         // B 类的 x 字段
((B)this).x     // B 类的 x 字段
((A)this).x     // A 类的 x 字段
super.super.x   // 非法，不能这样引用 A 类的 x 字段
```

类似地，如果 c 是 C 类的实例，那么可以像这样引用这三个字段：

```java
c.x      // C 类的 x 字段
((B)c).x // B 类的 x 字段
((A)c).x // A 类的 x 字段
```

类字段也能被遮盖

```java
public static final double PI = 3.14159265358979323846; // PlaneCircle 类中重新定义 PI
PI 或 PlaneCircle.PI   // PlaneCircle 类的 PI
super.PI 或 Circle.PI  // Circle 类的 PI
```

### 3.4.6 覆盖超类的实例方法

类字段、类方法、实例字段被遮盖，实例方法被覆盖

```java
class A {
    int i = 1;
    int f() { return i; }
    static char g() { return 'A'; }
}
class B extends A {
    int i = 2;                      // 遮盖 A 类的字段 i
    int f() { return -i; }          // 覆盖 A 类的方法 f()
    static char g() { return 'B'; } // 遮盖 A 类的类方法 g()
}
public class OverrideTest {
    public static void main(String args[]) {
        B b = new B();
        System.out.println(b.i);    // 引用 B.i，打印 2
        System.out.println(b.f());  // 引用 B.f()，打印 -2
        System.out.println(b.g());  // 引用 B.g()，打印 B
        System.out.println(B.g());  // 调用 B.g() 更好的方式

        A a = (A) b;
        System.out.println(a.i);    // 现在引用的是 A.i，打印 1
        System.out.println(a.f());  // 还是引用 B.f()，打印 -2
        System.out.println(a.g());  // 引用 A.g()，打印 A
        System.out.println(A.g());  // 调用 A.g() 更好的方式
    }
}
```

#### 使用 super 引用被遮盖的字段和被覆盖的方法

```java
class B extends A {
    int i;
    int f() {
        i = super.i + 1;      // 引用被遮盖的字段时，两者等价
        i = ((A)this).i + 1;
        return super.f() + i; // 可以像这样调用 A.f()
    }
}
```

- 使用 `super` 引用被遮盖的字段时，相当于把 `this` 校正为超类类型，然后通过超类类型访问字段
- 不过，使用 `super` 引用被覆盖的方法和校正 `this` 引用不是一回事
- 也即，表达式 `super.f()` 和 `((A)this).f()` 的作用不一样

**解释器使用 `super` 句法调用实例方法时，会执行一种修改过的虚拟方法查找**

- 第一步和常规的虚拟方法查找一样，确定调用方法的对象属于哪个类。运行时会在这个类中寻找对应的方法定义
- 但是，使用 `super` 句法调用方法时，先在这个类的超类中查找。如果超类直接实现了这个方法，那就调用这个方法。如果超类继承了这个方法，那就调用继承的方法

**注意**

- `super` 关键字调用的是方法的直接覆盖版本
- 只能在覆盖某个方法的类内部使用 `super` 调用被覆盖的方法

### 3.5.1 访问控制

#### 访问包

Java 不直接支持包的访问控制。访问控制一般在类和类的成员这些层级完成

#### 访问类

默认情况下，顶层类在定义它的包中可以访问。不过，如果顶层类声明为 public，那么在任何地方都能访问

#### 访问成员

- 类中的所有字段和方法在类的主体里始终可以使用
- 如果类的成员使用 `public` 修饰符声明，那么可以在能访问这个类的任何地方访问这个成员
- 如果类的成员声明为 `private`，那么除了在类内部之外，其他地方都不能访问这个成员
- 如果类的成员声明为 `protected`，那么包里的所有类都能访问这个成员（等同于默认的包访问规则），且在这个类的任何子类的主体中也能访问这个成员（不管子类在哪个包中定义）
- 如果声明类的成员时没使用任何修饰符，那么使用默认的访问规则（有时叫包访问）。即包中的所有类都能访问这个成员，但在包外部不能访问

> 默认的访问规则比 protected 严格，因为默认规则不允许在包外部的子类中访问成员

**使用 protected 成员时要格外小心**

- 假设 A 类使用 `protected` 声明了一个字段 x，而且在另一个包中定义的 B 类继承 A 类（重点是 B 类在另一包中定义）
- 因此，B 类继承了这个 `protected` 声明的字段 x，那么，在 B 类的代码中可以访问当前实例的这个字段，而且引用 B 类实例的代码也能访问这个字段
- **但是，在 B 类的代码中不能访问 A 类实例的受保护字段 x**

```java
package javanut6.ch03;

public class A {
    public String examine(A a) {    // 在 B 类的代码中不能访问 A 类实例的受保护字段
        return "B sees: " + a.name;
    }
}
```

```java
package javanut6.ch03.different;
import javanut6.ch03.A;

public class B extends A {
    public String examine(B b) {    // 引用 B 类实例的代码也能访问这个字段
        return "B sees another B: " + b.name;
    }
}
```

#### 访问控制和继承

- 子类继承超类中所有可以访问的实例字段和实例方法
- 如果子类和超类在同一个包中定义，那么子类继承所有未使用 `private` 声明的实例字段和方法
- 如果子类在其他包中定义，那么它继承所有使用 `protected` 和 `public` 声明的实例字段和方法
- 使用 `private` 声明的字段和方法绝不会被继承，类字段和类方法也一样
- 构造方法不会被继承（而是链在一起调用）

#### 成员访问规则总结

| 能否访问             | public | protected | 默认 | private |
| -------------------- | ------ | --------- | ---- | ------- |
| 定义成员的类         | 是     | 是        | 是   | 是      |
| 同一个包中的类       | 是     | 是        | 是   | 否      |
| 不同包中的子类       | 是     | 是        | 否   | 否      |
| 不同的包，也不是子类 | 是     | 否        | 否   | 否      |

**经验法则**

- 只使用 `public` 声明组成类的公开 API 的方法和常量。使用 `public` 声明的字段只能是常量和不能修改的对象，且必须都使用 `final` 声明
- 使用 `protected` 声明大多数使用这个类的程序员不会用到的字段和方法，但在其他包中定义子类时可能会用到
- 如果字段和方法供类的内部实现细节使用，但是同一个包中协作的类也要使用，那么就使用默认的包可见性
- 使用 `private` 声明只在类内部使用，在其他地方都要隐藏的字段和方法

### 数据访问器方法
