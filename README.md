# some-little-shit-backup
绑定国铁吉讯账号是小布建议中的一个功能，绑定后可在购票后获得速览、流体云、小布提醒等出行提醒功能，非常便捷。

但众所周知，OPPO / OnePlus / Realme 的高通平台设备解锁 BootLoader 后会将 KeyStore 切至另一个槽位（回锁后恢复原状），但该槽位默认缺少部分 Key，导致 Attestation 功能无法使用，而国铁吉讯账号绑定依赖该功能进行签名，自然也是无法绑定的，会出现如下提示。
虽然有些 Magisk 模块可以临时修复此问题，但原理是通过 hook 实现，极大概率会遗留一些漏洞被检测，并且一旦忘记 root 或掉 hook，账号绑定就会丢失，不能做到永久修复。

那么有没有办法永久修复呢，答案是有，修复该功能需要 root 权限，修复后无论是卸载 root 还是回锁后再次解锁都无需再进行修复，并且没有任何副作用！*

* 欧加系解锁后将 KeyStore 切至第二槽位，此时只能对该槽位进行操作，该槽位的内容对原槽位无任何影响。回锁后恢复原槽位，微信指纹支付正常启用，Play Integrity 依然能够达到 STRONG 等级，故不存在副作用。

（注：本教程以 OnePlus 13 为例，系统语言为简体（中文），已解锁 BootLoader 且 root 方式为 Kitsune Mask 27001；本教程默认读者拥有 adb、解锁 BootLoader 和 root 的基础知识并了解其风险，由此带来的任何不良后果的将由读者自行承担 ）

第一步：解锁 BootLoader、安装 root、启用 adb 调试
请参考教程： https://www.coolapk.com/feed/52328324?shareKey=MDU5YmFjYjE1YWRhNjcyY2RkYjM~ 

第二步：下载所需文件并解压
下载链接投币[酷币]评论区自取

后续步骤将分为电脑篇和手机篇，二选一即可，推荐使用电脑完成

【电脑篇】
第三步：复制文件到临时目录
1. 将手机以 adb 调试方式连接至电脑。
2. 打开终端，输入指令「adb push <文件地址> /data/local/tmp」，将<文件地址>（含括号）替换为第二步解压后的文件地址，地址前后需要添加空格。Windows 系统下可以先输入前半部分指令，之后将文件拖入终端，路径会自动填充，之后再输入后半部分指令。
3. 回车执行，看到「1 file pushed」即代表执行成功。

第四步：安装到 KeyStore
1. 输入指令「adb shell su -c exit」，激活 root 权限。
2. 在手机中点击「允许」授权。
3. 输入指令「adb shell su -c LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/local/tmp/attestation attestation true」。
4. 回车执行，看到「InstallKeybox is done!」即代表执行成功。
5. 输入指令「adb shell rm /data/local/tmp/attestation」，并回车执行。

电脑篇读者请直接跳到第五步

【手机篇】
第三步：复制文件到临时目录
1. 安装并打开能使用 root 权限的文件管理器，推荐「MT 管理器」，启动时授予 root 权限。
2. 将左侧目录跳转到第二步解压后的文件地址，将右侧目录跳转到「/data/local/tmp」。
3. 长按左侧文件，点击「复制」和「确定」。

第四步：安装到 KeyStore
1. 安装并打开终端模拟器应用，同样推荐「MT 管理器」，点击主页左上角菜单，点击「终端模拟器」。
2. 输入「su」回车，获取 root 权限，看到左侧变为「井号」即成功。
3. 输入指令「LD_LIBRARY_PATH=/vendor/lib64/hw KmInstallKeybox /data/local/tmp/attestation attestation true」。
4. 回车执行，看到「InstallKeybox is done!」即代表执行成功。
5. 输入指令「rm /data/local/tmp/attestation」，并回车执行。

----------

第五步：验证是否修复成功
1. 在桌面找到「拨号」并打开。
2. 输入「*# 899 #」。
3. 点击「手动测试」。
4. 将顶栏滑到最右侧，点击「其他」。
5. 点击「Key 状态」。
6. 检查是否除了「SOTER key」全为绿色，如果一切正常，这时小布建议已经可以正常绑定国铁吉讯账号了。

该教程也可以用来修复部分欧加机型 Netflix 启动时出现「网络错误 (15001) (-93)」的报错，且可正常使用 Widevine L1 FHD 画质。目前在 OnePlus 13 中测试成功，但在 OnePlus 12 中测试失败，故不保证能用于修复 Netflix，请读者自行测试。

#一加12# #一加11# #一加Ace3#
