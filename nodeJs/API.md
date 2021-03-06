[http://nodejs.cn/api/](http://nodejs.cn/api/)

# Node.js v8.7.0 文档

## 模块 module
### `__dirname`
```
console.log(__dirname);
// Prints: /Users/mjr
console.log(path.dirname(__filename));
// Prints: /Users/mjr
```

### `__filename`
```
console.log(__filename);
// Prints: /Users/mjr/example.js
console.log(__dirname);
// Prints: /Users/mjr
```

### module.exports
square.js
```
module.exports = (width) => {
  return {
    area : () => width ** 2
  };
};
```

### require
bar.js
```
const square = require('./square.js');
const mySquare = square(2);

console.log(`正方形的面积是 ${mySquare.area()}`);
```

### 循环
当循环调用 require() 时，一个模块可能在未完成执行时被返回。

例如以下情况:

a.js:
```
console.log('a 开始');
exports.done = false;
const b = require('./b.js');
console.log('在 a 中，b.done = %j', b.done);
exports.done = true;
console.log('a 结束');
```

b.js
```
console.log('b 开始');
exports.done = false;
const a = require('./a.js');
console.log('在 b 中，a.done = %j', a.done);
exports.done = true;
console.log('b 结束');
```

main.js
```
console.log('main 开始');
const a = require('./a.js');
const b = require('./b.js');
console.log('在 main 中，a.done=%j，b.done=%j', a.done, b.done);
```

当 main.js 加载 a.js 时，a.js 又加载 b.js。 此时，b.js 会尝试去加载 a.js。 为了防止无限的循环，会返回一个 a.js 的 exports 对象的 未完成的副本 给 b.js 模块。 然后 b.js 完成加载，并将 exports 对象提供给 a.js 模块。

当 main.js 加载这两个模块时，它们都已经完成加载。 因此，该程序的输出会是：

```
$ node main.js
main 开始
a 开始
b 开始
在 b 中，a.done = false
b 结束
在 a 中，b.done = true
a 结束
在 main 中，a.done=true，b.done=true
```

需要仔细的规划, 以允许循环模块依赖在应用程序内正常工作.

## 路径 path

①path.basename​：获取文件名，并可去除文件后缀

②path.delimiter​：获取路径分隔符，windows是“；”，Linux是“：”

主要用于分割路径：process.env.PATH.split(path.delimiter)  分割环境变量，“：”为分割符

③path.dirname：获取文件路径中的目录部分

④path.extname​：获取后缀名,或扩展名

⑤path.join是路径拼接作用，函数定义时没有形参，通过arguments获取用户传入的所有参数，因而传入的参数可为任意个​
