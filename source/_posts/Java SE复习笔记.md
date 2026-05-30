---
title: Java SE复习笔记
date: 2025-07-04 16:54:32
tags:
  - Java
categories:
  - 实习
---

> 第一次找实习wwwwwwww好紧张希望能顺利。

# Java SE复习笔记

## JVM

Java虚拟机（Java Virtual Machine，简称JVM）是运行所有Java程序的抽象计算机，是Java语言的运行环境。

Java语言的一个非常重要的特点就是**与平台的无关性**（一次编译、处处运行）。而使用Java虚拟机是实现这一特点的关键。一般的高级语言如果要在不同的平台上运行，至少需要编译成不同的目标代码。而引入Java语言虚拟机后，Java语言在不同平台上运行时不需要重新编译。Java语言使用模式Java虚拟机屏蔽了与具体平台相关的信息，使得Java语言编译程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。

JVM中提供的JIT（即时编译方式）将字节码直接转化成高性能的本地机器码，即JIT使得Java程序既能跨平台又能高速运行。

## Java集合

![img](https://cdn.jsdelivr.net/gh/bush-cn/TyporaImages@main/img/2025/07/04/20250704183013.gif)

### ArrayList

Java ArrayList 常用方法列表如下：

| 方法                                                         | 描述                                          |
| :----------------------------------------------------------- | :-------------------------------------------- |
| [add()](https://www.runoob.com/java/java-arraylist-add.html) | 将元素插入到指定位置的 arraylist 中           |
| [addAll()](https://www.runoob.com/java/java-arraylist-addall.html) | 添加集合中的所有元素到 arraylist 中           |
| [clear()](https://www.runoob.com/java/java-arraylist-clear.html) | 删除 arraylist 中的所有元素                   |
| [clone()](https://www.runoob.com/java/java-arraylist-clone.html) | 复制一份 arraylist                            |
| [contains()](https://www.runoob.com/java/java-arraylist-contains.html) | 判断元素是否在 arraylist                      |
| [get()](https://www.runoob.com/java/java-arraylist-get.html) | 通过索引值获取 arraylist 中的元素             |
| [indexOf()](https://www.runoob.com/java/java-arraylist-indexof.html) | 返回 arraylist 中元素的索引值                 |
| [removeAll()](https://www.runoob.com/java/java-arraylist-removeall.html) | 删除存在于指定集合中的 arraylist 里的所有元素 |
| [remove()](https://www.runoob.com/java/java-arraylist-remove.html) | 删除 arraylist 里的单个元素                   |
| [size()](https://www.runoob.com/java/java-arraylist-size.html) | 返回 arraylist 里元素数量                     |
| [isEmpty()](https://www.runoob.com/java/java-arraylist-isempty.html) | 判断 arraylist 是否为空                       |
| [subList()](https://www.runoob.com/java/java-arraylist-sublist.html) | 截取部分 arraylist 的元素                     |
| [set()](https://www.runoob.com/java/java-arraylist-set.html) | 替换 arraylist 中指定索引的元素               |
| [sort()](https://www.runoob.com/java/java-arraylist-sort.html) | 对 arraylist 元素进行排序                     |
| [toArray()](https://www.runoob.com/java/java-arraylist-toarray.html) | 将 arraylist 转换为数组                       |
| [toString()](https://www.runoob.com/java/java-arraylist-tostring.html) | 将 arraylist 转换为字符串                     |
| [ensureCapacity](https://www.runoob.com/java/java-arraylist-surecapacity.html)() | 设置指定容量大小的 arraylist                  |
| [lastIndexOf()](https://www.runoob.com/java/java-arraylist-lastindexof.html) | 返回指定元素在 arraylist 中最后一次出现的位置 |
| [retainAll()](https://www.runoob.com/java/java-arraylist-retainall.html) | 保留 arraylist 中在指定集合中也存在的那些元素 |
| [containsAll()](https://www.runoob.com/java/java-arraylist-containsall.html) | 查看 arraylist 是否包含指定集合中的所有元素   |
| [trimToSize()](https://www.runoob.com/java/java-arraylist-trimtosize.html) | 将 arraylist 中的容量调整为数组中的元素个数   |
| [removeRange()](https://www.runoob.com/java/java-arraylist-removerange.html) | 删除 arraylist 中指定索引之间存在的元素       |
| [replaceAll()](https://www.runoob.com/java/java-arraylist-replaceall.html) | 将给定的操作内容替换掉数组中每一个元素        |
| [removeIf()](https://www.runoob.com/java/java-arraylist-removeif.html) | 删除所有满足特定条件的 arraylist 元素         |
| [forEach()](https://www.runoob.com/java/java-arraylist-foreach.html) | 遍历 arraylist 中每一个元素并执行特定操作     |

### Linked List

**以下情况使用 ArrayList :**

- 频繁访问列表中的某一个元素。
- 只需要在列表末尾进行添加和删除元素操作。

**以下情况使用 LinkedList :**

- 你需要通过循环迭代来访问列表中的某些元素。
- 需要频繁的在列表开头、中间、末尾等位置进行添加和删除元素操作。

> LinkedList 实现了 Queue 接口、Deque 接口，可作为队列、双向队列使用。

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [public boolean add(E e)](https://www.runoob.com/java/java-linkedlist-add.html) | 链表末尾添加元素，返回是否成功，成功为 true，失败为 false。  |
| [public void add(int index, E element)](https://www.runoob.com/java/java-linkedlist-addall.html) | 向指定位置插入元素。                                         |
| [public boolean addAll(Collection c)](https://www.runoob.com/java/java-linkedlist-addall.html) | 将一个集合的所有元素添加到链表后面，返回是否成功，成功为 true，失败为 false。 |
| [public boolean addAll(int index, Collection c)](https://www.runoob.com/java/java-linkedlist-addall.html) | 将一个集合的所有元素添加到链表的指定位置后面，返回是否成功，成功为 true，失败为 false。 |
| [public void addFirst(E e)](https://www.runoob.com/java/java-linkedlist-addfirst.html) | 元素添加到头部。                                             |
| [public void addLast(E e)](https://www.runoob.com/java/java-linkedlist-addlast.html) | 元素添加到尾部。                                             |
| [public boolean offer(E e)](https://www.runoob.com/java/java-linkedlist-offer.html) | 向链表末尾添加元素，返回是否成功，成功为 true，失败为 false。 |
| [public boolean offerFirst(E e)](https://www.runoob.com/java/java-linkedlist-offerfirst.html) | 头部插入元素，返回是否成功，成功为 true，失败为 false。      |
| [public boolean offerLast(E e)](https://www.runoob.com/java/java-linkedlist-offerlast.html) | 尾部插入元素，返回是否成功，成功为 true，失败为 false。      |
| [public void clear()](https://www.runoob.com/java/java-linkedlist-clear.html) | 清空链表。                                                   |
| [public E removeFirst()](https://www.runoob.com/java/java-linkedlist-removefirst.html) | 删除并返回第一个元素。                                       |
| [public E removeLast()](https://www.runoob.com/java/java-linkedlist-removelast.html) | 删除并返回最后一个元素。                                     |
| [public boolean remove(Object o)](https://www.runoob.com/java/java-linkedlist-remove.html) | 删除某一元素，返回是否成功，成功为 true，失败为 false。      |
| [public E remove(int index)](https://www.runoob.com/java/java-linkedlist-remove.html) | 删除指定位置的元素。                                         |
| [public E poll()](https://www.runoob.com/java/java-linkedlist-poll.html) | 删除并返回第一个元素。                                       |
| [public E remove()](https://www.runoob.com/java/java-linkedlist-remove.html) | 删除并返回第一个元素。                                       |
| [public boolean contains(Object o)<](https://www.runoob.com/java/java-linkedlist-contains.html)/td> | 判断是否含有某一元素。                                       |
| [public E get(int index)](https://www.runoob.com/java/java-linkedlist-get.html) | 返回指定位置的元素。                                         |
| [public E getFirst()](https://www.runoob.com/java/java-linkedlist-getfirst.html) | 返回第一个元素。                                             |
| [public E getLast()](https://www.runoob.com/java/java-linkedlist-getlast.html) | 返回最后一个元素。                                           |
| [public int indexOf(Object o)](https://www.runoob.com/java/java-linkedlist-indexof.html) | 查找指定元素从前往后第一次出现的索引。                       |
| [public int lastIndexOf(Object o)](https://www.runoob.com/java/java-linkedlist-lastindexof.html) | 查找指定元素最后一次出现的索引。                             |
| [public E peek()](https://www.runoob.com/java/java-linkedlist-peek.html) | 返回第一个元素。                                             |
| [public E element()](https://www.runoob.com/java/java-linkedlist-element.html) | 返回第一个元素。                                             |
| [public E peekFirst()](https://www.runoob.com/java/java-linkedlist-peekfirst.html) | 返回头部元素。                                               |
| [public E peekLast()](https://www.runoob.com/java/java-linkedlist-peeklast.html) | 返回尾部元素。                                               |
| [public E set(int index, E element)](https://www.runoob.com/java/java-linkedlist-set.html) | 设置指定位置的元素。                                         |
| [public Object clone()](https://www.runoob.com/java/java-linkedlist-clone.html) | 克隆该列表。                                                 |
| [public Iterator descendingIterator()](https://www.runoob.com/java/java-linkedlist-descendingiterator.html) | 返回倒序迭代器。                                             |
| [public int size()](https://www.runoob.com/java/java-linkedlist-size.html) | 返回链表元素个数。                                           |
| [public ListIterator listIterator(int index)](https://www.runoob.com/java/java-linkedlist-listiterator.html) | 返回从指定位置开始到末尾的迭代器。                           |
| [public Object[\] toArray()](https://www.runoob.com/java/java-linkedlist-toarray.html) | 返回一个由链表元素组成的数组。                               |
| [public T[\] toArray(T[] a)](https://www.runoob.com/java/java-linkedlist-toarray.html) | 返回一个由链表元素转换类型而成的数组。                       |

### HashSet

| **方法**                            | **返回值**    | **说明**                                                | **示例**                                     |
| :---------------------------------- | :------------ | :------------------------------------------------------ | :------------------------------------------- |
| `add(E e)`                          | `boolean`     | 添加元素到集合，成功返回 `true`，重复元素返回 `false`。 | `set.add("Java");`                           |
| `remove(Object o)`                  | `boolean`     | 删除指定元素，成功返回 `true`，元素不存在返回 `false`。 | `set.remove("Python");`                      |
| `contains(Object o)`                | `boolean`     | 检查集合是否包含指定元素。                              | `if (set.contains("Java")) { ... }`          |
| `size()`                            | `int`         | 返回集合中的元素数量。                                  | `int count = set.size();`                    |
| `isEmpty()`                         | `boolean`     | 判断集合是否为空。                                      | `if (set.isEmpty()) { ... }`                 |
| `clear()`                           | `void`        | 清空集合中的所有元素。                                  | `set.clear();`                               |
| `iterator()`                        | `Iterator<E>` | 返回集合的迭代器，用于遍历元素。                        | `for (String s : set) { ... }`               |
| `toArray()`                         | `Object[]`    | 将集合转换为数组。                                      | `Object[] arr = set.toArray();`              |
| `toArray(T[] a)`                    | `T[]`         | 将集合转换为指定类型的数组。                            | `String[] arr = set.toArray(new String[0]);` |
| `addAll(Collection<? extends E> c)` | `boolean`     | 添加另一个集合的所有元素（并集操作）。                  | `set.addAll(Arrays.asList("A", "B"));`       |
| `retainAll(Collection<?> c)`        | `boolean`     | 仅保留与指定集合共有的元素（交集操作）。                | `set.retainAll(otherSet);`                   |
| `removeAll(Collection<?> c)`        | `boolean`     | 删除与指定集合共有的元素（差集操作）。                  | `set.removeAll(otherSet);`                   |

### HashMap

Java HashMap 常用方法列表如下：

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [clear()](https://www.runoob.com/java/java-hashmap-clear.html) | 删除 hashMap 中的所有键/值对                                 |
| [clone()](https://www.runoob.com/java/java-hashmap-clone.html) | 复制一份 hashMap                                             |
| [isEmpty()](https://www.runoob.com/java/java-hashmap-isempty.html) | 判断 hashMap 是否为空                                        |
| [size()](https://www.runoob.com/java/java-hashmap-size.html) | 计算 hashMap 中键/值对的数量                                 |
| [put()](https://www.runoob.com/java/java-hashmap-put.html)   | 将键/值对添加到 hashMap 中                                   |
| [putAll()](https://www.runoob.com/java/java-hashmap-putall.html) | 将所有键/值对添加到 hashMap 中                               |
| [putIfAbsent()](https://www.runoob.com/java/java-hashmap-putifabsent.html) | 如果 hashMap 中不存在指定的键，则将指定的键/值对插入到 hashMap 中。 |
| [remove()](https://www.runoob.com/java/java-hashmap-remove.html) | 删除 hashMap 中指定键 key 的映射关系                         |
| [containsKey()](https://www.runoob.com/java/java-hashmap-containskey.html) | 检查 hashMap 中是否存在指定的 key 对应的映射关系。           |
| [containsValue()](https://www.runoob.com/java/java-hashmap-containsvalue.html) | 检查 hashMap 中是否存在指定的 value 对应的映射关系。         |
| [replace()](https://www.runoob.com/java/java-hashmap-replace.html) | 替换 hashMap 中是指定的 key 对应的 value。                   |
| [replaceAll()](https://www.runoob.com/java/java-hashmap-replaceall.html) | 将 hashMap 中的所有映射关系替换成给定的函数所执行的结果。    |
| [get()](https://www.runoob.com/java/java-hashmap-get.html)   | 获取指定 key 对应对 value                                    |
| [getOrDefault()](https://www.runoob.com/java/java-hashmap-getordefault.html) | 获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值 |
| [forEach()](https://www.runoob.com/java/java-hashmap-foreach.html) | 对 hashMap 中的每个映射执行指定的操作。                      |
| [entrySet()](https://www.runoob.com/java/java-hashmap-entryset.html) | 返回 hashMap 中所有映射项的集合集合视图。                    |
| [keySet](https://www.runoob.com/java/java-hashmap-keyset.html)() | 返回 hashMap 中所有 key 组成的集合视图。                     |
| [values()](https://www.runoob.com/java/java-hashmap-values.html) | 返回 hashMap 中存在的所有 value 值。                         |
| [merge()](https://www.runoob.com/java/java-hashmap-merge.html) | 添加键值对到 hashMap 中                                      |
| [compute()](https://www.runoob.com/java/java-hashmap-compute.html) | 对 hashMap 中指定 key 的值进行重新计算                       |
| [computeIfAbsent()](https://www.runoob.com/java/java-hashmap-computeifabsent.html) | 对 hashMap 中指定 key 的值进行重新计算，如果不存在这个 key，则添加到 hashMap 中 |
| [computeIfPresent()](https://www.runoob.com/java/java-hashmap-computeifpresent.html) | 对 hashMap 中指定 key 的值进行重新计算，前提是该 key 存在于 hashMap 中。 |

## Java I/O

![img](https://cdn.jsdelivr.net/gh/bush-cn/TyporaImages@main/img/2025/07/05/20250705095318.png)

### 字节流（处理二进制数据）

#### InputStream

`InputStream`用于从源头（通常是文件）读取数据（字节信息）到内存中，`java.io.InputStream`抽象类是所有字节输入流的父类。

| 方法                               | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| `read()`                           | 返回输入流中下一个字节的数据。返回的值介于 0 到 255 之间。如果未读取任何字节，则代码返回 `-1` ，表示文件结束。 |
| `read(byte b[ ])`                  | 从输入流中读取一些字节存储到数组 `b` 中，返回读取的字节数。如果没有可用字节读取，返回 `-1`。 |
| `read(byte b[], int off, int len)` | 在`read(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。 |
| `skip(long n)`                     | 忽略输入流中的 n 个字节 ,返回实际忽略的字节数。              |
| `available()`                      | 返回输入流中可以读取的字节数。                               |
| `close()`                          | 关闭输入流释放相关的系统资源。                               |

从 Java 9 开始，`InputStream` 新增加了多个实用的方法：

- `readAllBytes()`：读取输入流中的所有字节，返回字节数组。
- `readNBytes(byte[] b, int off, int len)`：阻塞直到读取 `len` 个字节。
- `transferTo(OutputStream out)`：将所有字节从一个输入流传递到一个输出流。

---

`FileInputStream`配合`BufferedInputStream`实现读取String对象：

```java
// 新建一个 BufferedInputStream 对象
BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("input.txt"));
// 读取文件的内容并复制到 String 对象中
String result = new String(bufferedInputStream.readAllBytes());
System.out.println(result);
```

`DataInputStream`可读取多种类型的值、`ObjectInputStream`可以反序列化对象，使用方法都与上面类似，构造函数使用一个`FileInputStream`作为参数。

#### OutputStream

`OutputStream`用于将数据（字节信息）写入到目的地（通常是文件），`java.io.OutputStream`抽象类是所有字节输出流的父类。

| 方法                                | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| `write(int b)`                      | 将特定字节写入输出流。                                       |
| `write(byte b[ ])`                  | 将数组`b` 写入到输出流，等价于 `write(b, 0, b.length)` 。    |
| `write(byte[] b, int off, int len)` | 在`write(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。 |
| `flush()`                           | 刷新此输出流并强制写出所有缓冲的输出字节。                   |
| `close()`                           | 闭输出流释放相关的系统资源。                                 |

### 字符流（处理文本数据）

#### Reader

`Reader`用于从源头（通常是文件）读取数据（字符信息）到内存中，`java.io.Reader`抽象类是所有字符输入流的父类。

方法与`InputStream`类似，但是类型由`byte`改为`char`。

#### Writer

同理。但是除可写入`char[]`类型外还可写入`String`类。

### I/O模型

- BIO：**同步阻塞I/O**
- NIO（Non-Blocking I/O、New I/O）：**同步非阻塞 I/O**、**I/O 多路复用**
- AIO（Asynchronous I/O）：**信号驱动 I/O** 、**异步 I/O**

## 并发编程

### 进程和线程

- **进程**：进程是一个正在执行的程序实例。每个进程都有独立的内存空间、系统资源分配（如文件描述符等）以及一个或多个线程。
- **线程**：线程是进程内的基本执行单位，是操作系统进行运行操作的最小单元。一个进程内的所有线程共享该进程的资源，包括内存空间和文件描述符等。

> 共享进程的**堆**和**方法区**资源，但每个线程有自己的**程序计数器**、**虚拟机栈**和**本地方法栈**

#### 线程的生命周期和状态

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态：

- NEW: 初始状态，线程被创建出来但没有被调用 `start()` 。
- RUNNABLE: 运行状态，线程被调用了 `start()`等待运行的状态。
- BLOCKED：阻塞状态，需要等待锁释放。
- WAITING：等待状态，表示该线程需要等待其他线程做出一些特定动作（通知或中断）。
- TIME_WAITING：超时等待状态，可以在指定的时间后自行返回而不是像 WAITING 那样一直等待。
- TERMINATED：终止状态，表示该线程已经运行完毕。

![Java 线程状态变迁图](https://cdn.jsdelivr.net/gh/bush-cn/TyporaImages@main/img/2025/07/06/20250706105802.png)

### 并发与并行的区别

- **并发**：两个及两个以上的作业在同一 **时间段** 内执行。
- **并行**：两个及两个以上的作业在同一 **时刻** 执行。

最关键的点是：是否是 **同时** 执行。

### 同步和异步的区别

- **同步**：发出一个调用之后，在没有得到结果之前， 该调用就不可以返回，一直等待。
- **异步**：调用在发出之后，不用等待返回结果，该调用直接返回。

### 死锁

死锁的四个必要条件：

1. **互斥条件**：该资源任意一个时刻只由一个线程占用。
2. **请求与保持条件**：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
3. **不剥夺条件**：线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
4. **循环等待条件**：若干线程之间形成一种头尾相接的循环等待资源关系。

---

> 参考：https://javaguide.cn/
