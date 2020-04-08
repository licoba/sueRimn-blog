# WebRTC API之 mediaStream

`WebRTC API`包括媒体捕获、音频视频的编码和解码、传输层和会话管理。

* `getUserMedia()`：捕获音频和视频。
* `MediaRecorder`：录制音频和视频。
* `RTCPeerConnection`：在用户之间传输音频和视频。
* `RTCDataChannel`：用户之间的流数据。

在这里插入图片描述

![](../.gitbook/assets/image%20%282%29.png)

#### 1.媒体捕获`MediaStream`（又名`getUserMedia`） <a id="item-1"></a>

`MediaStream`接口是一个媒体内容的流，一个流包含几个轨道，比如视频和音频轨道。作用是从用户本地摄像机和麦克风访问媒体流。`getUserMedia()`方法是访问本机输入设备的主要方式。

第一步是访问用户设备的摄像头和麦克风。我们检测可用设备的类型,获得用户访问这些设备的权限,并管理数据流。

注意：

* 实时音视频以流对象的形式表示
* 通过询问用户是否授权，有安全控制，只允许授予一次权限，此后不再要求访问
* 输入设备选择由`mediaStream`处理
* 每个`mediaStream`对象包括几个`mediaStreamTRack`对象，代表来自不同设备的音视频
* 每个`mediaStreamTrack`对象可能包括几个信道（左声道和右声道）
* 两种方法输出`mediaStream`对象。首先将音视频输出显示，设置`srcObject`属性将`MediaStream`附加到视频元素，然后将输出发送到`RTCPeerConnection`对象，然后传送到远程对象。

**（1）获取本地媒体流，并检测浏览器是否支持**

```text
navigator.mediaDevices.getUserMedia  =                         navigator.mediaDevices.getUserMedia ||
 navigator.mediaDevices.webkitGetUserMedia ||
 navigator.mediaDevices.mozGetUserMedia ||
 navigator.mediaDevices.msGetUserMedia;

if (navigator.getUserMedia) {
    // 支持
} else {
    // 不支持
}
navigator.mediaDevices.getUserMedia({ video: true, audio: true }).then( ).catch( );
```

**（2）接受的参数**

`getUserMedia`方法接受三个参数，第一个参数是一个对象，即约束条件，另外两个是成功回调函数和失败回调函数。

**约束条件**

约束条件里包括捕获对象，表示要获取哪些多媒体设备，你获取本地媒体流时是要求获取摄像头还是麦克风，也可以设置视频分辨率的值、宽高比、面向模式（前置还是后置摄像头）、帧速率、高度、宽度等。可以单独把约束条件提出来单独写。

```text
navigator.mediaDevices.getUserMedia({
    // 以下就是约束条件
    video: true, 
    audio: true 
})
    .then(createConn )
    .catch(
    console.log(`getUserMedia() error: ${e.name}`);
    );

// 或者
let constraints = {
    video: true,
    audio: true,
    ...
}
```

`navigator.mediaDevices.getUserMedia(constraints,onSuccess, onError)`  
除了指定捕获对象之外，还可以指定一些限制条件，比如视频的宽高等等。

```text
let constraints = {
    video: {
        minWidth: 1280,
          minHeight: 720
    }
    ...
}
```

如果网页使用了`getUserMedia`方法，浏览器就会询问用户，是否同意浏览器调用麦克风或摄像头。如果用户同意，就调用回调函数`onSuccess`；如果用户拒绝，就调用回调函数`onError`

**成功回调函数**

获取多媒体设备成功时调用。`onSuccess`回调函数的参数是一个数据流对象`stream`。`stream.getAudioTracks`方法和`stream.getVideoTracks`方法，分别返回一个数组，其成员是数据流包含的音轨和视轨（`track`）。

使用的声音源和摄影头的数量，决定音轨和视轨的数量。比如，如果只使用一个摄像头获取视频，且不获取音频，那么视轨的数量为1，音轨的数量为0。每个音轨和视轨，有一个`kind`属性，表示种类（`video`或者`audio`），和一个label属性（比如`FaceTime HD Camera (Built-in)`）。

**失败回调函数**

获取多媒体失败时调用。`Error`对象的`code`属性有说明错误的类型：

* `PERMISSION_DENIED`：用户拒绝提供信息。
* `NOT_SUPPORTED_ERROR`：浏览器不支持硬件设备。
* `MANDATORY_UNSATISFIED_ERROR`：无法发现指定的硬件设备。

下面看一个完整例子：

```text
let constraints = {video: true};

function onSuccess(stream) {
  let video = document.querySelector("video");
    // video.src = window.URL.createObjectURL(stream);这种写法已被移除
  video.srcObject = stream;
}

function onError(error) {
  console.log("getUserMedia error: ", error);
}

navigator.getUserMedia(constraints, onSuccess, onError);
```

注意，如果存在回声，应该在`video`或者`audio`节点处添加`muted`，进行简单的回声消噪。

**（3）屏幕捕获**

可以看看这个[例子](https://www.webrtc-experiment.com/RecordRTC/)

> 参考资料：  
> [mediaStream-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStream)  
> [WebRTC Native APIs](https://webrtc.org/native-code/native-apis/)  
> [webRTC-javaScript](https://javascript.ruanyifeng.com/htmlapi/webrtc.html#toc1)

