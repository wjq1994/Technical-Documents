#### 如何学习javascript

```
1. 学习变量、函数等这些基本语法
2. 理解他的变量，以及函数在内存中的作用
```

#### 如何更好的理解js的语法

###### 1. 理解变量在内存中是如何工作的

变量分为基本类型和引用类型
> 《JavaScript高级程序设计》P68

基本数据类型： undefined、Null、Boolean、Number、String

``` javascript
var num1 = 5; /* 变量中保存的是真实的值 */
var num2 = num1; /* 在后面改动num1的值完全不影响num2的值 */
```

引用类型

``` javascript
var obj1 = new Object(); /* new Object() 是保存在堆内存中的 ，变量obj1只是new出来得这个对象的引用*/
var obj2 = obj1; /* obj1，obj2都是new出来得这个对象的引用 */
obj1.name = "Tom";  /* 所以后面更改obj1.name 会影响obj2.name的值*/
```
