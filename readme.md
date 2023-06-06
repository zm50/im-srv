## 后端 HTTP 服务接口

```
service HTTP{
    // 创建用户
    rpc UserRegist(UserRegistInfo) returns (UserRegistRes);
    // 用户登录
    rpc UserLogin(UserLoginInfo) returns (UserLoginRes);
    // 创建群聊
    rpc GroupRegist(GroupRegistInfo) returns (GroupRegistRes);
}

message UserRegistInfo{
    string name = 1;
    string pwd = 2;
}

message UserRegistRes {
    bool ok = 1;
    string msg = 2;
}

message UserLoginInfo{
    string name = 1;
    string pwd = 2;
}

message UserLoginRes {
    uint32 ID = 1;
    string Name = 2;
    string Token = 3;
}

message GroupRegistInfo{
    uint32 masterId = 1;
    string name  = 2;
    string introduce = 3;
}

message GroupRegistRes {
    bool ok = 1;
    string msg = 2;
    uint32 groupId = 3;
}
```


## 后端 WebSocket服务接口

```
service WebSocket{
	
	// ================================================好友模块================================================
    // 获取好友会话
    rpc GetFriendSession(Id2) returns (UserMessages);
	// 获取新好友信息
	rpc GetNewFriend(ProcessId) returns (Users);
	// 用户消息持久化
	rpc SaveUserMessage(UserMessage) returns (Ok);
	// 获取好友列表
	rpc GetFriendList(ProcessId) returns (Users);
	// 添加好友
	rpc AddFriend(Id2) returns (User);
	// 通过用户名称模糊查询用户
	rpc GetFuzzyUserByUserName(Name) returns (Users);
	// 同意新好友请求
	rpc AgreeNewFriend(Id2) returns (User);
	// 拒绝新好友请求
	rpc RefuseNewFriend(Id2) returns (Ok);

	// ================================================群聊模块================================================
	// 获取群聊会话
	rpc GetGroupSession(ProcessId) returns (GroupMessages);
	// 获取新群聊信息
	rpc GetNewGroup(ProcessId) returns (NewGroupMessages);
	// 发送群聊消息
	rpc SendGroupMsg(GroupMessage) returns (SendGroupMsgRes);
	// 获取群聊列表
	rpc GetGroupList(ProcessId) returns (Groups);
	// 添加群聊
	rpc AddGroup(AddGroupReq) returns (AddGroupRes);
	// 通过群聊名称模糊查询群聊
	rpc GetFuzzyGroupByGroupName(Name) returns (Groups);
	// 同意新群友请求
	rpc AgreeNewGroup(AgreeNewGroupReq) returns (AgreeNewGroupRes);
	// 拒绝新群友请求
	rpc RefuseNewGroup(Id2) returns (Ok);
}

message ProcessId {
	uint32 id = 1;
}

message Id2 {
    uint32 senderId = 1;
	uint32 processId = 2;
}

message Ids {
	repeated uint32 ids = 1;
}

message Name {
	string name = 1;
}

message User {
	uint32 ID = 1;
	// 名称
	string Name = 2;
	// 个人介绍
	string Introduce = 3;
}

message Users {
	repeated User users = 1;
}

message UserMessages {
    repeated UserMessage msgs = 1;
}

message UserMessage {
	uint64 ID = 1;
	// 发送方id
	uint32 SenderId = 2;
	// 接收方id
	uint32 ReceiverId = 3;
	// 消息类型 文字为0, 图片为1, 音频为2
	uint32 Type = 4;
	// 消息内容
	string Content = 5;
}

message GroupMessage {
	uint64 ID = 1;
    // 群聊id
    uint32 GroupId = 2;
    // 发送方id
    uint32 SenderId = 3;
    // 发送方名称
    string SenderName = 4;
    // 消息类型 文字为0, 图片为1, 音频为2
    uint32 Type = 5;
    // 消息内容
    string Content = 6;
}

message GroupMessages {
	repeated GroupMessage msgs = 1;
}

message NewGroupMessage {
	uint32 GroupId = 1;
    string Username = 2;
    string GroupName = 3;
}

message NewGroupMessages {
	repeated NewGroupMessage msgs = 1;
}

message SendGroupMsgRes {
	Ids userIds = 1;
	GroupMessage Message = 2;
}

message Group {
	uint32 ID = 1;
	// 群聊名称
	string Name = 2;
	// 群主id
	uint32 MasterId = 3;
	// 群聊简介
	string Introduce = 4;
}

message Groups {
	repeated Group groups = 1;
}

message AddGroupReq {
	Id2 id2 = 1;
	string senderName = 2;
}

message AddGroupRes {
	uint32 masterId = 1;
	uint32 groupId = 2;
	string groupName = 3;
}

message AgreeNewGroupReq {
	string senderName = 1;
	uint32 groupId = 2;
}

message AgreeNewGroupRes {
	Group group = 1;
	uint32 receiverId = 2;
}

message Ok {
	bool ok = 1;
}
```