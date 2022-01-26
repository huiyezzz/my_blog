<p class="tip">
  This is for beginners and pros, just enjoy!
</p>
# 设计模式
## 设计模式分类 :memo:
### 分类
- 创建型模式：
工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

- 结构型模式：
适配器模式、合成模式、装饰者模式、代理模式、享元模式、门面模式、桥接模式。

-  行为型模式：
策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、调停者模式、解释器模式。

## 面向对象设计的基本原则
### 总原则-开闭原则
对扩展开放，对修改关闭，不允许更改系统的抽象层和已有的实现层，允许添加新的实现。使程序的扩展性好，易于维护和升级。

### 单一职责原则 
如果一个类有多于一个的动机被改变，那么这个类就具有多于一个的职责。而单一职责原则就是指一个类或者模块应该有且只有一个改变的原因。

### 里氏替换原则
任何基类可以出现的地方，子类一定可以出现。里氏替换原则是继承复用的基石，只有当子类可以替换基类，软件单位的功能不受到影响时，基类才能真正被复用，而类也能够在基类的基础上增加新的行为

### 依赖倒置原则
依赖与抽象，而不依赖于具体实现。使用对象时，如果这个对象有一个抽象类型，应当使用这个抽象类型作为变量的静态类型。

### 接口隔离原则
客户端不应该依赖它不需要的接口；一个类对另一个类的依赖应该建立在最小的接口上。接口应该尽量细化，一个接口对应一个功能模块，同时接口里面的方法应该尽可能的少，使接口更加轻便灵活。

### 迪米特原则
一个对象应对其他对象尽可能少的了解。成员变量、方法参数、方法返回值之外的陌生的类不要作为局部变量出现在类中。

### 合成复用原则
尽量使用组合，少用继承

## 接口与抽象类
抽象类不能被实例化，设计师设计一个抽象类，一定是用来继承的，而具体类不是用来继承的，只要有可能，尽量不要从具体类继承。
抽象类应当具有尽可能多的共同代码：在一个抽象类到多个具体类的继承体系中，共同的代码应尽量的移到抽象类中。把重复的代码从子类移动到超类中，可以提高代码的复用率，当代码需要发生改变时，设计师只需要修改一个地方。
抽象类应当拥有尽可能少的数据：一个对象的数据不论是否使用都会占用资源，因此数据应当尽量放到具体类或者等级结构的末端。

### java API 违反里氏替换原则的反面教材
properties 继承自Hashtable类。hashtable是可以接受除null 以外任何类型的key和value的，但properties 只能接受string类型的key和value,而不能接受某些特殊类型如引用类型作为key或value。这意味着在父类Hashtable出现的地方不能使用properties替代，违反了里氏替换原则。

### 变量的静态类型和真实类型
变量被声明时的类型称为变量的静态类型，变量所引用对象的真实类型叫做变量的真实类型

```java
List list = new ArrayList<>();
```

list的静态类型是List,真实类型是ArrayList;
当我们需要引用一个对象时，如果这个对象有一个抽象类型，应当使用这个抽象类型作为变量的静态类型。
即尽量不要定义下面的语句：

```java
ArrayList list = new ArrayList<>();
```

而应定义成

```java
List list = new ArrayList<>();
```

后者的好处是，在需要将ArrayList替换为LinkedList或者vector类型时，不需要做太多的代码修改，这是多态的一种运用，也是依赖倒置和面向接口编程的体现，它使得程序具有更好的灵活性。

### 动态单分派与静态多分派
java的动态分派仅仅会考虑方法的所有者是哪个对象，因此属于单分派，java的静态分派即对重载方法的分派会考虑到方法的参数类型，因此属于多分派。方法重载是静态多分配，根据对象的静态类型决定调用哪个方法，而方法重写是动态单分派，根据对象的实际类型决定调用哪个方法体。

接口是定义混合类型的理想工具，所谓混合类型，就是在一个类主要类型之外的次要类型。一个混合类型表明，一个类不仅仅具有某个主要类型的行为，还具有其他的次要行为。抽象类不适合定义混合类型，因为java的类是单继承的，如果某一个类已经继承了一个抽象类，要想让这个类再具有次要的行为，就必须将这个次要行为的抽象类插入继承体系中，这会影响到继承体系下游所有的具体实现类。
例如HashTable的主要类型是Map，它的次要类型是Cloneable和Serializable，前者说明HashTable是一个map类型集合，后者表示它还是可以被克隆和可序列化的。

抽象类能够提供一些默认的实现，将重要的宏观逻辑以及子类共用的方法在抽象类中实现，再定义一些抽象方法，迫使子类去实现一些可以差异化的细节逻辑。这就让子类用尽可能少的代码达到了不同实现类有不同的实现，实现的差异就体现在这些抽象方法上，而大的宏观逻辑是共同的，这是模板方法设计模式，也是抽象类相比接口唯一的优点。

### 接口与抽象类的联合使用
由于java抽象类具有提供缺省实现的优点，而接口具有其他所有的优点，所以联合两者使用就是一种很好的选择。声明类型的工作由java 接口承担，同时给出一个抽象类，提供这个接口的一种默认实现。其他同属于这个类型的具体实现类，可以选择实现接口，也可以选择继承抽象类，如果直接实现接口的话，那它需要自己实现这个接口中所有的方法，相反，如果它继承抽象类，相当于遵循某种大的宏观逻辑，只需自己去实现一些差异化的细节方法，省去一些不必要的方法实现。如果实现类选择了继承抽象类，那么当接口中添加方法时，只需要在抽象类中添加相应的方法，所有继承自这个抽象类的子类都不必去实现这个新添加的方法，这就是缺省适配模式。
java语言的API中也运用了这种缺省适配模式，且全部遵循一定的命名规范：Abstract + 接口名，
如Map与AbstracMap,List与AbstractList。
联合使用接口和抽象类可以充分利用两者的优点，克服两者的缺点。

### 定制服务
定制服务也是一个重要的设计原则，它的意思是说，如果客户端仅仅需要一些方法的话，那么就应当向客户端提供这些需要的方法，而不要提供不需要的方法。为一个角色提供宽、窄两个不同的接口，以对付不同的客户端。因为向客户端提供的public接口是一种承诺，一个public接口一旦提供，就很难撤回，撤回public接口，将会使原有使用这些接口的客户端面临崩溃的风险。作为软件提供商，没有人愿意做出过多的承诺，特别是不必要的承诺。

## 工厂方法模式
### 简介
工厂方法模式是一种 创建型模式。
与一个对象相关联的职责通常有三种：
- 对象本身所具备的职责
- 创建对象的职责
- 使用对象的职责。

工厂模式有效的将对象的使用和创建分离开来。使系统更加符合“单一职责原则”。

### 使用场景

想象以下两个场景：
- 场景1：假设我们需要一个数据库连接对象，获取这个对象前需要先加载驱动，然后通过用户名和密码建立指定数据库的连接。如果我们要在很多类中使用这个对象，就意味着我们需要在每处使用该对象的地方加载驱动、建立连接，这存在着大量的代码重复。

- 场景2:某一个接口提供某种“服务”，许多地方都要使用到这个服务，基于某种原因我们需要对这个“服务”进行某种优化或者扩展，根据开闭原则，我们不能直接去更改重构原先提供该服务的类。面向接口编程也要求我们在出现新的需求时应增加新的实现类，而不是更改原先的实现类。这时候我们需要创建一个与该类实现相同接口的类，然后将所有使用原有服务对象的地方替换为新的服务对象。这需要我们寻找到所有用到原有服务对象的位置。

方案：将某类对象的创建和配置的相关过程放进该类的工厂类中，所有需要用到此类对象的地方直接从工厂中获取，这样就不用在每个需要使用该类服务的地方都对该类的对象进行创建和配置了。如果想用新的实现类对象替代原有的实现类，也只需要在工厂方法的返回上稍作修改就可以轻松达到此目的，而不需要在所有可能使用该服务的类中查找替换。这就是工厂方法设计模式。

### 工厂方法模式的作用
- 避免对象属性配置的重复代码
- 统一替换
- 还有一个隐晦的作用是可以增加代码可读性

### 简单工厂模式的缺点
- 工厂负责所有产品的创建，有新的产品加入到工厂中，需要修改工厂类。
- 工厂方法模式：核心的工厂类变为抽象类，仅给出具体工厂子类必须实现的接口，具体的创建工作交给子类。
- 简单工厂与抽象工厂一样，工厂方法返回抽象类型，而不返回具体类型。（多态性）

### 示例

```java
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


## 抽象工厂模式
### 简介
抽象工厂模式算是工厂方法模式的进阶版，也属于创建型模式， 工厂方法设计模式是由工厂统一的创建 【一个】 类或接口的实例，而抽象工厂模式则是由工厂统一提供【多个相关联】的类或接口的实例。即抽象工厂相当于多个普通工厂，抽象工厂包含的普通工厂提供的对象是具备某种关联关系的。

### 示例

- 工厂方法设计模式：由电脑屏幕工厂统一提供电脑的屏幕 ，由电脑cpu工厂统一提供电脑的cpu

- 抽象工厂设计模式：由苹果电脑工厂提供苹果电脑的屏幕和cpu，由惠普电脑工厂提供惠普电脑的屏幕和cpu 


```java
package design;

/**
 * Created by HUIYE on 2019/8/7.
 */
interface Cpu {
    void showName();
}
class AppleCpu implements Cpu{
    @Override
    public void showName() {
        System.out.println("Apple Cpu");
    }
}
class HpCpu implements Cpu{
    @Override
    public void showName() {
        System.out.println("Hp Cpu");

    }
}
interface Screen{
    void showName();
}
class AppleScreen implements Screen{
    @Override
    public void showName() {
        System.out.println("Apple Screen");
    }
}
class HpScreen implements Screen{
    @Override
    public void showName() {
        System.out.println("Hp Screen");
    }
}
interface AbstractFactory{
    Cpu getCpu();
    Screen getScreen();
}
class AppleFactory implements  AbstractFactory{

    @Override
    public Cpu getCpu() {
        return new AppleCpu();
    }

    @Override
    public Screen getScreen() {
        return new AppleScreen();
    }
}
class HpFactory implements  AbstractFactory{

    @Override
    public Cpu getCpu() {
        return new HpCpu();
    }

    @Override
    public Screen getScreen() {
        return new HpScreen();
    }
}
public class AbstractFoctoryTest{
    public static void main(String[] args) {
        AbstractFactory appleFactory = new AppleFactory();
        appleFactory.getCpu().showName();
        appleFactory.getScreen().showName();
        AbstractFactory hpFactory = new HpFactory();
        hpFactory.getCpu().showName();
        hpFactory.getScreen().showName();
    }
}
```



### 总结
  无论是工厂方法模式，还是抽象工厂模式，他们都属于工厂模式，在形式和特点上也是极为相似的，他们的最终目的都是为了解耦。在使用时，我们不必去在意这个模式到底工厂方法模式还是抽象工厂模式，因为他们之间的演变常常是令人琢磨不透的。经常你会发现，明明使用的工厂方法模式，当新需求来临，稍加修改，加入了一个新方法后，由于类中的产品构成了不同等级结构中的产品族，它就变成抽象工厂模式了；而对于抽象工厂模式，当减少一个方法使的提供的产品不再构成产品族之后，它就演变成了工厂方法模式。
  所以，在使用工厂模式时，只需要关心降低耦合度的目的是否达到了。


## 单例模式
### 简介
单例模式也是一种 创建型模式，也是最简单的设计模式之一。
单例模式有以下的特点：
- 单例类只能有一个实例
- 单例类必须自己创建自己的唯一实例
- 单例类必须给所有其他对象提供这一实例


### 饿汉式
优点：类加载时预先创建好实例，避免了线程安全问题，getInstance不需要加锁，执行效率很高。

缺点：类加载时就初始化，不能实现延迟加载，浪费内存。


```java
public class SingleDemo {
    private static SingleDemo singleDemo=new SingleDemo();

    private SingleDemo() {
    }
    public static SingleDemo getInstance(){
        return singleDemo;
    }
} 
```


### 懒汉式（非线程安全方式）
优点：获取类实例时才创建实例，达到延迟加载的效果，getInstance没有加锁，执行效率高

缺点：getInstance没有加锁，存在线程安全问题。

```java
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


### 懒汉式（线程安全方式）
优点：懒汉式都具备延迟加载的优点，加锁的懒汉式能保证线程安全

缺点：getInstance加锁，执行效率低

```java
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


### 懒汉式（双重检查成例 DCL）
由于singleDemo只有第一次被赋值时才存在线程安全问题，在singleDemo被创建出来之后，同步化便没有了作用，甚至还会影响getInstance的执行效率。因此我们只需要在创建实例的地方进行同步即可。

优点：延迟加载，线程安全，且只在第一次创建时需要同步，后续获取实例执行效率高。

缺点：实现比前面几种相对比较复杂。

```java
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

### 静态内部类（登记式）
这种方式能实现和双重检查成例一样的效果，但实现相对更简单一些。

优点：外部类加载时，内部类并不会跟着加载，内部类不加载则不会初始化其中的实例，当getInstance被调用时才去加载内部类和其中的实例，达到延迟加载的效果。且内部类中的实例和内部类一起加载，能避免线程安全问题。

缺点：无法向内部类传参

```java
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

## 建造者模式
### 简介
建造者模式也是一种 创建型模式，使用这种模式的目的是将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。建造者模式实际上是将一个对象的性质建造过程外部化到独立的建造者对象中，并通过一个指挥者角色对这些外部化的性质赋值过程进行协调。

创建者模式一般具备以下几种角色
- 产品对象：建造者模式需要创建的复杂对象。
- 抽象建造者：规范产品对象所需的各个组成成分的建造的抽象接口。产品对象所包含多少个”零件“，就应当有多少个建造方法。
- 具体建造者：实现抽象建造者接口，用于创建和提供具体产品对象的类。有多少个具体产品对象就应该有多少具体建造者类。
- 指挥者：负责控制指挥具体建造者一步步完成产品对象的建造，将创建完成的对象返回。指挥者是直接与业务逻辑层打交道的角色。
 
### 示例

**产品类：**计算机对象，拥有cpu和屏幕两个零件

```java
class Computer{
    private Cpu cpu;
    private Screen screen;

    public Cpu getCpu() {
        return cpu;
    }

    public void setCpu(Cpu cpu) {
        this.cpu = cpu;
    }

    public Screen getScreen() {
        return screen;
    }

    public void setScreen(Screen screen) {
        this.screen = screen;
    }
}
```


**抽象建造者：**定义了计算机cpu和屏幕的建造方法，用于规范实现此接口的具体建造者能够完成对必要零件的建造。

```java
interface ComputerBulider{
    void bulidCpu();
    void bulidScreen();
    Computer construct();
}
```


**具体建造者1：** 苹果电脑建造者，用苹果的零件构建电脑对象，创建出的对象是苹果电脑

```java
class AppleComputerBulider implements  ComputerBulider{
    private Computer computer=new Computer();

    @Override
    public void bulidCpu() {
        computer.setCpu(new AppleCpu());
    }

    @Override
    public void bulidScreen() {
        computer.setScreen(new AppleScreen());
    }

    @Override
    public Computer construct() {
        return computer;
    }
}
```


**具体创建者2：**惠普电脑建造者，用惠普的零件构建电脑对象，创建出的对象是惠普电脑

```java
class HpComputerBulider implements  ComputerBulider{
    private Computer computer=new Computer();
    @Override
    public void bulidCpu() {
        computer.setCpu(new HpCpu());
    }

    @Override
    public void bulidScreen() {
        computer.setScreen(new HpScreen());
    }

    @Override
    public Computer construct() {
        return computer;
    }
}
```


**指挥者：**控制通过参数传入的具体建造者完成产品的建造

```java
class Director{
    public static Computer construct(ComputerBulider bulider){
        bulider.bulidCpu();
        bulider.bulidScreen();
        return bulider.construct();
    }
}
```

**
客户端**

```java
public class BuliderPattern {
    public static void main(String[] args){
        Computer appleComputer = Director.construct(new AppleComputerBulider());
        Computer hpComputer = Director.construct(new HpComputerBulider());
    }
}
```


### 什么情况下使用建造者模式
- 需要生成的产品对象有复杂的内部结构

- 需要生成的产品对象属性相互依赖，如一个属性必须在另一个属性被赋值之后才能被赋值，造者模式可以强制实行一种分步骤进行的建造过程

- 产品对象创建过程需要用到其他系统中不易得到的对象

### 建造者模式的优点
- 使用建造者模式可以使客户端不必知道产品内部组成的细节。

- 具体的建造者类之间是相互独立的，这有利于系统的扩展。

- 具体建造者是相互独立的，因此可以对建造的过程逐步细化，而不会对其他产品的建造产生任何影响。

### 建造者模式的缺点
- 建造者模式所创建的产品一般具有较多的共同点，其组成部分相似；如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制。

- 如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大。

### 建造者模式与抽象工厂模式的比较

- 建造者模式返回一个组装好的完整产品，抽象工厂模式返回该产品一系列相关的“零件” 。

- 抽象工厂模式中，客户端实例化工厂类，然后调用工厂方法获取所需产品对象，而在建 
造者模式中，客户端一般不直接调用建造者的相关方法，而是通过指挥者类来指导如何生成对象，包括对象的组装过程和建造步骤，它侧重于一步步构造一个复杂对象，返回一个完整的对象。

- 抽象工厂模式更加具体，建造者模式更加宏观。一个系统可以同时使用这两种模式，客户端通过创建指挥者和建造者，建造者再调用抽象工厂里的某一个具体工厂角色，获取工厂返回的一系列零件，组装成一个完整的复杂对象。
    

## 原型模式
## 简介
原型模式也是一种 创建型模式。通过给出一个原型对象作为样本指明要创建的对象的类型，然后用克隆这个原型对象的方式创建出更多同类型的对象。

### 原型模式的应用场景
- 类初始化需要消化非常多的资源，这个资源包括数据、硬件资源等，通过原型拷贝避免这些 
消耗。 

- 通过new产生的一个对象需要非常繁琐的数据准备或者权限，这时可以使用原型模式。 

- 一个对象需要提供给其他对象访问，而且各个调用者可能都需要修改其值时，可以考虑使用
原型模式拷贝多个对象供调用者使用，即保护性拷贝。

Spring框架中的多例就是使用原型。


**原型模式主要用于对象的复制，原型类需要具备以下两个条件：** 
- （1）实现Cloneable接口。在java语言有一个Cloneable接口，它的作用只有一个，就是在运行时通知虚拟机可以安全地在实现了此接口的类上使用clone方法。在java虚拟机中，只有实现了这个接口的类才可以被拷贝，否则在运行时会抛出CloneNotSupportedException异常。 
- （2）重写Object类中的clone方法。Java中，所有类的父类都是Object类，Object类中有一个clone方法，作用是返回对象的一个拷贝，但是其作用域protected类型的，一般的类无法调用，因此Prototype类需要将clone方法的作用域修改为public类型。

原型模式的代码实现参照：java se基础/对象克隆。


## 适配器模式
### 简介
适配器模式是作为两个不兼容的接口之间的桥梁。将一个接口转换成客户希望的另一个接口（这两个接口一般是功能相同或者相近的），得原本由于接口不兼容而不能在一起工作的那些类可以一起工作，这种类型的设计模式属于结构型模式。

结构模式描述如何将类或者对象结合在一起，形成更大的结构。

**结构模式可以分为类的结构模式和对象的结构模式**：
- 类的结构模式：使用继承来把类、接口等组合在一起，形成更大的结构，类的结构模式是静态的
- 对象的结构模式：对象的结构模式描述怎样将各种不同类型的对象组合在一起，对象的结构模式是动态的

### 适配器模式所涉及的角色
- 目标角色（Target）:客户所期待的接口
- 源角色（Adaptee）：需要适配的已有接口。
- 适配器角色（Adapter）：将已有接口转换成目标接口。


### 类的适配器模式
下面的例子示范了通过充电头（适配器）将220v电压转换为5v电压，给手机充电的过程

```java
interface _5V_Voltage{
    int outPut5v();
}
class _220V_Voltage{
    int outPut220v(){
        return 220;
    }
}
class _220V_To_5V_Apapter extends _220V_Voltage implements  _5V_Voltage{

    @Override
    public int outPut5v() {
        return outPut220v()/44;
    }
}
class Phone{
    //电量
    private int electricity;
    //充电方法,只接受输出5v电压的接口
    void charging(_5V_Voltage voltage,int time){
        electricity=voltage.outPut5v()*time;
    }
}
public class ClassAdapter {
    public static void main(String[] args){
        Phone phone = new Phone();//手机对象
        phone.charging(new _220V_To_5V_Apapter(),10);//给手机充电
    }
}
```


### 对象的适配器模式
只需将适配器类由继承改为组合。（使用组合更加灵活）

```java
interface _5V_Voltage{
    int outPut5v();
}
class _220V_Voltage{
    int outPut220v(){
        return 220;
    }
}
class _220V_To_5V_Apapter  implements  _5V_Voltage{
    private  _220V_Voltage voltage;

    public _220V_To_5V_Apapter(_220V_Voltage voltage) {
        this.voltage = voltage;
    }

    @Override
    public int outPut5v() {
        return voltage.outPut220v()/44;
    }
}
class Phone{
    //电量
    private int electricity;
    //充电方法,只接受输出5v电压的接口
    void charging(_5V_Voltage voltage,int time){
        electricity=voltage.outPut5v()*time;
    }
}
public class ClassAdapter {
    public static void main(String[] args){
        Phone phone = new Phone();//手机对象
        _220V_Voltage powerAdaptee = new _220V_Voltage();//已有的输出220v电压的对象
        _5V_Voltage adapter = new _220V_To_5V_Apapter(powerAdaptee);//通过适配器转换为输出5v电压的接口
        phone.charging(adapter,10);//给手机充电
    }
}
```


### 适配器在架构层次上的应用举例
- linux环境运行windows程序，需要软件提供从windos到linux的适配，需要被适配的程序不需要做任何更改。
- 每种数据库的jdbc驱动程序都是java的抽象jdbc接口到数据库引擎的API适配。


## 缺省适配模式
### 简介
在任何时候，如果不准备实现一个接口所有的方法时，可以制造一个抽象类实现这个接口，为接口的所有方法提供空实现，子类继承这个抽象类，选择重写自己需要的方法。这样，子类就可以只实现自己需要的方法，忽略不必要的方法了，另外，当接口中添加了新的方法时，只需要在抽象类中添加这个方法的默认实现即可，而不需要对每个子类进行修改，这就是缺省适配模式。缺省适配器模式是结构型模式一种，由适配器模式简化而来，省略了适配器模式中目标接口，也就是源接口和目标接口相同，源接口为接口，目标接口为类。

java语言的API中也运用了这种缺省适配模式，且全部遵循一定的命名规范：Abstract + 接口名，
如Map与AbstracMap,List与AbstractList。

### 缺省适配器模式涉及角色
- 源接口：定义接口类型需要实现的方法
缺省适配器：实现源接口的抽象类（因为这个类不应当实例化） ，对接口中定义的方法给出空实现 
- 实现类：继承缺省适配器 ，选择性的重写需要实现的方法。

### 示例

```java
//一个验证器接口
interface Validator{
    boolean fingerprint();//指纹识别
    boolean face();//人脸识别
    boolean password();//密码识别
}
abstract class AbstractValidator implements  Validator{//缺省适配器
    @Override
    public boolean fingerprint() {
        return false;
    }
    @Override
    public boolean face() {
        return false;
    }

    @Override
    public boolean password() {
        return false;
    }
}
class HuiWei extends AbstractValidator{//只需要重写自己需要的识别方式
    @Override
    public boolean fingerprint() {
        System.out.println("华为使用指纹识别");
        return true;
    }
}
class Apple extends AbstractValidator{//只需要重写自己需要的识别方式
    @Override
    public boolean face() {
        System.out.println("苹果使用人脸识别");
        return true;
    }
}
```

## 合成模式
### 简介
合成模式，又叫做组合模式、部分-整体模式，属于对象的结构型模式，合成模式将对象组织到树结构中，用来描述整体与部分的关系。合成模式将单个对象和组合对象抽象为同一类型的对象，使得用户对单个对象和组合对象的使用具有一致性。

合成模式，说的是对象包含对象的问题。一个对象中包含其他对象，这些被包含的对象可能是终点对象（不再包含别的对象），也有可能是非终点对象（即组合对象，其内部还包含其他对象），我们将对象称为节点，即一个根节点包含许多子节点，这些子节点有的不再包含子节点，而有的仍然包含子节点。很明显，这是树形结构，终结点叫叶子节点，非终节点叫树枝节点，第一个节点叫根节点。

合成模式的实现根据所实现的接口区别分为两种形式，安全式和透明式。两者的区别在于在什么地方声明子对象的管理方法。

### 安全式
在树枝节点中，即组合对象中定义子对象的管理方法。
#### 缺点
不够透明，树枝比树叶多了管理子对象的方法，因此树枝和树叶并不完全相同。

#### 涉及角色
- 抽象角色：一个抽象的接口，定义树中每个节点默认的行为。
- 树叶节点角色：实现抽象构件接口，组合中没有下级子对象的对象。
- 树枝节点角色：实现抽象构件接口，组合中有下级子对象的对象，除了实现抽象构件给出的方法之外还定义了管理子对象的方法。

#### 代码实现

```java
interface Component{
    Component getComponent();
    void operation();
}
class LeafComponent implements Component{

    @Override
    public Component getComponent() {
        return this;
    }

    @Override
    public void operation() {}
}
public class Composite implements Component {
    private ArrayList<Component> components=new ArrayList<>();
    @Override
    public Component getComponent() {
        return this;
    }

    @Override
    public void operation() {

    }
    public void add(Component component){
        components.add(component);
    }
    public void remove(Component component){
        components.remove(component);
    }
}
```


### 透明式
在公共的抽象接口中定义管理子对象的方法。
#### 缺点
不够安全，由于管理子对象的方法定义在树枝和树叶的公共接口中，因此树叶节点对象也将包含这些管理子对象的方法，但很明显树叶节点是不包含子对象的，因此这些方法在树叶节点中是没有意义的，在运行时可能会出错。
#### 涉及角色
- 抽象角色：一个抽象的接口，定义树中每个节点默认的行为和管理子对象的行为。
- 树叶节点角色：实现抽象构件接口，组合中没有下级子对象的对象，不给出管理字对象的具体实现。
树枝节点角色：实现抽象构件接口，组合中有下级子对象的对象，给出管理子对象的具体实现。

#### 代码实现

```java
interface Component{
    Component getComponent();
    void operation();
    void add(Component component);
    void remove(Component component);
}
class LeafComponent implements Component{

    @Override
    public Component getComponent() {
        return this;
    }
    @Override
    public void operation() {}

    @Override
    public void add(Component component) {
        //...
    }
    @Override
    public void remove(Component component) {
        //...
    }
}
public class Composite implements Component {
    private ArrayList<Component> components=new ArrayList<>();
    @Override
    public Component getComponent() {
        return this;
    }
    @Override
    public void operation() {}
    
    public void add(Component component){
        components.add(component);
    }
    public void remove(Component component){
        components.remove(component);
    }
}
```



### 合成模式的应用场景
合成模式正是应树形结构而生，所以合成模式的使用场景就是出现树形结构的地方。比如：文件目录显示，多及目录呈现等树形结构数据的操作。
参考：https://www.runoob.com/w3cnote/java-composite-pattern-2.html


## 装饰器模式
### 简介
装饰器模式允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。这种模式创建了一个装饰类，用来包装原有的类，并在保持类方法签名完整性的前提下，提供了额外的功能。装饰器模式和继承都是要扩展对象的功能，但是装饰器模式可以提供比继承更好的灵活性。只要在装饰器类中内置一个要被装饰对象的抽象类型引用，就能用这个装饰器类扩展这个抽象类型的所有子类，而如果用继承，就需要针对每个子类进行扩展。

### 什么情况下使用装饰器模式
- 需要扩展一个类的功能
- 需要动态的为对象增加功能，这些功能可以再动态的撤销
- 需要增加由一些基本功能的排列组合而产生的非常大量的功能，从而使得用继承来扩展不太现实

### 半透明与完全透明的装饰器模式
**例：**

```java
/**
 * Created by HUIYE on 2019/8/16.
 */
interface Gun{//声明一个枪的接口
    void shooting();//射击方法
}
class AK47 implements Gun{
    @Override
    public void shooting() {
        System.out.println("AK47 射击");
    }
}
class M416 implements Gun{
    @Override
    public void shooting() {
        System.out.println("M416 射击");
    }
}
abstract class GunDecorator implements Gun{//抽象装饰器类
    private Gun gun;

    public GunDecorator(Gun gun) {
        this.gun = gun;
    }

    @Override
    public void shooting() {
        gun.shooting();
    }
}
class SightsGun extends GunDecorator{//带瞄准镜的枪，具体装饰器
    public SightsGun(Gun gun) {
        super(gun);
    }
    public void aimAt(){//瞄准方法
        System.out.println("瞄准");
    }
}
class MufflerGun extends GunDecorator{//带消音器的枪，具体装饰器

    public MufflerGun(Gun gun) {
        super(gun);
    }
    public void mute(){
        System.out.println("消音");
    }
}
public class DecoratorPattern {
    public static void main(String[] args) {
        Gun ak47 = new AK47();
        Gun m416 = new M416();
        SightsGun sightsGun = new SightsGun(ak47);//为ak47装上瞄准镜
        sightsGun.aimAt();//瞄准
        sightsGun.shooting();//射击
        MufflerGun mufflerGun = new MufflerGun(m416);//为M416装上消音器
        mufflerGun.shooting();//射击
        mufflerGun.mute();//消音
    }
}
```

上述代码演示了一种半透明式的装饰器模式，所谓半透明，是因为装饰器类在增强功能时，添加了新的公开的方法，即public的aimAt()和mute()方法，这些方法是被装饰者所不包含的，装饰器类和被装饰者包含的公开接口不一致，意味着我们不能将装饰器对象抽象化，如果声明如
Gun sightsGun = new SightsGun(ak47);
形式的向上转型语句，将丢失增强的行为。

将上述代码改写为完全透明的装饰器模式，代码如下：

```java
/**
 * Created by HUIYE on 2019/8/16.
 */
interface Gun{//声明一个枪的接口
    void shooting();//射击方法
}
class AK47 implements Gun{
    @Override
    public void shooting() {
        System.out.println("AK47 射击");
    }
}
class M416 implements Gun{
    @Override
    public void shooting() {
        System.out.println("M416 射击");
    }
}
abstract class GunDecorator implements Gun{//抽象装饰器类
    private Gun gun;

    public GunDecorator(Gun gun) {
        this.gun = gun;
    }

    @Override
    public void shooting() {
        gun.shooting();
    }
}
class SightsGun extends GunDecorator{//带瞄准镜的枪，具体装饰器
    public SightsGun(Gun gun) {
        super(gun);
    }

    @Override
    public void shooting() {
        aimAt();
        super.shooting();
    }

    private void aimAt(){//瞄准方法，非公开接口
        System.out.println("瞄准");
    }
}
class MufflerGun extends GunDecorator{//带消音器的枪，具体装饰器

    public MufflerGun(Gun gun) {
        super(gun);
    }
    @Override
    public void shooting() {
        super.shooting();
        mute();
    }
    private void mute(){//消音方法，非公开接口
        System.out.println("消音");
    }
}
public class DecoratorPattern {
    public static void main(String[] args) {
        Gun ak47 = new AK47();
        Gun m416 = new M416();
        Gun sightsGunAk47 = new SightsGun(ak47);//为ak47装上瞄准镜
        Gun mufflerGunAk47 = new MufflerGun(sightsGunAk47);//为ak47装上消音器
        mufflerGunAk47.shooting();//射击
        Gun sightsGunM416 = new SightsGun(m416);
        Gun mufflerGunM416 = new MufflerGun(sightsGunM416);
        mufflerGunM416.shooting();
    }
}
```

这就是一种完全透明的装饰器模式，装饰器类在被抽象化后为Gun后依然能够保持类的增强行为，因为它拥有与被装饰类完全相同的公开接口。这个示例能更改为完全透明的方式，是由于，“瞄准”与“消音”是与“射击”这一行为相关的，可以看作是对已有功能的增强，而不是添加新的功能。倘若我们需要添加新的功能，如给枪加上“刺刀”，这是与射击完全无关的，就必须要添加新的公开接口。

完全透明的装饰器模式，要求装饰器对象在抽象化之后依然能够保持类的增强行为。这要求装饰器类和被装饰类拥有完全相同的公开接口，而不能添加新的公开接口。这实际上是很难做到的，增强功能的时候往往都需要建立新的公开接口，所以完全透明的装饰器模式在实际应用中很难找到，大多数的装饰器模式都是半透明式的。
仔细想想，完全透明的装饰器模式其实和代理模式是非常相似的。

### 装饰器模式与适配器模式的区别
装饰器和适配器都需要包装源对象，但适配器模式的用意是要改变被包装对象的接口，而装饰器模式的用意是要增强被包装对象的功能。


## 代理模式
### 简介
代理模式是基本的设计模式之一，是一种对象的结构型模式，代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用(代理对象通常在将调用传递给被代理对象之前或者之后，都要执行某个操作，而不只是单纯的调用传递)。代理模式利用这种间接通信的方式改善系统设计，降低耦合度。

### 代理模式的运用
- 保护性代理：代理对象对用户进行权限检查后，决定是否调用被代理对象。
- 加载延缓：被代理对象的加载需要耗费一定的时间时，代理对象提升正在加载。

### 静态代理

```java
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

#### 静态代理的优缺点
##### 优点
代理模式有效的控制了客户端对具体实现类的直接访问，将两者的关系完全解耦，很好的隐藏和保护具体实现类对象的同时还可对具体实现类的功能进行扩展和拦截。
##### 缺点

代理类和具体实现类必须实现相同的接口，但代理类一般只对接口的某个方法进行代理，而 不需要实现接口中的其他方法，这意味着大量的冗余代码，接口添加方法，具体实现类和代理类都要实现新加的接口，难以维护。代理类只能对一个接口进行代理，如果需要对多个接口代理需要多个代理类。

### 动态代理

```java
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

##### 优点
解决了静态代理中代理类只能代理一个接口，需要编写大量代理类的问题。解决了代理类和被代理类一定要实现相同接口，代码冗余和不易维护的问题。
##### 缺点
被代理类一定要实现接口才能被代理。只能代理接口，不能代理类和抽象类。

### Cglib动态代理


```java
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


