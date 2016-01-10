## ShareSDK-iOS (v3.x) for Cocos2d-x 
* Cocos2d-x Developers to easily integrate ShareSDK.

## Content
* Getting started
    * [1、New Project and Download ShareSDK](#Download)
    * [2、initialization ShareSDK and set up social platforms](#init)
    * [3、call ShareSDK's interface](#interface)
* Notice
    * [Platform parameter configuration description](#SocialConfiguration)

## <a id="Download"></a>1、New Project and Download ShareSDK
* 1、Cocos2d-x's environment: [网页链接](http://www.jianshu.com/p/676f2e9096d4).
* 2、ShareSDK For Cocos2d-x (iOS) is rely on ShareSDK For iOS.So you must download the following two sections：

  （1）Download ShareSDK For iOS：[网页链接](http://www.mob.com/#/downloadDetail/ShareSDK/ios)
  
  （2）Download ShareSDK For Cocos2d-x：[网页链接](https://github.com/MobClub/New-C2DX-For-ShareSDK)
（include : demo）

![初始化](http://wiki.mob.com/wp-content/uploads/2015/12/tmp7f2f3116.png)

* drag two files inside the red box onto the new Cocos2d-x project

## <a id="init"></a> 2、initialization ShareSDK and set up social platforms
## iOS section

* 1、drag 'libraries' into the project
   
  
  ![img](http://wiki.mob.com/wp-content/uploads/2015/11/%E6%96%87%E6%A1%A32.png)

  Notice：Select the ShareSDK file and drag into your project (or just holding down the Control key on your keyboard and click your project,namely right-click your project, and choose “Add Files to …”). then you will see the following window, check the "Copy items into destination group's folder (if needed)" and click "Finish" button.



* 2、As shown below to make the next step  

  ![img](http://images2015.cnblogs.com/blog/708376/201512/708376-20151217215149318-1957357410.png)

* 3、add the necessary Framework

 Indispensable Framework：
 
  ```objc
  libicucore.dylib
  libz.dylib
  libstdc++.dylib
  JavaScriptCore.framework
  ```

 Optional Framework：
  
  ```objc
  necessary for the SSO Login of Sina weibo
  
  ImageIO.framework
  AdSupport.framework
  libsqlite3.dylib
  ```
  
  ```objc
  necessary for the SSO Login of WeChat
  
  libsqlite3.dylib
  ```
  
  ```objc
 necessary for the SSO Login of QZone or QQ Friend share
  
  libsqlite3.dylib
  ```
  
  ```objc
  necessary for Mail or SMS
  
  MessageUI.framework
  ```
  
  ```objc
  necessary for Google+ platform
  
  CoreMotion.framework
  CoreLocation.framework
  MediaPlayer.framework
  AssetsLibrary.framework
  ```

 The steps of adding the framework:

 ![img](http://wiki.mob.com/wp-content/uploads/2015/09/233D16A0-E241-4D4B-ACF2-4C03259F995A.png)

* 4、Each social platform configuration required（eg url schemes）You can reference documentation optional configuration item：[网页链接](http://wiki.mob.com/ios%E7%AE%80%E6%B4%81%E7%89%88%E5%BF%AB%E9%80%9F%E9%9B%86%E6%88%90/)

## Cocos2d-x section

* 1、Select the desired platform SDK and Cocos2d-x environment

    open C2DXShareSDK / iOS / C2DXiOSShareSDK.mm ，Comment unwanted Code

  ```
#define IMPORT_SINA_WEIBO_LIB               //import SinaWeibo Library
#define IMPORT_QZONE_QQ_LIB                 //import QQ Library
#define IMPORT_RENREN_LIB                   //import RENREN Library
#define IMPORT_GOOGLE_PLUS_LIB              //import GooglePlus Library
#define IMPORT_WECHAT_LIB                   //import Wechat Library
//#define IMPORT_ALIPAY_LIB                   //import Alipay Library
//#define IMPORT_KAKAO_LIB                    //import Kakao Library
```

    open C2DXShareSDK / C2DXShareSDKTypeDef.h ，Switching Cocos2d-x 2.x or 3.x environment
    
    ```
 //use Cocoa2D-X 2.x
 //#define UsingCocoa2DX2
 
 #ifdef UsingCocoa2DX2
 
 //...
```

* 2、modify "AppDelegate.cpp" 
  
  a、Open AppDelegate.cpp to import the .h file

  ```cpp
   #include "C2DXShareSDK.h"
  ```
  
  b、add the initialize code to the AppDelegate::applicationDidFinishLaunching() method (eg SinaWeibo、QQ、Wechat、Facebook、Twitter)
  
   ```cpp
    //Platforms
    __Dictionary *totalDict = __Dictionary::create();
    
    //Sina Weibo
    __Dictionary *sinaWeiboConf= __Dictionary::create();
    sinaWeiboConf->setObject(__String::create("568898243"), "app_key");
    sinaWeiboConf->setObject(__String::create("38a4f8204cc784f81f9f0daaf31e02e3"), "app_secret");
    sinaWeiboConf->setObject(__String::create("http://www.sharesdk.cn"), "redirect_uri");
    stringstream sina;
    sina << cn::sharesdk::C2DXPlatTypeSinaWeibo;
    totalDict->setObject(sinaWeiboConf, sina.str());
    
    //Wechat
    __Dictionary *wechatConf = __Dictionary::create();
    wechatConf->setObject(__String::create("wx4868b35061f87885"), "app_id");
    wechatConf->setObject(__String::create("64020361b8ec4c99936c0e3999a9f249"), "app_secret");
    stringstream wechat;
    wechat << cn::sharesdk::C2DXPlatTypeWechatPlatform;
    totalDict->setObject(wechatConf, wechat.str());
    
    //QQ
    __Dictionary *qqConf = __Dictionary::create();
    qqConf->setObject(__String::create("100371282"), "app_id");
    qqConf->setObject(__String::create("aed9b0303e3ed1e27bae87c33761161d"), "app_key");
    stringstream qq;
    qq << cn::sharesdk::C2DXPlatTypeQQPlatform;
    totalDict->setObject(qqConf, qq.str());
    
    //Facebook
    __Dictionary *fbConf = __Dictionary::create();
    fbConf->setObject(__String::create("107704292745179"), "api_key");
    fbConf->setObject(__String::create("38053202e1a5fe26c80c753071f0b573"), "app_secret");
    stringstream facebook;
    facebook << cn::sharesdk::C2DXPlatTypeFacebook;
    totalDict->setObject(fbConf, facebook.str());
    
    //Twitter
    __Dictionary *twConf = __Dictionary::create();
    twConf->setObject(__String::create("LRBM0H75rWrU9gNHvlEAA2aOy"), "consumer_key");
    twConf->setObject(__String::create("gbeWsZvA9ELJSdoBzJ5oLKX0TU09UOwrzdGfo9Tg7DjyGuMe8G"), "consumer_secret");
    twConf->setObject(__String::create("http://www.mob.com"), "redirect_uri");
    stringstream twitter;
    twitter << cn::sharesdk::C2DXPlatTypeTwitter;
    totalDict->setObject(twConf, twitter.str());
    
    //Log in to http://reg.sharesdk.cn/ to register to be a Mob developer , and click here to create a Mob application, then you will get the Appkey,and fill the Appkey to the fisrt parameter
    cn::sharesdk::C2DXShareSDK::registerAppAndSetPlatformConfig("8e3320a36606", totalDict); 
```

[app_key、app_secret..., Different sharing platform may be different，u can refer to this](#SocialConfiguration)

## <a id="interface"></a>3、3、call ShareSDK's interface

## Share
* 1、construct the share content and share it，Examples are as follows：

```cpp
reqID += 1; // Sharing Count
    
    __Dictionary *content = __Dictionary::create();
    content -> setObject(__String::create("分享文本"), "text");  
    content -> setObject(__String::create("HelloWorld.png"), "image");
    content -> setObject(__String::create("测试标题"), "title"); 
    content -> setObject(__String::create("http://www.mob.com"), "url"); 
    content -> setObject(__String::createWithFormat("%d", cn::sharesdk::C2DXContentTypeWebPage), "type"); 
```

* 2、call Share method：

```cpp
    C2DXShareSDK::showShareMenu(reqID,NULL,content,100,100,shareContentResultHandler); //4th and 5th parameters to setup the Coordinate points in iPad
```

* 3、Setting Share callback method: shareContentResultHandler，Examples are as follows：
 
  ```cpp
//Share callback method
void shareContentResultHandler(int seqId, cn::sharesdk::C2DXResponseState state, cn::sharesdk::C2DXPlatType platType, __Dictionary *result)
{
    switch (state)
    {
        case cn::sharesdk::C2DXResponseStateSuccess:
        {
            log("Success");
        }
            break;
        case cn::sharesdk::C2DXResponseStateFail:
        {
            log("Fail");
            //error messages
            __Array *allKeys = result->allKeys();
            allKeys->retain();
            for (int i = 0; i < allKeys-> count(); i++)
            {
                __String *key = (__String*)allKeys->getObjectAtIndex(i);
                Ref *obj = result->objectForKey(key->getCString());
                
                log("key = %s", key -> getCString());
                if (dynamic_cast<__String *>(obj))
                {
                    log("value = %s", dynamic_cast<__String *>(obj) -> getCString());
                }
                else if (dynamic_cast<__Integer *>(obj))
                {
                    log("value = %d", dynamic_cast<__Integer *>(obj) -> getValue());
                }
                else if (dynamic_cast<__Double *>(obj))
                {
                    log("value = %f", dynamic_cast<__Double *>(obj) -> getValue());
                }
            }
        }
            break;
        case cn::sharesdk::C2DXResponseStateCancel:
        {
            log("Cancel");
        }
            break;
        default:
            break;
    }
}
```

## Authorize

* 1、call authorize method

   ```cpp
  reqID += 1;
    
   C2DXShareSDK::getUserInfo(reqID, cn::sharesdk::C2DXPlatTypeSinaWeibo, getUserResultHandler);
   ```
   
   * 2、Setting callbacks to get user data: getUserResultHandler，Examples are as follows：

  ```cpp
void getUserResultHandler(int reqID, C2DXResponseState state, C2DXPlatType platType, __Dictionary *result)
{
    switch (state)
    {
        case cn::sharesdk::C2DXResponseStateSuccess:
        {
            log("Success");
            
            //Output messages
            try
            {
                __Array *allKeys = result -> allKeys();
                allKeys->retain();
                for (int i = 0; i < allKeys -> count(); i++)
                {
                    __String *key = (__String *)allKeys -> getObjectAtIndex(i);
                    Ref *obj = result -> objectForKey(key -> getCString());
                    
                    log("key = %s", key -> getCString());
                    if (dynamic_cast<__String *>(obj))
                    {
                        log("value = %s", dynamic_cast<__String *>(obj) -> getCString());
                    }
                    else if (dynamic_cast<__Integer *>(obj))
                    {
                        log("value = %d", dynamic_cast<__Integer *>(obj) -> getValue());
                    }
                    else if (dynamic_cast<__Double *>(obj))
                    {
                        log("value = %f", dynamic_cast<__Double *>(obj) -> getValue());
                    }
                }
                allKeys->release();
            }
            catch(...)
            {
                log("==============error");
            }
        }
            break;
        case cn::sharesdk::C2DXResponseStateFail:
        {
            log("Fail");
            //error messages
            __Array *allKeys = result->allKeys();
            allKeys->retain();
            for (int i = 0; i < allKeys-> count(); i++)
            {
                __String *key = (__String*)allKeys->getObjectAtIndex(i);
                Ref *obj = result->objectForKey(key->getCString());
                
                log("key = %s", key -> getCString());
                if (dynamic_cast<__String *>(obj))
                {
                    log("value = %s", dynamic_cast<__String *>(obj) -> getCString());
                }
                else if (dynamic_cast<__Integer *>(obj))
                {
                    log("value = %d", dynamic_cast<__Integer *>(obj) -> getValue());
                }
                else if (dynamic_cast<__Double *>(obj))
                {
                    log("value = %f", dynamic_cast<__Double *>(obj) -> getValue());
                }
            }
        }
            break;
        case cn::sharesdk::C2DXResponseStateCancel:
        {
            log("Cancel");
        }
            break;
        default:
            break;
    }
}
```

## <a id="SocialConfiguration"></a>Platform parameter configuration description
app_key、app_secret..., Different sharing platform may be different，u can refer to this

Platform            | General field    | General field        |General field       | iOS needs        | Android needs  |
--------------------|------------------|-----------------------|--------------------|------------------|-----------------------|
Sina Weibo          | app_key          | app_secret           |redirect_uri         | auth_type        |                      |
Tencent Weibo       | app_key          | app_secret           |redirect_uri         | ––               |                      |
douban              | api_key          | secret               | redirect_uri        | ––               |    | 
QQ series           | app_id           | app_key              | ––                  | auth_type        |    |
RENREN              | app_id            | app_key              |secret_key           | auth_type        |    |
Kaixin.com          | api_key          | secret_key           |redirect_uri         | ––               |    |
Facebook            | api_key          | app_secret           |  ––                 | auth_type        |    |
Twitter             | consumer_key     | consumer_secret      |redirect_uri         |  ––              |    |
GooglePlus          | client_id         | client_secret        |redirect_uri         | auth_type        |    |
Wechat series       | app_id          | app_secret           | ––                   |  ––             |    |
Pocket              | consumer_key     | ––                   |redirect_uri         | auth_type        |    |
Instragram          | client_id        | client_secret        |redirect_uri         |  ––              |    |
LinkedIn            | api_key          | secret_key           |redirect_uri         |  ––              |    |
Tumblr              | consumer_key     | consumer_secret      |callback_url         |  ––              |    |
Flicker             | api_key          | api_secret           | ––                  |  ––              |    |
YouDaoNote          | consumer_key     | consumer_secret      |oauth_callback       |  ––              |    |
Evernote            | consumer_key     |consumer_secret       |––                   | ––               |    |
AliPay              | app_id           | ––                   | ––                  | ––               |    |
Pinterest           | client_id         | ––                   |––                   | ––               |    |
Kakao series        | app_key          | rest_api_key         |redirect_uri         | auth_type        |     |
Dropbox             | app_key           | app_secret           |oauth_callback      |  ––              |     |
Vkontakte           | application_id    | secret_key           |––                   | ––              |     |
MingDao             | app_key           | app_secret           |redirect_uri        | ––              |     |
YiXin               | app_id            | app_secret           |redirect_uri        | auth_type        |     |
Instapaper          |consumer_key      | consumer_secret      |––                 |––               |    |
