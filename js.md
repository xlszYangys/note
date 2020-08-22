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
```javascript
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
```javascript
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
