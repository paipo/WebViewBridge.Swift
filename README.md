# WebViewBridge.Swift
IOS  javascript 与 Swift 代码交互
## 原生代码与 JS 的相互交互
### 原生代码调用 js handler
* html中或业务 js 中 添加 js handler
```javascript
ZHWVBridge.Core.registerJsHandler(
          "Device.updateAppVersion",
          function (version) {
            document.getElementById("native-version-container").textContent = version;
            return "js get version: " + version;
          });
```
ZHWVBridge.Core.registerJsHandler(handlerName, callback)
* 原生代码调用 js handler
```Swift
bridge.callJsHandler(
            "Device.updateAppVersion",
            args: ["1.2"],
            callback: { (data:AnyObject?) in
                // here data should be "js get version: 1.2"
                ...
        })
```
ZHWVBridge.Core.callNativeHandler(handlerName, 传递给原生handler的参数数组, 成功回调, 失败回调)

### Js 调用原生 handler
* 原生代码中, bridge 注册 native handler
```Swift
bridge.registerHandler("Image.updatePlaceHolder") { (args:[AnyObject]) -> (Bool, [AnyObject]?) in
            return (true, ["place_holder.png"])
        }
```
ZHWVBridge.Core.registerJsHandler(handlerName, callback)
* js 调用原生 handler
```javascript
ZHWVBridge.Core.callNativeHandler(
            "Image.updatePlaceHolder",
            [],
            function(placeHolder) {
              var items = document.getElementsByTagName('img');
              for (var i = 0, count = items.length; i < count; ++i) {
                var item = items[i];
                if (item.src.toLocaleLowerCase() == "file:///default_cover") {
                  item.src = placeHolder;
                }
              }
            });
```
ZHWVBridge.Core.callNativeHandler(handlerName, 传递给原生handler的参数数组, 成功回调, 失败回调)


## 演示
![image](https://github.com/paipo/WebViewBridge.Swift/blob/master/screenshots_1.gif)
