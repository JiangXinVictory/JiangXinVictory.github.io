---
title: h5与客户端bridge交互方法记录
date: 2020-01-10 08:56:04
tags:
    - javaScript
---

举报项目h5与客户端通过JS Bridge进行交互的方法
<!-- more -->

```
//设备类型判断
function deviceType() {
	var isIDevice = (/iphone|ipod/gi).test(navigator.platform),
		isIDeviceIpad = (/ipad/gi).test(navigator.platform),
		isAndroid = (/Android/gi).test(navigator.userAgent);
	if ((isIDevice || isIDeviceIpad) && !isAndroid) {
		return "isIos";
	} else if (isAndroid) {
		return "isAndroid";
	}

}

/*第三方交互库需要，已经区分ios和安卓*/
function setupWebViewJavascriptBridge(callback) {
	if (deviceType() == "isIos") {
		if (window.WebViewJavascriptBridge) {
			return callback(WebViewJavascriptBridge);
		}
		if (window.WVJBCallbacks) {
			return window.WVJBCallbacks.push(callback);
		}
		window.WVJBCallbacks = [callback];
		var WVJBIframe = document.createElement('iframe');
		WVJBIframe.style.display = 'none';
		WVJBIframe.src = 'wvjbscheme://__BRIDGE_LOADED__';
		document.documentElement.appendChild(WVJBIframe);
		setTimeout(function () {
			document.documentElement.removeChild(WVJBIframe)
		}, 0)
	} else if (deviceType() == "isAndroid") {
		if (window.WebViewJavascriptBridge) {
			callback(WebViewJavascriptBridge)
		} else {
			document.addEventListener(
				'WebViewJavascriptBridgeReady'
				, function () {
					callback(WebViewJavascriptBridge)
				},
				false
			);
		}
	}
}

//初始化函数,callback为使用逻辑函数
function initJsNative(callback) {
	setupWebViewJavascriptBridge(function (bridge) {
		if (deviceType() == "isAndroid") {
			//若果是安卓必须要注册bridge.init
			bridge.init(function (message, responseCallback) {});
		}
		callback(bridge);
	});
}


var jsNativeCommon = function (bridge) {
	/*交互的所有JS方法都要放在此处注册，才能调用通过JS调用Native或者让Native调用这里的JS*/

	//外部浏览器中打开页面
	this.nativeOpenWebView = function nativeOpenWebView(url) {
		console.log('外部浏览器打开页面')
		bridge.callHandler('openNav', {"url": url}, function (response) {})
	}

	this.nativeShowMessage = function nativeOpenWebView(msg) {
		console.log('显示消息')
		bridge.callHandler('showMessage', {"msg": msg}, function (response) {})
	}

	this.nativeOpenCamera = function nativeOpenWebView(element) {
		console.log('打开相机')
		bridge.callHandler('openCamera', {element:element}, function (response) {})
	}

	this.nativeSendMessage = function nativeOpenWebView(msg) {
		console.log('发送消息')
		bridge.callHandler('sendMessage', {msg:msg}, function (response) {})
	}

	//js传image的url数组到native
	this.nativeGetImageArr = function(urlAry){
		console.log('js传图片url1')
		bridge.callHandler('nativeGetImageArr', {"urlAry": urlAry}, function (response) {});
	}

	//js传image的url数组到native
	this.nativeLoadImage = function(urlAry){
		console.log('js传image的url2')
		bridge.callHandler('nativeLoadImage', {"urlAry": urlAry}, function (response) {});
	}
	this.jsLoginSuccess = function jsLoginSuccess(userInfo) {
		bridge.callHandler('JSLoginSuccess', {"userInfo": userInfo}, function (response) {})
	}
	//调用native图片放大
	this.nativeTapImage = function (url) {
		bridge.callHandler('nativeTapImage', {"url": url}, function (response) {});
	}
	bridge.registerHandler('nativeSendUserInfo', function (data,callback) {
		return callback('发送成功');
	});
	bridge.registerHandler('nativeIsLoginJS', function (data,callback) {
		 var userInfo = getSessionStorage("userInfo");
		 return callback(userInfo);
	});
	bridge.registerHandler('navtiveLoginOutJs', function (data,callback) {
        removeSessionStorage("userInfo");
		return callback('退出成功');
	});
	bridge.registerHandler('receiveImg', function (data) {
		console.log('展示图片');
	});
    bridge.registerHandler('nativeSendDeviceInfo', function (data, callback) {
        window.reportDevideInfo = data
    });
}
```
1.组件内调用
```
import { initJsNative, jsNativeCommon } from 'Brideg'
function () {
    // 初始化交互js
    let that = this;
    let commonJs = "";
    function callbackJsNative(bridge){
        commonJs = new jsNativeCommon(bridge);
        that.commonJs = commonJs;
    }
    initJsNative(function(bridge){
        callbackJsNative(bridge);
    });
    // commonJs.nativeOpenCamera(str);
    // delete window.WebViewJavascriptBridge;
}
```

2.直接调用
```
var commmonJs = "";
function callbackJsNative(bridge){
    commmonJs = new jsNativeCommon(bridge);
}
initJsNative(function(bridge){
    callbackJsNative(bridge);
});
```
