---
title: 微信小程序登录以及session_key数据解密
date: 2019-12-06 16:57:14
tags:
---

微信小程序登录 & 手机号获取
<!-- more -->

> 记一次小程序开发中，关于code的正确使用。
>
>最近两天在搞微信小程序的用户登录以及手机号获取，登录没什么问题，但是到手机号获取的时候，发现会有提示code失效的情况，后来仔细看了看小程序的文档，发现在登录态有效情况下，再次使用wx.login会导致登录态刷新，再结合后端的某些逻辑，导致解密失败。

```
onLaunch () {
    wx.login({
        success (res) {
            wx.setStorageSync('code', res.code)
        }
    })
},

// 授权回调
async bindGetUserInfo (res) {
    let code = await this.getCode()
    /*
    * 将res的内容以及getCode获取到的code，提交到服务器
    * 如果code有效，直接返回code，无效的话，重新生成code
    * 服务器端逻辑，使用code生成session_key后，需要将code和session_key保存，在code有效期内，前端不会再获取新的code，所以需要使用对应的session_key进行数据解密
    * 若重新执行wx.login，可能会刷新授权时的登录态，此时即使重新生成session_key，也不可能将之前的数据解密成功
    */
},

getCode () {
    let code = wx.getStorageSync('code')
    return new Promise((resolve, reject) => {
        wx.checkSession({
            success () {
                resolve(code)
            },
            fail () {
                wx.login({
                    success (res) {
                        wx.setStorageSync('code', res.code)
                        resolve(code)
                    }
                })
            }
        })
    })
}
````