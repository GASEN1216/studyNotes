> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_44577346/article/details/117618463?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170832905316800227429039%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170832905316800227429039&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-117618463-null-null.142^v99^pc_search_result_base6&utm_term=22%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F&spm=1018.2226.3001.4187)

#### 目录

*   [设计模式分类](#_2)
*   *   [创建型模式](#_18)
    *   *   [1、单例模式](#1_21)
        *   [2、工厂模式](#2_185)
        *   [3、原型模式](#3_280)
        *   [4、建造者模式](#4_340)
    *   [结构型模式](#_481)
    *   *   [1、代理模式](#1_482)
        *   [2、适配器模式](#2_592)
        *   [3、装饰者模式](#3_702)
        *   [4、桥接模式](#4_855)
        *   [5、外观模式](#5_931)
        *   [6、组合模式](#6_1026)
        *   [7、享元模式](#7_1153)
    *   [行为型模式](#_1250)
    *   *   [1、模板方法模式](#1_1251)
        *   [2、策略模式](#2_1341)
        *   [3、命令模式](#3_1419)
        *   [4、责任链模式](#4_1550)
        *   [5、状态模式](#5_1688)
        *   [6、观察者模式](#6_1872)
        *   [7、中介者模式](#7_1963)
        *   [8、迭代器模式](#8_2082)
        *   [9、访问者模式](#9_2217)
        *   [10、备忘录模式](#10_2341)

**本文参考自黑马、尚硅谷**

设计模式分类
------

*   **创建型模式**  
    用于描述怎么创建对象，主要特点是将对象创建与使用分离。  
    有单例、工厂方法、抽象工厂、原型和建造者这五种模式。
    
*   **结构型模式**  
    用于描述如何将类或对象按某种布局组成更大的结构。  
    有代理、适配器、桥接、装饰、外观、享元和组合这七种模式。
    
*   **行为型模式**  
    用于描述类或对象之间怎样相互协同，共同完成单个对象无法完成的任务，以及怎样分配职责。  
    有模板方法、策略、命令、职责链、状态、观察者、中介者、迭代器、访问者、备忘录、解释器等 11 种行为型模式  
    ![](https://img-blog.csdnimg.cn/ac0b65b807454454afb045a617202c31.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)
    

### 创建型模式

这种模式关注点是：如何创建对象。它将对象创建与使用分离开来，使用者不用关注创建的细节，降低了系统耦合。

#### 1、[单例模式](https://so.csdn.net/so/search?q=%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F&spm=1001.2101.3001.7020)

单一的某个类负责创建自己的对象，并保证只有一个对象被创建出来，在类内部已经实例化好对象，给外部提供一个方法供其访问，外部就不必实例化。

**1.1 单例模式结构**

*   单例类：只能创建一个实例的类
*   访问类：使用单例类

**1.2 单例模式分类**

*   饿汉式：当类加载时实例对象就被创建
*   懒汉式：类加载时不会创建实例对象，当使用时才会被创建

**1.2.1 饿汉式 - 静态变量方式**  
步骤如下：  
(1) 构造器私有化 (防止外部 new 一个实例化对象)  
(2) 类的内部创建对象  
(3) 给外部提供一个方法获取实例化对象

```
public class Singleton {
    private Singleton() {
    }
    private static Singleton singleton = new Singleton();

    public static Singleton getInstance() {
        return singleton;
    }
}

```

优缺点说明：

*   优点：写法简单，在类加载时就创建好对象，避免了线程同步问题
*   缺点：随着类的加载，对象也随之创建好，但是如果此对象一直没有使用，就会造成内存的浪费。

**1.2.2 饿汉式 - 静态代码块方式**

```
public class Singleton {
    private Singleton() {
    }
    private static Singleton singleton;
    static {
        singleton = new Singleton();
    }
    public static Singleton getInstance() {
        return singleton;
    }
}

```

跟静态变量方式类似，只不过将实例化过程放在了静态代码块中。优缺点也与静态变量方式一致。

**1.2.3 懒汉式 (线程不安全)**

```
public class Singleton {
    private Singleton() {
    }
    private static Singleton singleton;
   
    public static Singleton getInstance() {
        if(singleton == null){
            singleton = new Singleton();
        }
        return singleton;
    }
}

```

优缺点说明：

*   优点：实现了懒加载。
*   缺点：在多线程环境下，当一个线程进入 singleton == null 时，还没来得及创建对象，另一个线程进入，这时就会创建多个实例对象，出现线程不安全问题。

**1.2.4 懒汉式 (线程安全)**

```
public class Singleton {
    private Singleton() {
    }
    private static Singleton singleton;
    public static synchronized Singleton getInstance() {
        if(singleton == null){
            singleton = new Singleton();
        }
        return singleton;
    }
}

```

优缺点说明：

*   优点：解决了线程安全问题
*   缺点：效率较低，真正出现线程安全问题的是实例化那一步，当实例化完成后，以后访问只需要返回对象即可，不需要保证线程安全。但是此方法则每次访问时都保证线程安全。

**1.2.5 懒汉式 (双重检查锁)**

```
public class Singleton {
    private Singleton() {
    }
    private static Singleton singleton;
    public static Singleton getInstance() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}

```

优缺点说明：

*   优点：解决了单例、安全和性能等多方面的问题。
*   缺点：多线程环境下，可能会出现空指针问题。

解决方法：对静态变量添加 volatile 关键字

```
public class Singleton {
    private Singleton() {
    }
    private static volatile Singleton singleton;
    public static Singleton getInstance() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}

```

**1.2.6 懒汉式 (静态内部类)**

由内部类实例化对象。在加载外部类时，不会加载静态内部类，只有当静态内部类中属性或方法被调用时，才会被加载。

```
public class Singleton {
    private Singleton() {
    }
    private static class SingletonHolder{
        static final Singleton singleton = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.singleton;
    }
}

```

优点：避免了线程不安全，利用类加载机制实现了懒加载，效率较高。

**1.2.7 恶汉式 (枚举类)**

```
enum  Singleton {
   INSTANCE;
}

```

优点：枚举类先天就是线程安全的，还能防止反序列化创建新的对象。

**注意事项：**

*   单例模式保证了只有一个对象被创建，对于需要频繁创建、销毁的场景或创建对象耗费时间较长的情况，使用单例模式可以提高性能。
*   当使用单例模式时，不能用 new 来创建对象，要使用内部提供给外部的静态方法【getInstance()】来获取到实例化对象。

#### 2、工厂模式

**举例：**  
设计一个咖啡店点餐系统。咖啡店【CoffeeStore】具有点咖啡功能，可以点美式咖啡【AmericanCoffee】和拿铁咖啡【LatteCoffee】，咖啡制作过程为加牛奶，加糖。  
![](https://img-blog.csdnimg.cn/20210611164415911.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)  
在以前的做法中，直接 new 对象，会造成类之间耦合严重，当增加新的咖啡类时，orderCoffee 得修改，违反了开闭原则。所以，我们要使用工厂模式，用工厂来生产对象，我们只要和工厂打交道就可以了。当对象需要修改时，只要在工厂内修改就好了。因此，工厂模式最大的优点：**解耦**。

**2.1 简单工厂模式**

简单工厂模式结构

*   抽象产品：定义了产品的规范，描述了产品主要功能和特性。
*   具体产品：实现或继承抽象产品的子类。
*   具体工厂：提供了创建产品的方法，调用该方法来获取产品。

![](https://img-blog.csdnimg.cn/20210611171233848.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)  
由 SimpleCoffeeFactory 来创建对象，coffeeStore 中的 orderCoffee 方法调用工厂方法中的 createCoffee 来获取对象。解除了 coffeeStore 和  
具体实现类之间的耦合。但是同时增加了 coffeeStore 和工厂的耦合，工厂和具体实现类的耦合。

优缺点说明：

*   优点：封装了创建对象的过程，将对象的创建和使用分开了。直接从工厂类中获取对象，避免了修改客户端对象。
*   缺点：当增加新的类时，还需要修改工厂类。违反开闭原则。

**2.2 工厂方法模式**

定义一个用于创建对象的接口，让子类去决定实例化哪个产品类对象。工厂方法使一个产品类的实例化延迟到其工厂的子类。

工厂方法模式结构

*   抽象工厂：提供了创建产品的接口，调用者通过它访问具体工厂的工厂方法来创建产品。
*   具体工厂：实现抽象工厂中的抽象方法，完成具体产品的创建。
*   抽象产品：定义了产品的规范，描述了产品的主要特性和功能。
*   具体产品：实现了抽象产品角色所定义的接口，由具体工厂来创建。

![](https://img-blog.csdnimg.cn/20210627160754500.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)  
代码如下：

具体工厂

```
public class AmericanCoffeeFactory implements CoffeeFactory {
    public Coffee createCoffee() {
        return new AmericanCoffee();
    }
}
public class LatteCoffeeFactory implements CoffeeFactory {
    public Coffee createCoffee() {
        return new LatteCoffee();
    }
}

```

咖啡店类

```
public class CoffeeStore(){
	private CoffeeFactory factory;
	public CoffeeStore(CoffeeFactory factory){
		this.factory = factory;
	}
	public Coffee orderCoffee(){
		Coffee coffee = factory.createCoffee();
		coffee.addMilk();
		coffee.addSugar();
		return coffee;
	}
}

```

优点：

*   用户只需知道具体工厂的名称就可得到想要的产品，无需了解创建的细节
*   当增加新的产品时，不用修改原有代码，符合开闭原则

缺点：

*   当增加新的产品时，就需要增加一个新的具体产品类和一个具体工厂类，使整个系统比较复杂。

**2.3 抽象工厂模式**

工厂方法模式考虑的是一类产品的生产，如畜牧场只养动物，电视机厂只生产电视。  
这些工厂只生产同种类产品，同种类产品称为同等级产品，也就是说：工厂方法模式只生产同等级的产品，但是在现实生活中，大多是综合性的工厂，能生产多等级的产品，电器厂既生产电视机，又生产洗衣机，空调等。  
抽象工厂模式考虑的是多等级产品的生产，将同一个具体工厂所生产的位于不同等级的产品称为一个产品族。

抽象工厂模式结构：

*   抽象工厂：提供了创建产品的接口，调用者通过它访问具体工厂的工厂方法来创建产品。
*   具体工厂：实现抽象工厂中的抽象方法，完成具体产品的创建。
*   抽象产品：定义了产品的规范，描述了产品的主要特性和功能。
*   具体产品：实现了抽象产品角色所定义的接口，由具体工厂来创建。

现咖啡店业务发生改变，不仅要生产咖啡还要生产甜点，如提拉米苏、抹茶慕斯等，要是按照工厂方法模式，需要定义抽象甜品类，提拉米苏类、抹茶慕斯类、提拉米苏工厂、抹茶慕斯工厂、甜点工厂类，很容易发生类爆炸情况。其中拿铁咖啡、美式咖啡是一个产品等级，都是咖啡；提拉米苏、抹茶慕斯也是一个产品等级；拿铁咖啡和提拉米苏是同一产品族（也就是都属于意大利风味），美式咖啡和抹茶慕斯是同一产品族（也就是都属于美式风味）。所以这个案例可以使用抽象工厂模式实现。类图如下：

![](https://img-blog.csdnimg.cn/20210627171803539.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)  
优缺点说明：

*   优点：当一个产品族中包含有多个对象时，能保证只使用同一个产品族中对象。
*   缺点：当产品族中需要增加一个新的产品时，所有的工厂类都需要进行修改。

#### 3、原型模式

用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型对象相同的新对象。

结构：

*   抽象原型类：规定了具体原型对象必须实现的 clone() 方法
*   具体原型类：实现抽象原型类的 clone() 方法，它是可被复制的对象。
*   访问类：使用具体原型类中的 clone() 方法来复制新的对象。

原型模式的克隆分为浅克隆和深克隆：

```
浅克隆：创建一个新对象，新对象的属性和原来对象完全相同，对于非基本类型属性，仍指向原有属性所指向的对象的内存地址。
深克隆：创建一个新对象，属性中引用的其他对象也会被克隆，不再指向原有对象地址。

```

同一学校的 “三好学生” 奖状除了获奖人姓名不同，其他都相同，可以使用原型模式复制多个 “三好学生” 奖状出来，然后在修改奖状上的名字即可。

![](https://img-blog.csdnimg.cn/20210627181114127.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
public class Citation implements Cloneable {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public void show(){
        System.out.println(name + "获得了三好学生奖状");
    }
}

```

```
public class CitationTest {
    public static void main(String[] args) throws CloneNotSupportedException {
        Citation c1 = new Citation();
        c1.setName("张三");
        Citation c2 = (Citation) c1.clone();
        c2.setName("李四");
        c1.show();
        c2.show();
    }
}

```

注意事项与细节：

1.  创建新的对象比较复杂时，可以利用原型模式简化对象创建过程，同时也能提高效率。
2.  不用重新初始化对象，而是动态的获得对象运行时状态。
3.  如果原始对象发生变化，其他克隆对象也会发生相应变化，无需修改代码。
4.  在实现深克隆的时候可能需要复杂代码。

#### 4、建造者模式

*   建造者模式又叫生成器模式，是一种对象构建模式。它可以将复杂对象的建造过程抽象出来，使这个抽象过程的不同实现方法可以构造出不同表现的对象。
    
*   建造者模式是一步步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。
    

4.1 建造者模式的四个角色：

1.  产品角色：一个具体的产品对象。
2.  抽象建造者：创建一个产品对象的各个部件指定的接口 / 抽象类
3.  具体建造者：实现接口，构建和装配各个部件。
4.  指挥者：构建一个使用抽象建造者接口的对象。它主要是用于创建一个复杂的对象。有两个作用：一是：隔离了客户与对象的生产过程。二是：负责控制产品对象的生产过程。

4.2 建造者模式原理类图  
![](https://img-blog.csdnimg.cn/20210705173617710.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

4.3 实例

创建共享单车。生产自行车是一个复杂的过程，它包含了车架、车座等组件的生产。而车架又有碳纤维，铝合金等，车座有橡胶，真皮等材质。对于自行车的生产就可以使用建造者模式。  
这里 Bike 是产品，包含车架，车座等组件；Builder 是抽象建造者，MobikeBuilder 和 OfoBuilder 是具体的建造者；Director 是指挥者。类图如下：

![](https://img-blog.csdnimg.cn/20210705182959473.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)  
具体代码如下：

```
//自行车类
public class Bike {
    private String frame;
    private String seat;

    public String getFrame() {
        return frame;
    }

    public void setFrame(String frame) {
        this.frame = frame;
    }

    public String getSeat() {
        return seat;
    }

    public void setSeat(String seat) {
        this.seat = seat;
    }
}

//抽象Builder类
public abstract class Builder {
    protected Bike bike = new Bike();
    public abstract void buildFrame();
    public abstract void buildSeat();
    public abstract Bike createBike();
}

//摩拜单车Builder类
public class MobikeBuilder extends Builder {

    @Override
    public void buildFrame() {
        bike.setFrame("铝合金框架");
    }

    @Override
    public void buildSeat() {
        bike.setSeat("橡胶座椅");
    }

    @Override
    public Bike createBike() {
        return bike;
    }
}

//ofoBuilder类
public class OfoBuilder extends Builder {

    @Override
    public void buildFrame() {
        bike.setFrame("碳纤维框架");
    }

    @Override
    public void buildSeat() {
        bike.setSeat("真皮座椅");
    }

    @Override
    public Bike createBike() {
        return bike;
    }
}

//指挥者类
public class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }
    public Bike construct(){
        builder.buildFrame();
        builder.buildSeat();
        return builder.createBike();
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        Director director = new Director(new OfoBuilder());
        Bike bike = director.construct();
        System.out.println(bike.getFrame());
        System.out.println(bike.getSeat());
    }
}

```

4.4 优缺点说明

优点：

*   建造者模式的封装性很好。使用建造者模式可以很好的封装变化。
*   在建造者模式中，客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象。
*   可以更加精细控制产品创建过程。将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰，也更加方便使用程序来控制创建过程。

缺点：  
建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，如果产品之间差异性很大，则不适合使用建造者模式，因此其适用范围受到一定限制。

4.5 对比

**工厂方法模式 VS 建造者模式**：

工厂方法模式注重的是整体对象的创建方式；而建造者模式注重的是部件构建的过程，意在通过一步一步地精确构造创建出一个复杂的对象。  
举个简单例子来说明两者差异，如果要制造一个超人，如果使用工厂方法模式，直接产生出来的就是一个力大无穷，能够飞翔，内裤外穿的超人；而如果使用建造者模式，则需要组装手、头、脚、躯干等部分，然后再把内裤外穿，于是一个超人就诞生了。

**抽象工厂模式 VS 建造者模式**：

抽象工厂模式实现对产品家族的创建，一个产品家族是这样的一系列产品：具有不同分类维度的产品组合，采用抽象工厂模式则不需要关心构建过程，只关心什么产品由什么工厂生产即可。  
建造者模式则是要求按照指定的蓝图建造产品，它的主要目的是通过组装零件而产生一个新产品。  
如果将抽象工厂模式看成汽车配件生产工厂，生产一个产品族的产品，那么建造者模式就是一个汽车组装工厂，通过对部件的组装可以返回一辆完整的汽车。

### 结构型模式

#### 1、代理模式

为一个对象提供一个替身，以控制对这个对象的访问。即通过代理对象访问目标对象。这样做的好处是：可以在目标对象实现的基础上，增强额外的功能操作，即扩展目标对象的功能。  
Java 中的代理按照代理类生成时机不同又分为**静态代理**和**动态代理**。静态代理在编译期就生成，而动态代理类则是在 Java 运行时动态生成。动态代理又有 JDK 代理和 Cglib 代理两种。

1.1 结构

*   抽象主题类：通过接口或抽象类声明真实主题和代理对象实现的业务方法。
*   真实主题类：实现了抽象主题中的具体业务，是代理对象所代表的真实对象，是最终要引用的对象。
*   代理类：提供了与真实主题相同的接口，其内部含有对真实主题的引用，它可以访问、控制或扩展真实主题的功能。

1.2 静态代理  
如果要买火车票的话，需要去火车站买票，坐车到火车站，排队等一系列的操作，显然比较麻烦。而火车站在多个地方都有代售点，我们去代售点买票就方便很多了。这个例子其实就是典型的代理模式，火车站是目标对象，代售点是代理对象。类图如下：  
![](https://img-blog.csdnimg.cn/20210707155854369.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//抽象主题类
public interface SellTickets {
    void sell();
}

//真实主题类
public class TrainStation implements SellTickets {
    @Override
    public void sell() {
        System.out.println("火车站卖票");
    }
}

//代理类
public class ProxyPoint implements SellTickets {
    private TrainStation station = new TrainStation();
    @Override
    public void sell() {
        System.out.println("收取手续费");
        station.sell();
    }
}

//用户
public class Client {
    public static void main(String[] args) {
        ProxyPoint proxy = new ProxyPoint();
        proxy.sell();
    }
}

```

从中可以看出，用户直接访问的是 ProxyPoint 类对象，也就是说 ProxyPoint 作为访问对象和目标对象的中介。同时也对 sell() 方法进行了增强 (代理点收取一些费用)。

1.3 JDK 动态代理  
Java 中提供了一个动态代理类 Proxy,Proxy 并不是我们上述所说的代理对象的类，而是提供了一个创建代理对象的静态方法 (newProxyInstance 方法) 来获取代理对象。

```
//抽象主题类
public interface SellTickets {
    void sell();
}

//真实主题类
public class TrainStation implements SellTickets {
    @Override
    public void sell() {
        System.out.println("火车站卖票");
    }
}

//JDK动态代理
public class ProxyFactory {
    private TrainStation station = new TrainStation();

    public SellTickets getProxyObject(){
       SellTickets sellTickets = (SellTickets) Proxy.newProxyInstance(station.getClass().getClassLoader(), station.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("收取手续费");
                Object result = method.invoke(station, args);
                return result;
            }
        });
       return sellTickets;
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        ProxyFactory proxyFactory = new ProxyFactory();
        SellTickets proxyObject = proxyFactory.getProxyObject();
        proxyObject.sell();
    }
}

```

执行流程如下：

1.  在用户类中通过代理对象调用 sell() 方法
2.  根据多态特性，执行的是代理类中的 sell() 方法
3.  代理类中的 sell() 方法中又调用了 InvocationHandler 接口的子实现类对象的 invoke 方法
4.  invoke 方法通过反射执行了真实对象所属类中的 sell() 方法

1.4 动态代理和静态代理对比：

动态代理与静态代理相比，最大的好处是接口中声明的所有方法都被转移到调用处理器一个集中的方法中处理。这样，在接口数量比较多的时候，我们可以灵活处理，而不需像静态代理那样在每一个方法中中转。  
如果接口增加一个方法，静态代理除了所有实现类需要实现这个方法外，所有代理类也需要实现此方法。增加了代码维护复杂度。

1.5 代理模式优点：

优点：

*   代理模式在客户端与目标对象之间起到一个中介作用和保护目标对象的作用
*   代理对象可以扩展目标对象的功能
*   代理对象能将客户端与目标对象分离，在一定程度上降低了系统的耦合度。

#### 2、适配器模式

将一个类的接口转换成客户希望的另一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。  
适配器模式分为类适配器模式和对象适配器模式，前者类之间的耦合度比后者高，且要求程序员了解现有组件库中的相关组件的内部结构，所以应用相对较少些。

2.1 结构

*   目标接口：当前系统业务所期待的接口，它可以是抽象类或接口。
*   适配者类：它是被访问和适配的现存组件库中的组件接口。
*   适配器类：它是一个转换器，通过继承或引用适配者的对象，把适配者接口转换成目标接口，让客户按目标接口的格式访问适配者。

2.2 类适配器模式  
实现方式：定义一个适配器类来实现当前系统的业务接口，同时又继承现有组件库中已经存在的组件。

现有一台电脑只能读取 SD 卡，而要读取 TF 卡中的内容的话就需要使用到适配器模式。创建一个读卡器，将 TF 卡中的内容读取出来。  
类图如下：  
![](https://img-blog.csdnimg.cn/20210708162315593.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//tf卡接口
public interface TFCard {
    String readTF();
    void writeTF(String message);
}
//tf卡实现类
public class TFCardImpl implements TFCard {
    @Override
    public String readTF() {
        String msg = "read TF card";
        return msg;
    }
    @Override
    public void writeTF(String message) {
        System.out.println("write TFCard:" + message);
    }
}

//sd卡接口
public interface SDCard {
    String readSD();
    void writeSD(String message);
}

//适配器类
public class SDCardAdapterTFCard extends TFCardImpl implements SDCard {
    @Override
    public String readSD() {
        System.out.println("adapter read tf card ");
        return readTF();
    }
    @Override
    public void writeSD(String message) {
        System.out.println("adapter write tf card");
        writeTF(message);
    }
}
//电脑类
public class Computer {
    public String readSD(SDCard sdCard){
        return sdCard.readSD();
    }
}
//用户类
public class Client {
    public static void main(String[] args) {
        Computer computer = new Computer();
        SDCard adapter = new SDCardAdapterTFCard();
        computer.readSD(adapter);
    }
}

```

由上述代码可知，类适配器模式违背了合成复用原则。同时由于适配器继承了 TFCardImpl 类，一个类只能继承于一个父类，所以，这个适配器只能为这一个类服务。

2.3 对象适配器模式

实现方式：对象适配器模式可采用将现有组件库中已经实现的组件引入适配器类中，该类同时实现当前系统的业务接口。

类图如下：  
![](https://img-blog.csdnimg.cn/20210708165802960.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)  
其他代码没变，只需修改适配器代码和用户类代码

```
//适配器类
public class SDCardAdapterTFCard implements SDCard {
    private TFCard tfCard;
    public SDCardAdapterTFCard(TFCard tfCard) {
        this.tfCard = tfCard;
    }
    @Override
    public String readSD() {
        System.out.println("adapter read tf card ");
        return tfCard.readTF();
    }
    @Override
    public void writeSD(String message) {
        System.out.println("adapter write tf card");
        tfCard.writeTF(message);
    }
}
//用户类
public class Client {
    public static void main(String[] args) {
        Computer computer = new Computer();
        TFCard tfCard = new TFCardImpl();
        SDCard adapter = new SDCardAdapterTFCard(tfCard);
        computer.readSD(adapter);
    }
}

```

#### 3、装饰者模式

指在不改变现有对象结构的情况下，动态地给该对象增加一些职责 (即增加其额外功能) 的模式。

3.1 结构

*   抽象构件角色：定义一个抽象接口以规范准备接收附加责任的对象。
*   具体构件角色：实现抽象构件，通过装饰角色为其添加一些职责。
*   抽象装饰角色：继承或实现抽象构件，并包含具体构件的实例，可以通过子类扩展具体构件的功能。
*   具体装饰角色：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。

快餐店有炒面、炒饭这些快餐，可以额外附加鸡蛋、火腿、培根这些配菜，当然加配菜需要额外加钱，每个配菜的价钱通常不太一样，那么计算总价就会显得比较麻烦。

![](https://img-blog.csdnimg.cn/20210709163255641.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//快餐接口
public abstract class FastFood {
    private float price;
    private String desc;

    public FastFood() {
    }

    public FastFood(float price, String desc) {
        this.price = price;
        this.desc = desc;
    }

    public float getPrice() {
        return price;
    }

    public void setPrice(float price) {
        this.price = price;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }
    public abstract float cost();
}

//炒饭
public class FriedRice extends FastFood {
    public FriedRice() {
        super(12,"一份炒饭");
    }

    @Override
    public float cost() {
        return getPrice();
    }
}

//炒面
public class FriedNoodles extends FastFood {
    public FriedNoodles() {
        super(10,"一份炒面");
    }

    @Override
    public float cost() {
        return getPrice();
    }
}

//配料类
public abstract class Garnish extends FastFood {

    private FastFood fastFood;

    public Garnish(FastFood fastFood,float price, String desc) {
        super(price, desc);
        this.fastFood = fastFood;
    }

    public FastFood getFastFood() {
        return fastFood;
    }

    public void setFastFood(FastFood fastFood) {
        this.fastFood = fastFood;
    }
}

//鸡蛋配料
public class Egg extends Garnish {
    public Egg(FastFood fastFood) {
        super(fastFood, 1,"一个鸡蛋");
    }

    @Override
    public float cost() {
        return getFastFood().cost() + getPrice();
    }

    @Override
    public String getDesc() {
        return super.getDesc() + getFastFood().getDesc();
    }
}

//培根配料
public class Bacon extends Garnish {
    public Bacon(FastFood fastFood) {
        super(fastFood, 2,"一个培根");
    }

    @Override
    public float cost() {
        return getFastFood().cost() + getPrice();
    }

    @Override
    public String getDesc() {
        return super.getDesc() + getFastFood().getDesc();
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        FastFood fastFood = new FriedNoodles();
        System.out.println(fastFood.getDesc() +"价格：" + fastFood.cost());
        System.out.println("=======================");
        FastFood fastFood1 = new Egg(fastFood);
        System.out.println(fastFood1.getDesc()+ "价格" + fastFood1.cost());
        System.out.println("========================");
        FastFood fastFood2 = new Egg(fastFood1);
        System.out.println(fastFood2.getDesc()+ "价格" + fastFood2.cost());
        System.out.println("========================");
    }
}

```

3.2 优点

*   装饰者模式比继承更加灵活，可以通过组合不同的装饰者对象来获取具有不同行为状态的多样化结果。继承是静态的附加责任，装饰者是动态地附加责任。
*   装饰类和被装饰类可以独立发展，不会相互耦合。

3.3 代理和装饰者区别

*   相同点：  
    1、都要实现与目标类相同的业务接口  
    2、在两个类中都要声明目标对象  
    3、都可以在不修改目标类的前提下增强目标方法
    
*   不同点：  
    1、目的不同。装饰者是为了增强目标对象，静态代理是为了保护和隐藏目标对象。  
    2、获取目标对象构建的地方不同。装饰者是由外界传递进来，可以通过构造方法传递。静态代理是在代理内部创建，以此来隐藏目标对象。
    

#### 4、桥接模式

将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。

4.1 结构

*   抽象化角色：定义抽象类，并包含一个对实现化对象的引用。
*   扩展抽象化角色：是抽象化角色的子类，实现父类中的业务方法，并通过组合关系调用实现化角色中的业务方法。
*   实现化角色：定义实现化角色的接口，供扩展抽象化角色调用。
*   具体实现化角色：给出实现化角色接口的具体实现。

需要开发一个跨平台视频播放器，可以在不同操作系统平台（如 Windows、Mac、Linux 等）上播放多种格式的视频文件，常见的视频格式包括 RMVB、AVI、WMV 等。该播放器包含了两个维度，适合使用桥接模式。

类图如下：  
![](https://img-blog.csdnimg.cn/20210710153010343.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//视频文件
public interface VideoFile {
    void decode(String fileName);
}

//avi文件
public class AVIFile implements VideoFile {
    @Override
    public void decode(String fileName) {
        System.out.println("AVI视频文件" + fileName);
    }
}

//rmvb文件
public class RMVBFile implements VideoFile {
    @Override
    public void decode(String fileName) {
        System.out.println("RMVB视频文件" + fileName);
    }
}

//操作系统
public abstract class OperatingSystem {
    protected VideoFile videoFile;

    public OperatingSystem(VideoFile videoFile) {
        this.videoFile = videoFile;
    }
    public abstract void play(String fileName);
}

//windows
public class Windows extends OperatingSystem {
    public Windows(VideoFile videoFile) {
        super(videoFile);
    }

    @Override
    public void play(String fileName) {
        videoFile.decode(fileName);
    }
}

//mac
public class Mac extends OperatingSystem {
    public Mac(VideoFile videoFile) {
        super(videoFile);
    }

    @Override
    public void play(String fileName) {
        videoFile.decode(fileName);
    }
}

```

4.2 优点

*   桥接模式提高了系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统。
*   实现细节对客户透明。

#### 5、外观模式

又名门面模式，是一种通过多个复杂的子系统提供一个一致的接口，而使这些子系统更加容易被访问的模式。该模式对外有一个统一接口，外部应用程序不用关心内部子系统的具体细节，这样会大大降低应用程序的复杂度，提高程序的可维护性。

5.1 结构

*   外观角色：为多个子系统对外提供一个共同的接口。
*   子系统角色：实现系统的部分功能，客户可以通过外观角色访问它。

小明的爷爷已经 60 岁了，一个人在家生活：每次都需要打开灯、打开电视、打开空调；睡觉时关闭灯、关闭电视、关闭空调；操作起来都比较麻烦。所以小明给爷爷买了智能音箱，可以通过语音直接控制这些智能家电的开启和关闭。

类图如下：  
![](https://img-blog.csdnimg.cn/20210710161415830.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//灯类
public class Light {
    public void on(){
        System.out.println("打开灯");
    }
    public void off(){
        System.out.println("关闭灯");
    }
}

//电视类
public class TV {
    public void on(){
        System.out.println("打开电视");
    }
    public void off(){
        System.out.println("关闭电视");
    }
}

//空调类
public class AirCondition {
    public void on(){
        System.out.println("打开空调");
    }
    public void off(){
        System.out.println("关闭空调");
    }
}

//智能音箱
public class SmartApplicationFacade {
    private Light light;
    private TV tv;
    private AirCondition airCondition;

    public SmartApplicationFacade() {
        this.light = new Light();
        this.tv = new TV();
        this.airCondition = new AirCondition();
    }
    public void say(String message){
        if(message.contains("on")){
            on();
        }else if(message.contains("off")){
            off();
        }else{
            System.out.println("我听不懂您说的");
        }
    }
    private void on(){
        light.on();
        tv.on();
        airCondition.on();
    }
    private void off(){
        light.off();
        tv.off();
        airCondition.off();
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        SmartApplicationFacade smartApplicationFacade = new SmartApplicationFacade();
        smartApplicationFacade.say("on");
    }
}

```

5.2 优缺点说明

优点：

*   降低了子系统与客户端耦合度，使得子系统的变化不会影响调用它的客户类。
*   对客户屏蔽了子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易。

缺点：  
不符合开闭原则，修改很麻烦。

#### 6、组合模式

又名部分整体模式。是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。

6.1 结构

*   抽象根节点：定义系统各层次对象的共有方法和属性，可以预先定义一些默认行为和属性
*   树枝节点：定义树枝节点的行为，存储子节点，组合树枝节点和叶子节点形成一个树形结构。
*   叶子节点：叶子节点对象，其下再无分支，是系统层次遍历的最小单位。

如下图，我们在访问别的一些管理系统时，经常可以看到类似的菜单。一个菜单可以包含菜单项（菜单项是指不再包含其他内容的菜单条目），也可以包含带有其他菜单项的菜单，因此使用组合模式描述菜单就很恰当，我们的需求是针对一个菜单，打印出其包含的所有菜单以及菜单项的名称。

![](https://img-blog.csdnimg.cn/20210711151820489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70)  
类图如下：  
![](https://img-blog.csdnimg.cn/20210711154513536.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//菜单组件
public abstract class MenuComponent {
    protected String name;
    protected int level;

    public MenuComponent(String name, int level) {
        this.name = name;
        this.level = level;
    }

    public void add(MenuComponent menuComponent){
        throw new UnsupportedOperationException();
    }
    public void remove(MenuComponent menuComponent){
        throw new UnsupportedOperationException();
    }
    public MenuComponent getChild(int i){
        throw new UnsupportedOperationException();
    }
    public String getName(){
        return name;
    }
    public void print(){
        throw new UnsupportedOperationException();
    }
}

//菜单
public class Menu extends MenuComponent {
    private List<MenuComponent> list;

    public Menu(String name,int level) {
       super(name,level);
       list = new ArrayList<>();
    }

    @Override
    public void add(MenuComponent menuComponent) {
        list.add(menuComponent);
    }

    @Override
    public void remove(MenuComponent menuComponent) {
        list.remove(menuComponent);
    }

    @Override
    public MenuComponent getChild(int i) {
        return list.get(i);
    }

    @Override
    public void print() {
        for (int i = 0; i < level; i++) {
            System.out.print("==");
        }
        System.out.println(name);
        for (MenuComponent menuComponent : list) {
            menuComponent.print();
        }
    }
}

//菜单项
public class MenuItem extends MenuComponent{

    public MenuItem(String name, int level) {
        super(name, level);
    }

    @Override
    public void print() {
        for (int i = 0; i < level; i++) {
            System.out.print("==");
        }
        System.out.println(name);
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        MenuComponent menu1 = new Menu("菜单管理", 2);
        menu1.add(new MenuItem("页面访问",3));
        menu1.add(new MenuItem("展开菜单",3));
        menu1.add(new MenuItem("编辑菜单",3));
        menu1.add(new MenuItem("删除菜单",3));
        menu1.add(new MenuItem("新增菜单",3));
        
        MenuComponent menu2 = new Menu("权限配置", 2);
        menu2.add(new MenuItem("页面访问",3));
        menu2.add(new MenuItem("提交保存",3));
        
        MenuComponent menu3 = new Menu("角色管理", 2);
        menu3.add(new MenuItem("页面访问",3));
        menu3.add(new MenuItem("新增角色",3));
        menu3.add(new MenuItem("修改角色",3));
        
        MenuComponent menu = new Menu("系统管理",1);
        menu.add(menu1);
        menu.add(menu2);
        menu.add(menu3);
        menu.print();
    }

```

6.2 优点

*   组合模式可以清楚地定义分层次的复杂对象，表示对象的全部或部分层次，它让客户端忽略了层次的差异，方便对整个层次结构进行控制。
*   客户端可以一致的使用一个组合结构或其中单个对象，不必关心处理的是单个对象还是整个组合结构，简化了客户端代码。
*   在组合模式中增加新的树枝节点和叶子节点都很方便，无需对原有类库机进行修改，符合开闭原则。

#### 7、享元模式

运用共享技术来有效的支持大量细粒度对象的复用。它通过共享已经存在的对象来大幅度减少需要创建的对象数量，避免大量相似对象的开销，从而提高系统资源的利用率。

享元模式存在以下两种状态：

*   内部状态：既不会随着环境的改变而改变的可共享部分。
*   外部状态：指随着环境改变而改变的不可以共享的部分。  
    享元模式的实现要领就是区分应用中的这两种状态，并将外部状态外部化。

7.1 结构

*   抽象享元角色：通常是一个接口或抽象类，在抽象享元类中声明了具体享元类公共的方法，这些方法可以向外界提供享元对象的内部数据，同时也可通过这些方法来设置外部数据。
*   具体享元角色：它实现了抽象享元类，称为享元对象；在具体享元类中为内部状态提供了存储空间。通常我们可以结合单例模式来设计具体享元类，为每一个具体享元类提供唯一的享元对象。
*   非享元角色：并不是所有的抽象享元类的子类都需要被共享，不能被共享的子类可设计为非共享具体享元类；当需要一个非共享享元类的对象时可以直接通过实例化创建。
*   享元工厂角色：负责创建和管理共享角色。当客户对象请求一个享元对象时，享元工厂检查系统中是否存在符合要求的享元对象，如果存在则提供给客户；如果不存在，就创建一个新的享元对象。

下面的图片是众所周知的俄罗斯方块中的一个个方块，如果在俄罗斯方块这个游戏中，每个不同的方块都是一个实例对象，这些对象就要占用很多的内存空间，下面利用享元模式进行实现。

![](https://img-blog.csdnimg.cn/20210713165141706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70)  
类图如下：  
![](https://img-blog.csdnimg.cn/20210713170101266.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//抽象方块类
public abstract class AbstractBox {
    public abstract String getShape();
    public void display(String color){
        System.out.println("形状是：" + getShape() + ",颜色是：" + color);
    }
}

//I型方块
public class IBox extends AbstractBox{

    @Override
    public String getShape() {
        return "I";
    }
}

//L型方块
public class LBox extends AbstractBox{

    @Override
    public String getShape() {
        return "L";
    }
}

//O型方块
public class OBox extends AbstractBox{

    @Override
    public String getShape() {
        return "O";
    }
}

//工厂类
public class BoxFactory {
    private HashMap<String,AbstractBox> map;

    public BoxFactory() {
        map = new HashMap<>();
        AbstractBox IBox = new IBox();
        AbstractBox LBox = new LBox();
        AbstractBox OBox = new OBox();
        map.put("I",IBox);
        map.put("L",LBox);
        map.put("O",OBox);
    }
    public static BoxFactory getInstance(){
        return new BoxFactory();
    }
    public AbstractBox getBox(String key){
        return map.get(key);
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        BoxFactory boxFactory = BoxFactory.getInstance();
        AbstractBox box = boxFactory.getBox("I");
        box.display("红色");
    }
}

```

7.2 优缺点说明

优点：

*   极大减少内存中相似或相同对象数量，节约系统资源，提高系统性能。
*   享元模式中的外部状态相对独立，且不影响内部状态。

缺点：  
为了使对象可以共享，需要将享元对象的部分状态外部化，分离内部状态和外部状态，使程序逻辑复杂。

### 行为型模式

#### 1、模板方法模式

在面向对象程序设计过程中，程序员常常遇到这种情况：设计一个系统时，知道了算法所需的关键步骤，而且确定了这些步骤的执行顺序，但某些步骤的具体实现还未知，或者说某些步骤的实现与具体的环境相关。

定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤。

1.1 结构

*   抽象类：负责给出一个算法的轮廓和骨架。它由一个模板方法和若干个基本方法构成。
    
    *   模板方法 ：定义了算法的骨架，按某种顺序调用其包含的基本方法。
        
    *   基本方法：是实现算法各个步骤的方法，是模板方法的组成部分。基本方法又可以分为三种：
        
        *   抽象方法：一个抽象方法由抽象类声明和其具体子类实现。
        *   具体方法：一个具体方法由一个抽象类或具体类声明并实现，其子类可以进行覆盖，也可以直接继承。
        *   钩子方法：在抽象类中已经实现，包括用于判断的逻辑方法和需要子类重写的空方法两种。
    *   具体子类：实现抽象类中定义的抽象方法和钩子方法，它们是一个顶级逻辑的组成步骤。
        

炒菜的步骤是固定的，分为倒油、热油、倒蔬菜、倒调料品、翻炒等步骤。现通过模板方法模式来用代码模拟。  
类图如下：  
![](https://img-blog.csdnimg.cn/20210716155332850.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//抽象类
public abstract class AbstractClass {
    public void cookProcess(){
        pourOil();
        heatOil();
        pourVegetables();
        pourSauce();
        fry();
    }
    public void pourOil(){
        System.out.println("倒油");
    }
    public void heatOil(){
        System.out.println("热油");
    }
    public abstract void pourVegetables();
    public abstract void pourSauce();
    public void fry(){
        System.out.println("翻炒");
    }
}

//菜心具体子类
public class CaiXin extends AbstractClass {
    @Override
    public void pourVegetables() {
        System.out.println("下菜心");
    }

    @Override
    public void pourSauce() {
        System.out.println("下蒜蓉");
    }
}

//包菜具体子类
public class BaoCai extends AbstractClass {
    @Override
    public void pourVegetables() {
        System.out.println("下包菜");
    }

    @Override
    public void pourSauce() {
        System.out.println("下辣椒");
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        AbstractClass abstractClass = new CaiXin();
        abstractClass.cookProcess();
    }
}

```

1.2 优缺点说明：

优点：

*   提高代码复用性。将相同部分的代码放在抽象的父类中，而将不同的代码放入不同的子类中。
*   实现了反向控制。通过一个父类调用子类的操作，通过对子类的具体实现扩展不同的行为，实现了反向控制，并符合 “开闭原则”。

缺点：

*   对每个不同的实现都需要定义一个子类，这会导致类的个数增加，系统更加庞大。
*   父类中的抽象方法由子类实现，子类执行的结果会影响父类的如果，这导致一种反向的控制结构，他提高了代码阅读的难度。

#### 2、策略模式

该模式定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的变化不会影响使用算法的客户。策略模式属于对象行为模式，它通过对算法进行封装，把使用算法的责任和算法的实现分割开来，并委派给不同的对象对这些算法进行管理。

2.1 结构

*   抽象策略类：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。
*   具体策略类：实现了抽象策略定义的接口，提供具体的算法或行为。
*   环境类：持有一个策略类的引用，最终给客户端调用。

一家百货公司在定年度的促销活动。针对不同的节日（春节、中秋节、圣诞节）推出不同的促销活动，由促销员将促销活动展示给客户。

类图如下：  
![](https://img-blog.csdnimg.cn/20210718160656375.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//抽象策略类
public interface Strategy {
    void show();
}

//具体策略类A
public class StrategyA implements Strategy {
    @Override
    public void show() {
        System.out.println("买一送一");
    }
}

//具体策略类B
public class StrategyB implements Strategy {
    @Override
    public void show() {
        System.out.println("满一百减五十");
    }
}

//具体策略类C
public class StrategyC implements Strategy {
    @Override
    public void show() {
        System.out.println("送三十元优惠券");
    }
}

//环境角色
public class SalesMan {
    private Strategy strategy;

    public SalesMan(Strategy strategy) {
        this.strategy = strategy;
    }
    public void salesManShow(){
        strategy.show();
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        Strategy strategy = new StrategyB();
        SalesMan salesMan = new SalesMan(strategy);
        salesMan.salesManShow();
    }
}

```

2.1 优缺点说明

优点：

*   策略类之间可以自由切换
*   易于扩展，增加一个新的策略只需要添加一个具体的策略类即可，不需要改变原有代码，符合开闭原则。

缺点：

*   客户端必须知道所有的策略类，并自行决定使用哪一个策略类。
*   策略模式将造成产生很多策略类，可以通过享元模式在一定程度上减少对象的数量。

#### 3、命令模式

将一个请求封装成一个对象，使发出请求的责任与执行请求的责任分隔开。这样两者之间通过命令对象进行沟通，这样方便将命令对象进行存储、传递、调用和管理。

3.1 结构

*   抽象命令类：定义命令的接口，声明执行的方法。
*   具体命令类：具体的命令，实现命令接口，通常会持有接收者，并调用接收者的功能来完成命令要执行的操作。
*   接收者：接收者，真正执行命令的角色。任何类都可能成为一个接收者，只要他能够实现命令要求的相应功能。
*   请求者：要求命令对象执行请求，通常会持有命令对象，可以持有很多的命令对象。这个是客户端真正触发命令并要求命令执行相应操作的地方，也就是说相当于使用命令对象的入口。

![](https://img-blog.csdnimg.cn/20210718163148653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70)  
服务员：请求者，由她来发起命令。  
资深大厨：接收者，真正执行命令的对象。  
订单：命令中包含订单。

![](https://img-blog.csdnimg.cn/20210718170448699.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//抽象命令类
public interface Command {
    void execute();
}

//订单
public class Order {
    private int diningTable;
    private Map<String,Integer> foodDic = new HashMap<>();

    public int getDiningTable() {
        return diningTable;
    }

    public void setDiningTable(int diningTable) {
        this.diningTable = diningTable;
    }

    public Map<String, Integer> getFoodDic() {
        return foodDic;
    }

    public void setFoodDic(String name,int num) {
       foodDic.put(name,num);
    }
}

//厨师
public class SeniorChef {
    public void makeFood(int num,String foodName){
        System.out.println(num + "份"+ foodName);
    }
}

//具体命令类
public class OrderCommand implements Command{

    private SeniorChef seniorChef;
    private Order order;

    public OrderCommand(SeniorChef seniorChef, Order order) {
        this.seniorChef = seniorChef;
        this.order = order;
    }

    @Override
    public void execute() {
        System.out.println(order.getDiningTable()+"桌的订单");
        Set<String> keys = order.getFoodDic().keySet();
        for (String key : keys) {
          seniorChef.makeFood(order.getFoodDic().get(key), key);
        }
        System.out.println(order.getDiningTable()+"桌订单完成");
    }
}

//调用者类
public class Waitor {
    private ArrayList<Command> commands = new ArrayList<>();

    public void setCommand(Command cmd) {
      commands.add(cmd);
    }
    public void orderUp(){
        System.out.println("叮咚，新订单来了");
        for (Command command : commands) {
            command.execute();
        }
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        SeniorChef seniorChef = new SeniorChef();

        Order order1 = new Order();
        order1.setDiningTable(1);
        order1.setFoodDic("爆炒肉丝",1);
        order1.setFoodDic("溜肥肠",2);

        Order order2 = new Order();
        order2.setDiningTable(2);
        order2.setFoodDic("白菜炖豆腐",1);
        order2.setFoodDic("烤鸭",3);

        OrderCommand command1 = new OrderCommand(seniorChef,order1);
        OrderCommand command2 = new OrderCommand(seniorChef,order2);

        Waitor waitor = new Waitor();
        waitor.setCommand(command1);
        waitor.setCommand(command2);

        waitor.orderUp();

    }
}

```

2.2 优缺点说明

优点：

*   降低系统耦合度。命令模式能将调用操作的对象与实现该操作的对象解耦。
*   容易实现对请求的撤销与重做。
*   增加或删除命令非常方便。采用命令模式增加和删除不会影响其他类。

缺点：

*   使用命令模式可能会导致某些系统有过多的具体命令类。
*   系统结构更加复杂。

#### 4、责任链模式

又名职责链模式，为了避免请求发送者与多个请求处理者耦合在一起，将所有请求的处理者通过前一对象记住其下一个对象的引用而连成一条链，当有请求发生时，可将请求沿着这条链传递，直到有对象处理它为止。

4.1 结构

*   抽象处理者角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
*   具体处理者角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请求转给它的后继者。
*   客户类角色：创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。

现需要开发一个请假流程控制系统。请假一天以下的假只需要小组长同意即可；请假 1 天到 3 天的假还需要部门经理同意；请求 3 天到 7 天还需要总经理同意才行。  
类图如下：  
![](https://img-blog.csdnimg.cn/20210719160743792.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//抽象处理者角色
public abstract class Handler {
    protected final static int NUM_ONE = 1;
    protected final static int NUM_THREE = 3;
    protected final static int NUM_SEVEN = 7;
    private int numStart;
    private int numEnd;
    private Handler nextHandler;

    public Handler(int numStart, int numEnd) {
        this.numStart = numStart;
        this.numEnd = numEnd;
    }

    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }
    public void submit(LeaveRequest leave){
        if(leave.getNum() >= this.numStart){
            this.handleLeave(leave);
        }
        if(this.nextHandler != null && leave.getNum() > this.numEnd){
            this.nextHandler.handleLeave(leave);
        }else{
            System.out.println("流程结束");
        }
    }
    public abstract void handleLeave(LeaveRequest leave);
}

//小组长
public class GroupLEader extends Handler{
    public GroupLEader() {
        super(0,Handler.NUM_ONE);
    }

    @Override
    public void handleLeave(LeaveRequest leave) {
        System.out.println(leave.getName() + "因" + leave.getContent()+ "请假"+ leave.getNum() + "天");
        System.out.println("小组长审批通过");
    }
}

//部门经理
public class Manager extends Handler{
    public Manager() {
        super(Handler.NUM_ONE,Handler.NUM_THREE);
    }

    @Override
    public void handleLeave(LeaveRequest leave) {
        System.out.println(leave.getName() + "因" + leave.getContent()+ "请假"+ leave.getNum() + "天");
        System.out.println("部门经理审批通过");
    }
}

//总经理
public class GeneralManager extends Handler{
    public GeneralManager() {
        super(Handler.NUM_THREE,Handler.NUM_SEVEN);
    }

    @Override
    public void handleLeave(LeaveRequest leave) {
        System.out.println(leave.getName() + "因" + leave.getContent()+ "请假"+ leave.getNum() + "天");
        System.out.println("总经理审批通过");
    }
}

//请假条
public class LeaveRequest {
    private String name;
    private int num;
    private String content;

    public LeaveRequest(String name, int num, String content) {
        this.name = name;
        this.num = num;
        this.content = content;
    }

    public String getName() {
        return name;
    }

    public int getNum() {
        return num;
    }

    public String getContent() {
        return content;
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        LeaveRequest request = new LeaveRequest("小王",5,"胃疼去医院");

        GroupLEader groupLEader = new GroupLEader();
        Manager manager = new Manager();
        GeneralManager generalManager = new GeneralManager();

        groupLEader.setNextHandler(manager);
        manager.setNextHandler(generalManager);

        groupLEader.submit(request);
    }
}

```

4.2 优缺点说明

优点：

*   降低了对象之间耦合度
*   增强了系统的可扩展性。可以根据需要增加新的请求处理类，满足开闭原则。
*   增强了给对象指派职责的灵活性。当工作流程发生变化，可以动态地改变链内的成员或者修改它们的次序，也可以动态地新增或删除责任。

缺点：

*   不能保证每个请求一定被处理。由于一个请求没有明确的接收者，所以不能保证它一定会被处理，该请求可能一直传到链的末端都得不到处理。
*   对比较长的职责链，请求的处理可能涉及多个处理对象，系统性能将受到一定影响。

#### 5、状态模式

对有状态的对象，把复杂的判断逻辑提取到不同的状态对象中，允许状态对象在其内部状态发生改变时改变其行为。

5.1 结构

*   环境角色：也称为上下文，它定义了客户程序需要的接口，维护一个当前状态，并将与状态相关的操作委托给当前状态对象来处理。
*   抽象状态角色：定义一个接口，用以封装环境对象中的特定状态所对应的行为。
*   具体状态角色：实现抽象状态所对应的行为。

通过按钮来控制一个电梯的状态，一个电梯有开门状态，关门状态，停止状态，运行状态。每一种状态改变，都有可能要根据其他状态来更新处理。例如，如果电梯门现在处于运行时状态，就不能进行开门操作，而如果电梯门是停止状态，就可以执行开门操作。  
类图如下：

![](https://img-blog.csdnimg.cn/img_convert/51f868862e260417ec034a502e046d5d.png)

```
//抽象状态类
public abstract class LiftState {
    protected Context context;

    public void setContext(Context context) {
        this.context = context;
    }
    public abstract void open();
    public abstract void close();
    public abstract void stop();
    public abstract void run();
}

//开启状态类
public class OpeningState extends LiftState {
    @Override
    public void open() {
        System.out.println("电梯门开启");
    }

    @Override
    public void close() {
        super.context.setLiftState(Context.CLOSING_STATE);
        super.context.close();
    }

    @Override
    public void stop() {

    }

    @Override
    public void run() {

    }
}

//关闭状态类
public class ClosingState extends LiftState {
    @Override
    public void open() {
        super.context.setLiftState(Context.OPENING_STATE);
        super.context.open();
    }

    @Override
    public void close() {
        System.out.println("电梯门关闭");
    }

    @Override
    public void stop() {
        super.context.setLiftState(Context.STOPPING_STATE);
        super.context.stop();
    }

    @Override
    public void run() {
        super.context.setLiftState(Context.RUNNING_STATE);
        super.context.run();
    }
}

//停止状态类
public class StoppingState extends LiftState {
    @Override
    public void open() {
        super.context.setLiftState(Context.OPENING_STATE);
        super.context.open();
    }

    @Override
    public void close() {
        super.context.setLiftState(Context.CLOSING_STATE);
        super.context.close();
    }

    @Override
    public void stop() {
        System.out.println("电梯停止了");
    }

    @Override
    public void run() {
        super.context.setLiftState(Context.RUNNING_STATE);
        super.context.run();
    }
}

//运行状态类
public class RunningState extends LiftState {
    @Override
    public void open() {

    }

    @Override
    public void close() {

    }

    @Override
    public void stop() {
        super.context.setLiftState(Context.STOPPING_STATE);
        super.context.stop();
    }

    @Override
    public void run() {
        System.out.println("电梯开始运行");
    }
}

//环境类
public class Context {
    public static final OpeningState OPENING_STATE = new OpeningState();
    public static final ClosingState CLOSING_STATE = new ClosingState();
    public static final StoppingState STOPPING_STATE = new StoppingState();
    public static final RunningState RUNNING_STATE = new RunningState();
    private LiftState liftState;

    public LiftState getLiftState() {
        return liftState;
    }

    public void setLiftState(LiftState liftState) {
        this.liftState = liftState;
        this.liftState.setContext(this);
    }
    public void open(){
        liftState.open();
    }
    public void close(){
        liftState.close();
    }
    public void stop(){
        liftState.stop();
    }
    public void run(){
        liftState.run();
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        Context context = new Context();
        context.setLiftState(new ClosingState());

        context.open();
        context.run();
        context.close();
        context.stop();
    }
}

```

5.2 优缺点说明

优点：

*   将所有与某个状态有关的行为放到一个类中，并且方便地增加新的状态，只需改变对象状态即可改变对象的行为。
*   允许状态转换逻辑与状态对象合为一体，而不是某一个巨大的条件语句块。

缺点：

*   状态模式的使用必然会增加系统类和对象的个数。
*   状态模式的结构和实现都较为复杂，如果使用不当将导致程序结构的混乱。
*   对开闭模式的支持不太好。

#### 6、观察者模式

又被称为发布 - 订阅模式，它定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态变化时，会通知所有的观察者对象，使它们能够自动更新自己。

6.1 结构

*   抽象主题：抽象主题角色把所有观察者对象保存在一个集合中，每个主题都可以有任意数量的观察者，抽象主题提供一个接口，可以增加和删除观察者对象。
*   具体主题：该角色将有关状态存入具体观察者对象，在具体主题的内部状态发生改变时，给所有注册过的观察者发送通知。
*   抽象观察者：是观察者的抽象类，它定义了一个更新接口，使在得到主题更改通知时更新自己。
*   具体观察者：实现抽象观察者定义的更新接口，以便在得到主题更改通知时更新自身状态。

在使用微信公众号时，大家都会有这样的体验，当你关注的公众号中有新内容更新的话，它就会推送给关注公众号的微信用户端。我们使用观察者模式来模拟这样的场景，微信用户就是观察者，微信公众号是被观察者，有多个的微信用户关注了程序猿这个公众号。

类图如下：  
![](https://img-blog.csdnimg.cn/img_convert/871d964b30b1c7b9b6dd3110afd5243b.png#pic_center)

```
//抽象主题类
public interface Subject {
    public void attach(Observer observer);
    public void detach(Observer observer);
    public void notify(String message);
}

//具体主题类
public class SubscriptionSubject implements Subject {
    private List<Observer> weixinUserList = new ArrayList<>();

    @Override
    public void attach(Observer observer) {
        weixinUserList.add(observer);
    }

    @Override
    public void detach(Observer observer) {
        weixinUserList.remove(observer);
    }

    @Override
    public void notify(String message) {
        for (Observer observer : weixinUserList) {
            observer.update(message);
        }
    }
}

//抽象观察者类
public interface Observer {
    public void update(String message);
}

//具体观察者类
public class WeixinUser implements Observer {
    private String name;
    public WeixinUser(String name) {
        this.name = name;
    }
    @Override
    public void update(String message) {
        System.out.println(name + "---" + message);
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        Subject subject = new SubscriptionSubject();

        WeixinUser user1 = new WeixinUser("张三");
        WeixinUser user2 = new WeixinUser("李四");
        WeixinUser user3 = new WeixinUser("王五");

        subject.attach(user1);
        subject.attach(user2);
        subject.attach(user3);

        subject.notify("公众号更新了");
    }
}

```

6.2 优缺点说明

优点：

*   降低了目标与观察者之间的耦合关系，两者之间是抽象耦合关系。
*   被观察者发送通知，所有注册的观察者都会收到信息。

缺点：

*   如果观察者非常多的话，那么所有的观察者收到被观察者发送的通知会耗时。
*   如果被观察者有循环依赖的话，那么被观察者发送通知会使观察者循环调用，会导致系统崩溃。

#### 7、中介者模式

又叫调停模式，定义一个中介角色来封装一系列对象之间的交互，使原有对象之间的耦合松散，且可以独立地改变他们之间的交互。

7.1 结构

*   抽象中介者角色：它是中介者的接口，提供了同事对象注册与转发同事对象信息的抽象方法。
*   具体中介者角色：实现中介者接口，定义一个 List 来管理同事对象，协调各个同事角色之间的交互关系，因此，它依赖于同事角色。
*   抽象同事类角色：定义同事类的接口，保存中介者对象，提供同事对象交互的抽象方法，实现所有相互影响的同事类的公共功能。
*   具体同事类角色：是抽象同事类的实现者，当需要与其他同事对象交互时，由中介者对象负责后续的交互。

现在租房基本都是通过房屋中介，房主将房屋托管给房屋中介，而租房者从房屋中介获取房屋信息。房屋中介充当租房者与房屋所有者之间的中介者。  
![](https://img-blog.csdnimg.cn/img_convert/bb93d39be5f1f2e1b5942bd5413dc319.png#pic_center)

```
//抽象中介者
public interface Mediator {
    public void contact(String message,Person person);
}

//抽象同事类
public abstract class Person {
    protected String name;
    protected Mediator mediator;

    public Person(String name, Mediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }
}

//租客类
public class Tenant extends Person {

    public Tenant(String name, Mediator mediator) {
        super(name, mediator);
    }
    public void contact(String message){
        mediator.contact(message,this);
    }
    public void getMessage(String message){
        System.out.println("租客" + name + "获取到的信息：" + message);

    }
}

//房主类
public class HouseOwner extends Person {

    public HouseOwner(String name, Mediator mediator) {
        super(name, mediator);
    }
    public void contact(String message){
        mediator.contact(message,this);
    }
    public void getMessage(String message){
        System.out.println("房主" + name + "获取到的信息：" + message);
    }
}

//具体中介类
public class MediatorStructure implements Mediator {
    public HouseOwner houseOwner;
    public Tenant tenant;

    public HouseOwner getHouseOwner() {
        return houseOwner;
    }

    public void setHouseOwner(HouseOwner houseOwner) {
        this.houseOwner = houseOwner;
    }

    public Tenant getTenant() {
        return tenant;
    }

    public void setTenant(Tenant tenant) {
        this.tenant = tenant;
    }

    @Override
    public void contact(String message, Person person) {
        if(person == tenant){
            houseOwner.getMessage(message);
        }
        if(person == houseOwner){
            tenant.getMessage(message);
        }
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        MediatorStructure mediator = new MediatorStructure();

        HouseOwner houseOwner = new HouseOwner("张三",mediator);
        Tenant tenant = new Tenant("李四",mediator);

        mediator.setHouseOwner(houseOwner);
        mediator.setTenant(tenant);

        tenant.contact("我想租一个两居室");
    }
}

```

7.2 优缺点说明

优点：

*   松散耦合：中介者模式通过把多个同事对象之间的交互封装到中介者对象里面，从而使得同事对象之间松散耦合，基本上可以做到互补依赖。这样一来，同事对象就可以独立地变化和复用，而不再像以前那样  
    “牵一处而动全身” 了。
*   集中控制交互：多个同事对象的交互，被封装在中介者对象里面集中管理，使得这些交互行为发生变化的时候，只需要修改中介者对象就可以了，当然如果是已经做好的系统，那么就扩展中介者对象，而各个同事  
    类不需要做修改。
*   一对多关联转变为一对一的关联。没有使用中介者模式的时候，同事对象之间的关系通常是一对多的，引入中介者对象以后，中介者对象和同事对象的关系通常变成双向的一对一，这会让对象的关系更容易理解和实现。

缺点：当同事类太多时，中介者的职责将很大，它会变得复杂而庞大，以至于系统难以维护。

#### 8、迭代器模式

提供一个对象来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。

8.1 结构

*   抽象聚合角色：定义存储、添加、删除聚合元素以及创建迭代器对象的接口。
*   具体聚合角色：实现抽象聚合类，返回一个具体迭代器的实例。
*   抽象迭代器角色：定义访问和遍历聚合元素的接口，通常包含 hasNext()、next() 等方法。
*   具体迭代器角色：实现抽象迭代器接口中所定义的方法，实现对聚合对象的遍历，记录遍历的当前位置。

定义一个可以存储学生对象的容器对象，将遍历该容器的功能交由迭代器实现。  
类图如下：  
![](https://img-blog.csdnimg.cn/img_convert/d9bc95480947c93d5b91a840aff9ff00.png#pic_center)

```
//student类
public class Student {
    private String name;
    private String number;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    @Override
    public String toString() {
        return "Student{" +
                " + name + '\'' +
                ", number='" + number + '\'' +
                '}';
    }
}

//迭代器接口
public interface StudentIterator {
    public boolean hasNext();
    public Student next();
}

//具体的迭代器类
public class StudentIteratorImpl implements StudentIterator {
    public List<Student> list;
    private int position = 0;

    public StudentIteratorImpl(List<Student> list) {
        this.list = list;
    }

    @Override
    public boolean hasNext() {
        return position < list.size();
    }

    @Override
    public Student next() {
        Student student = list.get(position);
        ++position;
        return student;
    }
}

//抽象容器类
public interface StudentAggregate {
    public void addStudent(Student student);
    public void removeStudent(Student student);
    public StudentIterator getStudentIterator();
}

//具体容器类
public class StudentAggregateImpl implements StudentAggregate {
    private List<Student> list = new ArrayList<>();

    @Override
    public void addStudent(Student student) {
        list.add(student);
    }

    @Override
    public void removeStudent(Student student) {
        list.remove(student);
    }

    @Override
    public StudentIterator getStudentIterator() {
        return new StudentIteratorImpl(list);
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        Student s1 = new Student();
        s1.setName("张三");
        s1.setNumber("1");

        Student s2 = new Student();
        s2.setName("李四");
        s2.setNumber("2");

        StudentAggregate list = new StudentAggregateImpl();
        list.addStudent(s1);
        list.addStudent(s2);

        StudentIterator iterator = list.getStudentIterator();
        while(iterator.hasNext()){
           Student student = iterator.next();
           System.out.println(student);
       }

    }
}

```

8.2 优缺点说明

优点：

*   它支持以不同的方式遍历一个聚合对象，在同一个聚合对象上可以定义多种遍历方式。在迭代器模式中只需要用一个不同的迭代器来替换原有迭代器即可改变遍历算法，我们也可以自己定义迭代器的子类以支持新的遍历方式。
*   迭代器简化了聚合类。由于引入了迭代器，在原有的聚合对象中不需要再自行提供数据遍历等方法，这样可以简化聚合类的设计。
*   在迭代器模式中，由于引入了抽象层，增加新的聚合类和迭代器类都很方便，无须修改原有代码，满足 “开闭原则” 的要求。

缺点：增加了类的个数，这在一定程度上增加了系统复杂度。

#### 9、访问者模式

封装一些作用于某种数据结构中的各元素的操作，它可以在不改变这个数据结构的前提下定义作用于这些元素的新操作。

9.1 结构

*   抽象访问角色：定义了对每一个元素访问的行为，它的参数就是可以访问的元素，它的方法个数理论上来讲与元素类个数（Element 的实现类个数）是一样的，从这点不难看出，访问者模式要求元素类的个数不能改变。
*   具体访问者角色：给出对每一个元素类访问时所产生的具体行为。
*   抽象元素角色：定义了一个接受访问者的方法，其意义是指，每一个元素都要可以被访问者访问。
*   具体元素角色： 提供接受访问方法的具体实现，而这个具体的实现，通常情况下是使用访问者提供的访问该元素类的方法。
*   对象结构角色：定义当中所提到的对象结构，对象结构是一个抽象表述，具体点可以理解为一个具有容器性质或者复合对象特性的类，它会含有一组元素，并且可以迭代这些元素，供访问者访问。

现在养宠物的人特别多，我们就以这个为例，当然宠物还分为狗，猫等，要给宠物喂食的话，主人可以喂，其他人也可以喂食。

*   访问者角色：给宠物喂食的人
*   具体访问者角色：主人、其他人
*   抽象元素角色：动物抽象类
*   具体元素角色：宠物狗、宠物猫
*   结构对象角色：主人家

类图如下：  
![](https://img-blog.csdnimg.cn/img_convert/7e6b6602619f59acf0fa74306346b752.png)

```
//抽象访问者接口
public interface Person {
    void feed(Cat cat);
    void feed(Dog dog);
}

//具体访问者角色（主人）
public class Owner implements Person {
    @Override
    public void feed(Cat cat) {
        System.out.println("主人喂猫");
    }

    @Override
    public void feed(Dog dog) {
        System.out.println("主人喂狗");
    }
}

//具体访问者角色（其他人）
public class Someone implements Person {
    @Override
    public void feed(Cat cat) {
        System.out.println("其他人喂猫");
    }

    @Override
    public void feed(Dog dog) {
        System.out.println("其他人喂狗");
    }
}

//抽象元素角色
public interface Animal {
    void accept(Person person);
}

//具体元素角色（狗）
public class Dog implements Animal{
    @Override
    public void accept(Person person) {
        person.feed(this);
        System.out.println("真好吃，汪汪汪");
    }
}

//具体元素角色（猫）
public class Cat implements Animal{
    @Override
    public void accept(Person person) {
        person.feed(this);
        System.out.println("真好吃，喵喵喵");
    }
}

//对象结构
public class Home {
    private List<Animal> nodeList = new ArrayList<>();

    public void action(Person person){
        for (Animal animal : nodeList) {
            animal.accept(person);
        }
    }
    public void add(Animal animal){
        nodeList.add(animal);
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        Home home = new Home();

        Owner owner = new Owner();
        Someone someone = new Someone();

        Dog dog = new Dog();
        Cat cat = new Cat();

        home.add(dog);
        home.add(cat);

        home.action(owner);
    }
}

```

9.2 优缺点说明

优点：

*   扩展性好，在不修改对象结构中的元素的情况下，为对象结构中的元素添加新的功能。
*   复用性好。通过访问者来定义整个对象结构通用的功能，从而提高复用程度。
*   分离无关行为。通过访问者来分离无关的行为，把相关的行为封装在一起，构成一个访问者，这样每一个访问者的功能都比较单一。

缺点：

*   对象结构变化很困难。在访问者模式中，每增加一个新的元素类，都要在每一个具体访问者类中增加相应的具体操作，这违背了 “开闭原则”。
*   违反了依赖倒置原则。访问者模式依赖了具体类，而没有依赖抽象类。

#### 10、备忘录模式

又叫快照模式，在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后当需要时能将该对象恢复到原先保存的状态。

10.1 结构

*   发起人角色：记录当前时刻的内部状态信息，提供创建备忘录和恢复备忘录数据的功能，实现其他业务功能，它可以访问备忘录里的所有信息。
*   备忘录角色：负责存储发起人的内部状态，在需要时提供这些内部状态给发起人。
*   管理者角色：对备忘录进行管理，提供保存与获取备忘录的功能，但其不能对备忘录的内容进行访问与修改。

游戏中的某个场景，一游戏角色有生命力、攻击力、防御力等数据，在打 Boss 前和后一定会不一样的，我们允许玩家如果感觉与 Boss 决斗的效果不理想可以让游戏恢复到决斗之前的状态。

类图如下：  
![](https://img-blog.csdnimg.cn/16679a5d2f8642c8827ad72d33d0069c.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU3NzM0Ng==,size_16,color_FFFFFF,t_70#pic_center)

```
//游戏角色类
public class GameRole {
    private int vit;
    private int atk;
    private int def;
    public void initState(){
        this.vit = 100;
        this.atk = 100;
        this.def = 100;
    }
    public void fight(){
        this.vit = 0;
        this.atk = 0;
        this.def = 0;
    }
    public RoleStateMemento saveState(){
        return new RoleStateMemento(vit,atk,def);
    }
    public void recoverState(RoleStateMemento roleStateMemento){
        this.vit = roleStateMemento.getVit();
        this.atk = roleStateMemento.getAtk();
        this.def = roleStateMemento.getDef();
    }
    public void stateDisplay(){
        System.out.println("角色生命力：" + vit);
        System.out.println("角色攻击力：" + atk);
        System.out.println("角色防御力：" + def);
    }
}

//备忘录类
public class RoleStateMemento {
    private int vit;
    private int atk;
    private int def;

    public RoleStateMemento(int vit, int atk, int def) {
        this.vit = vit;
        this.atk = atk;
        this.def = def;
    }

    public int getVit() {
        return vit;
    }

    public int getAtk() {
        return atk;
    }

    public int getDef() {
        return def;
    }
}

//管理者类
public class RoleStateCaretaker {
    private RoleStateMemento roleStateMemento;

    public RoleStateMemento getRoleStateMemento() {
        return roleStateMemento;
    }

    public void setRoleStateMemento(RoleStateMemento roleStateMemento) {
        this.roleStateMemento = roleStateMemento;
    }
}

//用户类
public class Client {
    public static void main(String[] args) {
        GameRole gameRole = new GameRole();
        gameRole.initState();
        gameRole.stateDisplay();

        RoleStateCaretaker roleStateCaretaker = new RoleStateCaretaker();
        roleStateCaretaker.setRoleStateMemento(gameRole.saveState());

        gameRole.fight();
        gameRole.stateDisplay();

        gameRole.recoverState(roleStateCaretaker.getRoleStateMemento());
        gameRole.stateDisplay();
    }
}

```

10.2 优缺点说明

优点：

*   提供了一种可以恢复状态的机制。当用户需要时能够比较方便地将数据恢复到某个历史的状态。
*   简化了发起人类。发起人不需要管理和保存其内部状态的各个备份，所有状态信息都保存在备忘录中，并由管理者进行管理，这符合单一职责原则。

缺点：

*   资源消耗大。如果要保存的内部状态信息过多或者特别频繁，将会占用比较大的内存资源。