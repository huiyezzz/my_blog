## 代理模式
代理模式是基本的设计模式之一，是一种对象的结构型模式，代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用(代理对象通常在将调用传递给被代理对象之前或者之后，都要执行某个操作，而不只是单纯的调用传递)。代理模式利用这种间接通信的方式改善系统设计，降低耦合度。

### 代理模式的运用：
- 保护性代理：代理对象对用户进行权限检查后，决定是否调用被代理对象。
- 加载延缓：被代理对象的加载需要耗费一定的时间时，代理对象提升正在加载。

### 静态代理

```
interface Subject{
    void request();
}
class RealSubject implements Subject{
    @Override
    public void request() {
       System.out.println("Real request");
    }
}
class ProxySubject implements Subject{
    private Subject subject;
    @Override
    public void request() {
        if (subject==null)
            subject=new RealSubject();
        System.out.println("前置方法");
        subject.request();
        System.out.println("后置方法");
    }
}
class Test{
    public static void main(String[] args){
        Subject proxy = new ProxySubject();
        proxy.request();
    }
}
```


上述代码演示了静态代理的实现，代理类需要实现与被代理类相同的接口（这是为了通过抽象让客户端察觉不到代理的存在），然后在代理类中内置一个被代理对象的引用，代理类的接口方法中调用内置的被代理对象的对应方法，就可以实现代理了，同时可以在方法中加入一些其他的业务逻辑，如预处理，前后拦截，过滤等。

#### 静态代理的优缺点：
##### 优点：
代理模式有效的控制了客户端对具体实现类的直接访问，将两者的关系完全解耦，很好的隐藏和保护具体实现类对象的同时还可对具体实现类的功能进行扩展和拦截。
##### 缺点：

代理类和具体实现类必须实现相同的接口，但代理类一般只对接口的某个方法进行代理，而 不需要实现接口中的其他方法，这意味着大量的冗余代码，接口添加方法，具体实现类和代理类都要实现新加的接口，难以维护。代理类只能对一个接口进行代理，如果需要对多个接口代理需要多个代理类。

### 动态代理

```
class MyInvocationHandler implements InvocationHandler {
    private Object target;
    public MyInvocationHandler(Object target) {
        this.target = target;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("前置方法");
        Object invoke = method.invoke(target, args);
        System.out.println("后置方法");
        return null;
    }
}
class Test{
    public static void main(String[] args){
        Subject proxyInstance = (Subject) Proxy.newProxyInstance(ProxyTest.class.getClassLoader(), RealSubject.class.getInterfaces(),
                new MyInvocationHandler(new RealSubject()));
        proxyInstance.request();
    }
}
```


动态代理更加灵活，java反射库的Proxy类可以为任意接口类型生成代理对象，而静态代理的代理类只能代理一个接口。使用Proxy类的静态方法newProxyInstance()可以为我们创建动态代理对象。该方法需要三个参数，第一个参数为类加载器；第二个参数为要代理的接口列表（java jdk的动态代理只能代理接口，因此该Class数组中只能放接口类型），返回的代理对象可以选择转型为该列表中的任意一个接口类型；第三个参数是需要一个InvocationHandler接口的实现，该接口中的invoke方法是代理对象进行方法调用时实际执行的方法。

#### 动态代理的优缺点

##### 优点： 
解决了静态代理中代理类只能代理一个接口，需要编写大量代理类的问题。解决了代理类和被代理类一定要实现相同接口，代码冗余和不易维护的问题。
##### 缺点：
被代理类一定要实现接口才能被代理。只能代理接口，不能代理类和抽象类。

### Cglib动态代理


```
public class CglibProxy {
    public static void main(final String[] args) {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(new Student().getClass());//具体实现类的Class,也是要生成的代理对象的父类。
        enhancer.setCallback(new MethodInterceptor() {//设置回调方法
            public Object intercept(Object obj, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                System.out.println("前置方法");
                Object invoke = methodProxy.invokeSuper(obj, objects);
                System.out.println("后置方法");
                return invoke;
            }
        });
        Student proxy = (Student) enhancer.create();
        proxy.homeWork();

    }
}
class Student{
    public void homeWork(){
        System.out.println("写作业");
    }
}
```


CGlib动态代理通过继承 被代理类生成 代理类（因此final类没法使用动态代理），生成的代理类是被代理类的子类。Cglib动态代理解决了Java jdk动态代理具体实现类一定要实现接口的问题。