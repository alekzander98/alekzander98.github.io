---
title: 翻译-<Web development for beginners: Asynchronous JavaScript – Promises and async functions>
date: 2025-09-30T17:15:28+08:00
draft: false
tags: [异步,Translate]
---

>原文：[Web development for beginners Asynchronous JavaScript – Promises and async functions](https://2ality.com/2025/09/javascript-async.html)
**这篇博文是“Web development for beginners”系列的一部分**——该系列教从未编程过的人如何用JavaScript创建Web应用。

要下载项目，请访问GitHub仓库[`learning-web-dev-code`](https://github.com/rauschma/learning-web-dev-code)并按照那里的说明操作。

___

在本章中，我们将学习如何处理需要很长时间才能完成的任务——比如下载文件。实现这一点的机制，即*Promise*和*异步函数*，是JavaScript的一个重要基础，使我们能够做各种有趣的事情。

-   [这是一个具有挑战性的章节](https://2ality.com/2025/09/javascript-async.html#this-is-a-challenging-chapter)
-   [数据结构*队列*](https://2ality.com/2025/09/javascript-async.html#the-data-structure-queue)
-   [JavaScript代码在单个*线程*中运行](https://2ality.com/2025/09/javascript-async.html#javascript-code-runs-in-a-single-thread)
    -   [通过`setTimeout()`添加一次性任务](https://2ality.com/2025/09/javascript-async.html#adding-a-task-once-via-settimeout())
    -   [通过`setInterval()`重复添加任务](https://2ality.com/2025/09/javascript-async.html#adding-a-task-repeatedly-via-setinterval())
-   [类`Date`](https://2ality.com/2025/09/javascript-async.html#class-date)
    -   [为当前时刻创建日期时间字符串](https://2ality.com/2025/09/javascript-async.html#creating-a-date-time-string-for-the-current-moment)
    -   [自某个起点以来经过了多少毫秒？](https://2ality.com/2025/09/javascript-async.html#how-many-milliseconds-have-passed-since-a-starting-point%3F)
-   [`string.slice()`](https://2ality.com/2025/09/javascript-async.html#string.slice())
-   [项目: `log-time.js`](https://2ality.com/2025/09/javascript-async.html#project%3A-log-time.js)
-   [项目: `block-browser.html`](https://2ality.com/2025/09/javascript-async.html#project%3A-block-browser.html)
-   [通过Promise异步传递结果](https://2ality.com/2025/09/javascript-async.html#delivering-results-asynchronously-via-promises)
    -   [Promise的状态](https://2ality.com/2025/09/javascript-async.html#the-states-of-a-promise)
    -   [创建Promise](https://2ality.com/2025/09/javascript-async.html#creating-a-promise)
    -   [示例：等待HTML元素被点击](https://2ality.com/2025/09/javascript-async.html#example%3A-waiting-until-an-html-element-is-clicked)
    -   [同步函数与异步函数](https://2ality.com/2025/09/javascript-async.html#synchronous-functions-vs.-asynchronous-functions)
-   [异步函数和`await`：处理通过Promise传递的结果](https://2ality.com/2025/09/javascript-async.html#async-functions-and-await%3A-handling-results-delivered-via-promises)
    -   [通过`Promise.resolve()`和`Promise.reject()`创建Promise](https://2ality.com/2025/09/javascript-async.html#creating-promises-via-promise.resolve()-and-promise.reject())
    -   [项目: `wait-for-click.html`](https://2ality.com/2025/09/javascript-async.html#project%3A-wait-for-click.html)
    -   [异步函数的结果](https://2ality.com/2025/09/javascript-async.html#the-result-of-an-async-function)
    -   [异步函数在同步和异步之间转换](https://2ality.com/2025/09/javascript-async.html#async-functions-translate-between-sync-and-async)
    -   [如果我们省略`await`会发生什么？](https://2ality.com/2025/09/javascript-async.html#what-happens-if-we-omit-await%3F)
    -   [异步代码具有传染性](https://2ality.com/2025/09/javascript-async.html#asynchronous-code-is-viral)
-   [Node.js的异步`fs`函数](https://2ality.com/2025/09/javascript-async.html#node.js%E2%80%99s-asynchronous-fs-functions)
-   [`fetch()`](https://2ality.com/2025/09/javascript-async.html#fetch())
    -   [项目: `log-url-text.js`](https://2ality.com/2025/09/javascript-async.html#project%3A-log-url-text.js)
    -   [项目: `random-quote-browser/`](https://2ality.com/2025/09/javascript-async.html#project%3A-random-quote-browser%2F)
-   [练习（无答案）](https://2ality.com/2025/09/javascript-async.html#exercises-(without-solutions))

## 这是一个具有挑战性的章节

本章涉及一些具有挑战性的话题。你可能不会立刻理解所有内容。然而，这很正常：

-   给自己一些时间。让事情沉淀一下。一天或几天后再重读某些部分。
-   学习并试验代码。
-   查看网络上其他资源（文章、视频等）也可能会有所帮助，这些资源采用不同的方法来解释这些主题。MDN是一个你可以首先求助的安全资源。

## 数据结构*队列*

队列是一种我们向其添加值，然后稍后从中检索值的数据结构。它就像售票亭前排队的人群：我们检索的第一个值是第一个被添加的值。我们检索的第二个值是第二个被添加的值。等等。这就是为什么队列也被称为FIFO数据结构：先进先出。JavaScript数组可以通过以下两种方法用作队列：

-   `array.push(v)`将值`v`添加到数组的末尾。
-   `array.shift()`检索并删除数组的第一个元素。

这再次类似于人类排队：人们在队尾加入队列，并在排到第一位时离开。

以下代码演示了我们如何将数组用作队列：

```cpp
const queue = [];

queue.push('a');
queue.push('b');
assert.deepEqual(
  queue, ['a', 'b']
);

assert.equal(
  queue.shift(), 'a'
);
assert.deepEqual(
  queue, ['b']
);

queue.push('c');
assert.deepEqual(
  queue, ['b', 'c']
);
```

-   我们按此顺序`.push()`值：`'a', 'b', 'c'`。
-   `.shift()`以相同的顺序接收值。

如果我们对空数组调用`.shift()`，它将返回`undefined`：

```scss
> [].shift()
undefined
```

## JavaScript代码在单个*线程*中运行

现代操作系统能够进行*多任务处理*：可以同时运行多个*任务*（可以理解为一段代码）。相比之下，大多数JavaScript代码是单任务的：

-   任务运行的环境称为线程。
-   默认情况下，所有JavaScript任务都在浏览器或Node.js的*主线程*中运行：它们*顺序*执行（一个接一个），并由*事件循环*管理——事件循环如下所示：

```cpp
while (true) {
  const task = taskQueue.shift(); // (A)
  task();
}
```

如果任务队列为空，则A行的`.shift()`将等待直到有任务为止。

换句话说：任务队列不断地用要运行的代码填充主线程。

为什么“事件循环”这个名字里有“事件”？虽然浏览器在单个线程中运行JavaScript，但它在其他线程中运行其他功能：例如，鼠标点击等用户输入是在另一个线程中接收的（该线程与主线程并发运行）。每个用户输入都作为任务添加到队列中。该任务调用适当的事件侦听器。接下来的两个小节描述了向队列添加任务的其他方法。

### 通过`setTimeout()`添加一次性任务

以下函数在指定的`delay`（以毫秒为单位，1000毫秒为一秒）后将`task`添加到事件队列中。

```scss
setTimeout(task, delay);
```

请注意，添加任务是在主线程之外管理的，并且相对精确。但是，实际执行可能会延迟——这取决于添加任务时队列的繁忙程度。

这是使用`setTimeout()`的样子：

```javascript
setTimeout(
  () => {
    console.log('一秒后');
  },
  1000
);
```

我们为什么要使用`setTimeout()`？一个长时间运行的任务可能会冻结浏览器并阻止其接受任何用户输入。`setTimeout()`使当前任务能够暂停，允许主线程处理用户输入，然后再继续。我们稍后将更详细地探讨其工作原理。

### 通过`setInterval()`重复添加任务

`setInterval()`的工作方式与`setTimeout()`类似。然而，后者只添加一次任务，而前者会多次添加任务（直到被停止）：

```javascript
const id = setInterval(task, delay);
clearInterval(id);
```

`setInterval()`返回一个`id`——我们可以将其传递给`clearInterval()`以阻止`task`再次运行。`delay`决定了每次调用任务之前的等待时间（以毫秒为单位）。

我们将在下一个项目中使用`setInterval()`。

## `Date`类

在接下来的两个项目中，我们将需要一些日期时间功能。`Date`是用于表示日期时间值的对象的类。请注意，`Date`类有许多限制。已经有一个名为[`Temporal`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Temporal#browser_compatibility)的替代品，但许多JavaScript平台尚不支持它。

### 为当前时刻创建日期时间字符串

考虑以下交互：

```javascript
> new Date().toISOString()
'2161-10-11T09:42:21.117Z'
```

我们为当前时刻创建了一个`Date`对象，并将其转换为符合ISO标准日期时间值表示法的字符串。

### 自某个起点以来经过了多少毫秒？

`Date.now()`方法返回自1970年1月1日UTC零点以来经过的毫秒数。因此，它有助于我们确定经过了多少毫秒：

```javascript
const sleep = (milliseconds) => {
  let start = Date.now();
  while ((Date.now() - start) < milliseconds);
};
```

请注意，这不是一个好的等待方式！我们稍后将用它来演示一个任务如何完全阻塞浏览器。

## `string.slice()`

字符串方法`.slice()`的工作方式与数组方法`.slice()`类似：

```python
> '2161-10-11T09:42:21.117Z'.slice(11, 19)
'09:42:21'
```

我们提取了调用`.slice()`的字符串的一个片段：它从索引11开始，到索引19之前结束。

## 项目: `log-time.js`

`log-time.js`是一个shell命令，用于将当前时间记录到终端：

```javascript
setInterval(
  () => {
    const d = new Date();
    console.log(
      d.toISOString().slice(11, 19)
    );
  },
  // 每秒调用一次函数
  1000 // 毫秒
);
```

## 项目: `block-browser.html`

浏览器的主线程不仅运行JavaScript，还处理屏幕上显示内容的更新。因此，一个长时间运行的任务会完全阻塞浏览器，因为主线程中没有其他功能可以运行。这个项目展示了这种情况。它具有以下用户界面：

```php-template
<p>
  <a id="block" href="">阻塞浏览器5秒钟</a>
<p>
<button>这是一个按钮</button>
<div id="statusMessage"></div>
```

我们的想法是，我们点击“阻塞”，然后通过JavaScript执行一个长时间运行的循环。在该循环期间，我们无法点击按钮，因为主线程被阻塞了。

```javascript
setStatusMessage('正在阻塞...');
setTimeout(
  () => {
    sleep(5000); // 阻塞浏览器
    setStatusMessage('完成');
  },
  1
);
```

我们已经看到了`sleep()`的实现方式。`setStatusMessage()`的实现如下：

```javascript
const setStatusMessage = (msg) => {
  document.querySelector('#statusMessage').innerText = msg;
};
```

## 通过Promise异步传递结果

现在我们知道了长时间运行的任务是多么有问题：如果某件事需要很长时间怎么办？我们必须以某种方式暂停当前任务，稍后再回来处理它，以便其他任务有机会运行。

在这种情况下，使用以下方法：

```cpp
const promise = downloadText(someUrl);
promise.then(
  (str) => {
    // 处理`str`
  }
);
```

`downloadText()` 需要很长时间才能返回结果。因此，它不能立即返回结果（至少在不阻塞浏览器的情况下）。相反，它返回一个 *Promise*——一个实际结果的占位符（结果尚未交付）。有了这个 Promise，我们使用 `.then()` 方法注册一个事件监听器，一旦结果准备好，就会调用该监听器。

通过 Promise 传递结果的函数称为*异步*函数。普通函数称为*同步*函数——我们立即获得结果。

在本节中，我们将简要探讨 Promise 的生产方。之后，我们将探讨消费 Promise 的工具。

### Promise 的状态

以下是 Promise 的三种状态：

-   Pending（进行中）：此过程表示的操作仍在进行中。
-   Fulfilled（已完成）：结果已准备好被检索。
-   Rejected（已拒绝）：发生了错误。我们可以访问一个错误值。

一个不再是 pending 状态的 Promise 被称为 *settled*（已确定）——它要么是 fulfilled，要么是 rejected。

### 创建一个 Promise

这是我们创建 Promise 的方法：

```javascript
new Promise(
  (resolve, reject) => {
    // 调用 resolve() 和/或 reject()
  }
);
```

`new Promise()` 的回调函数包含与 Promise 关联的代码。它可以调用其参数 `resolve()` 来完成新的 Promise，或调用 `reject()` 来拒绝它。

### 示例：等待 HTML 元素被点击

如果我们调用以下函数，它会返回一个 Promise，该 Promise 只有在提供的 HTML 元素被点击后才会被解析：

```javascript
const waitForClick = (elem) => {
  return new Promise(
    (resolve, reject) => {
      elem.addEventListener(
        'click',
        (event) => {
          event.preventDefault();
          resolve(undefined); // (A)
        },
        { once: true } // (B)
      );
    }
  );
};
```

返回的 Promise 在 A 行被完成之前一直处于 pending 状态。这个函数实际上没有结果，所以我们用 `undefined` 来完成它。省略参数也会有同样的效果。

在 B 行，我们使用 `.addEventListener()` 的第三个参数来告诉它我们的事件监听器只应该被调用一次。

### 同步函数与异步函数

比较同步和异步函数如何传递结果和错误可能会有所帮助。`setTimeout()` 被用作任何需要一些时间才能完成的操作的替代品。

在成功的情况下，它们会传递一个结果：

```javascript
const successSync = () => {
  return 123;
};
const successAsync = () => {
  return new Promise(
    (resolve, reject) => {
      setTimeout(
        () => resolve(123)
      );
    }
  );
};
```

在失败的情况下，它们会报告一个错误：

```javascript
const failureSync = () => {
  throw new Error();
};
const failureAsync = () => {
  return new Promise(
    (resolve, reject) => {
      setTimeout(
        () => reject(new Error())
      );
    }
  );
};
```

## 异步函数和 `await`：处理通过 Promise 传递的结果

我们可以直接使用 Promise，并通过回调接收完成和拒绝的值。但有一个更方便的替代方法：`async` 函数。这是一个例子：

```javascript
const asyncFunc = async () => {
  console.log('Before');
  const result = await functionThatReturnsAPromise();
  console.log('After');
};
```

当我们调用 `asyncFunc()` 时，它会记录 `'Before'` 然后返回。为什么？因为函数的执行通过 `await` 暂停了。一旦我们等待的 Promise 被确定，执行就会继续：

-   如果 Promise 被完成，完成值将存储在 `result` 中，函数将记录 `'After'`。
-   如果 Promise 被拒绝，将抛出异常，函数将提前终止（在记录 `'After'` 之前）。

通过 `await` 暂停也称为*挂起*其周围的函数。继续执行函数也称为*恢复*它。

### 通过 `Promise.resolve()` 和 `Promise.reject()` 创建 Promise

以下两种方法不常用，但它们可以让我们在 JavaScript 控制台中试验 `await`：

-   `Promise.resolve(v)` 创建一个已经用值 `v` 完成的 Promise。
-   `Promise.reject(e)` 创建一个已经用值 `e` 拒绝的 Promise。

让我们在控制台中使用这些方法：

```javascript
> await Promise.resolve('Success!')
'Success!'
> await Promise.reject(new Error('Failure'))
Error: Failure
```

第一个 `await` 产生一个值。第二个 `await` 抛出一个异常。

### 项目: `wait-for-click.html`

我们已经看到了 `waitForClick()` 的代码。项目 `wait-for-click.html` 使用 `await` 来处理它返回的 Promise。它具有以下用户界面：

```php-template
<p>
  <a href="">Click here!</a>
</p>
<p id="status"></p>
```

这是用户界面背后的 JavaScript：

```javascript
await waitForClick(document.querySelector('a'));
document.querySelector('#status')
  .innerText = 'Clicked';
```

以前，我们需要一个事件监听器来等待点击。在这种情况下，我们只需要 `await` 一个 Promise。

### 异步函数的结果

异步函数总是返回一个 Promise：

```javascript
> const asyncFunc = async () => {};
> asyncFunc() instanceof Promise
true
```

异步函数的结果 Promise 在其执行第一次暂停（`await`）或永久停止（`return`、`throw`）时返回。它通过两种方式确定：

-   `return v` 用值 `v` 完成结果 Promise。不返回任何东西等同于返回 `undefined`。
-   `throw e` 用值 `e` 拒绝结果 Promise。

### 异步函数在同步和异步之间转换

有趣的是，异步函数如何在同步代码和异步代码的世界之间进行转换：

-   在内部，异步函数是同步的：`await` 将 Promise（异步的东西）转换成一个值或一个异常（同步的东西）。
-   在外部，异步函数是异步的：一个（同步的）`return` 完成其结果 Promise，一个（同步的）`throw` 拒绝其结果 Promise。

### 如果我们省略 `await` 会发生什么？

让我们用下面的代码来探究一下，如果我们不使用 `await` 调用异步函数会发生什么：

```javascript
const logAfterOneSecond = () => {
  return new Promise(
    (resolve) => { // (A)
      console.log('Waiting for one second...');
      setTimeout(
        () => {
          console.log('Logged');
          resolve(undefined); // (B)
        },
        1000
      );
    }
  );
};
console.log('Before');
await logAfterOneSecond(); // (C)
console.log('After');
```

在 A 行，我们省略了第二个参数 `reject`，因为我们不需要它。

这是在 C 行*使用* `await` 时的输出：我们等到 `logAfterOneSecond()` 返回的 Promise 在 B 行被确定。

```rust
Before
Waiting for one second...
Logged
After
```

如果我们在 C 行删除 `await`，输出将如下所示：

```rust
Before
Waiting for one second...
After
Logged
```

那么发生了什么？

1.  异步函数总是同步启动：在当前任务内。
2.  但其结果总是异步传递的：在未来的任务中（记住，`await` 之后发生的事情在一个新任务中运行）。

没有 `await`，第 1 部分在 `'After'` 之前运行。但我们不等待第 2 部分，而是记录 `'After'`。然后第 2 部分发生。

### 异步代码具有传染性

异步函数 `f()` 有一个有趣的传染性：我们不能真正在没有 `await` 的情况下调用 `f()`。这意味着无论我们从哪里调用 `f()`，都必须是异步的。依此类推！

## Node.js 的异步 `fs` 函数

我们已经使用 `node:fs` 中的以下两个函数来读写文本文件：

-   `fs.readFileSync()`
-   `fs.writeFileSync()`

但是，这些函数也有异步版本（注意第一行中不同的模块名称）：

```python
import * as fs from 'node:fs/promises';

const str = await fs.readFile(filePath, 'utf-8');
await fs.writeFile(filePath, str);
```

我们为什么要使用这些版本？它们使用起来不太方便，但在工作时不会阻塞主线程。这种阻塞在 shell 命令中无关紧要，但如果我用 Node.js 实现一个 Web 服务器（我们将在未来的章节中这样做），这就很重要了。在那里，我们希望主线程不被阻塞，并准备好处理更多的传入请求。

## `fetch()`

`fetch()` 是一个让我们从网上下载数据的函数。我们这样使用它：

```vbnet
const response = await fetch(url);
const text = await response.text();
```

-   第一步: `fetch()` 异步地返回一个对应 `url` 文件的响应对象。
-   第二步: 响应对象的 `.text()` 方法异步地返回文件的内容（字符串格式）。

原则上，我们也可以将这两个步骤合并，但在我看来这样写有点丑：

```vbnet
const text = await ((await fetch(url)).text());
```

`response` 还有更多的方法 - 例如，`response.json()` 可以为我们解析文件中的 JSON，省去了我们在 `.text()` 之后需要做的额外步骤。

### 项目: `log-url-text.js`

shell 命令 `log-url-text.js` 如下所示：

```vbnet
const url = process.argv[2];
const response = await fetch(url);
const text = await response.text();
console.log(text);
```

我们这样调用它：

```perl
node log-url-text.js https://example.com
```

### 项目: `random-quote-browser/`

项目 `random-quote-browser` 是 `random-quote-nodejs` 项目的浏览器版本。我们现在有名言的以下 HTML 用户界面：

```php-template
<p>
  <button disabled>显示随机名言</button>
</p>
<div id="quote"></div>
<div id="author"></div>
```

这个函数从与 HTML 文件相邻的 JSON 文件加载名言：

```javascript
const loadQuotes = async () => {
  const quotesUrl = new URL('quotes.json', import.meta.url); // (A)
  const quotesResponse = await fetch(quotesUrl);
  const quotes = await quotesResponse.json();
  return quotes;
};
```

在 A 行，我们再次为当前文件的同级文件构造一个 URL 对象。然后我们使用 `fetch()` 下载同级文件。

这是该项目剩余的 JavaScript 代码：

```javascript
const quotes = await loadQuotes();
showQuoteButton.addEventListener(
  'click',
  () => {
    const randomIndex = getRandomInteger(quotes.length);
    const randomQuote = quotes[randomIndex];

    quoteElem.innerText = randomQuote.quote;
    authorElem.innerText = '— ' + randomQuote.author;
  }
);
showQuoteButton.disabled = false; // (π)
```

请注意，`<button>` 最初是禁用的。这确保了用户在一切准备就绪之前不会点击它。一旦准备就绪，我们就启用按钮（π 行）。我们这样做的主要原因是下载名言（第 1 行）可能需要一些时间。

## 练习（无答案）

-   `item-store.js`: 重写代码，使其使用模块 `'node:fs/promises'`。
    -   所有执行文件操作的函数都必须变成异步的（因为它们使用 `await` 来调用 `fs` 方法）。
    -   `main()` 也必须变成异步的。而且你必须在最后一行 `await` 它的调用。这就是我们之前谈到的传染性。
-   `flash-cards`: 将 `data.js` 替换为通过 `fetch()` 下载的 JSON 文件。
-   `log-time.js`: 创建此 shell 命令的 Web 版本。