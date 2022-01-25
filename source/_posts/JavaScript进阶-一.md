---
title: JavaScript进阶---一
date: 2022-01-18 23:58:41
tags: ["JavaSript","前端","过程抽象","高阶函数"]
categories: 
- 前端
- JavaScript
---

JavaScript进阶教学一

这里比较适合有JavaScript基础的进行学习，其中包括了写好JS的三大原则。

<!--more-->

## 写好JS的原则

* 各司其责
* 组件封装
* 过程抽象

## 各司其责

![image-20220117091901386](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117091901386.png)

清晰的三层结构。

![image-20220117092744835](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117092744835.png)

```js
const btn = document.getElementById('modeBtn');
btn.addEventListener('click', (e) => {
  const body = document.body;
  if(e.target.innerHTML === '🌞') {
    body.style.backgroundColor = 'black';
    body.style.color = 'white';
    e.target.innerHTML = '🌜';
  } else {
    body.style.backgroundColor = 'white';
    body.style.color = 'black';
    e.target.innerHTML = '🌞';
  }
});
```

这是一个实现点击按钮实现深色模式的需求。

有什么问题呢？

* 这里在JS中时直接操作了CSS的style从而实现深色模式的需求，这样是不好的。

![image-20220117092933224](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117092933224.png)

```js
const btn = document.getElementById('modeBtn');
btn.addEventListener('click', (e) => {
  const body = document.body;
  if(body.className !== 'night') {
    body.className = 'night';
  } else {
    body.className = '';
  }
});
```

再来看版本二，这样我们在阅读代码时候很容易就知道他是在操作body的两种状态。这样结构会更加清晰。

那还能再简化吗？

我们尝试使用html和CSS来直接实现这个功能。

我们知道通过checkbox有两种状态，那么通过状态选择器每种状态就可以显示不同的页面，进一步通过label标签的for功能将对应按钮的点击直接输入到CheckBox，这样我们就不使用JS实现了这个功能，然后再将CheckBox隐藏即可。

![image-20220117094025499](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117094025499.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>深夜食堂</title>
</head>
<body>
  <input id="modeCheckBox" type="checkbox">
  <div class="content">
    <header>
      <label id="modeBtn" for="modeCheckBox"></label>
      <h1>深夜食堂</h1>
    </header>
    <main>
      <div class="pic">
        <img src="https://p2.ssl.qhimg.com/t0120cc20854dc91c1e.jpg">
      </div>
      <div class="description">
        <p>
            这是一间营业时间从午夜十二点到早上七点的特殊食堂。这里的老板，不太爱说话，却总叫人吃得热泪盈
            眶。在这里，自卑的舞蹈演员偶遇隐退多年舞界前辈，前辈不惜讲述自己不堪回首的经历不断鼓舞年轻人，最终令其重拾自信；轻言绝交的闺蜜因为吃到共同喜爱的美食，回忆起从前的友谊，重归于好；乐观的绝症患者遇到同命相连的女孩，两人相爱并相互给予力量，陪伴彼此完美地走过了最后一程；一味追求事业成功的白领，在这里结交了真正暖心的朋友，发现真情比成功更有意义。食物、故事、真情，汇聚了整部剧的主题，教会人们坦然面对得失，对生活充满期许和热情。每一个故事背后都饱含深情，情节跌宕起伏，令人流连忘返 [6]  。
        </p>
      </div>
    </main>
  </div>
</body>
</html>
```

```css
body, html {
  width: 100%;
  height: 100%;
  max-width: 600px;
  padding: 0;
  margin: 0;
  overflow: hidden;
}

body {
  box-sizing: border-box;
}

.content {
  padding: 10px;
  transition: background-color 1s, color 1s;
}

div.pic img {
  width: 100%;
}

#modeCheckBox {
  display: none;
}

#modeCheckBox:checked + .content {
  background-color: black;
  color: white;
  transition: all 1s;
}

#modeBtn {
  font-size: 2rem;
  float: right;
}

#modeBtn::after {
  content: '🌞';
}

#modeCheckBox:checked + .content #modeBtn::after {
  content: '🌜';
}
```



可以看到版本三实现的更加简单。

总结如下：

* html/css/js各司其责
* 避免js直接操作样式
* 可以使用class表示状态，js来切换状态。
* 纯展示类交互寻求零JS方案。

## 组件封装

例子：

使用JS写一个轮播图，应该怎么实现呢？

![image-20220117094616817](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117094616817.png)

>组件是Web页面上抽出来的一个个包含模板功能样式的单元，好的组件具备封装性，正确性扩展性，复用性。

### 结构HTML

轮播图是一个典型的列表结构。

我们使用无序列表实现：

```html
<div id="my-slider" class="slider-list">
  <ul>
    <li class="slider-list__item--selected">
      <img src="https://p5.ssl.qhimg.com/t0119c74624763dd070.png"/>
    </li>
    <li class="slider-list__item">
      <img src="https://p4.ssl.qhimg.com/t01adbe3351db853eb3.jpg"/>
    </li>
    <li class="slider-list__item">
      <img src="https://p2.ssl.qhimg.com/t01645cd5ba0c3b60cb.jpg"/>
    </li>
    <li class="slider-list__item">
      <img src="https://p4.ssl.qhimg.com/t01331ac159b58f5478.jpg"/>
    </li>
  </ul>
  <a class="slide-list__next"></a>
  <a class="slide-list__previous"></a>
  <div class="slide-list__control">
    <span class="slide-list__control-buttons--selected"></span>
    <span class="slide-list__control-buttons"></span>
    <span class="slide-list__control-buttons"></span>
    <span class="slide-list__control-buttons"></span>
  </div>
</div>
```



![image-20220117095015247](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117095015247.png)

### 表现CSS

```css
#my-slider{
  position: relative;
  width: 790px;
  height: 340px;
}

.slider-list ul{
  list-style-type:none;
  position: relative;
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}

.slider-list__item,
.slider-list__item--selected{
  position: absolute;
  transition: opacity 1s;
  opacity: 0;
  text-align: center;
}

.slider-list__item--selected{
  transition: opacity 1s;
  opacity: 1;
}

.slide-list__control{
  position: relative;
  display: table;
  background-color: rgba(255, 255, 255, 0.5);
  padding: 5px;
  border-radius: 12px;
  bottom: 30px;
  margin: auto;
}

.slide-list__next,
.slide-list__previous{
  display: inline-block;
  position: absolute;
  top: 50%;
  margin-top: -25px;
  width: 30px;
  height:50px;
  text-align: center;
  font-size: 24px;
  line-height: 50px;
  overflow: hidden;
  border: none;
  background: transparent;
  color: white;
  background: rgba(0,0,0,0.2);
  cursor: pointer;
  opacity: 0;
  transition: opacity .5s;
}

.slide-list__previous {
  left: 0;
}

.slide-list__next {
  right: 0;
}

#my-slider:hover .slide-list__previous {
  opacity: 1;
}


#my-slider:hover .slide-list__next {
  opacity: 1;
}

.slide-list__previous:after {
  content: '<';
}

.slide-list__next:after {
  content: '>';
}

.slide-list__control-buttons,
.slide-list__control-buttons--selected{
  display: inline-block;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  margin: 0 5px;
  background-color: white;
  cursor: pointer;
}

.slide-list__control-buttons--selected {
  background-color: red;
}

```



![image-20220117141644550](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117141644550.png)

![image-20220117095050181](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117095050181.png)

### 行为API

```javascript
class Slider{
  constructor(id, cycle = 3000){
    this.container = document.getElementById(id);
    this.items = this.container.querySelectorAll('.slider-list__item, .slider-list__item--selected');
    this.cycle = cycle;

    const controller = this.container.querySelector('.slide-list__control');
    if(controller){
      const buttons = controller.querySelectorAll('.slide-list__control-buttons, .slide-list__control-buttons--selected');
      controller.addEventListener('mouseover', evt=>{
        const idx = Array.from(buttons).indexOf(evt.target);
        if(idx >= 0){
          this.slideTo(idx);
          this.stop();
        }
      });
      
      controller.addEventListener('mouseout', evt=>{
        this.start();
      });
/*
getSelectedItem 返回第一个slider-list__item--selected元素
getSelectedItemIndex返回上述元素的列表序号
slideTo(idx)将已经选中的那个元素取消选中  将输入的idx的元素设置为选中状态。
slideNext 得到下一个idx的地址
slidePrevious   得到前一个idx的地址*/
      this.container.addEventListener('slide', evt => {
        const idx = evt.detail.index
        const selected = controller.querySelector('.slide-list__control-buttons--selected');
        if(selected) selected.className = 'slide-list__control-buttons';
        buttons[idx].className = 'slide-list__control-buttons--selected';
      })
    }
    
    const previous = this.container.querySelector('.slide-list__previous');
    if(previous){
      previous.addEventListener('click', evt => {
        this.stop();
        this.slidePrevious();
        this.start();
        evt.preventDefault();
      });
    }
    
    const next = this.container.querySelector('.slide-list__next');
    if(next){
      next.addEventListener('click', evt => {
        this.stop();
        this.slideNext();
        this.start();
        evt.preventDefault();
      });
    }
  }
  getSelectedItem(){
    let selected = this.container.querySelector('.slider-list__item--selected');
    return selected
  }
  getSelectedItemIndex(){
    return Array.from(this.items).indexOf(this.getSelectedItem());
  }
  slideTo(idx){
    let selected = this.getSelectedItem();
    if(selected){ 
      selected.className = 'slider-list__item';
    }
    let item = this.items[idx];
    if(item){
      item.className = 'slider-list__item--selected';
    }
  //这里通过自定义事件来解除耦合
    const detail = {index: idx}
    const event = new CustomEvent('slide', {bubbles:true, detail})
    this.container.dispatchEvent(event)
  }
  slideNext(){
    let currentIdx = this.getSelectedItemIndex();
    let nextIdx = (currentIdx + 1) % this.items.length;
    this.slideTo(nextIdx);
  }
  slidePrevious(){
    let currentIdx = this.getSelectedItemIndex();
    let previousIdx = (this.items.length + currentIdx - 1) % this.items.length;
    this.slideTo(previousIdx);  
  }
  start(){
    this.stop();
    this._timer = setInterval(()=>this.slideNext(), this.cycle);
  }
  stop(){
    clearInterval(this._timer);
  }
}

const slider = new Slider('my-slider');
slider.start();
```

>## JS HTML DOM
>
>HTML DOM实际上就是可以使用JavaScript使用的一系列方法。
>
>
>
>**HTML DOM 方法是您能够（在 HTML 元素上）执行的动作。**
>
>**HTML DOM 属性是您能够设置或改变的 HTML 元素的值。**
>
>
>
>### 查找HTML元素
>
>- 通过 id 查找 HTML 元素
>- 通过标签名查找 HTML 元素
>- 通过类名查找 HTML 元素
>- 通过 CSS 选择器查找 HTML 元素
>- 通过 HTML 对象集合查找 HTML 元素
>
>### querySelectorAll
>
>```javascript
>var x = document.querySelectorAll(".example");
>```
>
>内部使用CSS选择器样式。
>
>### querySelector
>
>与querySelectorAll类似，但是仅仅返回第一个元素。





![image-20220117095122682](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117095122682.png)



![image-20220117095159902](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117095159902.png)

### 行为 控制流

>JavaScript中任何事物都是对象，比如字符串、数值、数组。
>
>此外JavaScript允许自定义对象。
>
>* 使用键值对
>
>```javascript
>const a = {name : "大帅"}
>```
>
>* 使用object
>
>```javascript
>person=new Object();
>person.firstname="John";
>person.lastname="Doe";
>person.age=50;
>person.eyecolor="blue";
>```

使用**自定义事件来解耦。**

> `CustomEvent `事件是由程序创建的，可以有任意自定义功能的事件。
>
> #### 构造函数CustomEvent
>
> ```
> type
> ```
>
> 事件的类型名称。
>
> ```
> eventInitDict
> ```
>
> 一个提供事件的配置信息对象。查看[CustomEventInit](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomEvent#customeventinit)了解更多详情。
>
> #### CustomEventInit
>
> - `bubbles`
>
>   一个布尔值,表明该事件是否会冒泡。
>
> - `cancelable`
>
>   一个布尔值,表明该事件是否可以被取消。
>
> - `detail`
>
>   当事件初始化时传递的数据。
>
> 
>
> 向一个指定的事件目标派发一个[事件](https://developer.mozilla.org/zh-CN/docs/Web/API/Event), 并以合适的顺序**同步调用**目标元素相关的事件处理函数。标准事件处理规则(包括事件捕获和可选的冒泡过程)同样适用于通过手动的使用`dispatchEvent()`方法派发的事件

我们希望下面的四个小原点与左右按钮是没有联系没有耦合的。

### 改进空间

重构轮播图，优化扩展性和复用性。

### 解耦

* 将控制元素抽取成插件
* 插件与组件之间通过**依赖注入**方式建立联系

之前的版本构造器Slider很大，一般来说一个方法不应该超过20行代码。所以说应该要进行重构。

* 将HTML模板化，更易于扩展。

![image-20220117144359210](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117144359210.png)

![image-20220117144414671](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117144414671.png)

### 抽象 

将通用的组件模型抽象出来

![image-20220117144528360](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117144528360.png)

![image-20220117144634272](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117144634272.png)

```javascript
class Slider{
  constructor(id, cycle = 3000){
    this.container = document.getElementById(id);
    this.items = this.container.querySelectorAll('.slider-list__item, .slider-list__item--selected');
    this.cycle = cycle;
  }
  registerPlugins(...plugins){
    plugins.forEach(plugin => plugin(this));
  }
  getSelectedItem(){
    const selected = this.container.querySelector('.slider-list__item--selected');
    return selected
  }
  getSelectedItemIndex(){
    return Array.from(this.items).indexOf(this.getSelectedItem());
  }
  slideTo(idx){
    const selected = this.getSelectedItem();
    if(selected){ 
      selected.className = 'slider-list__item';
    }
    const item = this.items[idx];
    if(item){
      item.className = 'slider-list__item--selected';
    }

    const detail = {index: idx}
    const event = new CustomEvent('slide', {bubbles:true, detail})
    this.container.dispatchEvent(event)
  }
  slideNext(){
    const currentIdx = this.getSelectedItemIndex();
    const nextIdx = (currentIdx + 1) % this.items.length;
    this.slideTo(nextIdx);
  }
  slidePrevious(){
    const currentIdx = this.getSelectedItemIndex();
    const previousIdx = (this.items.length + currentIdx - 1) % this.items.length;
    this.slideTo(previousIdx);  
  }
  addEventListener(type, handler){
    this.container.addEventListener(type, handler)
  }
  start(){
    this.stop();
    this._timer = setInterval(()=>this.slideNext(), this.cycle);
  }
  stop(){
    clearInterval(this._timer);
  }
}

function pluginController(slider){
  const controller = slider.container.querySelector('.slide-list__control');
  if(controller){
    const buttons = controller.querySelectorAll('.slide-list__control-buttons, .slide-list__control-buttons--selected');
    controller.addEventListener('mouseover', evt=>{
      const idx = Array.from(buttons).indexOf(evt.target);
      if(idx >= 0){
        slider.slideTo(idx);
        slider.stop();
      }
    });

    controller.addEventListener('mouseout', evt=>{
      slider.start();
    });

    slider.addEventListener('slide', evt => {
      const idx = evt.detail.index
      const selected = controller.querySelector('.slide-list__control-buttons--selected');
      if(selected) selected.className = 'slide-list__control-buttons';
      buttons[idx].className = 'slide-list__control-buttons--selected';
    });
  }  
}

function pluginPrevious(slider){
  const previous = slider.container.querySelector('.slide-list__previous');
  if(previous){
    previous.addEventListener('click', evt => {
      slider.stop();
      slider.slidePrevious();
      slider.start();
      evt.preventDefault();
    });
  }  
}

function pluginNext(slider){
  const next = slider.container.querySelector('.slide-list__next');
  if(next){
    next.addEventListener('click', evt => {
      slider.stop();
      slider.slideNext();
      slider.start();
      evt.preventDefault();
    });
  }  
}


const slider = new Slider('my-slider');
slider.registerPlugins(pluginController, pluginPrevious, pluginNext);
slider.start();
```

进一步模板化

```html
<div id="my-slider" class="slider-list"></div>
```

```javascript
class Slider{
  constructor(id, opts = {images:[], cycle: 3000}){
    this.container = document.getElementById(id);
    this.options = opts;
    this.container.innerHTML = this.render();
    this.items = this.container.querySelectorAll('.slider-list__item, .slider-list__item--selected');
    this.cycle = opts.cycle || 3000;
    this.slideTo(0);
  }
  render(){
    const images = this.options.images;
    const content = images.map(image => `
      <li class="slider-list__item">
        <img src="${image}"/>
      </li>    
    `.trim());
    
    return `<ul>${content.join('')}</ul>`;
  }
  registerPlugins(...plugins){
    plugins.forEach(plugin => {
      const pluginContainer = document.createElement('div');
      pluginContainer.className = '.slider-list__plugin';
      pluginContainer.innerHTML = plugin.render(this.options.images);
      this.container.appendChild(pluginContainer);
      
      plugin.action(this);
    });
  }
  getSelectedItem(){
    const selected = this.container.querySelector('.slider-list__item--selected');
    return selected
  }
  getSelectedItemIndex(){
    return Array.from(this.items).indexOf(this.getSelectedItem());
  }
  slideTo(idx){
    const selected = this.getSelectedItem();
    if(selected){ 
      selected.className = 'slider-list__item';
    }
    let item = this.items[idx];
    if(item){
      item.className = 'slider-list__item--selected';
    }
    
    const detail = {index: idx}
    const event = new CustomEvent('slide', {bubbles:true, detail})
    this.container.dispatchEvent(event)
  }
  slideNext(){
    const currentIdx = this.getSelectedItemIndex();
    const nextIdx = (currentIdx + 1) % this.items.length;
    this.slideTo(nextIdx);
  }
  slidePrevious(){
    const currentIdx = this.getSelectedItemIndex();
    const previousIdx = (this.items.length + currentIdx - 1) % this.items.length;
    this.slideTo(previousIdx);  
  }
  addEventListener(type, handler){
    this.container.addEventListener(type, handler);
  }
  start(){
    this.stop();
    this._timer = setInterval(()=>this.slideNext(), this.cycle);
  }
  stop(){
    clearInterval(this._timer);
  }
}

const pluginController = {
  render(images){
    return `
      <div class="slide-list__control">
        ${images.map((image, i) => `
            <span class="slide-list__control-buttons${i===0?'--selected':''}"></span>
         `).join('')}
      </div>    
    `.trim();
  },
  action(slider){
    const controller = slider.container.querySelector('.slide-list__control');
    
    if(controller){
      const buttons = controller.querySelectorAll('.slide-list__control-buttons, .slide-list__control-buttons--selected');
      controller.addEventListener('mouseover', evt => {
        const idx = Array.from(buttons).indexOf(evt.target);
        if(idx >= 0){
          slider.slideTo(idx);
          slider.stop();
        }
      });

      controller.addEventListener('mouseout', evt => {
        slider.start();
      });

      slider.addEventListener('slide', evt => {
        const idx = evt.detail.index
        const selected = controller.querySelector('.slide-list__control-buttons--selected');
        if(selected) selected.className = 'slide-list__control-buttons';
        buttons[idx].className = 'slide-list__control-buttons--selected';
      });
    }    
  }
};

const pluginPrevious = {
  render(){
    return `<a class="slide-list__previous"></a>`;
  },
  action(slider){
    const previous = slider.container.querySelector('.slide-list__previous');
    if(previous){
      previous.addEventListener('click', evt => {
        slider.stop();
        slider.slidePrevious();
        slider.start();
        evt.preventDefault();
      });
    }  
  }
};

const pluginNext = {
  render(){
    return `<a class="slide-list__next"></a>`;
  },
  action(slider){
    const previous = slider.container.querySelector('.slide-list__next');
    if(previous){
      previous.addEventListener('click', evt => {
        slider.stop();
        slider.slideNext();
        slider.start();
        evt.preventDefault();
      });
    }  
  }
};

const slider = new Slider('my-slider', {images: ['https://p5.ssl.qhimg.com/t0119c74624763dd070.png',
     'https://p4.ssl.qhimg.com/t01adbe3351db853eb3.jpg',
     'https://p2.ssl.qhimg.com/t01645cd5ba0c3b60cb.jpg',
     'https://p4.ssl.qhimg.com/t01331ac159b58f5478.jpg'], cycle:3000});

slider.registerPlugins(pluginController, pluginPrevious, pluginNext);
slider.start();
```

可以看到模板化之后html文档就没有了，在JS文档中，我们有render方法去做渲染，有action方法去做逻辑。。。

这样直接将所有东西嵌入到js中实现。容易进行扩展。

进一步组件化实现。

```javascript
class Component{
  constructor(id, opts = {name, data:[]}){
    this.container = document.getElementById(id);
    this.options = opts;
    this.container.innerHTML = this.render(opts.data);
  }
  registerPlugins(...plugins){
    plugins.forEach(plugin => {
      const pluginContainer = document.createElement('div');
      pluginContainer.className = `.${name}__plugin`;
      pluginContainer.innerHTML = plugin.render(this.options.data);
      this.container.appendChild(pluginContainer);
      
      plugin.action(this);
    });
  }
  render(data) {
    /* abstract */
    return ''
  }
}

class Slider extends Component{
  constructor(id, opts = {name: 'slider-list', data:[], cycle: 3000}){
    super(id, opts);
    this.items = this.container.querySelectorAll('.slider-list__item, .slider-list__item--selected');
    this.cycle = opts.cycle || 3000;
    this.slideTo(0);
  }
  render(data){
    const content = data.map(image => `
      <li class="slider-list__item">
        <img src="${image}"/>
      </li>    
    `.trim());
    
    return `<ul>${content.join('')}</ul>`;
  }
  getSelectedItem(){
    const selected = this.container.querySelector('.slider-list__item--selected');
    return selected
  }
  getSelectedItemIndex(){
    return Array.from(this.items).indexOf(this.getSelectedItem());
  }
  slideTo(idx){
    const selected = this.getSelectedItem();
    if(selected){ 
      selected.className = 'slider-list__item';
    }
    const item = this.items[idx];
    if(item){
      item.className = 'slider-list__item--selected';
    }

    const detail = {index: idx}
    const event = new CustomEvent('slide', {bubbles:true, detail})
    this.container.dispatchEvent(event)
  }
  slideNext(){
    const currentIdx = this.getSelectedItemIndex();
    const nextIdx = (currentIdx + 1) % this.items.length;
    this.slideTo(nextIdx);
  }
  slidePrevious(){
    const currentIdx = this.getSelectedItemIndex();
    const previousIdx = (this.items.length + currentIdx - 1) % this.items.length;
    this.slideTo(previousIdx);  
  }
  addEventListener(type, handler){
    this.container.addEventListener(type, handler);
  }
  start(){
    this.stop();
    this._timer = setInterval(()=>this.slideNext(), this.cycle);
  }
  stop(){
    clearInterval(this._timer);
  }
}

const pluginController = {
  render(images){
    return `
      <div class="slide-list__control">
        ${images.map((image, i) => `
            <span class="slide-list__control-buttons${i===0?'--selected':''}"></span>
         `).join('')}
      </div>    
    `.trim();
  },
  action(slider){
    let controller = slider.container.querySelector('.slide-list__control');
    
    if(controller){
      let buttons = controller.querySelectorAll('.slide-list__control-buttons, .slide-list__control-buttons--selected');
      controller.addEventListener('mouseover', evt=>{
        var idx = Array.from(buttons).indexOf(evt.target);
        if(idx >= 0){
          slider.slideTo(idx);
          slider.stop();
        }
      });

      controller.addEventListener('mouseout', evt=>{
        slider.start();
      });

      slider.addEventListener('slide', evt => {
        const idx = evt.detail.index;
        let selected = controller.querySelector('.slide-list__control-buttons--selected');
        if(selected) selected.className = 'slide-list__control-buttons';
        buttons[idx].className = 'slide-list__control-buttons--selected';
      });
    }    
  }
};

const pluginPrevious = {
  render(){
    return `<a class="slide-list__previous"></a>`;
  },
  action(slider){
    let previous = slider.container.querySelector('.slide-list__previous');
    if(previous){
      previous.addEventListener('click', evt => {
        slider.stop();
        slider.slidePrevious();
        slider.start();
        evt.preventDefault();
      });
    }  
  }
};

const pluginNext = {
  render(){
    return `<a class="slide-list__next"></a>`;
  },
  action(slider){
    let previous = slider.container.querySelector('.slide-list__next');
    if(previous){
      previous.addEventListener('click', evt => {
        slider.stop();
        slider.slideNext();
        slider.start();
        evt.preventDefault();
      });
    }  
  }
};

const slider = new Slider('my-slider', {name: 'slide-list', data: ['https://p5.ssl.qhimg.com/t0119c74624763dd070.png',
     'https://p4.ssl.qhimg.com/t01adbe3351db853eb3.jpg',
     'https://p2.ssl.qhimg.com/t01645cd5ba0c3b60cb.jpg',
     'https://p4.ssl.qhimg.com/t01331ac159b58f5478.jpg'], cycle:3000});

slider.registerPlugins(pluginController, pluginPrevious, pluginNext);
slider.start();
```

## 操作次数限制

* 一些异步交互
* 一次性的HTTP请求

![image-20220117145717140](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117145717140.png)

![image-20220117145759451](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117145759451.png)

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
  <ul>
    <li><button></button><span>任务一：学习HTML</span></li>
    <li><button></button><span>任务二：学习CSS</span></li>
    <li><button></button><span>任务三：学习JavaScript</span></li>
  </ul>
</body>
</html>
```

```css
ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

li button {
  border: 0;
  background: transparent;
  cursor: pointer;
  outline: 0 none;
}

li.completed {
  transition: opacity 2s;
  opacity: 0;
}

li button:before {
  content: '☑️';
}

li.completed button:before {
  content: '✅';
}

```

```javascript
function once(fn) {
  return function(...args) {
    if(fn) {
      const ret = fn.apply(this, args);
      fn = null;
      return ret;
    }
  }
}

const list = document.querySelector('ul');
const buttons = list.querySelectorAll('button');
buttons.forEach((button) => {
  button.addEventListener('click', once((evt) => {
    const target = evt.target;
    target.parentNode.className = 'completed';
    setTimeout(() => {
      list.removeChild(target.parentNode);
    }, 2000);
  }));
});
//注意once的调用方法
const foo = once(() => {
  console.log('bar');
});

foo();
foo();
foo();
```

Once为了能够让"只执行一次"的需求覆盖不同的事件处理，我们将这个需求剥离出来，这个过程我们称之为**过程抽象**。

这里Once为**高阶函数**

![image-20220117150104229](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117150104229.png)

等价装饰器 等价范式，实际上其他的高阶函数都是等价范式的扩展。

![image-20220117222319743](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117222319743.png)

![image-20220117222406111](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220117222406111.png)

### 节流函数throttle

```html
每500毫秒可记录一次

<button id="btn">点我</button>

<div id="circle">0</div>
```

```css
#circle {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: red;
  line-height: 50px;
  text-align: center;
  color: white;
  opacity: 1.0;
  transition: opacity .25s;
}

#circle.fade {
  opacity: 0.0;
  transition: opacity .25s;
}
```

```javascript
function throttle(fn, time = 500){
  let timer;
  return function(...args){
    if(timer == null){
      fn.apply(this,  args);
      timer = setTimeout(() => {
        timer = null;
      }, time)
    }
  }
}
//这就是一个高阶函数
//限制没500ms执行一次函数  节流函数
btn.onclick = throttle(function(e){
  circle.innerHTML = parseInt(circle.innerHTML) + 1;
  circle.className = 'fade';
  setTimeout(() => circle.className = '', 250);
});
```

### 防抖函数Debounce

```html
<script src="https://s1.qhres2.com/!bd39e7fb/animator-0.2.0.min.js"></script>
<div id="bird" class="sprite bird1"></div>
```

```css
html, body {
  margin:0;
  padding:0;
}

.sprite {
  display:inline-block; overflow:hidden; 
  background-repeat: no-repeat;
  background-image:url(https://p1.ssl.qhimg.com/d/inn/0f86ff2a/8PQEganHkhynPxk-CUyDcJEk.png);
}

.bird0 {width:86px; height:60px; background-position: -178px -2px}
.bird1 {width:86px; height:60px; background-position: -90px -2px}
.bird2 {width:86px; height:60px; background-position: -2px -2px}

#bird{
  position: absolute;
  left: 100px;
  top: 100px;
  transform: scale(0.5);
  transform-origin: -50% -50%;
}
```

```javascript
var i = 0;
setInterval(function(){
  bird.className = "sprite " + 'bird' + ((i++) % 3);
}, 1000/10);

function debounce(fn, dur){
  dur = dur || 100;
  var timer;
  return function(){
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, arguments);
    }, dur);//这里是通过timer进行时间的计算的。
  }
}

document.addEventListener('mousemove', debounce(function(evt){
  var x = evt.clientX,
      y = evt.clientY,
      x0 = bird.offsetLeft,
      y0 = bird.offsetTop;
  
  console.log(x, y);
  
  var a1 = new Animator(1000, function(ep){
    bird.style.top = y0 + ep * (y - y0) + 'px';
    bird.style.left = x0 + ep * (x - x0) + 'px';
  }, p => p * p);
  
  a1.animate();
}, 100));
```

### Consumer

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>

</body>
</html>
```

```javascript
function consumer(fn, time){
  let tasks = [],
      timer;
  
  return function(...args){
    tasks.push(fn.bind(this, ...args));
    if(timer == null){
      timer = setInterval(() => {
        tasks.shift().call(this)
        if(tasks.length <= 0){
          clearInterval(timer);
          timer = null;
        }
      }, time)
    }
  }
}

function add(ref, x){
  const v = ref.value + x;
  console.log(`${ref.value} + ${x} = ${v}`);
  ref.value = v;
  return ref;
}

let consumerAdd = consumer(add, 1000);

const ref = {value: 0};
for(let i = 0; i < 10; i++){
  consumerAdd(ref, i);
}
//每隔1s出现一次
```

### Consumer2

```html
<div id="main">
  <button id="btn">Hit</button>
  <span id="count">+0</span>
</div>
```

```css
#main {
  padding-top: 20px;
  font-size: 26px;
}

#btn {
  font-size: 30px;
  border-radius: 15px;
  border: solid 3px #fa0;
}

#count {
  position: absolute;
  margin-left: 6px;
  opacity: 1.0;
  transform: translate(0, 10px);
}

#count.hit {
  opacity: 0.1;
  transform: translate(0, -20px);
  transition: all .5s;
}
```

```javascript
function consumer(fn, time){
  let tasks = [],
      timer;
  
  return function(...args){
    tasks.push(fn.bind(this, ...args));
    if(timer == null){
      timer = setInterval(() => {
        tasks.shift().call(this)
        if(tasks.length <= 0){
          clearInterval(timer);
          timer = null;
        }
      }, time)
    }
  }
}

btn.onclick = consumer((evt)=>{
  let t = parseInt(count.innerHTML.slice(1)) + 1;
  count.innerHTML = `+${t}`;
  count.className = 'hit';
  let r = t * 7 % 256,
      g = t * 17 % 128,
      b = t * 31 % 128;
  
  count.style.color = `rgb(${r},${g},${b})`.trim();
  setTimeout(()=>{
    count.className = 'hide';
  }, 500);
}, 800)

```

## 为什么要使用高阶函数?

纯函数和非纯函数

* 纯函数   容易做测试

```js
function add(x,y){
	return x + y;
}

add(1,2);//3
```

* 非纯函数  以html元素作为参数

```js
function setColors(els,color){
	for(const el of els){
		el.style.color = color;
	}
}
```

高阶函数是一个纯函数，通过高阶函数生成的非纯函数，可以更简单地通过测试。

因为我只要保证高阶函数正确，我就可以保证生成的函数正确。



## 混合的编程范式

![image-20220118224942870](JavaScript%E8%BF%9B%E9%98%B6-%E4%B8%80/image-20220118224942870.png)

js带有命令式和声明式代码。

这两种实际上没有优劣之分。

###  命令式

```js
switcher.onclick = function(evt){
  if(evt.target.className === 'on'){
    evt.target.className = 'off';
  }else{
    evt.target.className = 'on';
  }
}
```

### 声明式

```js
//定义toggle  注重写toggle是什么
function toggle(...actions){
  return function(...args){
    let action = actions.shift();
    actions.push(action);
    return action.apply(this, args);
  }
}

switcher.onclick = toggle(
  evt => evt.target.className = 'off',
  evt => evt.target.className = 'on'
);
```

所有的抽象都是为了提升可扩展性。

### 三态  声明式实现的

```js
function toggle(...actions){
  return function(...args){
    let action = actions.shift();
    actions.push(action);
    return action.apply(this, args);
  }
}

switcher.onclick = toggle(
  evt => evt.target.className = 'warn',
  evt => evt.target.className = 'off',
  evt => evt.target.className = 'on'
);
```

可以看到抽象的可扩展性。
