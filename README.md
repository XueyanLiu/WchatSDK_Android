# **Youyun Android Wchat_SDK 集成**

## 前期准备
在您阅读此文档之前，我们假定您已具备基础的 Android 应用开发经验，并能够理解相关基础概念。

### 1、注册开发者帐号
开发者在集成游云即时通讯、实时网络能力前，需前往 [游云官方网站](http://17youyun.com/) 注册创建游云开发者帐号。
### 2、下载 SDK
您可以到 [游云官方网站](http://17youyun.com/) 下载游云 SDK。下载包中分为如下两部分：
- IMLib - 游云 IM 通讯能力库和相关库
- IM Dome

 Dome简单集成了IM功能，供您参考。
### 3、创建应用
您要进行应用开发之前，需要先在游云开发者平台创建应用。如果您已经注册了游云开发者帐号，请前往 [游云开发者平台](http://17youyun.com/) 创建应用。

您创建完应用后，首先需要了解的是 App ClientID / Secret，它们是游云 SDK 连接服务器所必须的标识，每一个 App 对应一套 App ClientID / Secret。

## 集成开发
### 1、将您下载的jar包导入您项目的libs目录，IM需要如下jar包

```
commons-fileupload-1.2.1.jar
commons-httpclient-3.1.jar
commons-lang-2.6.jar
youyun-protobuf-java-2.4.1.jar
youyun-wchat-android-1.0.1.jar
```
### 2、在您项目的AndroidManifest.xml文件中加入以下权限

```
 <uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.GET_TASKS" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
```

### 3、在您项目的AndroidManifest.xml文件中加入下面服务

```
<service android:name="com.ioyouyun.wchat.countly.OpenUDID_service" >
        <intent-filter>
            <action android:name="org.OpenUDID.GETUDID" />
        </intent-filter>
</service>
```


### 4、在您项目的AndroidManifest.xml文件中加入下面广播

```
<receiver android:name="com.ioyouyun.wchat.util.NetworkReceiver" >
        <intent-filter android:priority="2147483647" >
            <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
        </intent-filter>
</receiver>
```


### 5、调用SDK接口即可开发

```
WeimiInstance.getInstance().xxx();
```
## 一、接口文档

### 1、初始化SDK

```
@param context 上下文
@param udid 设备唯一标识
@param clientId ClientId
@param secret 密钥
@param timeout 超时 单位秒
@return AuthResultData{boolean success, String userInfo} 
AuthResultData registerApp(Context context, String udid, String clientId, String secret, int timeout);
```
### 2、初始化SDK（测试）
```
@param context 上下文
@param udid 设备唯一标识
@param clientId ClientId
@param secret 密钥
@param timeout 超时 单位秒
@return AuthResultData{boolean success, String userInfo}
AuthResultData testRegisterApp(Context context, String udid, String clientId, String secret, int timeout);
```
### 3、获取用户Id

```
@return 用户Id
String getUID();
```
### 4、注销

```
@return true or false
boolean logout();
```
### 5、发送文本
```
@param msgId 消息ID (必填)
@param touid 接收方 (必填)
@param text 发送的文本内容 (必填)
@param convType single or group 单聊/群聊 (必填)
@param padding 附加信息
@param timeout 超时 单位秒
@return boolean 是否进入发送消息队列
boolean sendText(String msgId, String touid, String text, ConvType convType, byte[] padding, int timeout);
```
### 6、发送自定义消息
```
@param msgId 消息ID (必填)
@param touid 接收方 (必填)
@param text 发送的文本内容 (必填)
@param convType single or group 单聊/群聊 (必填)
@param padding 附加信息
@param timeout 超时 单位秒
@return boolean 是否进入发送消息队列
boolean sendMixedText(String msgId, String touid, String text, ConvType convType, byte[] padding, int timeout);
```
### 7、发送文件
```
@param msgId 消息ID (必填)
@param touid 接收方 (必填)
@param filepath 图片的保存路径 (必填)
@param filename 文件名 (必填)
@param type MetaMessageType.image
@param index 包含要上传的分片编号组成的数组 (可选) null
@param convType single or group 单聊/群聊 (必填)
@param padding 附加信息
@param thumbnail 缩略图
@param timeout 超时 单位秒
@return int分片的总数
int sendFile(String msgId, String touid, String filepath, String filename, MetaMessageType type, int[] index, ConvType convType, byte[] padding, byte[] thumbnail, int timeout);
```
### 8、下载文件
```
@param fileId 需要的文件ID (必填)
@param downloadPath 指定下载到的路径 (必填)
@param fileLength 文件长度单位 byte (必填)
@param index 包含要下载的分片编号组成的数组 (可选) null
@param type MetaMessageType.image
@param index 包含要上传的分片编号组成的数组 (可选) null
@param pieceSize
@param timeout 超时 单位秒
@return boolean 是否进入发送队列
int downloadFile(String fileId, String downloadPath, int fileLength, int index[], int pieceSize, int timeout);
```
### 9、发送语音
```
@param msgId 消息ID (必填)
@param touid 接收方 (必填)
@param spanId 语音唯一标识
@param spanSequenceNo 语音分片编号, 如 1, 2, 3, ... -1, -1 表示结束
@param endTag
@param audioData 语音内容二进制流
@param convType single or group 单聊/群聊 (必填)
@param padding 附加信息
@param timeout 超时 单位秒
@return boolean 是否进入发送队列
boolean sendVoiceContinue(String msgId, String touid, String spanId, int spanSequenceNo, boolean endTag, byte[] audioData, ConvType convType, byte[] padding, int timeout);
```
### 10、创建群组

```
@param httpCallback回调 (String groupId)
@timeout 超时 单位秒
@return
boolean shortGroupCreate(HttpCallback httpCallback, int timeout);
```
### 11、退出群组

```
@param gid  群组Id
@param httpCallback回调 {"apistatus":1,"result":true}
@timeout 超时 单位秒
@return
boolean shortExitGroup(long gid, HttpCallback httpCallback, int timeout);
```
### 12、添加群成员

```
@param gid  群组Id
@param uids 用户Ids ["",""]
@param httpCallback回调{"apistatus":1,"result":1}
@timeout 超时 单位秒
@return
boolean shortGroupuserAdd(long gid, String uids, HttpCallback httpCallback, int timeout);
```
### 13、删除群成员
```
@param gid  群组Id
@param uids 用户Ids ["",""]
@param httpCallback回调{"apistatus":1,"result":1}
@timeout 超时 单位秒
@return
boolean shortGroupuserDel(long gid, String uids, HttpCallback httpCallback, int timeout);
```
### 14、查看群成员
```
@param gid  群组Id
@param httpCallback回调 [12345, 12345]
@timeout 超时 单位秒
@return
boolean shortGroupuserList(long gid, HttpCallback httpCallback, int timeout);
```
### 15、查看用户群列表
```
@param gid  群组Id
@param httpCallback回调 [12345, 12345]
@timeout 超时 单位秒
@return
boolean shortGroupList(String uid, HttpCallback httpCallback, int timeout);
```
## 二、异步接收消息

```
// 阻塞接收文件
WeimiNotice weimiNotice =  (WeimiNotice)NotifyCenter.clientNotifyChannel.take();
NoticeType type = weimiNotice.getNoticeType();
swiwch(type){
		case textmessage: // 文本
			 TextMessage textMessage = (TextMessage) weimiNotice.getObject();
			 break;
		case filemessage: // 文件
			 FileMessage fileMessage = (FileMessage) weimiNotice.getObject();
			 break;
		case sendfile: // 发送文件回调
			 List<Integer> unUploadSliceList = (List<Integer>) weimiNotice.getObject();
		     break;
		case downloadfile: // 下载文件回调
		     FileMessage fileMessage = (FileMessage) weimiNotice.getObject();
			 break;
	}
```
