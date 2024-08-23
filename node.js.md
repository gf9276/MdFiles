

# npm

```
npm config set registry https://registry.npmmirror.com
```

<!-- 管理员模式打开powershell

```
# 以前的
npm install -g cnpm --registry=https://registry.npm.taobao.org

# 2022年后的
npm install -g cnpm --registry=https://registry.npmmirror.com
``` -->

打开前端项目

<!-- ```
cnpm install
``` -->

```
npm install
```

如果安装失败，可以

```
 npm cache clean -f
```

然后再 install 一次。
不过我有个疑问，为啥会报python的错呢？

```
# 启动服务
npm run dev
```