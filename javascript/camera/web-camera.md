<!--
 * @author: ares
 * @date: 2022-01-14 15:35:45
 * @lastEditTime: 2022-01-14 15:49:44
 * @lastEditors: ares
 * @description: 
 * 
-->
## 浏览器使用camera

### 声明Camera类, 扩展相关方法

```js
function Camera(canvasDom, videoDom) {
    this.track = null;
    this.imgSrc = null;
    this.canvas = canvasDom;
    this.context2d = canvasDom.getContext("2d");
    this.video = videoDom;
    this.constraints = {
        audio: true,
        video: {
            // facingMode: "user",    //移动端优先使用前置摄像头（如果有的话）
            // facingMode: { exact: "environment" },  //移动端强制使用后置摄像头
            width: 320,
            height: 320
        }
    };
    // 老的浏览器可能根本没有实现 mediaDevices，所以我们可以先设置一个空的对象
    if (navigator.mediaDevices === undefined) {
        navigator['mediaDevices'] = {};
    }

    // 一些浏览器部分支持 mediaDevices。我们不能直接给对象设置 getUserMedia
    // 因为这样可能会覆盖已有的属性。这里我们只会在没有getUserMedia属性的时候添加它。
    if (navigator.mediaDevices.getUserMedia === undefined) {
        navigator.mediaDevices.getUserMedia = function (constraints) {

            // 首先，如果有getUserMedia的话，就获得它
            var getUserMedia = navigator['webkitGetUserMedia'] || navigator['mozGetUserMedia'];

            // 一些浏览器根本没实现它 - 那么就返回一个error到promise的reject来保持一个统一的接口
            if (!getUserMedia) {
                return Promise.reject(new Error('getUserMedia is not implemented in this browser'));
            }

            // 否则，为老的navigator.getUserMedia方法包裹一个Promise
            return new Promise(function (resolve, reject) {
                getUserMedia.call(navigator, constraints, resolve, reject);
            });
        }
    }
    this.init();
}

Camera.prototype.init = function () {
    var self = this;
    navigator.mediaDevices.getUserMedia(this.constraints).then(function (stream) {
        self.track = stream.getTracks()[0];
        // 旧的浏览器可能没有srcObject
        if ("srcObject" in self.video) {
            self.video.srcObject = stream;
        } else {
            // 防止再新的浏览器里使用它，因为它已经不再支持了
            self.video.src = window.URL.createObjectURL(stream);
        }
        self.video.onloadedmetadata = function (e) {
            self.video.play();
        };
        stream.active = false;
    }).catch(function (err) {
        console.log(err.name + ": " + err.message);
    });
};


Camera.prototype.getPhoto = function () {
    this.context2d.drawImage(this.video, 0, 0, 320, 320);
    this.imgSrc = this.canvas.toDataURL("image/png");
};

Camera.prototype.turnoff = function () {
    this.track.stop();
};

Camera.prototype.dataURLtoFile = function (dataUrl) {
    var arr = dataUrl.split(','),
        mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]),
        n = bstr.length,
        u8arr = new Uint8Array(n);
    while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
    }
    // return new Blob([u8arr], {type:mime});
    return new File([u8arr], 'head.png', { type: mime });
};
```

### 使用方法

在html页面中需要准备 **video** 和 **canvas** 及操作按钮

```html
<div class="modal fade" id="camera" tabindex="-1" role="dialog" aria-hidden="true">
    <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
            <div class="modal-body text-center">
                <video id="video" src="" width="320" height="320"></video>
                <canvas id="canvas"  width="320" height="320"></canvas>
                <div class="text-left">
                    <button id="get-photo" class="btn btn-success">拍照</button>
                </div>
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" data-dismiss="modal">取消</button>
                <button id="confirm-photo" class="btn btn-primary">确定</button>
            </div>
        </div>
    </div>
</div>
```

```js
var camera;
// 打开拍照模态框, 初始化camera
$("#add-emr-pic").on('click', function (event) {
    $('#camera').modal('show');
    camera = new Camera(document.getElementById('canvas'), document.getElementById('video'));
})
// 拍照
$("#get-photo").on('click',function (event) {
    camera.getPhoto();
})
// 模态框关闭时, 关闭camera
$('#camera').on('hidden.bs.modal', function (e) {
    console.log(camera);
    camera.turnoff();
})
// 关闭模态框, 在页面中显示拍照得到的照片
$("#confirm-photo").on('click',function (event) {
    $("#camera").modal('hide');
    $("#add-emr-pic").attr('src',camera.imgSrc);
})
```