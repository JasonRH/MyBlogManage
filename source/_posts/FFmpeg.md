---
title: FFmpeg视频工具
categories: 其它
tags:
  - 视频
abbrlink: 40339
date: 2019-06-10 13:00:00
---

FFmpeg是非常强大的多媒体视频处理工具。
功能包括视频采集功能、视频格式转换、视频抓图、给视频加水印等。
下载地址：https://ffmpeg.org/

#### 命令

1080P转720P
    
    ./ffmpeg -i A.mp4 -c copy -c:v libx264 -vf scale=-2:720 B.mp4

mp3转换pcm
    
    ./ffmpeg -y -i test.mp3 -acodec pcm_s16le -f s16le -ac 1 -ar 16000 16k.pcm


参数	 | 说明
---|---
-y | 	允许覆盖
-i test.mp3 | 源文件
-acodec pcm_s16le	| 编码器
-f s16le | 强制文件格式
-ac 2 | 双声道
-ar 16000 | 采样率

    
    
pcm转mp3    

    ffmpeg -y -f s16be -ac 2 -ar 16000 -acodec pcm_s16le -i 16k.pcm new_mp3.mp3

pcm播放
    
    ./ffplay -ar 16000 -channels 1 -f s16le -i xxx.pcm
    
MP3截取

    ffmpeg -y -i test.mp3 -ss 00:00:00 -t 00:00:03 -acodec copy output_mp3.mp3    
参数|说明
---|---
-y	| 允许覆盖
-i test.mp3	| 源文件
-ss 00:00:00 | 开始时间
-t 00:00:03 | 结束时间
-acodec copy | 编码格式复制    