# Untitled

`RTCPeerConnection API`是每个浏览器之间点对点连接的核心，`RTCPeerConnection`是`WebRTC`组件，用于处理对等体之间流数据的稳定和有效通信。

`RTCPeerConnection`可以保护Web开发人员免受潜伏在其中的无数复杂性的影响。`WebRTC`使用的编解码器和协议可以进行大量工作，即使在不可靠的网络上也可以进行实时通信：

* 丢包隐藏
* 回声消除
* 带宽适应性
* 动态抖动缓冲
* 自动增益控制
* 降噪和抑制
* 图像'清洁'。

```text
// 创建实例
let pc = RTCPeerConnection(serverConfig);
```

根据你是发起者还是被发起对象，在连接的每一边会使用稍微不同的方式使用`RtcPeerConnection`对象。

`serverConfigconfig`配置参数中包含`iceServers`参数。它是包含有关`STUN`和`TURN`服务器的信息的`URL`对象数组,在查找`ICE`候选时使用。可以在`code.google.com`找到可用的公共`STUN`服务器的列表。

现实中，无论你的应用如何见到那，`webRTC`都需要服务器，因为：

* 通信用户发现彼此并交换自己的“真实世界”的详细信息；
* `webRTC`客户端（对等方）交换网络信息；
* `peers`交换有关每天的数据，如视频格式和分辨率
* `webRTC`客户端遍历`NAT`网关和防火墙

换句话说，`WebRTC`需要四种类型的服务器端功能：

* 发现用户并沟通。
* 使用`STUN`服务器连接用户信号。
* 使用`NAT` /防火墙遍历。
* 在对等通信失败的情况下，使用中继服务器。

`ICE`是用于连接对等体的框架，例如两个视频聊天客户端。最初，`ICE`尝试通过`UDP` _直接_连接对等端，以尽可能低的延迟。在此过程中，`STUN`服务器只有一个任务：使`NAT`后面的对等体能够找到其公共地址和端口。

**下面是调用流程**：

1.获取本地媒体设备成功之后，创建一个新的`RTCPeerConnection`对象，初始化将本地音视频轨道加入到RTCPeerConnection

```text
 function createConn(stream) {
     localStream = stream
     // 显示本地视频流
    localVideo.srcObject = stream;
     //谷歌公共stun服务器
    let serverConfig = {
        "iceServers": [
            { "urls": ["turn:192.168.1.133:3478"], 
            "username": "webrtc", 
            "credential": "webrtc" 
            }
        ]
    };
     // 呼叫者
    let localPeer = new RTCPeerConnection(serverConfig)
    // 被呼叫者
    let remotePeer = new RTCPeerConnection(serverConfig)
    // 设置媒体流监听，将本地流添加到RTCPeerConnection对象
    localStream.getTracks().forEach((track) => {
      localPeer.addTrack(track, localStream);
    });
    localPeer.addStream(stream)
    
 }
```

2.注册`onicecandidate`处理程序，并监听获取自己的`ICE`协商信息，它将任何`ICE`候选发送给其他对等方

```text
function createConn(stream) {
    ...
    // 当获得到自己的公网地址后，发送给其它客户端
    localPeer.onicecandidate = function(event) {
    console.log('I got my icecandidate info')
    if (event.candidate) {
        console.log(event.candidate.candidate)
    }
        socket.emit('onicecandidate', event.candidate);
    }
    // 如果监测到本地媒体流连接到本地，将其绑定到一个video标签上输出
     localPeer.ontrack = function(e) {
         // 因为媒体流是一个数组
        if (remoteVideo.srcObject !== e.streams[0]) {
        remoteVideo.srcObject = e.streams[0];
        console.log('received remote stream');
    }
    };
}
```

3.提前注册消息处理程序。信令服务器还应该有一个处理来自远程计算机的消息处理程序。如果消息包含`RTCSessionDescription`对象,则应该使用`RTCSessionDescription()`方法将其添加到`RTCPeerConnection`对象。如果消息包含`RTCIceCandidate`对象,则应该使用`addIceCandidate()`方法将其添加到`RTCPeerConnection`对象。

消息处理程序会根据谁是呼叫方和被呼叫方被调用。

```text
//呼叫方收到对方回复的SDP时调用的消息处理程序
    localPeer.setRemoteDescription(new RTCSessionDescription(answer));


//被呼叫方收到对方发送的SOP时调用的消息处理程序
    localPeer.addIceCandidate(new RTCIceCandidate(candidate));
```

4.拨通对方，发送自己的`SDP`信息，开始提供/回答协商过程，这是呼叫者的流量不同于被呼叫者的唯一步骤。呼叫者使用`createoffer( )`方法开始协商，并注册一个收到`RTCSessiondescription`对象的回调。然后这个回调应该使用`setlocaldescription( )`将这个`rtcsessiondescription`对象添加到`rtcpeerconnection`对象中。最后,调用者应该使用信令服务器将这个`rtcsessiondescription`发送到远程计算机。另一方面，被呼叫者，在`createanswer()`方法中注册相同的回调。请注意,只有在从调用者收到通知后，才会启动流。

```text
//当本地开始拨打对方的时候，发送自己的SDP信息
const offerOptions = {
    offerToReceiveAudio: 1,
    offerToReceiveVideo: 1
};
function call() {
    console.log('Starting call');

    try {
        console.log('localPeerConnection createOffer start');
        const offer = await pc1.createOffer(offerOptions);
        console.log(offer);
        localPeer.setLocalDescription(offer)
        socket.emit('offer', offer);
    } catch (e) {
        console.log(`Failed to create session description: ${e.toString()}`);
    }
}

// 当本地收到对方的拨号通知时 收到对方的SDP信息，然后生成回复SDP信息
function handleOffer(offer, name) {
    connectedUser = name;
    console.log("I got offer: ");
    localPeer.setRemoteDescription(new RTCSessionDescription(offer));
    //create an answer to an offer 
    localPeer.createAnswer(function(answer) {
        localPeer.setLocalDescription(answer);
         console.log("I will reply a answer")
        send({
            type: "answer",
            answer: answer
        });
    }, function(error) {
        alert("Error when creating an answer");
    });
};
// 当拨号方收到对方回复的SDP后，设置到连接中，调用消息处理程序
socket.on('answer', (desc) => {
    console.log("I got answer: ", desc.sdp);
    localPeerConnection.setRemoteDescription(desc);
})
```

