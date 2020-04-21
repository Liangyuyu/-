---
marp: true
theme: default
class: invert
---
# 移动开发 lecture4
# <!-- fit --> **小程序开发入门（四）**

---
<!-- _backgroundColor: #005992  -->
<!-- _color: white -->
* ## 上节回顾
* ## 小程序原生UI组件与API简介（三）
* ## 项目案例（二） -- 录音功能
* ## 项目案例（三） -- 书架功能
* ## 项目案例（四） -- 仿新闻小应用“用户中心” （lecture6)

---
<!-- _backgroundColor: #005966  -->
<!-- _color: white -->
# 上节回顾
* ## 文件操作、网络请求
* ## 交互反馈
* ## 逻辑层对组件数据的操作
* ## 仿新闻小应用
  - 通过tarBar实现页面切换
  - 顶部滚动菜单
  - 新闻列表

---
## 上节回顾
* ## 文件操作、网络请求

---
* ## 交互反馈

---
* ## 逻辑层对组件数据的操作

---
* ## 仿新闻小应用
  - 通过tarBar实现页面切换
  - 顶部滚动菜单
  - 新闻列表

---
# [小程序整体架构](img/lec2/wxapp_arch.jpg)
![height:640px](img/lec2/wxapp_arch.jpg)


---
# [小程序框架](https://developers.weixin.qq.com/miniprogram/dev/reference/)
* ## [小程序框架UI组件](https://developers.weixin.qq.com/miniprogram/dev/component/)
* ## [小程序示例源码](https://github.com/wechat-miniprogram/miniprogram-demo)
  - 页面底部扫码
  - 开发工具打开说明
  - [微信小程序管理后台](https://mp.weixin.qq.com)
# [小程序官方API](https://developers.weixin.qq.com/miniprogram/dev/api)

---
<!-- _backgroundColor: #005966  -->
<!-- _color: white -->
# 小程序原生UI组件与API简介（三）
* ## 表单组件（二）
* ## 微信小程序API（二）
  
---
# 小程序表单组件（二）
* ## 地图组件
* ## checkbox与checkbox-group
* ## radio
* ## switch

---
# 小程序表单组件（三）（lecture6)
* ## canvas
* ## slider
* ## picker与picker-view
* ## form表单综合案例

---
* ## 地图组件
```
<!--test.wxml-->
<map id="map" longitude="113.324520" latitude="23.099994" scale="14" 
controls="{{controls}}" bindcontroltap="controltap" 
markers="{{markers}}" bindmarkertap="markertap"
 polyline="{{polyline}}" bindregionchange="regionchange" 
 bindtap="bindtapFun" show-location 
 style="width: 100%; height: 300px;"></map>


<map id="map" longitude="113.324520" latitude="23.099994" 
scale="14" circles="{{circles}}" show-location 
style="width: 100%; height: 300px;"></map>
```
[// test.js](code/map.js.html)

---
* ## checkbox与checkbox-group
```
<!--test.wxml-->
<!--checkbox-group 有个监听事件bindchange，监听数据选中和取消-->
<checkbox-group bindchange="listenCheckboxChange">
<!--这里用view显示内容，for循环写法 wx:for-items 默认item为每一项-->
    <view style="display: flex;"  wx:for-items="{{items}}">
    <!--value值和默认选中状态都是通过数据绑定在js中的-->
        <checkbox color="#ff5400" value="{{item.name}}" checked="{{item.checked}}"/>{{item.value}}
    </view>
</checkbox-group>

```
---
```
//test.js
Page({
  data: {
    items: [
      {name: 'CHN', value: '中国', checked: 'true'},
      {name: 'USA', value: '美国'}, {name: 'ENG', value: '英国'},
      {name: 'TUR', value: '法国'}, {name: 'BRA', value: '巴西'},
      {name: 'JPN', value: '日本'},  ]
  },
  checkboxChange: function(e) {
    console.log('checkbox发生change事件，携带value值为：', e.detail.value)
  },
   /**
   * 监听checkbox事件
   */
  listenCheckboxChange:function(e) {
      console.log('当checkbox-group中的checkbox选中或者取消是我被调用');
      //打印对象包含的详细信息
      console.log(e);
  },
})
```

---
* ## radio
```
<!--pages/viewPage8/viewPage8.wxml-->
<!--绑定事件，当点击radio时调用-->
<radio-group bindchange="radioChange">
<!--label常与radio和checkbox结合使用-->
    <label style="display: flex" wx:for-items="{{items}}">
        <radio color="#ff5400" value="{{item.name}}" checked="{{item.checked}}"/>{{item.value}}
    </label>
</radio-group>


<view class="section">自定义radio</view>
<view class="radio-group" >
        <label wx:for="{{areas}}" wx:for-item="area">
            <text bindtap="selectAreaOk" data-areaId="{{area.id}}">{{area.value}}</text> 
            <icon wx:if="{{area.isSelect}}" type="success" size="20"/> 
            <icon wx:else type="circle" size="20"/>
        </label>
</view>
```
[//test.js](code/radio.js.html)

---
* ### switch
```

<!--test.wxml-->
<view class="section">
  <switch checked bindchange="switchChange1" />
  <switch bindchange="switchChange2" />
</view>

<!--switch类型开关-->
<view class="section">switch类型开关</view>
<switch type="switch" checked="true" bindchange="listenerSwitch" />

<!--checkbox类型开关-->
<view class="section">checkbox类型开关</view>
<switch type="checkbox" bindchange="listenerCheckboxSwitch" />
<view class="section">按钮和开关组件联合使用</view>
<switch checked="{{switchSelect}}"/>
<button type="default" bindtap="openClick">打开</button>
<button type="default" bindtap="closeClick">关闭</button>

<view class="section">组件：滑动选择器slider</view>
 <slider bindchange="sliderBindchange" min="{{min}}" max="{{max}}" show-value/>
 <view>最小值：{{min}}</view>
 <view>最大值：{{max}}</view>
 <view>当前值：{{text}}</view>
 <view class="section"></view>
 <view>组件：开关组件switch</view>
 <switch checked type="switch" bindchange="switchBindchange"/>
 <view>当前状态：{{switchState}}</view>
<view class="section">END</view>
//test.wxss
.section {  margin-top: 50px;}
```
[//test.js](code/switch.js.html)

---
# 微信小程序API（二）
* ## 复习
  - ## 文件
  - ## 数据缓存
  - ## 界面交互
* ## 位置
* ## 设备（一）
* ## 媒体：图片、录音；音频、音乐与视频控制
  
---
# 微信小程序API（三）(lecture6)
* ## 网络WebSocket  (课堂作业：了解websocket协议)
* ## 设备（二）
* ## 开发API
  - ### 登录
  - ### 授权
  - ### 用户信息
  - ### 微信支付
  
---
* ## 位置
```

<button  bindtap="testButtonClick1" class="section">获取位置</button>

<button  bindtap="testButtonClick2" class="section">打开地图选择位置 </button>

<button  bindtap="testButtonClick3" class="section">使用微信内置地图查看位置 </button>

<view class="section2">地图组件控制 </view>
<map id="myMap" show-location />
<button type="primary" bindtap="getCenterLocation">获取位置</button>
<button type="primary" bindtap="moveToLocation">移动位置</button>

/**test.wxss**/
.section {
  margin-top: 10px;
}
.section2 {
  margin-top: 50px;
}

```
[//test.js](code/location.js.html)

---
* ## 设备（一）
```
view class="">4.6.1  系统信息 </view>
<button  bindtap="testButtonClick1" class="section">异步获取系统信息</button>
<button  bindtap="testButtonClick2" class="section">同步获取系统信息</button>
<button  bindtap="testAPI" class="section">API兼容判断</button>
<button  bindtap="testParameter" class="section">参数兼容</button>

<button wx:if="{{canIUse}}" open-type="contact"> 客服消息 </button>
<contact-button wx:else></contact-button>

<view class="section2">4.6.2  网络状态 </view>
<button  bindtap="networkButtonClick1" class="section">获取网络类型</button>
<button  bindtap="networkButtonClick2" class="section">监听网络状态变化</button>
<view class="section2">4.6.3  加速度计 </view>
<button  bindtap="accelerometerClick1" class="section">监听加速度数据
</button>
<button  bindtap="accelerometerClick2" class="section">开始监听加速度数据</button>
<button  bindtap="accelerometerClick3" class="section">停止监听加速度数据</button>

<view class="section2">4.6.4  罗盘 </view>
<button  bindtap="compassClick1" class="section">监听罗盘数据</button>
<button  bindtap="compassClick2" class="section">开始监听罗盘数据</button>
<button  bindtap="compassClick3" class="section">结束监听罗盘数据</button>

<view class="section2">4.6.5  拨打电话 </view>
<button  bindtap="phoneButtonClick" class="section">拨打电话
</button>

<view class="section2">4.6.6  扫码 </view>
<button  bindtap="scanCodeClick" class="section">扫码</button>
```

---
```
//test.js
//获取版本号
const SDKVersion = wx.getSystemInfoSync().SDKVersion || '1.0.0'
//版本号 转换
const [MAJOR, MINOR, PATCH] = SDKVersion.split('.').map(Number)
const canIUse = apiName => {
  if (apiName === 'showModal.cancel') {
    return MAJOR >= 1 && MINOR >= 1
  }
  return true
}
Page({
  data: {
    canIUse: canIUse('button.open-type.contact')
  },
  onLoad: function () {
    console.log('onLoad')
  },
```

---
```
// 异步获取系统信息
  testButtonClick1: function () {
    wx.getSystemInfo({
      success: function (res) {
        console.log(res.model)//手机型号
        console.log(res.pixelRatio)//设备像素比
        console.log(res.screenWidth)//屏幕宽度
        console.log(res.screenHeight)//屏幕高度
        console.log(res.windowWidth)//可使用窗口宽度
        console.log(res.windowHeight)//可使用窗口高度
        console.log(res.language)//微信设置的语言
        console.log(res.version)//微信版本号
        console.log(res.system)//操作系统版本
        console.log(res.platform)//客户端平台
        console.log(res.SDKVersion)//客户端基础库版本
      },
      fail: function (res) {
        console.log("获取系统信息失败")
      },
      complete: function (res) {
        console.log("获取系统信息完成")
      }
    })
  },
  // 同步获取系统信息
  testButtonClick2: function () {
    try {
      var res = wx.getSystemInfoSync()
      console.log(res.model)//手机型号
      console.log(res.pixelRatio)//设备像素比
      console.log(res.screenWidth)//屏幕宽度
      console.log(res.screenHeight)//屏幕高度
      console.log(res.windowWidth)//可使用窗口宽度
      console.log(res.windowHeight)//可使用窗口高度
      console.log(res.language)//微信设置的语言
      console.log(res.version)//微信版本号
      console.log(res.system)//操作系统版本
      console.log(res.platform)//客户端平台
      console.log(res.SDKVersion)//客户端基础库版本
    } catch (e) {
      // Do something when catch error
    }
  },
```


---
```
//API兼容判断
  testAPI: function () {
    // 如果支持openBluetoothAdapter API
    if (wx.openBluetoothAdapter) {
      wx.openBluetoothAdapter()
    } else {
      //不支持的话，弹出提示
      wx.showModal({
        title: '提示',
        content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后重试。'
      })
    }
  },
  //参数兼容
  testParameter: function () {
    wx.showModal({
      success: function (res) {
        //调用canIUse
        if (canIUse('showModal.cancel')) {
          console.log(res.cancel)
        }
      }
    })
  },
  // 获取网络类型
  networkButtonClick1: function () {
    wx.getNetworkType({
      success: function (res) {
        // 返回网络类型, 有效值：
        // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
        var networkType = res.networkType
        console.log(networkType)
      }
    })
  },
```

---
```
  //监听加速度数据
  accelerometerClick1: function () {
    wx.onAccelerometerChange(function (res) {
      console.log(res.x)
      console.log(res.y)
      console.log(res.z)
    })
  },
  //开始监听加速度数据
  accelerometerClick2: function () {
    wx.startAccelerometer({
      //成功
      success: function (res) {
      },
      //失败
      fail: function (res) {
      },
      //完毕
      complete: function (res) {
      },
    })
  },
  //停止监听加速度数据
  accelerometerClick3: function () {
    wx.stopAccelerometer({

      //成功
      success: function (res) {
      },
      //失败
      fail: function (res) {
      },
      //完毕
      complete: function (res) {
      },
    })
  },
```

---
```
  // 监听罗盘数据
  compassClick1: function () {
    wx.onCompassChange(function (res) {
      console.log(res.direction)
    })
  },

  // 开始监听罗盘数据
  compassClick2: function () {
    wx.startCompass({

      //成功
      success: function (res) {
      },
      //失败
      fail: function (res) {
      },
      //完毕
      complete: function (res) {
      },
    })
  },

  // 结束监听罗盘数据
  compassClick3: function () {
    wx.stopCompass({

      //成功
      success: function (res) {
      },
      //失败
      fail: function (res) {
      },
      //完毕
      complete: function (res) {
      },
    })
  },

```

---
```
//拨打电话
  phoneButtonClick: function () {
    wx.makePhoneCall({

      phoneNumber: '185****5656',//演示电话
      //成功
      success: function (res) {
      },
      //失败
      fail: function (res) {
      }
    })
  },

  // 扫码
  scanCodeClick: function () {
    wx.scanCode({
      //成功
      success: function (res) {
        console.log(res.result) //内容
        console.log(res.scanType)//类型
        console.log(res.charSet)//字符集
        console.log(res.path)//二维码携带的 path
      },
      //失败
      fail: function (res) {
      },
      //完成
      complete: function (res) {
      }
    })
  },
});
```

---
* ## 媒体
  - ### 图片
  - ###  录音
  - ###  音频、音乐控制
  - ###  视频控制

---
 - ### 图片 录音
```
<view class="">图片</view>
<button  bindtap="testButtonClick11" class="section"> 选择 </button>
<button  bindtap="testButtonClick12" class="section"> 预览 </button>
<button  bindtap="testButtonClick13" class="section"> 获取图片信息 </button>
<button  bindtap="testButtonClick14" class="section"> 保存图片到系统相册 </button> 

<view class="section2"> 录音 </view>
<button  bindtap="testButtonClick21" class="section">开始录音</button>
<button  bindtap="testButtonClick22" class="section">结束录音 </button> 

<view class="section2">音频播放控制 </view>
<button  bindtap="testButtonClick31" class="section">开始播放</button>
<button  bindtap="testButtonClick32" class="section">暂停播放 </button>
<button  bindtap="testButtonClick33" class="section">继续播放 </button>
<button  bindtap="testButtonClick34" class="section">结束播放</button> 
```

---
- ###  音频、音乐控制
```
<view class="section2">音乐播放控制和音乐组件  </view>
<button  bindtap="testButtonClick41" class="section">获取后台音乐播放状态</button>
<button  bindtap="testButtonClick42" class="section">播放音乐 </button>
<button  bindtap="testButtonClick43" class="section">暂停播放音乐</button>
<button  bindtap="testButtonClick44" class="section">控制音乐播放进度</button>
<button  bindtap="testButtonClick45" class="section">停止播放音乐</button> 

<view class="section">音乐播放组件  </view>
<audio poster="{{poster}}" name="{{name}}" author="{{author}}" src="{{src}}" id="myAudio" controls loop></audio>
<button type="primary" bindtap="audioPlay">播放</button>
<button type="primary" bindtap="audioPause">暂停</button>
<button type="primary" bindtap="audio16">设置当前播放时间为16秒</button>
<button type="primary" bindtap="audioStart">回到开头</button> 

<view class="section">背景音频管理器  </view>
<button type="primary" bindtap="backAudioPlay">播放</button>
<button type="primary" bindtap="backAudioPause">暂停</button>
<button type="primary" bindtap="backAudio16">设置当前播放时间为16秒</button>
<button type="primary" bindtap="backAudioStart">停止</button>

```

---
 - ###  视频控制
```
<view class="section2">视频与视频组件  </view>  
<video src="{{videoParh}}"></video>
<button  bindtap="testButtonClick51" class="section">拍摄视频或从手机相册中选视频</button>

<view class="section2">视频组件</view>  
<video id="myVideo" src="ccNews.mp4"   enable-danmu danmu-btn controls></video>
<input bindblur="bindInputBlur"/>
<button bindtap="bindSendDanmu">发送弹幕</button>
<button type="primary" bindtap="videoPlay">播放</button>
<button type="primary" bindtap="videoPause">暂停</button>
<button type="primary" bindtap="video16">设置当前播放时间为16秒</button>
```
[//test.js](code/media.js.html)

---

<!-- _backgroundColor: #238869  -->
<!-- _color: white -->
# 课堂作业（一）
* ### 作业要求
   花10分钟时间，在自己电脑的微信开发工具中编写调用微信“位置”相关API的程序，并进行线上调试，通过手机扫码运行，将运行结果截图。
* ### 提交方式
   将调研的初步结果，统一提交给班长（学委）后，由班长（学委）压缩后发给老师；调研的详细结果，写成一篇短文，提交到自己的“移动开发”课程学习github仓库中。

---
<!-- _backgroundColor: #005966  -->
<!-- _color: white -->
# 项目案例（二） -- 录音功能
* ## wx录音API
* ## 页面实现
* ## 逻辑实现 
* ## 拓展功能与后台功能思考

---
* ## wx录音API
* wx.startRecord
* wx.stopRecord
* wx.playVoice
---
```
<!--index.wxml-->
 <!--录音音量动画显示图片-->
<!--开始录音，显示该view-->
<view  wx:if="{{isSpeaking}}"  class="speakPicViewClass">
<!--音量图片1-->
<image wx:if="{{picId==1}}" class="imageClass" src="../../images/sound1.png" ></image>
<!--音量图片2-->
<image wx:if="{{picId==2}}" class="imageClass" src="../../images/sound2.png" ></image>
<!--音量图片3-->
<image wx:if="{{picId==3}}" class="imageClass" src="../../images/sound3.png" ></image>
<!--音量图片4-->
<image wx:if="{{picId==4}}" class="imageClass" src="../../images/sound4.png" ></image>
<!--音量图片5-->
<image wx:if="{{picId==5}}"class="imageClass" src="../../images/sound5.png" ></image>
 </view>

<!--显示录音的列表-->
<scroll-view>
<block  wx:for="{{voices}}">
    <view class="cellClass">
        <view class="cellRowClass" data-key="{{item.filePath}}" bindtap="playAudio" > 
            <view  class="dateClass">文件路径:{{item.filePath}}</view>
            <view  class="dateClass" >录音时间:{{item.createTime}}</view>
            <view  class="dateClass">文件大小:{{item.size}}KB</view>
        </view>  
    </view>
</block>
</scroll-view>

<!--录音按钮-->
 <view class="recordClass">
 <button class="recordButtonClass" bindtouchstart="touchDown" bindtouchend="touchUp">按住 录音</button>
 </view>
```

---
* ## 样式与逻辑实现
* ### 样式文件
  [//record.wxss](code/record.wxss.html)
* ### 逻辑控制
  [//record.js](code/record.js.html)


---
<!-- _backgroundColor: #005966  -->
<!-- _color: white -->
# 项目案例（三） -- 书架功能
* ## 精彩推荐模块
* ## 热门书籍模块
* ## 精品书籍模块

---
//app.json
```
{
    "pages": [
        "pages/home/home",
        "pages/myBooks/myBooks",
        "pages/search/search",
        "pages/detail/detail"
    ],
    "window": {
        "backgroundTextStyle": "dark",
        "navigationBarBackgroundColor": "#FFA589",
        "navigationBarTitleText": "首页",
        "navigationBarTextStyle": "white",
        "backgroundColor": "#EEEEEE",
        "enablePullDownRefresh":"true"
    },
    "tabBar": {
        "list": [
            {
                "pagePath": "pages/home/home","text": "朗读广场",
                "iconPath": "images/TabbarHomeN.png","selectedIconPath": "images/TabbarHomeS.png"
            },
            {
                "pagePath": "pages/search/search","text": "寻找朗读",
                "iconPath": "images/TabbarSearchN.png", "selectedIconPath": "images/TabbarSearchS.png"
            },{
                "pagePath": "pages/myBooks/myBooks","text": "我的朗读",
                "iconPath": "images/TabbarCollectN.png","selectedIconPath": "images/TabbarCollectS.png"
            }
        ],
        "borderStyle": "white",
        "backgroundColor": "#f0f0f0",
        "selectedColor": "#1aad16"
    }
}
```

---
```
<!--pages/home/home.wxml-->
<view class="mainClass">

<!--精彩推荐 栏目-->
  <view class="topBarClass">
    <image src="../../images/icon.png" class="icon"></image>
    <text>   精彩推荐</text>
    <image src="../../images/mark.png" class="book-icon"></image>
  </view>

  <!--顶部滚动视图-->
  <swiper indicator-dots="true" autoplay="true" interval="6000" duration="3000">
    <block wx:for="{{recommendBooks}}">
      <swiper-item>
        <image src="{{item.image}}" class="slide-image" data-purchase="{{item.url}}" bindtap="bindToDetailTap"/>
      </swiper-item>
    </block>
  </swiper>
```
```
<!--热门书单 标题栏目-->
  <view class="hot-books">
    <view class="hb-bar">
      <text> 热门书籍</text>
      <image src="../../images/mark.png" class="book-icon"></image>
    </view>

<!--热门书 板块-->
    <view class="hb-booklist">
      <view class="hb-book" wx:for="{{hotBooks}}" bindtap="bindToDetailTap" data-purchase="{{item.url}}" data-id="{{item.id}}">
        <image src="{{item.image}}"></image>
        <text class="book-name">{{item.name}}</text>
      </view>
    </view>
  </view>
```

---
```
  <!--精品书籍 标题模块-->
  <view class="boutique-books">
    <view class="bb-bar">
      <text> 精品书籍</text>
      <image src="../../images/mark.png" class="book-icon"></image>
    </view>

    <!--精品书籍列表-->
    <view class="bb-booklist">
      <view class="book" wx:for="{{boutiqueBooks}}" data-id="{{item.id}}" data-purchase="{{item.url}}" bindtap="bindToDetailTap">
        <image src="{{item.images.title_img}}" class="title-img"></image>
        <image src="{{item.images.author}}" class="author"></image>
        <text class="book-name">{{item.name}}</text>
        <text class="book-summary">{{item.miniSummary}}</text>
      </view>
    </view>
  </view>
</view>
```
* [wxss样式](code/myReading/pages/home/home.wxss)
* [页面逻辑](code/myReading/pages/home/home.js)
* [完整代码](code/myReading/)

---


<!-- _backgroundColor: #238869  -->
<!-- _color: white -->
# 课后作业
* 作业要求
   - 作业一：录音并播放录音，涉及的API调用
      + 录音API
      + 文件管理API
      + 音频控制API
   - 作业二：设计左右滚动界面，界面中的元素可以承载文字、图片、音频和视频
      + 上述界面的文字、图片、音频、视频需要设置一个初始源；
      + 上述界面的文字、图片、音频、视频的源可以通过界面下面的一些组件绑定的事件进行设置，并可在设置后在滚动界面中查看到源更新后的效果
* 提交方式
  - 提交到自己的“数据库原理”课程学习github仓库中。
  - 每次作业需要单独建立一个文件夹，并提供README文件
---

<!-- _backgroundColor: #df492e  -->
<!-- _color: white -->
# 个人项目
* ### 项目要求
   用微信官方或第三方开发工具，完成一个移动应用设计开发（微信小程序、手机App或H5网站），以下主题任选：
   - 记帐本应用
   - 在线小说阅读应用
   - 图片分享应用
   - 地图应用：场景自由设计（地图记帐、附近爱好者。。。）
   - 其他：题目和功能自拟

* ### 提交方式
  - 提交到自己的“数据库原理”课程学习github仓库中。
  - 个人项目需要单独建立一个文件夹，并提供README文件