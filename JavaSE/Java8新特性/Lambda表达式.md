### 为什么需要Lambda表达式

---

在Java中，我们无法将函数作为参数传递给一个方法，也无法声明返回一个函数方法。

Lambda表达式为Java添加了缺失的函数式编程的特性，使我们能将函数当做一等公民看待，`在Java中，Lambda表达式是对象`，他们必须依附于一类特别的对象类型——函数式接口。

下面是一个普通的Swing程序使用匿名内部类与Lambda的写法

```java
JFrame jFrame=new JFrame("My JFrame");
JButton jButton=new JButton("My JButton");

//下面是一个匿名内部类
/*jButton.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Button Pressed!");
    }
});*/

//使用Lambda替换匿名内部类
jButton.addActionListener(event->System.out.println("Button Pressed2"));

//如果有多个执行块，需要用{}包起来
/*jButton.addActionListener(event-> {
    System.out.println("Button Pressed2");
    System.out.println("Button");
});*/

//通过类型推断自动推断出event的类型，因此，上面写是可行的
/*jButton.addActionListener((ActionEvent event)-> {
    System.out.println("Button Pressed2");
});*/

jFrame.add(jButton);
jFrame.pack();
jFrame.setVisible(true);
jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
```

<br>
<br>

### Lambda表达式的基本结构

---

```java
(param1,param2,param3)->{
    
}
```

一个Lambda表达式可以有零个或多个参数

参数的类型既可以明确声明，也可以根据上下文来推断。

所有参数需包含在圆括号内，参数之间用逗号相隔。

空圆括号代表参数集为空。

当只有一个参数，且其类型可推导时，圆括号可以省略。

Lambda表达式的主体可以包含多条语句。

如果Lambda表达式的主体语句只有一条，花括号可以省略，匿名函数的返回类型与该主体表达式一致。

### 遍历数组

---

```java
List<Integer> list=Arrays.asList(1,2,3,4,5);
//最初的遍历集合
for(int i=0;i<list.size();i++){
    System.out.println(list.get(i));
}
System.out.println("--------------");
//Jdk1.5引入增强for循环
for (Integer i:list) {
    System.out.println(i);
}
System.out.println("---------------");
//Java8（forEach方法，使用了匿名内部类）
list.forEach(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) {
        System.out.println(integer);
    }
});
System.out.println("------------------");
//简化
list.forEach(li-> System.out.println(li));
```

上面的Consumer接口，其使用了@FunctionalInterface注解，即函数式接口注解。

@FunctionalInterface是一个通知性的注解类型，用于去表示某一个接口的类型声明旨在成为一个函数式接口。



### 函数式接口

---

1. 如果一个接口只有一个抽象方法（Java8后接口也可以有自己的实现类），那么该接口就是一个函数式接口。
2. 如果我们在某个接口上声明了@FunctionalInterface注解，那么编译器就会按照函数式接口的定义来要求该接口。
3. 如果某个接口只有一个抽象方法，但我们并没有给该接口声明FunctionalInterface注解，那么编译器依旧会将该接口看作是函数式接口。



### Java8中接口与抽象类的区别

---

上面说到，Java8后接口也可以有自己的实现类，那么Java8中接口与抽象类有何区别？

主要的区别在于数据成员和方法的可访问性。

对于接口来说，其字段类型基本都是public static final的。

而抽象类允许非static和非final，同时，方法允许public、private、protected



### 自己实现函数式接口

---

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void method();

    //当加入了第二个抽象方法，程序报错。
    //void method2();

    default void method3(){
        System.out.println("method3");
    }
}
```

对实现的函数式接口进行测试

```java
public class MyInterface{
    public void myTest(MyFunctionalInterface myFunctionalInterface){
        myFunctionalInterface.method();
    }
    public static void main(String[] args) {
        MyInterface myInterface=new MyInterface();
        //使用Lambda表达式进行测试
        myInterface.myTest(()-> System.out.println("Hello World"));
    }
}
```

使用Lambda表达式对接口进行实现

```java
MyFunctionalInterface myFunctionalInterface=()->{
    System.out.println("Hello Hello");
};
myFunctionalInterface.method();
```

