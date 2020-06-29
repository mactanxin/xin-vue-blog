# 使用Lighthouse 优化

一直想要 ~~但是没有勇气~~ 使用Lighthouse测试一下性能, 最近终于在自己的博客上开刀了.  
我没有开PWA, 在Desktop模式下测试了一下, 主要针对这几类, 对自己的问题做个总结.  

## Desktop

### Performance - 100分

_Max Potential First Input Delay 170 ms_ 这一项比较低, 有待改进

### Accessibility - 92分

_Background and foreground colors do not have a sufficient contrast ratio._
主要是针对配色的, 不太想改了.  

### Best Practises

#### Links to cross-origin destinations are unsafe

_Add `rel="noopener"` or `rel="noreferrer"` to any external links to improve performance and prevent security vulnerabilities._  
我的 Github 外链没有加这个`rel`, 查了一下是个新特性

当你浏览一个页面点击一个a标签连接 <a href="www.baidu.com" target="_blank"> 跳转到另一个页面时，

在新打开的页面（baidu）中可以通过 window.opener获取到源页面的部分控制权， 即使新打开的页面是跨域的也照样可以（例如 location 就不存在跨域问题）。  

`rel=noopener` 新特性

```html
<a href="www.baidu.com" target="_blank" rel="noopener noreferrer"></a>
```

在chrome 49+，Opera 36+，打开添加了rel=noopener的链接， `window.opener` 会为`null`。在老的浏览器中，可以使用 `rel=noreferrer` 禁用HTTP头部的Referer属性，使用下面`JavaScript`代替`target='_blank'`的解决此问题：

var otherWindow = window.open('http://keenwon.com');
otherWindow.opener = null;
otherWindow.location = url;
使用 window.open 打开页面，手动剑opener设置为null。

### SEO - 100 分

之前少一项 `<meta name="description">` 减了不少分.  
还有 `<img>` tag都缺少 `alt` 属性.
` <meta name="viewport"> ` 的属性,
` <title> ` 属性,
