## 合成模式
合成模式，又叫做组合模式、部分-整体模式，属于对象的结构型模式，合成模式将对象组织到树结构中，用来描述整体与部分的关系。合成模式将单个对象和组合对象抽象为同一类型的对象，使得用户对单个对象和组合对象的使用具有一致性。

合成模式，说的是对象包含对象的问题。一个对象中包含其他对象，这些被包含的对象可能是终点对象（不再包含别的对象），也有可能是非终点对象（即组合对象，其内部还包含其他对象），我们将对象称为节点，即一个根节点包含许多子节点，这些子节点有的不再包含子节点，而有的仍然包含子节点。很明显，这是树形结构，终结点叫叶子节点，非终节点叫树枝节点，第一个节点叫根节点。

合成模式的实现根据所实现的接口区别分为两种形式，安全式和透明式。两者的区别在于在什么地方声明子对象的管理方法。

### 安全式：
在树枝节点中，即组合对象中定义子对象的管理方法。
#### 缺点：
不够透明，树枝比树叶多了管理子对象的方法，因此树枝和树叶并不完全相同。

#### 涉及角色：
- 抽象角色：一个抽象的接口，定义树中每个节点默认的行为。
- 树叶节点角色：实现抽象构件接口，组合中没有下级子对象的对象。
- 树枝节点角色：实现抽象构件接口，组合中有下级子对象的对象，除了实现抽象构件给出的方法之外还定义了管理子对象的方法。

#### 代码实现

```
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


### 透明式：
在公共的抽象接口中定义管理子对象的方法。
#### 缺点：
不够安全，由于管理子对象的方法定义在树枝和树叶的公共接口中，因此树叶节点对象也将包含这些管理子对象的方法，但很明显树叶节点是不包含子对象的，因此这些方法在树叶节点中是没有意义的，在运行时可能会出错。
#### 涉及角色：
- 抽象角色：一个抽象的接口，定义树中每个节点默认的行为和管理子对象的行为。
- 树叶节点角色：实现抽象构件接口，组合中没有下级子对象的对象，不给出管理字对象的具体实现。
树枝节点角色：实现抽象构件接口，组合中有下级子对象的对象，给出管理子对象的具体实现。

#### 代码实现

```
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



### 合成模式的应用场景：
合成模式正是应树形结构而生，所以合成模式的使用场景就是出现树形结构的地方。比如：文件目录显示，多及目录呈现等树形结构数据的操作。
参考：https://www.runoob.com/w3cnote/java-composite-pattern-2.html