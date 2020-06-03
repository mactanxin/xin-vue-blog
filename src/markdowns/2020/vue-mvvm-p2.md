# 自己动手系列之: 手写一个MVVM (Part 2): 实现Compile

上一篇已经可以监听每个数据的变化了，那么监听到变化之后就是实现 `Compile`  

compile主要做的事情是解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图，  
如下图👇:   
![示例](https://raw.githubusercontent.com/mactanxin/xin-vue-blog/master/src/statics/images/compile.png "")


