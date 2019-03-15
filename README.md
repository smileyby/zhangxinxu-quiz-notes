# 前端小测笔记

## CSS部分

### CSS基础测试3

> CSS基础测试3-题目 【2019-03-09】
> ![CSS基础测试3](/resource/css-test3.jpg)
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