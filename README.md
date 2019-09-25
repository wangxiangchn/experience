#### 背景图拉伸或挤压

解决思路   把背景图分层  背景和主图单独适应  背景拉伸没关系 主图等比缩放

```css
  background: url('***') no-repeat center bottom / 100% auto ,url('***') no-repeat center center / 100% 100%;

```

可能在一些低浏览器出现不兼容问题 建议

```css
  background: url('***');
	background: url('***') no-repeat center bottom / 100% auto ,url('***') no-repeat center center / 100% 100%;
```



------

#### input 软键盘打开挤压页面

input在 ios 不会发生问题 但是在安卓问题有点多

解决思路给 body 加一个定高 而不是 100%

```js
window.document.onreadystatechange = function(){
    var u = navigator.userAgent;
    var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1;
    if( window.document.readyState === "complete" && isAndroid ){
     document.getElementsByTagName('body')[0].style.height = document.getElementsByTagName('body')[0].clientHeight + 'px';
    };
  };
```



------

#### input 软键盘收回页面不会滚

```js
input.addEventListener('blur', function () {
    window.scroll(0,0);
  } );
```



------

#### 视频

建议选择mmd-plugin.min.js+mmd.videoplayer.

可参阅[链接](https://tgideas.qq.com/doc/frontend/component/m/mmd.html)

在安卓上加全屏属性 否则在微信上视频播放完毕会弹出广告页

```html
<video id="video" x5-video-player-type="h5" x5-video-player-fullscreen="true">

```

在微信播放视频 默写全面屏手机会隐藏微信顶部导航条 并在视频播放后不会退出

解决思路  视频播放后强行退出

```js
var myVideo = document.getElementById('video');
        myVideo.addEventListener('ended', function () {
            this.webkitExitFullScreen();
        });
        myVideo.addEventListener('ended', function () {
            this.srcObject = new window.webkitMediaStream;
        });
```

另 display:none的组件不会预加载 

视频需要点击触发播放 建议加 loading 页

```js
videoData.addEventListener('canplay', function() {
            // 可播放
        });
```

在一些手机加载速度太慢  考虑用户体验 不应一直加载下去  而是加一个兜底时间 就算没有加载完毕也进行播放



------

#### 分享图片

如果遇到一些需要生产分享图片的项目 

建议 dom 转成 canvas 再转图片  在浏览器 钉钉 微信 皆可长按保存

[**html2canvas**](https://html2canvas.hertzen.com/)  完成 dom 转 canvas

[**canvas2image**](https://github.com/hongru/canvas2image) 完成 canvas 转图片   也可用原生方法

在微信不支持保存 png 图片 用 canvas2image 转成 jpeg 即可

```js
  Canvas2Image.convertToJPEG(canvas);
```



------

#### 长按事件

手撸可以参考使用

```js
function longpress(id, func) {
  var timeOutEvent;
  document.querySelector('#' + id).addEventListener('touchstart', 	function(e) {
     e.preventDefault()
     clearTimeout(timeOutEvent);
     timeOutEvent = setTimeout(function() {
           func();
      }, 1500);
     });
    
   document.querySelector('#' +id).addEventListener('touchmove', function(e) {
       e.preventDefault()
       clearTimeout(timeOutEvent);
      });
    
    document.querySelector('#' + id).addEventListener('touchend', function(e) {
        clearTimeout(timeOutEvent);
        });
      }
```



------

#### 假 loading

代码简单 可 更改配合项目使用

```js
function genProcess() {
   var start = 0;
   var end = 100;
   var duration = 10000;
   var startTime = Date.now();
    
   function animate() {
     var delta = Math.min(duration, (Date.now() - startTime));
     var value = (delta / duration) * (end - start) + start;
     // var result = `${parseInt(value, 10)}%`;
     var result = `${parseInt(value, 10)}%`;
     //判断3d资源是否加载完成 如果没有加载完就一直百分之99
     var load3dResources = localStorage.getItem('loading');
         if (result === "100%") {
             if (load3dResources != "true" || !videoPlay) {
                        result = "99%";
                    } else {
                        result = "100%";
                    }
                }
                progress.innerHTML = result;
                if (delta < duration) {
                    requestAnimationFrame(animate);
                }
                if (result === '100%') {
                    // eslint-disable-next-line no-return-assign
                    setTimeout(function() {
                        progress.innerHTML = '点击进入';
                    }, 1000)
                    return 
                }
            }
            animate();
        }
```

