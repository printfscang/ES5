# ES5
ES5核心技巧


1、作用域
（1）在ES5中变量是函数级作用域，在当前函数作用域顶端进行提升
（2）js有变量的提升，同时也有函数的提升。优先级：函数提升>变量提升
例1：(function(){
alert(a);
var a = 1;
function a(){
}
})()
结果为：function a(){}   
代码分析：
(function(){
function a(){
}
var a;
alert(a);
a = 1;
})()
当var a;没有被赋值时，程序自动忽略这段代码，反之，打印出1

例2：(function(){
var a = b = 1;
})()
alert(a);
alert(b);
结果为：a is not defined      1
代码分析：
(function(){
var a = 1;   //赋值运算符从右至左运算
b = 1;         //b为全局变量
})()

2、变量回收
function(){
var a = 1;
return funtion(){
//eval();
//with();
//try{}catch(){}
}
}
test();
代码分析：当变量未被使用时会回收到VO对象中，以上注释的3个方法不会导致a被回收，因为不确定a是否会被用到

3、this（谁调用指向谁）
this.a = ‘extern’;
var test = {
a:‘inner’,
init:function(){
alert(this.a);
}
}
test.init(); 
var  win = test.init;
win();

结果为：inner   extern
代码分析：test.init()当前init函数被test所调用，因此执行test函数里的a。而变量win相当于把init函数体复制到test函数外面，test函数的外层函数是window，此时是window调用win，相当于window.win()，因此执行外面的this.a = 'extern'

特殊：
this.a = 'extern';
var test = {
a:'inner',
init:function(){
funtion win(){
alert(this.a);
}
win();
}
}
test.init();
结果为：extern
代码分析：当找不到对应的执行体，直接就指向window

4、箭头函数
this.a = 'extern';
var test = {
a:'inner',
init:()=>{
alert(this.a);
}
}
test.init(); 
结果：extern
分析：箭头函数会bind父函数的this。this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数。

5、原型链prototype
function test(){
this.a = 20;
}
test.prototype.a = 30;
alert((new test()).a);
结果：20
