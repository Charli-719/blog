---
title: json对象遍历
date: 2018-08-30 15:08:36
tags: 前端
categories: 前端
---

![autumn](json对象遍历/autumn.jpg)
之前一直都是对json数组进行操作，顺便看一下(*^__^*)
<!-- more -->
# 遍历json对象

```
var jsonObj = {"name": "jeans", "password": "1111"};
for (var val in jsonObj) {
    alert(val + " " + jsonObj[val]);//输出如:name
}
```
# 遍历json数组，元素为json对象
```
## 无规律json数组
var jsonArr = [{"name":"jeans","age":24,"sex":"女",city:"江苏"},
            {"text":"shelly","content":123654}];

for (var i = 0, l = jsonArr.length; i < l; i++) {
    for (var key in jsonArr[i]) {
        alert(key + ':' + jsonArr[i][key]);
    }
}
## 有规律json数组

var jsonArr =[
{"name":"jeans","age":24,"sex":"女",city:"江苏"},
{"name":"shelly","age":21},"sex":"女","city":"上海"}
]
(1)、
for (var p in jsonArr) {//遍历json数组时，这么写p为索引，0,1
    alert(jsonArr[p].name + " " + jsonArr[p].age);
}
(2)、
for(var i = 0; i < jsonArr.length; i++){
   alert(jsonArr[i].name + " " + jsonArr[i].age);
}
(3)、es6 的 map 方法
const html = jsonArr.map((x)=>{
console.lo(x); //x={"name":"jeans","age":24,"sex":"女",city:"江苏"}
return <p>名字:{x.name};年龄:{x.age};性别:{x.sex};城市:{x.city}</p>
})
//map后的结果插入到页面中就可以了
```




# 深度遍历复合Json结构数据
```
JSON对象里面可以嵌套多层对象(数组或对象)，嵌套层数未知
```
```
/**
 * 深度遍历
 * 复合的Json结构数据，JSON对象里面可以嵌套多层对象(数组或对象)
 */
 function deepJson(json){
  // 1. 变量为json对象：将key输出，value进行递归
     if(isType(json, "Object")){
          for(var key in json){
           $("#out").append(key + ' : ');
           if(isType(json[key], "Array") || isType(json[key], "Object")){
            $("#out").append("下面为子项内容<br/>");
           }
           deepTraverse(json[key]);
          }
     }
     // 2. 变量为json数组：逐个元素递归
     else if(isType(json, "Array")){
          for(var i=0; i<json.length; i++){
               var jsonObj = json[i];
               deepTraverse(jsonObj);
              // 遍历数组中的元素(为json对象)后输出：分隔线+一个换行符
              if(isType(jsonObj, "Object")){
                $("#out").append("------------------------<br/>");
               }
          }
     }
     // 3. 变量为简单数据类型：直接输出（递归函数的终止条件）
     else if(isType(json, "String") || isType(json, "Number") ||
      isType(json,"Boolean") || isType(json,"Null")){
          $("#out").append(json);
          $("#out").append("<br/>");
     }
 }
```



