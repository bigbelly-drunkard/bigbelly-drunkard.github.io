---
title: httponly原理？
date: 2024-03-05 12:25:54
mathjax: true
tags:
    - httponly
    - xss
categories:
    - 网络安全

---

## 设置了httponly后无法被javascript调用？那用户怎么发xhr请求？

浏览器自动发送带有`HttpOnly`标志的Cookie是其内置功能的一部分，不需要开发者或用户进行特别设置。当服务器通过设置一个带有`HttpOnly`标志的Cookie响应一个HTTP请求时，这个Cookie会被浏览器存储起来。之后，每当用户的浏览器向同一域名发送请求时，浏览器会自动附加这个域名下的所有Cookie，包括那些标记为`HttpOnly`的Cookie。

### 实现自动发送`HttpOnly` Cookie的步骤：

1. **服务器设置Cookie**：在服务器处理用户登录或其他需要设置Cookie的请求时，通过响应头`Set-Cookie`加上`HttpOnly`标志来设置Cookie。例如：

   ```
   复制代码Set-Cookie: sessionId=abc123; Path=/; HttpOnly
   ```

   这告诉浏览器保存这个名为`sessionId`的Cookie，并在之后的请求中自动发送给服务器，同时因为`HttpOnly`的存在，这个Cookie不能被客户端JavaScript访问。

2. **浏览器存储和管理Cookie**：一旦浏览器接收到这个设置了`HttpOnly`标志的Cookie，它会将其存储起来，并在之后向相同域名发送的所有请求中自动包含这个Cookie。

3. **自动附加Cookie到请求**：用户在与服务器交互时（例如，浏览网页、提交表单等），浏览器会根据需要自动将存储的Cookie附加到向该域名的请求头中，包括那些`HttpOnly`的Cookie。

### 重要点：

- **域名和路径**：浏览器只会将Cookie发送给设置它们的相同域名和路径。如果Cookie的`Domain`和`Path`属性限制了其作用范围，浏览器会遵守这些规则，只在相应的域和路径下发送Cookie。
- **安全和策略**：为增强安全性，建议使用`Secure`标志（仅通过HTTPS发送Cookie）和`SameSite`标志（控制Cookie在跨站请求中的发送行为）与`HttpOnly`标志一同使用。

通过这种方式，浏览器确保了`HttpOnly` Cookie的安全性，使其在客户端脚本中不可访问，同时保证了这些Cookie能够在用户与服务器交互时被自动使用，从而支持如会话管理和用户身份验证等功能。

注：

httponly能防止客户端js脚本访问和修改凭据，可以在一定程度上防御xss，例如黑客无法做到通过js将用户的cookie发送给自己，但是如果诱导用户自己执行js脚本发送请求，请求还是会发送成功。因此增加了httponly后，黑客实施xss攻击确实更加繁琐了。
