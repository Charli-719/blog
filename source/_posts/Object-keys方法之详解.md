---
title: Object.keys方法之详解
date: 2019-02-11 18:32:00
tags: 前端
categories: 前端
---


本文转自 [Object.keys方法之详解](https://blog.csdn.net/suwu150/article/details/60965257).

![Object.keys方法之详解](Object-keys方法之详解/object.jpg)
<!--more-->
# Object.keys方法之详解
   在实际开发中，我们有时需要知道对象的所有属性，原生js给我们提供了一个很好的方法：Object.keys()，该方法返回一个数组,其中这个数组的内容就是这个对象的所有键值

传入对象，返回属性名

```
var obj = {'a':'123','b':'345'};
console.log(Object.keys(obj));  //['a','b']

var obj1 = { 100: "a", 2: "b", 7: "c"};
console.log(Object.keys(obj1)); // console: ["2", "7", "100"]

var obj2 = Object.create({}, { getFoo : { value : function () { return this.foo } } });
obj2.foo = 1;
console.log(Object.keys(obj2)); // console: ["foo"]
```

传入字符串，返回索引

```
var str = 'ab1234';
console.log(Object.keys(str));  //[0,1,2,3,4,5]
```
如果我们想要获取字符串中的某一个值,那么我们就可以通过下面的方法获取:
```
var str = 'ab1234';
console.log(Object.keys(str));  //[0,1,2,3,4,5]

Object.keys(str).map(function (index) {
  console.log(str[index]);
});
```
获取的结果如下面所示:
```
[ '0', '1', '2', '3', '4', '5' ]
a
b
1
2
3
4
```
我们可以进行自己的处理

构造函数 返回空数组或者属性名
```
    function Pasta(name, age, gender) {
            this.name = name;
            this.age = age;
            this.gender = gender;
            this.toString = function () {
                    return (this.name + ", " + this.age + ", " + this.gender);
            }
    }
    console.log(Object.keys(Pasta)); //console: []
    var spaghetti = new Pasta("Tom", 20, "male");
    console.log(Object.keys(spaghetti)); //console: ["name", "age", "gender", "toString"]
    ```
数组 返回索引
```
    var arr = ["a", "b", "c"];
    console.log(Object.keys(arr)); // console: ["0", "1", "2"]
```
# Notes
In ES5, if the argument to this method is not an object (a primitive), then it will cause a TypeError. In ES6, a non-object argument will be coerced to an object.

```
Object.keys("foo");
// TypeError: "foo" is not an object (ES5 code)

Object.keys("foo");
// ["0", "1", "2"]                   (ES6 code)
```
