
# 1. WriteUp

## 1. find_me

题目：树师傅在自己最喜欢的照片里藏了flag，你能找出来吗？ 顺便一提，树师傅最喜欢的16进制编辑器是010 Editor。

<img width="306" height="432" alt="ssf" src="https://github.com/user-attachments/assets/124dbee7-dc6f-47ca-a940-870b6562cfc7" />

根据题目提示，直接使用010 Editor打开该图片。

查看文件头与文件尾。

<img width="467" height="302" alt="image" src="https://github.com/user-attachments/assets/f201b441-0bfb-47f4-a981-65ad724efbc5" />

发现文件尾存在flag。

提交，正解。

## 2. 金三胖

解压得到一张动图，仔细看发现其中闪过几张不同的图片。

<img width="173" height="146" alt="image" src="https://github.com/user-attachments/assets/5ff4f849-7596-4954-b35e-b6f0f71683ff" />

使用线上分解动图工具，得到照片（工具地址：https://uutool.cn/gif2img/     ）。

<img width="219" height="186" alt="第21帧" src="https://github.com/user-attachments/assets/5ec7516f-eb41-4cbc-a169-b633db952a4b" />

<img width="219" height="186" alt="第51帧" src="https://github.com/user-attachments/assets/bb695a14-8f63-4623-9963-be9abaf687e9" />

<img width="219" height="186" alt="第79帧" src="https://github.com/user-attachments/assets/04d91d77-1b3f-4bbd-b69b-c7f00311750c" />

提交显示正确。

## 3. 大白

大白少了下半身。

使用010 Editor打开，修改第二行第八个字节（管理图片宽高）为e8，得到完整的图片.

<img width="469" height="332" alt="image" src="https://github.com/user-attachments/assets/bc65a356-5d49-4f93-8d4b-adba5e4604f5" />

## 4. 寻找黑客的家

开始有点摸不着头脑，后来发现自己把红荔村理解成一个组织了，然后就解出来了。以后可以用这种办法开盒大批量发朋友圈的同学。

<img width="1080" height="1440" alt="附件1" src="https://github.com/user-attachments/assets/d923c6d9-81d4-47ee-bda9-9db444c1df11" />

<img width="1440" height="1471" alt="附件2" src="https://github.com/user-attachments/assets/b68bd9a9-8acf-4af6-8cb5-2a96e387bc3c" />

首先给了两张照片，第一张有用信息不多，就只有一个汉明宫的招牌。

第二张指明了一个地点（感觉说话不像正常语序）红荔村。

使用无敌的百度搜索一下，得知在广东省深圳市

然后要求知晓是哪条路，直接搜索红荔村汉明宫足疗（价格还挺高），存在两个地点，一个是福田区燕南路，一个是龙华区清泉路

都尝试了一下，得到结果是深圳市龙华区清泉路

# 额外的题

## 1.星空

解压压缩包，得到一张图片

<img width="983" height="558" alt="out" src="https://github.com/user-attachments/assets/6ff31573-4cc1-4b0b-a870-91a8f5b3eb83" />

使用010 editor打开图片，搜索png文件尾16进制代码，发现后面存在类似rar压缩包的文件，提取出来

<img width="1473" height="949" alt="屏幕截图 2026-04-18 181118" src="https://github.com/user-attachments/assets/50ab065f-c4d9-4259-8f5d-862ffd5b226c" />

解压后，得到400多张图片，要使用专门的工具进行拼合，但是我下载失败了

<img width="978" height="668" alt="屏幕截图 2026-04-22 211651" src="https://github.com/user-attachments/assets/34aec47a-ba3b-447b-9eae-8c15dde7728a" />


## 2.rabbit

解压压缩包，得到一张兔子图片，可爱捏

<img width="1440" height="3200" alt="Rabbit" src="https://github.com/user-attachments/assets/4f6b4f61-015b-4759-925b-888c729a0615" />

一开始以为是对文件的宽度进行了修改，但是使用010 editor打开后发现文件尾存在类似base 64的字符

<img width="1464" height="933" alt="屏幕截图 2026-04-22 222833" src="https://github.com/user-attachments/assets/2d5f740c-c470-4ec6-a86a-38f0e000d422" />

使用base 家族的编码进行解码，失败了

后来在网上搜索，发现存在rabbit编码，在线解码，得到flag。

## 3.zip套娃

基础的破解压缩包密码的题目，还好没明文解密，这个是一点不会

首先没一点提示，直接爆破，得到4位密码

<img width="665" height="293" alt="屏幕截图 2026-04-22 223358" src="https://github.com/user-attachments/assets/ba79c2d8-511e-4672-a3e9-289bcd788681" />

然后得到提示，设置掩码为1234567？？？，选择所有数字与字母，得到密码

<img width="662" height="286" alt="屏幕截图 2026-04-22 223556" src="https://github.com/user-attachments/assets/738609d2-0e21-4bbe-a7a4-82e1afe16cf7" />

最后得到的提示与第二层相同（不知道是不是bug），但是由于解压软件有点强，直接破解出来了（他甚至还提醒我压缩包有点问题），得到最终的flag

<img width="1615" height="820" alt="屏幕截图 2026-04-22 224054" src="https://github.com/user-attachments/assets/b350e9c9-38b8-42f0-8051-a6c96cd1d155" />

# 2. 工具部署

## 1. 010Editor

<img width="737" height="475" alt="image" src="https://github.com/user-attachments/assets/ab413e1a-3ad2-4849-9e3f-54227261ac4a" />

## 2. StegSolve

<img width="419" height="470" alt="image" src="https://github.com/user-attachments/assets/2aaadb2b-358b-4f31-8a83-6527eed696a3" />

## 3. Python

<img width="1038" height="743" alt="image" src="https://github.com/user-attachments/assets/b1670099-fc64-4f7a-91ba-93b4a201d6c6" />
