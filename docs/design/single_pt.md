## 单例模式
单例模式也是一种 创建型模式，也是最简单的设计模式之一。
单例模式有以下的特点：
- 单例类只能有一个实例
- 单例类必须自己创建自己的唯一实例
- 单例类必须给所有其他对象提供这一实例

### 单例模式的几种实现方式：
#### 1：饿汉式
优点：类加载时预先创建好实例，避免了线程安全问题，getInstance不需要加锁，执行效率很高。

缺点：类加载时就初始化，不能实现延迟加载，浪费内存。


```
public class SingleDemo {
    private static SingleDemo singleDemo=new SingleDemo();

    private SingleDemo() {
    }
    public static SingleDemo getInstance(){
        return singleDemo;
    }
} 
```


#### 2：懒汉式（非线程安全方式）
优点：获取类实例时才创建实例，达到延迟加载的效果，getInstance没有加锁，执行效率高

缺点：getInstance没有加锁，存在线程安全问题。

```
public class SingleDemo {
    private static SingleDemo singleDemo;

    private SingleDemo() {
    }
    public static SingleDemo getInstance(){
        if(singleDemo==null)
            singleDemo=new SingleDemo();
        return singleDemo;
    }
}
```


#### 3：懒汉式（线程安全方式）
优点：懒汉式都具备延迟加载的优点，加锁的懒汉式能保证线程安全

缺点：getInstance加锁，执行效率低

```
public class SingleDemo {
    private static SingleDemo singleDemo;

    private SingleDemo() {
    }
    public synchronized static SingleDemo getInstance(){
        if(singleDemo==null)
            singleDemo=new SingleDemo();
        return singleDemo;
    }
}
```


#### 4：懒汉式（双重检查成例 DCL）
由于singleDemo只有第一次被赋值时才存在线程安全问题，在singleDemo被创建出来之后，同步化便没有了作用，甚至还会影响getInstance的执行效率。因此我们只需要在创建实例的地方进行同步即可。

优点：延迟加载，线程安全，且只在第一次创建时需要同步，后续获取实例执行效率高。

缺点：实现比前面几种相对比较复杂。

```
public class SingleDemo {
    private static volatile SingleDemo singleDemo;

    private SingleDemo() {
    }
    public  static SingleDemo getInstance(){
        if(singleDemo==null){//第一次检查
            synchronized (SingleDemo.class){
                if(singleDemo==null)//第二次检查
                    singleDemo=new SingleDemo();
            }
        }
        return singleDemo;
    }
}
```

注意：静态变量SingleDemo前面加了volatile修饰，这是为了解决无序写入的问题，也就是说在jdk1.5之后，java才支持这种双重检查的方式。

#### 5：静态内部类（登记式）
这种方式能实现和双重检查成例一样的效果，但实现相对更简单一些。

优点：外部类加载时，内部类并不会跟着加载，内部类不加载则不会初始化其中的实例，当getInstance被调用时才去加载内部类和其中的实例，达到延迟加载的效果。且内部类中的实例和内部类一起加载，能避免线程安全问题。

缺点：无法向内部类传参

```
public class SingleDemo {
    private SingleDemo() {
    }
    private static class SingleHolder{
        private static final SingleDemo single=new SingleDemo();
    }
    public SingleDemo getInstance(){
        return SingleHolder.single;
    }
}
```

