### 本地线程变量

---

变量值共享可以使用public static 变量的形式，所有的线程都可以使用同一个变量。

但是如果想要每一个线程都有自己的共享变量，就可以使用ThreadLocal。

<br>

ThreadLocal就好比一个Map

set存数据就好比 put(Thread.getCurrentThread,value)

get取数据就好比 get(Thread.getCurrentThread)

<br><br>

### ThreadLocal的使用

---

```java
public class ThreadLocalTest {
	public static ThreadLocal tl=new ThreadLocal<>();
	public static void main(String[] args) {
		if(tl.get()==null) {
			System.out.println("从未放过值");
			tl.set("我的值");
		}
		System.out.println(tl.get());
		System.out.println(tl.get());
	}
}
```

对于ThreadLocal 的 get() 方法，如果没有存放数据，则返回的结果为null。

<br><br>

### 为ThreadLocal设置初始值

---

上面的例子可以知道，如果没有存放数据，ThreadLocal返回的将会是一个null

我们也可以通过重写ThreadLocal中的 initialValue（）方法来为它设置初始值

```java
private ThreadLocal<Integer> threadLocal=new ThreadLocal<Integer>(){
	protected Integer initialValue() {
		return 0;
	};
};
```

<br><br>

### ThreadLocal可能引起内存泄露

---

当线程结束但没有使用remove方法时，可能引起内存泄露

因为在一个操作系统中，线程是有数量上限的。在操作系统中，确定线程的唯一标志就是线程的ID。

操作系统回收线程的时候，不是一定要杀死线程，在繁忙的时候，只会做线程栈数据的清除，重复使用线程或进程，但堆中ThreadLocal对象的数据是不会被清掉的。

因此，当其他线程再次以同样的ID获取去获取数据时，得到的数据可能是上一个线程留下的，出现了内存泄露。