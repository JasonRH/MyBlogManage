---
title: HashMap和Bundle
categories: 前端
tags:
  - HashMap
  - Bundle
abbrlink: 25454
date: 2018-12-01 15:03:44
---

## HashMap(JAVA)

HashMap 是一个散列表，它存储的内容是键值对(key-value)映射。

HashMap 继承于AbstractMap，实现了Map、Cloneable、java.io.Serializable接口。

HashMap 的实现不是同步的，这意味着它不是线程安全的。它的key、value都可以为null。此外，HashMap中的映射不是有序的。

HashMap主要是用Entry数组来存储数据的，我们都知道它会对key进行哈希运算，哈系运算会有重复的哈希值，对于哈希值的冲突，HashMap采用链表来解决的。

HashMap 的实例有两个参数影响其性能：“初始容量” 和 “加载因子”。容量 是哈希表中桶的数量，初始容量 只是哈希表在创建时的容量。加载因子 是哈希表在其容量自动增加之前可以达到多满的一种尺度。当哈希表中的条目数超出了加载因子与当前容量的乘积时，则要对该哈希表进行 rehash 操作（即重建内部数据结构），从而哈希表将具有大约两倍的桶数。

通常，默认加载因子是 0.75, 这是在时间和空间成本上寻求一种折衷。加载因子过高虽然减少了空间开销，但同时也增加了查询成本（在大多数 HashMap 类的操作中，包括 get 和 put 操作，都反映了这一点）。在设置初始容量时应该考虑到映射中所需的条目数及其加载因子，以便最大限度地减少 rehash 操作次数。如果初始容量大于最大条目数除以加载因子，则不会发生 rehash 操作。

**HashMap的API**

|                  |                                           |
| ---------------- | ----------------------------------------- |
| void             | clear()                                   |
| Object           | clone()                                   |
| boolean          | containsKey(Object key)                   |
| boolean          | containsValue(Object value)               |
| Set<Entry<K, V>> | entrySet()                                |
| V                | get(Object key)                           |
| boolean          | isEmpty()                                 |
| Set<K>           | keySet()                                  |
| V                | put(K key, V value)                       |
| void             | putAll(Map<? extends K, ? extends V> map) |
| V                | remove(Object key)                        |
| int              | size()                                    |
| Collection<V>    | values()                                  |

```java
private static void testHashMapAPIs() {
    // 初始化随机种子
    Random r = new Random();
    // 新建HashMap
    HashMap map = new HashMap();
    // 添加操作
    map.put("one", r.nextInt(10));
    map.put("two", r.nextInt(10));
    map.put("three", r.nextInt(10));
    // 打印出map
    System.out.println("map:"+map );
    // 通过Iterator遍历key-value
    Iterator iter = map.entrySet().iterator();
    while(iter.hasNext()) {
        Map.Entry entry = (Map.Entry)iter.next();
        System.out.println("next : "+ entry.getKey() +" - "+entry.getValue());
    }
    // HashMap的键值对个数        
    System.out.println("size:"+map.size());
    // containsKey(Object key) :是否包含键key
    System.out.println("contains key two : "+map.containsKey("two"));
    System.out.println("contains key five : "+map.containsKey("five"));
    // containsValue(Object value) :是否包含值value
    System.out.println("contains value 0 : "+map.containsValue(new Integer(0)));
    // remove(Object key) ： 删除键key对应的键值对
    map.remove("three");
    System.out.println("map:"+map );
    // clear() ： 清空HashMap
    map.clear();
    // isEmpty() : HashMap是否为空
    System.out.println((map.isEmpty()?"map is empty":"map is not empty") );
}

```

---

## Bundle（Android）

一种存放字符串和Parcelable类型数据的map类型的容器类，通过存放数据键（key）获取对应的各种类型的值（value），而且必须通过键（key）获取。

**代码示例**

```java
//TestBundle.java
Bundle bundle = new Bundle();//创建一个句柄
bundle.putString("name", nameinfo);//将nameinfo填充入句柄
Intent mIntent = new Intent(TestBundle.this,TestBundle_getvalue.class);
mIntent.putExtras(bundle);
startActivity(mIntent);

//TestBundle_getvalue.java
Bundle bundle = getIntent().getExtras();//获取一个句柄
String nameString=bundle.get("name");//通过key为“name”来获取value即 nameString.

```

---

## Android为什么要设计出Bundle而不是直接使用HashMap来进行数据传递？

1. Bundle内部是由ArrayMap实现的，ArrayMap的内部实现是两个数组，一个int数组是存储对象数据对应下标，一个对象数组保存key和value，内部使用二分法对key进行排序，所以在添加、删除、查找数据的时候，都会使用二分法查找，只适合于小数据量操作，如果在数据量比较大的情况下，那么它的性能将退化。而HashMap内部则是数组+链表结构，所以在数据量较少的时候，HashMap的Entry Array比ArrayMap占用更多的内存。因为使用Bundle的场景大多数为小数据量，我没见过在两个Activity之间传递10个以上数据的场景，所以相比之下，在这种情况下使用ArrayMap保存数据，在操作速度和内存占用上都具有优势，因此使用Bundle来传递数据，可以保证更快的速度和更少的内存占用。
2. 另外一个原因，则是在Android中如果使用Intent来携带数据的话，需要数据是基本类型或者是可序列化类型，HashMap使用Serializable进行序列化，而Bundle则是使用Parcelable进行序列化。而在Android平台中，更推荐使用Parcelable实现序列化，虽然写法复杂，但是开销更小，所以为了更加快速的进行数据的序列化和反序列化，系统封装了Bundle类，方便我们进行数据的传输。
