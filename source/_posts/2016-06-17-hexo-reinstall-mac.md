title: Hexo在Mac上重新安装
date: 2016-06-17 17:48:26
tags: hexo
---
博客和空间好久没动了，最近觉得对于知识和想法在进行输出之后才能更好得为自己所用，于是打算重新拾起来这个地方。
电脑换了，所以要在Mac上重新安装hexo,在nodejs,git都已经装全的情况下，用npm安装了hexo,但是出现了下面的错误:
```bash
    { [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
    { [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
    { [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```
#### 失败方案
把错误甩到Google，发现[网上的解决方案](1)，一般为换个npm源，然后再重新安装，但在我这里无法成功，具体原因不明。
```bash
    npm --registry https://registry.npm.taobao.org
    npm install hexo-cli —no-optional
```
#### 成功方案
仍然是重新安装，但重点是先卸载hexo:

1. 根据[hexo官方网站](2)mac用户的说明，先安装Xcode,并安装Command Line tools 
![for mac user](http://o8wr0ngra.bkt.clouddn.com/hexo_for_mac.png)

2. 卸载之前安装的hexo  (**重要**) ,不卸载在我的机器上无法成功
	
	```bash
	npm uninstall hexo-cli -g #3.0.0版本执行
	npm uninstall hexo -g #之前版本执行
	```

3. 配置国内npm数据源并重新安装(之前使用cnpm似乎也不能成功)

	```bash
	npm --registry https://registry.npm.taobao.org
	npm install hexo-cli -g
	```

4. 在blog根目录下启动安装hexo模块并启动hexo server

	```bash
	npm install
	hexo server
   	```

---
##### 参考文献:
\[1\]: [https://segmentfault.com/a/1190000002979092][1]  
\[2\]: [https://hexo.io/docs][2]

[1]: https://segmentfault.com/a/1190000002979092 "hexo配置解决"
[2]: https://hexo.io/docs "官方文档"
