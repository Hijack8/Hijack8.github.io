---
title: JavaScript进阶---二
date: 2022-01-18 23:58:47
tags: ["前端","JavaScript","异步","正则表达式"]
categories: 
- 前端
- JavaScript
---

让我们回归到js的实际功能上来。

看一段代码：

```js
function isUnitMatrix2d(m){
	return m[0] === 1 && m[1] === 0 && m[2] === 0 && m[3] === 1 && m[4] === 1 && m[5] === 0;
}
```

这段代码好不好？

这段代码是实际上在实现功能手写的，判断是不是单位矩阵，这个函数调用十分频繁，所以说我们要尽量减少计算时间，虽然说这个代码比较冗长，但是性能是最好的。

<!--more-->



## 交通灯 关于异步

### 版本一

```js
const traffic = document.getElementById('traffic');

(function reset(){
  traffic.className = 's1';
  
  setTimeout(function(){
      traffic.className = 's2';
      setTimeout(function(){
        traffic.className = 's3';
        setTimeout(function(){
          traffic.className = 's4';
          setTimeout(function(){
            traffic.className = 's5';
            setTimeout(reset, 1000)
          }, 1000)
        }, 1000)
      }, 1000)
  }, 1000);
})();
```

### 版本二

```js
const traffic = document.getElementById('traffic');

const stateList = [
  {state: 'wait', last: 1000},
  {state: 'stop', last: 3000},
  {state: 'pass', last: 3000},
];

function start(traffic, stateList){
  function applyState(stateIdx) {
    const {state, last} = stateList[stateIdx];
    traffic.className = state;
    setTimeout(() => {
      applyState((stateIdx + 1) % stateList.length);
    }, last)
  }
  applyState(0);
}

start(traffic, stateList);
```

### 版本三  过程抽象

```js
const traffic = document.getElementById('traffic');

function wait(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
//轮询方法
function poll(...fnList){
  let stateIndex = 0;
  
  return async function(...args){
    let fn = fnList[stateIndex++ % fnList.length];
    return await fn.apply(this, args);
  }
}

async function setState(state, ms){
  traffic.className = state;
  await wait(ms);
}

let trafficStatePoll = poll(setState.bind(null, 'wait', 1000),
                            setState.bind(null, 'stop', 3000),
                            setState.bind(null, 'pass', 3000));

(async function() {
  // noprotect
  while(1) {
    await trafficStatePoll();
  }
}());

```

### 版本四 容易理解 命令式

```js
const traffic = document.getElementById('traffic');
//等待
function wait(time){
  return new Promise(resolve => setTimeout(resolve, time));
}
//切换状态
function setState(state){
  traffic.className = state;
}

async function start(){
  //noprotect
  while(1){
    setState('wait');
    await wait(1000);
    setState('stop');
    await wait(3000);
    setState('pass');
    await wait(3000);
  }
}

start();
```

版本4最符合我们直接的理解和抽象，最符合我们的直觉，虽然版本二版本三都是抽象很好的，但是理解起来有成本。

## 判断是否为4的幂

### 最简单的方法

```js
function isPowerOfFour(num){
	num = parseInt(num);
    while(num > 1){
        if(num % 4)return false;
        num /= 4;
    }
    return false;
}
```

### 右移？ 与？

```js
function isPowerOfFour(num){
	num = parseInt(num);
    while(num > 1){
        if(num & 0b11)return false;
        num >>>= 4;
    }
    return false;
}
```

### 二进制数的判断方法    

```js
function isPowerOfFour(num){
  num = parseInt(num);
  
  return num > 0 &&
         (num & (num - 1)) === 0 &&
         (num & 0xAAAAAAAA) === 0;
}

num.addEventListener('input', function(){
  num.className = '';
});

checkBtn.addEventListener('click', function(){
  let value = num.value;
  num.className = isPowerOfFour(value) ? 'yes' : 'no';
});
```

### 正则表达式

```js
function isPowerOfFour(num){
  num = parseInt(num).toString(2);
  
  return /^1(?:00)*$/.test(num);
}
```



## 洗牌

```js
const cards = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

function shuffle(cards) {
  return [...cards].sort(() => Math.random() > 0.5 ? -1 : 1);
}

console.log(shuffle(cards));
```

### 洗牌 算法不正确

```js
const cards = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

function shuffle(cards) {
  return [...cards].sort(() => Math.random() > 0.5 ? -1 : 1);
}

const result = Array(10).fill(0);

for(let i = 0; i < 1000000; i++) {
  const c = shuffle(cards);
  for(let j = 0; j < 10; j++) {
    result[j] += c[j];
  }
}

console.log(result);
```

### 洗牌  正确的算法

抽一个放到第一个，剩余抽一个放第二个，剩余抽一个放第三个。。。

```js
const cards = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

function shuffle(cards) {
  const c = [...cards];
  for(let i = c.length; i > 0; i--) {
    const pIdx = Math.floor(Math.random() * i);
    [c[pIdx], c[i - 1]] = [c[i - 1], c[pIdx]];
  }
  return c;
}

const result = Array(10).fill(0);

for(let i = 0; i < 10000; i++) {
  const c = shuffle(cards);
  for(let j = 0; j < 10; j++) {
    result[j] += c[j];
  }
}

console.log(shuffle(cards));
console.log(result);
```

### 洗牌

```js
const cards = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

function * draw(cards){
    const c = [...cards];

  for(let i = c.length; i > 0; i--) {
    const pIdx = Math.floor(Math.random() * i);
    [c[pIdx], c[i - 1]] = [c[i - 1], c[pIdx]];
    yield c[i - 1];
  }
}

const result = draw(cards);
console.log([...result]);
```

## 分红包

```html
<h2>红包生成器</h2>
<div id="setting">
  <div><label>红包金额：<input id="amount" value=100.00></input></label></div>
  <div><label>红包数量：<input id="count" value="10"></input></label></div>
  <div><button id="generateBtn">随机生成</button></div>
</div>

<ul id="result">
</ul>
```

```css
#setting button {
  margin-top: 10px;
}

#result {
  padding: 0;
}

#result li {
  list-style: none;
}
```

### 切西瓜的方式，找大的切

```js
function generate(amount, count){
  let ret = [amount];
  
  while(count > 1){
    //挑选出最大一块进行切分
    let cake = Math.max(...ret),
        idx = ret.indexOf(cake),
        part = 1 + Math.floor((cake / 2) * Math.random()),
        rest = cake - part;
    
    ret.splice(idx, 1, part, rest);
    
    count--;
  }
  return ret;
}

const amountEl = document.getElementById('amount');
const countEl = document.getElementById('count');
const generateBtn = document.getElementById('generateBtn');
const resultEl = document.getElementById('result');

generateBtn.onclick = function(){
  let amount = Math.round(parseFloat(amountEl.value) * 100);
  let count = parseInt(countEl.value);
  
  let output = [];
  
  if(isNaN(amount) || isNaN(count) 
     || amount <= 0 || count <= 0){
    output.push('输入格式不正确！');
  }else if(amount < count){
    output.push('钱不够分')
  }else{
    output.push(...generate(amount, count));
    output = output.map(m => (m / 100).toFixed(2));
  }
  resultEl.innerHTML = '<li>' + 
                        output.join('</li><li>') +
                       '</li>';
}


```

### 抽牌的方式

```js
function * draw(cards){
  const c = [...cards];

  for(let i = c.length; i > 0; i--) {
    const pIdx = Math.floor(Math.random() * i);
    [c[pIdx], c[i - 1]] = [c[i - 1], c[pIdx]];
    yield c[i - 1];
  }
}

function generate(amount, count){
  if(count <= 1) return [amount];
  const cards = Array(amount - 1).fill(0).map((_, i) => i + 1);
  const pick = draw(cards);
  const result = [];
  for(let i = 0; i < count; i++) {
    result.push(pick.next().value);
  }
  result.sort((a, b) => a - b);
  for(let i = count - 1; i > 0; i--) {
    result[i] = result[i] - result[i - 1];
  }
  return result;
}

const amountEl = document.getElementById('amount');
const countEl = document.getElementById('count');
const generateBtn = document.getElementById('generateBtn');
const resultEl = document.getElementById('result');

generateBtn.onclick = function(){
  let amount = Math.round(parseFloat(amountEl.value) * 100);
  let count = parseInt(countEl.value);
  
  let output = [];
  
  if(isNaN(amount) || isNaN(count) 
     || amount <= 0 || count <= 0){
    output.push('输入格式不正确！');
  }else if(amount < count){
    output.push('钱不够分')
  }else{
    output.push(...generate(amount, count));
    output = output.map(m => (m / 100).toFixed(2));
  }
  resultEl.innerHTML = '<li>' + 
                        output.join('</li><li>') +
                       '</li>';
}
```

