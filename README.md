# moku hybrid协议

注册 moku://协议进行h5与客户端的通讯

完整的协议为组成
moku://[api的名字string型]?[传输给客户端的参数, 里面固定参数sid为当前会话id，JSONString，encode过，客户端拿到需要decode]

客户端截获上述规则的url后，不做跳转处理。并解析出相关参数做对应动作，处理完后通过给到前端回调：
```
loadUrl('javascript:window.moku_123&&window.moku_123({"code":"SUCCESS","data":"xxx"})');
```
其中   
1. mokuapi_123由2部分组成“moku_”为固定部分，后面123数字会h5调用时传入的会话id  
2. {"code":"SUCCESS","data":"xxx"} 为JSON格式（也可以为JSONstring类型，看如何方便）。  
3. code为返回码，成功时为“SUCCESS”，其它情况均用大写字母告之，如超时TIMEOUT，文件找不到FILE_NOT_FOUNT之类错误码  
4. data非必须。  

## 这里定义几个api

### 复制文本到粘贴板

```
 moku://clipboard.copy?{'sid':1,'text':'需要粘贴的文字'}
```

回调给前端的参数
```
{
    "code": "SUCCESS"
}
```


### 保存图片至本地

```
 moku://image.download?{'sid':2,'images':[url1,url2,url3]}
```

回调给前端的参数
```
{
    "code": "SUCCESS" // 部分失败的情况需要确定
}
```

### 快捷下载：粘贴文本以及下载图片列表，并弹出结果界面
```
 moku://navigator.feed.download?{'sid':3,'text':'需要粘贴的文本','images':[url1,url2,url3]}
 // 其中url为客户端界面指定的导航链接
```

### 跳转到客户端各个界面

```
 moku://navigator?{'sid':3,'url':'xxx.xxx.xxx'}
 // 其中url为客户端界面指定的导航链接
```

回调给前端的参数
```
{
    "code": "SUCCESS" 
}
```

### 云库h5下拉刷新后需要通知客户端当前拿到的最大nodeId  

```
moku://feed.notify?{sid:4,'nodeId':55,'gid':0}
```

回调给前端的参数
```
{
    "code": "SUCCESS" 
}
```

========================================================
调皮的分割线
========================================================
# 前端调用接口列表

需要moku-hybrid.js

### 粘贴文本  
```
/**
 * 将文字复制至粘贴版  
 * @param [string] 需要粘贴的文本内容
 * @callback [function] 客户端回调
 */
Moku.copyToClipboard(params, callback);
```

### 下载图片url至本地  
```
/**
 * 下载图片url至本地  
 * @param [string]|[array] 需要下载的图片url。当为1个图片时，可直接传一个url；当为多个时用数组传递
 * @callback [function] 客户端回调
 */
Moku.downloadImages(params, callback);
```

###  云库下载界面    
```
/**
 * 云库h5下拉刷新后需要通知客户端当前拿到的最大nodeId
 * @param [object]   
 * @param.text [string] 需要粘贴的文本
 * @param.images [array] 需要保存至相册的图片url列表
 * @callback [function] 客户端回调  可缺省
 */
Moku.feedDownLoad(params, callback);
```

###  通知客户端云库页面拿到的最大nodeId   

```
/**
 * 云库h5下拉刷新后需要通知客户端当前拿到的最大nodeId
 * @param [object]   
 * @param.nodeId [int] 当前拿到的最大nodeId
 * @callback [function] 客户端回调  可缺省
 */
Moku.feedNotify(params, callback);
```




