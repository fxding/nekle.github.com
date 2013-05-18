---
layout: post

title: STL-Vector 内部结构

tags: STL Vector

---

<script type="text/javascript" src="/js/syntax-js/shBrushCpp.js"></script>

2011年7月11日
16:00

### 一、vector结构
> vector是一个顺序存储容器，内部使用动态数组实现。支持元素的随机访问，所以只要知道位置，就可以在常数时间内存取该元素。在末尾插入和删除元素时，vector的性能很好，可以在常数时间内完成。如果实在前段或中部插入或删除元素，性能很差，需在线性时间内完成，因为动作点之后的每一个元素都必须移动，每一次都必须调用赋值运算。

> ![img](http://farm9.staticflickr.com/8130/8748723621_8399faf990.jpg)

### 二、空间管理
* 1、 初始换空间分配
    * a、默认请况下，空间分配为0.（第一次插入空间变为 1 ）
    * b、可设定空间大下和初始化值。`Vector(10, "a")`；
* 2、空间不足
    * a、当空间不足时，重新申请一个原来2倍的空间，拷贝原来的值，然后析构原来的空间(若空间分配失败，则析构已分配的空间，rollback)
* 3、若想减少空间，可利用swap函数，但效率低下。

 
### 三、具体动作空间管理
* 1、重分配空间大小`reserve(size_type n)`，若n大于现在空间则重新申请空间，将数据拷贝到新空间，并设置iterator信息，否则不做处理。
* 2、管理数据数量`resize(size_type new_size, const T& x)`，若new_size小于现在数据size则析构多余数据，否则用x补够数据。
* 3、`clear`析构所有数据空间

### 四、动作与相关型别失效
* 1、插入和删除元素，都会使动作点之后的元素的references、pointers、iterators失效，如果插入动作会引起空间大小重新分配，那么该容器上的所有references、pointers、iterators都会失效。
* 2、`swap`则所有references、pointers、iterators都会失效。
* 3、
 
### 二、相关型别
 <pre class="brush: cpp"> 
// 以下标示(1),(2),(3),(4),(5)，代表 iterator_traits< I> 所服务的5个型别。
  typedef T value_type;						//  (1)
  typedef value_type* pointer;			//  (2)
  typedef const value_type* const_pointer;
  typedef const value_type* const_iterator;
  typedef value_type& reference;			// (3)
  typedef const value_type& const_reference;
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;		// (4)
  // 以下，由于vector 所维护的是一个连续线性空间，所以不论其元素型别为何，
  // 原生指标都可以做为其迭代器而满足所有需求。
  typedef value_type* iterator;
  // 根据上述写法，如果客端写出这样的码：
  // vector< Shape>::iterator is;
  // is 的型别其实就是Shape*
  // 而STL 内部运用 iterator_traits< is>::reference 时，获得Shape&
  // 运用iterator_traits< is>::iterator_category 时，获得
  // random_access_iterator_tag        (5)
  // (此乃iterator_traits 针对原生指标的特化结果)
 </pre>