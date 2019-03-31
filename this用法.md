# this用法

#### 说明

```
如何确定this：
分析函数的两个过程，一个是执行环境，一个是创建环境
普通函数：函数什么时候执行，this指向执行时候的调用对象
箭头函数：先找到函数什么时候创建，创建的环境是什么，this指向创建时候的调用对象
```

------

#### 目录

[this的默认绑定](#list1)

[this的隐式绑定](#list2)

[this的显式绑定：(call和bind方法)](#list3)

[箭头函数中this的绑定](#list4)


------

#### <div id="list1"></div>this的默认绑定

###### 找到调用函数的对象window
```javascript
function fire () {
	console.log(this === window)
}
fire(); // 输出true
```


###### 调用定义在函数内部的函数
> 理解关键：明确函数的调用对象
> 
```javascript
// innerSay是内部函数
// 误区: say函数的作用域对innerSay的影响
// 理解: 虽然innerSay函数是在say函数里执行（和调用它的对象不是一回事），没有明确的调用对象的时候，将对函数的this使用默认绑定：绑定到全局的window对象
function say() {
	function innerSay() {
		console.log(this == window);
	}
	innerSay();
}
say()
```

###### 隐式绑定
> 理解关键：明确函数的调用对象
> 
```javascript
// innerSay是对象内部函数的调用
// 误区: innerSay在obj.say里执行，所以执行环境是obj.say
// 理解: 虽然innerSay函数是在say函数里执行（和调用innerSay的对象不是一回事），innerSay函数没有前面的调用（如果obj.innerSay(),obj就是函数的调用对象）
var obj = {
	say: function () {
		function innerSay() {
			console.log(this === window)
		}
		innerSay();
	}
}
obj.say()

```

###### 总结
```
凡事函数作为独立函数调用，无论它的位置在哪里，它的行为表现，都和直接在全局环境中调用无异
```

#### <div id="list2"></div>this的隐式绑定

###### 什么是隐式绑定
> 当函数被一个对象“包含”的时候，我们称函数的this被隐式绑定到这个对象里面了
> 

###### 函数作为对象的一个属性
> 理解关键：明确函数的调用对象
> fire函数作为
```javascript
// 
var obj = {
    a: 1,
    fire: function () {
        console.log(this.a)
    }
}
obj.fire(); // 输出1
```

###### this是动态绑定的，是在代码运行期绑定而不是在定义期
```javascript
// 我是第一段代码
function fire () {
      console.log(this.a)
}
  
var obj = {
      a: 1,
      fire: fire
  }
obj.fire(); // 输出1
 
// 我是第二段代码
var obj = {
        a: 1,
        fire: function () {
             console.log(this.a)
         }
}
obj.fire(); // 输出1
```

###### 隐式绑定中的this传递丢失问题
>  理解的关键：函数运行时，调用的对象
>  this传递丢失问题，可以使用call，bind解决

```javascript

var obj = {
	a: 1,    // a是定义在对象obj中的属性   1
	fire: function () {
		console.log(this.a)
	}
}
 
var a = 2;  // a是定义在全局环境中的变量    2
var fireInGrobal = obj.fire;
//obj.fire相当于一个变量 指向function () {console.log(this.a)}这个函数
//fireInGrobal = obj.fire 现在fireInGrobal这个变量也指向函数
fireInGrobal(); //  输出 2
//所以执行函数的时候，调用的对象就是window
```

#### <div id="list3"></div>this的显式绑定：(call和bind方法)
###### bind用法
> bind指定调用的对象（this传递不会丢失）
> 
> 变量先声明，后初始化（声明提升，初始化不会提升（初始化可以理解为赋值））

```javascript
var obj = {
	say: function () {
		function _say() {
			// this 是什么？想想为什么？
			this === window; //true
		}
		//这里绑定的obj，在代码执行到这一步的时候，只是声明了obj，并没有赋值（未定义），所以绑定的obj是undefined
		return _say.bind(obj) //obj=undefined
	}()
}
obj.say()
```

```javascript
var obj = {};
obj.say = function () {
	function _say() {
		// this 是什么？想想为什么？
		this === obj;//true
	}
	return _say.bind(obj)
}()
obj.say()
```

#### <div id="list4"></div>箭头函数中this的绑定
#### [mdn this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)

######  箭头函数中this被设置为他被创建时的环境（区分与执行时）

```javascript
var name = 'window'
function Person(name) {
	this.name = name;
	this.show1 = function() {
		console.log(this.name)
	}
	this.show2 = () => console.log(this.name)
	this.show3 = function() {
		return function() {
			console.log(this.name)
		}
	}
	this.show4 = function() {
		//this;
		return() => console.log(this.name)
	}
}

var personA = new Person('personA')
var personB = new Person('personB')

personA.show1()
personA.show1.call(personB)

personA.show2()
personA.show2.call(personB)

personA.show3()()
personA.show3().call(personB)
personA.show3.call(personB)()

//当this.show4所指向的函数执行的时候，才有() => console.log(this.name)这个函数的创建，所以箭头函数this所指向的环境，就是this.show4所指向的函数执行的环境。
personA.show4()()
personA.show4().call(personB)
personA.show4.call(personB)()
```
