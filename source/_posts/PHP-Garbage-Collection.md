---
title: PHP Garbage Collection
categories: program
tags: php
comments: true
date: 2018-12-27
---
Explains Garbage Collection (also known as GC) of PHP

### Related concepts

- Garbage Collection

- zval  ：所有的变量都是用一个结构 zval 结构来保存的

  - value : 值，是真正保存数据的关键部分，定义为一个联合体(union)
  - type : 储存变量的类型 
  - is_ref ：被 & 引用的数量
  - refcount ：引用计数，记录了当前的 zval 被引用的次数（这里的引用并不是真正的 & ，而是有几个变量指向它）

- Copy On Write  : COW

- arithmetic

  PHP5.2 : [Reference Counting](http://php.net/manual/en/features.gc.refcounting-basics.php) : 引用计数，GC根本算法

  PHP5.3 : [Concurrent Cycle Collection in Reference Counted Systems](http://researcher.watson.ibm.com/researcher/files/us-bacon/Bacon01Concurrent.pdf)

- function : [memory_get_usage](http://php.net/manual/en/function.memory-get-usage.php)

- function : xdebug_debug_zval() : need xdebug extension

- function : [debug_zval_dump](http://php.net/manual/en/function.debug-zval-dump.php)

### PHP Language characteristics

PHP 是脚本语言，所谓脚本语言，就是说PHP并不是独立运行的，要运行PHP代码需要PHP解析器，用户编写的PHP代码最终都会被PHP解析器解析执行，PHP的执行是通过 `Zend engine`（ZE, Zend引擎），ZE是用C编写的，用户编写的PHP代码最终都会被翻译成PHP的虚拟机ZE的虚拟指令（`OPCODES`）来执行，也就说最终会被翻译成一条条的指令

### Sketch

- `zval sketch`

  声明一个变量

  ```php
  $addr = 'i value';
  ```

  PHP内部都是使用 zval 来表示变量的，那对于上面的脚本，ZE是如何把 addr 和内部的 zval 结构联系起来的呢？变量都是有名字的（本例中变量名为 addr ），而 zval 中并没有相应的字段来体现变量名。PHP内部有一个机制，来实现变量名到 zval 的映射，在PHP中，所有的变量都会存储在一个 `hash table`中，当你创建一个变量的时候，PHP会为这个变量分配一个 zval，填入相应的信息，然后将这个变量的名字和指向这个 zval 的指针填入一个数组中。当你获取这个变量的时候，PHP会通过查找 hash table，取得对应的 zval

  `注意：数组和对象这类复合类型在生成zval时，会为每个单元生成一个 zval`

  ![clipboard.png](https://segmentfault.com/img/bVsniw)


- `释放内存`

  我们经常说每个变量都有一个内存地址，那这个 zval 和变量的内存地址，这俩有什么关系吗？定义一个变量会开辟一块内存，这块内存好比一个盒子，盒子里放了zval，zval里保存了变量的相关信息，需要开辟多大的内存，是由zval所占空间大小决定的，zval是内存对象，垃圾回收的时候会把zval和内存地址（盒子）分别释放掉

- `refcount  is_ref `

  ```php
  $a = 'string';
  $b = $a;
  unset($a);
  ```

  第一行代码创建变量 a ,申请了 6 字节内存，

  第二行代码定义了变量 b, 将 a 的值赋予 b

  第三行代码释放了变量 a

  如果对于每一个变量重新分配内存，那么变量 a b要申请 12 字节的内存，并且变量 a,还是个无用的数据（被unset了），那么有没有什么方法节省这块资源呢？将变量 a b对应的指针指向同一个 zval 即可

  `refcount`

  refcount 指的是变量被引用的次数（reference count ?）,这里的引用并不是真正的 & ，而是有几个变量指向它

  ```php
  $a = 1;
  $b = $a;
  ```

  第一行，创建了一个变量 a，变量值是 1。 此时保存int 1 的这个 zval 的 refcount 为 1
  第二行，创建了一个新的整形变量（通过赋值的方式），变量也指向刚才创建的 zval，并将这个 zval 的 refcount 加1，此时这个 zval 的 refcount 为2
  所以，这个时候（通过值传递的方式赋值给别的变量），并没有产生新的 zval，两个变量指向同一 zval，通过一个计数器来共用 zval 及内存地址，以达到节省内存空间的目的
  `当一个变量被第一次创建的时候，它对应的 zval 结构的 refcount 的值会被初始化为 1`，因为只有这一个变量在用它。但是当你把这个变量赋值给别的变量时，refcount 属性便会 加1 变成 2，因为现在有两个变量在用这个 zval 结构了

- `debug_zval_dump`

  ```php
  $a = 1;
  debug_zval_dump($a);
  $b = $a;
  debug_zval_dump($a);
  ```

  输出：

  ```php
  long(1) refcount(2)
  long(1) refcount(3)
  ```

  如果你奇怪 ，`var的refcount应该是1`啊？
  我们知道，对于简单变量，PHP是以传值的形式传参数的。也就是说，当执行debug_zval_dump($var)的时候，var会以传值的方式传递给debug_zval_dump，也就是会导致var的refcount加1，所以只要能看到，当变量赋值给一个变量以后，能导致zval的refcount加 1 这个结果即可



### Reference Counting

PHP5.2中使用的内存回收算法是[Reference Counting](http://en.wikipedia.org/wiki/Reference_counting)，中文叫做“引用计数”，其思想非常直观和简洁：为每个内存对象分配一个计数器，当一个内存对象建立时计数器初始化为1（因此此时总是有一个变量引用此对象），以后每有一个新变量引用此内存对象，则计数器加1，而每当减少一个引用此内存对象的变量则计数器减1，当垃圾回收机制运作的时候，将所有计数器为0的内存对象销毁并回收其占用的内存。而PHP中内存对象就是zval，而计数器就是refcount。

Important : [Reference Counting Basics](http://php.net/manual/en/features.gc.refcounting-basics.php)



出现的问题 ： 引用的值为变量自身，内存泄漏 -> [泄露实例](http://php.net/manual/en/features.gc.refcounting-basics.php)

### Concurrent Cycle Collection in Reference Counted Systems

PHP5.3的垃圾回收算法仍然以引用计数为基础，但是不再是使用简单计数作为回收准则，而是使用了一种同步回收算法，这个算法由IBM的工程师在论文[Concurrent Cycle Collection in Reference Counted Systems](http://www.research.ibm.com/people/d/dfb/papers/Bacon01Concurrent.pdf)中提出。 

首先PHP会分配一个固定大小的“根缓冲区”，这个缓冲区用于存放固定数量的zval，这个数量默认是10,000，如果需要修改则需要修改源代码Zend/zend_gc.c中的常量GC_ROOT_BUFFER_MAX_ENTRIES然后重新编译。

由上文我们可以知道，一个zval如果有引用，要么被全局符号表中的符号引用，要么被其它表示复杂类型的zval中的符号引用。因此在zval中存在一些可能根（root）。这里我们暂且不讨论PHP是如何发现这些可能根的，这是个很复杂的问题，总之PHP有办法发现这些可能根zval并将它们投入根缓冲区。

当根缓冲区满额时，PHP就会执行垃圾回收，此回收算法如下：

1. 对每个根缓冲区中的根 zval 按照`深度优先遍历算法`遍历所有能遍历到的 zval ，并将每个 zval 的 refcount 减1，同时为了避免对同一 zval 多次减1（因为可能不同的根能遍历到同一个 zval ），每次对某个zval减1后就对其标记为“已减”。

2. 再次对每个缓冲区中的根 zval 深度优先遍历，如果某个 zval 的 refcount 不为0，则对其加1，否则保持其为0。

3. 清空根缓冲区中的所有根（注意是把这些 zval 从缓冲区中清除而不是销毁它们），然后销毁所有 refcount 为0的zval，并收回其内存。

如果不能完全理解也没有关系，只需记住PHP5.3的垃圾回收算法有以下几点特性：

1. 并不是每次 refcount 减少时都进入回收周期，只有根缓冲区满额后在开始垃圾回收。

2. 可以解决循环引用问题。

3. 可以总将内存泄露保持在一个阈值以下。

### PHP 5.2 与 PHP 5.3 垃圾回收算法的性能比较

[参考 PHP Manual](http://php.net/manual/en/features.gc.performance-considerations.php)

### References

- [PHP Manual GC](http://php.net/manual/en/features.gc.php)

- [zval _ 引用计数 _ 变量分离 _ 写时拷贝](https://segmentfault.com/a/1190000004340427)

- [浅谈PHP5中垃圾回收算法(Garbage Collection)的演化](https://segmentfault.com/a/1190000008481434)

