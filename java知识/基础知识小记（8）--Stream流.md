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



























