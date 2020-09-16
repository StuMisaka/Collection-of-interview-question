##### 1.

```js
var msg = 'hello';
for(var i = 0;i<10;i++){
	var msg = 'hello' + i*2 +i;
}
alert(msg);    //hello189
```

两个msg处于同一作用域，因此因是 hello + 18 + 9

##### 2.

```js
let x = 10;
let foo = () => {
	console.log(x);
	let x = 20;
	x ++;
}
foo();   //ReferenceError
```

暂时死区，let并不会变量提升，但实际上js引擎会意识到后边的let定义，只是不支持在let声明语句前引用该变量。

也存在另外一种说法，let也会变量提升，但暂时死区限制了在let x前使用x。这种说法认为let的创建过程被提升了，而初始化过程没有被提升，var的创建和初始化过程（初始化为undefined）都被提升了，function的创建，初始化和赋值过程都被提升。

##### 3.

```js
function test(){
	var n = 4399;
	
	funtion add(){
		n++;
		console.log(n);
	}
	
	return {n:n,add:add}
}

var result = test();
var result2 = test();

result.add();                    //4400
result.add();                    //4401
console.log(result.n);//4399
result2.add();                //4400
```

首先,js引擎会开辟堆内存，存放代码块，这里包括test,result,result2的声明。执行过程中，result被赋值，开辟一个堆内存，形成一个作用域。result2同理，所以他们之间互相不受影响。而执行result.add()时又会形成新的私有作用域。所以依次是4400 4401 4399(这里是堆内存里的那个n) 4400。

##### 4.

```js
typeof null = object    //true
type of undefined = object //false  应是undefinr
typeof [] = object //true
typeof 5 = object //false  应是number
```

null会被认为是一个空的对象引用，所以会返回object

##### 5.

```js
function foo(){
	console.log('first');
	setTimeout(function(){
		console.log('second');
	},5);
}

for(var i = 0;i < 43999999999;i++){
    foo();
}

//先全部输出first，再全部输出second
```

js是单线程的，setTimeout之类的异步任务会放在macrotask（事件队列）中，所有的执行栈中的代码全部执行完后才会再执行事件队列中的代码。

##### 6.

```js
(function() {
	var a = b =5;
})();
console.log(b);  //5
console.log(a);  //not defined
```

这里相当于 `var a = b; b = 5`。如果是非严格模式，那么b会被自动声明为全局变量，所以为5，而a依然为局部变量，所以是not defined

##### 