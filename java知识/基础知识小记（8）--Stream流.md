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





























