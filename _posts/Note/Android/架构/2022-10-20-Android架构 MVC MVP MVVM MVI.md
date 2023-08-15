---
layout: post
tag: Android
---

# MVC

在远古时期，Android使用的一种架构
以 xml 布局 作为 View
以 Activity 作为 Control
以 数据模型 或 Repository 作为 Model 

缺点是 Activity 会随着业务的复杂而逐渐臃肿膨胀，难以维护，耦合性高

# MVP

依赖接口通信的解耦方式

Model - 负责数据处理部分
View - 负责UI界面展示及用户操作交互，Activity/Fragment 实现 View 接口
Presenter - 通过 Model 获取数据，处理大部分逻辑，调用 View 接口 的 UI 更新 函数，降低 Model 与 View 的耦合性

缺点是 要创建大量的接口，V 与 P 耦合性过高，更适合大型项目

---

# MVVM

M - 负责数据处理部分
V - Activity/Fragment View 布局等
VM - ViewModel 连接 M 与 V 的桥梁，VM 与 M 直接交互，通过观察者模式（DataBinding 或 ViewBinding） 间接把数据更新通知给 View 以实现解耦

M 负责处理数据存储网络传输,  
VM 不持有 V 对象，作为被观察者, 负责处理逻辑和调用获取数据函数等,  
V 持有 VM 的对象, 作为观察者, 根据 VM 数据的更新去处理 UI,  

使用 DataBinding 的项目 可以实现双向绑定 缺点是 `DataBinding` 构建时间需要更久

官方已不推荐使用 `DataBinding` 现在推荐使用 `ViewBinding`

显然 `ViewBinding` 淘汰了 `DataBinding` `anko` `kotlin-android-extensions`

---

# MVI

Model-View-Intent

M - 负责处理数据的状态和逻辑
V - 负责展示数据和用户界面
I - 用户操作如点击和输入，事件 意图 动作

主要为了ViewModel层和View层的交互由双向转化为单向

是一种响应式和流式的处理思想，是单向数据流的
将意图事件（用户操作），通过函数转换为特定Model（状态），将其结果反馈给用户（渲染界面）。

响应式编程: RxJava Kotlin的Flow

