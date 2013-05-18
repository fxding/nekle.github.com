---
layout: post
titile: Modeling、Concept、Refinement and Inheritance
tags: STL
---
 
### Concept：
* concept是一组型别条件（type requirement）。
* concept是型别的集合。
* concept是一组合法程序。
 
 
### Modeling：（一个型别和一组型别之间的关系）
	如果型别T满足某个concept C的所有条件，则型别T就是Concept C的一个model, model是型别
	与concept间的关系.
 
### Refinement：（一组型别和另一组型别之间的关系）
	如果concept C2提供concept C1的所有功能，再加上其他可能的额外功能，则说C2是C1的refinement.

* 自反性（自身）
* 涵盖性（满足C2的model必满足C1）
* 传递性（concept间）refinement是concept间的关系。
 
### inheritance：（两个型别之间的关系）


<a href="http://www.flickr.com/photos/95621335@N02/8737635679/" title="d3bcbb57806c75ff295731c5633d23fe by top us, on Flickr"><img src="http://farm8.staticflickr.com/7286/8737635679_7cf00554d4.jpg" width="351" height="308" alt="d3bcbb57806c75ff295731c5633d23fe"></a>