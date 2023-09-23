## Math 类
Math 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数

1. abs 绝对值
2. pow 求幂
3. ceil 向上取整,返回>=该参数的最小整数（转成 double）
4. floor 向下取整，返回<=该参数的最大整数(转成 double)
5. round 四舍五入 Math.floor（该参数 + 0.5）
6. sqrt 求开方
7. random 求随机数。 random 返回的是 0 <= x < 1 之间的一个随机小数。 返回 [a ,b ] 之间的一个随机整数，``` (int)(a + Math.random() * ( b - a + 1) )```
8. max , min 返回最大值和最小值

- int 强转是 向下取整

## Arrays 类
### 常见方法
包含一系列静态方法，用于管理或操作数组 （比如排序和搜索）

1. toString 返回数组的字符串形式
2. sort 排序 （自然排序和定制排序）, 也可以通过传入一个接口 Comparator 实现定制排序。 
    - 调用 定制排序 时，传入两个参数 (1) 排序的数组 arr
    - 实现了 Comparator 接口的匿名内部类 , 要求实现 compare 方法
    - 将来的底层框架和源码的使用方式，会非常常见
3. binarySearch 通过二分搜索法进行查找，必须排好序
    - 要求该数组是有序的. 如果该数组是无序的，不能使用 binarySearch
    - 如果数组中不存在该元素，就返回 return -(low + 1); // key not found ; low 数据应该在的位置。
4. copyOf 数组元素的复制
    - 从 arr 数组中，拷贝 arr.length 个元素到 newArr 数组中
    - 如果拷贝的长度 > arr.length 就在新数组的后面 增加 null
    - 如果拷贝长度 < 0 就抛出异常 NegativeArraySizeException
    - 该方法的底层使用的是 System.arraycopy()
1. fill 数组元素的填充
2. equals 比较连哥哥数组元素内容是否完全一致
3. asList 将一组值，转换成 list
## System 类
1. exit 退出当前程序
2. arraycopy 复制数组元素，比较适合底层调用，一般使用 Arrays.copyOf 完成复制数组
3. currrentTimeMilis: f=返回当前时间距离 1970-1-1 的毫秒数
4. gc: 运行垃圾回收机制

## BigInteger 和 BigDecimal 类
- BigInteger 适合保存比较大的整型，字符串保存
- BigDecimal 适合保存精度更高的浮点型
### 常见方法
1. add 加
2. subtract 减
3. multiply
4. divide 除
    - 在调用 divide 方法时，指定精度即可. BigDecimal.ROUND_CEILING，如果有无限循环小数，就会保留 分子 的精度

## 日期类
### 第一代日期类
1. Date: 精确到毫秒，代表特定的瞬间
2. SimpleDataFormat: 格式和解析日期的类。允许进行格式化（日期 -> 文本）， 解析 （文本 -> 日期） 和 规范化。``` SimpleDateFormat sdf = new SimpleDateFormat("yyyy 年 MM 月 dd 日 hh:mm:ss E")```
3. 在把 String -> Date ， 使用的 sdf 格式需要和你给的 String 的格式一样，否则会抛出转换异常

### 第二代日期类
1. Calendar 类 （日历）
2. Calendar 类是一个抽象类，它为特定瞬间与一组等日历字段之间的转换提供了一些方法，并未操作日历字段提供了一些方法
#### 方法
1. Calendar 是一个抽象类， 并且构造器是 private
2. 可以通过 getInstance() 来获取实例
3. 提供大量的方法和字段提供给程序员
4. Calendar 没有提供对应的格式化的类，因此需要程序员自己组合来输出(灵活)
5. Calendar 返回月时候，是按照 0 开始编号
6. 如果我们需要按照 24 小时进制来获取时间， Calendar.HOUR ==改成=> Calendar.HOUR_OF_DAY

### 第三代日期类
JDK 1.0 中包含了一个 java.util.Date 类，但是它的大多数方法已经在 JDK 1.1 引入 Calendar 类之后被弃用了。而 Calendar 也存在如下问题：
1. 可变性：像日期和时间这样的类应该是不可变的
2. 偏移性：Date 中的年份是从 1900 开始的，而月份都是从 0 开始
3. 格式化： 格式化只对 Date 有用， Calendar 则不行
4. 他们是线程不安全的；不能处理闰秒（每隔两天，多出 1 s）

第三代：

LocalDate(日期 / 年月日)、LocalTime(时间 / 时分秒)、LocalDateTime (日期时间) JDK 8 加入
- LocalDate 只包含日期
- LocalTime 只包含时间
- LocalDateTime 包含 日期 + 时间
    1. 使用 now() 返回表示当前日期时间的 对象
    2. 使用 DateTimeFormatter 对象来进行格式化
    3. 提供 plus 和 minus 方法可以对当前时间进行加或者减
#### DateTimeFormatter 格式日期类
```java
DateTimeFormat dtf = DateTImeFormatter.ofPattern(格式)；
String str = dtf.format(日期对象)；
```
#### Instant 时间戳
类似于 Date 提供了一系列 和 Date 类转换的方式
```java
Instant ---> Date:
通过 from 可以把 Instant 转成 Date
Date date = Date.from(Instant)

Date ---> Instant:
通过 date 的 toInstant() 可以把 date 转成 Instant 对象
Instant instant = date.toInstant()
```
#### 更多方法
- LocalDateTime 类
- MonthDay 类：检查重复时间
- 是否是闰年
- 增加日期的某个部分
- 使用 plus 方法测试增加时间的某个部分
- 使用 minus 方法

## String 翻转 
双指针翻转
```java 
char [] c = str.toCharArray();
return new String(c);
throw new RuntimeException("参数不正确");
`s``
字符格式化：``` String.format()``` %s %d
 