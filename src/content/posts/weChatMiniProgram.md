---
title: 微信小程序
published: 2023-11-01
tags: [微信小程序, 记录]
category: 前端
draft: false
---

# 微信小程序

## 起步

### 什么是小程序？

**小程序是什么呢？**

- 小程序（Mini Program）是一种不需要下载安装即可使用的应用，它实现了“触手可及”的梦想，使用起来方便快捷，用完即走
- 事实上，目前小程序在我们生活中已经随处可见（特别是这次疫情的推动，不管是什么岗位、什么年龄阶段的人，都哪都需要打开健康码）
- 最初我们提到小程序时，往往指的是 微信小程序
- 但是目前小程序技术本身已经被各个平台所实现和支持
- 目前常见的小程序: 微信小程序、支付宝小程序、淘宝小程序、抖音小程序、头条小程序、QQ小程序、美团小程序等

**小程序与普通网页开发的区别**

- 网页运行在浏览器环境中, 小程序运行在微信环境中
- 由于运行环境的不同，所以小程序中，无法调用 DOM 和 BOM 的 API，但是，小程序中可以调用微信环境提供的各种 API，如：地理定位，扫码，支付...
- 网页的开发模式：浏览器 + 代码编辑器
- 小程序有自己的一套标准开发模式：申请小程序开发账号 + 安装小程序开发者工具 + 创建和配置小程序项目

**开发小程序的技术选型**

- 原生小程序开发
  - 微信小程序：https://developers.weixin.qq.com/miniprogram/dev/framework/
  - 微信小程序主要技术包括：WXML、WXSS、JavaScript；
  - 支付宝小程序：https://opendocs.alipay.com/mini/developer
  - 支付宝主要技术包括：AXML、ACSS、JavaScript；
- 选择框架开发小程序：
  - mpvue是一个使用Vue开发小程序的前端框架，也是 支持 微信小程序、百度智能小程序，头条小程序 和 支付宝小程序
  - mpvue框架在2018年之后就不再维护和更新了，所以目前已经被放弃
  - WePY (发音: /'wepi/)是由腾讯开源的，一款让小程序支持组件化开发的框架，通过预编译的手段让开发者可以选择自己喜欢的开发风格去开发小程序
  - WePY框架目前维护的也较少，在前两年还有挺多的项目在使用，不推荐使用
  - uni-app由DCloud团队开发和维护,是一个使用 Vue 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/飞书/QQ/快手/钉钉/淘宝）、快应用等多个平台。
  - uni-app目前是很多公司的技术选型，特别是希望适配移动端App的公司
  - taro，由京东团队开发和维护，是一个开放式 跨端 跨框架 解决方案，支持使用 React/Vue/Nerv 等框架来开发 微信 / 京东 / 百度 / 支付宝 / 字节跳动 / QQ / 飞书 小程序 / H5 / RN 等应用
  - taro因为本身支持React、Vue的选择，给了我们更加灵活的选择空间
  - 特别是在Taro3.x之后，支持Vue3、React Hook写法等

**uni-app和taro开发原生App**

- 无论是适配原生小程序还是原生App，都有较多的适配问题，所以你还是需要多了解原生的一些开发知识
- 产品使用体验整体相较于原生App差很多
- 也有其他的技术选项来开发原生App：ReactNative、Flutter

**开发小程序需要掌握的预备知识**

- 页面布局：WXML，类似HTML
- 页面样式：WXSS，几乎就是CSS(某些不支持，某些进行了增强，但是基本是一致的)
- 页面脚本：JavaScript+WXS(WeixinScript)
- 如果你之前已经掌握了Vue或者React等框架开发，那么学习小程序是更简单的，因为里面的核心思想都是一致的（比如组件化开发、数据响应式、mustache语法、事件绑定等等）

## 小程序的代码构成

# 自定义组件

## 自定义导航栏

# 地图

## 获取用户定位

### wx.getLocation

通过`wx.getLocation`我们得到用户的经纬度位置。

使用`wx.getLocation`之前需要简单的配置一下。因为获取用户地理位置的操作需要用户同意，所以我们在`app.json`文件里面加上配置：

```js
"permission": {
    "scope.userLocation": {
      "desc": "你的位置信息将用于小程序位置接口的效果展示"
    }
 },
 "requiredPrivateInfos": [
    "getLocation"
 ],
```

> [!CAUTION]
>
> 暂只针对如下类目的小程序开放，需要先通过类目审核，再在小程序管理后台，「开发」-「开发管理」-「接口设置」中自助开通该接口权限。 接口权限申请入口将于2022年3月11日开始内测，于3月31日全量上线。并从4月18日开始，在代码审核环节将检测该接口是否已完成开通，如未开通，将在代码提审环节进行拦截。
>
> 国内主体开发类目可查看：https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.getLocation.html

之后调用：

```js
wx.getLocation({
 type: 'wgs84',
 success (res) {
   const latitude = res.latitude
   const longitude = res.longitude
   const speed = res.speed
   const accuracy = res.accuracy
 }
})
```

其中`latitude`是纬度，`longitude`是经度。

到这里我们的第一步已经完成了。

### wx.getFuzzyLocation

获取当前的模糊地理位置。

#### 基础库版本要求

首先需要确认小程序基础库版本是否符合要求：

- 微信小程序基础库必须在2.25.0及以上版本才能使用`wx.getFuzzyLocation`接口
- 可以在微信开发者工具中查看和修改基础库版本

| **配置项** | **要求说明** |
| ---------- | ------------ |
| 基础库版本 | ≥2.25.0      |
| 开发者工具 | 最新版本     |

#### 接口权限配置

##### 小程序后台配置

登录微信小程序后台，进入"开发管理"-"接口设置"，找到`wx.getFuzzyLocation`接口并申请开通权限

##### app.json配置

使用`wx.getFuzzyLocation`之前也需要在`app.json`配置一下，在`app.json`文件里面加上配置：

```js
"permission": {
  "scope.userFuzzyLocation": {
    "desc": "你的位置信息将用于小程序位置接口的效果展示"
  }
},
"requiredPrivateInfos": ["getFuzzyLocation"]
```

##### 页面配置（可选）

```js
"permission": {
  "scope.userFuzzyLocation": {
    "desc": "位置信息效果展示"
  }
}
```

##### 代码实现

###### 授权调用流程

在调用`wx.getFuzzyLocation`之前，必须先进行授权：

```js
wx.authorize({
  scope: 'scope.userFuzzyLocation',
  success(res) {
    console.log(res);
    if(res.errMsg == 'authorize:ok'){
      wx.getFuzzyLocation({
        type: 'wgs84', // 或 'gcj02'
        success(res) {
          console.log('定位成功:', res);
        },
        fail(err) {
          console.log('定位失败:', err);
        }
      });
    }
  },
  fail(err) {
    console.log('授权失败:', err);
  }
});
```

获取对应的`latitude`和`longtitude`

###### 完整示例

```js
Page({
  onLoad() {
    this.getLocation();
  },
  getLocation() {
    wx.authorize({
      scope: 'scope.userFuzzyLocation',
      success: (authRes) => {
        if(authRes.errMsg === 'authorize:ok') {
          wx.getFuzzyLocation({
            type: 'gcj02',
            success: (locRes) => {
              console.log('获取到模糊定位:', locRes);
              // 处理定位数据
            },
            fail: (locErr) => {
              console.error('定位失败:', locErr);
            }
          });
        }
      },
      fail: (authErr) => {
        console.error('授权失败:', authErr);
        // 可以在这里提示用户手动授权
      }
    });
  }
});
```

##### 真机调试要点

1. 由于某些接口在微信开发者工具中可能无法正常使用，建议使用真机进行调试
2. 确保在真机上测试时，小程序的权限配置和代码逻辑都是正确的
3. 开发者工具中的模拟定位可能不会触发真实的权限校验

##### 常见问题处理

###### 授权失败处理

当授权失败时，可以引导用户手动打开设置页面进行授权：

```js
wx.openSetting({
  success(res) {
    console.log(res.authSetting);
    // 检查scope.userFuzzyLocation是否已授权
  }
});
```

###### 兼容性处理

建议添加接口兼容性判断：

```js
if (wx.getFuzzyLocation) {
  // 使用模糊定位
} else {
  // 降级处理方案，如使用wx.chooseLocation
}
```

##### 企业微信小程序注意事项

1. 企业微信小程序对定位接口的调用存在额外限制
2. 需要同时满足微信小程序和企业微信的接口调用规范
3. 建议优先考虑模糊定位方案，相比精确定位接口更容易通过审核
