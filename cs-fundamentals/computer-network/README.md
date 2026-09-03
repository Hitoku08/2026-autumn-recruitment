# 计算机网络

## 学习清单

- [ ] OSI 与 TCP/IP 分层模型
- [ ] 从输入 URL 到页面展示发生了什么
- [ ] HTTP 方法、状态码、缓存与版本演进
- [ ] HTTPS、TLS 握手与证书验证
- [ ] TCP 三次握手、四次挥手、可靠传输与拥塞控制
- [ ] UDP 的特点与适用场景
- [ ] DNS 解析流程
- [ ] Cookie、Session 与 Token
- [ ] 常见网络攻击与防护
- [ ] 网络故障排查思路

## 笔记

### TCP 服务端一直停留在 CLOSE_WAIT 大概率因为什么

**一句话回答：** 服务端应用层没有调用 `close()` / `shutdown()` 关闭连接。

**原理：**

- CLOSE_WAIT 是被动关闭方（服务端）收到对端 FIN、回 ACK 后进入的状态，含义是「对端已关闭，等我发 FIN」。
- 正常路径：收到 FIN → 回 ACK → 进入 CLOSE_WAIT → 应用调用 close() 发 FIN → 进入 LAST_ACK → 对端回 ACK → CLOSED。
- 若应用收到 EOF（read/recv 返回 0）后不关闭 socket，就永远停在 CLOSE_WAIT。

**常见追问：**

- 大量 CLOSE_WAIT 的后果？→ 连接泄漏，最终耗尽文件描述符（fd），无法 accept 新连接。
- 是内核还是应用问题？→ 应用层问题；`netstat` / 抓包看到大量 CLOSE_WAIT，基本可定位到应用漏 close。

**易错点：**

- 别跟 TIME_WAIT 混淆：TIME_WAIT 是主动关闭方等 2MSL，属正常机制；CLOSE_WAIT 是被动方没关连接，是 bug。
- read() 返回 0 是 EOF 信号，必须在该分支关闭 socket。

## 笔记模板

### 问题

**一句话回答：**

**原理：**

**常见追问：**

**易错点：**
