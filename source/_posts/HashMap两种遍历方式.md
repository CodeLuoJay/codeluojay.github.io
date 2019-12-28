---
title: HashMap两种遍历方式
date: 2019-07-08 20:30:35
tags: HashMap
categories:  Java
images: "https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/JavaSE-HashMap/JavaSE-HashMap-index.jpg"
---

写在最前面的话：HashMap是Java集合中面试会被问得最多的一个知识点，而HashMap的遍历方法可以很好融合Java的泛型，自定义封装类的知识，所以很又必要掌握它的遍历。<!--more-->

### HashMap的遍历方式

```
Java 官方API文档提供三种视图给我们来遍历元素：
1. Set<K> keySet() ：返回一个 Set的key视图包含在这个Map。
2. Set<Map.Entry<K,V>> entrySet() ：返回一个 Set视图的映射包含在这个Map。
3. Collection<V> values() ：返回一个 Collection视图的值包含在这个Map。  
这里只记录前面两种方式遍历Map，所谓遍历就是要找到key和value值，并取出这两个值。
```

HashMap数据结构图示，默认创建长度为16的数组，根据哈希值计算在数组的索引位置，然后在相同索引位置的添加节点元素并以单向链表的形式排列节点元素。
![hashMap数据结构](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/hash_data_structure.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1567438101;1630510101&q-key-time=1567438101;1630510101&q-header-list=&q-url-param-list=&q-signature=1437633863269cb1def3067fe2431071391b9a6f)
其中每个节点元素的结构由Entry<key,value>和Node<k,v>组成，这里只对Entry<key,value>作图解释
![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568728879;1631800879&q-key-time=1568728879;1631800879&q-header-list=&q-url-param-list=&q-signature=27323730916030a77837f4b32d9f4189f6bfc138)



方式一：通过Set的key视图来遍历即通过 API文档中Set<K> keySet() 来获取，这种方式获取的思路如下：

1.通过HashMap中的keySet() 获取到所有的HashMap中的key,存放在一个Set集合中

2.遍历Set集合中的key去找每一个Key所对应的的value值

通过Set视图遍历就是一开始只会获取到一组Key的值，只知道Set集合里面的Key如下图所示

![Set视图](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map_traverse_01.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568731168;1631803168&q-key-time=1568731168;1631803168&q-header-list=&q-url-param-list=&q-signature=4d5a2b5dc23f673e1820e411656307bc1e2f25e6)

遍历Set集合中的key去找每一个Key所对应的的value值：按图中①到④的方式依次遍历

![key遍历value过程图](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map_traverse_01_1.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568732131;1631804131&q-key-time=1568732131;1631804131&q-header-list=&q-url-param-list=&q-signature=beee26c239a10cf2c5ceb649b42005eb94ae5603)

如果理解上述的思路及过程，那么便可以解决HashMap嵌套HashMap的问题

这里举个栗子：高中时候大多数学校都会划分重点班（实验班）和普通班，来因材施教（为了升学率）。

所以以这个场景为例提出下列需求，加深对方式一的理解：

| 学号     | 姓名   | 年龄 | 班级类型                |
| -------- | ------ | ---- | ----------------------- |
| 20190201 | bobi   | 21   | 普通班（NormalClass）   |
| 20190202 | luojie | 22   | 普通班（NormalClass）   |
| 20190101 | jay    | 18   | 重点班（AdvancedClass） |
| 20190102 | rose   | 16   | 重点班（AdvancedClass） |

将普通班所有学生各自封装成一个HashMap对象，重点班的所有学生封装成一个HashMap,学生封装成学生类（字段：姓名和年龄），然后再把两个HashMap对象存放在HashMap中，遍历输出如下

```java
Advancedclass	
				20190102------Student [name=rose, age=16]
				20190101------Student [name=jay, age=18]
Normalclass	
                20190201------Student [name=bobi, age=21]
                20190202------Student [name=luojie, age=22]
```

这里只给出Student类的字段，利用系统生成重写toString(),equals(),HashCode()和生成Getter及Setter即可完成JavaBean的标准书写，用于封装学生类数据。

```
public class Student {
	private String name; //姓名
	private Integer age; //年龄
}
```

然后编写添加学生数据的方法，返回双重HashMap对象seniorSchool抽象表示高中

```java
public HashMap<String,HashMap<String,Student>> addStudent() {
			HashMap<String,HashMap<String,Student>> seniorSchool = 
            new HashMap<String,HashMap<String,Student>>();//编写双重HashMap表示高中
			
    		//创建一个HashMap对象advancedClass表示重点班，往里加数据
			HashMap<String, Student> advancedClass = new HashMap<String,Student>();
			advancedClass.put("20190101", new Student("jay",18));  
			advancedClass.put("20190102", new Student("rose",16)); 
			seniorSchool.put("Advancedclass", advancedClass);
			//创建一个HashMap对象normalClass表示普通班，往里加数据
			HashMap<String, Student> normalClass = new HashMap<String,Student>();
			normalClass.put("20190201", new Student("bobi",21));  
			normalClass.put("20190202", new Student("luojie",22)); 
			seniorSchool.put("Normalclass",normalClass);
			
			return seniorSchool;
	}
```

编写遍历高中每一个班级的每一个学生并打印输出的方法

```java
public void getStudent() {
		HashMap<String, HashMap<String, Student>> senior = addStudent();
		//遍历外层的HashMap,调用外层HashMap对象.keySet()返回所有班级名称的Key存放在Set集合中
    	//Set<String> classNames = senior.keySet();
		for (String className : senior.keySet()) {
			System.out.println(className+"\t");
            //通过每一个key值，调用外层HashMap对象.get(key)去获取所有value（即班级类型）
			HashMap<String, Student> classMaps = senior.get(className);
			//Set<String> studentIds = classMaps.keySet();
			for(String studentId:classMaps.keySet()) {
                //内层也是调用KeySet()方法返回Set视图,然后通过每一个Key找到对应Value值
				System.out.println("\t\t"+studentId+"------"+classMaps.get(studentId));
			}
		}
	}
```

编写测试方法来测试打印输出

```java
	@Test
	public void testGetStudent() {
		getStudent();
	}
```

```
Advancedclass	
				20190102------Student [name=rose, age=16]
				20190101------Student [name=jay, age=18]
Normalclass	
                20190201------Student [name=bobi, age=21]
                20190202------Student [name=luojie, age=22]
```

方式二：通过 Set视图的映射包含在这个Map来遍历即调用 API文档中Set<Map.Entry<K,V>> entrySet() ，这里 Set视图的映射指的是Entry类，它是一个Map内部类，保存了键值对的地址，所以称为映射。

涉及到的方法在Java官方文档Map.Entry<K,V>接口中可以找到下面的两个方法

| `K`  | `getKey()`  返回对应于此项的键。   |
| ---- | ---------------------------------- |
| `V`  | `getValue()`  返回对应于此项的值。 |

这种方式获取的思路如下：

1.通过调用HashMap对象的entrySet()方法返回所有键值对的地址值，存放在一个Set集合中

2.通过地址值获取对应的key和value（调用上面的`getKey()`和`getValue()`）

实现的图解实例：

一开始只能获取到Set的Entry类中的地址值，其他的均获取不到

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map_traverse_02.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568740447;1631812447&q-key-time=1568740447;1631812447&q-header-list=&q-url-param-list=&q-signature=d06f2f63250ceb8989c6a60b7582dc7b0610e676)

接下来通过每一个地址值0xxxx去找到对应的键值对对象，通过它调用`getKey()`，`getValue()`

获取key和value。

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map_traverse_02_1.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568740894;1631812894&q-key-time=1568740894;1631812894&q-header-list=&q-url-param-list=&q-signature=091980c51524e498386f930eb3f6b200abc11a94)

调用`getKey()`,`getValue()`逐个获取key和value，一直获取到最后

![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map_traverse_02_2.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568740934;1631812934&q-key-time=1568740934;1631812934&q-header-list=&q-url-param-list=&q-signature=67f4577786dbb30b30a9025d4a7f8399c3f9420c)

这里还是以上面的高中划分重点班和普通班的场景为例子，使用方式二遍历输出

```java
public void getStudentForEntry() {
		HashMap<String, HashMap<String, Student>> senior = addStudent();
		//调用外层HashMap的entrySet()获取到外层所有班级的entries
		Set<Map.Entry<String, HashMap<String, Student>>> classentries=senior.entrySet();
		for(Map.Entry<String, HashMap<String, Student>> classentry:classentries) {
			
			System.out.println(classentry.getKey());
			//循环遍历每个classentry获取到每个学生类的HashMap集合
			HashMap<String, Student> studentMap = classentry.getValue();
			//学生类HashMap对象调用entrySet(),获取到所有学生类的实体studentEntries
			Set<Map.Entry<String, Student>> studentEntries= studentMap.entrySet();
			for(Map.Entry<String, Student> studentEntry:studentEntries) {
			//循环遍历每个学生类实体studentEntry，通过它来调用getKey()和getValue()获取键和值
		System.out.println("\t\t"+studentEntry.getKey()+"---"+studentEntry.getValue()); 
			}
		}
	}
```
输出结果如下：
![](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/Map_traverse_result.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1568768240;1631840240&q-key-time=1568768240;1631840240&q-header-list=&q-url-param-list=&q-signature=d77b44ddd8742af152f20907e9dbf701a3f03b0f)

### 遍历总结

HashMap的嵌套遍历需要我们学会分步拆解，从整体出发，屏蔽干扰，例如

`HashMap<String,HashMap<String,Student>> senior  `

可以先把内层的`HashMap<String,Student>`看作是一个value,从而思想上简化成一个HashMap变成如下代码

外层HashMap经过简化后变成`HashMap<String,value> senior  `，变成熟悉HashMap<key,value>形式，只不过key的类型是String,value是HashMap<String,Student>，这里我们简化后手写代码就会屏蔽HashMap泛型的干扰

```java
1.Set<String> seniorKeys = senior.keySet();
//seniorKeys表示所有key值组成String类型的Set集合的对象 senior表示HashMap对象地址的引用

2.for(String seniorKey:seniorKeys){
    System.out.println(seniorKey);//此处依次输出的是Advancedclass和Normalclass
	HashMap<String,Student> classvalue = senior.get(seniorKey);
	//这里返回的每一个值都是外层value的值,用HashMap<String,Student>替换
	//seniorKeys去掉后，理解为每一个key，通过调用HashMap对象.get(key)获取键值对映射中key所对应value
}
```

简化后外层HashMap后，得到内层HashMap相对而言就简单很多，变成以下代码块

`HashMap<String,Student> classvalue = senior.get(seniorKey)`,这里的Key类型是String,value的类型是Studen类型，内层HashMap的对象是classvalue,按照`HashMap<key,value> `遍历即可。

```
Set<String> studentkeys = classvalue.keySet();
for(String studentkey:studentkeys){
	System.out.println(studentkey);//这里输出的是学号，学号是存放在内层HashMap的Key里
	Student student = classvalue.get(studentkey);
	System.out.println(student);//这里输出的是具体学生姓名和年龄信息，因为重写ToString()
	//所以表现为Student [name=xxxx, age=16]形式
}
```

上述的是简化思想的代码，熟练后可以把`Set<String> studentkeys = classvalue.keySet()`以及`Student student = classvalue.get(studentkey)`代码优化，使代码更加简洁。

```java
for(String seniorKey:senior.keySet()){
    System.out.println(seniorKey);
	HashMap<String,Student> classvalue = senior.get(seniorKey);
    for(String studentkey:classvalue.keySet()){
        System.out.println("\t\t"+studentkey+"---"+classvalue.get(studentkey));
        // \t表示制表符，使输出更好看一点而已
    }
}
```

方式二的总结这里就不总结了，有些东西总是需要自己提炼才会记忆深刻，拿来主义看的懂固然很好，但相信来得快忘记得也挺快，还需自己一步一步地学习，才能有所提高。

### 最后的最后

最后的最后，写这篇博文时候，其实是为了提高自己的编程思想，两层的HashMap嵌套其实是个中间点，经过思想上的简化，可以简化成单层HashMap，由此对于三层及以上的HashMap嵌套，采用简化思想也会迎刃而解，只不过是时间和熟练的问题，而对于其他List嵌套HashMap的`List<HashMap<String,Integer>>`那么就更加容易解决，因为HashMap嵌套是双列集合嵌套双列集合，对于一个单列嵌套双列，是不是简化了一个维度？