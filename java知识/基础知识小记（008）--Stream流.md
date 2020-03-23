# Stream流
* 作用：高效遍历集合，对元素进行过滤
* 使用传统的方式,遍历集合,对集合中的数据进行过滤，如下
```
public class Demo01List {

public static void main(String[] args) {
    //创建一个List集合,存储姓名
    List<String> list = new ArrayList<>();
    list.add("张无忌");
    list.add("周芷若");
    list.add("赵敏");
    list.add("张强");
    list.add("张三丰");

    //对list集合中的元素进行过滤,只要以张开头的元素,存储到一个新的集合中
    List<String> listA = new ArrayList<>();
    for(String s : list){
        if(s.startsWith("张")){
            listA.add(s);
        }
    }

    //对listA集合进行过滤,只要姓名长度为3的人,存储到一个新集合中
    List<String> listB = new ArrayList<>();
    for (String s : listA) {
        if(s.length()==3){
            listB.add(s);
        }
    }

    //遍历listB集合
    for (String s : listB) {
        System.out.println(s);
    }
}
}
```
* 使用流式思想，使代码更加优雅简洁
```
使用Stream流的方式,遍历集合,对集合中的数据进行过滤
Stream流是JDK1.8之后出现的
关注的是做什么,而不是怎么做
*/
public class Demo02Stream {

public static void main(String[] args) {
    //创建一个List集合,存储姓名
    List<String> list = new ArrayList<>();
    list.add("张无忌");
    list.add("周芷若");
    list.add("赵敏");
    list.add("张强");
    list.add("张三丰");

    //对list集合中的元素进行过滤,只要以张开头的元素,存储到一个新的集合中
    //对listA集合进行过滤,只要姓名长度为3的人,存储到一个新集合中
    //遍历listB集合
    list.stream()
            .filter(name->name.startsWith("张"))
        .filter(name->name.length()==3)
        .forEach(name-> System.out.println(name));
}
}
```
* Stream调用方法操作后原有Stream对象不改变，返回一个新的Stream对象，这就允许对其操作可以像链条一样排列，变成一个管道一样
* 三步：获取一个数据源（数组集合等）、数据转换、执行操作获取想要的结果
## 两种获取Stream流的方法
>        java.util.stream.Stream<T>是Java 8新加入的最常用的流接口。（这并不是一个函数式接口。）
>        获取一个流非常简单，有以下几种常用的方式：
>            - 所有的Collection集合都可以通过stream默认方法获取流；
>                default Stream<E> stream()
>            - Stream接口的静态方法of可以获取数组对应的流。
>                static <T> Stream<T> of(T... values)
>                参数是一个可变参数,那么我们就可以传递一个数组
```
public class Demo01GetStream {

public static void main(String[] args) {
    //把集合转换为Stream流
    List<String> list = new ArrayList<>();
    Stream<String> stream1 = list.stream();

    Set<String> set = new HashSet<>();
    Stream<String> stream2 = set.stream();

    Map<String,String> map = new HashMap<>();
    //获取键,存储到一个Set集合中
    Set<String> keySet = map.keySet();
    Stream<String> stream3 = keySet.stream();

    //获取值,存储到一个Collection集合中
    Collection<String> values = map.values();
    Stream<String> stream4 = values.stream();

    //获取键值对(键与值的映射关系 entrySet)
    Set<Map.Entry<String, String>> entries = map.entrySet();
    Stream<Map.Entry<String, String>> stream5 = entries.stream();

    //把数组转换为Stream流
    Stream<Integer> stream6 = Stream.of(1, 2, 3, 4, 5);
    //可变参数可以传递数组
    Integer[] arr = {1,2,3,4,5};
    Stream<Integer> stream7 = Stream.of(arr);
    String[] arr2 = {"a","bb","ccc"};
    Stream<String> stream8 = Stream.of(arr2);
}
} 
```

## Stream流中的方法可分为：延迟方法和终结方法
* 延迟方法：返回值类型任然是Stream接口自身类型的方法，因此支持链式调用。（除了终结方法外，其余方法均为延迟方法）
* 终结方法：返回值类型不再是Stream接口自身类型的方法，因此不再支持类似StringBuilder那样的链式调用。
* 逐一处理：forEach
>       Stream流中的常用方法_forEach
>       void forEach(Consumer<? super T> action);
>       该方法接收一个Consumer接口函数，会将每一个流元素交给该函数进行处理。
>       Consumer接口是一个消费型的函数式接口,可以传递Lambda表达式,消费数据
>       简单记:
>           forEach方法,用来遍历流中的数据
>           是一个终结方法,遍历之后就不能继续调用Stream流中的其他方法
* 例子

```
public class Demo02Stream_forEach {

public static void main(String[] args) {
    //获取一个Stream流
    Stream<String> stream = Stream.of("张三", "李四", "王五", "赵六", "田七");
    //使用Stream流中的方法forEach对Stream流中的数据进行遍历
    /*stream.forEach((String name)->{
        System.out.println(name);
    });*/

    stream.forEach(name->System.out.println(name));
}
}
```
* Stream流中的常用方法_filter:用于对Stream流中的数据进行过滤，延迟方法
>       Stream<T> filter(Predicate<? super T> predicate);
>       filter方法的参数Predicate是一个函数式接口,所以可以传递Lambda表达式,对数据进行过滤
>       Predicate中的抽象方法:
>           boolean test(T t);
```
public class Demo03Stream_filter {

public static void main(String[] args) {
    //创建一个Stream流
    Stream<String> stream = Stream.of("张三丰", "张翠山", "赵敏", "周芷若", "张无忌");
    //对Stream流中的元素进行过滤,只要姓张的人
    Stream<String> stream2 = stream.filter((String name)->{return name.startsWith("张");});
    //遍历stream2流
    stream2.forEach(name-> System.out.println(name));


    //遍历stream流
    stream.forEach(name-> System.out.println(name));
}
}
```

*Stream流属于管道流,只能被消费(使用)一次
        第一个Stream流调用完毕方法,数据就会流转到下一个Stream上
        而这时第一个Stream流已经使用完毕,就会关闭了
        所以第一个Stream流就不能再调用方法了
        IllegalStateException: stream has already been operated upon or closed*
## Stream流中的常用方法_map:用于类型转换,延迟方法
>       如果需要将流中的元素映射到另一个流中，可以使用map方法.
>       <R> Stream<R> map(Function<? super T, ? extends R> mapper);
>       该接口需要一个Function函数式接口参数，可以将当前流中的T类型数据转换为另一种R类型的流。
>       Function中的抽象方法:
>           R apply(T t);
```
public class Demo04Stream_map {

public static void main(String[] args) {
    //获取一个String类型的Stream流
    Stream<String> stream = Stream.of("1", "2", "3", "4");
    //使用map方法,把字符串类型的整数,转换(映射)为Integer类型的整数
    Stream<Integer> stream2 = stream.map((String s)->{
        return Integer.parseInt(s);
    });
    //遍历Stream2流
    stream2.forEach(i-> System.out.println(i));
}
}
```
## Stream流中的常用方法_count:用于统计Stream流中元素的个数，终结方法
>       long count();
>       count方法是一个终结方法,返回值是一个long类型的整数
>       所以不能再继续调用Stream流中的其他方法了
```
public class Demo05Stream_count {

public static void main(String[] args) {
    //获取一个Stream流
    ArrayList<Integer> list = new ArrayList<>();
    list.add(1);
    list.add(2);
    list.add(3);
    list.add(4);
    list.add(5);
    list.add(6);
    list.add(7);
    Stream<Integer> stream = list.stream();
    long count = stream.count();
    System.out.println(count);//7
}
}
```
## Stream流中的常用方法_limit:用于截取流中的元素
>       limit方法可以对流进行截取，只取用前n个。方法签名：
>       Stream<T> limit(long maxSize);
>           参数是一个long型，如果集合当前长度大于参数则进行截取；否则不进行操作
>       limit方法是一个延迟方法,只是对流中的元素进行截取,返回的是一个新的流,所以可以继续调用Stream流中的其他方法
```
    public class Demo06Stream_limit {

public static void main(String[] args) {
    //获取一个Stream流
    String[] arr = {"美羊羊","喜洋洋","懒洋洋","灰太狼","红太狼"};
    Stream<String> stream = Stream.of(arr);
    //使用limit对Stream流中的元素进行截取,只要前3个元素
    Stream<String> stream2 = stream.limit(3);
    //遍历stream2流
    stream2.forEach(name-> System.out.println(name));
}
}  
```
## Stream流中的常用方法_skip:用于跳过元素
>       如果希望跳过前几个元素，可以使用skip方法获取一个截取之后的新流：
>       Stream<T> skip(long n);
>           如果流的当前长度大于n，则跳过前n个；否则将会得到一个长度为0的空流。
```
   public class Demo07Stream_skip {

public static void main(String[] args) {
    //获取一个Stream流
    String[] arr = {"美羊羊","喜洋洋","懒洋洋","灰太狼","红太狼"};
    Stream<String> stream = Stream.of(arr);
    //使用skip方法跳过前3个元素
    Stream<String> stream2 = stream.skip(3);
    //遍历stream2流
    stream2.forEach(name-> System.out.println(name));
}
} 
```
## Stream流中的常用方法_concat:用于把流组合到一起
>       如果有两个流，希望合并成为一个流，那么可以使用Stream接口的静态方法concat
>       static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
```
public class Demo08Stream_concat {

public static void main(String[] args) {
    //创建一个Stream流
    Stream<String> stream1 = Stream.of("张三丰", "张翠山", "赵敏", "周芷若", "张无忌");
    //获取一个Stream流
    String[] arr = {"美羊羊","喜洋洋","懒洋洋","灰太狼","红太狼"};
    Stream<String> stream2 = Stream.of(arr);
    //把以上两个流组合为一个流
    Stream<String> concat = Stream.concat(stream1, stream2);
    //遍历concat流
    concat.forEach(name-> System.out.println(name));
}
}
```
## 练习
* 练习:集合元素处理（传统方式）
>           现在有两个ArrayList集合存储队伍当中的多个成员姓名，要求使用传统的for循环（或增强for循环）依次进行以下若干操作步骤：
>           1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
>           2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
>           3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
>           4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
>           5. 将两个队伍合并为一个队伍；存储到一个新集合中。
>           6. 根据姓名创建Person对象；存储到一个新集合中。
>           7. 打印整个队伍的Person对象信息。
```
public class Demo01StreamTest {

public static void main(String[] args) {
    //第一支队伍
    ArrayList<String> one = new ArrayList<>();
    one.add("迪丽热巴");
    one.add("宋远桥");
    one.add("苏星河");
    one.add("石破天");
    one.add("石中玉");
    one.add("老子");
    one.add("庄子");
    one.add("洪七公");
    //1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
    ArrayList<String> one1 = new ArrayList<>();
    for (String name : one) {
        if(name.length()==3){
            one1.add(name);
        }
    }
    //2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
    ArrayList<String> one2 = new ArrayList<>();
    for (int i = 0; i <3 ; i++) {
        one2.add(one1.get(i));//i = 0,1,2
    }

    //第二支队伍
    ArrayList<String> two = new ArrayList<>();
    two.add("古力娜扎");
    two.add("张无忌");
    two.add("赵丽颖");
    two.add("张三丰");
    two.add("尼古拉斯赵四");
    two.add("张天爱");
    two.add("张二狗");
    //3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
    ArrayList<String> two1 = new ArrayList<>();
    for (String name : two) {
        if(name.startsWith("张")){
            two1.add(name);
        }
    }
    //4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
    ArrayList<String> two2 = new ArrayList<>();
    for (int i = 2; i <two1.size() ; i++) {
        two2.add(two1.get(i)); //i 不包含0 1
    }

    //5. 将两个队伍合并为一个队伍；存储到一个新集合中。
    ArrayList<String> all = new ArrayList<>();
    all.addAll(one2);
    all.addAll(two2);

    //6. 根据姓名创建Person对象；存储到一个新集合中。
    ArrayList<Person> list = new ArrayList<>();
    for (String name : all) {
        list.add(new Person(name));
    }

    //7. 打印整个队伍的Person对象信息。
    for (Person person : list) {
        System.out.println(person);
    }
}
} 
```
* 练习：集合元素处理（Stream方式）
>       将上一题当中的传统for循环写法更换为Stream流式处理方式。
>       两个集合的初始内容不变，Person类的定义也不变。
```
public class Demo02StreamTest {

public static void main(String[] args) {
    //第一支队伍
    ArrayList<String> one = new ArrayList<>();
    one.add("迪丽热巴");
    one.add("宋远桥");
    one.add("苏星河");
    one.add("石破天");
    one.add("石中玉");
    one.add("老子");
    one.add("庄子");
    one.add("洪七公");
    //1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
    //2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
    Stream<String> oneStream = one.stream().filter(name -> name.length() == 3).limit(3);

    //第二支队伍
    ArrayList<String> two = new ArrayList<>();
    two.add("古力娜扎");
    two.add("张无忌");
    two.add("赵丽颖");
    two.add("张三丰");
    two.add("尼古拉斯赵四");
    two.add("张天爱");
    two.add("张二狗");
    //3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
    //4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
    Stream<String> twoStream = two.stream().filter(name -> name.startsWith("张")).skip(2);

    //5. 将两个队伍合并为一个队伍；存储到一个新集合中。
    //6. 根据姓名创建Person对象；存储到一个新集合中。
    //7. 打印整个队伍的Person对象信息。
    Stream.concat(oneStream,twoStream).map(name->new Person(name)).forEach(p-> System.out.println(p));
}
}
```










