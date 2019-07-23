---
title: php-garbage-collection
permalink: php-garbage-collection
categories:
- program
tags: 
- php
comments: true
date: 2018-12-09
updated: 2018-12-09
---

## Explains Garbage Collection (also known as GC) of PHP



### PHP Language

PHP 是脚本语言，所谓脚本语言，就是说PHP并不是独立运行的，要运行PHP代码需要PHP解析器，用户编写的PHP代码最终都会被PHP解析器解析执行，PHP的执行是通过 `Zend engine`（ZE, Zend引擎），ZE是用C编写的，用户编写的PHP代码最终都会被翻译成PHP的虚拟机ZE的虚拟指令（`OPCODES`）来执行，也就说最终会被翻译成一条条的指令



### 概念

- Garbage Collection : GC

- PHP 5.2以前, PHP使用引用计数(Reference counting)来做资源管理，PHP 5.3才引入GC

- zval  ：所有的变量都是用一个结构 zval 结构来保存的

  - value : 值，是真正保存数据的关键部分，定义为一个联合体(union)
  - type : 储存变量的类型 
  - is_ref ：被 & 引用的数量
  - refcount ：引用计数，记录了当前的 zval 被引用的次数（这里的引用并不是真正的 & ，而是有几个变量指向它）

- Copy On Write  

- Change On Write

- 过程

  PHP5.2 : [Reference Counting](http://php.net/manual/en/features.gc.refcounting-basics.php) : 引用计数，GC根本算法

  PHP5.3 : [Concurrent Cycle Collection in Reference Counted Systems](http://researcher.watson.ibm.com/researcher/files/us-bacon/Bacon01Concurrent.pdf)

- function : [memory_get_usage](http://php.net/manual/en/function.memory-get-usage.php)

- function : xdebug_debug_zval() : 需要安装xdebug

- function : [debug_zval_dump](http://php.net/manual/en/function.debug-zval-dump.php)  当不使用xdebug时，可以作为替代xdebug_debug_zval 的方法



### Introdution

- `zval`

  声明一个变量

  ```php
  $addr = 'i value';
  ```

  PHP内部都是使用 zval 来表示变量的，那对于上面的脚本，ZE是如何把 `$addr` 变量 和内部的 zval 结构联系起来的呢？变量都是有名字的（本例中变量名为 `$addr` ），而 zval 中并没有相应的字段来体现变量名。PHP内部有一个机制，来实现变量名到 zval 的映射，在PHP中，所有的变量都会存储在一个 `hash table`中，当你创建一个变量的时候，PHP会为这个变量分配一个 zval，填入相应的信息，然后将这个变量的名字和指向这个 zval 的指针填入一个数组中。当你获取这个变量的时候，PHP会通过查找 hash table，取得对应的 zval

  `注意：数组和对象这类复合类型在生成zval时，会为每个单元生成一个 zval`

  ![](https://image-static.segmentfault.com/101/144/1011448460-569f6db18305e_articlex)


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
  我们知道，对于简单变量，PHP是以传值的形式传参数的。也就是说，当执行debug_zval_dump($var)的时候，var会以传值的方式传递给debug_zval_dump，也就是会导致var的refcount加1，所以只要能看到，当变量赋值给一个变量以后，能导致zval的refcount加 1

  例子：

  ```php
  $a = 1;
  $b = $a;
  $c = $b;
  $d = $a;
  # long(1) refcount(5)
  debug_zval_dump($a);
  ```

- `unset`

  当 unset(var) 的时候，它删除符号表里的var的信息，准备清理它对应的zval及内存空间，这时它发现var对应的zval结构的 refcount 值是 > 1，也就是说，还有另外一个变量在一起用着这个zval，所以unset只需把这个zval的refcount减去1就行了

  例子：

  ```php
  $a = 1;
  $b = $a;
  unset($a);
  
  # long(1) refcount(2)
  debug_zval_dump($b);
  ```

- `Copy On Write`

  写入时复制是指：在 用变量对变量进行赋值时，这些相同值的变量指向同一块内存，只有当这些指向同一块内存的 相同值的变量 中的某一个变量的值 `发生改变`的时候，才需要进行`变量分离`，即：将 值发生改变的变量分离出来

  使用场景：变量的多次赋值；函数的参数传递。

  PHP中，Zend引擎为了区分同一块内存是否被多个变量引用，在zval结构中定义了ref_count和is_ref两个变量。

  **ref_count定义了内存被变量引用的次数，次数为0时销毁**

  **is_ref定义了变量是否被强制引用，被强制引用时，值为1**

  ```php
  $a = 1;
  $b = &$a;
  
  $a 的 is_ref = 1;
  ```

  例子：

  ```php
  $a = 1;
  $b = $a;
  $a = 2;
  
  # long(2) refcount(2)
  debug_zval_dump($a);
  # long(1) refcount(2)
  debug_zval_dump($b);
  ```

  PHP在修改一个变量以前，会首先查看这个变量的refcount，如果refcount大于1，PHP就会执行一个分离的过程（在Zend引擎中，分离是破坏一个引用对的过程）对于上面的代码，当执行到第三行的时候，PHP发现var想要改变，并且它指向的zval的refcount大于1，那么PHP就会复制一个新的zval出来，改变其值，将改变的变量指向新的zval（，并将原zval的refcount减1，并修改symbol_table里该变量的指针，使得 a 和 b 分离(Separation)。这个机制就是所谓的copy on write（写时复制，这里的写包括普通变量的修改及数组对象里的增加、删除单元操作）

- `Change On Write`

  使用变量复制的时候 ，PHP内部并不是真正的复制，而是采用指向相同的zval结构来节约开销。那么，对于PHP中的引用，又是如何实现呢

  ```php
  $a = 1;
  $reference = &$a;
  $a = 2;
  
  # refcount(2)  is_ref(1)
  xdebug_debug_zval( 'a' );
  ```

  代码运行结果，$a 会被改为 2, 这个过程叫做 `change on write`, ZE 如何得知是否采用 Separation ？这个需要用到 

  $a 的 is_ref属性，它代表是否被 & 引用，变量的 is_ref 默认为 0 ，大于 0则表示被引用，当 is_ref > 0 或者 refcount = 1,此时不需要 Separation，而是直接修改 zval 的值

  ```php
  if($if_ref || $refcount = 1){
      # alter zval instead of Separation
  }
  ```

  `尽管已经存在写时复制和写时改变，但仍然还存在一些不能通过is_ref和refcount来解决的问题`

  ```php
  $var = 1;
  $var_dup = $a;
  $var_ref = &$var;
  ```

  当执行第二行代码的时候,变量的值必须分离成两份完全独立的存在，也就是说php将一个zval的isref从0设为1之前，当然此时refcount还没有增加，会看该zval的refcount，如果refcount>1，则会分离, 将var_dup分离出去，并将var和var_ref做change on write关联。也就是，refcount=2, is_ref=1.
  所以内存会给变量var_dup 分配出一个新的zval，类型与值同 var和var_ref指向的zval一样，是新分配出来的，尽管他们拥有同样的值，但是必须通过两个zval来实现。试想一下，如果三者指向同一个zval的话，改变 `$vardup`的值，那么var和 var_ref 也会受到影响，这样是错误的

  ![](https://image-static.segmentfault.com/238/459/2384596612-569f6e23462c4_articlex)

  类似的：

  ```php
  $a = 1;
  $b = &$a;
  $c = $a;
  ```

  ![](https://image-static.segmentfault.com/113/070/113070557-569f6e35da911_articlex)

- `debug_zval_dump 参数是引用的话，refcount永远为1`

  ```php
  $a = 1;
  $b = &$a;
  
  # long(1) refcount(1)
  debug_zval_dump($a);
  ```

  PHP先看变量指向的zval是否被引用，如果是引用，则不再产生新的zval
  甭管哪个变量引用了它，比如有个变量a被引用了，b=&a，就算自己引用自己a=&a，a所指向的zval都不会被复制，改变其中一个变量的值，另一个值也被改变（change on write）
  如果is_ref为0且refcount大于1，改变其中一个变量时，复制新的zval（copy on write）

### Reference Counting

PHP5.2中使用的内存回收算法是[Reference Counting](http://en.wikipedia.org/wiki/Reference_counting)，中文叫做“引用计数”，其思想非常直观和简洁：为每个内存对象分配一个计数器，当一个内存对象建立时计数器初始化为1（因此此时总是有一个变量引用此对象），以后每有一个新变量引用此内存对象，则计数器加1，而每当减少一个引用此内存对象的变量则计数器减1，当垃圾回收机制运作的时候，将所有计数器为0的内存对象销毁并回收其占用的内存。而PHP中内存对象就是zval，而计数器就是refcount。

Important : [Reference Counting Basics](http://php.net/manual/en/features.gc.refcounting-basics.php)

出现的问题 ： 引用的值为变量自身，内存泄漏 -> [泄露实例](http://php.net/manual/en/features.gc.refcounting-basics.php)

```php
$a = array( 'one' );
$a[] =& $a;
xdebug_debug_zval( 'a' );
```

```bash
# 类似如下
a: (refcount=2, is_ref=1)=array (
   0 => (refcount=1, is_ref=0)='one',
   1 => (refcount=2, is_ref=1)=...
)
```

图示：

![](http://php.net/manual/zh/images/12f37b1c6963c1c5c18f30495416a197-loop-array.png)

能看到数组变量 (a) 同时也是这个数组的第二个元素(1) 指向的变量容器中“refcount”为 *2*。上面的输出结果中的"..."说明发生了递归操作, 显然在这种情况下意味着"..."指向原始数组。

跟刚刚一样，对一个变量调用unset，将删除这个符号，且它指向的变量容器中的引用次数也减1。所以，如果我们在执行完上面的代码后，对变量 a 调用unset, 那么变量 $a 和数组元素 "1" 所指向的变量容器的引用次数减1, 从"2"变成"1". 下例可以说明:

```php
unset($a);

xdebug_debug_zval( 'a' );

(refcount=1, is_ref=1)=array (
   0 => (refcount=1, is_ref=0)='one',
   1 => (refcount=1, is_ref=1)=...
)
```

![](http://php.net/manual/zh/images/12f37b1c6963c1c5c18f30495416a197-leak-array.png)

### 清理变量容器的问题(Cleanup Problems)

尽管不再有某个作用域中的任何符号指向这个结构(就是变量容器)，由于数组元素“1”仍然指向数组本身，所以这个容器不能被清除 。因为没有另外的符号指向它，用户没有办法清除这个结构，结果就会导致`内存泄漏`。庆幸的是，php将在脚本执行结束时清除这个数据结构，但是在php清除之前，将耗费不少内存。如果你要实现分析算法，或者要做其他像一个子元素指向它的父元素这样的事情，这种情况就会经常发生。当然，同样的情况也会发生在对象上，实际上对象更有可能出现这种情况，因为对象总是隐式的被引用。

如果上面的情况发生仅仅一两次倒没什么，但是如果出现几千次，甚至几十万次的内存泄漏，这显然是个大问题。这样的问题往往发生在长时间运行的脚本中，比如请求基本上不会结束的守护进程(deamons)或者单元测试中的大的套件(sets)中。后者的例子：在给巨大的eZ(一个知名的PHP Library) 组件库的模板组件做单元测试时，就可能会出现问题。有时测试可能需要耗用2GB的内存，而测试服务器很可能没有这么大的内存。

###回收周期(Collecting Cycles)

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

