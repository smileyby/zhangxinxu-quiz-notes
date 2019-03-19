# 前端小测笔记
> 笔记主要是记录 [zhangxinxu-前端小测答题收集专用](https://github.com/zhangxinxu/quiz) 中所涉及的知识点。

## CSS部分

### CSS基础测试1

> CSS基础测试1-题目【2019-03-19】

![CSS基础测试3](/resource/css-test3.jpg)

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

## DOM部分

## JS部分