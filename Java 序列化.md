# Java 序列化

## 什么是对象序列化？
在序列化 的过程中，对象和它的元数据（比如对象的类名和它的属性名称）存储为一种特殊的二进制格式。将对象存储为这种格式（序列化 它）会保留所有必要的信息，使您在需要时能够重建（或去序列化）对象。

对象序列化的两个主要使用场景包括：

- 对象持久化：将对象的状态存储在一种永久的持久性机制中，比如数据库
- 对象远程存储：将对象发送到另一台计算机或另一个系统

## java.io.Serializable
实现序列化的第一步是使对象能够使用该机制。您希望能够序列化的每个对象必须实现一个名为 java.io.Serializable 的接口：

    import java.io.Serializable;
    public class Person implements Serializable {
      // etc...
    }
    
在此示例中，Serializable 接口将 Person 类（和 Person 的每个子类的对象）向运行时标记为 serializable。

如果 Java 运行时尝试序列化您的对象，无法序列化的对象的每个属性会导致它抛出一个 NotSerializableException。可以使用 transient 关键字管理此行为，告诉运行时不要尝试序列化一些属性。在这种情况下，您应该负责确保恢复这些属性（在必要时），以便您的对象能正常运行。

## serialVersionUID
在中间件和远程对象通信的发展初期，开发人员主要负责控制其对象的 “连接格式”，随着技术的演变，这会引起了大量头疼的问题。

假设您向一个对象添加了一个属性，重新编译了它，然后将该代码重新分发到一个应用集群中的每台计算机。
该对象的一个序列化代码版本存储在一台计算机中，但由其他可能具有不同代码版本的计算机访问。在这些计算机尝试去序列化该对象时，通常会出现一些糟糕的事情。

Java 序列化元数据（二进制序列化格式中包含的信息）很复杂，解决了困扰早期中间件开发人员的许多问题。但它并不能解决所有问题。

Java 序列化使用一个称为 serialVersionUID 的特性来帮助处理一个序列化场景中的不同对象版本。您不需要在对象上声明此特性；默认情况下，Java 平台会使用一种算法，该算法基于类的属性、类名和它在庞大的本地集群中的位置来计算值。在大多数情况下，该算法都能正常运行。.但是，如果您添加或删除一个属性，这个动态生成的值就会发生更改，而且 Java 运行时会抛出一个 InvalidClassException。

要避免此结果，可以养成显式声明 serialVersionUID 的习惯：

    import java.io.Serializable;
      public class Person implements Serializable {
      private static final long serialVersionUID = 20180312;
      // etc...
      }
      
我推荐对 serialVersionUID 版本号使用某种模式（我在前面的示例中使用了当前日期）。而且您应该将 serialVersionUID 声明为 private static final 和 long 类型。

您可能想知道应在何时更改此特性。简单的答案是，只要对代码执行了不兼容的更改（这通常意味着您添加或删除了某个属性），就应该更改它。如果您在一台计算机上的对象版本中添加或删除了该属性，而且该对象远程传输到了一台包含缺少或需要该属性的对象版本的计算机，就会发生一些怪异的事情。这时就可以使用 Java 平台的内置 serialVersionUID 进行检查。

作为一条经验规则，无论任何时候添加或删除一个类特征（即属性或其他任何实例级状态变量），都需要更改它的 serialVersionUID。在连接的另一端获得一个 java.io.InvalidClassException，比由不兼容的类更改导致应用程序错误要好。

