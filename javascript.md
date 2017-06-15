#### **1.隐式全局变量和显式全局变量的区别**

隐式的全局变量和显式定义的全局变量之间有着细微的差别，差别在于通过delete来删除它们的时候表现不一致。

* 通过var创建的全局变量（在任何函数体之外创建的变量）不能被删除。
* 没有用var创建的隐式全局变量（不考虑函数内的情况）可以被删除。

也就是说，隐式全局变量并不算是真正的变量，但他们是全局对象的属性成员。属性是可以通过delete运算符删除的，而变量不可以被删除。

#### **2.枚举man对象的实例属性**

![](/assets/import2-1.png)

```js
for (var i in man) {
    console.log(i, ":", man[i]);
}
```

```js
var i,
    hasOwn = Object.prototype.hasOwnProperty;
for (i in man) {
    if (hasOwn.call(man, i)) { // filter
        console.log(i, ":", man[i]);
    }
}
```

#### **3.eval\(\)**

new Function\(\)的用法和eval\(\)非常类似，应当特别注意。这种构造函数的方式很强大，但往往被误用。如果你不得不使用eval\(\)，你可以尝试用new Function\(\)来代替。这有一个潜在的好处，在new Function\(\)中运行的代码会在一个局部函数作用域内执行，因此源码中所有用var定义的变量不会自动变成全局变量。还有一种方法可以避免eval\(\)中定义的变量转换为全局变量，即是将eval\(\)包装在一个立即执行的匿名函数内。

```js
console.log(typeof un);// "undefined"
console.log(typeof deux); // "undefined"
console.log(typeof trois); // "undefined"

var jsstring = "var un = 1; console.log(un);";
eval(jsstring); // logs "1"

jsstring = "var deux = 2; console.log(deux);";
new Function(jsstring)(); // logs "2"

jsstring = "var trois = 3; console.log(trois);";
(function () {
    eval(jsstring);
}()); // logs "3"

console.log(typeof un); // "number"
console.log(typeof deux); // "undefined"
console.log(typeof trois); // "undefined"
```

eval\(\)和Function构造函数还有一个区别，就是eval\(\)可以修改作用域链，而Function更像是一个沙箱。不管在什么地方执行Function，它只能看到全局作用域。因此它不会太严重的污染局部变量。在下面的示例代码中，eval\(\)可以访问且修改其作用域之外的变量，而Function不能（注意，使用Function和new Function是完全一样的）。

```js
(function () {
    var local = 1;
    eval("local = 3; console.log(local)"); // logs 3
    console.log(local); // logs 3
}());

(function () {
    var local = 1;
    Function("console.log(typeof local);")(); // logs undefined
}());
```

#### **4.空格**

适合使用空格的地方包括：

* for循环中的分号之后，比如
  `for (var i = 0; i < 10; i += 1) {...}`
* for循环中初始化多个变量，比如
  `for (var i = 0, max = 10; i < max; i += 1) {...}`
* 分隔数组项的逗号之后，
  `var a = [1, 2, 3];`
* 对象属性后的逗号以及名值对之间的冒号之后，
  `var o = {a: 1, b: 2};`
* 函数参数中，
  `myFunc(a, b, c)`
* 函数声明的花括号之前，
  `function myFunc() {}`
* 匿名函数表达式function之后，
  `var myFunc = function () {};`

另外，我们推荐在运算符和操作数之间添加空格。也就是说在+, -, \*, =, &lt;, &gt;, &lt;=, &gt;=, ===, !==, &&, \|\|, +=符号前后都添加空格。

