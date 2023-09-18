# github.md

# 2FA验证

[官方文档](https://docs.github.com/zh/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication)


1. 使用`Authenticator app`方式：手机下载app： Microsoft Authenticator（直接去应用商店搜索，不需要注册，用来实时扫码就行）

![](https://cdn.jsdelivr.net/gh/gf9276/image/github/20230918143848.png)

2. 打开软件，扫描QRCode，必须保存他给的`github-recovery-codes.txt`（当你手机丢了，软件删了，可以用这个恢复登录）。

3. 为了安全（防止你丢了手机又丢了恢复码），使用`Security keys`方式（绑定硬件）：按提示输入一个`key name`，然后会跳出来windows设备验证，windows用户直接用锁屏pin码验证即可。
