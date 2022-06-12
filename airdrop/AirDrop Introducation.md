# 仿Airdrop实现信息互传

## 介绍
苹果生态的AirDrop在传输文件时非常方便，本实验将仿照AirDrop的原理，基于PC终端与Android终端之间的Socket通信模型，实现当两个终端处于统一局域网时：
1. 由Android端选择发送图片，PC端接收；
2. 由Android端复制文字，PC端获取剪切板文字内容；
3. 由PC端发送文字消息，Android端接收。

## 界面设计
Android端的界面如下：   
![1.jpg](https://s2.loli.net/2022/06/12/7zjP4XvAflI5c3O.jpg)  


## 使用方法
1. 在手机上安装Android客户端apk文件；   
2. 在PC端上运行服务端程序；   
3. 发送图片：在客户端界面，选择手机相册的图片，点击发送，即可在PC端对应的文件夹下查看到传递的图片；     
4. 发送剪切板内容：手机在其他APP中复制文字，回到界面中，直接点击发送剪切板内容，即可在PC端接受到内容。

## 功能实现思路
### 手机向电脑发送图片
在手机中，用户先点击打开系统文件管理器，选择图片；   
选择后根据返回的URI得到文件路径；    
在用户点击发送后，程序建立与PC端socket的连接，并根据所得文件路径向PC端发送文件；   
PC端获得文件后，写入PC端中相应的文件夹路径下。
### 手机向电脑发送剪切板内容
用户在其他APP中复制文字；  
返回到本界面中，点击发送，程序即可获取用户剪切板内容，将内容发送到电脑端，并将显示在软件界面中；   
电脑端接受到内容后，在控制台输出。
### 电脑向手机发送消息
用户在PC端控制台输入文字，按下回车即可发送；  
客户端接受的消息，将其显示在手机界面上。

## 亮点
1. 启用不同的端口传送图片与文字，使得接收方可以监听多个端口的连接请求，以此区分需接收的信息的类型。
2. 对于发送剪切板内容，不需要用户手动复制粘贴，只需点击发送按钮，由程序自动获取剪切板内容；
3. 实现用户自主选择相册中的图片进行传送。
4. 实现电脑向手机发送消息。

## 不足
1. 目前由PC端的编译器控制台充当服务器，暂时没有编写桌面软件或网页，从而从PC端发送文件时不能自主选择文件；
2. 目前只实现了选择手机系统文件中的图片进行传递，没有实现文件传输;
3. 在Android端从系统文件管理器中选择图片时，只能选择最近相册中的图片，选择其他图片会出现闪退的问题，原因是利用URI获取文件路径时存在问题，通过查找资料，目前尚未解决；
4. 由于对actions的使用不太熟练，所以在持续集成中只进行了简单、基础的持续集成；
5. 在单元测试部分，由于是在android和jsp中进行通信，就没有使用正式的单元测试，而是对每个功能板块进行单独测试，以及最后对整个软件的测试。