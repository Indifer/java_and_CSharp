# Java & C#
最近在学习 java，总结些东西方便记忆，后续可以提供给会 C# 开发，并想学习 Java 的同学们参考。
很多语法都是简单的星星点点的示例，要好好掌握这门语言，还是需要自己系统去详细学习下。
**切记，作为一名研发，不要因为语言而限制住你的发展!~~**

## 基本语法的区别

### for-each 循环

```C#
//C#
int[] array = new int[] { 1, 2, 3, 7, 8, 9 };
foreach (int item in array)
{
    Console.WriteLine(item);
}
```

```Java
//Java
int[] array = new int[]{1, 2, 3, 7, 8, 9};
for (int item : array) {
    System.out.println(item);
}
```

### 访问修饰符

- 需要注意的是 protected，java 和 C# 是有区别的。

C# :

访问权限     | 说明
-------     | --
public | 访问不受限制
protected | 访问限于包含类或派生自包含类的类型
Internal | 访问限于当前程序集
protected internal | 访问限于当前程序集或派生自包含类的类型
private | 访问限于包含类

Java :

访问权限     | 同类 | 同包不同类（不含子类） | 同包子类 | 不同包不同类（不含子类） | 不同包子类
-------     | -- | -- | --- | ----- | ----
public      | ∨ | ∨ | ∨ | ∨ | ∨
protected   | ∨ | ∨ | ∨ | × | ∨
无(default) | ∨ | ∨ | ∨ | × | ×
private     | ∨ | × | × | × | ×

### Java 的静态块和 C# 的静态构造函数

C#只允许一个无参构造函数；java可以有多个静态块，并且按照指定的顺序执行。

```C#
//C#
class Birds
{
    public static bool isVertebrate;
    public static bool isOvipara;

    static Birds()
    {
        isVertebrate = true;
        isOvipara = true;
    }
}
```

```Java
//Java
class Birds {
    public static Boolean isVertebrate;
    public static Boolean isOvipara;

    static{
        isVertebrate = true;
    }

    static{
        isOvipara = true;
    }
}
```

### 继承、构造函数
C# 的静态字段和常量都可以被子类访问，java 的静态 final 一样可以被子类访问。
注意的是 Java 的构造函数里的 this、 super 必须在代码第一行。

```C#
//C#
class Birds
{
    public const int WingsNumber = 2;
    public static readonly int PawsNumber = 2;

    protected string color = null;
    protected bool canSwim;

    public Birds()
        : this(null)
    {
    }

    public Birds(String color)
    {
        this.color = color;
        this.canSwim = false;
    }

    public Birds(String color, Boolean canSwim)
    {
        this.color = color;
        this.canSwim = canSwim;
    }

}

class Duck : Birds
{

    public Duck()
        : this(null)
    {
        //some code
    }

    public Duck(String color)
        : this(color, true)
    {
        //some code
    }

    public Duck(String color, Boolean canSwim)
        : base(color, canSwim)
    {
        //some code
    }
}
```

```Java
//Java
class Birds {
    public final static int WingsNumber = 2;
    public final int PawsNumber = 2;

    protected String color = null;
    protected Boolean canSwim ;

    public Birds() {
        this(null);
    }

    public Birds(String color) {
        this.color = color;
        this.canSwim = false;
    }

    public Birds(String color, Boolean canSwim) {
        this.color = color;
        this.canSwim = canSwim;
    }
}

class Duck extends Birds{

    public Duck() {
        this(null);
        //some code
    }

    public Duck(String color) {
        this(color, true);
        //some code
    }

    public Duck(String color, Boolean canSwim) {
        super(color, canSwim);
        //some code
    }
}
```

### C# const 和 static 字段
在 C# 中，无法通过 class 的实例来访问静态字段和常量

```C#
//C#
class Birds
{
    public const int WingsNumber = 2;
    public static readonly int PawsNumber = 2;
}

public class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine($"wings number:{Birds.WingsNumber}");
        Console.WriteLine($"paws number:{Birds.PawsNumber}");
        //Console.WriteLine($"paws number:{new Birds().PawsNumber}");  //error
    }
}
```

### Java final字段
在 Java 中，static final 字段，可以被 class 的实例访问（这种方式并不推荐）。

```Java
//Java
class Birds {
    public static final int WingsNumber = 2;
    public final int PawsNumber = 2;
}

public class Main {

    public static void main(String[] args) {

        System.out.println("wings number:" + new Birds().WingsNumber);
        System.out.println("wings number:" + Birds.WingsNumber);

        System.out.println("paws number:" + new Birds().PawsNumber);
        //System.out.println("paws number:" + Birds.PawsNumber);  //error
    }
}
```

### 嵌套类
Java 和 C# 的嵌套类是完全不一样的

C# 的嵌套类的最大用途是控制访问可见性，使用基本上和普通类一样，没什么滑头
```C#
//C#
public class Outer
{
    private int size = 0;
    public class Inner
    {
        public void someMethod()
        {
            //size++;  //error
        } 
    }
}
public class Program
{
    static void Main(string[] args)
    {
        Outer.Inner inner = new Outer.Inner();
        Outer outer = new Outer();
    }
}          
```
Java 中的，内部类总是与实例相关联，下面在创建 Inner 类实例的时候用的是 Outer 的实例，outer.new Inner()，所以 Inner 类的实例可以访问 Inner 类的内部字段。
```Java
//Java
public class Outer
{
    private int size = 0;
    public class Inner
    {
        public void someMethod()
        {
            size++;
        }
    }
}
public class Main {

    public static void main(String[] args) {

        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();

        inner.someMethod();
    }
}
```
java里，嵌套类同名字段作用域
```Java
//Java
public class Outer {
    private int size = 0;

    public Inner createInner(){
        return this.new Inner();
    }

    public int getSize(){
        return size;
    }

    public class Inner {

        private int size = 1;
        public void someMethod() {
            size++;
            System.out.println(size);   	//2
            System.out.println(Outer.this.size);  //0
        }
    }
}
public class Main {

    public static void main(String[] args) {

        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();

        inner.someMethod();
    }
}
```

### 静态类
- Java 中一个普通类是不可以定义为 static 的，只有内部类可以为静态类。 
   而 C# 中是可以直接定义一个静态类的。

- Java 中的静态内部类中可以定义静态成员也可以定义非静态成员，静态成员可以用类名直接访问，
   而非静态成员只有 new 一个静态内部类的实例才可以访问到。
   Java 静态内部类中只能访问外部类的静态成员，因为如果可以访问外部类的非静态成员，这时候外部类
   可能还没有实例化，这时就会出错。

- c# 中的静态类中只能定义静态成员，且不能 new 一个静态类的实例 。普通类中的静态成员只能用类名进行引用，不能用类的实例进行引用。

下面是java静态类和非静态类的示例
```Java
//Java
public class Outer {
    private int size = 0;
    private static int staticSize = 3;

    public Inner createInner(){
        return this.new Inner();
    }

    public int getSize(){
        return size;
    }

    public class Inner {

        private int size = 1;
        public void someMethod() {
            size++;
            System.out.println(size);
            System.out.println(Outer.this.size);
        }
    }

    public static class StaticInner{

        public void someMethod() {
            System.out.println(staticSize);
        }
    }
}
public class Main {

    public static void main(String[] args) {

        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();

        inner.someMethod();
        Outer.StaticInner staticInner = new Outer.StaticInner();
        staticInner.someMethod();
    }
}
```

### Java 局部类

局部类的重要特性
- 局部类仅在定义自身的代码块中是可见和可用的。
- 除了

下面是 Java 中局部类示例，Local 就是一个局部类（ps：有点像 javascript 里的 funtion 玩法）

```Java
//Java
public class Outer {

    public class Inner {

        private int size = 1;
        public void someMethod() {

            int y = 5;
            class Local{

                int x = 6;
                public void someMethod(){
                    System.out.println(size);
                    System.out.println(y);
                    System.out.println(x);
                }
            }

            new Local().someMethod();
        }
    }
}
```

### Java 匿名类

```Java
//Java
abstract class Person {
    public abstract void eat();
}

class Child extends Person {
    public void eat() {
        System.out.println("eat something");
    }
}

public class Main {
    public static void main(String[] args) {

        Person p1 = new Child();
        p1.eat();

        Person p2 = new Person() {
            public void eat() {
                System.out.println("eat something");
            }
        };
        p2.eat();
    }
}
```

C# 中有匿名对象，匿名委托，还有 Action 和 Func 等委托对象，这里就不列举了。

### try-catch-finally

Java 的 finally语法存在设计缺陷。 finally 块允许包含一条跳转语句，break、 continue 或 return。 finally 如果有 return 会覆盖catch 里的 throw，同样如果 finally 里有 throw 会覆盖 catch 里的 return。

```Java
//Java
class Test{

    public Boolean test() throws Exception{


        boolean ret = true;
        try{
            String path = "";
            File file = new File(path);
            file.createNewFile();
            return ret;
        }
        catch (IOException ex){
            System.out.println("catch:" + ex.getMessage());
            ret = false;
            throw ex;
        }
        finally {
            System.out.println("finally");
        }
    }
}
public class Main {

    public static void main(String[] args) {

        boolean res = true;
        try{
           res = new Test().test();
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(res);
    }
}
```
```console
finally
true
Java.io.IOException: 系统找不到指定的路径。
	at Java.io.WinNTFileSystem.createFileExclusively(Native Method)
	at Java.io.File.createNewFile(File.Java:1012)
	at Test.test(Main.Java:126)
	at Main.main(Main.Java:59)
```

下面示例中, finally 中的 return 覆盖了 cache 里的 throw，异常丢了。
```Java
//Java
class Test{

    public Boolean test() throws Exception{


        boolean ret = true;
        try{
            String path = "";
            File file = new File(path);
            file.createNewFile();
            return ret;
        }
        catch (IOException ex){
            System.out.println("catch:" + ex.getMessage());
            ret = false;
            throw ex;
        }
        finally {
            System.out.println("finally");
            return ret;
        }
    }
}
public class Main {

    public static void main(String[] args) {

        boolean res = true;
        try{
           res = new Test().test();
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(res);
    }
}
```
```console
catch:系统找不到指定的路径。
finally
false
```
下面示例中, finally 中的 throw 覆盖了 cache 里的 return。
```Java
//Java
class Test{

    public Boolean test() throws Exception{


        boolean ret = true;
        try{
            String path = "";
            File file = new File(path);
            file.createNewFile();
            return ret;
        }
        catch (IOException ex){
            System.out.println("catch:" + ex.getMessage());
            return false;
        }
        finally {
            System.out.println("finally");
            throw new Exception("throw new exception");
        }
    }
}
public class Main {

    public static void main(String[] args) {

        boolean res = true;
        try{
           res = new Test().test();
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(res);
    }
}
```
```console
catch:系统找不到指定的路径。
finally
true
Java.lang.Exception: throw new exception
	at Test.test(Main.Java:136)
	at Main.main(Main.Java:59)
```
C# 通过禁止在 finally 块中使用任何跳转语句来避免 Java 中的缺陷。
```C#
//C#
class Program
{
    static void Main(string[] args)
    {
        switch(1)
        {
            case 1:
                try { }
                catch (Exception ex1) { }
                finally
                {
                    break; //error 控制不能离开 finally 子句主体
                }
        }

        try { }
        catch (Exception ex1) { }
        finally
        {
            return; //error 控制不能离开 finally 子句主体
        }
    }
}

```

### 枚举类型
J2SE 5.0 引入了 enum，Java 的 enum 其实就是类，可以添加方法、字段，实现接口等。
```Java
//Java
enum Converter {

    KG("KG") {
        @Override
        double performConversion(double f) {
            return f *= 0.45;
        }
    },
    CARAT("carat") {
        @Override
        double performConversion(double f) {

            return f *= 2267.96;
        }

    },
    GMS("gms") {
        @Override
        double performConversion(double f) {
            return f *= 453.59;
        }
    }
    ;

    private final String symbol;

    Converter(String symbol) {
        this.symbol = symbol;
    }

    @Override
    public String toString() {
        return symbol.toString();
    }

    abstract double performConversion(double f);
}

public class Main {
    public static void main(String[] args) {

        double d = 11.11;
        for (Converter conv : Converter.values()) {
            System.out.println(conv.toString() + ":" + conv.performConversion(d));
        }

    }
}
```
C# 里的 enum 是值类型，并且不能有构造函数，方法等。
```C#
//C#
class Program
{
    static void Main(string[] args)
    {
        foreach (Color e in Enum.GetValues(typeof(Color)))
        {
            Console.WriteLine($"{e}:{(int)e}");
        }
    }
}
public enum Color
{
    Red = 0,
    Yellow = 1,
    Blue = 3
}

```

### Java 的 annotation (注解)和 C# (Attribute) 的特性

Java的注解规则：
- 使用参数的类型声明方法
- 方法不应包含任何参数
- 方法不应包含任何throws子句
- 方法的返回类型应该为：基本类型、字符串、类、枚举、这些类型的数组

```Java
//Java
public @interface Task {
    int value() default 0;
    String desc() default "";
}
public class Birds {
    @Task
    public Birds() {
        this(null);
    }

    @Task(123)
    public Birds(String color) {
    }

    @Task(value = 123, desc = "456")
    public Birds(String color, Boolean canSwim) {
    }
}
```

C# 的特性就是class，以Attribute为结尾命名，派生于Attribute类
```C#
//C#
public class MyAttribute : Attribute
{
    public int ID { get; set; }

    public string Desc { get; set; }

    public void Say()
    {
        System.Console.WriteLine($"ID:{ID}, Desc:{Desc}");
    }
}
class Birds
{
    public Birds()
        : this(null)
    {
    }

    [My]
    public Birds(String color)
    {
        this.color = color;
        this.canSwim = false;
    }

    [My(ID = 123, Desc = "456")]
    public Birds(String color, Boolean canSwim)
    {
        this.color = color;
        this.canSwim = canSwim;
    }
}

```

具体使用都是基于反射原理，这里就不详细说明了。