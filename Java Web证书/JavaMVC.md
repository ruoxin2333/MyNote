### 方法
定义：
https://www.runoob.com/java/java-methods.html
```java
package step1;

public class HelloWorld {
	/********** Begin **********/
	//定义一个方法，用来和老师打招呼
	public static void hellow(){
		System.out.print("hello teacher!");
	}
	/********** End **********/
	public static void main(String[] args) {
		/********** Begin **********/  
		HelloWorld.hellow();
		//调用方法
		/********** End **********/
	}
}
```
### 对象
定义：
https://www.runoob.com/java/java-object-classes.html
```java
package step1;

public class Test {
    public static void main(String[] args) {
        /********** Begin **********/
        //创建Dog对象
        //设置Dog对象的属性
        Dog d = new Dog();
        d.name = "五花肉";
        d.color = "棕色";
        d.variety = "阿拉斯加";
        //输出小狗的属性
        System.out.println("名字：" +    d.name  + "，毛色：" + d.color  + "，品种：" +   d.variety);
        //调用方法
        d.eat();
        d.movements();
        /********** End **********/
    }
}
//在这里定义Dog类
/********** Begin **********/
class Dog{
    String name;
    String color;
    String variety;
    public static void eat(){
        System.out.println("啃骨头");
    }
    public static void movements(){
        System.out.print("叼着骨头跑");
    }
}
```
创建方法，设置属性，调用方法

### 构造方法
定义：
https://www.liaoxuefeng.com/wiki/1252599548343744/1260454185794944
>方便在没有创建对象的情况下来进行调用（方法/变量）。
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Xiao Ming", 15);
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}

```
