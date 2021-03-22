## 	shake.js摇一摇不能在非https下使用

+ 即使在ios下手动启用，设备方向数据仍然被禁用
+ 所以在本地环境中，发现怎么摇也触发不了自定义事件shake
+ ![image-20210203165752759](/Users/notion/Library/Application Support/typora-user-images/image-20210203165752759.png)
+ 将下面网页挂到https网站下，就能触发shake摇一摇函数了

```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <script src="./shake.js"></script>
    <script>
      var myShakeEvent = new Shake({
        threshold: 15, // 摇动阈值
        timeout: 1000, // 事件发生频率，是可选值
      });

      开始监听;
      myShakeEvent.start();

      绑定函数;
      window.addEventListener("shake", shakeEventDidOccur, false);
      function shakeEventDidOccur() {
        //回调函数
        alert("shake!"); // do it;
      }
    </script>
  </body>
</html>

```

