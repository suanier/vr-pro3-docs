![image.png](https://cdn.nlark.com/yuque/0/2021/png/21666232/1621502736894-4a483c2e-9bbd-47d4-b7aa-b2e1af7fead5.png#clientId=ud408436c-2512-4&from=paste&height=577&id=uf51bd9fc&originHeight=577&originWidth=762&originalType=binary&size=67492&status=done&style=none&taskId=uca9aa038-ace0-44f0-abe7-23ba28eec68&width=762)
首先 微信二维码页面 通过 postMessage 存储数据 到中转页面 ，如果扫码成功后 通过扫描 id 获取到扫描时间，然后把微信二维码页面里的数据（产品型号+扫描时间）存储到中转页面对应的域名下 localStorage 中，存储成功后返回给当前页 “完成本地存储” 的消息。
然后在列表页中，它要获取微信二维码页面存储的数据，先向 中转页面 发送我要拿数据的消息，并且监听 “中转页面”返回的数据，然后就拿到数据（所有产品型号及扫描时间），通过排序找到最新的扫描时间对应的产品型号。
[点击查看【processon】](https://www.processon.com/embed/60a623fc0791296d60a53941) 1.列表页开发
（1）新用户或无本地记录用户，展示所有报告入口，更改之前页面的文案及样式，并且按照一下产品顺序排序：VD ＞ VR ＞ VR Pro3 ＞ AIR ＞ Classic；
（2）老用户，仅展示【最近使用】的报告入口，其他报告入口隐藏，根据所查报告的扫描时间，然后按照本地记录的最新时间，将对应产品报告入口标记“最近使用”，只标记一台设备。
当点击其他设备的测量报告下拉框时，显示列表页，并且标记“最近使用”的入口排列第一，其他报告入口按照一下产品顺序排序：VD ＞ VR ＞ VR Pro3 ＞ AIR ＞ Classic；

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21666232/1621506156164-4a44a3c9-f45a-42da-ac7f-f111e695a1d0.png#clientId=ud408436c-2512-4&from=paste&height=589&id=u50d008f6&originHeight=812&originWidth=375&originalType=binary&size=108556&status=done&style=none&taskId=u14746223-8ffc-4da2-ba23-ffe5bb12a3d&width=272)  
 ![image.png](https://cdn.nlark.com/yuque/0/2021/png/21666232/1621506192658-3556b5e6-e07f-4f30-8aaf-70222766e9c6.png#clientId=ud408436c-2512-4&from=paste&height=591&id=u1168c00d&originHeight=748&originWidth=352&originalType=binary&size=53993&status=done&style=none&taskId=uf73c2207-9d8f-42cc-b8a9-b3467517c8a&width=278) ![image.png](https://cdn.nlark.com/yuque/0/2021/png/21666232/1621506234110-db4c345a-a13e-41d1-a2f8-03a72461a6f1.png#clientId=ud408436c-2512-4&from=paste&height=592&id=u3c895e67&originHeight=1058&originWidth=375&originalType=binary&size=118273&status=done&style=none&taskId=u9fa3bb00-0d9d-465a-8cac-929405ac931&width=210)
