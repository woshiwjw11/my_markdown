## 每天写一点js学习

### 计算数组的极值

```javascript
function smallest(array){
  return Math.min.apply(Math,array);
}//计算最小值
function largest(array){
  return Math.max.apply(Math,array);//
}
//原理，主要是执行Math中的数学函数，通过apply来传递参数，第一个参数代表函数所属于的对象，第二个参数必须是数组，表示传入的参数。

```

### 将参数转化成数组

```javascript
function transformToArray(arg){
  return Array.prototype.slice.call(arg);
}
```

### 简单实现合并对象

```javascript
funtion merge(root){
  for(var i=1;i<arguments.length;i++){
    for(var key in arguments[i]){
      if(object.hasOwnProperty(key)){
        root[key]=argument[i][key];
      }
    }
  }
  return root;
}
var merged=merge({name:'shokc'},{city:"shenzhen"})
```

### 上传图片预览功能

```html
<input type="file" name="file" onchange="showpre(this)"/>
<img id ="portrait" src="" width="70" height="75">
```

```javascript
function showpre(source){
  var file=source.files[0];
  if(window.FileReader){
    var fr=new FileReader();
    fr.onloadend=function(e){
      document.getElementById("portrait").src=e.target.result;
    };
    fr.readAsDataURL(file);
  }
}
```

### hasOwnProperty

> hasOwnProperty是Object.prototype的一个方法，它可是个好东西，他能判断一个对象是否包含自定义属性而不是原型链上的属性，因为hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数。

```javascript
// 修改Object.prototype
Object.prototype.bar = 1; 
var foo = {goo: undefined};

foo.bar; // 1
'bar' in foo; // true

foo.hasOwnProperty('bar'); // false
foo.hasOwnProperty('goo'); // true
//当检查对象上某个属性是否存在时，hasOwnProperty 是唯一可用的方法。同时在使用 for in loop 遍历对象时，推荐总是使用 hasOwnProperty 方法，这将会避免原型对象扩展带来的干扰
```

### ES6学习

* let指令，在ES6中，新增加了ES6的指令，该指令和var指令差不多，不同之处在于，let指令定义的变量只在该代码块下存在如：

```javascript
{
  var i=1;
  let j=1;
}
console.log(i);//1
console.log(j);//undefind
```

* let在循环语句中的应用主要有以下几点

```javascript
for(let i=0;i<10;i++){
  
}
console.log(i)//undefind let只在代码块中实现
for(let i=0;i<10;i++){
  let i="abc";
  console.log(i);
}
//输出三个abc
//for循环中的父语句，和循环内的子语句互影响
```

* let声明的变量不会有有变量提升的说法

```javascript
//var 的情况
console.log(i);//undefind
var i=2;
//在定义变量之前，该变量已经存在 只是没有值，所以会告诉系统为undefind
//let的情况
console.log(b);//报错
let b="11"
//用let表示声明之前，该变量是不存在的，如果有使用到的话，则会报错。
var temp="abc";
if(true){
  temp='abc';
  let temp;//如果在块级区域之内的话，在声明之前就无法使用该变量，就算是全局变量也是一样的。
}
//let 不允许重复声明
```
### 什么是闭包

> 闭包的作用使能够让一个函数从内部访问其外部函数的作用域。

* 闭包使用的例子

> 闭包的使用用途之一就是，将对对象数据的私有化。在 JavaScript 中，闭包是用来实现数据私有的原生机制。当你使用闭包来实现数据私有时，被封装的变量只能在闭包容器函数作用域中使用。例如：

```javascript
const getSecret = (secret) => {
  return {
    get: () => secret
  };
};

test('Closure for object privacy.', assert => {
  const msg = '.get() should have access to the closure.';
  const expected = 1;
  const obj = getSecret(1);

  const actual = obj.get();

  try {
    assert.ok(secret, 'This throws an error.');
  } catch (e) {
    assert.ok(true, `The secret var is only available
      to privileged methods.`);
  }

  assert.equal(actual, expected, msg);
  assert.end();
});
```

> 在上面的例子里，`get()` 方法定义在 `getSecret()` 作用域下，这让它可以访问任何 `getSecret()` 中的变量，于是它就是一个被授权的方法。在这个例子里，它可以访问参数 `secret`。
>
> 通过返回函数来获取外部函数的参数。这样就变成了一个私有变量。对于闭包的使用案例，和场景还有许多，需要更加深入的了解。

### 理解js中this的含义

```javascript
function a(){
    var user = "s";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();
//由上述代码可以看得出，this指代的是最终调用它的对象。
```

> 情况1：如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window，这里需要说明的是在js的严格版中this指向的不是window。
>
> 情况2：如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象。
>
> 情况3：如果一个函数中有this，**这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象**
>
> 例子如下所示：
>
> 

```javascript
var o = {
    user:"11",
    fn:function(){
        console.log(this.user);  //11
    }
}
o.fn();

var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12
        }
    }
}
o.b.fn();


```

* 构造函数版this

```javascript
function Fn(){
    this.user = "s";
}
var a = new Fn();
console.log(a.user); //s
```

> 这里之所以对象a可以点出函数Fn里面的user是因为new关键字可以改变this的指向，将这个this指向对象a，为什么我说a是对象，因为用了new关键字就是创建一个对象实例。