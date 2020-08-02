
# 简介

[本项目](https://github.com/TonyJiangWJ/Ant-Forest) 由 [https://github.com/Nick-Hopps/Ant-Forest-autoscript](https://github.com/Nick-Hopps/Ant-Forest-autoscript) fork 而来，但是经过了各种改动，和原版功能差异较大 现在已经单独复制不再作为fork分支开发。

基于 Autojs 的蚂蚁森林自动收能量脚本，采用 4.1.1 Alpha2 版本开发。解锁模块参考自：[https://github.com/e1399579/autojs](https://github.com/e1399579/autojs)

- 脚本执行依赖于：[AutoJs 4.1.1 alpha2](http://47.110.40.234/autojs/autojs-4.1.1-alpha2.apk) 若无法访问请百度搜索一下
- 设备系统要求

  - 具有 ROOT 权限的安卓 5.0 及以上版本
  - 没有 ROOT 权限的安卓 7.0 及以上版本

## 其他脚本

- [蚂蚁庄园传送门](https://github.com/TonyJiangWJ/Ant-Manor)
- [支付宝积分签到传送门](https://github.com/TonyJiangWJ/Alipay-Credits)
- [京东签到传送门](https://github.com/TonyJiangWJ/JingDongBeans)
- 拆分出来了基础项目，用于快速开发AutoJS脚本[AutoScriptBase](https://github.com/TonyJiangWJ/AutoScriptBase)

## 使用

- 下载安装 [AutoJs 4.1.1 alpha2](http://47.110.40.234/autojs/autojs-4.1.1-alpha2.apk) 之后把整个脚本项目放进 **"/sdcard/脚本/"** 文件夹下面。打开软件后下拉刷新，然后运行项目或者 main 即可。
- 给与软件必要权限 `后台弹出界面`、`显示悬浮窗`、`自启动`、`电量无限制`，并将软件保持后台运行
- 定时启动脚本，点击 `main.js` 的菜单，选择 `更多` `定时任务` 即可配置定时启动
- 如果运行提示有任务正在队列中，请运行配置 `config.js` 然后进到 `进阶配置` 中勾选 `单脚本运行`，该功能是用于多个脚本同时运行时的任务队列 相当于一个调度程序，避免多个脚本抢占前台导致出错
- 默认配置下已开启基于图像识别的方式来执行，当前因为森林更新基于控件方式基本不再可用，后续会直接移除相关代码。更多配置信息见[配置小节](#配置)
- 运行有问题请查看[#常见问题小节](#常见问题)
- 不同手机的解锁方法不同可能不适配，需要自行编写解锁方法，具体见[#添加解锁设备](#添加解锁设备)小节

## 配置

运行 config.js 后可以看到如下配置：

- 常用配置都在基本配置中，可以设置悬浮窗颜色 位置等
- 运行配置后右上角菜单可以重置所有配置信息为默认值
- 运行配置后可以看到百度API调用总次数和剩余次数
- 配置导出导入功能，点击右上角菜单即可导出当前配置到local_config.cfg中，默认已加密加密密码为device.getAndriodId() 如果需要在免费版和付费版AutoJS之间同步 需要自行输入密码
- 运行时数据导出导入功能同上所述
- **重要** 基于图像分析模式必须按如下配置，否则脚本无法正常执行
- 前往百度[通用文字识别](https://ai.baidu.com/tech/ocr/general)申请API权限，并配置好用于识别倒计时时间，因为目前无法基于控件识别到倒计时时间了。
- ~~直接进入排行榜后运行 `test/MockDetect.js` 得到具体像素点个数，分析倒计时和小手的个数差异，设置为他们的一个区分点，比如大多数倒计时像素点个数大于1900，而小手像素点个数小于1900，则设置 `小手像素点个数` 为1900即可。~~ 新版本不再需要配置，如果自动识别失败，请手动开启 `基于像素点个数判断小手` 并配置像素点阈值
- 在运行config.js之后进入 `进阶配置` 直接点击 `实时查看可视化配置信息` 按钮来刷新配置并显示当前的框选区域。也可以在配置完之后可以运行`test/全局悬浮窗显示-配置信息.js` 查看配置生效状况，可在 `resources` 目录下截图保存 `region_check.jpg` ，然后可以悬浮窗显示半透明信息，方便在运行 `config.js` 时拖动进度条快速调整具体区域
- 勾选 `拖动输入区域` 可以显示可拖动进度条
- 请确保框选区域符合如下样例
- ~~排行榜区域框选如下所示~~ 排行榜识别区域现在可以运行时自动设置，该配置为自定义优化保留
![rank_check](./resources/rank_region.jpg)
- ~~底部区域框选住 `了` 的上半部分；或者选择其他区域，并配置相应的颜色值，比如 `邀请` 按钮~~ 当前可以在运行时自动设置，该配置为自定义优化保留
![bottom_check](./resources/bottom_region.jpg)
- 偶尔会出现好友首页的能量球无法通过控件识别（大概率是以后的常态了），请进行如下配置：
- 基于图像分析的能量球收取，请运行config.js修改 `基于图像收集能量球范围`，默认为`[150, 500, 800, 350]`, 开启 `是否直接基于图像分析收取和帮助好友` 后生效
  将分析范围控制在如图所示即可，具体查看可以运行 `test/TestDetectBall.js`
  ![能量球检测范围1](./resources/check_collect_0.jpg)
  ![能量球检测范围2](./resources/check_collect_ball.jpg)
- 基于图像收取能量球失效或者不会配置，请尝试取消勾选 `是否直接基于图像分析收取和帮助好友` 然后勾选 `区域点击来收取能量`, 同时需要自己扩展区域点击的方法，见下方的 [#添加自定义区域点击代码](#添加自定义区域点击代码)

- 其他配置信息请运行 `config.js`

## 功能

- 自动匹配不同系统下自动化的方式，安卓 7 及以上可以通过无障碍服务模拟操作，7以下版本需要通过 root 权限模拟操作；
- 自动识别屏幕锁定方式并根据配置的密码解锁，支持图形解锁，PIN 解锁，混合密码解锁；特殊设备需要自行扩展，具体见[#添加解锁设备](#添加解锁设备)小节
- 同时支持支付宝手势解锁
- 识别自己能量球的倒计时，和好友列表中的倒计时做对比，取最小值作为下次收取的等待时间；
- 识别好友能量罩，下一次收取时跳过开启能量罩的好友；
- 默认使用倒计时收取，可通过配置打开循环收取；
- 可选择永不停止模式，无倒计时或超过激活时间则在激活时间之后继续执行，否则按倒计时时间等待，实现全天不间断收集；
- 根据设置选择是否帮助好友收取能量；
- 根据白名单实现不收取特定好友能量；
- 可以设定收取达到一定阈值后自动浇水回馈 默认阈值当日收集超过40克即浇水一次，同时可配置不浇水回馈的黑名单
- 浇水回馈数量可配置，可选：`10` `18` `33` `66`
- 脚本运行时可以显示悬浮窗展示当前状态
- 开始收集的时候按 `音量减` 可以延迟五分钟再执行，适合需要使用手机的时候使用，按 `音量加` 则关闭脚本终止执行
- 收取完毕后悬浮框显示收取的能量数量
- 可以自动打开无障碍，需要配合adb赋权

  ```shell
  adb shell pm grant org.autojs.autojs android.permission.WRITE_SECURE_SETTINGS
  ```

- ROOT设备可以实现自动锁屏，非ROOT设备需要扩展锁屏方法，具体见[#添加自定义锁屏代码](#添加自定义锁屏代码)，默认实现的是下拉状态栏中指定位置放了个锁屏按键
- 脚本更新 可以执行`update/检测更新.js`
- 相对完善的基于图像分析的收取，倒计时无法直接获取，请申请百度识图API或者通过永不停止模式来定时轮询。另外注意永不停止模式不要全天运行，1-6点执行无意义且可能封号。
- 加入开关，可以自动识别基于图像分析还是控件分析，好友数较多的建议直接使用图像分析
- 个人首页、好友首页等等都是基于控件信息识别的，如使用英文版的请运行配置，修改 `控件文本配置` 中的控件文本信息，中文版识别失败的也需要自行配置
- 可以将配置数据以及运行时数据进行导入和导出，内容通过AES加密，默认密码是 `device.getAndroidId()`，因此仅本机可用。如果需要跨设备或者免费版和Pro版之间备份，自行获取 `device.getAndroidId()` 然后根据提示输入即可
- 通话状态监听，当通话中或者来电时自动延迟五分钟执行，需要授予AutoJS软件获取通话状态的权限[该功能暂不可靠，且Pro版无法使用]
- 可以配置在锁屏状态下判断设备姿势，防止在裤兜内误触（基于重力加速度传感器）
- 加入了基于百度文字识别的接口 来识别倒计时数据
  - [通用文字识别](https://ai.baidu.com/tech/ocr/general)
  - 经过测试通用文字识别没法识别，但是网络图片识别接口可用，但是可惜的是每天只有500次的免费调用机会
  - 因此设置中加入了对倒计时绿色像素点的判断，像素点越多则代表倒计时的数值越小，这个时候可以进一步通过百度的接口判断实际的时间
  - 具体运行config.js 勾选百度OCR识别然后填写你申请的APIKey和SecretKey即可，上面的阈值随你设置，反正记住每天只有500次就对了
  - 注意APIKey和SecretKey一定要自己进入AI平台申请，不填写是无效的
- `unit` 下提供了多个自定义模式的切换脚本，执行后会自动打断当前运行中的脚本然后按新的设置启动。
  - `自定义1永不停止.js` 30分钟轮询一次，有倒计时按倒计时时间执行，适合9-23点。可以对它设置每天9点的定时任务
  - `自定义2计时停止.js` 按倒计时时间执行，最长等待时间60分钟，适合早上执行和晚上23点执行，避免0点后继续无意义的永不停止。可以对它设置7点、23点以及0点的定时任务
  - `自定义3循环千次只收自己.js` 循环收集自己的，适合自己能量快要生成的时候执行，因为每天步行能量生成时间是固定的，因此在生成前一分钟设置定时任务即可，然后再设置2分钟后的定时任务`自定义2计时停止.js`
  - 其他自定义方式请自行创建，内容参考以上文件和config.js中的字段

## 常见问题

- config.js 执行异常，运行 `unit/功能测试-重置默认配置.js` 依旧有问题，尝试重启AutoJS，并检查AutoJS的版本是否为 `4.1.1 Alpah2` 或者 `较新版本的AutoJS Pro`，目前生命周期监听功能只能在免费版上实现，故暂不推荐通过Pro来使用
- 如果报错 `Function importClass must be called with a class;...` 直接强制关闭AutoJS软件，然后再打开即可。一般只在跨版本更新后才会出现这个问题
- 另外如果不断的运行异常，强制关闭AutoJS软件后重新执行脚本。同时建议定期强制关闭AutoJS软件，避免内存不断增长导致卡顿
- 图像分析模式 如果识别有遗漏，尝试将 `颜色相似度` 调低 当前默认值已改为20
- 软件的定时任务，点击main.js的 三个点菜单->更多->定时任务 然后设置相应的时间即可
- 排行榜卡住不动，修改模拟滑动参数 `滑动速度` 和 `模拟滑动距离底部的高度`，当前支付宝去除了排行榜中的控件，必须使用模拟滑动才能正常使用。滑动速度请不要过低，一般200以上即可，否则无法滑动
- 排行榜列表底部卡住，默认情况下会自动识别底部区域，但是需要一定时间来完成请不要手动关闭脚本，识别完成后如果需要修改请见 [配置部分](#配置)。或者关闭 `基于图像判断列表底部`，修改 `排行榜下拉次数` 次数为总好友数除以8左右，具体自行调试
- 排行榜识别区域会自动设置，如果异常请手动修改配置中的 `校验排行榜分析范围`
- 报错 `获取截图失败多次` 请修改 `获取截图等待时间` 默认为500毫秒，自行调试选择适合自己的，或者直接取消勾选 `是否异步等待截图`
- 其他问题可以提ISSUE，但是请将日志文件大小调整为1024，打开开发模式并提供出错位置的日志信息 `logs/log-verboses.log`
- 如果已经按说明配置后开启 `是否直接基于图像分析收取和帮助好友` 运行不正常，请先取消勾选，然后勾选 `区域点击来收取能量`, 同时自己扩展区域点击的方法，见下方的 [#添加自定义区域点击代码](#添加自定义区域点击代码)

## 添加解锁设备

- 具体开发需要获取到锁屏界面的控件信息，可以运行`/unit/获取当前页面布局信息.js` 后手动锁屏并滑动进入密码界面 操作需要在3秒钟内，在短震动前完成打开解锁界面，觉得太短了自行修改一下代码。等待一小段时间（长震动）后手动解锁获取布局信息，然后根据布局信息进行开发。
- 脚本根目录下新建extends文件夹，然后创建ExternalUnlockDevice.js文件，内容格式如下自定义
- 更多扩展可以参考`extends/ExternalUnlockDevice-demo.js`

```javascript
module.exports = function (obj) {
  this.__proto__ = obj

  this.unlock = function(password) {
    // 此处为自行编写的解锁代码

    // 在结尾返回此语句用于判断是否解锁成功
    return this.check_unlock()
  }

}
```

## 添加自定义锁屏代码

- 同解锁设备，在extends文件夹下创建LockScreen.js，内容可以参考LockScreen-demo.js 实现自定义锁屏
- 扩展代码之后可以执行 `test/TestLockScreen.js` 来调试是否生效

```javascript
  let { config: _config } = require('../config.js')(runtime, this)

  module.exports = function () {
    // MIUI 12 偏右上角下拉新控制中心
    swipe(800, 10, 800, 1000, 500)
    // 等待动画执行完毕
    sleep(500)
    // 点击锁屏按钮
    click(parseInt(_config.lock_x), parseInt(_config.lock_y))
  }
```

## 添加自定义区域点击代码

- 当开启 `是否尝试区域点击来收取能量` 时点击有问题请扩展该代码
- 而当开启 `是否直接基于图像分析收取和帮助好友` 后不再需要扩展该代码，只需要修改 `控件文本配置` 中的 `基于图像收集能量球范围` 即可，当前推荐开启该选项，具体见 [#使用说明](#使用)
- 同解锁设备，在extends文件夹下创建MultiTouchCollect.js，内容可以参考MultiTouchCollect-demo.js 实现自定义区域点击

```javascript

  module.exports = function () {
    // 循环点击1080P 分辨率下的区域(起始[200, 700]-结束[900, 700])，其他分辨率根据实际情况微调
    let y = 700
    for (let x = 200; x <= 900; x += 100) {
      let px = x
      let py = x < 550 ? y - (0.5 * x - 150) : y - (-0.5 * x + 400)
      automator.click(px, py)
      sleep(20)
    }
  }
```

- 修改扩展代码之后，可以运行 `test/全局悬浮窗显示-点击区域信息-音量上键关闭.js` 可以进到森林页面实时查看配置效果，通过此方法可以随你调试编写各种花样点击，也可以仅仅修改y值，修正上下偏移量 默认是y=700 部分刘海屏可能需要修改成600
- ![全局悬浮窗显示](./resources/check_multi_position.jpg)

## 分享你的配置

- 如果你想分享你的自定义扩展代码，可以提交到 [这个分支下](https://github.com/TonyJiangWJ/Ant-Forest/tree/share_configs)，比如解锁代码 可以命名为 `ExternalUnlockDevice-手机型号.js` 并提交到 `extends` 目录下，方便其他用户下载使用
- 想获取其他网友分享的代码可以前往[这个分支](https://github.com/TonyJiangWJ/Ant-Forest/tree/share_configs)下载，或者等我集成发布

## 更新记录

- 历史版本更新记录可前往[RELEASES 页面](https://github.com/TonyJiangWJ/Ant-Forest/releases) 和 [RELEASES(旧仓库)](https://github.com/TonyJiangWJ/Ant-Forest-autoscript/releases) 查看

## 目前存在的问题

- 可能存在收集完一个好友后，因为没有获取到该好友剩余能量球的倒计时导致漏收
- 部分系统，如我使用的MIUI12因为省电策略的问题（即便设置了白名单无限制自启动），导致AutoJS软件的定时任务无法准时运行。非脚本自身问题
- 新发现问题请提交ISSUE，我会尽快跟进解决

## 请开发者喝咖啡

- 欢迎使用支付宝或微信请我喝杯咖啡
  - 一元喝速溶、5元喝胶囊、12买全家、33星巴克感激不尽
  
  ![alipay_qrcode](./resources/alipay_qrcode.jpg)  ![wechat_qrcode](./resources/wechat_qrcode.png)
