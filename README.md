# 前端小测笔记
> 笔记主要是记录 [zhangxinxu-前端小测答题收集专用](https://github.com/zhangxinxu/quiz) 中所涉及的知识点。

## CSS部分

### CSS基础测试1

> CSS基础测试1-题目【2019-03-19】

![CSS基础测试1](/resource/css-test1.jpg)

> 解析
> 1、 `* {margin: 0; padding: 0;}`不推荐这种写法，会带来不必要的开销，针对有默认值的标签做处理就好
> 
> 2、 避免容器高度固定，为后期的扩展做好准备
> 
> 3、 使用左右各50%实现，在实际项目中可能会由于某一个内容过多而产生内容堆积在一起的问题（建议左侧安全宽度，右侧自动分配剩余空间）
> 
> 4、 关于绝对定位的实现

```css
dl {
  border: 1px solid #000;
  position: relative;
}
dd {
  position: absolute;
  right: 0;
  margin: -1.2em 0 0 0;
}
```
> 5、 关于左右浮动的实现（极端情况会出现错位问题）

```css
dt{
    float: left;
    clear: both;
}
dd{
    float: right;
}
```
> 6、 有必要考虑极端情况
>> 6.1 文字内容很多
>> 6.2 连续的一串英文
>> 6.3 没有文字内容

```css
dd {
	word-break: break-all;
}
dd:empty::before {
	content: '-';
	color: #999;
}
```
> 比较好的实现方法如下：
> 
```css
/* 公共部分 */
dl {
    line-height: 1.5;
    margin: 0; padding: 10px;
    border: 1px solid #ccc;
    background-color: #fff;    
}
dd {
    word-break: break-all;    
    text-align: right;
    margin-left: 0;
}
dd:empty::before {
    content: '-';    
    color: #999;
}

/*答案1-dt标签绝对定位*/
dt { position: absolute; }
dd {text-align: right; margin-left: 5em; }

/*答案2-Flex布局实现*/
dl {
    display: flex;
    flex-wrap: wrap;
}
dt {
    width: 5em;
}
dd {
    width: calc(100% - 5em);
    text-align: right;
}

/*答案3-Grid布局实现*/
dl {
    display: grid;
    grid-template-columns: auto 1fr;
    grid-column-gap: 1em;
}
dd {
    text-align: right;
}

/*答案4-float浮动实现*/
dt {
    width: 5em;
    float: left;    
}
dd {
    text-align: right;
    overflow: hidden;    
}

/*答案5-借助原生流体特性实现*/
dd {
    margin: -1.5em 0 0 5em;
    text-align: right;    
}
```

### CSS基础测试3

> CSS基础测试3-题目 【2019-03-09】

![CSS基础测试3](/resource/css-test3.jpg)

>
> 解析
> 1. 使用form元素增强交互体验
> 
> 2. 按钮使用button，并指定type为`submit`类型
> 
> 3. form + submit 可以在按回车键的时候触发表单的提交行为
> 
> 4. 标题如果使用div语意弱了一些，可以使用`h标签`来增强语义，最好的做法是使用 `fieldset`和`legend`
> 
> 5. 如果输入框的类型是`text`，可以直接省略掉`type`属性节约代码，因为input标签默认类型就是`text`
> 
> 6. required 是必须要的，不想显示默认的验证提示，可以在form元素上添加 `novalidate`属性
> 
> 7. `autocomplete` 把浏览器自动提示关掉，将验证码的提示关掉
> 
> 8. 忘记密码，使用`a`标签，这样可以在按tab键时被focus住，当用户鼠标不能操作的时候依然可以用键盘操作选中忘记密码。（如果使用非控件元素，则不能为focus如`div` `span` `p`等）
> 
> 9. 如果页面中有很多导航按钮，用户进入到登陆界面操作，我们给第一个表单元素添加`autofocus`属性，来增强用户体验，`autofocus`存在兼容性问题，老IE版本不支持
> 
> 10. 由于autofocus，可能会被其他JS清除掉，所以需要给`input`标签添加`tabindex`属性来重置效果（鑫大大在测试的时候，发现只有在form被focus的时候这个属性才生效）

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width">
	<title>表单登陆</title>
</head>
<body>
	<nav>
		<a href="">导航1</a>
		<a href="">导航2</a>
		<a href="">导航3</a>
		<a href="">导航4</a>
	</nav>
	<form class="login-box" novalidate>
		<fieldset>
			<lenged>登录</lenged>
			<div class="login-body">
				<p class="account">
					<input name="account" placeholder="账号/手机" required autofocus tabindex="1">
				</p>
				<p class="password">
					<input type="password" name="password" placeholder="密码" required tabindex="2">
					<a href="">忘记密码</a>
				</p>
				<p class="captcha">
					<input name="code" autocomplete="off" placeholder="验证码" required tabindex="3">
					<img src="" alt="验证码">
				</p>
			</div>
			<button type="submit" tabindex="4">登陆</button>
			<a href="xxx" target="_blank">立即注册</a>
		</fieldset>
	</from>
</body>
</html>
```

### CSS基础测试4

> CSS基础测试4-题目 【2019-03-27】

![CSS基础测试3](/resource/css-test4.png)

> 弹性问题
> 使用`@media`当页面尺寸变化是对页面字体大小和元素尺寸进行微调
> 以下代码出处：[张鑫旭-基于vw等viewport视区单位配合rem响应式排版和布局](https://www.zhangxinxu.com/wordpress/2016/08/vw-viewport-responsive-layout-typography/)

```css
html {
    font-size: 16px;
}

@media screen and (min-width: 375px) {
    html {
        /* iPhone6的375px尺寸作为16px基准，414px正好18px大小, 600 20px */
        font-size: calc(100% + 2 * (100vw - 375px) / 39);
        font-size: calc(16px + 2 * (100vw - 375px) / 39);
    }
}
@media screen and (min-width: 414px) {
    html {
        /* 414px-1000px每100像素宽字体增加1px(18px-22px) */
        font-size: calc(112.5% + 4 * (100vw - 414px) / 586);
        font-size: calc(18px + 4 * (100vw - 414px) / 586);
    }
}
@media screen and (min-width: 600px) {
    html {
        /* 600px-1000px每100像素宽字体增加1px(20px-24px) */
        font-size: calc(125% + 4 * (100vw - 600px) / 400);
        font-size: calc(20px + 4 * (100vw - 600px) / 400);
    }
}
@media screen and (min-width: 1000px) {
    html {
        /* 1000px往后是每100像素0.5px增加 */
        font-size: calc(137.5% + 6 * (100vw - 1000px) / 1000);
        font-size: calc(22px + 6 * (100vw - 1000px) / 1000);
    }
}
```

> 基本要点：
> 1. 基准字号使用16px，不用其他像素，也不用使用100px
> 2. media查询和vw技巧实现html基础尺寸动态化（无需js）
> 3. 要统一的命名空间，类似chat-XXX
> 4. 遇到不同性质的命名，通常两种方式：1）类名，但是命名上明显区分，例如chat-item__left；2）使用属性选择器，例如data-type="friend"
> 5. 避免没必要的嵌套，百害无一益（嵌套一般使用在清除默认样式方面）
> 6. 小尾巴的实现：边框+圆角、box-shadow+圆角、径向渐变
> 7. 左右对称布局的实现：`direction: rtl`配合CSS逻辑属性
> 8. 不推荐使用dl标签，可以给每个列表增加一个tabindex=0


## DOM部分

> DOM基础测试29

![DOM基础测试29](/resource/dom-test29.png)

> 学习目标：
> 1. 了解Element.closest()原生API方法
> 2. 了解Element.matches()原生API方法
> 3. 学会自定义扩展需要的Element API
>
> 应用场景：在**事件委托**的时候非常有用
>
> 答疑：
> 1. 返回结果，应和原生dom的其他方法返回的结果相符合（返回空数组）
> 2. 把匹配到的数组进行倒叙 `reverse()`（这个细节做的很好）
> 3. 还有就是形参名要符合传进来的值相符合（selector）

[各位同学的给出的答案-猛戳](https://github.com/zhangxinxu/quiz/issues/15)

> 下面是我当时给出的答案，看了答疑后，存在的问题和不足：`形参名`，`获取全部返回值的细节处理`，`需要去了解matches`，`代码冗余`，

```javascript
Element.prototype.myClosest = function myClosest(value){
  if (value === '') {
    return null;
  }
	
  if (this.closest) {
    return this.closest(value);
  }	
	
  let nodeAry = document.querySelectorAll(value),
      nodeLength = nodeAry.length,
      lastCloseNode = null;
  if (nodeLength <= 0) {
    return null;
  }
	
  if (nodeLength === 1 && nodeAry[0] === this) {
    return this;
  }
	
  for (let i = 0; i < nodeLength; i += 1) {
    if (nodeAry[i].contains(this)) {
      lastCloseNode = nodeAry[i];
    }
  }
  return lastCloseNode;
};
Element.prototype.closestAll = function closestAll(value){
  if (value === '') {
    return [];
  }
	
  let nodeAry = document.querySelectorAll(value),
      nodeLength = nodeAry.length,
      allCloseNode = [];
  if (nodeLength <= 0) { 
    return [];
  }

  for (let i = 0; i < nodeLength; i += 1) {
    if (nodeAry[i].contains(this) || nodeAry[i] === this) { 
      allCloseNode.push(nodeAry[i]);
    }
  }
  return allCloseNode;
};
```


## JS部分
