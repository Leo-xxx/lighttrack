## 推荐！京东开源姿态跟踪新框架LightTrack！

原创： CV君 [我爱计算机视觉](javascript:void(0);) *昨天*

点击我爱计算机视觉标星，更快获取CVML新技术

------



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZI8MSc2I5Aicr5JqHIe6uOP2huUoGaOzYibVwo63IHzic7uOlfTuic0RCoyA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

今天，京东数字科技发表论文并开源了轻量级、在线计算、置顶向下的人体姿态跟踪新框架：**LightTrack**，这是该领域的首个框架！

也是最近最值得参考的姿态跟踪方面的工作～



在论文《LightTrack: A Generic Framework for Online Top-Down Human Pose Tracking》中，作者详细介绍了该算法框架。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZIDibkpfD5ibZyE4rA8NNeiazk7ZG7bQcj2QH4baia0ubdj5lv5pffWiaK7lA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



该论文是由京东数字科技美国研发中心的研究人员发表的。



**姿态跟踪**



姿态跟踪是指在视频中对多人进行姿态关键点定位与跟踪，这是一项综合性技术，如下图所示，展示了**LightTrack** 框架的的主要部分。



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZIiboghkLjTXSYmaOGQ1sXC549R1quF4FHlqOF2ojYaibPxibWicUZtAVZYg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**LightTrack**采用的是置顶向下的姿态估计方法，即先检测到人体，然后再针对每个人进行人体关键点定位。



在跟踪的过程中，涉及到姿态估计、目标跟踪成功、目标丢失等状态的转换。

另外，因为是对多人跟踪，所以涉及到对不同帧中人体个体的关联。



**LightTrack**是一种在线计算的姿态跟踪，即只使用当前帧及之前的视频帧，这更加符合现实中实时应用的场景。而很多离线计算的姿态跟踪，则会使用未来帧的数据。



所以这是一项综合性的计算机视觉问题，涉及到目标检测、姿态估计、多目标跟踪、行人重识别，并需要协同配合。



**LightTrack算法方案**



人体检测部分使用Deformable FPN算法，来自论文：

J. Dai, H. Qi, Y. Xiong, Y. Li, G. Zhang, H. Hu, and Y. Wei. Deformable convolutional networks. CoRR, abs/1703.06211, 1(2):3, 2017.

T.-Y. Lin, P. Dollar, R. B. Girshick, K. He, B. Hariharan, and ´ S. J. Belongie. Feature pyramid networks for object detection. In CVPR, volume 1, page 3, 2017.

使用论文提供的预训练模型。

检测只在关键帧做。



单人姿态估计部分使用CPN101  与 MSRA152模型，分别来自论文：

Y. Chen, Z. Wang, Y. Peng, Z. Zhang, G. Yu, and J. Sun. Cascaded Pyramid Network for Multi-Person Pose Estimation. In CVPR, 2018.

B. Xiao, H. Wu, and Y. Wei. Simple baselines for human pose estimation and tracking. ECCV, 2018.

并做了轻微改进。



多目标跟踪和行人重识别部分，作者将其建模为姿态匹配问题，即在上一帧人体位置扩大范围（+20%）进行单人姿态估计，如果检测到人体姿态，则将这些人体姿态与上一帧人体姿态进行匹配，使用的方法是孪生图卷积神经网络（如下图所示）。



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZIwicASH8AKY1GpIgQUKf3vicnPjHSjyLPWJ7iazBUDLWHIp710SzF7tYvg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果没检测到人体，则认为是目标丢失。



作者发现这种简单扩大范围姿态估计，然后再进行姿态匹配的方法是行之有效的。如下图：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZILCDh36hG80w0N5s14Js2ogqEQ92WvWqV9WpKY4ATlZNZgJl8Ucp0xw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



上图人物在镜头焦距突变的时候，虽然场景和人体表面特征变化比较大，但人体姿态变化并不大。



另外如果在关键帧人体检测中出现了新目标，也使用上述姿态匹配的方式进行人物个体关联，相当于行人重识别。



**实验结果**



作者在Posetrack 2017 Test set 与 Posetrack 2018 Validation Set 进行了实验。



结果如下：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZIvXM4zv6lpNZIft5DxGAEUBUZC8ibvdEeibw94zj0Z8EpJicCzV6w10qOg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可见，即使与目前精度最高的离线姿态跟踪算法（HRNet [CVPR2019 | 微软、中科大开源基于深度高分辨表示学习的姿态估计算法](http://mp.weixin.qq.com/s?__biz=MzIwMTE1NjQxMQ==&mid=2247485656&idx=1&sn=ef690cfdf366e0dfc11b7515da2c8540&chksm=96f37a8ca184f39a088d1488bafab51146cdec63b87a33183dc8cc7f177736a03e2e00790dd2&scene=21#wechat_redirect)）相比，**LightTrack**也取得了可匹敌的精度。

与在线计算的姿态跟踪算法PoseFlow、JointFlow比，则取得了大幅的精度提升，而且帧率也更高（使用Telsa P40 GPU，达到47/48 fps）。



值得一提的是，该框架不仅是一套完整的姿态跟踪算法，而且还允许用户非常容易地对该流程中的各个步骤进行替换、改进与评估，是该领域进一步研究的绝好工具！



下图为姿态跟踪的部分示例：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTtGFhLvYwcHPxQiaibX2jLZZIQsDBD2RFWYLetdp4yib4ufjpSYINVpNgN77LzGpLpD3XmIiax7iaHjlUQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



作者提供了Demo 动图，不过超出了公众号的文件大小限制，流量充裕的朋友可以点击左下角“阅读原文”查看～



**重点来了！**



论文地址：

https://arxiv.org/abs/1905.02822v1

项目地址：

http://guanghan.info/projects/LightTrack/

代码地址：

https://github.com/Guanghan/lighttrack



京东虽然主业不是计算机视觉，但在视觉领域的投入还是蛮多的，希望越来越多的优秀工作开源！



**加群交流**



关注**姿态估计与跟踪**技术，欢迎加入52CV-姿态专业讨论群，扫码添加52CV君拉你入群，

**（请务必注明:姿态）**

**![img](https://mmbiz.qpic.cn/mmbiz_jpg/BJbRvwibeSTv7ZnPMnOZ0WwM7DNjLlTdqfsdrzbObbTrIEnMDHLY6WBOHAJAJbozbEeLZLMicbicic7VQfP7IXQITw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**

喜欢在QQ交流的童鞋，可以加52CV官方**QQ群**：702781905。

（不会时时在线，如果没能及时通过验证还请见谅）

------

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTvVOnJBvePcP1qFUSWpyvrjpYAWNIZTZzUA7Zq4VPlReicJWcIeozxic5VhHlwNQNAFXmKQBtKf5xAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

长按关注我爱计算机视觉

**麻烦给我一个“在看”****！**

文章已于2019-05-10修改

[阅读原文](https://mp.weixin.qq.com/s?__biz=MzIwMTE1NjQxMQ==&mid=2247486818&idx=1&sn=647a5dd33b04597d7ce3d53de6e7c854&chksm=96f37f36a184f6200d328fd5d45f879fc26fb9b500c23fde70fce44e430bfcb47c5889a54a5a&mpshare=1&scene=1&srcid=&key=90581f21d61583cc96259e6db5e5eb2a746e096f5bcf1866e0ecb9c0215fe2fccce166a5b8da0b5b19f0876f8bd564e725fae6a2e4b45b70ce0b0746b572ff8d320bf4ada239a20b8147351e63fc5f73&ascene=1&uin=MjMzNDA2ODYyNQ%3D%3D&devicetype=Windows+10&version=62060739&lang=zh_CN&pass_ticket=K%2BIGcTnQy%2BCRjHMZMZc4gLhvLUTWu%2BI%2BZJi%2FUvviiuQ63hwm9X0fqql5iypl8%2BX8##)







微信扫一扫
关注该公众号