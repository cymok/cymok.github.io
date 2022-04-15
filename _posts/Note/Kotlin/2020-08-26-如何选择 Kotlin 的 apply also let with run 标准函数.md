---
layout: post
tags: Kotlin
---

如何选择标准函数

需要返回值本身(this)

- YES 返回调用者

  传递参数

  - this -> T.apply()

  - it -> T.also()

- NO 返回函数返回值

  需要扩展函数(空检测 链式调用)

  - YES 

    传递参数

    - this -> T.run()

    - it -> T.let()

  - NO 

    传递参数

    - this -> with(T)

    - it -> run(T)
	