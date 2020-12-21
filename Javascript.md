# **1、undefined与null**
执行以下代码，注意 == 与===
```javascript
var person;
document.write(person + "<br>");
var car=null
document.write(car + "<br>");
document.write(person==car? "true<br>":"false<br>");//输出true
document.write(person===car);//输出false
```
根据
[*undefined与null的区别*][http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html]

**null表示"没有对象"，即该处不应该有值。**

    （1） 作为函数的参数，表示该函数的参数不是对象。

    （2） 作为对象原型链的终点。

**undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。**

    （1）变量被声明了，但没有赋值时，就等于undefined。

    （2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。

    （3）对象没有赋值的属性，该属性的值为undefined。

    （4）函数没有返回值时，默认返回undefined。
# **2、正则表达式**

    i	执行对大小写不敏感的匹配。
    g	执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
    m	执行多行匹配。
[正则][https://www.runoob.com/js/js-regexp.html]

# **3、Undefined 不是 Null**
在 JavaScript 中, null 用于对象, undefined 用于变量，属性和方法。

对象只有被定义才有可能为 null，否则为 undefined。

如果我们想测试对象是否存在，在对象还没定义时将会抛出一个错误。

错误的使用方式：

    if (myObj !== null && typeof myObj !== "undefined") 
正确的方式是我们需要先使用 typeof 来检测对象是否已定义：

    if (typeof myObj !== "undefined" && myObj !== null) 

# **4、Promise**

Promise 构造函数只有一个参数，是一个函数，这个函数在构造之后会直接被异步运行，所以我们称之为起始函数。起始函数包含两个参数 resolve 和 reject。

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

输出

    a / b = 0
    End

Promise 类有 .then() .catch() 和 .finally() 三个方法，这三个方法的参数都是一个函数，.then() 可以将参数中的函数添加到当前 Promise 的正常执行序列，.catch() 则是设定 Promise 的异常处理序列，.finally() 是在 Promise 执行的最后一定会执行的序列。 .then() 传入的函数会按顺序依次执行，有任何异常都会直接跳到 catch 序列，
resolve() 中可以放置一个参数用于向下一个 then 传递一个值，then 中的函数也可以返回一个值传递给 then。但是，如果 then 中返回的是一个 Promise 对象，那么下一个 then 将相当于对这个返回的 Promise 进行操作，这一点从刚才的计时器的例子中可以看出来。

reject() 参数中一般会传递一个异常给之后的 catch 函数用于处理异常。

但是请注意以下两点：

resolve 和 reject 的作用域只有起始函数，不包括 then 以及其他序列；
resolve 和 reject 并不能够使起始函数停止运行，别忘了 return。

异步函数


异步函数（async function）是 ECMAScript 2017 (ECMA-262) 标准的规范，几乎被所有浏览器所支持，除了 Internet Explorer。

在 Promise 中我们编写过一个 Promise 函数：

 
    function print(delay, message) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log(message);
            resolve();
        }, delay);
    });
}
然后用不同的时间间隔输出了三行文本：

 

    print(1000, "First").then(function () {
    return print(4000, "Second");
        }).then(function () {
    print(3000, "Third");
});

我们可以将这段代码变得更好看：

 
    async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
    }
    asyncFunc();