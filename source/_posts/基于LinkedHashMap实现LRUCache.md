---
title: 基于LinkedHashMap实现LRUCache
date: 2019-12-03 14:56:13
tags: 面试
---



基于LinkedHashMap实现LRUCache    <!-- more --> 

```java
public class LruCache<K, V> extends LinkedHashMap<K, V> {
  public LruCache(int size) {
    // 第三个参数是accessOrder，LinkedHashMap初始化时默认是false.
    super(size, .75f, true);
    this.size = size;
  }

  public void setSize(int size) {
    this.size = size;
  }

  private int size;

  // removeEldestEntry是在上面的方法中afterNodeInsertion()调用的
  @Override
  protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
    return size() > size ? true : false;
  }

  public int getSize() {
    return size;
  }
}

@Test
public void test1(){
   LruCache<Integer, Integer> cache = new LruCache<>(5);
   for (int i = 0; i < 5; i++) {
       cache.put(i, i);
   }
   assertEquals(0, cache.get(0));
   cache.put(5, 5);
   assertNull(cache.get(1));
   assertEquals(5, cache.getSize());
}
```

###### 分析

 test1()方法体中LruCache数据的变化

1、cache.put(i, i);循环体之后cache数据内容，注意数据顺序的变化

> cache:[0,1,2,3,4]

2、assertEquals(0, cache.get(0));

> cache:[1,2,3,4,0]

3、cache.put(5, 5); 因为1最先被插入的，所以被移除掉

> cache:[5,2,3,4,0]

4、assertNull(cache.get(1)); 这里由于元素1已经被移除，所以返回的是null

5、assertEquals(5, cache.getSize()); 这里的缓存内的数据长度仍是5