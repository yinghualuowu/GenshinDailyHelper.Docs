<h1 align="center">

GenshinDailyHelper.Docs

</h1>

**该项目建议采用云函数或docker方式部署**

**原神的签到福利是需要单独下载APP进行才可以领取，并且每天需要打卡，虽然奖励并不是很可观，但有一些摩拉，食材和可观的经验书累计起来还是挺有吸引力的。可能本身不怎么刷论坛的玩家往往会忽略这些奖励。**

自动签到步骤为
* **获取账号信息（区域和UID）**
* **判断是否已经完成签到**
* **未进行签到到执行签到动作**
* **项目链接 https://github.com/yinghualuowu/GenshinDailyTools**

重要提示

**该项目支持多号签到/多角色签到，但各位旅行者爱惜羊角包，不要滥用**

**如果可以的话，左上角的```Star```顺便点一下吧 > 3 <**

## 使用方法 - 腾讯云函数(即将收费)
签到是通过接口模拟请求达成目的，因此需要cookie信息来作为第一步

### 1.1 第一步，获取自己的Cookie信息
- 通过浏览器登录米哈游论坛 https://bbs.mihoyo.com/ys/
- 按```F12```，打开```开发者工具 -> Network``` 点击进入
- 刷新网页，找到以下的位置,复制Cookie后放在记事本或其它可以保存的地方

![Cookie所在位置](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/cookie.png)

Cookie获取方式新方法
- https://github.com/Womsxd/AutoMihoyoBBS#%E8%8E%B7%E5%8F%96%E7%B1%B3%E6%B8%B8%E7%A4%BECookie

### 1.2 第二步，登录腾讯云，创建函数
- 进入腾讯云 https://cloud.tencent.com/
- 进入个人中心，找到云函数进入页面 https://console.cloud.tencent.com/developer
![云函数所在位置](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf.png)

- 点击新建进入创建函数页面
![新建函数](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-new.png)
- 依次选择 自定义创建,运行环境(CustomRuntime),本地上传zip包(上传zip包)，其他按照下图默认
![创建函数](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-config.png)
- 高级设置，描述(取自己喜欢的名字)
- 环境配置, 初始化超时时间(30s)，执行超时时间(30s)，这个没有固定，认为自己合适的时间就行。环境变量(scf_cookie),其他按照下图默认
![高级设置](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-ad-config.png)
- 其他配置，异步执行和状态追踪改为启用,其他按照下图默认
![其他配置](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-other-config.png)
- 触发器设置需要测试完才可以配置,现在可以直接创建函数，过渡到测试步骤

### 1.3 第三部，测试函数执行
- 创建好函数后，进入函数管理，开始执行测试，注意必须部署才可以执行测试
![执行测试](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-test-run.png)
- 如果执行正常，进入触发管理，开始设置定时任务
![测试结果](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-test-run-result.png)
- 设置定时时间等信息，触发周期为自定义触发周期（注意这里时间是北京时间，和Github的定时不一样），格式依旧是Cron格式
![触发器](https://cdn.jsdelivr.net/gh/yinghualuowu/SakuraWallpaper@e6384d0/cnblog/head/genshin/scf-event.png)
- 可以在日志中查看每天执行的结果，以后会接入Service酱消息通知
