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
// 适用于：格式化银行卡号/电话号码等
bankCode.match(/\d{3,4}/g).join(' ');

// 2
// toLocalString方法默认只保留小数点后三位（且四舍五入）
// Intl（ECMAScript国际化API的一个命名空间，它提供了精确的字符串对比/数字格式化/日期时间格式化）[Intl](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl)
// 适用于：将指定字符串按照规定格式格式化
Number(numberCode).toLocaleString();

// 3
// 适用于：计算文件大小
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

> JS基础测试36

```
1. Ajax上传图片功能，假设图片对象名称是file，请根据提示补全下面代码
var xhr = new XMLHttpRequset();
// 请补全进度/成功/失败这三个回调处理方法（回调函数里面逻辑不需要写）

xhr.open('POST', '/upload', true);
xhr.send();
2.用户往往一次上传多张图，假设此时<input type="file" multiple>空间获取的文件序列是files，请过滤尺寸大于1M的file文件
3.假设files中是所有图片对象会同时进行ajax上传，由于所有上传都是异步的，请问，你该如何判断所有图片上传结束（上传失败也认为结束）
```

```javascript
// 1
var xhr = new XMLHttprequest();
// xhr.onprogress和xhr.upload.onprogress的区别：这两个都能显示进度百分比，但是，前者显示的是服务器返回的数据，后者是发送给服务器的。例如我们ajax.get一张图片，则前者合适；如果我们是ajax.post上传一张图片，则后者合适
xhr.upload.onprogress = function(){
    // 上传进度
}
xhr.upload.onload = function(){
    // 上传成功
}
xhr.upload.onerror = function(){
    // 上传失败
}

// 2
const filterData = [...files].filter(file => file.size < 1024 ** 2);

// 3
const promise = function(file){
    return new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        var target = xhr.upload;
        // onloadend 上传失败和成功都会触发
        target.onloadend = function(){
            resolve();
        }
        target.onerror = function(){
            reject();
        }
        xhr.open("POST", '/upload', true);
        xhr.send(file);
    })
}
const promises = [...files].map(function(file){
    return promise(file);
});
Promise.allSettled(promises).then(function(values){
   // all complete 
})
// 使用场景：文件批量上传，监听进度/成功/失败等状态，对文件格式大小做限制
```

> JS基础测试35

```
所有题目公用一个上下文
const object1 = {
	userid: 123,
	username: '王二',
	tel: '13208033621'
};
1. 一句话说明encodeURI和encodeURIComponent的主要区别？
2.把object1转变成userid=123&username=%E7%8E%8B%E4%BA%8C...这种字符串形式
3. 获取当前URL地址栏中的第一个？后面查询内容（包括问号）
4. 把？后面的查询字符串转换成object1这样的键值对对象格式
5. 如果键有重复，则值以数组呈现
```

```javascript
const object1 = {
	userid: 123,
	username: '王二',
	tel: '13208033621'
};
// 1
// encodeURI转换的范围没有encodeURIComponent广
// encodeURI('&?#@=') => "&?#@="
// encodeURIComponent('&?#@=') => "%26%3F%23%40%3D"

// 2
Object.keys(object1).map(key => `${key}=${encodeURI(object1[key])}`).join('&')

// 3
location.search;

// 4
// 测试用例：?a=1&b=2&url=a.html?b=1&c=1
// 测试用例：?a=1&b&c=&d=2
var reg = /([^?#&=]+)=([^#&]*)/g
// 多个以数组存储
var obj = {};
'?a=1&b=2&url=a.html?b=1&c=1'.replace(reg, function(input, s1, s2){
   if (!obj[s1]) {
       obj[s1] = s2;
   } else {
       if (Array.isArray(obj[s1])) {
           obj[s1].push(s2);
       } else {
           obj[s1] = [obj[s1], s2];
       }
   }  
});
console.log(obj);
// 使用场景：对地址栏参数进行处理，get请求拼接参数等
// 题中的问题，也可以使用URLSearchParams 解决，但存在兼容性问题IE不支持，需要引入polyfill https://github.com/WebReflection/url-search-params

// URLSearchParams 解法
const obj1 = {}
const myParams = new URLSearchParams(location.search)
for (var p of myParams.keys()){
	obj[p]=myParams.getAll(p).length > 1 ? myParams.getAll(p) : myParams.get(p);
};
```



