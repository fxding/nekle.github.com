---
layout: post
title: STL Set/MultiSet 内部结构
tags: STL Set/MultiSet
---

<script type="text/javascript" src="/js/syntax-js/shBrushCpp.js"></script>

2011年7月13日
21:39

### 一、空间结构
set和multiset都是采用rb_tree实现，内部方法都是调用的rb_tree方法。

* 抽象结构：

>![img](http://farm9.staticflickr.com/8551/8749903434_c6a8325423.jpg)


### 二、空间管理
>参见rb_tree
 
### 三、set与multiset比较
**相同点：**

* 1、采用rb_tree实现
* 2、只允许插入和删除操作，修改是不允许的(iterator为const类型)
* 3、提供接口完全相同。
* 4、若要修改元素值，必须先删除原来的值插入新值。
* 5、元素都是排好序的，所以修改元素值排序会被破坏。
* 6、移除等值元素erase(value)，移除所有等于value的元素。(list 用remove(value))
     
**不同点：**

* 1、set中元素不允许重复，而multiset中元素允许重复。
* 2、插入时set采用insert_unique，而multiset采用insert_equal
* 3、插入返回值不同，set由于不允许元素重复，所以要么成功要么失败；而multiset则允许元素重复

*注意：*其中pos_hint只是一个位置提示，它可以提升插入性能，也是为了提供一种统一的接口。
 
### 四、具体动作对指针影响
   参见rb_tree
 
### 五、相关型别
<pre class="brush: cpp">
public:
  // typedefs:
 
  typedef Key key_type;
  typedef Key value_type;
  // 注意，以下 key_compare 和 value_compare 使用相同的比较函式
  typedef Compare key_compare;
  typedef Compare value_compare;
 
private:
  /* 注意，identity 定义于 < stl_function.h>，参见第7章，其定义为：
    template < class T>
    struct identity : public unary_function< T, T> {
      const T& operator()(const T& x) const { return x; }
    };
  */
  // 以下，rb_tree< Key, Value, KeyOfValue, Compare, Alloc>
  typedef rb_tree< key_type, value_type,
                     identity< value_type>, key_compare, Alloc> rep_type;
  rep_type t;  // 采用红黑树（RB-tree）来表现 set
public:
  typedef typename rep_type::const_pointer pointer;
  typedef typename rep_type::const_pointer const_pointer;
  typedef typename rep_type::const_reference reference;
  typedef typename rep_type::const_reference const_reference;
  typedef typename rep_type::const_iterator iterator;
  // 注意上一行，iterator 定义为 RB-tree 的 const_iterator，这表示 set 的
  // 迭代器无法执行写入动作。这是因为 set 的元素有一定次序安排，
  // 不允许使用者在任意处做写入动作。
  typedef typename rep_type::const_iterator const_iterator;
  typedef typename rep_type::const_reverse_iterator reverse_iterator;
  typedef typename rep_type::const_reverse_iterator const_reverse_iterator;
  typedef typename rep_type::size_type size_type;
  typedef typename rep_type::difference_type difference_type;

</pre>