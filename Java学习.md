# 开发环境配置

# 基础语法

### 程序：一系列对象的集合

- 类：模板，描述一类对象的行为和状态
- 对象：类的实例，具有状态和行为
- 方法：描述对象的行为
- 实例属性：对象独有的实例属性，描述对象的状态

### 基本语法：

- 大小写敏感
- 包名：所有字母小写
- 类名：首字母大写，多个单词构成时，每个单词首字母应大写
- 方法名：小写字母开头，多个单词构成时，后续单词首字母大写
- 源文件：源文件名必须与  `public class` 类名相同，以`.java` 结尾，且一个文件中只能有一个 `public class`
- 主方法入口：所有 java 程序都有 `public static viod main(String[] args)` 开始执行
- 变量定义：需在前面声明变量的类型

### 标识符：

类命、变量名、方法名都可成为标识符

- 由数字、字母、`$`、`_` 造成，不能以数字开头
- 大小写敏感
- 不能使用 java 关键字和保留字

### 修饰符：

修饰符用来修饰类中的方法和属性，分为两类

- 访问控制修饰符：default，public，protected，private
- 非访问控制修饰符：final，abstract，satic，synchronized

### 变量：

- 局部变量
- 类变量（静态变量）
- 成员变量（非静态变量）

### 枚举：

枚举是指用预先设置好的值来限制变量的值，以减少代码中的bug

```java
class FreshJuice{
    enum FreshJuiceSize {SMALL,MEDIUM,LARGE}
    FreshJuiceSize size;
}


public class Demo {
    public static void main(String []arg){
        //new 用来创建实例
        FreshJuice fresh = new FreshJuice();
        fresh.size = FreshJuice.FreshJuiceSize.MEDIUM;
        System.out.println(fresh.size);
    }
}
```

### 关键字：

#### 访问控制：

- private：私有的
- protected：受保护的
- public：公共的
- default：默认的

#### 类、方法和变量修饰符：

- abstract：声明抽象
- class：类
- extends：扩充，继承
- final：最终的，不可改变的
- implements：实现（接口）
- interface：接口
- native：本地，原生方法（非Java实现）
- new：新，创建
- static：静态
- strictfp：严格，精准
- synchronized：线程，同步
- transient：短暂
- volatile：易失

#### 程序控制语句：

- for：循环
- while：循环
- break：跳出循环
- continue：继续
- default：默认
- do：运行
- return：返回
- if：如果
- else：否则
- instanceof：实例
- switch：根据值选择执行
- case：定义一个之以供 swich选择

#### 错误处理：

- assert：断言表达式是否为真
- try：捕获异常
- catch：捕捉异常
- throw：抛出一个异常
- finally：有没有异常都指向

#### 包相关：

- import：引入
- package：包

基本类型：

- byte：字节型
- int：整型
- short：短整型
- long：长整型
- double：双精度浮点
- float：单精度浮点
- char：字符型
- boolean：布尔型

#### 变量引用：

- super：父类，超类
- this：本类
- void：无返回值

#### 保留关键字：

- goto：关键字，不能使用
- const：关键字，不能使用
- null：空

### 注释和空行：

注释和空行会被编译器自行忽略

```java
public class HelloWorld{
    /*该代码输出 hello world！

	上面空行会被自动忽略
    这是一个多行注释*/
    public static viod main(String[] args){
        //这是一个单行注释
        /*这也是一个单行注释*/
        System.out.println("hello world!");
    }
}
```

#### 注释模板：

##### 类注释：

```java
/**
* Copyright (C), 2006-2010, ChengDu Lovo info. Co., Ltd.
* FileName: Test.java
* 类的详细说明
*
* @author 类创建者姓名
* @Date    创建日期
* @version 1.00
*/
```

##### 属性注释：

```java
/** 提示信息 */
private String strMsg = null;
```

##### 方法注释：

```java
/**
* 类方法的详细使用说明
*
* @param 参数1 参数1的使用说明
* @return 返回结果的说明
* @throws 异常类型.错误代码 注明从此类方法中抛出异常的说明
*/
```

##### 构造方法注释：

```java
/**
* 构造方法的详细使用说明
*
* @param 参数1 参数1的使用说明
* @throws 异常类型.错误代码 注明从此类方法中抛出异常的说明
*/
```

##### 方法内部注释：

```java
//背景颜色
Color bgColor = Color.RED
```

### 接口：

Java 中，接口可理解为对象间相互通信的协议

接口只定义派生类要用到的方法，但方法的具体实现完全取决于派生类

### Java 源程序与编译型运行区别：

![img](E:\Markdown\images\ZSSDMld.png)

#### 编译型：

编译型源程序 >> 可执行 exe >> 操作系统

#### 源程序：

源程序(.java) >> 字节码程序(.class) >> Java 解释器 >> 操作系统

### 源文件声明规则：

- 一个文件中只能有一个 `public` 类
- 一个文件中可以有多个非 `public` 类
- 源文件名应该和 `public` 类的名称保持一致，且以 `.java` 结尾
- 如果一个类定义在某个包中，`package` 语句应该在源文件的首行
- 源文件包含 `import` 语句时，应该放在 `package` 语句和类定义之间，否则应该在源文件最前面
- `import` 和 `package` 对源文件中所有定义的类都有效。同一源文件中，不能给不同的类不同的包声明。

### Java 包：

包主要用来对类和接口进行分类。当开发 Java 程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。

### Import 语句：

用来提供一个合理的路径，使得编译器可以找到某个类。

```java
import java.io.* //载入 java_installation/java/io 路径下的所有类
```



# 类和对象

- 类：类是一个模板，描述一类对象的行为和状态
- 对象：是类的一个实例，具有一定的状态和行为

### 简单定义类：

关键字 `class`

```java
public class Dog{
    //变量
    String breed;
    int age;
    String color;
	//方法
    void eat(){
    }
    void sleep(){
    }
    void run(){
    }
}
```

### 构造方法：

- 创建对象时调用类的构造方法
- 没有显示定义构造方法，java 编译器会为类提供一个默认的缺省的构造方法
- 构造方法名必须与类名相同（大小写敏感），且可以有多个
- 同一类型的构造方法有多个时，只调用第一个

```java
public class Pubby{
    //构造器
    public Pubby(){
    }
    //构造器只有一个参数
    public Pubby(String name){
        System.out.println(name);
    }
}
```

### 变量：

- 局部变量：在方法、构造方法、语句块中定义的变量为局部变量，变量声明和初始化都在方法中，方法结束后自动销毁
- 成员变量：成员变量为定义在类中，方法体之外的变量。该变量在创建对象的过程中实例化，成员变量可以被类中方法、构造方法和特定类的语句块访问
- 类变量：又称静态变量，类变量也声明在类中、方法体之外，但必须声明为 `static` 类型

类变量可以被类名、成员对象调用，成员变量只能被对象调用

```java
public class Pubby {
    static String breed="哈士奇"; //类变量
    int pubbyAge; //成员变量
    public Pubby(String name, int age){
        //构造方法实例化
        String pubbyName=name; //局部变量
        pubbyAge = age;
        System.out.println(pubbyName);
    }
    public static void main(String[] args){
        System.out.println(breed); //直接访问类变量
        System.out.println(Pubby.breed); //类命调用类变量
        Pubby pubby = new Pubby("tom",3); //实例化
        System.out.println(pubby.pubbyAge); //成员变量只能对象调用
        System.out.println(pubby.breed); //对象调用类变量
        // System.out.println(pubby,pubbyName); //局部变量不能访问
    }
    
}
```



### 创建对象：

使用关键字 `new` 来创建一个对象

- 声明：声明一个对象，包括对象名称和对象类型
- 实例化：使用关键字 `new` 来创建一个对象
- 初始化：使用 `new` 创建对象时，会调用构造方法初始化对象

```java
public class Pubby{
	public Pubby(String name){
        //构造器
        System.out.println("小狗的名字为"+name)
    }
    public static void main(String[] args){
        //创建对象
        Pubby puby = new Pubby("tommy")
    }
}
```

### 访问实例对象和方法：

- 实例化对象：`Object obj = new Object()`
- 访问类中的变量：`obj.variable`
- 访问类中的方法：`obj.method()`

```java
public class Pubby{
 	int pubbyAge;
    public Pubby(String name){
        //构造器
        System.out.println("小狗的名字为："+name);
    }
    public void setAge(int age){
        pubbyAge = age;
    }
    public int getAge(){
        System.out.println("小狗的年龄为："+pubbyAge);
    	return pubbyAge;
    }
    public static void main(String[] args){
        //创建对象
        Pubby myPubby = new Pubby("Tom");
        //通过方法来设定 age
        myPubby.setAge(3);
        //调用方法获取 age
        myPubby.getAge();
        //另一种方式访问成员变量
        System.out.println(myPubby.pubbyAge)
    }
}
```

### 内部类 和 匿名类

- [Java 内部类 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-inner-class.html)
- [Java 匿名类 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-anonymous-class.html)
- [java 中的内部类总结 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/java-inner-class-summary.html)
- [Java 内部类详解 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/java-inner-class-intro.html)



# Java 基本数据类型

Java 内存管理系统根据变量的类型分配存储空间，分配的空间只能储存该类型的数据。

Java 基本数据类型可分为两类：

- 内置数据类型
- 引用数据类型

### 内置数据类型：

- byte：8 位有符号数值， 大小为`-128（-2^7） ~ 127（2^7-1）`，默认值为 `0`
- short：16 位有符号数值，大小为 `-32768（-2^15）~ 32767(2^15-1)`，默认值为 `0`
- int：32 位有符号数值，大小为 `-2,147,483,648（-2^31）~ 2,147,483,647（2^31 - 1）`，默认值为 `0`
- long：64 位有符号数值，大小为 `-9,223,372,036,854,775,808（-2^63）~9,223,372,036,854,775,807（2^63 -1）`，默认值为 `0L`
- float：单精度、32 位浮点数，默认值为 `0.0f`
- double：双精度、64 位浮点数，默认值为 `0.0d`
- boolean：布尔值，只有 `true` 和 `false` 两个取值，默认值为 `false`
- char：单一的 16 位的 Unicode 字符，大小为 `\u0000（0） ~ \uffff（65535）`，char 字符只能用 `‘’` 表示

Java编译器内置了各数据类型的二进制位数，及组最大、最小值

- SIZE：二进制位数，如：`Byte.SIZE`
- MIN_VALUE：最小值，如：`Byte.MIN_VALUE`
- MAX_VALUE：最大值，如：`Byte.MIN_VALUE`

各类型默认值：

| **数据类型**           | **默认值** |
| :--------------------- | :--------- |
| byte                   | 0          |
| short                  | 0          |
| int                    | 0          |
| long                   | 0L         |
| float                  | 0.0f       |
| double                 | 0.0d       |
| char                   | 'u0000'    |
| String (or any object) | null       |
| boolean                | false      |

### 引用数据类型：

- 引用类型指向一个对象，指向对象的变量就是引用变量，该引用变量在声明时需指定类型，变量一旦声明后，就不能再改变类型
- 对象、数组都是引用数据类型
- 所有引用类型的默认值都为 `null`

### 常量：

- 使用 `final` 关键字来修饰常量，声明方式和变量类似，如：`final double PI=3.1415926;`
- 常量名通常大写
- 字面量可以赋值给任何内置类型的变量，如：`byte a = 68;
  char a = 'A';`

### 转义字符：

| 符号   | 字符含义                 |
| :----- | :----------------------- |
| \n     | 换行 (0x0a)              |
| \r     | 回车 (0x0d)              |
| \f     | 换页符(0x0c)             |
| \b     | 退格 (0x08)              |
| \0     | 空字符 (0x0)             |
| \s     | 空格 (0x20)              |
| \t     | 制表符 (tab)                    |
| \"     | 双引号                   |
| \'     | 单引号                   |
| \\     | 反斜杠                   |
| \ddd   | 八进制字符 (ddd)         |
| \uxxxx | 16进制Unicode字符 (xxxx) |

### 类型转换

整型、常量、字符型数据可以混合运算，运算中，不同类型的数据先转换为同一类型，然后再运算。

#### 转换顺序：

> byte,short,char >> int >> long >> float >> double 

转换规则：

- 不能对 `boolean` 类型进行类型转换
- 不能把对象类型转换成不相关类的对象
- 容量大的类型转换为容量小的类型必须使用强制转换
- 转换过程中可能导致溢出或损失精度
- 浮点数到整数的转换是通过舍弃小数得到，而不是四舍五入

#### 自动类型转换：

- 必须满足转换前的数据类型位数 低于 转换后的数据类型位数
- 系统自动转换

```java
public class ZiDongZhuanHuan{
    public static void main(String[] args){
        char c='a';
        int i=c;
        double d=i;
        System.out.println("char 转换为 int 后为："+i);
        System.out.println("int 转换为 double 后为："+d);
    }
}
```



#### 强制类型转换：

容量小的类型转为容量大的类型时需使用强制转换

- 语法：`(type) value` ，type 时要强制转换后的数据类型
- 转换的数据类型必须是兼容的

```java
public class QiangZhiZhuanHuan{
    public static void main(String[] args){
        int i = 123;
        byte b = (byte)1;//强制类型转换为byte
        System.out.println("int强制类型转换为byte后的值等于"+b);
    }
}
```

# Java 变量类型

### 变量声明：
> type identifiler [=value][, identifiler [=value]...]

- 声明多个同类型变量：`int a,b,c;`
- 声明多个同类型变量并赋值：`int a=1,b=2,c=3;`
- 声明并初始化：`byte b=22; String s='tom';`

### 变量类型：
Java 语言支持的变量类型：
- 类变量：独立于方法之外的变量，用关键字 `static` 修饰
- 实例变量：独立于方法之外的变量，没有用关键字 `static` 修饰
- 局部变量：类的方法之中的变量

```java
public class Variable{
    static int all=0;  //类变量
    String str='tom';  //实例变量

    public void method(){
        int i=0;  //局部变量
    }
}
```

#### 局部变量：
- 局部变量声明在方法、构造方法或语句块中
- 在方法、构造方法、语句块被执行时创建，执行完成后销毁
- 局部变量只在声明它的构造方法、方法或语句块中可见
- 访问修饰符不能用于局部变量
- 局部变量**没有默认值**，必须经过初始化才能使用
- 局部变量是在栈上分配的

```java
public class LocalVariable{
    public void pubAge(){
        int age=0; //局部变量声明及初始化
        age = age+7;
        System.out.println("小狗的年龄是："+age);
    }
    public static void main(String[] args){
        LocalVariable local = new LocalVariable();
        local.pubAge();
    }
}
```

#### 实例变量：
- 实例变量声明在类中，但在构造方法、方法或语句块之外
- 实例变量在对象创建时创建，在对象销毁时销毁
- 一个对象实例化后，实例变量的值就跟着确定
- 实例变量可以声明在使用前或使用后
- 实例变量对在类中的方法、构造方法或语句块中是可见的，一般实例变量设为私有，通过访问修饰符可以使实例变量对子类可见
- 访问修饰符可以修饰实例变量
- 实例变量具有默认值，默认值同数据类型

```java
public class InstanceVariable{
    public String name; //该实例变量对子类可见
    private double salary; //私有变量
    public InstanceVariable(String name){
        this.name=name;
    }
    public void setSalary(double empSal){
        salary = empSal;
    }
    public void print(){
        System.out.println("名字："+name);
        System.out.println("薪水："+salary);
    }

    public static void main(String[] args){
        InstanceVariable instance = InstanceVariable();
        instance.print();
    }
}
```

#### 类变量

- 类变量也被称为静态变量，用关键字 `static` 声明，且必须在方法之外
- 静态变量在第一次被访问时创建，在程序结束后销毁
- 与实例变量具有相似的可见性，为对类的使用者可见，大多静态变量声明为 `public` 类型
- 静态变量被储存在静态存储区
- 静态变量是指声明为 `public`/`private`, `final` 和 `static` 类型的变量，初始化后不可改变
- 静态变量可以通过 `ClassName.VariableName` 的方式访问
- 静态变量的默认值同数据类型
- 类变量被声明为 `public static final` 时，一般默认使用大写字母

```java
public ClassVariabl{
    private static double salary;
    public static final String DEPARTMENT="开发人员";
    public static void main(String[] args){
        System.out.println(DEPARTMENT+"平均工资"+salary)
    }
}
```

# Java 修饰符

### 访问控制修饰符

访问控制修饰符用来保护对类、变量、方法和构造方法的访问，按级别分为四类：
- private：同一类内可见，用于变量、方法。**注意：不能修饰外类（外部类）**
- default：（即默认，什么也不写）只有同一内可见，用于类、接口、变量、方法
- protected：只有同一包内和所有子类可见，用于变量、方法。**注意：不能修饰外类（外部类）**
- public：所有类可见，用于类、接口、变量、方法

#### 私有访问修饰符：private
- 最严格的访问级别
- 被声明的变量、方法和构造方法只能被所属类访问
- 私有变量的访问只能通过类中公共的 `getter`来访问，不能通过实例来访问

```java
class AccessDemo{
    private String priValue; //私有变量
    public String getValue(){
        //设置公共方法getter方法获取私有变量
        //可以被所属类访问
        return this.priValue; 
    }
    public void setValue(String value){
        //为私有变量设置值
        this.priValue=value;
    }
}

public class AccessModifier {
    public static void main(String[] args){
        AccessDemo acces =new  AccessDemo();
        acces.setValue("private_value");
        System.out.println(acces.getValue());  //out: private_value
        //System.out.println(acces.priValue);  //不能访问
        // System.out.println(AccessDemo.priValue); //不能访问
    }
}

```

#### 默认访问修饰符：不用任何关键字
- 不使用任何修饰符（default）
- 被声明的类、变量、方法只对同一包内可见
- 不同包的子类也不可见

```java
class AccessDemo{
    static String defaultValue="default"; //默认访问
    static boolean defaultMethod(){
        return true;
    }
}


public class AccessModifier {
    public static void main(String[] args){
        AccessDemo acces =new  AccessDemo();
        System.out.println(acces.defaultValue); //out: default
        System.out.println(acces.defaultMethod()); //out: true
        System.out.println(AccessDemo.defaultValue); //out: default
        System.out.println(AccessDemo.defaultMethod()); //out: true
    }
}

```

#### 受保护的访问修饰符：protected
- 只能用于修饰变量、方法，不能修饰类（外部类）
- 被声明的变量、方法只对同一包内、不同包的子类可见
- 不同包内，子类实例只能访问基类继承的 `protected` 方法，而不能访问基类**实例**的`protected`方法

```java
class AccessDemo{
    protected String protectValue;
    protected void setProtect(String value){
        this.protectValue=value;
    }
    protected String getProtect(){
        return this.protectValue;
    }
}

public class AccessModifier {
    public static void main(String[] args){
        AccessDemo acces =new  AccessDemo();
        acces.setProtect("protected_value");
        System.out.println(acces.protectValue); //out: protected_value
        System.out.println(acces.getProtect()); //out: protected_value

    }
}
```
#### 公共访问修饰符：public
- 被 `public` 声明的类、方法、接口可以被任何类访问
- 由于类的继承性，类的所有 `public` 变量、方法都会被子类继承
- 如果几个相互访问的 public 类分布在不同的包中，则需要导入相应 public 类所在的包

```java
public static void main(String[] arguments) {
   // ...
}
```

### 非访问修饰符

为了实现一些功能，Java 中也提供了许多非访问修饰符：
- static：用来修饰类方法、类变量
- final：用来修饰类、方法、变量，`final` 修饰的类不能被继承，修饰的方法不能被子类修改，修饰的变量为常量，不可修改
- abstract：修饰抽象类、抽象方法
- synchronized/volatile：主要用于线程的编程

#### static:
- 静态变量：`static` 关键字用来声明独立于对象的静态变量，无论类实例多少个变量，它的静态变量只有一份拷贝（共用存储空间）
- 静态方法：`static` 关键字用来声明独立于对象的静态方法；静态方法不能使用类的非静态变量；静态变量从参数列表中得到数据，并计算数据

```java
public class InstanceCounter{
    private static int numInstances=0; //创建私有静态变量初始化
    protected static int getCount(){
        //创建受保护的静态方法
        return numInstances;
    }
    private static void addCount(){
        //创建私有静态方法
        numInstances++;
    }
    InstanceCounter(){
        //构造方法中调用静态方法
        InstanceCounter.addCount();
    }

    public static void main(String[] args){
        System.out.println("start with "+InstanceCount.getCount()+" instances");
        for(int i=0;i<500;i++){
            new IntanceCount();
        }
        System.out.println("created "+InstanceCount.getCount()+" instances");
    }
}
```
#### final:

- `final` 修饰的变量一旦被赋值后，就不能再被重新赋值（报错)
- `final` 修饰的实例变量需要显示的指定初始值
- `final` 关键字通常和 `static` 关键字一起表示常量
- `final` 修饰的方法可以被子类继承，但不能被重写
- `final` 修饰的类不能被继承

```java
public class FinalTest{
    final int value=10;
    //定义常量
    public static final int MAX=12;
    static final String NAME="Too";
}
```

#### abstract：
- `abstract` 关键字用来修饰抽象类或抽象方法
- 抽象类的作用时为了将来扩充类的功能
- 一个类不能同时被 `abstract`、`final`修饰
- 一个类中如果有抽象方法，则该类必须声明为抽象类
- 抽象类可以包含抽象方法、其他方法
- 抽象方法为没有任何实现的方法，该方法的实现由子类扩充
- 抽象方法不能被声明为 `static`、`final` 
- 抽象类的子类必须实现父类的所有抽象方法，否则子类也是抽象类
- 抽象类中可以没有抽象方法

```java
public abstract class AbsDemo{
    public static int value=1;
    public abstract void m();
}

class SubDemo extends AbsDemo{
    public void m(){
        //实现抽象方法
    }
}

```
#### synchronized：
- `synchronized` 关键字声明的方法同一时间只能被一个线程访问
- `synchronized` 可以应用于4个访问修饰符

```java
public synchronized void syncMethod(){
    ......
}
```

#### transient：
- 该修饰符包含在定义变量的语句中，用来预处理类和变量
- 序列化的对象包含被 `transient` 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量

```java
public transient int limit = 55;   // 不会持久化
public int b; // 持久化
```

#### volatile：
- `volatile` 关键字能保持变量在线程中的同一
- `volatile` 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该变量值，当成员变量发生变化，都强制线程将变化值回写到共享内存中
- 一个 `valatile` 对象引用可能是 null

```java
public class MyRunnable implements Runnable{
    private volatile boolean active;
    public void run(){
        active =true;  //第一行
        while (active){
            System.out.println("循环中")
        }
    }
    public void stop(){
        active=false;  //第二行
    }
}
```
通常情况下，在一个线程调用 run() 方法（在 Runnable 开启的线程），在另一个线程调用 stop() 方法。 如果 第一行 中缓冲区的 active 值被使用，那么在 第二行 的 active 值为 false 时循环不会停止。

但是以上代码中我们使用了 volatile 修饰 active，所以该循环会停止。

# Java 运算符：

## 运算符分类

Java 中的运算符主要有以下几类：
- 算术运算符
- 关系运算符
- 位运算符
- 逻辑运算符
- 赋值运算符
- 其他运算符

### 算术运算符
| 操作符 | 描述                              | 例子                               |
| :----- | :-------------------------------- | :--------------------------------- |
| +      | 加法 - 相加运算符两侧的值         | A + B 等于 30                      |
| -      | 减法 - 左操作数减去右操作数       | A – B 等于 -10                     |
| *      | 乘法 - 相乘操作符两侧的值         | A * B等于200                       |
| /      | 除法 - 左操作数除以右操作数       | B / A等于2                         |
| ％     | 取余 - 左操作数除以右操作数的余数 | B%A等于0                           |
| ++     | 自增: 操作数的值增加1             | B++ 或 ++B 等于 21（区别详见下文） |
| --     | 自减: 操作数的值减少1             | B-- 或 --B 等于 19（区别详见下文） |

自增自减运算符

- 自增（++）自减（- -）运算符是一种特殊的算术运算符，在算术运算符中需要两个操作数来进行运算，而自增自减运算符是一个操作数
- 前缀自增自减法（++a,- -a）：先进行自增或自减运算，再进行表达式运算
- 后缀自增自减法（a++,a- -）：先进行表达式运算，再进行自增或自减运算

```java
public class Operator{
    public static void main(String[] args){
        int a = 10;
        int b = 20;
        int c = 25;
        int d = 25;
        int x = c++;
        int y = ++d;
        System.out.println("a + b = " + (a + b) ); //30
        System.out.println("a - b = " + (a - b) ); //-10
        System.out.println("a * b = " + (a * b) ); //200
        System.out.println("b / a = " + (b / a) ); //2
        System.out.println("b % a = " + (b % a) ); //0
        System.out.println("c % a = " + (c % a) ); //5
        System.out.println("a++   = " +  (a++) ); //10
        System.out.println("a--   = " +  (a--) ); //11
        // 查看  d++ 与 ++d 的不同
        System.out.println("x=c++ =" +  (x)+"; c="+(c) ); //25, x=c, c=c+1 所以 x=25,c=26
        System.out.println("y=++d =" +  (y)+"; d="+(d) ); //27, d=d+1, y=d 所以 y=d=26
    }
}
```

### 关系运算符

| 运算符 | 描述                                                         | 例子               |
| :----- | :----------------------------------------------------------- | :----------------- |
| ==     | 检查如果两个操作数的值是否相等，如果相等则条件为真。         | （10 == 20）为假。 |
| !=     | 检查如果两个操作数的值是否相等，如果值不相等则条件为真。     | (10 != 20) 为真。  |
| >      | 检查左操作数的值是否大于右操作数的值，如果是那么条件为真。   | （10> 20）为假。   |
| <      | 检查左操作数的值是否小于右操作数的值，如果是那么条件为真。   | （10 <20）为真。   |
| >=     | 检查左操作数的值是否大于或等于右操作数的值，如果是那么条件为真。 | （10> = 20）为假。 |
| <=     | 检查左操作数的值是否小于或等于右操作数的值，如果是那么条件为真。 | （10 <= 20）为真。 |

```java
public class Operator{
    public static void main(String[] args){
        int a = 10;
        int b = 20;
        System.out.println("a == b = " + (a == b) ); //false
        System.out.println("a != b = " + (a != b) ); //true
        System.out.println("a > b = " + (a > b) ); //false
        System.out.println("a < b = " + (a < b) ); //true
        System.out.println("b >= a = " + (b >= a) ); //true
        System.out.println("b <= a = " + (b <= a) ); //false
    }
}
```



### 位运算符

位运算符应用于整数类型(int)，长整型(long)，短整型(short)，字符型(char)，和字节型(byte)等类型。

| 操作符 | 描述                                                         | 例子                             |
| :----- | :----------------------------------------------------------- | :------------------------------- |
| ＆     | 如果相对应位都是1，则结果为1，否则为0                        | （60＆13），得到12，即0000 1100  |
| \|     | 如果相对应位都是 0，则结果为 0，否则为 1                     | （60 \| 13）得到61，即 0011 1101 |
| ^      | 如果相对应位值相同，则结果为0，否则为1                       | （60 ^ 13）得到49，即 0011 0001  |
| 〜     | 按位取反运算符翻转操作数的每一位，即0变成1，1变成0。         | （〜60）得到-61，即1100 0011     |
| <<     | 按位左移运算符。左操作数按位左移右操作数指定的位数。         | 60 << 2得到240，即 1111 0000     |
| >>     | 按位右移运算符。左操作数按位右移右操作数指定的位数。         | 60 >> 2得到15即 1111             |
| >>>    | 按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。 | 60>>>2得到15即0000 1111          |

```java
public class Operator{
    public static void main(String[] args){
        int a = 60; /* 60 = 0011 1100 */ 
        int b = 13; /* 13 = 0000 1101 */
        System.out.println("a & b = " +  (a&b) ); //12 = 0000 1100
        System.out.println("a | b = " + (a|b) ); //61 = 0011 1101
        System.out.println("a ^ b = " + (a^b) ); //49 = 0011 0001
        System.out.println("~a = " + (~a) ); //-61 = 1100 0011
        System.out.println("a << 2 = " + (a << 2) ); //240 = 1111 0000
        System.out.println("a >> 2  = " + (a >> 2)); //15 = 1111
        System.out.println("a >>> 2 = " + (a >>> 2) ); //15 = 0000 1111
    }
}
```

### 逻辑运算符
操作符|描述|例子(A为真，B为假)
--|--|--
&&|称为逻辑与运算符。当且仅当两个操作数都为真，条件才为真。|（A && B）为假。
\|\||称为逻辑或操作符。如果任何两个操作数任何一个为真，条件为真。|（A 
！|称为逻辑非运算符。用来反转操作数的逻辑状态。如果条件为true，则逻辑非运算符将得到false。 |！（A && B）为真。

```java
public class Operator{
    public static void main(String[] args){
 boolean a = true;
     boolean b = false;
     System.out.println("a && b = " + (a&&b));
     System.out.println("a || b = " + (a||b) );
     System.out.println("!(a && b) = " + !(a && b));
   }
}
```

### 赋值运算符

操作符|描述|例子
--|--|--
=|简单的赋值运算符，将右操作数的值赋给左侧操作数|C = A + B将把A + B得到的值赋给C
+=|加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数|C + = A等价于C = C + A
-=|减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数|C - = A等价于C = C - A
*=|乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数|C * = A等价于C = C * A
/=|除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数|C / = A，C 与 A 同类型时等价于 C = C / A
（％）=|取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数|C％= A等价于C = C％A
<<=|左移位赋值运算符|C << = 2等价于C = C << 2
\>>=|右移位赋值运算符|C >> = 2等价于C = C >> 2
＆=|按位与赋值运算符|C＆= 2等价于C = C＆2
^=|按位异或赋值操作符|C ^ = 2等价于C = C ^ 2
\|=|按位或赋值操作符|C \|= 2等价于C = C | 2

```java
public class Operator{
    public static void main(String[] args){
        int a = 10;
        int b = 20;
        int c = 0;
        c = a + b;
        System.out.println("c = a + b = " + c );  //30
        c += a ;
        System.out.println("c += a  = " + c ); //40
        c -= a ;
        System.out.println("c -= a = " + c ); //30
        c *= a ;
        System.out.println("c *= a = " + c ); //300
        a = 10;
        c = 15;
        c /= a ;
        System.out.println("c /= a = " + c ); //1
        a = 10;
        c = 15;
        c %= a ;
        System.out.println("c %= a  = " + c ); //5
        c <<= 2 ;
        System.out.println("c <<= 2 = " + c ); //20
        c >>= 2 ;
        System.out.println("c >>= 2 = " + c ); //5
        c >>= 2 ;
        System.out.println("c >>= 2 = " + c ); //1
        c &= a ;
        System.out.println("c &= a  = " + c );  //0
        c ^= a ;
        System.out.println("c ^= a   = " + c ); //10
        c |= a ;
        System.out.println("c |= a   = " + c ); //10
    }
}
```

**注意**：`s += a` 和 `s= s+a` 的区别：

- `+=`运算符既能实现运算，又不会改变原数据的数据类型  
- `s=s+a` 要求s、a是统一类型，否则无法直接运算（需类型转换）

```java
public class Operator{
    public static void main(String[] args){
        short s=10;
        //s = s+3; //编译不通过
        //s = (short)(s+3); //强制类型转换
        s+=1;
        System.out.println("s+=1 后s的值是："+(s));
    }
}
```





### 条件运算符

条件运算符也被称为三元运算符
> variable x = (expression) ? value if true : value if false

```java
public class Operator{
    public static void main(String[] args){
        int a , b;
        a = 10;
        // 如果 a 等于 1 成立，则设置 b 为 20，否则为 30
        b = (a == 1) ? 20 : 30;
        System.out.println( "Value of b is : " +  b );
    
        // 如果 a 等于 10 成立，则设置 b 为 20，否则为 30
        b = (a == 10) ? 20 : 30;
        System.out.println( "Value of b is : " + b );
    }
}
```

**注意：**三元运算符的关联性为右关联性，如 `a?b:c?d:e`，先执行 `c?d:e`，在执行 `a?b:(c?d:e)`

### instanceof 运算符

- `instanceof` 用于操作对象实例，检查该对象是否是一个特定类型（类实例或接口实例）

- 语法：

  > ( Object reference variable ) instanceof  (class/interface type)

- 子类是父类的类型，但父类不是子类的类型；子类的实例是父类的类型，但父类的实例不是子类的类型

```java
class Vehicle{

}

public class Car extends Vehicle {
    public static void main(String args[]){
        Vehicle v1 = new Vehicle(); //父类型
        Vehicle v2 = new Car(); //子类的实例可以声明为父类型
        Car c1 = new Car();    // 子类型
        // Car c2 = new Vehicle(); //这句会报错，父类型的实例不能声明为子类型

        //Car（子类）是Vehicle（父类）类型, Vehicle（父类）不是Car（子类）类型
        boolean result1 =  c1 instanceof Vehicle;    // true
        boolean result2 =  c1 instanceof Car;        // true
        boolean result3 =  v1 instanceof Vehicle;    // true
        boolean result4 =  v1 instanceof Car;          // false
        boolean result5 =  v2 instanceof Vehicle;    // true
        boolean result6 =  v2 instanceof Car;          // true

        System.out.println(result1);
        System.out.println(result2);
        System.out.println(result3);
        System.out.println(result4);
        System.out.println(result5);
        System.out.println(result6);
   }
}
```

## 运算符优先级

多个运算符在表达式中时，按照如下的优先级进行计算(优先级由高到低，同一优先级按关联性计算)


| 类别     | 操作符                                     | 关联性   |
| :------- | :----------------------------------------- | :------- |
| 后缀     | () [] . (点操作符)                         | 左到右   |
| 一元     | expr++ expr--                              | 从左到右 |
| 一元     | ++expr --expr + - ～ ！                    | 从右到左 |
| 乘性     | * /％                                      | 左到右   |
| 加性     | + -                                        | 左到右   |
| 移位     | >> >>>  <<                                 | 左到右   |
| 关系     | > >= < <=                                  | 左到右   |
| 相等     | == !=                                      | 左到右   |
| 按位与   | ＆                                         | 左到右   |
| 按位异或 | ^                                          | 左到右   |
| 按位或   | \|                                         | 左到右   |
| 逻辑与   | &&                                         | 左到右   |
| 逻辑或   | \| \|                                      | 左到右   |
| 条件     | ？：                                       | 从右到左 |
| 赋值     | = + = - = * = / =％= >> = << =＆= ^ = \| = | 从右到左 |
| 逗号     | ，                                         | 左到右   |

# Java 条件语句

### if
语法：
> if(expr){expr为true时执行}

### if...else
语法：
> if(expr){expr为true时执行}else{expr为false时执行}

### if...else if...else
语法：
> if(expr1){expr1为true时执行}else if(expr2){expr2为true时执行}else{ expr1、expr2为false时执行}
- 可以有多个`else if`, 但必须在 `else` 之前
- 如果有 `else if` 执行，其他`else if`、`else` 不再执行



# Java 循环结构

- `while` 循环
- `do...while` 循环
- `for` 循环

### while 循环


语法：

> while ( bool expr ) { code };
```java
public class Test{
    public static main(String[],args){
        int x=10;
        while (x<20){
            System.out.println(x);
            x++;
        }
    }
}
```

### do...while
相比于 while 循环，至少会执行一次 do
语法：
> do { code } while ( bool expr );
```java
public class Test{
    public static main(String[],args){
        int x=10;
        do{
            System.out.println("x is "+x);
            x++
        }while(x<20);
        System.out.println("x is "+x);
    }
}
```

### for 循环
语法：
> for (初始化;条件表达式;更新){ 代码块 };
- 可初始化多个循环变量，也可以为空语句

```java
public class Test{
    public static void main(String[] args){
        for (int x=10;x<20;i++>){ 
            System.out.println("x is "+x);
        }
    }
}
```
### for...each 循环
语法：
> for (声明语句:数组)){ 代码块 }
```java
public class Test{
    public static void main(String[] args){
        String [] names={"James","Larry", "Tom", "Lacy"};
        for (String name:names){
            System.out.println(name);
        }
    }
}
```


### break 和 continue
- `break` 关键字用来跳出循环（循环结束）
- `continue` 关键字用来结束本次循环，进行下一次循环（跳到循环条件判断语句，循环继续）

```java
public class Test{
    public static void main(String[] args){
        int [] numbers = {10,20,30,40,50,60};
        for (int n:numbers){
            if (n==30){
                continue; //跳出本次循环，开始下次循环
            }
            if (n==50){
                break; //结束整个循环
            }
            System.out.println(n);
        }
    }
}
```
### 案例
``` java
public class LoopDemo {
    public static void main(String[] args){
        //while 循环
        int x=10;
        while (x<20){
            System.out.println("while 循环："+x);
            x++;
        };
        //do...while 循环
        do{
            System.out.println("do...while 循环："+x);
            x--;
        }while(x>10);
        //for 循环
        for(int i=0;i<10;i++){
            System.out.println("for 循环："+i);
        }
        //for...each 循环
        String [] names = {"James","Larry", "Tom", "Lacy"};
        for (String name:names){
            System.out.println("for...each 循环："+name);
        }
        //break 和 continue
        int [] numbers = {10,20,30,40,50,60};
        for (int n:numbers){
            if (n==30){
                continue;
            }
            if (n==50){
                break;
            }
            System.out.println("break and continue："+n);
        }

        //案例
        //九九乘法表
        for (int i=1;i<10;i++){
            for (int j=1;j<=i;j++){
                System.out.printf("%d*%d=%d ", i,j,i*j);
            };
            System.out.println();
        }
        //等腰三角形
        for (int i=0;i<5;i++){
            for (int j=0;j<10;j++){
                if (Math.abs(5-j)<=i){
                    System.out.print('*');
                }else{
                    System.out.print(' ');
                }
            }
            System.out.println();
        }
        
    }
}
```
# Java 分支

### swich...case分支语句

`swich...case` 用来判断一个变量与一系列值是否相等，每个值被称为一个分支

语法：
> swich(expresion){
    case value1: //语句
    case value2: //语句
    default:
}

- `swich` 语句可以有多个 `case`分支，每个`case` 分支后跟一个比较值和`:`
- `case` 比较值数据类型需与变量类型相同，且必须是常量和字面量
- 变量值与 `case`值相等时，执行`case` 后的语句，直到遇到 `break`跳出 swich
- 如果没有`break`，匹配成功后，当前`case`语句和后续的`case`、`default` 分支都会被执行
- `swich` 语句最后可以有一个 `default` 分支，当没有 `case`值符合的时候执行`default` 分支，`default` 分支不需要 `break` 

```java
public class Test {
   public static void main(String args[]){
      //char grade = args[0].charAt(0);
      char grade = 'C';
 
      switch(grade){
         case 'A' :
            System.out.println("优秀"); 
            break;
         case 'B' :
         case 'C' :
            System.out.println("良好");
            break;  //跳出swich
         case 'D' :
            System.out.println("及格");
            break;
         case 'F' :
            System.out.println("你需要再努力努力");
            break;
         default :
            System.out.println("未知等级");
      }
      System.out.println("你的等级是 " + grade);
        
    switch(grade){
        //没有break,后续分支都会被执行
        case 'A' :
        System.out.println("优秀"); 
        case 'B' :
        case 'C' :  //
        System.out.println("良好");
        case 'D' :
        System.out.println("及格");
        case 'F' :
        System.out.println("你需要再努力努力");
        default :
        System.out.println("未知等级");
   }
}
```

# Nubmer & Math 类

### Number 类

Java 为每个基本数据类型都提供了包装类，包装类可以为我们提供基本数据对象
所有的包装类（Byte,Short,Integer,Long,Float,Double）都是抽象类 Number 类的子类

#### 基本类型对应的包装类：

包装类	|基本数据类型
--|--
Boolean	|boolean
Byte	|byte
Short	|short
Integer	|int
Long	|long
Character	|char
Float	|float
Double	|double

包装类继承关系：

![aaa](https://www.runoob.com/wp-content/uploads/2013/12/OOP_WrapperClass.png)


这种编译器所支持的包装成为装箱。
当内置数据类型被当作对象使用时，编译器会把内置类型装箱为包装类。
同样，编译器也可以将对象拆箱为内置类型。

```java
public class Test{
 
   public static void main(String[] args){
      Integer x = 5; //x被声明为一个对象，x被赋值时，编译器会对x进行装箱
      x =  x + 10; //x是一个对象，为了能够加法计算，编译器会对x进行拆箱
      System.out.println(x); 
   }
}
```
#### Number 类常用方法：
方法|作用
--|--
xxxValue()|将 Number 对象转换为xxx数据类型的值并返回。如x.byteValue()
compareTo()|将number对象与参数比较(只能相同类型比较)。如x.compareTo(v);x>v返回1,x=v返回0,x<v返回-1
equals()|判断number对象是否与参数相等(返回true/false)。
valueOf()|返回指定参数的number对象。如：Integer.valueOf(5)
toString()|以字符串形式返回值。
parseInt()|将字符串解析为int类型。parseDouble()等同理

```java
public class Test {  
    public static void main (String []args)  
    {  
        Integer x=5;
        Integer y = Integer.valueOf(3);
        Integer z = Integer.valueOf("10");
        Integer w = Integer.valueOf("55",16);
        int a = Integer.parseInt("5");
        int b = Integer.parseInt("44",16);
        double c = Double.parseDouble("5");


        System.out.println(x.byteValue()); //5
        System.out.println(x.shortValue()); //5
        System.out.println(x.longValue()); //5
        System.out.println(x.floatValue()); //5.0f
        System.out.println(x.doubleValue()); //5.0

        System.out.println(x.compareTo(3)); //1
        System.out.println(x.compareTo(5)); //0
        System.out.println(x.compareTo(8)); //-1

        System.out.println(x.equals(4)); //false

        System.out.println(y); //3
        System.out.println(z); //10
        System.out.println(w); //85
        System.out.println(y instanceof Integer); //true

        System.out.println(x.toString()); //"5"
        System.out.println(Integer.toString(12)); //"12"

        System.out.println(a); //5
        System.out.println(b); //68
        System.out.println(c); //5.0    
    }  
}
```

#### int 和 Integer 的区别
1. int 是基本数据类型，变量存的是值；Integer 是引用类型，存的是引用对象的地址

2. new 生成新的对象，内存地址不同
```java
Integer i = new Integer(100);
Integer j = new Integer(100);
System.out.print(i == j); //false
```

3. int 占用内存更少。Integer 是一个对象，需要存储对象元数据；而 int 是原始类型的数据
4. 非 new 生成的 Integer 变量和 new Integer() 生成的变量比较，结果为false。
因为非 new 生成的 Integer 变量指向的是 java 常量池中的对象，而 new Integer() 生成的变量指向堆中新建的对象，两者在内存中的地址不同

```java
Integer i= new Integer(200);
Integer j = 200;
System.out.print(i == j);//输出：false
```
5. 两个非 new 生成的 Integer 对象进行比较，如果两个变量的值在区间 [-128,127] 之间，比较结果为 true；否则，结果为 false。
```java
Integer i1 = 127;
Integer ji = 127;
System.out.println(i1 == ji);//输出：true
Integer i2 = 128;
Integer j2 = 128;
System.out.println(i2 == j2);//输出：false
```
6. Integer 变量(无论是否是 new 生成的)与 int 变量比较，只要两个变量的值是相等的（自动拆箱），结果都为 true。

```java
Integer i1 = 200;
Integer i2 = new Integer(200);
int j = 200;
System.out.println(i1 == j);//输出：true
System.out.println(i2 == j);//输出：true
```


### Math 类

Java 中 Math 类提供了用于执行基本数据计算的方法和属性
Math 的方法都被定义为 `static` 形式，通过 `Math` 类可以直接调用

```java
public class Test {  
    public static void main (String []args)  
    {  
        System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));  //1.0
        System.out.println("0度的余弦值：" + Math.cos(0));  //1.0
        System.out.println("60度的正切值：" + Math.tan(Math.PI/3));  //1.7320508075688767
        System.out.println("1的反正切值： " + Math.atan(1));  //0.7853981633974483
        System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2)); //90.0
        System.out.println(Math.PI);  //3.141592653589793
    }  
}
```
Math 类常用方法：
方法|作用
--|--
abs()|返回参数的绝对值。
ceil()|返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。
floor()|返回小于等于（<=）给定参数的最大整数 。
rint()|返回与参数最接近的整数。返回类型为double。
round()|它表示四舍五入，算法为 Math.floor(x+0.5)，即将原来的数字加上 0.5 后再向下取整，所以，Math.round(11.5) 的结果为12，Math.round(-11.5) 的结果为-11。
min()|返回两个参数中的最小值。
max()|返回两个参数中的最大值。
exp()|返回自然数底数e的参数次方。
log()|返回参数的自然数底数的对数值。
pow()|返回第一个参数的第二个参数次方。
sqrt()|求参数的算术平方根。
sin()|求指定double类型参数的正弦值。
cos()|求指定double类型参数的余弦值。
tan()|求指定double类型参数的正切值。
asin()|求指定double类型参数的反正弦值。
acos()|求指定double类型参数的反余弦值。
atan()|求指定double类型参数的反正切值。
atan2()|将笛卡尔坐标转换为极坐标，并返回极坐标的角度值。
toDegrees()|将参数转化为角度。
toRadians()|将角度转换为弧度。
random()|返回一个随机数。

Math 的 floor,round 和 ceil 方法实例比较:

参数|Math.floor|Math.round|Math.ceil
-|:-:|:-:|-
1.4|1|1|2
1.5|1|2|2
1.6|1|2|2
-1.4|-2|-1|-1
-1.5|-2|-1|-1
-1.6|-2|-2|-1


# Character 类

- `Character` 类用于对单个字符进行操作
- `Character` 类在对象中包装一个基本类型 char 的值

创建 `Character` 对象：
> Character ch = new Character('a');
> Character ch = Character.valueOf('a');

Java 会自动将 `char` 基本类型抓换为 `Character` 对象，这被称为装箱，反之称为拆箱

```java
Character ch = 'a'; //装箱
char c = ch; //拆箱
```
### Character 方法
方法|描述
-|-
isLetter()|是否是一个字母
isDigit()|是否是一个数字字符
isWhitespace()|是否是一个空白字符
isUpperCase()|是否是大写字母
isLowerCase()|是否是小写字母
toUpperCase()|指定字母的大写形式
toLowerCase()|指定字母的小写形式
toString()|返回字符的字符串形式，字符串的长度仅为1

# String 类

Java 中字符串属于对象，Java提供了 String 类来创建和操作字符

创建字符串：
> String str = "tom";
> String str2 = new String("tom");

`String` 创建的字符串存储在常量池中，创建新的相同字符串时直接引用常量池中的地址。而 `new` 创建的字符串对象在堆中，每次创建都会创建一个新的对象
```java
String s1 = "Runoob";              // String 直接创建
String s2 = "Runoob";              // String 直接创建
String s3 = s1;                    // 相同引用
String s4 = new String("Runoob");   // String 对象创建
String s5 = new String("Runoob");   // String 对象创建
```

![img](https://www.runoob.com/wp-content/uploads/2013/12/java-string-1-2020-12-01.png)

String 类中有11中构造方法，提供不同的参数用来初始化字符串
如：字符数组参数
```java
public class StringDemo{
    public static void main(String[] args){
        char[] charArray = {'h','e','l','l','o','!'};
        String hello = new String(charArray);
        System.out.println(hello);
    }
}
```

**注意：** `String` 类是不可改变的，如果需要对字符串进行修改，应选择使用 `StringBuffer/StringBuilder`类
```java
String s = "Google";
System.out.println("s = " + s); //Google

s = "Runoob"; //创造一个新对象，google还在内存中
System.out.println("s = " + s); //Runoob
```

### 格式化字符串
- `String` 类使用 f`ormat()` 方法返回一个 `String` 对象，而不是一个 `PrintStream` 对象
- `String.format()` 方法返回一个可复用的格式化字符串

##### 常用的格式符：

格式符|适用于|输出
a|浮点数 (除了BigDecimal)|浮点数的十六进制输出
b|任何类型|如果为非空则为“true”，为空则为“false”
c|字符|Unicode字符
d|证书(包括byte, short, int, long, bigint)|十进制整数
e|浮点数|科学计数的十进制数
f|浮点数|十进制数
g|浮点数|十进制数，根据值和精度可能以科学计数法显示
h|任何类型|通过hashCode()方法输出的16进制数
n|无|平台相关的换行符
o|整数(包括byte, short, int, long, bigint)|八进制数
s|任何类型|字符串
t|日期/时间 (包含long, Calendar, Date 和TemporalAccessor)|%t是日期/时间转换的前缀。后面还需要跟其他的标识，请参考下面的日期/时间转换。
%x|整数(包含byte, short, int, long, bigint)|十六进制字符串


```java
 String ft = String.format("浮点型变量的值为 " +
        "%f, 整型变量的值为 " +
        " %d, 字符串变量的值为 " +
        " %s", 10.3f, 25, "hello");
System.out.println(ft);
```

### String 方法：
方法|描述
-|-
char charAt(int index)|返回指定索引处的 char 值。
int compareTo(Object o)|把这个字符串和另一个对象比较(返回不同值得ascii码差值或两个字符串的长度差)。
int compareTo(String anotherString)|按字典顺序比较两个字符串(同comparaTo)
int compareToIgnoreCase(String str)|按字典顺序比较两个字符串，不考虑大小写(同comparaTo)。
String concat(String str)|将指定字符串连接到此字符串的结尾。
boolean contentEquals(StringBuffer sb)|当且仅当字符串与指定的StringBuffer有相同顺序的字符时候返回真。
static String copyValueOf(char[] data)|返回指定数组中表示该字符序列的 String（字符拼接）。
static String copyValueOf(char[] data, int offset, int count)|返回指定数组中表示该字符序列的 String（字符拼接）。
boolean endsWith(String suffix)|测试此字符串是否以指定的后缀结束。
boolean equals(Object anObject)|将此字符串与指定的对象比较(内容比较)。
boolean equalsIgnoreCase(String anotherString)|将此 String 与另一个 String 比较，不考虑大小写。
byte[] getBytes()| 使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中(String转为byte[])。
byte[] getBytes(String charsetName)|使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。
void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)|将字符从此字符串复制到目标字符数组。
int hashCode()|返回此字符串的哈希码。
int indexOf(int ch)|返回指定字符在此字符串中第一次出现处的索引。
int indexOf(int ch, int fromIndex)|返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。
int indexOf(String str)|返回指定子字符串在此字符串中第一次出现处的索引。
int indexOf(String str, int fromIndex)|返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。
String intern()|返回字符串对象的规范化表示形式。
int lastIndexOf(int ch)|返回指定字符在此字符串中最后一次出现处的索引。
int lastIndexOf(int ch, int fromIndex)|返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。
int lastIndexOf(String str)|返回指定子字符串在此字符串中最右边出现处的索引。
int lastIndexOf(String str, int fromIndex)|返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。
int length()|返回此字符串的长度。
boolean matches(String regex)|告知此字符串是否匹配给定的正则表达式。
boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)|测试两个字符串区域是否相等。
boolean regionMatches(int toffset, String other, int ooffset, int len)|测试两个字符串区域是否相等。
String replace(char oldChar, char newChar)|返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。
String replaceAll(String regex, String replacement)|使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。
String replaceFirst(String regex, String replacement)|使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。
String[] split(String regex)|根据给定正则表达式的匹配拆分此字符串。
String[] split(String regex, int limit)|根据匹配给定的正则表达式来拆分此字符串。
boolean startsWith(String prefix)|测试此字符串是否以指定的前缀开始。
boolean startsWith(String prefix, int toffset)|测试此字符串从指定索引开始的子字符串是否以指定前缀开始。
CharSequence subSequence(int beginIndex, int endIndex)|返回一个新的字符序列，它是此序列的一个子序列。
String substring(int beginIndex)|返回一个新的字符串，它是此字符串的一个子字符串。
String substring(int bupeginIndex, int endIndex)|返回一个新字符串，它是此字符串的一个子字符串。
char[] toCharArray()|将此字符串转换为一个新的字符数组。
String toLowerCase()|使用默认语言环境的规则将此 String 中的所有字符都转换为小写。
String toLowerCase(Locale locale)|使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。
String toString()|返回此对象本身（它已经是一个字符串！）。
String toUpperCase()|使用默认语言环境的规则将此 String 中的所有字符都转换为大写。
String toUpperCase(Locale locale)|使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。
String trim()|返回字符串的副本，删除前导空白和尾部空白。
static String valueOf(primitive data type x)|返回给定data type类型x参数的字符串表示形式。
contains(CharSequence chars)|判断是否包含指定的字符系列。
isEmpty()|判断字符串是否为空。

```java
import javax.sound.midi.SysexMessage;

public class StringDemo {
    public static void main(String[] args){
        String str="Hello";
        String str1= new String("world");

        str = "hello ";
        System.out.println(str);

        char[] charArray = {'h','e','l','l','o','!'};
        String hello = new String(charArray);
        System.out.println(hello);  //hello!
        System.out.println(hello.length()); //6

        String ft = String.format("浮点型变量的值为 " +
        "%f, 整型变量的值为 " +
        " %d, 字符串变量的值为 " +
        " %s", 10.3f, 25, "hello");
        System.out.println(ft); //
        
        System.out.println(hello.charAt(3)); //l
        System.out.println(hello.compareTo("world")); //-15
        System.out.println(hello.compareTo(str)); //32
        System.out.println(hello.compareToIgnoreCase(str)); //1
        System.out.println(hello.concat(str1)); //hello!world
        StringBuffer sb = new StringBuffer("hello!"); //true
        System.out.println(hello.contentEquals(sb)); //false
        char[] ch={'h','e','l','l','o','w','o','r','l','d'};
        System.out.println(String.copyValueOf(ch)); //helloworld
        System.out.println(hello.endsWith("o"));//false
        System.out.println(hello.equals("hello!")); //true 值比较
        System.out.println(hello.equalsIgnoreCase("hellO!")); //true
        System.out.println(hello.getBytes()); //[B@214c265e
        char[] cha = new char[6];
        hello.getChars(1, 3, cha, 0);
        System.out.println(cha); //el
        System.out.println(hello.hashCode()); //-1220935281
        System.out.println(hello.indexOf('l')); //2
        System.out.println(hello.indexOf("lo")); //3
        System.out.println(hello.length()); //6
        System.out.println(hello.matches("(.*)!")); //true
        System.out.println(hello.replace("!", " world!"));
        String str2 = "www.baidu.com";
        for (String string:str2.split("\\.")){
            System.out.println(string); //www baidu com
        }
        System.out.println(hello.substring(1)); //ello!
        System.out.println(hello.substring(1,3)); //el
        System.out.println(hello.subSequence(1, 3)); //el
        System.out.println(hello.toCharArray()); //hello!
        System.out.println(hello.toUpperCase()); //HELLO!
        System.out.println(hello.toLowerCase()); //hello!
        System.out.println(hello.trim()); //hello!
        System.out.println(String.valueOf(3.14)); //3.14
        System.out.println(String.valueOf(hello.toCharArray())); //hello!
        System.out.println(hello.contains("he")); //true
        System.out.println(hello.isEmpty());
        System.out.println(hello.isBlank());
    }
}
```

# StringBuffer/StringBuilder 类

与 `String` 相比，`StringBuffer/StringBuilder` 类能够被多次修改，且不产生新的未使用对象

![](https://www.runoob.com/wp-content/uploads/2013/12/java-string-20201208.png)

`StringBuffer`和`StringBuilder`的区别：
- `StringBuilder` 非线程安全（不能同步访问）
- 在要求线程安全的情况下使用 `StringBuffer`
- `StringBuilder` 有速度优势

```java
public class StringDemo{
    public static void main(String args[]){
        StringBuilder sb = new StringBuilder(10);
        sb.append("Runoob..");
        System.out.println(sb);  
        sb.append("!");
        System.out.println(sb); 
        sb.insert(8, "Java");
        System.out.println(sb); 
        sb.delete(5,8);
        System.out.println(sb);  
    }
}
```
![](https://www.runoob.com/wp-content/uploads/2013/12/2021-03-01-java-stringbuffer.svg)

StringBuffer 常用方法

方法|描述
-|-
public StringBuffer append(String s)|将指定的字符串追加到此字符序列。
public StringBuffer reverse()|将此字符序列用其反转形式取代。
public delete(int start, int end)|移除此序列的子字符串中的字符。
public insert(int offset, int i)|将 int 参数的字符串表示形式插入此序列中。insert(int offset, String str)|将 str 参数的字符串插入此序列中。
replace(int start, int end, String str)|使用给定 String 中的字符替换此序列的子字符串中的字符。

以下方法和 `String` 方法类似

方法|描述
-|-
int capacity()|返回当前容量(所分配的内存最大字符数)。
char charAt(int index)|返回此序列中指定索引处的 char 值。
void ensureCapacity(int minimumCapacity)|确保容量至少等于指定的最小值。
void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)|将字符从此序列复制到目标字符数组 dst。
int indexOf(String str)|返回第一次出现的指定子字符串在该字符串中的索引。
int indexOf(String str, int fromIndex)|从指定的索引处开始，返回第一次出现的指定子字符串在该字符串中的索引。
int lastIndexOf(String str)|返回最右边出现的指定子字符串在此字符串中的索引。
int lastIndexOf(String str, int fromIndex)|返回 String 对象中子字符串最后出现的位置。
int length()|返回长度（字符数）。
void setCharAt(int index, char ch)|将给定索引处的字符设置为 ch。
void setLength(int newLength)|设置字符序列的长度。
CharSequence subSequence(int start, int end)|返回一个新的字符序列，该字符序列是此序列的子序列。
String substring(int start)|返回一个新的 String，它包含此字符序列当前所包含的字符子序列。
String substring(int start, int end)|返回一个新的 String，它包含此序列当前所包含的字符子序列。
String toString()|返回此序列中数据的字符串表示形式。

# 数组

Java 数组就是用来存储固定大小的同类型元素

### 一维数组
- 数组的声明：`dataType[]`, 如声明一个字符串数组: `String[]`
- 数组的创建：`new dataType[arraySize]`, 如创建一个大小为10的数组：`new String[10]`
- 声明和创建数组通常放一起：`String[] strArray = new String[10]`
- 另一种创建数组的方式：`dataType[] arrayName={typeValue1,...}`
- 数组元素是通过索引来访问的，索引从 `0`到`array.length-1`
- 数组的处理是通过 基本循环 或 For-Each 来处理的
- 数组可以作为函数的参数，也可以作为函数的返回值

```java
public class ArrayDemo{

    static String[] reverse(String[] arr){//
        //数组可以作为参数，也可以作为返回值
        String[] result= new String[arr.length];
        for (int i=0,j=arr.length-1;i<arr.length;i++){
            result[i] = arr[j];
        }
        return result ;
    }

    public static void main(String[] args){
        double[] arr1 = {1.2, 3.14, 5.0, 7.7};  //创建数组
        String[] arr2 = new String[5]; //创建数组
        arr2[0]="one";
        arr2[1]="two";
        arr2[2]="three";
        arr2[3]="four";
        arr2[4]="five";
        
        double arrSum=0;
        for (double nub:arr1){
            arrSum+=nub;
        }
        System.out.println(arrSum); //17.04
        String[] result = ArrayDemo.reverse(arr2);
        System.out.println(result[0]); //five
    }
}
```
### 多维数组

- 声明与创建：`dataType[][] arrName = new dataType[length1][;ength2]` 或 `dataType[][] arrName ={{v1,...},{a1,,,}};`
- 多维数组可以在创建时为每一维度指定空间，也可以分别为每个维度分配空间：`dataType[][] arrName = new dataType[length][]; arrName[0] = new dataType[2];  arrName[0] = new dataType[3];` 
- 多维数组通过复合索引来访问：`arr[index1][index2]`

```java
import javax.swing.text.html.HTMLDocument.RunElement;

public class ArrayDemo{
    public static void main(String[] args){
        int[][] arr3= {{1,2},{3,4}};
        int[][] arr5=new int[2][3] //2*3的数组
        for (int[] arr:arr3){
            for (int a:arr){
                System.out.println(a);
            }
        }
        String[][] arr4 = new String[2][];
        arr4[0] = new String[2]; //单独指定空间
        arr4[1] = new String[3];
        arr4[0][0] = new String("Good");
        arr4[0][1] = new String("Luck");
        arr4[1][0] = new String("to");
        arr4[1][1] = new String("you");
        arr4[1][2] = new String("!");
        
    }
}
```

### Arrays 类

`java.util.Arrays` 类能够方便的操作数组，它提供的所有方法都是静态的，具有以下功能：
- 为数组赋值：fill 方法
- 对数组排序：sort 方法
- 比较数组：通过equals方法比较两数组元素是否相等
- 查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

方法|描述
-|-
public static int binarySearch(Object[] a, Object key)|用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点) - 1)。
public static boolean equals(long[] a, long[] a2)
|如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。
public static void fill(int[] a, int val)
|将指定的 int 值分配给指定 int 型数组指定范围中的每个元素(赋值)。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。
public static void sort(Object[] a)
|对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。逆序添加`Collections.reverseOrder()` 参数
public static void copyOf(E[] e, newLength)|为数组扩容，新建一个容量为 newlength 的数组

```java
public class ArrayDemo{

    static String[] reverse(String[] arr){//
        //数组可以作为参数，也可以作为返回值
        String[] result= new String[arr.length];
        for (int i=0,j=arr.length-1;i<arr.length;i++){
            result[i] = arr[j];
        }
        return result ;
    }

    public static void main(String[] args){
        double[] arr1 = {1.2, 3.14, 5.0, 7.7};
        double[] arr6 = new double[4];
        arr6[0] = 1.2;
        arr6[1] = 3.14;
        arr6[2] = 5.0;
        arr6[3] = 7.7;
        System.out.println(Arrays.equals(arr1, arr6));//比较元素是否相等（顺序）
        Arrays.fill(arr6,1,3,2); //arr6[1:3]的元素赋值为2
        Arrays.sort(arr1); //排序
        for (double s:arr1){
            System.out.println(s);
        }
    }
}
```

# Java 日期时间

### Date 类
java.util 中提供了 `Date` 类来封装当前的日期和时间。
`Date` 类提供了两个构造函数来初始化日期：
- Date()
- Date(long millisec)：该参数是1970年1月1日起的毫秒数

以下是 `Date` 对象一些常用的方法：

方法| 描述
-|-
boolean after(Date date)|Data 对象在指定日期之前返回 true，否则返回 false
boolean before(Date date)|Date对象在指定日期之前返回true,否则返回false
Object clone()|返回此对象的副本
int compareTo(Date date)|比较当调用此方法的Date对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数
int compareTo(Object obj)|若obj是Date类型则操作等同于compareTo(Date) 。否则它抛出ClassCastException。
boolean equals(Object date)|当调用此方法的Date对象和指定日期相等时候返回true,否则返回false。
long getTime()|返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。
int hashCode()|返回此对象的哈希值
void setTime(long time)|用自1970年1月1日00:00:00 GTM 以后 time 毫秒数设置时间和日期。
String toString()|把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)。

```java
import java.util.Date;

public class DateDemo {
    public static void main(String[] args){
        Date date1 = new Date();
        Date date2 = new Date(19879699);
        Date date3 = new Date();
        date3.setTime(162143562);

        System.out.println(date1); //Wed May 19 22:40:43 GMT+08:00 2021
        System.out.println(date2);//Thu Jan 01 13:31:19 GMT+08:00 1970
        System.out.println(date3); //Tue Jan 20 02:23:55 GMT+08:00 1970
        System.out.println(date1.after(date2)); //true
        System.out.println(date1.equals(date1.clone())); //true
        System.out.println(date1.compareTo(date2)); //1
        System.out.println(date1.getTime()); //1621435936340
        System.out.println(date1.hashCode()); //-2062393947
        System.out.println(date1.toString()); //Wed May 19 22:40:43 GMT+08:00 2021

    }
}

```

### 日期的比较：
- 使用 `getDate()`获取两个日期（1970.1.1 以来的毫秒数），然后比较它们
- 使用 `after()`、`before()`、`equals()` 方法
- 使用 `compareTo()` 方法，它是由 `Comparable` 接口定义的，`Date` 类实现了这个接口

### 日期格式化：
#### 格式化编码

以下字符用来指定时间格式：
字母|描述|示例
-|-|-
G|纪元标记|AD
y|四位年份|2001
M|月份|July or 07
d|一个月的日期|10
h|A.M./P.M. (1~12)格式小时|12
H|一天中的小时 (0~23)|22
m|分钟数|30
s|秒数|55
S|毫秒数|234
E|星期几|Tuesday
D|一年中的日子|360
F|一个月中第几周的周几|2 (second Wed. in July)
w|一年中第几周|40
W|一个月中第几周|1
a|A.M./P.M. 标记|PM
k|一天中的小时(1~24)|24
K|A.M./P.M. (0~11)格式小时|10
z|时区|Eastern Standard Time
'|文字定界符|Delimiter
"|单引号|`

#### 使用 SimpleDateFormat 格式化日期
语法：
> SimpleDateFormat ft = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");

```java
Date date = new Date();
SimpleDateFormat ft = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
System.out.println(ft.format(date)); //2021-05-19 11:04:00
```

#### 使用 printf 格式化日期

printf 使用两个字母格式来格式化日期和时间，它以 `%t` 开头，并以日期转换符结尾

##### 日期转换符说明

转换符|说明|示例
-|-|-
a|星期简称|Sun 周六
A|星期全程|Sunday 星期六
b|月份简称|Jan 5月
B|月份全称|January 五月
c|包括全部日期和时间信|星期六 十月 27 14:21:20 CST 2007
C|年的世纪部分, 格式为两位数|如 19
d|两位的日期格式, 从01到31|05 25
D|"月/日/年"格式|10/27/07
e|没有前导0的日期, 从1到31|5 25
F|"年-月-日"格式|2007-10-27
H|24小时制的小时, 从00到23|01 15
I|12小时制的小时, 从01到12|07
j|带前导0的年中的日期(一年的第几天)|005 256
k|没有前导0的24小时制, 从0到23|5 15
l|没有前导0的12小时制, 从1到12|5 10
m|带前导0的月份, 从01到12
M|带前导0的分钟, 从00到59|23
N|带前导0的9位纳秒数, 从000000000到999999999|
p|和区域相关的 am 或 pm 标记
Q|1970年1月1日00:00:00 UTC 以来的毫秒
r|"HH:MM:SS PM"格式（12时制）|02:25:51 下午
R|"HH:MM"格式（24时制）|14:28
s|1970年1月1日00:00:00 UTC以后的秒数。
S|2位数字格式的秒，从00到60。60需要支持闰秒。
T|"HH:MM:SS"格式（24时制）|14:28:16
Y|4位的年份格式，从0000到9999。
y|2位的年份格式，从00到99。
Z|时区缩写，如：UTC, PST。
z|与GMT的时区偏移量，如 -0800。

```java
Date date = new Date();
System.out.printf("全部日期和时间信息：%tc",date); //三 5月 19 23:15:38 GMT+08:00 2021
System.out.printf("年-月-日格式：%tF",date); //2021-05-19
System.out.printf("月/日/年格式：%tD",date); //05/19/21
System.out.printf("HH:MM:SS PM格式（12时制）：%tr",date); //11:15:38 下午
System.out.printf("HH:MM:SS格式（24时制）：%tT",date); //23:15:38
System.out.printf("HH:MM格式（24时制）：%tR",date); //23:15
```

 格式化需要重复使用时间时，可以用以下两个方法：
 - 使用索引，索引必须在 `%` 后，且必须以 `$` 结束：如 `%1$t`
 - 使用 `<` 标识，表明先前使用的日期再次使用

 ```java
Date date = new Date();
System.out.printf("%1$s %2$tF %2$tR", "Due date:", date);
System.out.printf("%s %tF %<tR", "Due date:", date);
 ```

通过日期转换符可以将日期转换为指定格式得字符串

```java
String str=String.format(Locale.US,"英文月份简称：%tb",date);       
System.out.println(str);                                                                              
System.out.printf("本地月份简称：%tb%n",date);  
//B的使用，月份全称  
str=String.format(Locale.US,"英文月份全称：%tB",date);  
System.out.println(str);  
System.out.printf("本地月份全称：%tB%n",date);  
//a的使用，星期简称  
str=String.format(Locale.US,"英文星期的简称：%ta",date);  
System.out.println(str);  
//A的使用，星期全称  
System.out.printf("本地星期的简称：%tA%n",date);  
//C的使用，年前两位  
System.out.printf("年的前两位数字（不足两位前面补0）：%tC%n",date);  
//y的使用，年后两位  
System.out.printf("年的后两位数字（不足两位前面补0）：%ty%n",date);  
//j的使用，一年的天数  
System.out.printf("一年中的天数（即年的第几天）：%tj%n",date);  
//m的使用，月份  
System.out.printf("两位数字的月份（不足两位前面补0）：%tm%n",date);  
//d的使用，日（二位，不够补零）  
System.out.printf("两位数字的日（不足两位前面补0）：%td%n",date);  
//e的使用，日（一位不补零）  
System.out.printf("月份的日（前面不补0）：%te",date);  
```

#### 字符串解析为时间格式
- SimpleDateFormat 类提供 parse() 方法，按照给定的 SimpleDateFormat 对象的格式来解析字符串
- 如：SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 

```java
import java.util.*;
import java.text.*;
  
public class DateDemo {
 
   public static void main(String args[]) {
      SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 
 
      String input = args.length == 0 ? "1818-11-11" : args[0]; 
 
      System.out.print(input + " Parses as "); 
 
      Date t; 
 
      try { 
          t = ft.parse(input); 
          System.out.println(t); 
      } catch (ParseException e) { 
          System.out.println("Unparseable using " + ft); 
      }
   }
}
```

### Java 休眠（sleep）
#### Thread.sleep
`Thread.sleep()` 会阻塞当前线程一段时间，让出cpu，给其他线程执行的机会，休眠时间单位为 ms

```java
try { 
    System.out.println(new Date( ) + "\n"); 
    Thread.sleep(1000*3);   // 休眠3秒
    System.out.println(new Date( ) + "\n"); 
} catch (Exception e) { 
    System.out.println("Got an exception!"); 
}
```

#### System.currentTimeMillis
`System.currentTimeMillis` 表示当前时间的毫秒数，通过该方法能够更精确的测量时间

```java
import java.util.*;
  
public class DiffDemo {
 
   public static void main(String args[]) {
      try {
         long start = System.currentTimeMillis( );
         System.out.println(new Date( ) + "\n");
         Thread.sleep(5*60*10);
         System.out.println(new Date( ) + "\n");
         long end = System.currentTimeMillis( );
         long diff = end - start;
         System.out.println("Difference is : " + diff);
      } catch (Exception e) {
         System.out.println("Got an exception!");
      }
   }
}
```

### Calendar 类
- `Calendar` 类 比 `Date` 类功能更强大，实现也更复杂
- `Calendar` 类是抽象类，使用时需是实现特定子类的对象：`Calendar.getInstance()`

**创建 `Calendar` 对象**

> `Calendar ca = Calendar.getInstance();`：默认为当前时间


**使用 `set()`/`add()`设置时间**

> `public final void set(int year,int month,int date)`：设置全部时间
> `public void set(int field, int value)`：单独设置年月日
> `public void add(int field, int value)`：单独添加年月日

**使用 `get()` 方法获取时间**

> `public int get(int field)` 

可获取的常量字段：
常量|描述
-|-
Calendar.YEAR|年份
Calendar.MONTH|月份
Calendar.DATE|日期
Calendar.AM_PM|上午(0)还是下午(1)
Calendar.HOUR|12小时制的小时
Calendar.HOUR_OF_DAY|24小时制的小时
Calendar.MINUTE|分钟
Calendar.SECOND|秒
Calendar.MILLISECOND|毫秒
Calendar.DAY_OF_WEEK|当周的第几天
Calendar.DAY_OF_MONTH|当月的第几天
Calendar.DAY_OF_YEAR|当年的第几天
Calendar.DAY_OF_WEEK_IN_MONTH|当前周N是当年的第几个周N
Calendar.WEEK_OF_MONTH|当月的第几周
Calendar.WEEK_OF_YEAR|当年的第几周

```java
Calendar ca1 = Calendar.getInstance();
Calendar ca2 = Calendar.getInstance();
Calendar ca3 = Calendar.getInstance();
ca2.set(2021,05,20); //set直接设置日期
ca3.set(Calendar.YEAR, 2020); //set分别设置日期
ca3.add(Calendar.MONTH, 6); //add 添加日期
ca3.add(Calendar.DATE, 21); //add 添加日期

System.out.println("当前日期：" + ca1.get(Calendar.DATE)); //20
System.out.println("当前月份：" + ca1.get(Calendar.MONTH)); //4
System.out.println("当前年份：" + ca1.get(Calendar.YEAR)); //2021
System.out.println("当周的第几天：" + ca1.get(Calendar.DAY_OF_WEEK));//5
System.out.println("当月的第几天：" + ca1.get(Calendar.DAY_OF_MONTH)); //20
System.out.println("当年的第几天：" + ca1.get(Calendar.DAY_OF_YEAR)); //140
System.out.println("当前周N是当年的第几个周N" + ca1.get(Calendar.DAY_OF_WEEK_IN_MONTH)); //3
System.out.println("当月的第几周：" + ca1.get(Calendar.WEEK_OF_MONTH)); //4
System.out.println("当年的第几周：" + ca1.get(Calendar.WEEK_OF_YEAR)); //21

System.out.println("12时制的小时：" + ca1.get(Calendar.HOUR));//4
System.out.println("24时制的小时：" + ca1.get(Calendar.HOUR_OF_DAY));//16
System.out.println("分钟：" + ca1.get(Calendar.MINUTE));//5
System.out.println("秒：" + ca1.get(Calendar.SECOND));//39
System.out.println("毫秒：" + ca1.get(Calendar.MILLISECOND)); //152
System.out.println(ca2);
System.out.println(ca3);
```
### GregorianCalendar类

- `Calendar` 类实现了公历日历，`GregorianCalendar` 是 `Calendar` 类的一个具体实现。
- `Calendar` 的 `getInstance()`方法返回一个默认用当前的语言环境和时区初始化的`GregorianCalendar` 对象。`GregorianCalendar`定义了两个字段：`AD` 和 `BC`。这是代表公历定义的两个时代

`GregorianCalendar` 对象的几个构造方法：

序号|构造函数和说明
-|-
GregorianCalendar()|在具有默认语言环境的默认时区内使用当前时间构造一个默认的 GregorianCalendar。
GregorianCalendar(int year, int month, int date)|在具有默认语言环境的默认时区内构造一个带有给定日期设置的 GregorianCalendar
GregorianCalendar(int year, int month, int date, int hour, int minute)|为具有默认语言环境的默认时区构造一个具有给定日期和时间设置的 GregorianCalendar。
GregorianCalendar(int year, int month, int date, int hour, int minute, int second)|为具有默认语言环境的默认时区构造一个具有给定日期和时间设置的 GregorianCalendar。
GregorianCalendar(Locale aLocale)|在具有给定语言环境的默认时区内构造一个基于当前时间的 GregorianCalendar。
GregorianCalendar(TimeZone zone)|在具有默认语言环境的给定时区内构造一个基于当前时间的 GregorianCalendar。
GregorianCalendar(TimeZone zone, Locale aLocale)|在具有给定语言环境的给定时区内构造一个基于当前时间的 GregorianCalendar。

GregorianCalendar 类提供的一些有用的方法列表：

序号	方法和说明
--|--
void add(int field, int amount)|根据日历规则，将指定的（有符号的）时间量添加到给定的日历字段中。
protected void computeFields()|转换UTC毫秒值为时间域值
protected void computeTime()|覆盖Calendar ，转换时间域值为UTC毫秒值
boolean equals(Object obj)|比较此 GregorianCalendar 与指定的 Object。
int get(int field)|获取指定字段的时间值
int getActualMaximum(int field)|返回当前日期，给定字段的最大值
int getActualMinimum(int field)|返回当前日期，给定字段的最小值
int getGreatestMinimum(int field)| 返回此 GregorianCalendar 实例给定日历字段的最高的最小值。
Date getGregorianChange()|获得格里高利历的更改日期。
int getLeastMaximum(int field)|返回此 GregorianCalendar 实例给定日历字段的最低的最大值
int getMaximum(int field)|返回此 GregorianCalendar 实例的给定日历字段的最大值。
Date getTime()|获取日历当前时间。
long getTimeInMillis()|获取用长整型表示的日历的当前时间
TimeZone getTimeZone()|获取时区。
int getMinimum(int field)|返回给定字段的最小值。
int hashCode()|重写hashCode
boolean isLeapYear(int year)|确定给定的年份是否为闰年。
void roll(int field, boolean up)|在给定的时间字段上添加或减去（上/下）单个时间单元，不更改更大的字段。
void set(int field, int value)|用给定的值设置时间字段。
void set(int year, int month, int date)|设置年、月、日的值。
void set(int year, int month, int date, int hour, int minute)|设置年、月、日、小时、分钟的值。
void set(int year, int month, int date, int hour, int minute, int second)|设置年、月、日、小时、分钟、秒的值。
void setGregorianChange(Date date)|设置 GregorianCalendar 的更改日期。
void setTime(Date date)|用给定的日期设置Calendar的当前时间。
void setTimeInMillis(long millis)|用给定的long型毫秒数设置Calendar的当前时间。
void setTimeZone(TimeZone value)|用给定时区值设置当前时区。
String toString()|返回代表日历的字符串。

# Java 正则表达式

# Java 方法

### 什么是方法？

Java方法是语句的集合，它们在一起执行一个功能。

- 方法是解决一类问题的步骤的有序组合
- 方法包含于类或对象中
- 方法在程序中被创建，在其他地方被引用

#### 优点：
1. 使程序变得更简短而清晰。
2. 有利于程序维护。
3. 可以提高程序开发的效率。
4. 提高了代码的重用性。

#### 命名规则：
- 小写字母开头，后续字母首字母大写，且不使用连接符
- 

### 方法的定义
#### 语法：

```java
修饰符 返回值类型 方法名(参数类型 参数名){
    ...
    方法体
    ...
    return 返回值;
}
```

#### 组成部分：
- 修饰符：修饰符，这是可选的，告诉编译器如何调用该方法。定义了该方法的访问类型。
- 返回值类型 ：方法可能会返回值。returnValueType 是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。在这种情况下，returnValueType 是关键字void。
- 方法名：是方法的实际名称。方法名和参数表共同构成方法签名。
- 参数类型：参数像是一个占位符。当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。
- 方法体：方法体包含具体的语句，定义该方法的功能

![](https://www.runoob.com/wp-content/uploads/2013/12/D53C92B3-9643-4871-8A72-33D491299653.jpg)

```java
public class TestMax {
   /** 主方法 */
   public static void main(String[] args) {
      int i = 5;
      int j = 2;
      int k = max(i, j);
      System.out.println( i + " 和 " + j + " 比较，最大值是：" + k);
   }
 
   /** 返回两个整数变量较大的值 */
   public static int max(int num1, int num2) {
      int result;
      if (num1 > num2)
         result = num1;
      else
         result = num2;
 
      return result; 
   }
}
```

### 其他：
- 方法通过值传递参数
- 方法内部定义的变量为局部变量，局部变量必须声明才能使用
- 方法的参数范围覆盖整个方法，实际也是一个局部变量
- 一个方法中只能有一个可变参数(指定参数类型后加`...`)，且必须是方法的最后一个参数。


# Java 流(Stream)、文件(File)和IO

### 控制台的输入、输出

Java 中控制台的输入由 System.in 控制
为了获得一个稳定的绑定到控制台的字符流，使用 `BufferedReader` 对象包装 `System.in` 来创建一个字符流：

> BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

#### 读取控制台输入

- `int read() throws IoException` 用来读取单个字符
- `String readline() throws IoException` 用来读取字符串

```java
import java.io.*;


public class IoDemo{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        // System.out.println(br.read());  //读取单个字符
        System.out.println(br.readLine());  //读取字符串
    }
}
```

#### 控制台输出
- `print()/println()`：这些方法由 `PrintStream` 定义，`Sys.out` 是对该类的一个引用。
- `write()`：`PrintStream` 继承了 `OutputStream` 类，并且实现了`write()`方法。

```java
import java.io.*;
//演示 System.out.write().
public class WriteDemo {
    public static void main(String[] args) {
        int b;
        b = 'A';
        System.out.write(b);
        System.out.write('\n');
    }
}
```

### 文件读写

流实际是一个数据序列。
输入流用于从源读取数据，输出流用于向目标写数据。
下图为输入流和输出流的类层次图：
![image](https://www.runoob.com/wp-content/uploads/2013/12/iostream2xx.png)

#### FileInputStream

该流用于从文件中读取数据
- 使用 文件名 来创建一个输入流对象读取数据：`InputStream f=new FileInputStream("c:/java/hello");`
- 使用 文件对象 创建一个输入流对象读取数据：`File f = new File("c:/java/hello"); InputStream in = new FileInputStream(f);`

`InputStream` 对象常用方法：

方法|描述
-|-
public void close() throws IOException{}|关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常。
protected void finalize()throws IOException{}|这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出IOException异常。
public int read(int r)throws IOException{}|这个方法从 InputStream 对象读取指定字节的数据。返回为整数值。返回下一字节数据，如果已经到结尾则返回-1。
public int read(byte[] r) throws IOException{}|这个方法从输入流读取r.length长度的字节。返回读取的字节数。如果是文件结尾则返回-1。
public int available() throws IOException{}|返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节数。返回一个整数值。

```java
import java.io.*;

public class IoDemo{
    public static void main(String[] args) throws IOException{
        InputStream f = new FileInputStream("E:/Markdown/待看.txt");
        int size = f.available(); //查看文件字节数
        for (int i=0;i<size;i++){
            System.out.print((char)f.read());
        }
    }
}
```

#### FileOutputStream

该流用来创建一个文件并向文件中写数据（目标文件不存在时会自动创建该文件）
- 使用文件名创建一个输出流对象：`OutputStream f = new FileOutputStream("C:/java/hello")`
- 使用文件对象创建一个输出流对象：`File f = new File("C:/java/hello"); OutputStream f = new FileOutputStream(f);`

`FileOutputStream` 常用方法：
方法|描述
--|--
public void close() throws IOException{}|关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常。
protected void finalize()throws IOException {}|这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出IOException异常。
public void write(int w)throws IOException{}|这个方法把指定的字节写到输出流中。
public void write(byte[] w)|把指定数组中w.length长度的字节写到OutputStream中。

#### 编码问题（待补充）

# Java Scanner 类（待补充）

# Java 异常处理 (待补充)

Java 中异常分为三类:

- 检查性异常：
- 运行时异常：
- 错误：

# Java 继承

#### 继承的特性：
继承就是子类继承父类的特称和行为，从而拥有父类的属性和方法。
- java 继承不支持多继承，但支持多重继承
- 子类可以拥有父类 非 private 的属性和方法
- 子类可以拥有自己的属性和方法，可以对父类进行扩展
- 子类可以重写父类的方法
- 继承提高了类之间的耦合

![](https://www.runoob.com/wp-content/uploads/2013/12/java-extends-2020-12-08.png)

#### 继承关键字：

- `extends`：单继承，一个子类只能有一个父类
- `implements`：变相的使 java 具有多继承的特性，适用于类继承接口，多个接口间用 `,` 隔开
- `super`：用来引用当前对象的父类，实现对父类成员的访问
- `this`：对象本身，指向自身的引用
- `final`：声明类不能被继承，声明方法不能被子类重写

##### extends 语法：

> class subClass extends superClass{}

```java

class Animal{
    private String name;
    private int id;
    public Animal(String myName, int myId){
        this.name = myName;
        this.id = myId;
    }

    public void eat(){
        System.out.println(this.name + "正在吃");
    }
    public void sleep(){
        System.out.println(this.name + "正在睡");
    }

    public void introduction(){
        System.out.println("大家好！我是"         + id + "号" + name + ".");
    }
}

class Cat extends Animal{
    // 继承 Animal 类
    public Cat(String myName, int myId){
        super(myName,myId); //引用父类构造方法
    }

    public void introduction(){
        super.introduction(); //引用父类方法
    }
    }
}
```

#### implements 语法

> class subClass implements superClass1,superClass2{}

```java
public interface A {
    public void eat();
    public void sleep();
}
 
public interface B {
    public void show();
}
 
public class C implements A,B {
}
```

#### 构造方法
