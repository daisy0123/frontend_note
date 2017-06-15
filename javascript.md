#### **1.隐式全局变量和显式全局变量的区别**

隐式的全局变量和显式定义的全局变量之间有着细微的差别，差别在于通过delete来删除它们的时候表现不一致。

* 通过var创建的全局变量（在任何函数体之外创建的变量）不能被删除。
* 没有用var创建的隐式全局变量（不考虑函数内的情况）可以被删除。

也就是说，隐式全局变量并不算是真正的变量，但他们是全局对象的属性成员。属性是可以通过delete运算符删除的，而变量不可以被删除。

#### **2.枚举man对象的实例属性**

![](/assets/import2-1.png)

```js
var i,
    hasOwn = Object.prototype.hasOwnProperty;
for (i in man) {
    if (hasOwn.call(man, i)) { // filter
        console.log(i, ":", man[i]);
    }
}
```



