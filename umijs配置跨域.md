### Umijs 配置跨域

+ 直接在config/config.js下配置

  ```js
  "proxy": {
      "/api": {
        "target": "http://localhost:3001",
        "changeOrigin": true,
        "pathRewrite": { "^/api" : "" }
      }
    }
  ```

  

注：**浏览器network下request url 的地址不会改变，还是你当前的http://本地地址/api/xxxx**

​		但是能够跨域了。

​	**代理只是服务请求代理，这个地址是不会变的。 原理可以简单的理解为，在本地启了一个服务，你先请求了本地的服务，本地的服务转发了你的请求到实际服务器。所以你在浏览器上看到的请求地址还是http://localhost:8000/xxx 。以服务端是否收到请求为准**