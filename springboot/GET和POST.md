页面跳转通常使用 GET 请求而不是 POST 请求。POST 请求主要用于向服务器提交数据，而不是用于页面的导航。当你使用 POST 请求时，浏览器会将 POST 请求的响应内容作为结果返回给客户端，而不会执行页面跳转。
例如：jquery前端使用
`window.location.href = '/user/login'; `进行页面跳转时（use/login）为服务端代码
![[Pasted image 20240219162232.png]]
此处必须使用GetMapping，不能使用PostMapping，用了就报错。

POST数据交换方式：当请求头设置成'Content-Type': 'application/json'时，前端会将数据打包成json
后端交互json数据时不能用getParameter(),
应当这样
![[Pasted image 20240420030710.png]]
GET可以直接用request.getParameter()