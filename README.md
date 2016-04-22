# moku hybrid协议

注册 mokuapi://协议进行h5与客户端的通讯

完整的协议为组成
mokuapi://[api的名字string型]:[当前会话id，int型]/?[传输给客户端的参数，JSONString，encode过，客户端拿到需要decode]

客户端截获上述规则的url后，不做跳转处理。并解析出相关参数做对应动作，处理完后通过给到前端回调：
```
loadUrl('javascript:window.mokuapi_123&&window.mokuapi_123({"code":"SUCCESS","data":"xxx"})');
```
其中 
1. mokuapi_123由2部分组成“mokuapi_”为固定部分，后面123数字会h5调用时传入的会话id
2. {"code":"SUCCESS","data":"xxx"} 为JSON格式（也可以为JSONstring类型，看如何方便）。code为返回码，成功时为“SUCCESS”，其它情况均用大写字母告之，如超时TIMEOUT，文件找不到FILE_NOT_FOUNT之类错误码

## 这里定义几个api

### 复制文本到粘贴板

```
 mokuapi://clipboard.copy:123/?{text:'需要粘贴的文字'}
```

回调给前端的参数
```
{
    "code": "SUCCESS"
}
```


### 保存图片至本地

```
 mokuapi://image.save:123/?{image:[url1,url2,url3]}
```

回调给前端的参数
```
{
    "code": "SUCCESS" // 部分失败的情况需要确定
}
```

### 跳转到客户端各个界面

```
 mokuapi://navigator:123/?{url:'xxx.xxx.xxx'}
 // 其中url为客户端界面指定的导航链接
```

回调给前端的参数
```
{
    "code": "SUCCESS" 
}
```

### 

