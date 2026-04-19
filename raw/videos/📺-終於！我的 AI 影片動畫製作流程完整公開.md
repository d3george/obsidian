---
title: "終於！我的 AI 影片動畫製作流程完整公開"
source: "https://www.youtube.com/watch?v=yfQ3fucECME"
cover: "https://yt3.googleusercontent.com/5yD9fcNcHpRV3hoImj-ICxC275EzH_oC6wPJUd8we22viEgQY0q8kDA12NkhgkWlIjD21VLVmJM=s900-c-k-c0x00ffffff-no-rj"
author:
  - "Debug 土撥鼠"
description: "---上一集很多人敲碗，想知道影片裡的動畫是怎麼做的。這集就一次大公開 —— 從工具選擇、Remotion 安裝、搭配 Claude Code 客製化，到怎麼用 DaVinci Resolve 讓動畫跟旁白對上，完整流程不藏私。最後也聊了一件很多人想做但幾乎都會失敗的事：全自動 AI 產片 —— 為什麼流程不是問題，腳本才是。📌 這支影片你會看到：- Remotion 是什麼？為什"
created: 2026-04-16
---
```vid
https://www.youtube.com/watch?v=yfQ3fucECME
Title: 終於！我的 AI 影片動畫製作流程完整公開
Author: Debug 土撥鼠
Thumbnail: https://i.ytimg.com/vi/yfQ3fucECME/mqdefault.jpg
AuthorUrl: https://www.youtube.com/@debugtuboshu
```

![](https://www.youtube.com/watch?v=yfQ3fucECME)

\---  
上一集很多人敲碗，想知道影片裡的動畫是怎麼做的。這集就一次大公開 —— 從工具選擇、Remotion 安裝、搭配 Claude Code 客製化，到怎麼用 DaVinci Resolve 讓動畫跟旁白對上，完整流程不藏私。  
  
最後也聊了一件很多人想做但幾乎都會失敗的事：全自動 AI 產片 —— 為什麼流程不是問題，腳本才是。  
  
📌 這支影片你會看到：  
  
\- Remotion 是什麼？為什麼用程式碼做動畫比手做更快？  
\- Remotion 完整安裝流程（從零開始）  
\- 搭配 Claude Code 客製化動畫的實際操作  
\- 透明背景影片怎麼輸出？（ProRes 正確設定）  
\- 音效怎麼挑？讓 AI 從資料夾裡自動配音效  
\- 沒有模板時，Prompt 要怎麼下才有效？  
\- DaVinci Resolve 四個核心手法：分割、加快、放慢、凍結  
\- 動畫跟旁白怎麼對齊？完整示範拆解  
\- 全自動 AI 產片可行嗎？我的看法  
  
📌 相關連結  
  
Remotion Lab 網站：https://remotionlab.com  
Remotion Lab 社團：https://www.facebook.com/groups/2029456680937896  
  
0:00 上一集很多人敲碗  
0:09 Remotion 是什麼？  
0:14 Remotion 安裝教學  
0:56 Remotion Studio 介紹  
1:03 Remotion 能做哪些動畫？  
1:20 我做的 Remotion 範例網站  
1:33 搭配 Claude Code 快速客製化  
2:12 透明背景影片輸出方式（ProRes）  
2:45 音效從哪裡找？讓 AI 幫你挑  
2:59 沒有模板，Prompt 怎麼下？  
3:32 腦中有畫面，才能駕馭 AI  
4:12 DaVinci Resolve 四個核心功能  
4:33 動畫跟旁白怎麼對齊？完整示範  
5:58 全自動 AI 產片，為什麼幾乎都會失敗？  
6:27 結語  
  
#Remotion #ClaudeCode #VibeCoding #影片剪輯 #DaVinciResolve #AI做影片 #YouTuber #程式做動畫 #自媒體 #內容創作

## Transcript

### Many asked after the last episode

**0:00** · 上一集很多人敲碗 这些视频动画是怎么做的 今天就一次大公开我的剪辑流程 主要用到两个工具 1. Remotion 2. DaVinci Resolve Remotion 是一个可以直接通过程序代码的方式来做动画的工具 安装的步骤很简单 我快速带大家走过 首先打开终端机 然后创建一个文件夹 用 cd 这个指令进入文件夹后 可以输入 node --version 确认一下 Node 版本至少要在 16 以上

### What is Remotion?

### Remotion installation guide

**0:25** · 接下来输入画面上这个指令 就可以按照指示安装 第一个跳出来询问是否安装模板 我们选择 Blank 是否添加 TailwindCSS 选择 Yes 是否安装 Skill 选择 Yes 接下来都选择预设值就可以完成安装了 操作完安装指示后 要记得输入 npm i 来安装一些套件

**0:45** · 套件安装完后 输入 npm run dev 就可以开启 Remotion Studio 打开浏览器 网址栏上输入 localhost:3000 就可以开启 Remotion Studio 这个后台 未来做完的动画都会呈现在这里 可以在这里浏览和汇出文件 Remotion 实际上可以做到哪些动画呢?

### Introducing Remotion Studio

### What animations can Remotion make?

**1:03** · 字卡 可以从四面八方跳出说明文字 片头片尾 可以调整出现的速度和方式 社群媒体 可以做一个假的社群贴文样式 输入自己要的内容 甚至是我过去做的所有视频 穿插在屏幕录影之间 也都是 Remotion 做的 我做了一个网站 里面包含了官方文件的中译版 也包含一些基础的教学在里面 看完应该会有所收获 如果懒的话 我也把足够多的范例放到上面了 大家可以下载回去之后改成自己需要的版本

### My Remotion example website

**1:32** · 我这边举一个例子 如果我今天想要类似这个字卡的效果 登录后把文件下载下来 并且可以直接在上面写 想要怎么改这个提示词 例如说我希望改成四个阶段 而不是三个阶段 接着把文件丢给 Claude Code 以及刚刚复制下来的 Prompt 也丢给 Claude Code 他就会直接修改 修改完成后打开 Remotion Studio 发现说 欸? 怎么没有视频?

### Quick customization with Claude Code

**1:54** · 这时候再回到 Claude Code 跟他说 请帮我处理 这个问题 修改完成后 Remotion Studio 这边就有视频了 就可以看到 原本模板只有三个阶段 但我们可以定制化改成四个阶段 不只可以改卡片的张数 也可以改文字 想要怎么改直接跟 Claude Code 说就可以了 若要输出成视频文件 点击左上角的 File Render Render Video 视频就会出现在 Out 这个文件夹里面

### Exporting video with transparent background (ProRes)

**2:17** · 我们再举一个例子 如果要显示这个字卡 一样可以把文件下载下来 并且修改提示词 再将文件和 Prompt 直接丢给 Claude Code 就可以得到编辑后的字卡 透明背景的视频输出方式比较不同 点击 File Render ProRes 4444XQ 左边 Picture 选择 PNG Format 选最下面的 Render Video 文件一样会在 Out 文件夹里面 在这边看背景一样是黑的 但在剪辑软件里面看其实已经是透明的了

**2:43** · 接下来我们来看一下音效怎么处理 音效我都会在 Envato 这个网站下载 我会一次下载好所有需要的音效 我会把所有音效放到一个文件夹当中 并且告诉 Claude Code 从这个文件夹当中挑选适合的音效 接下来我们就可以看到 动画有套上音效了 再来如果找不到模板 想要自己定制化的话 Prompt 该怎么下呢?

### Where to find sound effects? Let AI pick for you

### No template — how do you write your Prompt?

**3:02** · 举例来说 如果今天有一幕 是要呈现台北 好吃 好玩 又方便 那这一幕的 Prompt 该怎么下呢?

**3:08** · 为了避免它只是输出文字给我 所以我都会请它用 SVG 的动画 加简单的标题来呈现 我们可以看到成果 其实效果不差 它甚至把 101 都画出来了 但整体的风格并不是我喜欢的 因此我重新调整了 Prompt 我希望是简单干净的呈现方式

**3:24** · 可以看到成果 现在的样子就会比较像是 我喜欢的风格 也就是我目前 YouTube 大部分动画呈现的模式 所以我实验下来 如果要能够比较好的 去操控 Remotion 的方式是 脑中一定要对于成果 是有想象的 然后尽可能的描述 不然效果可能会不如预期 举例来说 今天有一幕是指说 Claude Code 加 Remotion 可以快速做出很多视频动画 是视频制作的好工具 在我脑中 针对这一幕的想象就是 两个 Logo 对撞之后 会往外炸裂出一张一张的卡片 每个卡片都是代表一个动画的特效

### You need a vision in your head to control AI

**3:55** · 我们可以看到成果 Claude Code 就尽可能的 依照我给他的描述 来呈现动画的效果 跟 Claude Code 最好的协作方式 就是你当脑 他当手 如果还有 Remotion 相关的问题 可以在社群当中发问 我也会即时更新最新的信息喔 看完了 Remotion 制作视频的方法后

### Four core features of DaVinci Resolve

**4:12** · 接下来我们看到剪辑软件 DaVinci Resolve 会选这个剪辑软件 只因为它是我接触的 第一个免费的剪辑软件 替换成别的软件也没问题 因为我只用到四个功能 分割 加快 放慢 冻结 举例来说 如果我 今天有一个动画长这样子

**4:28** · 我有三个重点想要讲 我要怎么让我的动画 跟我的旁白对在一起呢 第一步 我会先录音 用 Remotion 制作视频 要注意以下三件事情 一 不要过度动画 有时候画面一直闪 会让观众感到不适 二 不要图文不符 因为图文不符 就失去做动画的意义了 直接拿开源的视频模板 套一套就好了 三 风格一致性 不管是用色 排版 速度等 一致性可以做出自己的风格 也可以让观众看起来更舒服 这段视频有一个最大的问题 就是上面的动画很短 下面的语音很长 所以要进到第二步骤 将动画做分割 我会先把三张卡出现的时间点

### How to sync animations with voiceover? Full demo

**4:55** · 切割出来 以及动画在淡出的位置 也分割出来 切割出来后 再摆到旁白念 1 2 3 的位置 以及把刚刚最后一段 淡出的片段 放到结尾的地方 第三步 把空缺补满 先来看第一个缺口 按下 Command+R 会出现箭头 可以拖曳改变片段的速度 这边用放慢的方式来补足缺口

**5:17** · 针对第二种缺口 我用另外一种方式 在第二个片段 结尾的地方 切出一小帧 选择这一帧之后按下右键 点击改变片段速度 勾选冻结帧 按下 Change 拖曳这一帧的长度 补足这个缺口 剩下的缺口依样画葫芦 就可以完成了 我们来看成果 Remotion 制作视频 要注意以下三件事情 1 不要过度动画 有时候画面一直闪 会让观众感到不适 2 不要图文不符 因为图文不符就失去做动画的意义了 直接拿开源的视频模板 套一套就好了 3 风格一致性 不管是用色 排版 速度等 一致性可以做出自己的风格 也可以让观众看起来更舒服 看起来成果蛮不错的

**5:47** · 放慢跟加快是同样的概念 一个拉长 一个缩短 这边就不演示了 而至于要怎么运用这四个手法 大家可以排列组合试试看 舒服的呈现最重要 没有一定的标准 学到这里 有人会开始思考一件事情 如果我能用 n8n 或其他自动化的方式 请AI写脚本 然后用 Elevenlabs 等工具 将文字转成语音 最后套用 Remotion 来做动画 用这样的方式就可以无限产出视频

### Fully automated AI video production — why does it almost always fail?

**6:11** · 就可以无痛经营自媒体 这边温馨提醒一下 这样的方法失败率是很高的 因为自媒体最大的问题不是流程 而是脚本 所以延续上一集说的 自动化是第二步 第一步是怎么用很人工 费力的方式 先做出好的内容 才是关键 以上就是全部的内容了 至今为止所有的视频 都是用这个工作流做的 整体蛮稳定简单 欢迎大家尝试看看 我们下一集见 拜拜




