### 什么是Stream

​	一种流式操作，需要一个数据源，数组或者集合。每次操作都会产生一个新的流对象，方便链式操作，但是原有的流对象不变。

### 流的操作可以分为两类

* 中间操作：可以有多个，每次操作都会产生一个新的流对象，可以进行链式操作。
* 终端操作：有且只有一个，必须放到最后，终结这个流操作。

返回类型未Stream的为中间操作，返回非Stream类型的为终端操作。

### 1、创建流

数组有两种方法：Arrays.stream()、Stream.of();

集合可以使用stream()方法直接生成，该方法已经加入Collection接口中；

list.parallelStream()可以创建并发流。

```java
public class CreatStreamDemo {
    public static void main(String[] args) {   
        //数组1
        String[] strs = {"zs", "ls", "ww"};
        Stream<String> stream1 = Arrays.stream(strs);

        //数组2
        Stream<String> stream2 = Stream.of(strs);
        
        //集合
        List<String> list = new ArrayList<>();
        list.add("zs");
        list.add("ls");
        list.add("ww");
        Stream<String> stream = list.stream();
        
        //并发流
        Stream<String> stringStream = list.parallelStream();

    }
}
```

### 2、操作流

#### 2.1、筛选数据

##### 	1、通过filter()方法可以用于筛选数据。

```java
public class FilterStreamDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("zs");
        list.add("ls");
        list.add("ww");
        //创建流
        Stream<String> stream = list.stream();
        //流中间操作
        Stream<String> s = stream.filter(c -> c.contains("s"));
        //流终结操作
        s.forEach(System.out::println);

        //同一个流只能遍历一次
        //stream.filter(a-> a.contains("s")).forEach(System.out::println);

        //合并简写
        System.out.println("合并简写");
        list.stream().filter(a-> a.contains("s")).forEach(System.out::println);
    }
}
输出结果为：
    zs
    ls
```

##### 2、通过distinct()方法进行去重

```java
public class DistinctStreamDemo {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("a", "b", "c", "d", "e", "a", "b");
        List<String> collect = list.stream().distinct().collect(Collectors.toList());
        System.out.println(collect);
    }
}
输出结果为：["a","b","c","d","e"]
```

##### 3、通过limit()方法截断流

```java
public class LimitStreamDemo {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("a", "b", "c", "d", "e", "a", "b");
        //截取list的前3个元素放入collect中
        List<String> collect = list.stream().limit(3).collect(Collectors.toList());
        System.out.println(collect);
    }
}
输出结果为：["a","b","c"]
```

##### 4、通过skip()方法跳过前n个元素

```java
public class SkipStreamDemo {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("a","b","c");
        List<String> collect = list.stream().skip(2).collect(Collectors.toList());
        System.out.println(collect);
    }
}
输出结果：["c"]
```

#### 2.2、映射

##### 1、map()方法，将一个类型的List转换为另一个类型List

```java
public class MapStreamDemo {
    public static void main(String[] args) {
        List<String> strings = Arrays.asList("hello", "world");
        //将String类型的List转换为String[]类型的List
        List<String[]> collect = strings.stream().map(w -> w.split("")).collect(Collectors.toList());

        collect.forEach(d -> {
            for (String s : d) {
                System.out.println(s);
            }
        });
    }
}
```

#### 2.3、查找与匹配

##### 1、anyMatch()方法

​	判断流中是否有一个元素匹配，只要存在一个就返回true，否则返回false。

```java
public class FindMatchStreamDemo {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("a", "b", "c", "d", "e", "a", "b");
        System.out.println(list.stream().anyMatch(a -> a.contains("a")));
        System.out.println(list.stream().anyMatch(a -> a.contains("g")));
    }
}
返回结果：
    true
    false
```

##### 2、allMatch()方法与noneMatch()方法

​	allMatch()判断流中是否所有的元素都匹配，所有的都匹配就返回true，否则返回false。noneMatch()与allMatch()相反。

```java
public class FindMatchStreamDemo {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(3, 5, 6,7, 7, 3, 1);
        System.out.println(list.stream().allMatch(a -> a > 6));
        System.out.println(list.stream().noneMatch(a -> a > 6));
        System.out.println(list.stream().allMatch(a -> a < 9));
        System.out.println(list.stream().noneMatch(a -> a < 9));
    }
}
返回结果：
    false
    false
    true
    false
```

##### 3、findAny()方法

​	查找当前流任一元素，返回一个Optional对象

```java

```

