# Java Introspector·内省

> 引用自https://www.jianshu.com/p/205444f4b1eb

### 什么是Introspector

 Introspector 是操作 javaBean 的 API，用来访问某个属性的 getter/setter 方法。

对于一个标准的 javaBean 来说，它包括属性、get 方法和 set 方法，这是一个约定俗成的规范。为此 sun 提供了 Introspector 工具包，来使开发者更好或者更灵活的操作 javaBean。


### 都有哪些 API 可以用

![img](E:\PycharmProjects\SomeTips\pictures\4047674-2c6fc8660249f92d.webp)

核心类是 `Introspector`, 它提供了的 `getBeanInfo` 系类方法，可以拿到一个 `JavaBean` 的所有信息。

通过 `BeanInfo` 的 `getPropertyDescriptors` 方法和` getMethodDescriptors `方法可以拿到 `javaBean` 的字段信息列表和 `getter` 和 `setter` 方法信息列表。

`PropertyDescriptors `可以根据字段直接获得该字段的` getter` 和 `setter` 方法。

`MethodDescriptors `可以获得方法的元信息，比如方法名，参数个数，参数字段类型等。



### 怎么用

创建一个标准的 javabean

```java
public class User {
    private String name;
    private int age;
    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

测试1

```java
    @Test
    public void test1() throws Exception {
        // 获取整个Bean的信息
        // BeanInfo beanInfo= Introspector.getBeanInfo(user.getClass());
        // 在Object类时候停止检索，可以选择在任意一个父类停止
        BeanInfo beanInfo = Introspector.getBeanInfo(User.class, Object.class);

        System.out.println("所有属性描述：");
        // 获取所有的属性描述
        PropertyDescriptor[] pds = beanInfo.getPropertyDescriptors();
        for (PropertyDescriptor propertyDescriptor : pds) {
            System.out.println(propertyDescriptor.getName());
        }
        System.out.println("所有方法描述：");
        for (MethodDescriptor methodDescriptor : beanInfo.getMethodDescriptors()) {
            System.out.println(methodDescriptor.getName());
            // Method method = methodDescriptor.getMethod();
        }
    }
```

输出：

```java
所有属性描述：
age
name
所有方法描述：
getName
setAge
setName
getAge
```

测试2，可以直接构造 PropertyDescriptors

```java
    @Test
    public void test2 () throws Exception {
        User user = new User("jack", 21);

        String propertyName = "name";
        PropertyDescriptor namePd = new PropertyDescriptor(propertyName, User.class);

        System.out.println("名字：" + namePd.getReadMethod().invoke(user));
        namePd.getWriteMethod().invoke(user, "tom");
        System.out.println("名字：" + namePd.getReadMethod().invoke(user));

        System.out.println("========================================");

        String agePropertyName = "age";
        PropertyDescriptor agePd = new PropertyDescriptor(agePropertyName, User.class);

        System.out.println("年龄：" + agePd.getReadMethod().invoke(user));
        agePd.getWriteMethod().invoke(user, 22);
        System.out.println("年龄：" + agePd.getReadMethod().invoke(user));

    }
```

结果:

```java
名字：jack
名字：tom
========================================
年龄：21
年龄：22
```