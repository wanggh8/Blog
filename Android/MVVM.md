---
title: MVVM框架模式 
date: 2020-05-27 10:31:12
tags: Android


---

<!--more-->

MVVM各个对应的层的职责相似：

Model层，主要负责数据的提供。Model层提供业务逻辑的数据结构（比如，实体类），提供数据的获取（比如，从本地数据库或者远程网络获取数据），提供数据的存储。
View层，主要负责界面的显示。View层不涉及任何的业务逻辑处理，它持有ViewModel层的引用，当需要进行业务逻辑处理时通知ViewModel层。
ViewModel层，主要负责业务逻辑的处理。ViewModel层不涉及任何的视图操作。通过官方提供的Data Binding库，View层和ViewModel层中的数据可以实现绑定，ViewModel层中数据的变化可以自动通知View层进行更新，因此ViewModel层不需要持有View层的引用。ViewModel层可以看作是View层的数据模型和Presenter层的结合。

https://blog.csdn.net/u012317510/article/details/80247756



https://github.com/darryrzhong/Android-MvvmComponent-App





