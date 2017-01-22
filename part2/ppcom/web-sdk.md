# PPCom Web SDK

Web SDK 帮助开发者在 Web 端集成 PPCom。

----

#### 集成 Web SDK
集成 Web SDK 之前你应该在 PPConsole 创建一个客服团队，然后在**团队设置-基本信息**栏获取团队 uuid (下文的app_uuid），之后可以选择**嵌入代码**或者**加载文件**两种方式进行 Web 集成。

#### 嵌入代码
PPKefu 的**设置-组件代码**栏目提供了最简版本的嵌入代码。要在某一个网页中集成 PPCom，只需要复制嵌入代码并将其插入到网页 html 文件的 **head** 标签之中即可。

最简版的嵌入代码示例如下，你可以参考下文的 **Web SDK 接口** 修改 window.ppSettings 对象。如果你想要调用其他的接口，请使用下文的**加载文件**方式集成。

```html
<script>
window.ppSettings = {
    // replace app_uuid
    app_uuid:'my-app-uuid'
};
(function(){var w=window,d=document;function l(){var a=d.createElement('script');a.type='text/javascript';a.async=!0;a.src='https://ppmessage.com/ppcom/assets/pp-library.min.js';var b=d.getElementsByTagName('script')[0];b.parentNode.insertBefore(a,b)}w.attachEvent?w.attachEvent('onload',l):w.addEventListener('load',l,!1);})()
</script>
```

#### 加载文件
使用直接加载 Javascript 文件的方法集成 PPCom。

方法很简单，只需要将下列代码插入到html文件的**head**标签里，然后就可以在你的 Javascript 程序代码中调用 PPCom SDK接口。具体接口请参考下文的 **Web SDK 接口**。

```html
<script src="https://ppmessage.com/ppcom/assets/pp-library.min.js" type="text/javascript"></script>
```

#### Web SDK 接口

- 配置选项
  
  在调用`PP.boot`或`PP.update`时传入的参数和`window.ppSettings`格式一样。如果调用时不传入参数，会以`window.ppSettings`作为替代参数。
  
  `window.ppSettings`内容如下所示：

  ```javascript
  window.ppSettings = {
	// app_uuid，必填字段，是客服团队的uuid，可在PPConsole的 团队设置-基本信息 中找到
	app_uuid: "xxxxxx",
	
    // 第三方用户email，可选字段，不填则会以匿名用户身份启动PPCom，否则以具名用户身份启动PPCom
	user_email: "somebody.web@ppmessage.com",
    // 用户姓名，可选字段，客服看到的PPCom用户的名称
    user_name: "张三",
    // 用户头像，可选字段，客服看到的PPCom用户的头像
    user_icon: "http://xxxx.com/avatar.png",
	// 语言配置，可选字段，zh-CN:"中文"，en:"英文", 默认为中文，决定PPCom界面显示语言
	language: "zh-CN",
  };
  ```

- `PPCom.boot`
  
  以匿名或具名用户的身份启动 PPCom。一旦启动 PPCom，再次调用`PP.boot`不会重启 PPCom，即不会执行任何操作。如果确实需要重启 PPCom，你需要先调用`PP.shutdown`，然后调用`PP.boot`。
  
  参数可选，不填写参数的话，则默认使用`window.ppSettings`来启动`PPCom`。
  
  ```javascript
  PP.boot({
    app_uuid: 'your-app_uuid',
    user_name: 'ppcom-user-name',
    user_icon: 'ppcom-user-icon',
    language: 'zh-CN'
  }, function(isSucess, errorCode) {
    //回调函数
  });
  ```

- `PPCom.update`
  
  当需要更换 PPCom 用户，更新 PPCom 用户的信息（邮箱，头像，显示名称）时，调用 PP.update 方法。
  
  参数可选，不填写参数的话，则默认使用`window.ppSettings`来更新`PPCom`。
  
  ```javascript
  /**
   * @description 更新 PPCom
   * @return 无返回值
   */
  PP.update({
    app_uuid: 'your-app_uuid',
    user_email: 'somebody.web@ppmessage.com',
    user_name: 'new-user-name',
    user_icon: 'new-user-icon'
    user_language: 'zh-CN'
  });
  ```
  
- `PP.show`
 
  显示`PPCom`消息面板。消息面板指的是对话列表或对话界面。用户可以通过在对话列表和对话界面，点击最小化按钮隐藏消息面板。要重新显示消息面板，调用`PP.show`。

  ```javascript
  /**
   * @description 显示 PPCom
   * @return 无返回值
   */
  PP.show();
  ```

- `PP.hide`
 
  隐藏`PPCom`消息面板。消息面板指的是对话列表或对话界面。用户可以通过在对话列表和对话界面，点击最小化按钮关闭消息面板。
  
  ```javascript
  /**
   * @description 隐藏 PPCom
   * @return 无返回值
   */
  PP.dismiss();
  ```

- `PPCom.shutdown`
  
  完全移除PPCom，但是不会清空window.ppSettings对象。一旦关闭PPCom，如果想要重新显示PPCom，需要调用PP.boot方法。

  当用户注销的时候，可以调用此接口销毁用户数据，此接口将会移除掉PPCom。

  ```javascript
  /**
   * @description 关闭 PPCom
   * @return 无返回值
   */
  PP.shutdown();
  ```


#### 使用 Web SDK

当 PPCom 启动时，开发者应该根据网站的访客是否已经成为该网站的注册用户来调用 PPCom，如果 PPCom 启动时带着`ent_user_id`参数，则该用户为具名用户，否则为匿名用户。具名用户就是该用户是该网站的注册用户，并且告诉 PPCom 这个访客的 ent user id，这个 id 是网站用来跟踪访客的，不属于 PPMessage。如果没有 ent user id，那么 PPMessage 认为这个访客没有在这个网站上注册，即一个匿名访客。


考虑以下用户行为：

* **网站用户打开网站**

  
  如果你想让PPCom在网站一打开就自动出现，只需要创建`window.ppSettings`对象即可，SDK加载完成后会自动以此对象作为参数启动 PPCom。

  ```javascript
  window.ppSettings = {
      app_uuid: 'your-app-uuid',
      user_name: 'anonymous-user',
      user_icon: 'http://xxx/anonymous-user/icon.png',
      language: 'zh-CN'
  };
  ```
  
  如果你想要自己控制 PPCom 何时出现，则不要创建`window.ppSettings`对象，而是直接调用`PP.boot`方法。
  
  ```javascript
  PP.boot({
    app_uuid: 'your-app-uuid',
    user_name: 'anonymous-user',
    user_icon: 'http://xxx/anonymous-user/icon.png',
    language: 'zh-CN'
  });
  ```
  
* **网站用户登录**

 
  网站用户登录网站后，开发者需要以具名用户的身份更新PPCom。这样，当网站客服收到网站用户的消息时，就能知道该用户是网站的注册用户。
  
  ```javascript
  PP.update({
    app_uuid: 'your-app-uuid',
    ent_user_id: 123,
    ent_user_name: 'named-user',
  });
 
* **网站用户修改自身信息**

  网站用户修改了自身信息（显示名称、头像）后，开发者需要告诉 PPCom 更新用户信息。`user_email`标识需要更新的用户。
  
  ```javascript
  PP.update({
    app_uuid: 'your-app-uuid',
    user_email: 'nameduser@test.com',
    user_name: 'named-user-123',
    user_icon: 'http://xxx/named-user-123/icon.png',
    language: 'en'
  });
  ```
  
* **网站用户退出登录**

  网站用户退出登录后，开发者需要以匿名用户身份更新 PPCom。之前创建的匿名用户在本地已经缓存，所以此次不会创建新的匿名用户。
  
  ```javascript
    PP.update({
      app_uuid: 'your-app-uuid'
      user_name: 'anonymous-user',
      user_icon: 'http://xxx/anonymous-user/icon.png',
      language: 'zh-CN'
    });
  ```
  
#### 错误码

当`window.ppSettings`的配置信息有误而导致`PPCom`无法正常显示的时候，`PPCom`会在浏览器的`console`中打印出相应的错误信息和错误码。

下面是所有可能出现的错误码所对应的错误描述信息。

Error Code(错误码) | Error description(错误描述)
----------------- | -------------------------------
10000             | app_uuid 填写有误
10001             | 找不到 user_email 对应的用户，可能未注册或者邮箱填写有误
10002             | PPCom 暂时还无法运行在 IE9 或以下版本中
10003             | 无法连接PPMessage服务器
10004             | user_email 不是有效的邮箱
