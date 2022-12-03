### 1.版本管理的问题

#### 1.1 semver版本规范是X.Y.Z

- X主版本号：当你做了不兼容的API修改（可能不兼容之前的版本）
- Y次版本号：当你做了向下的功能新增（新功能增加，但兼容之前的版本）
- Z修订版号：当你做了向下的问题修正（没有新功能，修复了之前版本的bug）

#### 1.2 ^ 和~的区别

- ^X.Y.Z：表示x保持不变的，y和z永远安装最新版本
- ~X.Y.Z：表示x和y保持不变，z永远安装最新版本

### 2. npm install 原理

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/2cf86f79ef60146228b28ddc3655d3fc&showdoc=.jpg)



### 3. npx

> npx 是npm5.2之后自带的一个命令

- npx 作用非常多，但是比较常用的就是使用他来**调用项目中某个模块的指令**

```powershell
// 例如查看项目中webpack的版本号
// 直接使用 查看的是全局安装的webpack版本，而非是项目中安装的版本
webpack --version 

// 使用npx,可以在项目的node_modules中查找webpack模块，调用webpack指令
npx webpack --version
```

