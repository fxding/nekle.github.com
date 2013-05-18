---
layout: post
title: STL Map/MultiMap
tags: STL Map/MultiMap 内部结构
---
<script type="text/javascript" src="/js/syntax-js/shBrushCpp.js"></script>

2011年7月14日
9:38

### 一、空间结构
>map和multimap将key-value pair当做元素，进行管理。它们可根据key的排序准则将元素排序。multimap允许元素重复，map不允许。

* 它们内部都采用rb_tree实现
      
>![img](http://farm9.staticflickr.com/8139/8749903444_ca65feb22d.jpg)
 
### 二、空间管理
>参见rb_tree
 
### 三、map与multimap比较
   参见set与multiset比较，不同在于map与multimap如果value_type 不是const的则可修改value值。
 
### 四、相关型别

<pre class="brush: cpp">
public:
 
  // typedefs:
 
  typedef Key key_type;           // 键值型别
  typedef T data_type;            // 资料（真值）型别
  typedef T mapped_type;          // (未被使用)
  typedef pair< const Key, T> value_type;    // 元素型别（键值/真值）
  typedef Compare key_compare;    // 键值比较函式
 
private:
  // 以下定义表述型别（representation type）。以map元素型别（一个pair）
  // 的第一型别，做为RB-tree节点的键值型别。
  typedef rb_tree< key_type, value_type,
                  select1st< value_type>, key_compare, Alloc> rep_type;
  rep_type t;  // 以红黑树（RB-tree）表现 map
public:
  typedef typename rep_type::pointer pointer;
  typedef typename rep_type::const_pointer const_pointer;
  typedef typename rep_type::reference reference;
  typedef typename rep_type::const_reference const_reference;
  typedef typename rep_type::iterator iterator;
  // 注意上一行，为什么不像set一样地将iterator 定义为 RB-tree 的 const_iterator？
  // 按说map 的元素有一定次序安排，不允许使用者在任意处做写入动作，因此
  // 迭代器应该无法执行写入动作才是。
  typedef typename rep_type::const_iterator const_iterator;
  typedef typename rep_type::reverse_iterator reverse_iterator;
  typedef typename rep_type::const_reverse_iterator const_reverse_iterator;
  typedef typename rep_type::size_type size_type;
  typedef typename rep_type::difference_type difference_type;


</pre> 


