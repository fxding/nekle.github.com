---
layout: post
title: Input Iterator
tags: STL 
---

2011年3月27日
19:48

<script type="text/javascript" src="/js/syntax-js/shBrushCpp.js"></script>

## 应用：find
<pre class="brush: cpp">
template < class InputIterator, class T>
InputIterator find(InputIterator first, InputIterator last, const T& value) {
  while (first != last && *first != value) ++first;
  return first;
}
</pre>
 
### Concept:

* ①是可取值的通过`*`操作，即`*p`有充分意义。
* ②可比较两个input iterator对象的相等性，如first != last。可以判断是否到达range尾端。
* ③可以被复制，如first、last值的传入调用的copy constructor。
* ④每个iterator都有一个value type，即iterator所指对象的类型。
* ⑤可对一个input iterator进行累加操作，即`++p`和`p++`有意义。
* ⑥我们可以通过`*iterator`获得iterator所指的值，但不一定能通过`*it = x`修改其值。
* ⑦input iterator仅支持指针运算中的一小部分，如input iterator可以累加，但不一定提供递减和算术运算。
* ⑧input  iterator也只支持测试两个input  iterator是否相等，不能比较先后顺序。
 
**注意:**
这些Concept限制是运用input iterator的算法所受到的限制，而不是构造input iterator所受到的限制，任何运用input iterator的算法都不能超出input iterator的限制。
 
例：
<pre class="brush: cpp">
// 这是一个input iterator，能够为「来自某一 basic_istream」的对象执行
// 格式化输入动作。注意，此版本为旧有之 HP 规格，未符合标准接口：
// istream_iterator< T, char T, traits, Distance>
// 然而一般使用input iterators 时都只使用第一个template 参数，此时以下仍适用。
// 注：SGI STL 3.3已实作出符合标准接口的 istream_iterator。作法与本版大同小异。
// 本版可读性较高。
template < class T, class Distance = ptrdiff_t>
class istream_iterator {
  friend bool
  operator== __STL_NULL_TMPL_ARGS (const istream_iterator< T, Distance>& x,
                                          const istream_iterator< T, Distance>& y);
  // 以上语法很奇特，请参考C++ Primer p834: bound friend function template
  // 在 < stl_config.h> 中，__STL_NULL_TMPL_ARGS 被定义为 < >
protected:
  istream* stream;
  T value;
  bool end_marker;
  void read() {
    end_marker = (* stream) ? true : false;
    if (end_marker) * stream >> value;
    // 以上，输入之后，stream 的状态可能改变，所以下面再判断一次以决定 end_marker
    // 当读到 eof 或读到型别不符的数据，stream 即处于 false 状态。
    end_marker = (* stream) ? true : false;
  }
public:
  typedef input_iterator_tag     iterator_category;
  typedef T                          value_type;
  typedef Distance               difference_type;
  typedef const T*               pointer;
  typedef const T&               reference;
  // 以上，因身为input iterator，所以采用 const 比较保险
 
  // 下面这些ctors 使 istream_iterator 和某个istream object 系结起来。
  istream_iterator() : stream(&cin), end_marker(false) {}
  istream_iterator(istream& s) : stream(&s) { read(); }
  // 以上两行的用法：
  //  istream_iterator< int> eos;            造成 end_marker 为 false。
  //  istream_iterator< int> initer(cin);   引发 read()。程序至此会等待输入。
  // 因此，下面这两行客端程序：
  //  istream_iterator< int> initer(cin);       (A)
  //  cout << "please input..." << endl;           (B)
  // 会停留在 (A) 等待一个输入，然后才执行 (B) 出现提示讯息。这是不合理的现象。
  // 规避之道：永远在最必要的时候，才定义一个 istream_iterator。
 
  reference operator* () const { return value; }
 #ifndef __SGI_STL_NO_ARROW_OPERATOR
  pointer operator->() const { return &(operator* ()); }
 #endif /* __SGI_STL_NO_ARROW_OPERATOR */
 
  // 迭代器前进一个位置，就代表要读取一笔数据
  istream_iterator< T, Distance>& operator++() {
    read();
    return *this;
  }
  istream_iterator< T, Distance> operator++(int)  {
    istream_iterator< T, Distance> tmp = *this;
    read();
    return tmp;
  }
};
 
 #ifndef __STL_CLASS_PARTIAL_SPECIALIZATION
 
template < class T, class Distance>
inline input_iterator_tag
iterator_category(const istream_iterator< T, Distance>&) {
  return input_iterator_tag();
}
 
template < class T, class Distance>
inline T* value_type(const istream_iterator< T, Distance>&) { return (T*) 0; }
 
template < class T, class Distance>
inline Distance* distance_type(const istream_iterator< T, Distance>&) {
  return (Distance*) 0;
}
 
 #endif /* __STL_CLASS_PARTIAL_SPECIALIZATION */
 
template < class T, class Distance>
inline bool operator==(const istream_iterator< T, Distance>& x,
                       const istream_iterator< T, Distance>& y) {
  return x.stream == y.stream && x.end_marker == y.end_marker ||
         x.end_marker == false && y.end_marker == false;
}
</pre>