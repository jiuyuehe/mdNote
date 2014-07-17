javaScript 学习
-----
* 学习的目标：
	* 对象
	* 函数
	* 数组
	* 类与模块

对象
----
    * **内置对象**：数组，函数，日期，正则
    * **宿主对象**： dom
    * **自定义对象**： 由运行的js创建
    * **自有属性**： 直接在对象中定义的属性
    * **继承属性**： 是在对象的原型对象中定义的属性

创建对象： 对象直接量，关键字new ,Object.create();

原型：
  	inherit() 集成原型函数的熟属性，但是不修改原型函数相同属性的值。
	var len = book.subtitile.length;//不存在，因为undefined没有length属性
	同样的写法有：
	var len = undefined;
	if(book){
		if(book.subtitle) len = book.subtitle.length;
	 } 等同于
	
	var len = book && booke.subtitile && book.subtitle.length;


数组
---
* 数组的方法
* forEach(); 3个参数：数组元素，元素索引，数组本身。
	`var data =[1,2,3,4,5,6];
    var sum = 0;
    data.forEach(function(value){ sum +=value});
    alert(sum);
    data.forEach(function(v,i,a){a[i] = v+1});
    alert(data);`

