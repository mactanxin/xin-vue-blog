# Vue中使用iView上传组件配合XLSX库读取Excel文件
## 在页面上可以使用原生方法或iView组件都可以达到相同效果.
### Demo : 使用原生input tag配合iView的Button组件实现
先在页面中加入input tag以及button
```html
<input type="file" id="file" style="display:none;"  @change="fileLoadChanged"/>
<Button type="primary" id="button" name="button" value="Upload" @click="importScene" class="hasBgBtn">导入</Button>

```

之后再加入对应的JS代码: 

```javascript
//第一个方法只是对应input激活事件
importScene(/evt/) {
      document.getElementById("file").click();
},
//真正的检测文件改变以及读取excel
fileLoadChanged(/evt/) {
      console.info(evt);
      let files = evt.target.files;
      console.info(files);
      if(files && files[0]) {
        console.info('file existed');
        /this/.loadExcel(files[0]);
      }
},
```

### Demo: 使用iView的Upload组件

代码如下:
```javascript
<Upload
            ref="upload" 
            :show-upload-list="false"
            :before-upload="handleUpload" 
            accept=".xls,.xlsx"
            :format="['xls','xlsx']" 
            :on-success="handleSuccess" 
            :max-size="1024" 
            :on-exceeded-size="handleMaxSize"
            :on-format-error="handleFormatError"
            action="/"
            style="display: inline-block;" 
          >
              <Button type="primary" class="hasBgBtn">导入</Button>
          </Upload>

```

需要注意的是一旦使用了 :before-upload 方法, 那么<Upload> 组件就会停止代理其他事件例如 :on-format-error 或 :on-success 这些. 需要自己在自定义的 handleUpload方法中实现.
这里可以配合 iView 中的 $Notice 组件进行提示

```javascript
    handleMaxSize () {
      /this/.$Notice.error({
          title: '文件超出最大限制',
          desc: '最大文件支持20MB'
      });      
    },
    handleFormatError(/file/){
      console.info('err');
      /this/.$Notice.error({
          title: '文件格式不正确',
          desc: '仅支持后缀为.xls或.xlsx格式'
      });
    },
    handleSuccess() {
      /this/.$Notice.success({
          title: '读取成功',
          desc: '正在加载数据请稍后'
      });
    },
    handleUpload(/file/) {
      if(file.size > 160000 ) {
        /this/.handleMaxSize();
        return false;
      } else {
        /this/.handleSuccess();
        const reader = new FileReader();
        /this/.loadExcel(file);
        return false;
      }
    },

```