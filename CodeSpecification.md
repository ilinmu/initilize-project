
# 代码规范

## 原则

代码可读性要强，容易让人理解。要符合大部分人的思维。

javscript 统一使用 [ES6](https://es6.ruanyifeng.com/) 语法。

不要使用 `var` 声明变量。

对象使用解构赋值。

优先使用对象展开运算符 `...` 来做对象浅拷贝而不是使用 `Object.assign`，数组同理。

### 变量

* 变量名要有意义，不要使用诸如 `a`,`b`,`c`,`obj`,`arr`这一类命名，正确命名应该是`userList`，`optionsMap` 等；

* 统一使用驼峰式命名规则。用 `usetList`，而非 `usetlist` 或 `user_list`;

* 尽量不要使用缩写，例如 `delFile` 应写成 `deleteFile`；

* 变量命名不要添加无意义的上下文，假设有一个 Dialog 组件，它的标题应该为 `title`，而不是 `dialogTitle`。

```javascript
  const car = {
    carColor: 'red',  // bad
    color: 'red',  // googd
  }
```

* 项目中不要出现魔术数字，比如 `setTimeout(func, 86400000)`, 应该将 `86400000` 独立命名，`const MILLISECONDS_IN_A_DAY = 86400000`，主要好处：
方便搜索；
根据变量命名理解意义，便于理解代码；
多处复用，减少书写 bug。

### 函数

使用参数默认值方式：

```javascript
// bad
function handleConvertName(name) {
  const paramsName = name || 'John';
  // ...
}
// googd
function handleConvertName(name = 'John') {
  // ...
}
```

函数参数不应多于2个，参数比较多的情况下传解构参数

```javascript
// bad
function createDialog(title, content, buttonText, cancellable) {
  // ...
}
// googd
function createDialog({ title, content, buttonText, cancellable }) {
  // ...
}
```

函数职责应单一，`KISS（ Keep It Simple, Stupid）`原则，便于函数复用、可维护、提升代码可测性；

```javascript
// bad
function sendEmailToActiveClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}

// googd
function sendEmailToActiveClients(clients) {
  clients
    .filter(isActiveClient)
    .forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

合并重复代码，`DRY (Don't Repeat Yourself)`原则, 写代码的时候注意一下，重复代码比较多，一定要想办法做合并；

```javascript
// bad
function doSomething() {
  // ...
  if (test) {
    return {
      name: '张三',
      age: 20,
    };
  }
  return {
    name: '李四',
    age: 20,
  };
}
// googd
function doSomething() {
  // ...
  return {
    name: test ? '张三' : '李四',
    age: 20,
  }
}

// bad
function jumpToPage() {
  // ...
  if(id === 0){
    navigateTo({
      url: ''
    })
  }
  else if (id === 1) {
    navigateTo({
      url: ''
    })
  }
  else if (id === 2) {
    navigateTo({
      url: ''
    })
  }
  // ...
}

// good
function jumpTOPage() {
  // ...
  let url = '';
  switch(id) {
    case 1:
      url = 'a';
      break;
    case 2:
      url = 'b';
      break;
    default:
      url = 'c';
      break;
  }
  navigateTo({
    url,
  });
}
```

多行导入应该像多行数组和对象文字一样缩进；
```javascript
// good
import {
  longNameA,
  longNameB,
  longNameC,
  longNameD,
  longNameE,
} from 'path'

// bad
import { longNameA, longNameB, longNameC, longNameD, longNameE } from 'path'
```

使用 === 和 !== 而非 == 和 !=;
```javascript
// good
if (flag === 'test')
if (flag !== 'test')

// bad
if (flag == 'test')
if (flag != 'test') 
```

### 样式

样式书写一般有两种：一种是紧凑格式 (Compact)

```css
.test { display: block; width: 50px; }
```
一种是展开格式（Expanded）

```css
.test {
  display: block;
  width: 50px;
}
```

统一使用展开格式书写样式。

分号每个属性声明末尾都要加分号；

```css
.test {
  width: 100%;
  height: 100%;
}
```

代码易读性左括号与类名之间一个空格，冒号与属性值之间一个空格。
```css
/* googd */
.test {
  width: 100%;
}
/* bad */
.test{
  width:100%;
}
```

逗号分隔的取值，逗号之后一个空格。

```css
/* good */
.test {
  box-shadow: 1px 1px 1px #333, 2px 2px 2px #ccc;
}
/* bad */
.test {
  box-shadow: 1px 1px 1px #333,2px 2px 2px #ccc;
}
```

不要为 0 指明单位。

```css
/* good */
.test {
  margin: 0 10px;
}
/* bad */
.test {
  margin: 0px 10px;
}
```

颜色为 16 进制时，用小写字母，尽量简写。

```css
/* good */
.element {
  color: #abcdef;
  background-color: #012;
}

/* bad */
.element {
  color: #ABCDEF;
  background-color: #001122;
}

```

### 媒体

图片都应该经过压缩处理，压缩后的图片不应该出现肉眼可感知的失真。60 质量的 JPEG 格式图片与质量大于 60 的相比，肉眼已看不出明显的区别，因此保存 JPEG 图的时候，质量一般控制在 60，若保真度要求高的图片可适量提高到 80，图片大小控制在 200KB 以内。

### HTML

HTML文件必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明：
```html
<!DOCTYPE html>
```

页面编码统一使用 `UTF-8`
```html
<meta charset="UTF-8">
```

### Git

提交代码禁止无意义信息，例如“0625”，“bug”，“修复bug”等。代码应多次提交，每次完成一个独立功能提交一次代码，例如“完成客户信息组件开发”，“完成公司信息组件开发”，“修复客户信息显示错误的bug”，“与后端进行客户信息相关的接口联调”等。提交的信息一定要明确表明改动的代码的用途。
