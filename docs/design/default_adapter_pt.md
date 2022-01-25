## 缺省适配模式
在任何时候，如果不准备实现一个接口所有的方法时，可以制造一个抽象类实现这个接口，为接口的所有方法提供空实现，子类继承这个抽象类，选择重写自己需要的方法。这样，子类就可以只实现自己需要的方法，忽略不必要的方法了，另外，当接口中添加了新的方法时，只需要在抽象类中添加这个方法的默认实现即可，而不需要对每个子类进行修改，这就是缺省适配模式。缺省适配器模式是结构型模式一种，由适配器模式简化而来，省略了适配器模式中目标接口，也就是源接口和目标接口相同，源接口为接口，目标接口为类。

java语言的API中也运用了这种缺省适配模式，且全部遵循一定的命名规范：Abstract + 接口名，
如Map与AbstracMap,List与AbstractList。

### 缺省适配器模式涉及角色：
- 源接口：定义接口类型需要实现的方法
缺省适配器：实现源接口的抽象类（因为这个类不应当实例化） ，对接口中定义的方法给出空实现 
- 实现类：继承缺省适配器 ，选择性的重写需要实现的方法。

**例：**

```
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

