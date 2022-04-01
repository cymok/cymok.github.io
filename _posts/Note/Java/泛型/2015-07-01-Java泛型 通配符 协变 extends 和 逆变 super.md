---
layout: post
tags: Java
---

#### 定义

逆变与协变用来描述类型转换（type transformation）后的继承关系，其定义：如果A、B表示类型，f(⋅)表示类型转换，≤表示继承关系（比如，A≤B表示A是由B派生出来的子类）

f(⋅)是逆变（contravariant）的，当A≤B时有f(B)≤f(A)成立；  
f(⋅)是协变（covariant）的，当A≤B时有f(A)≤f(B)成立；  
f(⋅)是不变（invariant）的，当A≤B时上述两个式子均不成立，即f(A)与f(B)相互之间没有继承关系  

#### 协变

`<? extends>` 实现了泛型的协变
```
List<? extends Number> list = new ArrayList<Integer>();
```
?是子类,即表示E及其子类,固定上边界

#### 逆变

`<? super>` 实现了泛型的逆变
```
List<? super Number> list = new ArrayList<Object>();
list.add(new Integer(1));
list.add(new Float(1.2f));
```
固定下边界

---

#### 使用时机：

- 要从泛型类取数据时，用 extends

- 要往泛型类写数据时，用 super

- 既要取又要写，就不用通配符（即 extends 与 super 都不用）

《Effective Java》中提到: producer-extends, consumer-super.
