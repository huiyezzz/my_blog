## 装饰器模式
装饰器模式允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。这种模式创建了一个装饰类，用来包装原有的类，并在保持类方法签名完整性的前提下，提供了额外的功能。装饰器模式和继承都是要扩展对象的功能，但是装饰器模式可以提供比继承更好的灵活性。只要在装饰器类中内置一个要被装饰对象的抽象类型引用，就能用这个装饰器类扩展这个抽象类型的所有子类，而如果用继承，就需要针对每个子类进行扩展。

### 什么情况下使用装饰器模式：
- 需要扩展一个类的功能
- 需要动态的为对象增加功能，这些功能可以再动态的撤销
- 需要增加由一些基本功能的排列组合而产生的非常大量的功能，从而使得用继承来扩展不太现实

### 半透明与完全透明的装饰器模式：
**例：**

```
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

```
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

### 装饰器模式与适配器模式的区别：
装饰器和适配器都需要包装源对象，但适配器模式的用意是要改变被包装对象的接口，而装饰器模式的用意是要增强被包装对象的功能。