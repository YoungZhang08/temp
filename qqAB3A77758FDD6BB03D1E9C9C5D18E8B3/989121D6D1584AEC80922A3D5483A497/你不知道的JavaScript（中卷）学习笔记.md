1. js中有七种内置类型:null，undefined，boolean，number，string，object，symbol
(除object外都称为基本类型)

2. null是基本类型中的唯一一个假值，typeof对它返回值为object

3. function也是js的内置类型，实际是object的子类型。具体，函数是可调用对象，内部的[[call]]属性使得它能够被调用。typeof对function的返回值为function

4. Array也是object的子类型，也是对象，typeof对它返回值为object

5. 对变量使用typeof操作符时，得到的结果是该变量持有的值的类型，而非该变量的类型，js中变量没有类型

6. undefined(变量已在作用域中声明但没有赋值)＝＝＝undefined，使用typeof返回undefined
 
    undeclared(变量还没有在作用域中声明)＝＝＝ReferenceError: x is not defined，使用typeof依然返回undefined[因为typeof内部有一个特殊的安全防范机制]

---

7. 含有空白或者空缺单元的数组时，空白或者空缺单元的元素值为undefined，使用delete可以将单元从数组中删除，但是，删除后数组的length属性不会发生改变

8. 数组通过数字进行索引，但是因为它们也是对象，所以也可以包含字符串健值和属性(这些不计算在数组长度内)。还有，如果字符串健值能被强制类型转换为十进制数字的话，它会被当作数字索引来处理。

9. 类数组可以通过数组工具函数(indexOf，concat，forEach，slice等)来实现。

```
function foo() {
    var arr ＝ Array.prototype.slice.call(arguments);
    arr.push("bam");
    console.log(arr);
}
foo("bar"，"baz"); // ["bar"，"baz"，"bam"]
```

10. 字符串和字符串数组不同，字符串不可变，数组可变。字符串不可变是指字符串的成员函数不会改变其原始值，而是创建并返回一个新的字符串。而数组的成员函数都是在其原始值上进行操作。

11. 字符串可以借用数组的非变更成员方法来处理字符串，例如:Array. prototype.join.call。

12. 字符串不可变，无法借用数组的可变更成员函数实现字符串反转，变通方法为:先将字符串转换为数组，处理完之后再转换为字符串。
 var c ＝ a.split("").reverse().join();

13. JS只有一种数值类型：number（数字），包括整数（没有小数的十进制数）和带小数点的十进制数。JS中使用双精度格式存储浮点数