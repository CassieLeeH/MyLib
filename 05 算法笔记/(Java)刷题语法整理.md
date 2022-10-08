# 输入输出问题汇总

## BufferedReader/Scanner输入

```java
//BufferedReader类位于java.io包中,要引入java.io
import java.io.BufferedReader;
import java.io.InputStreamReader;

BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String s = br.readLine();// 单行读取

String[] s = br.readLine().split(" ");//按“ ”划分
int n = Integer.parseInt(s[0]);// 第一个值为由string转为int n
int x = Integer.parseInt(s[1]);
int y = Integer.parseInt(s[2]);
```

- BufferedReader.readLine()方法会返回用户在按下Enter键之前的所有字符输入,不包括最后按下的Enter返回字符.
- 使用BufferedReader对象的readLine()方法必须处理java.io.IOException异常(Exception)。
- BufferReader读取的数据都以字符串的形式存储，如果需要其他形式的数据，需要进行强制转换。

## 输出

```java
//待补充 
sout
System.out.println(n);
```


# 类型转换

## String转换为char、int、Integer

- String -> char(i) 
  - 使用**String.charAt(index)（返回值为char）**
  - 可以得到String中某一指定位置的char。
- String -> char[ ] 
  - 使用**String.toCharArray()（返回值为char[ ]）**
  - 可以得到将包含整个String的char数组。这样我们就能够使用从0开始的位置索引来访问string中的任意位置的元素。
- String -> int 
  - 使用**Integer.parseInt(str)** 进行转换，返回str所代表的的int值大小。
- String -> Integer 
  - 使用**Integer.valueOf(str)**，返回Integer对象。

## char转换为String、int

- char -> int  使用    (int)(char - '0')
- char -> String	

```java
1. String s = String.valueOf('c'); //效率最高的方法

2. String s = String.valueOf(new char[]'c'}); //将一个char数组转换成String

3. String s = "" + 'c';
/*** 虽然这个方法很简单，但这是效率最低的方法
Java中的String Object的值实际上是不可变的，是一个final的变量。
所以我们每次对String做出任何改变，都是初始化了一个全新的String Object并将原来的变量指向了这个String。
而Java对使用+运算符处理String相加进行了方法重载。
字符串直接相加连接实际上调用了如下方法：
	new StringBuilder().append("").append('c').toString();
***/

```

## Int转换为String、Integer、char

```java
//int型 转 String型
        String str1=Integer.toString(in);  //使用Integer.toString()
        String str2=String.valueOf(in);	//使用String.valueOf(int i);返回String
        String str3 = "" + in; //少用的方式
//int型 转 char型
        char cha=(char)(in+'0');
//int型 转 Integer型
		    Integer integer=new Integer(in);
```

## String数组转为int数组

```java
String[] strings = {"1", "2", "3"};
//1. 使用stream流 
int[] array = Arrays.asList(strings).stream().mapToInt(Integer::parseInt).toArray();
	
//2. 或
int[] array = Arrays.stream(strings).mapToInt(Integer::parseInt).toArray();

// 3.对String数组中的每一个str进行操作
int[] intarray = new int[strings.length];
int i=0;
for(String str:strings){
    intarray[i++]=Integer.parseInt(str);
}
```

# 数据结构
## 栈

```java
Stack<Integer> stack = new Stack<>();
Deque<Integer> stack = new LinkedList<Integer>();
//压栈
stack.push(object);
//出栈
stack.pop();
//查看栈顶元素
stack.peek();
//判断是否为空
stack.empty();
```

## 队列

```java
//queue 单向队列
Queue<Integer> queue = new LinkedList<Integer>();
// 入队
queue.offer(object);
// 出队
queue.poll();
// 查看队首元素
queue.peek();


//deque 双端队列
LinkedList<Integer> deque = new LinkedList<Integer>();
// 从头或者尾入队
deque.offerFirst(object);
deque.offerLast(object);
// 从头或者尾出队
deque.pollFirst();
deque.pollLast();
//查看两段元素
deque.peekFirst();
deque.peekLast();

```

# 容器

## Arrays

```java
// 初始化
int[] a = new int[10];

// 长度
arrays.length
// 转换为字符串
arrays.toString()


// 排序
Arrays.sort(a);  // 1 默认排序，升序
Arrays.sort(a, firstIdx, lastIdx);  // 2 指定位置进行排序，默认升序，排序区间为左闭右开 [firstIdx, lastIdx)
Arrays.sort(b, new Comparator<Integer>() {  
  // 3 自定义的形式排序
  public int compare(Integer o1, Integer o2) {
    return o2 - o1;
  }
});
Arrays.sort(b, firstIdx, lastIdx, new Comparator<Integer>() {  
  // 4 指定位置进行自定义排序
  public int compare(Integer o1, Integer o2) {
    return o2 - o1;
  }
});
// 由于 Java 8 后有 Lambda 表达式，第三个重载及第四个重载亦可写为 
Arrays.sort(b, (x, y) -> {  // 5 同3
  return y - x;
});
Arrays.sort(b, (x, y) -> {  // 6 同4
  return y - x;
});

// Arrays.binarySearch() 是对数组连续区间进行二分搜索的方法，前提是数组必须有序；
Arrays.binarySearch(a, key); 
Arrays.binarySearch(a, firstIdx, lastIdx, key);  

// Arrays.fill() 将数组中连续位置的元素赋值为统一元素
Arrays.fill(a,2,5,100); 
//其接受的参数为数组、fromIndex、toIndex 和需要填充的数。
                                                                     
```


## Set

`Set` 是保持容器中的元素不重复的一种数据结构。

```java
// 随机位置插入的 Set
Set<Integer> s1 = new HashSet<>();
// 保持插入顺序的 Set
Set<Integer> s2 = new LinkedHashSet<>();
// 保持容器中元素有序的 Set，默认为升序
Set<Integer> s3 = new TreeSet<>();
Set<Integer> s4 = new TreeSet<>((x, y) -> {return y - x;});  // 降序 
// 遍历
for (int key : s1) {
  //
}
```

常用方法：

|          函数名           |                   功能                   |
| :-----------------------: | :--------------------------------------: |
|         `size()`          |             返回 this 的长度             |
|    `add(Integer val)`     |           插入一个元素进 this            |
|  `contains(Integer val)`  |        判断 this 中是否有元素 val        |
|  `addAll(Collection e)`   |    将一个容器里的所有元素添加进 this     |
| `retainAll(Collection e)` | 将 this 改为两个容器内相同的元素（查重） |
| `removeAll(Collection e)` |      将 this 中与 e 相同的元素删除       |

## Map

`Map` 是维护键值对 `<Key, Value>` 的一种数据结构，其中 `Key` 唯一。

```java

//随机位置插入的 Map
Map<Integer, Integer> map1 = new HashMap<>();
//保持插入顺序的 Map
Map<Integer, Integer> map2 = new LinkedHashMap<>();
//保持 key 有序的 Map，默认升序。
Map<Integer, Integer> map3 = new TreeMap<>();
Map<Integer, Integer> map4 = new TreeMap<>((x, y) -> {return y - x;});  // 降序

//遍历
for (int key : map1.keySet()) {
	//
}

//当Map[集合]中有这个key时，就使用这个key对应的value值，如果没有就使用默认值defaultValue
getOrDefault(Object key, V defaultValue)  

```

常用方法：

|              函数名               |                 功能                  |
| :-------------------------------: | :-----------------------------------: |
| `put(Integer key, Integer value)` |          插入一个元素进 this          |
|             `size()`              |           返回 this 的长度            |
|    `containsKey(Integer val)`     |   判断 this 中是否有元素 key 为 val   |
|        `get(Integer key)`         |  将 this 中对应的 key 的 value 返回   |
|             `keySet`              | 将 this 中所有元素的 key 作为集合返回 |



# String类方法

```java
String name;
// 用于字符串拼接
StringBuilder sb = new StringBuilder();

//获取字符串长度
name.length());

//指定字符在此字符串中第一次出现的索引
name.indexOf('z'));
name.indexOf("zhj");
name.indexOf("zhj",0);//fromIndex指从哪边开始第一个出现

//"指定字符在此字符串中最后一次出现的索引";
name.lastIndexOf('z');
name.lastIndexOf("zhj");
name.lastIndexOf("zhj",5);//fromIndex指从哪边结束的最后一个出现
name.charAt(0);//返回对应索引上的字符

// 重复n次
name.repeat(n);

//将字符串转换为字符数组
name.toCharArray();

//将int型转换为字符串
String.valueOf(123);

//将String中所有字符变成小写/大写
name.toLowerCase();
name.toUpperCase();

//用新的str代替旧的str
name.replace("zhj","xxx");

String trim1 ="  zhj 123  ";
// 除去字符串前后空格
trim1.trim();

// 用特定规则分割字符串，比如','
String str1 = "aa,bb,cc";
String[] split1 = str1.split(",");
for (int i = 0; i < split1.length ; i++) {
	System.out.println(split1[i]);
}
// 或者
for (String s: str1.split(SEP)) {  
	、、、
}


// 截取字符,从beginIndex开始，包括第一个,不包括最后一个
name.substring(1);
name.substring(0,1);


// 比较两个字符串是否相等
String name2 = "ZHJ";
name.equals(name2));//false

// 判断字符串是否以指定的字符串开始/结束
name.startsWith("zhj");//true
name.endsWith("zhj");//true

// 字符串里是否包含指定的字符串
name.contains("zhj");//true

// 判断字符串长度是否为0;
name.isEmpty();//false

```


# Math类方法整理
```java
import java.math.*;
 
public class Test {
    public static void main(String[] args){
        //返回a的绝对值
        double a = -5.0;
        double abs = Math.abs(a);
        System.out.println("a的绝对值:"+abs);
 
        //返回两个数中的最大值、最小值
        double b =10.0;
        double max = Math.max(a,b);
        double min = Math.min(a,b);
        System.out.println("a，b的最大值为："+max);
        System.out.println("a，b的最小值为："+min);
 
        //产生一个0-1之间的随机数（包括0，不包括1）
        double random = Math.random();
        System.out.println("产生一个0-1之间的随机数:"+random);
 
        //返回a的3次幂, pow(a,2)就是平方
        double pow = Math.pow(a,3);
        System.out.println("a的3次幂为："+pow);
 
        //返回b的平方根
        double sqrt = Math.sqrt(b);
        System.out.println("b的平方根为："+sqrt);
 
        //返回c的对数
        double c = 8.0;
        double log = Math.log(c);
        System.out.println("c的对数为："+log);
 
        //返回d的正弦值
        double d = 0.5;
        double sin = Math.sin(d);
        System.out.println("d的正弦值为："+sin);
 
        //返回d的反正弦值
        double asin = Math.asin(d);
        System.out.println("d的反正弦值为："+asin);
 
        //返回大于d的最小整数，并将该整数转化为double数据
        double ceil = Math.ceil(d);
        System.out.println("大于d的最小整数为："+ceil);
 
        //返回小于d的最大整数，并将该整数转化为double数据
        double floor = Math.floor(d);
        System.out.println("小于d的最大整数为："+floor);
 
        //返回某个数的四舍五入的值
        System.out.println(Math.round(15.6));
        System.out.println(Math.round(15.4));
        System.out.println(Math.round(-15.5));
        System.out.println(Math.round(-15.6));
        /*
        如果该数为非负数，小数大于或等于0.5入，小于0.5舍
        如果该数为负数，小数大于0.5入，小于或等于0.5舍
         */
    }
}

```

oj题解收集，学习java解题风格

```java



import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.Arrays;
 
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine().trim());
        String[] strSeq = br.readLine().trim().split(" ");
        int[] seq = new int[n];
        for(int i = 0; i < n; i++) seq[i] = Integer.parseInt(strSeq[i]);
        Arrays.sort(seq);
        int modifyTimes = 0;
        for(int i = 1; i <= n; i++) modifyTimes += Math.abs(seq[i - 1] - i);
        System.out.println(modifyTimes);
    }


  
import java.io.*;
import java.util.*;
 
public class Main {
 
    public static void main(String[] args) throws IOException{
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(System.out));
        int T = Integer.parseInt(reader.readLine());
        for (int i = 0; i < T; i++) {
            int N = Integer.parseInt(reader.readLine());
            String tables = reader.readLine();
            int M = Integer.parseInt(reader.readLine());
            String enters = reader.readLine();
 
            int[] res = solve(tables, enters);
            for (int r : res) {
                writer.write(Integer.toString(r));
                writer.newLine();
            }
        }
        writer.flush();
    }
 
    private static int[] solve(String tables, String enters) {
        List<PriorityQueue<Integer>> pqs = new ArrayList<>(3);
        pqs.add(new PriorityQueue<>());
        pqs.add(new PriorityQueue<>());
        pqs.add(new PriorityQueue<>());
        for (int i = 0; i < tables.length(); i++) {
            pqs.get(tables.charAt(i) - '0').add(i);
        }
        int[] res = new int[enters.length()];
        for (int i = 0; i < enters.length(); i++) {
            int table;
            if (enters.charAt(i) == 'M') {
                if (pqs.get(1).isEmpty()) {
                    table = pqs.get(0).poll();
                    pqs.get(1).add(table);
                } else {
                    table = pqs.get(1).poll();
                    pqs.get(2).add(table);
                }
            } else {
                if (!pqs.get(0).isEmpty()) {
                    table = pqs.get(0).poll();
                    pqs.get(1).add(table);
                } else {
                    table = pqs.get(1).poll();
                    pqs.get(2).add(table);
                }
            }
            res[i] = table + 1;
        }
 
        return res;
    }
 
}
```

