# 简介
```
本次实验旨在通过分组实践，设计一个智能汽车相关的网站，在HTML、CSS和JavaScript设计页面的基础上使用Web框架改写该网站。
选择并安装React、Vue、Angular、Bootstrap等前端框架中的任意一种 
- 改写后的网站具有完整的搜索栏，并且可以实现搜索
- 改写后的网站需要具有响应式布局并可以正常运行
- 阐述如何选择合适的Web前端框架，并利用该框架改写原有的智能汽车相关的网站
```

## 编译环境
```
Pycharm Pro Edition 2025.1.1.1 + VUE 3
```

### 使用前准备
1.检查是否安装过Node.js
Node.js官网https://nodejs.org/zh-cn
```
node -v
npm -v
```
如果出现v22.xx.x和10.x.x则说明Node安装成功；如果出现'xxxx'不是内部或外部命令，也不是可运行的程序或批处理文件,则重新进行安装。

2.全局安装Vue CLI
```
npm install -g @vue/cli
```
验证安装
```
vue --version
```
若安装成功则会输出@vue/cli 5.0.8

3.安装依赖项在项目文件中
在资源管理器中文件地址顶部的地址目录中输入cmd打开控制台（或Win+R打开控制台然后cd项目地址）
```
npm install
```
这样项目文件中就多了一个node_modules的文件夹

4.启动项目
```
npm run serve
```
等待一段时间后会显示【DONE】Complied successfully in xxx ms
以及App running at:
Local:http://localhost:8080/
Network:http://youripaddress:8080/
这时候项目已经启动成功
```
http://localhost:8080/
```
将http://localhost:8080/
复制到浏览器打开即可


### 更多
[原神启动](https://autopatchcn.yuanshen.com/client_app/download/launcher/20250508182406_qVf7ZaTdA9uIdSJ7/mihoyo/yuanshen_setup_202504252217.exe)
