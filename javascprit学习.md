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

### 加快js加载和执行的速度

> 1.在head之后选择加载js代码文件
>
> 2.无阻塞脚本，在HTML页面加载完毕后才进行脚本的下载。
>
> 3.延迟加载脚本，在script脚本中加入参数defer
>
> 4.动态加载脚本代码示例
>
> ```javascript
> var script = document.createElement ("script"); 
>    script.type = "text/javascript"; 
>    script.src = "script1.js"; 
>    document.getElementsByTagName("head")[0].appendChild(script);
> ```

### iframe

> ```html
> <iframe src="demo_iframe_sandbox.htm"></iframe>//最基本的iframe用法
> ```

> iframe标签中的属性：
>
> ```
> 1.frameborder:是否显示边框，1(yes),0(no)
>
> 2.height:框架作为一个普通元素的高度，建议在使用css设置。
>
> 3.width:框架作为一个普通元素的宽度，建议使用css设置。
>
> 4.name:框架的名称，window.frames[name]时专用的属性。
>
> 5.scrolling:框架的是否滚动。yes,no,auto。
>
> 6.src：内框架的地址，可以使页面地址，也可以是图片的地址。
> 7.srcdoc , 用来替代原来HTML body里面的内容。但是IE不支持, 不过也没什么卵用
> 8.sandbox: 对iframe进行一些列限制，IE10+支持
> ```

* 如何获取iframe中的内容

> 主要利用两个api来进行获取
>
> contentWindow获取iframe的window对象
>
> contentDocument获取iframe的document对象
>
> 示例

```javascript
var iframe = document.getElementById("iframe1");
 var iwindow = iframe.contentWindow;
 var idoc = iwindow.document;
        console.log("window",iwindow);//获取iframe的window对象
        console.log("document",idoc);  //获取iframe的document
        console.log("html",idoc.documentElement);//获取iframe的html
        console.log("head",idoc.head);  //获取head
        console.log("body",idoc.body);  //获取body
```

> 通过name的方式

```html
<iframe src ="/index.html" id="ifr1" name="ifr1" scrolling="yes">
  <p>Your browser does not support iframes.</p>
</iframe>
<script type="text/javascript">
    console.log(window.frames['ifr1'].window);
console.dir(document.getElementById("ifr1").contentWindow);
</script>
```

### 采用localStorage/sessionStorage来保存数据

* 原理localStorage/sessionStorage都是一个本地存储数据接口。唯一的差别是时效，localStorage是永久存在本地的，就算关闭浏览器，也不会删除数据。sessionStorage 也是持久保存，唯一的差别是，在重启浏览器的时候，会清空数据。
* 使用方法

```javascript
 localStorage.setItem('localData','localStorage test data');//设置数据
    var localData = localStorage.getItem('localData');//取出数据
    sessionStorage.setItem('sessionData','session test data');//设置数据
    var sessionData = sessionStorage.getItem('sessionData');//取出数据
```

* 使用场景ocalStorage/sessionStorage适合保存那种在较九不需要修改的数据信息，比如用户登陆网站的配置信息等！而不适合保存一次性数据！

```javascript
localStorage.removeItem('localData');//删除数据
    sessionStorage.removeItem('sessionData');//删除数据
```

* 在关闭浏览器的时候选择将数据进行清除。这样就能够更好的进行操作。

### JavaScript的变量提升

> 变量提升，就是js将所有声明的声明提升到当前作用域的顶部，这就是我们可以在变量声明之前使用该变量的主要原因。但是，虽然会提升声明，但是 并不会执行初始化的过程。

### use strict的作用

> use strict 就是所谓的严格模式，最主要的一个作用是，强制开发者在未声明变量之前无法使用该变量。
>
> ```
> //example
> "use strict";
> test();
> function test() {
>   x=3.14; //会报错
>   return x*x;
> }
> ```