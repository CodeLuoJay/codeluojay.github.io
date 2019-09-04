---
title: HashMap学习
date: 2019-07-06 12:23:21
tags: HashMap
categories:  Java
---
## 本文思维导图

![HashMap知识点](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/HashMap.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1567431563;1630503563&q-key-time=1567431563;1630503563&q-header-list=&q-url-param-list=&q-signature=ea88575ab261704c0152ca4977ee7e721d2d309c)
<!-- more -->
## HashMap的存储特点
HashMap是Map接口的实现类，所以Map有的特点它也有：
1.HashMap存储的是以键值对形式双列数据，键值对即（key-value）
2.HashMap存储的是无序不可重复的数据
3.HashMap存储的数据可以是null值，key和value都可以是null值
4.HashMap的不可重复特指Key不可重复,value可以是重复的。

## HashMap如何保证添加的数据不重复
HashMap底层数据结构是哈希表，在jdk1.7版本表现形式是数组+链表，jdk1.8中是数组+链表+红黑树，当需要往哈希表中添加数据时需要通过hash值和Equals方法去判断数据在哈希表中是否存在相同的数据。

![hashMap数据结构](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/hash_data_structure.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1567438101;1630510101&q-key-time=1567438101;1630510101&q-header-list=&q-url-param-list=&q-signature=1437633863269cb1def3067fe2431071391b9a6f)

实现过程：
1.首先第一次往哈希表添加数据时，会计算数据的key的hash值，从而计算出存放在数组索引位置，因为第一次添加，哈希表里面为空不涉及到比较hash值和equals方法，所以直接添加。
2.第二次添加数据时，也会计算数据的key的hash值，从而计算出存放在数组索引位置，当第二次添加时，算出的索引位置后会在数组中的索引位置查看是否有元素，没有也则直接添加。
3.当多次添加后，默认为16长度的大小数组大部分索引位置都有元素时，再次添加数据进来，计算出索引位置相同时，这时就要判断数据是否重复。

	当索引位置相同时，这又称为哈希碰撞。通过比较键值对中key值的hash值是否和数组中已有的键值对的key的hash值相同。
		如果不相同的hash值，则在相同的索引位置以链表的形式把数据添加进去
		如果相同hash值，则比较通过equals方法比较两个key值是否相同
			如果不相同，则也在相同的索引位置以链表的形式把数据添加进去
			如果相同，则不添加数据。
```java
if (p.hash == hash &&
((k = p.key) == key || (key != null && key.equals(k))))
e = p;
//这里的代码对应上述的第三点
```
总结：

|           | 比较索引值 | 比较hash值 | 比较equals方法 | 情况描述                         |
| --------- | ----- | ------- | ---------- | ---------------------------- |
| 首次调用put() | 不需要   | 不需要     | 不需要        | 直接添加                         |
| 发生哈希碰撞    | 需要    | 需要      | 需要         | hash值同equals值为false,替换value值 |
| 发生哈希碰撞    | 需要    | 需要      | 需要         | hash值不同equals值为false,链表式添加   |
| 发生哈希碰撞    | 需要    | 需要      | 需要         | hash值不同equals值为true,不添加      |

## hash值如何计算
由上面的的保证HashMap数据不重复就可以看出hash值的重要性，那么hash值到低是什么东西？
Java中有一种哈希表数据结构,它通过hash算法算出的结果就是hash值,这个算法叫hash算法.
hash值怎么算?先了解hashCode方法,它是Object类中的一个用于计算哈希码的值即hash值的方法
这里我自定义一个JavaBean的Student类
```java
   private String name;
   private Integer age;
       @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + (age != null ? age.hashCode() : 0);
        return result;
    }
```
通过分析生成hashCode()，可以得知，hashCode涉及的计算算法与成员变量（name,age）有关
如果成员变量是基本数据类型的值， 那么用这个值 直接参与计算； 
如果成员变量是引用数据类型的值，那么获取到这个成员变量的哈希码值后，再参数计算；

再通过查看String类重写的hashCode()方法
```java
  public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;
            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
```
再写一个再main方法中的小测试：输出结果值为：3029737
```java
 String book = new String("book");
 System.out.println(book.hashCode());
```
下面就来讲解这个值是怎么求出来的
book这四个字符在ASCII码表中的值分别是98，111，111，107
```java
 char val[] = value;
            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
```
根据上面String重写的hashCode的核心代码：
31*98+98+31*111+111+31*111+111+31*107+107=3029737

所以对象的哈希码就是通过这样算出来的

那么这里有个关键问题：为什么要乘以31？
1. 因为31是一个奇质数，奇质数又是什么鬼？其实就是奇数中的质数，质数是与它相乘的数的结果只能被1和本身还有相乘数整除。
   这样选择31作为乘积数的原因是希望能减少哈希码冲突，31是质子数中一个“不大不小”的存在，如果你使用的是一个如2的较小质数，那么得出的乘积会在一个很小的范围，很容易造成哈希值的冲突
2. 31可以被JVM优化

JVM里最有效的计算方式就是进行位运算了
左移运算 << : 左边的最高位丢弃，右边补全0（把 << 左边的数据*2的移动次幂）。

右移运算 >> : 把>>左边的数据/2的移动次幂。

无符号右移运算>>> : 无论最高位是0还是1，左边补齐0。 　　

31 * i = (i << 5) - i（左边  31*2=62,右边   2*2^5-2=62） 左右两边相等，JVM就可以高效的进行计算

31=  2的五次方-1   =（1<<5）-1  

(1<<5)-1 =	二进制的000001向左移动5位	100000-1=32-1=31

## hash值在HashMap中的应用
hash值主要用于计算键值对存放的索引位置,利用hashCode()同时再结合对应JDK版本的扰动算法就能算出索引位置。这里主要用到三个运算符：与运算& ,异或运算符^,无符号右移位运算符>>>

与运算&: 	0&0 =0 ,0&1=0,1&1=1,1&0=0

异或运算^: 	0&0 =0 ,0&1=1,1&1=0,1&0=1

无符号右移位运算符>>>:	把二进制数向右移动多少位 如（1000 0000>>>7）=0000 0001

### JDK1.8中的哈希表索引值计算

将 键key 转换成 哈希码（hash值）操作 = 使用hashCode() + 1次位运算 + 1次异或运算（2次扰动）
```java
 static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
        //哈希码右位移运算16位再异或运算哈希码
    }
 static int indexFor(int h, int length) {  
          return h & (length-1); 
          // 将对哈希码扰动处理后的结果 与运算(&) （数组长度-1）默认为16 数组长度-1=15=00001111，最终得到存储在数组table的位置（即数组下标、索引）
          }
```
还是以上面的3029737为例 转换为二进制对应为0010 1110 0011 1010 1110 1001

![hash值计算](https://bobi-1258060032.cos.ap-chengdu.myqcloud.com/Java/hashcode_math.png?q-sign-algorithm=sha1&q-ak=AKIDVnsTrvTgFf9G9myYbpmT3OVgeOypNtAE&q-sign-time=1567440718;1630512718&q-key-time=1567440718;1630512718&q-header-list=&q-url-param-list=&q-signature=b13ec9d27e562c8eb8d5f94918e45d96cedd28c0)

所以最终算出哈希表索引 index=0111 转换为10进制为7

### JDK1.7中的哈希表索引值计算
将键key 转换成 哈希码（hash值）操作  = 使用hashCode() + 4次位运算 + 5次异或运算（9次扰动）
```java
static final int hash(int h) {
        h ^= k.hashCode(); 
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
        //
     }
static int indexFor(int h, int length) {  
          return h & (length-1); 
          // 将对哈希码扰动处理后的结果 与运算(&) （数组长度-1）默认为16 数组长度-1=15=00001111，最终得到存储在数组table的位置（即数组下标、索引）
          }
```
只是运算次数改变，本质上的套路都是一样。