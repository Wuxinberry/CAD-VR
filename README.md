# CAD-VR
不定期上传一些关于CAD、VR以及CAD-VR领域的资料整理

## 入门资料
**《虚拟现实：理论、技术、开发与应用》**：清华的VR课程教材，内容比较浅显，适合入门对VR领域作基础了解（也仅限于此）。附录的射击游戏项目有小白级操作指导和模型，但省略了建模过程。在微信读书app上可以免费阅读。
<br>
**浙江大学王锐老师的ppt**：可以作入门参考，文字少
<br>
**《浩瀚的CADCAM软件 2021》**：一篇介绍CAD/CAM发展历史、研究现状的推文，可用于增进对CAD/CAM概况的把控。<br>
**《A Review of the Capabilities of Current Low-Cost Virtual Reality Technology and Its Potential to Enhance the Design Process》**：一篇关于VR和VR在设计领域潜在功能的综述，介绍了VR与AR、MR的区别以及当前的硬件发展状况(包括CAVEs和HMD)，其中也包含了对当前多数VR硬件的参数对比图（没有最新的Oculus Quest 2，更新到了Oculus Rift），可以协助个人开发者做设备购买的决定。
<br>
<br>

## UG 开发
UG(NX DESIGN)是西门子旗下的一款CAD软件（还包括CAE等）。官方提供的API比较糟糕，需要对CAD软件足够熟悉。推荐的开发方式是Visual Studio向导开发。为了跟实验室统一，我使用的是Qt开发，之后有空会附上配置文档。<br>
**《Siemens NX 二次开发》**：这是一本2021年10月份的新书，介绍还算比较全面，对API以及代码框架也有一点介绍，适合新手阅读，介绍了多种VS二次开发UG的方式。由于网络上的示例代码很少，因此可以参考书中的代码进行学习。<br>
**宏录制功能**：NX DESIGN中有宏录制功能，又称为 journal，可以录制用户的操作并自动翻译成c++代码，可以给开发者提供一些实例，方便开发人员寻找响应的API。但是生成的都是C+代码，不包含基于C的API(UF, user function)。同时，由于是自动生成的代码，通常比较冗余。<br>

<br>
<br>


更新中......
