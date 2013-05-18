---
layout: post
title: Output Iterator
tags: STL

---

<script type="text/javascript" src="/js/syntax-js/shBrushCpp.js"></script>

2011年3月28日
15:22


##应用：copy
<pre class="brush: cpp">
template < class InputIterator, class OutputIterator>
OutputIterator copy(InputIerator first, InputIerator last, OutputIterator result)
{
	for (; first != last; ++result, ++first)
		*result = *first;
	return result;
}
</pre>

##Concept：
* ①复制（参数传入）和赋值（`*result=*first`）；
* ②可累加（`++result`），表达式`++p`与`p++`有意义；
* ③single-pass（单回），只写性（可以`*p=x`，但不能`x=*p`）；
* ④不提供比较（!=、==、<、>）
* ⑤
 
##例：ostream_itertor
<pre class="brush:cpp">
// 这是一个output iterator，能够将对象格式化输出到某个 basic_ostream 上。
// 注意，此版本为旧有之 HP 规格，未符合标准接口：
// ostream_iterator< T, char T, traits>
// 然而一般使用output iterators 时都只使用第一个template 参数，此时以下仍适用。
// 注：SGI STL 3.3已实作出符合标准接口的 ostream_iterator。作法与本版大同小异。
// 本版可读性较高。
template < class T>
class ostream_iterator {
protected:
  ostream* stream;
  const char* string;        // 每次输出后的间隔符号。
public:
  typedef output_iterator_tag     iterator_category;
  typedef void                    value_type;
  typedef void                    difference_type;
  typedef void                    pointer;
  typedef void                    reference;
 
  // 下面这些ctors 使 istream_iterator 和某个 istream object 系结起来。
  ostream_iterator(ostream& s) : stream(&s), string(0) {}
  ostream_iterator(ostream& s, const char* c) : stream(&s), string(c)  {}
  // 以上 ctors 的用法：
  //  ostream_iterator< int> outiter(cout, ' ');  输出至 cout，每次间隔一个空格
 
  // 对迭代器做赋值（assign）动作，就代表要输出一笔数据
  ostream_iterator< T>& operator=(const T& value) {
    *stream << value;                // 先输出数值
    if (string) *stream << string;    // 如果状态无误，再输出间隔符号
    return *this;
  }
  ostream_iterator< T>& operator*() { return *this; }
  ostream_iterator< T>& operator++() { return *this; }
  ostream_iterator< T>& operator++(int) { return *this; }
};
 
 </pre>