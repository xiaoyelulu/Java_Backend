### TreeMap 源码解析
## Collections 工具类
### Collections 工具类介绍
1. Collectiohns 是一个操作 Set、 List、 和 Map 等集合的工具类
2. Collections 中提供了一系列静态的方法对集合元素进行排序、查找和修改等操作

### 排序操作： （均为 static 方法）
1. reverse(List) : 反转 List 中元素的顺序
2. shuffle(List) : 对 List 集合元素进行随机排序
3. sort (List) : 根据元素的自然顺序对指定 List 集合元素按升序排序
4. sort (List, Comparator) : 根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
5. swap(List, int, int) : 将指定 list 集合中的 i 处元素和 j 处元素进行交换

### 查找、替换
1. Object max(Collection) : 根据元素的自然顺序，返回给定集合中的最大元素
2. Object max(Collection, Comparator) : 根据 Comparator 指定的顺序，返回给定集合中的最大元素
3. Object min(Collection)
4. Object min(Collection, Comparator)
5. int frequency(Collection, Object) : 返回指定集合中指定元素出现的次数
6. void copy(List dest, List src) : 将 src 中的内容复制到 dest 中
    - 为了完成一个完整拷贝，我们需要先给 dest 赋值，大小和 src 大小相同
7. boolean replaceAll （List list, Object oldVal, Object newVal） ： 使用新值替换 List 对象的所有旧值

## 试分析 HashSet 和 TreeSet 分别如何实现去重的
1. HashSet 去重机制： hashCode() + equals()，底层先通过存入对象，进行运算得到一个 hash 值，通过 hash 值得到对应的索引，如果发现 table 索引所在的位置，没有数据，就直接存放，如果有数据，就进行 equals 比较 [遍历比较]，如果比较后，不相同，就加入，否则不加入
2. TreeSet 去重机制： 如果你传入了一个 Comparator 匿名对象，就使用实现的 compare 去重，如果方法返回 0 ，就认为是相同的元素 / 数据，就不添加，如果你没有传入一个 Comparator 匿名对象，则以你添加的对象实现的 Compareable 接口的 compareTo去重。
   
### 代码检测：
```java
main{
    // 会报错，
    // add 方法，因为 TreeSet() 构造器没有传入 Comparator 接口的匿名内部类
    //所以底层在执行 (Comparable<? super K>)k1 报错
    //即把 Person 类 转成 Comparable 类型
    TreeSet treeSet = new TreeSet();
    treeSet.add(new Person());

}

class Person{};

//修改为如下既不报错
class Person implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        return 0;
    }

}
```

### 家庭作业
对Hash 放进入的依据，和加入、删除的语法更深层次掌握

![Alt text](pictures/java后端入门第18天.png)

解读：
1. `set.remove(p1);` 根据 新的值 1001 和 CC 计算对应的 hash 值，而原来的 p1 是根据 1001 AA 的 hash 值而放置的位置，所以找不到原来的位置了，就不能删除 p1 了
2. ` set.add(new Person(1001, "CC"));` 同理，虽然 set 中 p1 的以及 变为 1001 和 CC ，但放入数据时，hash 值是根据 1001 和 AA 计算放置的，所以能成果放置
3. ` set.add(new Person(1001, "AA"));`，可以根据 Hash 值加入到 p1 后面，但由于内容已经不同，所以可以成功加入

# 第 15 章 泛型
## 泛型的理解和好处
### 使用传统方法的问题分析
1. 不能对加入到集合 ArrayList 中的数据类型进行约束（不安全）
2. 遍历的时候，需要进行类型转换，如果集合中的数据量较大，对效率有影响

### 泛型的好处
1. 编译时，检查添加元素的类型，提高了安全性
2. 减少了类型转换的次数，提高效率
3. 不再提醒编译警告

## 泛型介绍
泛（广泛）型（类型） => Integer， String， Dog
1. 泛型又称参数化类型，是 Jdk 5.0 出现的新特性，解决数据类型的安全性问题
2. 在类声明或实例化时只要指定好需要的具体的类型即可
3. java 泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生 ClassCastException 异常。同时，代码更加简洁、健壮
4. 泛型的作用 ： 可以在类声明时通过一个标识表示类中某个属性的类型，或者时某个方法的返回值类型，或者是参数类型。

## 泛型的语法
### 泛型的声明
`interface 接口 <T> {} 和 class 类<K ,V> {}
`

**说明：**
1. 其中，T、K、V 不代表值，而是表示类型
2. 任意字母都可以。常用 T 表示， Type 缩写

### 泛型的实例化
要在类名后面指定类型参数的值（类型）。如：
1. ` List <String> strList = new ArrayList<String>()`
2. ` Iterator <Customer> iterator = customers.iterator();`

### 泛型注意事项 和 细节
1. `interface List<T> {}，public class HashSet<E> {} `...等等
    - T、E只能是引用类型 
    - 不能是 int 、float 等基本数据类型
2. 在给泛型指定具体类型后，可以传入该类型或者其子类类型
3. 泛型使用形式
    - ` List <Integer> list1 = new ArrayList<Integer>();`
    - ` List <Integer> list2 = new ArrayList<>();` //编译器会进行类型推断
4. 如果我们这样写 ` List list3 = new ArrayList();` ， 默认给它的 泛型时 `[<E> E 就是 Object]`
5. 封装后可复用性，和可维护性大大增强

## 自定义泛型
### 自定义泛型类(可以接收数据类型的一种数据类型)
**基本语法：**
```java
class 类名 <T, R...>{ // ...表示可以有多个泛型
    成员；
}
```
**注意细节：**
1. 普通成员可以使用泛型（属性、方法）
2. 泛型标识符可以有多个
3. 使用泛型的数组，不能初始化。 `//因为数组在 new 不能确定 T 的类型，就无法在内存开空间`
4. 静态方法中不能使用类的泛型。 `//因为静态是和类相关的，在类加载时，对象还没有创建，所以，如果静态方法和静态属性使用了泛型，JVM 就无法完成初始化`
5. 泛型类的类型，是在创建对象时确定的（因为创建对象时，需要指定确定类型）
6. 如果在创建对象时，没有指定类型，默认为 Object

### 自定义泛型接口 
**基本语法：**
```java
interface 接口名<T, R...>{

}
```
**注意细节：**
1. 接口中，静态成员也不能使用泛型（和泛型类规定一致），所有静态变量都是静态的
2. 泛型接口的类型，在继承接口或者实现接口时确定
3. 没有指定类型，默认为 Object
4. 在 jdk8 中， 可以在接口中，使用默认方法，也是可以使用泛型

### 自定义泛型方法
**基本语法：**
```java
修饰符 <T, R...> 返回类型 方法名（参数列表）{

}
```
**注意细节：**
1. 泛型方法，可以定义在普通类中，也可以定义在泛型类中
2. 当泛型方法被调用时，类型会被确定 ;`//当调用方法时，传入参数，编译器，就会确定类型`
3. ` public void eat(E e){ }，`修饰符后没有<T, R...> eat 方法不是泛型方法，而是使用了类声明的泛型。 `//泛型方法，可以使用类声明的泛型，也可以使用自己声明泛型`

## 泛型的继承和通配符
### 泛型的继承和通配符说明
1. 泛型不具备继承性
2. `<?>` : 支持任意泛型类型
3. `<? extends A>` : 支持 A 类以及 A 类的子类，规定了泛型的上限 （实现 A）
4. `<? super A>` : 支持 A 类以及 A 类的父类，不限于直接父类，规定了泛型的下限 (超过 A)

## Junit
### 为什么需要 JUnit
1. 一个类有很多功能代码需要测试，为了测试，就需要写入到 main 方法中
2. 如果有多个功能代码测试，就需要来回注销，切换很麻烦
3. 如果可以直接运行一个方法，就很方便，并且可以给出相关信息 -> JUnit
### 基本介绍
1. JUnit 是一个 Java 语言的单元测试框架
2. 多数 java 的开发环境都已经集成了 JUnit 作为单元测试的工具

```java
@Test（alt + enter）
```
