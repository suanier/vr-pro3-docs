## 售后流程

审批编号 202203221819000331368

![IMG_2924.JPG](https://cdn.nlark.com/yuque/0/2022/jpeg/287793/1648285940242-07d059c0-a87a-4d44-bdda-4fb8df182ca3.jpeg#clientId=uadefb4a9-4b88-4&from=drop&id=u2df98676&originHeight=4157&originWidth=828&originalType=binary&ratio=1&rotation=0&showTitle=false&size=574626&status=done&style=none&taskId=ub6529fb1-2d85-4c91-864d-5b7035c572a&title=)

## 问题表现

开机一段时间后机器黑屏，按键无法操作，需要重新断电开机才能恢复，明确知道的发生时间 3-22、3-24 17:23

## 排查记录

### 1、排查客户端日志

查看 22 和 24 号日志发现客户端日志异常，在一段时间内没有日志，初步分析这段时间应该是黑屏发生的时间段
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648286498121-bd31953d-e4a4-4ca8-b1c0-3ac9d21414ad.png#clientId=uadefb4a9-4b88-4&from=paste&height=419&id=u7c9d39b1&originHeight=838&originWidth=2320&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1408489&status=done&style=none&taskId=u7d1b30e5-da63-4868-a2b5-4a375561559&title=&width=1160)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648286565315-441221c0-d2bc-4604-a7bc-01d01b1c3825.png#clientId=uadefb4a9-4b88-4&from=paste&height=353&id=u87049ff6&originHeight=706&originWidth=2056&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1168053&status=done&style=none&taskId=u4e931da9-705d-40d0-9f45-0cc16929047&title=&width=1028)

### 2、排查后台服务日志

查找对应时间段内后台服务日志，后台服务日志正常，排除断电关机情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648286827062-09ae2063-d198-448a-a2d5-17fe50923907.png#clientId=uadefb4a9-4b88-4&from=paste&height=755&id=ucfe5bded&originHeight=1510&originWidth=2630&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6669100&status=done&style=none&taskId=ubfef467e-cc17-4506-949b-d8360b14d22&title=&width=1315)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648286922968-b30aac09-7e27-487e-8f00-eb994e0baad7.png#clientId=uadefb4a9-4b88-4&from=paste&height=798&id=u123a1bbb&originHeight=1596&originWidth=2610&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6916496&status=done&style=none&taskId=u3c855f01-94a0-40fd-afce-37ee26fa991&title=&width=1305)

### 3、排查 Linux 系统日志

在前端日志丢失的开始时间点都找到了同样的内核错误日志，指向的都是客户端可行性程序
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648287039773-3d7eb9ce-1945-4307-8ee3-85d19c68fbed.png#clientId=uadefb4a9-4b88-4&from=paste&height=313&id=ub5dbaac8&originHeight=626&originWidth=2848&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1156510&status=done&style=none&taskId=uaf3bee7c-dd3c-42e0-8665-a103a70cda5&title=&width=1424)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648287078939-c869c435-f4cd-4eca-97a6-a69c767a9360.png#clientId=uadefb4a9-4b88-4&from=paste&height=272&id=udbcb3d85&originHeight=544&originWidth=2860&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1027456&status=done&style=none&taskId=u16b1a3aa-6d30-4fda-b03c-e2ce46596da&title=&width=1430)

### 4、排查 supervisor 日志

排查客户端程序是否挂掉，挂掉的话 supervisor 会重新拉起并有对应的日志记录，排查发现客户端并没有挂掉
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648288065484-9373f2d2-e38b-4eeb-bd0e-ebc74807cd3d.png#clientId=uadefb4a9-4b88-4&from=paste&height=152&id=u2d5b3d4f&originHeight=304&originWidth=2068&originalType=binary&ratio=1&rotation=0&showTitle=false&size=498365&status=done&style=none&taskId=u66f5950f-bcbd-402a-b136-68305039ec9&title=&width=1034)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648288178750-785cbefc-8d37-46f7-add8-0109dee6e727.png#clientId=uadefb4a9-4b88-4&from=paste&height=261&id=ud7e4fd8c&originHeight=522&originWidth=2300&originalType=binary&ratio=1&rotation=0&showTitle=false&size=985768&status=done&style=none&taskId=u8332e3a8-874a-4510-8772-98e5a229249&title=&width=1150)

## 初步结论

初步分析是客户端渲染线程崩掉，主线程还在，程序并没有直接挂掉，因为日志也在渲染线程中，渲染线程挂掉日志也就没了

客户端窗口的背景色设置的就是黑色，也符合描述的现象
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648288923271-64549f6c-f6f9-49e5-8c1a-904d53816434.png#clientId=ua1c23bbb-1262-4&from=paste&height=327&id=uaa5551dd&originHeight=654&originWidth=1268&originalType=binary&ratio=1&rotation=0&showTitle=false&size=298220&status=done&style=none&taskId=ubb17f07a-988b-44b9-a7ee-21661b2b9d8&title=&width=634)

## 原因分析

### 缩小范围

分析两次客户端异常时间点前后日志，发现两次都是客户端状态通知服务断开（网络问题可能性大，运维的 zabbix 资源监控这段时间也没上报数据），在重连尝试很多次后出现的问题，基本已经可以缩小范围是网络断网重连这块引发了一些未知的 bug，导致客户端黑屏了
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648289344750-52ecab05-b8c0-4e3e-afd6-bbb561bc1b7a.png#clientId=uc1cc6675-a5a3-4&from=paste&height=434&id=uedc7397f&originHeight=868&originWidth=1696&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1484170&status=done&style=none&taskId=u0fc44809-3234-427e-a46c-0d838ee2b4e&title=&width=848)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/287793/1648289446969-349b8311-ea3e-43b2-b17f-76d547d99f02.png#clientId=uc1cc6675-a5a3-4&from=paste&height=464&id=u2eab022e&originHeight=928&originWidth=1768&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1545057&status=done&style=none&taskId=u1bd9a58b-4b15-482c-8534-2e7f491cc45&title=&width=884)

### 下一步计划

1、下周在 vr-pro3 机器上模拟弱网环境重连恢复，看是否能够复现
2、继续排查分析断网重连这块的代码逻辑，找到可能存在的问题
