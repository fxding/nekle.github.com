---
layout: post
title: (STL)Value Type(值类型)
tags: STL
---

<script type="text/javascript" src="/js/syntax-js/shBrushCpp.js"></script>

每个`iterator`都会指向某物，此物便是`iterator`的`value type`。
 
一个`iterator i`所指物的值，可以通过`*i`得到，`typeof（*i）`便是`iterator`的值类型。
 
### `iterator` 的值类型获取：

#### 通过函数嵌套方法间接获得：
<pre class="brush: cpp">
	template <\class I, class T>
	void f_impl(I iter, T t)
	{
		T tmp; // T 是 I 的 value type
		...
	}
	
	tempalte <\class T> inline void f(I iter)
	{
		f_impl(iter, *iter);
	}
</pre>

T便被实例化为iterator的值类型，可通过`T tmp`；声明iterator的值类型的临时变量。却不可以声明iterator值类型的返回类型。
 
#### 通过给iterator类增加一个型别类型成员变量：
<pre class="brush: cpp">
template <\class Node>
struct node_wrap
{
	typedef Node value_type;
	Node *ptr;
	...
}
</pre>

iterator的value type可以通过`typename I::valu_type`获得，但此法只适用于`iterator class`，而对于non-class iterator却无能为力，而且由于iterator是作为指针的特化，由于指针是non-class iterator，这样做会破坏iterator的一致性。
 
#### 定义专有`template class`获得iterator的值类型: 

<pre class="brush: cpp">
template <\class Iterator>
struct iterator_traits 
{
	typedef typename Iterator::value_type value_type;
}
</pre>
这样我们就可以使用下面的方式获得 iterator I 的 value type:

`typename iterator_traits<I>::value_type`


*  
<pre class="brush: cpp">
template<\class T>
struct iterator_traits<\T*>{
	typedef T value_type;
}
</pre>

* 

<pre class="brush: cpp">
template<\class T>
struct iterator_traits<\const T*>{
	typedef T value_type;
}
</pre>

* 应用
 
<pre class="brush: cpp">
template<\class InputIerator>
typename iterator_traits<\InputIterator>::value_type
sum_nonempty(InputIerator first, InputIerator last){
	typename iterator_traits<\InputIerator>::value_type result = 
 		*first++;
 	for (; first!=last; ++first) 
 		result += *first;
 	return result;
 }
 </pre>

 