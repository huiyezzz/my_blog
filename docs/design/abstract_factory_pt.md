## 抽象工厂模式
抽象工厂模式算是工厂方法模式的进阶版，也属于创建型模式， 工厂方法设计模式是由工厂统一的创建 【一个】 类或接口的实例，而抽象工厂模式则是由工厂统一提供【多个相关联】的类或接口的实例。即抽象工厂相当于多个普通工厂，抽象工厂包含的普通工厂提供的对象是具备某种关联关系的。

**例：**

- 工厂方法设计模式：由电脑屏幕工厂统一提供电脑的屏幕 ，由电脑cpu工厂统一提供电脑的cpu

- 抽象工厂设计模式：由苹果电脑工厂提供苹果电脑的屏幕和cpu，由惠普电脑工厂提供惠普电脑的屏幕和cpu 


```
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



### 总结：
  无论是工厂方法模式，还是抽象工厂模式，他们都属于工厂模式，在形式和特点上也是极为相似的，他们的最终目的都是为了解耦。在使用时，我们不必去在意这个模式到底工厂方法模式还是抽象工厂模式，因为他们之间的演变常常是令人琢磨不透的。经常你会发现，明明使用的工厂方法模式，当新需求来临，稍加修改，加入了一个新方法后，由于类中的产品构成了不同等级结构中的产品族，它就变成抽象工厂模式了；而对于抽象工厂模式，当减少一个方法使的提供的产品不再构成产品族之后，它就演变成了工厂方法模式。
  所以，在使用工厂模式时，只需要关心降低耦合度的目的是否达到了。