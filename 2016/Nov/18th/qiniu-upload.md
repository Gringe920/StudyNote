## 项目有关于七牛上传图片接口(实现步骤)

1. html实现上传图片预览

2. 将图片数据传给服务器接口

3. 服务器接口返回key,token去请求七牛接口(以下示例)

### 图片预览
```html
<input type="file" id="filechooser" class="filechooser">
```

```css
.filechooser {
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    opacity: 0;
    filter: alpha(opacity=0);
    position: absolute;
    left: 0;
    top: 0;
    font-size: 100px;
    width: 100%;
    overflow: hidden;
    z-index: 10;
}
```
取消样式，并隐藏掉，自定义其样式

```JavaScript
let uploadImg = () => { 
    let filechooser = $('.filechooser'),
        license = '',  //用于请求服务器接口即步骤2
        license_back = ''; //用于请求服务器接口即步骤2
    // 200 KB 对应的字节数
    const maxsize = 200 * 1024;
    filechooser.each(function(i, item){
        $(item).on('change',function(){
            let file = this.files[0];
            let _this = this;

            // 接受 jpeg, jpg, png 类型的图片
            if (!/\/(?:jpeg|jpg|png)/i.test(file.type)) return;

            var reader = new FileReader();
            reader.onload = function() {
                var result = this.result;
                //License = result;
                var img = new Image();

                // 如果图片小于 200kb，不压缩
                if (result.length <= maxsize) {
                    toPreviewer(_this,result,i);
                    return;
                }

                img.onload = function() {
                    var compressedDataUrl = compress(img, file.type);
                    //License_back = compressedDataUrl;
                    toPreviewer(_this,compressedDataUrl,i);
                    img = null;
                };

                img.src = result;
            };

            reader.readAsDataURL(file);
            
            
        });
    });
    

    let toPreviewer = (_this,dataUrl,i) => {

        i == 0 ? license = dataUrl : license_back = dataUrl;
        $('.had-upload').each(function(i,item){
            $(item).show();
        });

        requestModifyCars(dataCar,license,license_back); //请求服务器接口返回key,token
        $(_this).next('.previewer').attr('src', dataUrl);
        filechooser.value = '';
    };


    let compress = (img, fileType) => {
        var canvas = document.createElement("canvas");
        var ctx = canvas.getContext('2d');

        var width = img.width;
        var height = img.height;

        canvas.width = width;
        canvas.height = height;

        ctx.fillStyle = "#fff";
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(img, 0, 0, width, height);

        // 压缩
        var base64data = canvas.toDataURL(fileType, 0.75);
        canvas = ctx = null;

        return base64data;
    };

};

export default uploadImg;
```


### 图片上传七牛接口示例：

```JavaScript
let requestQiniuUpload = function (key,token,i){
    let requestUrl = 'http://up.qiniu.com';
    let formData = new FormData();
    formData.append('key', key);
    formData.append('token', token);
    formData.append('file', $('.filechooser')[i].files[0]);
    $.ajax({
        url: requestUrl,
        type: 'POST',
        data: formData,
        processData: false,
        contentType: false,
        timeout: 60000,
        beforeSend: function(){
            __DEBUG__ && console.log(requestUrl, formData);
        },
        success: function(data) {
            __DEBUG__ && console.log(data);
        },
        error: function(XMLHttpRequest, textStatus, errorThrown){
            console.log(XMLHttpRequest, textStatus, errorThrown);
        },
        complete: function(){
        }
    });
};

export default requestQiniuUpload;
```

*file,通过input元素$('.filechooser')[i].files[0]);取到。这边有两个$('.filechooser')。

### bug:

接口报错:`failed to load resource the server responded with a status of 401 (unauthorized)`

原因是：token过期了，检查服务器接口拿到正确的token再去请求