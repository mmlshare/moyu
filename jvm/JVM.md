# JVM

## 1. JVM基础

### 1.1.  JVM 是什么？

- jvm是一种用于计算机设备的一种规范。
- 是虚构出来的一台计算机，它有自己的字节码指令集和内存管理机制（栈，堆，方法区等）。
- 它屏蔽了与具体平台的相关信息，使得只要将代码编译成符合jvm规范的字节码，就能在不同的平台上运行。

### 1.2. 常见的JVM实现

- Hotspot-oracle
- Jrockit -BEA,曾经号称世界上最快的JVM，被Oracle收购，合并于hotspot
- J9- IBM
- Microsoft VM
- TaoBaoVM-hotspot深度定制版
- LiquidVM-针对硬件的
- azul zing -最新的垃圾回收的业界标杆。https://www.azul.com/products/zing/

### 1.3. JDK组成

 JDK = JRE + development kit

​	-- JRE = JVM+ core lib

​		-- JVM



## 2.  class 文件

### 2.1. java代码怎么执行？

- `*.java`文件先通过` javac`编译成` *.class`文件。

- `*.class`文件通过java命令将其交由 ***JVM*** 执行：

  通过`ClassLoader`加载该文件，通过字节码解释器和JIT即时编译器翻译成本机机器代码。

### 2.2. Class文件格式

```java
ClassFile {
 u4 magic; //Class文件标识，固定为 0xCAFEBABE
 u2 minor_version; // Class 副版本
 u2 major_version; // Class 主版本
 u2 constant_pool_count; //常量池长度
 cp_info constant_pool[constant_pool_count-1]; //常量池内容
 u2 access_flags; // Class 访问标识
 u2 this_class;   // 当前类
 u2 super_class;  //父类
 u2 interfaces_count; //继承的接口数量
 u2 interfaces[interfaces_count]; //接口内容
 u2 fields_count; //字段数量
 field_info fields[fields_count]; //字段内容
 u2 methods_count; //方法数量
 method_info methods[methods_count]; //方法信息
 u2 attributes_count; //属性数量
 attribute_info attributes[attributes_count]; //属性信息
}

```

可通过 IDE 插件 `jclasslib Bytecode viewer` 查看class文件的具体结构和格式。



## 3. class加载到 JVM 的过程

Loading => Linking => Initializing

### 3.1. Loading(加载)

加载：把`class`文件加载到内存。

#### 3.1.1. 类加载器

采用双亲委派机制，加载过程中从下到上查找，查找不到就从上到下寻找加载器把`class`加载到内存。

- Custom ClassLoader

  用户自定义加载器，可加载指定路径下的`class`文件。

- App ClassLoader

  应用加载器，加载程序所在目录的`class`文件。

- Extension  ClassLoader

  标准扩展库加载器，加载扩展库，如`classpath`中的`jre` ，`javax.*`或者
  `java.ext.dir` 指定位置中的类，开发者可以直接使用标准扩展类加载器。

- Bootstrap  ClassLoader

  `c++`编写，加载`java`核心库 `java.*`,构造`ExtClassLoader`和`AppClassLoader`。

#### 3.1.2. 实现自己的类加载器,如何破坏双亲委派机制？

```java
public class MyClassLoader extends ClassLoader{

    /**
     * 实现自己的ClassLoader
     * 重写findClass方法，加载自己制定目录的类
     */
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        return super.findClass(name);
    }

    /**
     * 破坏双亲委派机制
     * 重写loadClass方法，重写加载类的逻辑(将双亲委派机制的逻辑修改掉)
     */
    @Override
    public Class<?> loadClass(String name) throws ClassNotFoundException {
        return super.loadClass(name);
    }
}
```



### 3.2. Linking(链接)

链接：包括Verification、Preparation、Resolution三个过程。

- Verification: 验证文件是否符合JVM规定。

- Preparation：静态成员变量赋默认值。

- Resolution：将类、方法、属性等符号引用解析为直接引用，常量池中的各种符号引用解析为指针、偏移量等内存地址的直接引用

  符号引用:即一个字符串，但是这个字符串给出了一些能够唯一性识别一个方法，一个变量，一个类的相关信息。

  直接引用:可以理解为一个内存地址，或者一个偏移量。比如类方法，类变量的直接引用是指向方法区的指针；而实例方法，实例变量的直接引用则是从实例的头指针开始算起到这个实例变量位置的偏移量。

### 3.3. Initializing(初始化)

初始化:调用类初始化代码`init`，给静态成员变量赋初始值。







## 2. 类加载-初始化 Class Loading Linking Initializing

1. 加载过程

   1. Loading

      把class文件加载到内存，双亲委派，主要为安全考虑

   2. Linking
   
      1. Verification
   
         验证检查文件
   
      2. Preparation
   
      3. Resolution
   
   3. Initializing

![image-20200613194406440](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200613194406440.png)

双亲委派机制

![image-20200614091455017](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614091455017.png)

![image-20200614092228452](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614092228452.png)

![image-20200614092655512](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614092655512.png)

![image-20200614093103682](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614093103682.png)

![image-20200614100509526](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614100509526.png)

![image-20200614101315428](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614101315428.png)





![image-20200614143500254](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614143500254.png)

![image-20200614150439598](C:\Users\LPSHARE\AppData\Roaming\Typora\typora-user-images\image-20200614150439598.png)