# PPCom Web SDK

Web SDK 帮助开发者在 Web 端集成 PPCom。

----

### 集成 Web SDK

#### 嵌入代码
PPKefu 的**设置-组件代码**栏目提供了最简版本的嵌入代码。要在某一个网页中集成 PPCom，只需要复制嵌入代码并将其插入到网页 html 文件的 **head** 标签之中即可。

嵌入代码如下：

```html
<script>
window.ppSettings = {
    // replace app_uuid
    app_uuid:'my-app-uuid'
};
(function(){var w=window,d=document;function l(){var a=d.createElement('script');a.type='text/javascript';a.async=!0;a.src='https://ppmessage.com/ppcom/assets/pp-library.min.js';var b=d.getElementsByTagName('script')[0];b.parentNode.insertBefore(a,b)}w.attachEvent?w.attachEvent('onload',l):w.addEventListener('load',l,!1);})()
</script>
```

> 如何找到 app_uuid？，`PPKefu／设置／开发者`，或直接使用 `PPKefu／设置／组件代码` 已经准备好的嵌入代码。

> 测试这段代码的方法很简单，打开任何一个网站，比如 https://jd.com， 在网页空白处点击鼠标右键，选择 “检查” （Inspect），复制嵌入代码（app_uuid 已经替换）到 ‘控制台’（console）中，去掉 <script></script>这个外围的标签。回车，PPCom 的聊天启动按钮就会在网站的右下角显示出来。

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
	// app_uuid，必填字段
	app_uuid: "replace_with_your_app_uuid",

    // 用户唯一ID，对于注册用户是必填字段，网站唯一标记客户的ID，如果是数字ID，转成string
    user_uuid: "123",

    // 用户是什么时候注册的，TIMESTAMP，毫秒级别
    user_createtime: 1232132312,
    
    // 用户email，可选字段，
	user_email: "somebody.web@ppmessage.com",
    
    // 用户姓名，可选字段
    user_name: "张三",

    // 用户头像，可选字段
    user_icon: "http://xxxx.com/avatar.png",

    // 用户手机，可选字段
    user_mobile: "+8613999881122",
    
	// 语言配置，可选字段，zh-CN:"中文"，en:"英文", 默认为 zh-CN， 决定PPCom界面显示语言
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

