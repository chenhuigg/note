### Function接口
--------------------------------------------------------------------------------
Function接口，是函数式接口，在用户实际使用的时候可以传递任意的操作给该函数，它可以用来传递行为。
对于 Function<T,R>的参数，其表示传入T类型的对象，返回R类型的对象。
```java
public class FunctionTest{    
    public int compute(int a, Function<Integer,Integer> function){
        return function.apply(a);
    }
    public String convert(int a,Function<Integer,String> function){
        return function.apply(a);
    }
}
```
使用上面的方法
```java
System.out.println(functionTest.compute(1,integer->integer));
System.out.println(functionTest.compute(1,integer->integer+10));
System.out.println(functionTest.convert(1,i->String.valueOf(i+"HELLO")));
System.out.println(functionTest.convert(1,i->i.toString()));
```
由上面的例子可以看出，Function所传递的其实是一种行为，而上面所写的方法，并没有在其定义时实现，而是在使用的时候，由用户自己去实现。

我们也可以直接创建行为将其传递
```java
Function<Integer,Integer> function=(integer)->integer;
System.out.println(functionTest.compute(100,function));
```
<br><br>

### Function接口的compose方法和endThen方法
--------------------------------------------------------------------------------
compose方法和endThen方法是Function接口中定义的默认方法，方法本身接收了一个函数式接口，同时又返回了一个函数式接口。
```java
default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
    Objects.requireNonNull(before);
    return (V v) -> apply(before.apply(v));
}
```
```java
default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
    Objects.requireNonNull(after);
    return (T t) -> after.apply(apply(t));
}
```
下面对其进行测试
```java
public int compute(int a, Function<Integer,Integer> function1,Function<Integer,Integer> function2){
    return function1.compose(function2).apply(a);
}
public int compute2(int a, Function<Integer,Integer> function1,Function<Integer,Integer> function2){
    return function1.andThen(function2).apply(a);
}
//下面为main方法中编写的测试
FunctionTest2 test2=new FunctionTest2();
System.out.println(test2.compute(10,integer -> integer*3,integer -> integer*integer));
System.out.println(test2.compute2(10,integer -> integer*3,integer -> integer*integer));
```
上面的测试结果为300和900

对于compose方法调用来说，它先对输入参数function2应用apply方法，将function2的结果作为function1的输入。

<br><br>
### BiFunction接口
--------------------------------------------------------------------------------
上面的Function接口只有一个输入和一个输出，如果我们有两个输入，想要去得到一个输出，我们可以使用BiFunction（Bi→Bidirectional：两个，双向）函数式接口。
```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
```
T表示传递给函数的第一个参数类型，U表示传递给函数的第二个参数类型
R表示函数的结果类型。

<br><br>
### 使用BiFunction实现程序
--------------------------------------------------------------------------------
定义一个四则运算
```java
public int biFunction(int a,int b,BiFunction<Integer,Integer,Integer> biFunction){
    return biFunction.apply(a,b);
}
```
调用该四则运算
```java
BiFunctionTest biFunctionTest=new BiFunctionTest();
System.out.println(biFunctionTest.biFunction(1,2,(Integer a,Integer b)->{return a + b;}));
```

<br><br>
### BiFunction的andThen方法
--------------------------------------------------------------------------------
```java
default <V> BiFunction<T, U, V> andThen(Function<? super R, ? extends V> after) {
    Objects.requireNonNull(after);
    return (T t, U u) -> after.apply(apply(t, u));
}
```
该方法传入的参数是Function而不是BiFunction，是因为首先它将传进来的两个参数进行操作，得到一个结果，而after对该结果进行操作，只接收该结果，即一个参数，因此传入的参数为Function即可。

**对andThen方法进行测试**
```java
public int compute(int a, int b,
                   BiFunction<Integer,Integer,Integer> biFunction,Function<Integer,Integer> function){
    return biFunction.andThen(function).apply(a,b);
}

System.out.println(biFunctionTest.compute(1,2, (value1,value2)->value1+value2,(value)->value * value));
```
上面的计算结果为9

<br><br>
### BiFunction没有compose方法
--------------------------------------------------------------------------------
在BiFunction接口中并没有compose方法，因为行为参数的执行结果必定是唯一的，而对于BiFunction的apply()方法来说，它必须传入两个参数，因此compose是不合理的。

<br><br>
### Predicate接口
--------------------------------------------------------------------------------
Predicate表示一个判断，针对给定的参数T，来进行计算，如果输入参数符合匹配的条件，返回ture，否则返回false。

<br><br>
### Predicate的test方法
--------------------------------------------------------------------------------
```java
boolean test(T t);
```

其传入一个行为，然后返回true或者false

<br><br>
### 对Predicate进行测试
--------------------------------------------------------------------------------
定义一个方法对List集合进行状态的过滤
```java
public static void conditionFilter(List<Integer>list,Predicate<Integer> predicate){
    list.forEach(integer -> {
        if(predicate.test(integer)){
            System.out.println(integer);
        }
    });
}
```
对上面的方法进行测试
```java
List<Integer> list= Arrays.asList(2,3,12,34,12,3,2,12,13);
//输出小于3的集合
conditionFilter(list,integer -> integer<3)
//输出所有的集合
conditionFilter(list,integer -> true);
```

<br><br>
### Predicate的默认方法
--------------------------------------------------------------------------------
**and方法**
```java
default Predicate<T> and(Predicate<? super T> other) {
    Objects.requireNonNull(other);
    return (t) -> test(t) && other.test(t);
}
```
当前的Predicate与另外的一个Predicate进行逻辑与，如果当前的Predicate返回的结果为false，那么后者将不会被计算。

**negate方法**
```java
default Predicate<T> negate() {
    return (t) -> !test(t);
}
```
表示当前的Predicate逻辑的相反面

**or方法**
```java
default Predicate<T> or(Predicate<? super T> other) {
    Objects.requireNonNull(other);
    return (t) -> test(t) || other.test(t);
}
```
表示当前的Predicate与另外一个Predicate的逻辑或的操作

<br><br>
### Supplier接口
--------------------------------------------------------------------------------
Supplier表示结果的供应者，其作用就是不接收任何参数，同时返回一个结果。
其结构如下
```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
<br><br>

### 对Supplier接口进行测试
--------------------------------------------------------------------------------
```java
//定义一个Lambda表达式实例，要求不接收参数，同时返回一个结果
Supplier<String> supplier=()-> UUID.randomUUID().toString();
System.out.println(supplier.get());
```