通过流的方式，我们可以更好的操作集合。

流由3部分构成：源、零个或多个中间操作、终止操作

流操作共有两种操作类型，惰性求值、及早求值

对于流来说，总是由零个或多个中间操作（惰性操作）和一个终止操作（及早操作）组成

如果没有终止操作，实际上代码是不会被执行的，当进行了终止操作，代码被执行，结果将被立刻得到。

<br><br>

### 得到Stream的三种方式

---

```java
//第一种方式，通过Stream的静态方法创建
Stream stream1=Stream.of("hello","world","hello world");
//第二种方式，通过数组创建流
String[] array=new String[]{"hello","world","hello world"};
Stream stream2=Stream.of(array);
Stream stream3= Arrays.stream(array);
//第三种方式，通过集合创建
List<String> list=Arrays.asList(array);
Stream stream4=list.stream();
```

<br><br>

### 对Stream的简单使用

---

```java
IntStream .of(5,6,7).forEach(System.out::println);
System.out.println("===========");
IntStream.range(3,8).forEach(System.out::println);
System.out.println("===========");
```



```java
//将整型的list乘以2再求和
List<Integer> list= Arrays.asList(2,3,4,6,4,1);
//先获取流，再将一个元素转换成另一个元素，最后求和
Integer i = list.stream().map(temp -> temp * 2).reduce(0, (a, b) -> a + b);
System.out.println(i);
```



```java
//取大于2的元素，再乘以2，去掉前两个，获得前两个，最后计算总和
Integer reduce = Stream.iterate(1, item -> item + 1)
        .limit(10).filter(item -> item > 2).map(item -> item * 2)
        .limit(2).reduce(0, (i, j) -> i + j);
System.out.println(reduce);
```



### 对流的理解

---

Collection提供了新的stream()方法。

流不存储值，通过管道的方式获取值。

本质是函数式的，对流的操作会生成一个结果，不过并不会修改底层的数据源，集合可以作为流的底层数据源。

延迟查找，很多流操作（过滤、映射、排序）都可以延迟实现。

<br><br>

### 并发流

---

```java
public static void main(String[] args) {
    List<String> list=new ArrayList<>(500000);
    for(int i=0;i<500000;i++){
        list.add(UUID.randomUUID().toString());
    }
    System.out.println("开始排序");
    long startTime=System.currentTimeMillis();
    //list.stream().sorted().count();
    //list.parallelStream().sorted().count();
    long endTime=System.currentTimeMillis();
    System.out.println(endTime-startTime);
}
```



### 流的短路

---

```java
List<String> list= Arrays.asList("hello","world","hello world");
//找出第一个长度为5的单词，打出长度
list.stream().filter(item->{
    System.out.println(item);
    return item.length()==5;
}).limit(1).forEach(item-> System.out.println(item.length()));
```

上面的例子中，输出的结果如下

```
hello 
5
```

 可以把流看成一个存放操作的容器，流将第1个元素进行操作1，再进行操作2，并不是说所有的元素都进行操作1后再进行操作2。同时由于流存在短路，只要符合条件，那么后面就不再执行了。

```java
public static void main(String[] args) {
    List<String> list= Arrays.asList("hello","world","hello world");
    //找出前两个长度为5的单词，打出长度
    list.stream().filter(item->{
        System.out.println(item);
        return item.length()==5;
    }).limit(2).forEach(item-> System.out.println(item.length()));
}
```

执行的结果

```java
hello
5
world 
5 
```

