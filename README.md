# Android社会化平台授权及分享组件

目前支持微信 微博 QQ三个平台

# How to use(基于android studio):
##配置
1. 将libsocialize module集成到你的项目中
2. 配置主module的清单文件，加入如下代码:
```
<!-- for 微信授权 -->
<activity
    android:name=".wxapi.WXEntryActivity"
    android:label="@string/app_name"
    android:exported="true"
    android:theme="@android:style/Theme.Translucent.NoTitleBar"/>
<!-- for QQ授权 -->
<activity
    android:name="com.tencent.tauth.AuthActivity"
    android:launchMode="singleTask"
    android:noHistory="true">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="1104681413" />
    </intent-filter>
</activity>

<!-- for QQ授权 -->
<activity
    android:name="com.tencent.connect.common.AssistActivity"
    android:configChanges="orientation|keyboardHidden"
    android:screenOrientation="behind"
    android:theme="@android:style/Theme.Translucent.NoTitleBar" />
```

3. 在安卓package目录下新建wxapi/WXEntryActivity类,继承BaseWXEntryActivity类,类体为空即可，之所以要有这个类，是因为微信授权/分享操作的回调只能反馈到此类.

4. 在主module的Application子类中初始化社会化平台的配置，示例代码:
``` java
//初始化平台配置
SocialPlatform.Configuration socialConfiguration = new SocialPlatform.Configuration();
socialConfiguration.configWeibo(ShareConfig.WEIBO_APP_KEY, ShareConfig.WEIBO_REDIRECT_URL, ShareConfig.WEIBO_SCOPE)
        .configWeChat(ShareConfig.WECHAT_APP_ID, ShareConfig.WECHAT_APP_SECRET)
        .configQQ(ShareConfig.QQ_APP_ID);
SocialPlatform.init(this, socialConfiguration);

//分享配置，在分享对话框中显示分享到哪些平台.
SocialShare.getInstance()
        .enableQQ()
        .enableWeChat(this)
        .enableWeChatMoment(this)
        .enableWeiBo();
```
##授权

1. 授权回调:
授权回调的逻辑集成在BaseAuthActivity类中，所以需要继承BaseAuthActivity, 在子类中处理授权回调，如和服务器交互，弹出toast等等。 但是在很多场景中，activity本身已有特定的父类，推荐的办法是：拷贝BaseAuthAcitivty到主module中，并将其父类修改成你需要的activity父类.

2. 使用授权功能 eg:
``` java
SocialAuth.getAuth(AuthType.WECHAT).authorize(activity, authCallback)
```

##分享
分享功能所在的acitivity须继承BaseShareActivity, 不过就实际的项目，基本需要将此类拷贝一份，再将此类的父类更改为你自定义的类。

分享信息包装在ShareInfo类中， 调用SocialShare类的showShareDialog方法，显示分享对话框，目前分享对话框显示在UI底部。
使用: 
SocialShare.getInstance().showShareDialog(getSupportFragmentManager(), shareInfo, sharecallback);

# 捐赠
此项目fork自markzhai的开源项目，深受启发，所以如果觉得此项目对您有用，请使用下面的二维码给他捐赠。

If you find this repository helpful, you may make a donation to me via alipay or wechat.
![alipay](http://blog.zhaiyifan.cn/images/donation-alipay.png "alipay") ![wechat](http://blog.zhaiyifan.cn/images/donation.jpg "wechat")
