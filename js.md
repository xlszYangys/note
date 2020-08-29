##componentWillMount 和 componentDidMount的区别

1、componentWillMount  将要装载，在render之前调用；

      componentDidMount，（装载完成），在render之后调用

2、componentWillMount  每一个组件render之前立即调用；

      componentDidMount  render之后并不会立即调用，而是所有的子组件都render完之后才可以调用

3、componentWillMount  可以在服务端被调用，也可以在浏览器端被调用；

      componentDidMount  只能在浏览器端被调用，在服务器端使用react的时候不会被调用



[node-sass 安装失败的各种坑](https://www.jianshu.com/p/92afe92db99f)

###ubuntu 20.04版本更新软件源为国内源（清华、网易、阿里云等等）
```bash
/etc/apt/sources.list

deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

sudo apt update 
sudo apt upgrade
```






















devRedict

##[在React中使用react-router-dom路由](https://www.jianshu.com/p/8954e9fb0c7e)



###git modified
+ 目录没有被跟踪

##git restore --staged <file>...
[git restore](https://www.jianshu.com/p/dcef204dba74)
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  
  
rebase





















## JavaScript 异步编程

[JavaScript 异步编程](https://www.runoob.com/js/js-async.html)

##回调函数
```javascript
function print() {
    document.getElementById("demo").innerHTML="RUNOOB!";
}
setTimeout(print, 3000);
```
```javascript
setTimeout(function () {
    document.getElementById("demo").innerHTML="RUNOOB!";
}, 3000);
```
```javascript
setTimeout(function () {
    console.log("1");
}, 1000);
console.log("2");
```

##异步 AJAX
```javascript
var xhr = new XMLHttpRequest();
xhr.onload = function () {
    // 输出接收到的文字数据
    document.getElementById("demo").innerHTML=xhr.responseText;
}
xhr.onerror = function () {
    document.getElementById("demo").innerHTML="请求出错";
}
// 发送异步 GET 请求
xhr.open("GET", "https://www.runoob.com/try/ajax/ajax_info.txt", true);
xhr.send();
```
```javascript
$.get("https://www.runoob.com/try/ajax/demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
});
```
##JavaScript Promise
###构造 Promise
```javascript
new Promise(function (resolve, reject) {
    // 要做的事情...
});
```
例如，如果我想分三次输出字符串，第一次间隔 1 秒，第二次间隔 4 秒，第三次间隔 3 秒：
```javascript
setTimeout(function () {
    console.log("First");
    setTimeout(function () {
        console.log("Second");
        setTimeout(function () {
            console.log("Third");
        }, 3000);
    }, 4000);
}, 1000);
```
## 现在我们用 Promise 来实现同样的功能：
```javascript
new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log("First");
        resolve();
    }, 1000);
}).then(function () {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log("Second");
            resolve();
        }, 4000);
    });
}).then(function () {
    setTimeout(function () {
        console.log("Third");
    }, 3000);
});``
```
Promise 构造函数只有一个参数，是一个函数，这个函数在构造之后会直接被异步运行，所以我们称之为起始函数。起始函数包含两个参数 resolve 和 reject。
当 Promise 被构造时，起始函数会被异步执行：
```javascript
new Promise(function (resolve, reject) {
    console.log("Run");
});
```
这段程序会直接输出 Run。
resolve 和 reject 都是函数，其中调用 resolve 代表一切正常，reject 是出现异常时所调用的：
```javascript
new Promise(function (resolve, reject) {
    var a = 0;
    var b = 1;
    if (b == 0) reject("Diveide zero");
    else resolve(a / b);
}).then(function (value) {
    console.log("a / b = " + value);
}).catch(function (err) {
    console.log(err);
}).finally(function () {
    console.log("End");
});
```
这段程序执行结果是:
```text
a / b = 0
End
```
```javascript
new Promise(function (resolve, reject) {
    console.log(1111);
    resolve(2222);
}).then(function (value) {
    console.log(value);
    return 3333;
}).then(function (value) {
    console.log(value);
    throw "An error";
}).catch(function (err) {
    console.log(err);
});
```
执行结果：
```text
1111
2222
3333
An error
```
resolve() 中可以放置一个参数用于向下一个 then 传递一个值，then 中的函数也可以返回一个值传递给 then。但是，如果 then 中返回的是一个 Promise 对象，那么下一个 then 将相当于对这个返回的 Promise 进行操作，这一点从刚才的计时器的例子中可以看出来。
reject() 参数中一般会传递一个异常给之后的 catch 函数用于处理异常。

但是请注意以下两点：
* resolve 和 reject 的作用域只有起始函数，不包括 then 以及其他序列；
* resolve 和 reject 并不能够使起始函数停止运行，别忘了 return。
##Promise 函数
```javascript
function print(delay, message) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log(message);
            resolve();
        }, delay);
    });
}
```

```javascript
print(1000, "First").then(function () {
    return print(4000, "Second");
}).then(function () {
    print(3000, "Third");
});
```

##闭包
	
闭包是一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。
直观的说就是形成一个不销毁的栈环境。

```javascript
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();
 
add();
add();
add();
 
// 计数器为 3
```
###实例解析
变量 add 指定了函数自我调用的返回字值。
自我调用函数只执行一次。设置计数器为 0。并返回函数表达式。
add变量可以作为一个函数使用。非常棒的部分是它可以访问函数上一层作用域的计数器。
这个叫作 JavaScript 闭包。它使得函数拥有私有变量变成可能。
计数器受匿名函数的作用域保护，只能通过 add 方法修改。

##JavaScript 验证 API
```html
<input id="id1" type="number" min="100" max="300" required>
<button onclick="myFunction()">验证</button>
<p id="demo"></p>
 
<script>
function myFunction() {
    var inpObj = document.getElementById("id1");
    if (inpObj.checkValidity() == false) {
        document.getElementById("demo").innerHTML = inpObj.validationMessage;
    }
}
</script>
```

#Event.target
+ 触发事件的对象 (某个DOM元素) 的引用。当事件处理程序在事件的冒泡或捕获阶段被调用时，它与event.currentTarget不同。
+ event.target 属性可以用来实现事件委托 (event delegation)。

```javascript
// Make a list
var ul = document.createElement('ul');
document.body.appendChild(ul);

var li1 = document.createElement('li');
var li2 = document.createElement('li');
ul.appendChild(li1);
ul.appendChild(li2);

function hide(e){
  // e.target 引用着 <li> 元素
  // 不像 e.currentTarget 引用着其父级的 <ul> 元素.
  e.target.style.visibility = 'hidden';
}

// 添加监听事件到列表，当每个 <li> 被点击的时候都会触发。
ul.addEventListener('click', hide, false);
```

##Web API 接口参考
[Web API 接口参考](https://developer.mozilla.org/zh-CN/docs/Web/API)