## 适配器模式
适配器模式是作为两个不兼容的接口之间的桥梁。将一个接口转换成客户希望的另一个接口（这两个接口一般是功能相同或者相近的），得原本由于接口不兼容而不能在一起工作的那些类可以一起工作，这种类型的设计模式属于结构型模式。

结构模式描述如何将类或者对象结合在一起，形成更大的结构。

**结构模式可以分为类的结构模式和对象的结构模式**：
- 类的结构模式：使用继承来把类、接口等组合在一起，形成更大的结构，类的结构模式是静态的
- 对象的结构模式：对象的结构模式描述怎样将各种不同类型的对象组合在一起，对象的结构模式是动态的

### 适配器模式所涉及的角色有：
- 目标角色（Target）:客户所期待的接口
- 源角色（Adaptee）：需要适配的已有接口。
- 适配器角色（Adapter）：将已有接口转换成目标接口。


### 类的适配器模式：
下面的例子示范了通过充电头（适配器）将220v电压转换为5v电压，给手机充电的过程

```
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


### 对象的适配器模式:
只需将适配器类由继承改为组合。（使用组合更加灵活）

```
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


### 适配器在架构层次上的应用举例：
- linux环境运行windows程序，需要软件提供从windos到linux的适配，需要被适配的程序不需要做任何更改。
- 每种数据库的jdbc驱动程序都是java的抽象jdbc接口到数据库引擎的API适配。