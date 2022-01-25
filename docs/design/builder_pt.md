## 建造者模式
建造者模式也是一种 创建型模式，使用这种模式的目的是将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。建造者模式实际上是将一个对象的性质建造过程外部化到独立的建造者对象中，并通过一个指挥者角色对这些外部化的性质赋值过程进行协调。

创建者模式一般具备以下几种角色
- 产品对象：建造者模式需要创建的复杂对象。
- 抽象建造者：规范产品对象所需的各个组成成分的建造的抽象接口。产品对象所包含多少个”零件“，就应当有多少个建造方法。
- 具体建造者：实现抽象建造者接口，用于创建和提供具体产品对象的类。有多少个具体产品对象就应该有多少具体建造者类。
- 指挥者：负责控制指挥具体建造者一步步完成产品对象的建造，将创建完成的对象返回。指挥者是直接与业务逻辑层打交道的角色。
 
**例：**

**产品类：**计算机对象，拥有cpu和屏幕两个零件

```
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

```
interface ComputerBulider{
    void bulidCpu();
    void bulidScreen();
    Computer construct();
}
```


**具体建造者1：** 苹果电脑建造者，用苹果的零件构建电脑对象，创建出的对象是苹果电脑

```
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

```
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

```
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

```
public class BuliderPattern {
    public static void main(String[] args){
        Computer appleComputer = Director.construct(new AppleComputerBulider());
        Computer hpComputer = Director.construct(new HpComputerBulider());
    }
}
```


### 什么情况下使用建造者模式：
- 需要生成的产品对象有复杂的内部结构

- 需要生成的产品对象属性相互依赖，如一个属性必须在另一个属性被赋值之后才能被赋值，造者模式可以强制实行一种分步骤进行的建造过程

- 产品对象创建过程需要用到其他系统中不易得到的对象

### 建造者模式的优点：
- 使用建造者模式可以使客户端不必知道产品内部组成的细节。

- 具体的建造者类之间是相互独立的，这有利于系统的扩展。

- 具体建造者是相互独立的，因此可以对建造的过程逐步细化，而不会对其他产品的建造产生任何影响。

### 建造者模式的缺点：
- 建造者模式所创建的产品一般具有较多的共同点，其组成部分相似；如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制。

- 如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大。

### 建造者模式与抽象工厂模式的比较:

- 建造者模式返回一个组装好的完整产品，抽象工厂模式返回该产品一系列相关的“零件” 。

- 抽象工厂模式中，客户端实例化工厂类，然后调用工厂方法获取所需产品对象，而在建 
造者模式中，客户端一般不直接调用建造者的相关方法，而是通过指挥者类来指导如何生成对象，包括对象的组装过程和建造步骤，它侧重于一步步构造一个复杂对象，返回一个完整的对象。

- 抽象工厂模式更加具体，建造者模式更加宏观。一个系统可以同时使用这两种模式，客户端通过创建指挥者和建造者，建造者再调用抽象工厂里的某一个具体工厂角色，获取工厂返回的一系列零件，组装成一个完整的复杂对象。
    