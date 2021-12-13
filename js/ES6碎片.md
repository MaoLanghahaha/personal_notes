#### 1、块级作用域
ES5只有全局作用域和函数作用域，没有块级作用域，导致错误应用场景：
```javascript
// 场景1：内层变量可能会覆盖外层变量
var tmp = new Date()
function fn() {
  console.log(tmp)
  if (false) {
    var tmp = 'hello world'
  }
}
fn() // undefined 
```
分析:内层tmp==变量提升==[^变量提升]，覆盖了外层tmp变量
更多：见《小知识点（理论）.md》 —— 2、ES5变量提升
[^变量提升]: 变量提升到它所在作用域的顶端去执行，到我们代码所在的位置来赋值