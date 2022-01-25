## 工厂方法模式
### 简介
工厂方法模式是一种 创建型模式。
与一个对象相关联的职责通常有三种：
- 对象本身所具备的职责
- 创建对象的职责
- 使用对象的职责。

工厂模式有效的将对象的使用和创建分离开来。使系统更加符合“单一职责原则”。

### 使用场景：

想象以下两个场景：
- 场景1：假设我们需要一个数据库连接对象，获取这个对象前需要先加载驱动，然后通过用户名和密码建立指定数据库的连接。如果我们要在很多类中使用这个对象，就意味着我们需要在每处使用该对象的地方加载驱动、建立连接，这存在着大量的代码重复。

- 场景2:某一个接口提供某种“服务”，许多地方都要使用到这个服务，基于某种原因我们需要对这个“服务”进行某种优化或者扩展，根据开闭原则，我们不能直接去更改重构原先提供该服务的类。面向接口编程也要求我们在出现新的需求时应增加新的实现类，而不是更改原先的实现类。这时候我们需要创建一个与该类实现相同接口的类，然后将所有使用原有服务对象的地方替换为新的服务对象。这需要我们寻找到所有用到原有服务对象的位置。

方案：将某类对象的创建和配置的相关过程放进该类的工厂类中，所有需要用到此类对象的地方直接从工厂中获取，这样就不用在每个需要使用该类服务的地方都对该类的对象进行创建和配置了。如果想用新的实现类对象替代原有的实现类，也只需要在工厂方法的返回上稍作修改就可以轻松达到此目的，而不需要在所有可能使用该服务的类中查找替换。这就是工厂方法设计模式。

### 总结上面工厂方法模式的作用：
- 避免对象属性配置的重复代码
- 统一替换
- 还有一个隐晦的作用是可以增加代码可读性

### 简单工厂模式的缺点：
- 工厂负责所有产品的创建，有新的产品加入到工厂中，需要修改工厂类。
- 工厂方法模式：核心的工厂类变为抽象类，仅给出具体工厂子类必须实现的接口，具体的创建工作交给子类。
- 简单工厂与抽象工厂一样，工厂方法返回抽象类型，而不返回具体类型。（多态性）

### 工厂方法模式的使用示例：

```
package designpatterns;

/*工厂方法设计模式*/
interface Service{
    void method1();
    void method2();
}
interface ServiceFactory{
    Service getService();
}
class Implementation1 implements Service{
    public static ServiceFactory serviceFactory=new ServiceFactory() {
        @Override
        public Service getService() {
            return new Implementation1();
        }
    };
    //私有化构造器
    private Implementation1(){};
    @Override
    public void method1() {
        System.out.println("Implementation1 method1");
    }
    @Override
    public void method2() {
        System.out.println("Implementation1 method2");
    }
}
class Implementation2 implements Service{
    public static ServiceFactory serviceFactory=new ServiceFactory() {
        @Override
        public Service getService() {
            return new Implementation2();
        }
    };
    //私有化构造器
    private Implementation2(){};
    @Override
    public void method1() {
        System.out.println("Implementation2 method1");
    }
    @Override
    public void method2() {
        System.out.println("Implementation2 method2");
    }
}
public class Factories {
    public static void main(String[] args) {
        Service service1 = Implementation1.serviceFactory.getService();
        Service service2 = Implementation2.serviceFactory.getService();
        service1.method1();
        service2.method2();
    }
}
```

输出：Implementation1 method1
         Implementation2 method2

上面的Implementation1与Implementation2都已将构造器私有化，这就确保我们无法在外部使用new创建它们的对象。我们没有任何必要去创建作为工厂的具名类，也不需要用到多个工厂对象，因此上面的例子中使用匿名内部类的形式声明了一个工厂，并用static修饰。

#### 关于工厂模式作用更详细的解释参照以下博客：
https://blog.csdn.net/lovelion/article/details/7523392


