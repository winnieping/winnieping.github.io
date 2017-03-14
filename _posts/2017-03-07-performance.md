---
layout: post
title: '网页性能初体验(Performance)'
---
# 网页性能初体验(Performance)  
#### 网页性能可以从多方面，多维度来评估

### 1、浏览器是如何构建页面的？---浏览器加载   
####  \*减少http请求	  
css、js优化，使用压缩工具进行打包压缩，合并为一个文件，这样浏览器能减少请求次数。  
项目中使用gulp，webpack 压缩工具
####  \*控制资源文件加载优先级    
浏览器解析html是从上往下，CSS 提前加载，把js放在body底部。
####  \*利用浏览器缓存

### 2、浏览器性能工具 ---chrome devtools timeline 
#### 2.1、工具栏  
<img src="/../assets/performace01-timeline.png"> 
<br/>
capture后面的的选项分别是：网络、js简介、截图、存储、描绘  
用户可以选者需要捕获的选项
#### 2.2、信息栏  
<img src="/../assets/performace-02.png"> 
<br/>
1：FPS 每秒刷新频率
浏览器刷新频率是60次／秒，如果页面有动画或者渐变效果，那么浏览器渲染动画或页面的每一帧时间要在1秒/60次 = 16毫秒以内，这样我们才不会感觉到卡顿，实际每帧的时间大约为10秒，因为浏览器还要做一些渲染队列管理，渲染线程切换等。  
2:CPU cpu资源占用   
表明消耗cpu资源 

3:NET  
不同颜色代表不同资源请求  
浅色代表资源请求被发送到收到第一个响应字节的时间，深色代表从收到第一个字节到这个资源完全下载完成。  

