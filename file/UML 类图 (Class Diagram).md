# UML 类图 (Class Diagram)

1. 类（Class)表示：

   * 属性(Attributes)：可见性 名称 ：类型 [ = 默认值]
   * 方法(method)：可见性 名称 (参数列表) [ : 返回类型]

2. 类关系表示：

   * 关联(Association)关系

     * 双向关系：类关联双向   ————

     * 单向关系：类的关联关系也可以是单向的，在UML中单向关联用带箭头的实线表示 ————>

     * 自关联：在系统中可能会存在一些类的属性对象类型为该类本身，这种特殊的关联关系称为自关联

     * 多重性关联（关联对象在数量上的对应关系）：

       | 表示方式 | 多重性说明                                                 |
       | -------- | ---------------------------------------------------------- |
       | 1. . 1   | 表示另一个类的一个对象只与该类的一个对象有关系             |
       | 0. . *   | 表示另一个类的一个对象与该类的零个或多个对象有关系         |
       | 1. . *   | 表示另一个类的一个对象与该类的一个或多个对象有关系         |
       | 0. . 1   | 表示另一个类的一个对象没有或只与该类的一个对象有关系       |
       | m. . n   | 表示另一个类的一个对象与该类最少m，最多n个对象有关系（m≤n) |

     * 聚合关系：聚合（Aggregation）关系表示整体与部分的关系。在聚合关系中，成员对象是整体对象的一部分，但是**成员对象可以脱离整体对象独立存在**。

     * 组合关系： 表示类之间整体和部分的关系，但是在组合关系中整体对象可以控制成员对象的生命周期，**一旦整体对象不存在，成员对象也将不存在**。

   * 依赖关系： 关系是一种使用关系，特定事物的改变有可能会影响到使用该事物的其他事物，在需要表示一个事物使用另一个事物时使用依赖关系。  A ----> B  A调用了 B的方法

   * 泛化关系： 继续关系， Java extends

   * 接口与实现关系 Java implements

​	面向对象的设计原则：

![](https://tryu-image.oss-cn-hangzhou.aliyuncs.com/img/desgin-pattern01.png)

创建型模式：1. What, Who, When

![](https://tryu-image.oss-cn-hangzhou.aliyuncs.com/img/design-pattern02.png)

创建过程线程安全的懒汉模式的单例：

1. 构造函数私有， 

2. 需要保证实例化过程是无重排指令的，用 volatile 

3. 判断了二次为null, 为了减少竞争

```java
class LazySingleton {
       private static volatile LazySingleton instance = null;
       
       private LazySingletion(){
       }
       
       private static LazySingletion getInstance() {
           if(this.instance == null) {
               synchronized (LazySingletion) {
                   if(this.instance == null) {
                       return new LazySingletion();
                   }
               }
           }
       }
   }
```

创建过程线程安全的饿汉模式的单例：

1. 构造函数私有
2. static final 保证了在加载类的时候从中就已经创建了单例对象，并且保证了在创建过程的不可见性

```java
class EagerSingleton {
    private static final EagerSingleton instance = new EagerSingleton();
    
    private EagerSingleton(){}
    
    private static EagerSingleton getInstance() {
        return instance;
    }
}
```

集中两种优点的单例（静态内部类）

利用静态内部类的静态资源只在调用时才加载的特点

```java
class Singleton {
    private Singletion() {}
    
    private static class HolderClass {
        private static final Singleton instance = new Singletion;
    }
    
    public static Singletion getInstance() {
        return HolderClass.insatnce;
    }
}
```

**以上单例写法都克隆、反射和反序列化对单例模式的破坏**[^1]

枚举类型的懒加载的单例

```java
class Singleton {
    private Singleton(){}
    
    public enum SingletionEnum {
        SINGLETON;
        
        private Singletion insatnce = null;
        
        private SingletonEnum() {
            instance = new Singleton();
        }
        
        public Sigletion getInstance() {
            return insatnce;
        }
    }
}
```

原因：

1. JVM保证了枚举类型不能被反射和反序列化
2. JVM保证了枚举类型的构造函数只能被执行一次

**项目使用思考：** 

1. 在JVM上运行的单例需要只能有一个实例，注意区别Spring的单例模式只是在Spring Context 范围内只有一个实例。
2. 静态方法本身就替代掉单例，要是只是平时的使用完全可以只使用普通类的静态方法和静态类 [^2]

简单工厂模式 ---> 工厂方法模式 ---> 抽象工厂模式

简单工厂模式：就是将创建对象的职责统一放到了Factory内，若是生成的对象少的情况下。可以使用。

工厂方法模式：一个对象，对应一个对象工厂；

抽象工厂模式： 还是没有理解。。。。。





[^1]: https://blog.csdn.net/LoveLion/article/details/110983839



[^2]: https://blog.51cto.com/zhangxueliang/3002864