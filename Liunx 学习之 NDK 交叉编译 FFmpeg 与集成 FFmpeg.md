Liunx 学习之 NDK 交叉编译 FFmpeg 与集成 FFmpeg 
NDK 与 FFmpeg 环境配置
什么是 FFmpeg
FFmpeg 帮助选项
在 Ubuntu 中编译 FFmpeg
FFmpeg 编译的命令操作
FFmpeg 编译工作

Android 音视频基础知识
音视频完整解码播放流程
编码原理
H.264 是什么
H.264 编码分析

音视频播放器
CMake 配置项目环境讲解
上层代码的编码
项目流程图与FFmpeg函数
Java 层 Player
C++ 层 Player搭建与线程解封装
Java 与 C++ 层交互
阻塞式
C++ 层阻塞式队列的实现
将 C++ 层音视频压缩包加入队列
视频解码与播放渲染工作
C++ 层视频解码与播放渲染
函数指针回调rgba数据逻辑
ANativeWindow 渲染代码编写
为何要字节对齐

音频解码
OpenSLES 基础使用
OpenSLES 创建工作
OpenSLES 原理
OpenSLES 回掉函数工作
音频缓存讲解
音频重采样工作

Native 层内存泄漏分析与隐患补救
如何排查 Native 层错误
fps概念与时间基 Timebase 概念
音视频同步编码实现
特殊无法播放视频的解决方案

如何显示总时长
总时长区分直播与视频
C++层的播放更新上层拖动条
上册拖动条控制 C++ 播放 seek
播放器的释放