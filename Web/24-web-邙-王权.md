## 一、工具安装记录
### 1. 浏览器开发者工具（F12）
+ 安装方式：Chrome自带，按F12打开
- [ ] 使用截图：<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776342311275-69b71b77-c050-426c-8587-03b71370a061.png)
- [ ] <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776342329319-35e50d6a-a3a9-4251-85e9-01fe30cb73ee.png)
- [ ] <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776342343037-50f747b1-9206-408d-ac19-39f104bd7bd9.png)

### 2. Hackbar插件
+ 安装方式：

1. 下载 HackBar 扩展包并解压到本地文件夹。

2. 打开 Edge 浏览器，进入扩展管理页面（ edge://extensions/ ），开启「开发者模式」。

3. 点击「加载已解压的扩展程序」，选择解压好的 HackBar 文件夹，完成安装。

4. 安装完成后，在任意页面按  F12  打开开发者工具，即可在标签栏中找到 HackBar。

+ 使用截图：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776398433446-8d43f6af-81f9-47d6-a7f0-b999eb30a4ce.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776398433709-16d06958-c0ea-40b0-bf9b-e52db48fbacd.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776398433679-f9342914-a65f-42e3-b4b9-598966d525b6.png)

### 3.Burp Suite
+ 安装步骤：前往PortSwigger官网（[https://portswigger.net/burp/communitydownload](https://portswigger.net/burp/communitydownload)）下载Burp Suite Community Edition安装包，运行安装包按提示完成安装并首次启动，选择临时项目模式进入主界面，随后导出CA证书，在浏览器中配置127.0.0.1:8080的代理并导入该证书，开启抓包拦截即可正常使用。
+ 使用截图：<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776361974887-de25c048-1482-430f-a294-8c0402184225.png)

     <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776361990578-9c1cf3c4-dd8a-4a5c-b24b-0441eb2db211.png)

     <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776362032228-46edbb1d-f5e5-4e51-9d57-8def012c4044.png)

###  4. Yakit
+ 安装步骤：进入[https://yaklang.com/](about:blank)下载Windows系统版本；首次启动进入默认项目，打开MITM劫持功能，配置浏览器代理为127.0.0.1:8083，安装Yakit根证书并重启浏览器，即可正常抓包。<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776356954198-933a6439-2289-4e9a-b590-480a82ecc31a.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776357009261-bd3df228-7d3a-4d34-84b7-7b466ada29a9.png)

+ 使用截图：<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776356537874-caf78811-3c86-4773-a704-d96d4823f337.png)

## <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776357072336-b396da19-b476-4fd8-ba79-45c1b20edee7.png)5. AI
## <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776388127653-69f6e2ba-1f65-4586-af0b-ce355c926457.png)
## 6. dirsearch工具
+ 安装方式：通过pip安装dirsearch：确认Python 3.10.11环境后，执行 pip install dirsearch 完成安装，再用 dirsearch --version 验证成功，最后运行扫描命令测试。<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776363260698-c5fefff0-58c6-4c9e-bb6c-aed0b231a88b.png)

     <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776363306792-5493c6e8-c7e5-4df4-8de0-a3b21f7ebaf4.png)

+ 使用截图：<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776363361360-6bed6dea-f4d2-4321-9f0f-6d7462946603.png)

---

## 二、题目Writeup
### 题目1：[SWPUCTF 2021新生赛]gift_F12
+ 题目来源：SWPUCTF 2021新生赛
+ 解题工具：浏览器F12
+ 解题过程：
    1. 打开题目链接，按F12查看网页源代码，使用浏览器‘view-source:’协议可直接访问页面源码，绕过JS限制
    2. 在源码中‘Ctrl+F’搜索‘flag’，找到隐藏的flag：`flag{xxxxxx}`，提交‘NSSCTF{}’格式
+ 关键截图：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776391147456-2fc0a9cf-6c45-404d-9e8b-4a9792b8435d.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776391173589-710f86f5-0467-449a-ab72-76861523b7d6.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776391311669-1efb9265-4001-42ed-bc18-2579e6ae6c63.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776391323616-9d7ca074-b67a-4016-8c86-006f56a31c3a.png)

+ 总结：学会了用F12查看网页隐藏信息

### 题目2：[qsctf-编号0902]robots.txt
+ 题目来源：qsctf
+ 解题工具：浏览器
+ 解题过程：
    1. 访问题目链接 + `/robots.txt`
    2. 找到隐藏的文件路径，访问后拿到flag：`flag{xxxxxx}`
+ 关键截图：<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776392852714-1ed0eaa2-68ac-4416-a62a-169cc09ec97b.png)



  <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776392921628-6595bf60-1842-493a-9ca6-d430e3d02290.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776392937038-76e448ad-b061-495c-ad0e-b71d53c3cba5.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/jpeg/67537253/1776393003237-a3194d60-4351-4ed1-a6b2-d49f2c9ae0ad.jpeg)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393023508-7e3359b4-dfec-4846-ad94-c3f77a5cba8f.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393050940-bb6a6e09-967f-4a31-8178-9469c30e80ab.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393074506-e601f688-73ca-43c0-9835-88c72d7b7382.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393087779-c6d3c2d5-06cf-4c2e-a4cc-7a09554ecce5.png)

+ 总结：了解了robots.txt的作用

### 题目3：[MoeCTF 2021]Web安全入门指北—GET
+ 题目来源：MoeCTF 2021
+ 解题工具：浏览器、AI
+ 解题过程：
    1. 用AI了解GET请求的作用
    2. 分析题目中的PHP代码，构造GET参数,在靶场地址后拼接‘？moe=flag’
    3. 提交参数后拿到flag：`flag{xxxxxx}`
+ 关键截图：



<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393611939-73220249-7f1e-4ff9-80fa-de6a99e32cfc.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393642464-8c5b17dc-5604-4311-83bd-43e126107dd4.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776393669000-042bb923-3a12-4932-b3ee-c54dedb464ee.png)

+ 总结：学会了GET请求的基本用法

---

## 三、Linux环境搭建与学习笔记
### 1. 环境搭建
+ 搭建方式：VMware安装Ubuntu 22.04
+ 搭建步骤：
    1. 下载Ubuntu镜像文件
    2. VMware新建虚拟机，安装系统
    3. 配置网络和用户
+ 搭建截图：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776396500864-da437907-969e-4b35-9de2-cf010147e119.png)

抓包测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776396519465-c831c43c-afc6-49a5-9bc0-7a7cf99f2e01.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776396540578-b0079a04-e550-41b9-9c71-963ecf0dd5a3.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/67537253/1776396540519-3cd80028-ffbb-48f4-a095-c76db385eb80.png)

### 2. 基础命令学习笔记
| 命令 | 作用 | 例子 |
| --- | --- | --- |
| ls | 查看当前目录文件 | `ls -l` |
| cd | 切换目录 | `cd /home` |
| mkdir | 创建文件夹 | `mkdir test` |
| rm | 删除文件/文件夹 | `rm test.txt` |
| cat | 查看文件内容 | `cat flag.txt` |


---

## 四、学习总结
本周学会了Web安全入门工具的使用，完成了3道入门题目，搭建了Linux环境，掌握了基础的Linux命令，为后续的Web安全学习打下了基础。

