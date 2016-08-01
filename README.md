# **Youyun Android Wchat_SDK 集成**

## 前期准备
在您阅读此文档之前，我们假定您已具备基础的 Android 应用开发经验，并能够理解相关基础概念。

### 1、注册开发者帐号
开发者在集成游云即时通讯、实时网络能力前，需前往 [游云官方网站](http://17youyun.com/) 注册创建游云开发者帐号。
### 2、下载 SDK
您可以到 [游云官方网站](http://17youyun.com/) 下载游云 SDK。下载包中分为如下两部分：
- IMLib - 游云 IM 通讯能力库和相关库
- IM Demo

 Demo简单集成了IM功能，供您参考。
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
youyun-wchat-android-4.0.0.jar
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


### 3、在您项目的AndroidManifest.xml文件中加入下面广播

```
<receiver android:name="com.ioyouyun.wchat.util.NetworkReceiver" >
        <intent-filter android:priority="2147483647" >
            <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
        </intent-filter>
</receiver>
```


### 4、调用SDK接口即可开发

```
WeimiInstance.getInstance().xxx();
```
## 一、IM长链

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
### 10、按时间获取历史消息
```
@param toUId Id 用户Id/群组Id/聊天室Id
@param timestamp 起始时间戳
@param num 消息条数
@param convType single or group 单聊/群聊 (必填)
@param httpCallback 回调
@param timeout 超时 单位秒
@return boolean
boolean shortGetHistoryByTime(String toUId, long timestamp, int num, ConvType convType, HttpCallback httpCallback, int timeout);
```

## 二、群组短链接口

### 1、创建群

```
@param name 群名称
@param intro 群简介
@param cat1  群类型,0 为临时群, 1为私密群, 2为公开群, 3为聊天室
@param cat2  群类型：0 表示群组, 1 表示群联盟
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean createGroup(String name, String intra, int cat1, int cat2, HttpCallback httpCallback, int timeout);
```
### 2、删除群

```
@param gid 群Id
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean deleteGroup(String gid, HttpCallback httpCallback, int timeout);
```
### 3、添加群成员

```
@param gid 群Id
@param uids 加入人员列表,用逗号隔开
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortGroupuserAdd(long gid, String uids, HttpCallback httpCallback, int timeout);
```
### 4、删除群成员

```
@param gid 群Id
@param uids 需剔出的成员,用逗号隔开
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortGroupuserDel(long gid, String uids, HttpCallback httpCallback, int timeout);
```
### 5、退出群组

```
@param gid 群Id
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortExitGroup(long gid, HttpCallback httpCallback, int timeout);
```
### 6、获取用户群列表

```
@param uid 用户Id
@param cat1 群类型,0 为临时群, 1为私密群, 2为公开群, 3为聊天室  可以是多种类型的组合,以","分割
@param cat2 群类型：0 表示群组, 1 表示群联盟  可以是多种类型的组合,以","分割
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean getGroupList(long uid, String cat1, String cat2, HttpCallback httpCallback, int timeout);
```
### 7、获取群成员列表

```
@param gid 群Id
@param role 按角色过滤,默认所有成员,1.普通成员 2.vip用户 3.管理员 4.群主
@param count 获取成员个数，默认所有
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortExitGroup(long gid, HttpCallback httpCallback, int timeout);
```
### 8、获取群信息

```
@param gid 群Id
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean getGroupInfo(long gid, HttpCallback httpCallback, int timeout);
```
### 9、修改群信息

```
@param gid 群Id
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean getGroupInfo(long gid, HttpCallback httpCallback, int timeout);
```
### 10、申请入群

```
@param gid 群Id
@param msg 附言
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean applyAddGroup(long gid, String msg, HttpCallback httpCallback, int timeout);
```

## 三、聊天室短链接口

### 1、创建聊天室

```
@param roomName 房间名
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortCreateRoom(String roomName, HttpCallback httpCallback, int timeout);
```
### 2、删除聊天室

```
@param roomId 房间Id
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortDeleteRoom(String roomId, HttpCallback httpCallback, int timeout);
```
### 3、加入聊天室

```
@param roomId 房间Id
@param userId 第三方uid
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortEnterRoom(String roomId, String userId, HttpCallback httpCallback, int timeout);
```
### 4、退出聊天室

```
@param roomId 房间Id
@param userId 第三方uid
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortExitRoom(String roomId, String userId, HttpCallback httpCallback, int timeout);
```
### 5、聊天室用户列表

```
@param roomId 房间Id
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortRoomUserList(String roomId, HttpCallback httpCallback, int timeout);
```
### 6、禁言/解禁

```
@param uids 游云Id, 被禁言用户,多个用户之间用逗号隔开
@param status true:禁言 false:解禁
@param roomId 房间Id
@param gagTime 禁言时间 单位:分钟
@param httpCallback 回调
@timeout 超时 单位秒
@return
boolean shortUsersGag(String uids, boolean status, String roomId, long gagTime, HttpCallback httpCallback, int timeout);
```

## 四、异步接收消息

```
// 阻塞接收文件
WeimiNotice weimiNotice =  (WeimiNotice)NotifyCenter.clientNotifyChannel.take();
NoticeType type = weimiNotice.getNoticeType();

handleExtendActionForMsgReceive(weimiNotice);

swiwch(type){
	case textmessage: // 文本
	case mixedtextmessage: // 富文本
	    TextMessage textMessage = (TextMessage) weimiNotice.getObject();
		break;
        case audiomessage: // 语音
	    AudioMessage audioMessage = (AudioMessage) weimiNotice.getObject();
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
	case sendfinished:
	    Log.v("Tag", "消息发送到网关成功");
	    break;
	case sendback:
	    Log.v("Tag", "发送消息成功,msgId:" + weimiNotice.getWithtag());
	    SendBackMessage sendBackMessage = (SendBackMessage)weimiNotice.getObject();
	    break;
	case connecting: // 连接中
	    break;
	case connected: // 连接成功
	    break;
	case reconnected: // 重连成功
	    break;
	case recvUnreadNum: // 获取未读数目
	    break;
	case authLapse: // 认证失效
	    break;
	case notice: // 服务器下发
	    break;
	case system: // 系统通知，包括群系统通知，群消息，变更信息，正在输入信息
	    break;
	case exception: // 异常
             WChatException wChatException = (WChatException) weimiNotice.getObject();
             int statusCode = wChatException.getStatusCode();
             switch (statusCode) {
                case WChatException.InputParamError: // 输入参数错误
                    Log.v("Tag", "输入参数错误:" + wChatException.getMessage());
                    break;
                case WChatException.CodeException: // 编码处理错误
                    Log.v("Tag", "编码处理错误:" + wChatException.getMessage());
                    Log.v("Tag", "编码处理错误:" + wChatException.getCause());
                    break;
                case WChatException.InterruptedException: // 请求被中断
                    Log.v("Tag", "请求被中断:" + wChatException.getMessage());
                    Log.v("Tag", "请求被中断:" + wChatException.getCause());
                    break;
                case WChatException.RequestTimeout: // 会话请求超时
                    Log.v("Tag", "会话请求超时:" + wChatException.getMessage());
                    break;
                case WChatException.BadFileRead: // 文件读取失败
                    Log.v("Tag", "文件读取失败:" + wChatException.getMessage());
                    Log.v("Tag", "文件读取失败:" + wChatException.getCause());
                    break;
                case WChatException.ResponseError: // 返回结果错误
                    Log.v("Tag", "返回结果错误:" + wChatException.getMessage());
                    Log.v("Tag", "返回结果错误:" + wChatException.getCause());
                    break;
                case WChatException.AuthFailed: // 认证失败
                    Log.v("Tag", "认证失败:" + wChatException.getMessage());
                    break;
                case WChatException.NetworkInterruption: // 网络中断
                    Log.v("Tag", "网络中断:" + "NetworkInterruption");
                    Log.v("Tag", "网络中断:" + wChatException.getMessage());
                    break;
                case WChatException.MediaHeartBeatError: // mediaSDK心跳失败
                    Log.v("Tag", "mediaSDK心跳失败:" + wChatException.getMessage());
                    break;
                case WChatException.DataDecodeError: // 数据解析异常
                    Log.v("Tag", "数据解析异常:" + wChatException.getMessage());
                    break;
                case WChatException.SOCKET_CHANNEL_CLOSED: // 网络socket端开
                    Log.v("Tag", "网络socket端开:" + wChatException.getMessage());
                    break;
                default:
                    Log.v("Tag", "WChatException:" + wChatException.getMessage());
                    Log.v("Tag", "WChatException:" + wChatException.getStatusCode());
            }
            break;
	}
	
	// 额外事件处理
	private void handleExtendActionForMsgReceive(WeimiNotice weimiNotice) {
	    int msgType = 0;
	    String msg = null;
	    if (weimiNotice.getNoticeType() == NoticeType.system) {
	         SystemMessage message = (SystemMessage) weimiNotice.getObject();
                 msg = message.content;
	         msgType = getSysMsgType(msg);
	    }else if (weimiNotice.getNoticeType() == NoticeType.textmessage
                || weimiNotice.getNoticeType() == NoticeType.mixedtextmessage) {
                 TextMessage message = (TextMessage) weimiNotice.getObject();
                 msg = message.text;
                 msgType = getSysMsgType(msg);
            }
            if (msgType == 0 || msg == null) {
               return;
            }
        
            if (msgType == 50001) {
                /*
	        * 系统事件消息 50001 由系统用户下发的事件消息，表示和用户或者群组相关的状态变更，客户端可能需要根据该消息进行逻辑处理。
	        * 比如用户加入群组，用户退出群组。客户端可能需要根据该事件更新用户的联系人列表等。这种消息一般不显示，如果显示也只是在消
	        * 息界面显示一条文本提示信息。客户端的处理新旧版本兼容策略是该消息如果有desc文本内容则显示desc，然后根据事件类型判断，
	        * 识别则处理，不识别则抛弃。
	        */
            }else if (msgType == 50002) {
                /*
	        * 系统提醒消息 50002 由系统用户下发的提醒信息，用于提醒用户某个信息或者提醒用户进行一些行为。比如 有人评论了用户的动态，
	        * 有人申请加入用户管理的群，有人加用户为好友等等。这种消息的关注点是提醒，客户端只需展示并且支持用户进行下一步行为，不需
	        * 要根据消息进行特殊的逻辑操作。这种消息的关注点是展示，所以使用统一的格式。
	        */
            }else if (msgType == 50003) {
                /*
	        * 内容消息 50003 显示为内容的消息，包括普通的 文本语音图片，以及富文本信息（图文混排的新闻，地理位置，用户名片，
	        * feed进chat）。这种消息发送方可能是系统用户，也可能是普通用户。如果是普通用户，则显示用户头像，表现为一个用户发出的
	        * 消息。如果是系统用户则不显示用户头像，表现为系统信息。富文本信息也通过模板方式定义格式。客户端的兼容策略是如果识别则显示， 不识别则提示用户升级。
	        * 注意：50003里面的json "body"后面的json以字符串保存
	        */
            }
        
        
	}
	
	// 解析json
	private int getSysMsgType(String msg) {
            int type = 0;
            try {
                JSONObject msgJson = new JSONObject(msg);
                if (msgJson.has("id"))
                    type = msgJson.getInt("id");
            } catch (JSONException e) {
                e.printStackTrace();
            }
            return type;
        }
	
```
