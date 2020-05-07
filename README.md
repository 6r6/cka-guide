# CKA 认证从零到一
**🐳 CKA from zero to hero - 2020/05**

---

## 报考理由
- 持证可以往**容器运维岗位**方向转，以冲击更高的工钱。在招聘平台搜索`k8s`、`kubernetes`、`容器`即可看到相关岗位的工资是十分可观的，有相当大一部分岗位月薪是在**20K**以上的。
- 考试费[国际站](https://training.linuxfoundation.org/april-2020-promo/)近两个月全场7折，考试只需要**210刀**。如果在[中国区](https://training.linuxfoundation.cn/certificate/details/1)考，这几天也是**7折~¥1466**，且为中国人监考、支持支付宝付款和以及开发票。

## 报考材料

- 国际站报考要求**护照** 或者 **港澳通行证** 或者 **驾驶证翻译件(支付宝可以免费申请) + 驾照原件**
- 国际站报考要求**外币信用卡**，用于报名费支付

## 考试形式
### 考试形式介绍

🐧内容来自网络，主要来源有：

[CKA考试经验：报考和考纲](https://blog.csdn.net/fly910905/article/details/102966474)

[2020年CKA考试最新最全指南](https://zhuanlan.zhihu.com/p/106090560)

————————————————

- 3 个小时， 24 道题目。需要在封闭无人的房间进行，要求桌面没有其他东西，并且房间没有人进出
- 在线考试，国际区需要网络环境十分稳定，并且Socks5科学工具也需要稳定，不然断线了的话会很麻烦
- 考试全部都是实际操作题，会给出多个 kubernetes 集群，要求你完成指定的操作
- 考试题目的分数按照操作的难度变化， 1~8 分不等
- 开始需要出示有英文名字的官方证件，一般护照最好。如果没有护照可以使用驾照翻译件等其他证明的方式
- 考试中是可以查阅 kubernestes.io 的官网的，并且可以使用事先定义好的书签，来快速查找到自己之前查看过得内容
- 考试题目普遍不难，但是细节上批改地非常严格，需要做题时仔细阅读题干

### 考试界面介绍
具体界面如下：
![Imgur](https://i.imgur.com/iUOTBmr.png)

- 在考试过程中，可以选择具体的语言，可以是中文。但是中文翻译有时不是很准确，因此需要对照进行
- 在考试过程中，考官需要你进行共享屏幕和共享摄像头，并且摄像头需要完整地能够看到你的脸的
- 其他工具中有记事本可以记录过程中的问题，可以将有问题的题目先记录进行然后再进行修改

### 集群的切换
在 CKA 考试中，集群的切换时通过跳板机中的参数来决定的。在具体操作时，我们只需要根据题目提示操作即可：
- 考试题干会给出非常详尽的提示，不会让你因为文件保存错位置、跳板机登陆错误等低级错误扣分
- 考试题干里全部的变量例如 `kucc04` 都是可以一键复制的
- 排错题需要使用 `ssh node01`进入节点
- 排错题需要使用 `sudo -i` 来获取 root 权限

## 如何学习
- 学习docker的基础知识
- 学习kubernetes的基础知识
- 针对cka认证学习相关的真题

## 奇技淫巧
参数切忌死记硬背，命令后加`-h`就有了。而且一定要理解每个参数的含义。例如常见的 `-o`，我们要知道：

```
#-o参数，指定生成yaml或者json格式的描述文件

[(-o|--output=)json|yaml|wide|custom-columns=...|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=...]

-o yaml 
-o json
--output=yaml
--output=json

```

考试界面自带的记事本非常好用，修改起来也很方便。直接从官方文档拷贝需要的yaml片段，在记事本里加以修改即可。也可以选择选复制到文件中，然后用vi编辑器修改。整个考试虽然给了3小时时间，但是其实大概`1.5h`就能够基本搞定。

例如这一道daemonset真题。

> daemonset简单理解就是每个节点只允许一个副本；再简单点，工地每一片区域里只允许放1间豪华板房。

```
Set configuration context $ kubectl config use-context k8s

Ensure a single instance of Pod nginx is running on each node of the kubernetes cluster where nginx also represents the image name which has to be used. Do no override any taints currently in place.

Use Daemonsets to complete this task and use ds.kusc00201 as Daemonset name. Question weight 3%

```
```
先粘贴，后用VI编辑：
(1)只需要打开官方daemonset文档，找到yaml片段：https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
(2)复制后，在vi用/g/fluentd-elasticsearch/s//nginx/g批量替换
(3)删除labels段、修改name即可。。
```

又比如这道真题，需要我们生成一个deployment的yaml文件，要求生成后清理掉命令产生的对象，则我们用`--dry-run`这个参数即可"模拟运行"。

```
Set configuration context $ kubectl config use-context k8s Create a deployment spec file that will:
Launch 7 replicas of the redis image with the label: app_env_stage=dev

Deployment name: kual00201

Save a copy of this spec file to /opt/KUAL00201/deploy_spec.yaml (or .json)

When you are done, clean up (delete) any new k8s API objects that you produced during this task Question weight: 3%
```

```
# 一条搞定
kubectl run kual00201 --generator=deployment/apps.v1 --image=redis --labels=app_env_stage=dev --replicas=7 -o yaml --dry-run > /opt/KUAL00201/deploy_spec.yaml
```


## 在线练习

[kodekloud免登陆联系-传送门](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests/lectures/9816528)
