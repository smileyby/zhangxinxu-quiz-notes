> JS基础测试37

```
1. 已知某银行卡号是6222081812002934027，好长的一串号码，看的眼睛疼，请把上面字符串处理成4 4 4 4 3空格分隔形式
var bankCode = '6222081812002934027';
2. 移动端网页上有一串数字，是你9月份的意外之财，共计5702375，谁知道呈现的时候被浏览器当成了收集号码给高亮了，这性质可就不一样了，不能忍，请对上面的一串数字进行处理，变成使用千分位分割表示（类似10,100,232）以便浏览器能正确识别位数字。
var numberCode = '5702375';
3. 小明选择了一个文件上传，返回的文件尺寸是2837475字节，哎呀，眼睛有疼了，根本看不出来究竟文件有多大，请把字节使用K,M,G单位表示，再大的尺寸本题不考虑。
var fileSize = 2837475;
```



```javascript
var bankCode = '6222081812002934027';
var numberCode = '5702375';
var fileSize = 2837475;

// 1
// 正则表达式一般趋向于最大长度匹配，也就是所谓的贪婪匹配
// 开启非贪婪匹配：在正则表达式后面加个 `?`
bankCode.match(/\d{3,4}/g).join(' ');

// 2
// toLocalString方法默认只保留小数点后三位（且四舍五入）
// Intl（ECMAScript国际化API的一个命名空间，它提供了精确的字符串对比/数字格式化/日期时间格式化）[Intl](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl)
Number(numberCode).toLocaleString();

// 3
var fileSize = 2837475;
function format(size, fixedNum) {
  var kb = 1024;
  var mb = 1024 ** 2;
  var gb = 1024 ** 3;
  var unit = '';    
  if (fileSize > mb) {
      if (fileSize > gb) {
          size = (size / gb)
          unit = 'G';
      } else {
          size = (size / mb)
          unit = 'M';
      }
  } else {
      size = (size / kb);
      unit = 'K';
  }  
  return `${size.toFixed(fixedNum)}${unit}`; 
}
console.log(format(fileSize, 2));
```

