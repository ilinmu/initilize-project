
# 代码规范

## 原则

代码可读性要强，容易让人理解。要符合大部分人的思维。

### 变量

* 变量名要有意义，不要使用诸如 `a`,`b`,`c`,`obj`,`arr`这一类命名，正确命名应该是`userList`，`optionsMap` 等；

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

### 媒体

图片都应该经过压缩处理，压缩后的图片不应该出现肉眼可感知的失真。60 质量的 JPEG 格式图片与质量大于 60 的相比，肉眼已看不出明显的区别，因此保存 JPEG 图的时候，质量一般控制在 60，若保真度要求高的图片可适量提高到 80，图片大小控制在 200KB 以内。


