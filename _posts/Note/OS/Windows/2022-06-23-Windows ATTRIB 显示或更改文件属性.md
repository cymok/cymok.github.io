---
layout: post
tags: Windows
---

Windows 显示或更改文件属性

```
ATTRIB [+R | -R] [+A | -A] [+S | -S] [+H | -H] [+O | -O] [+I | -I] [+X | -X] [+P | -P] [+U | -U]
       [drive:][path][filename] [/S [/D]] [/L]

  +   设置属性。
  -   清除属性。

  R   只读文件属性。
  A   存档文件属性。
  S   系统文件属性。
  H   隐藏文件属性。
  O   脱机属性。
  I   无内容索引文件属性。
  X   无清理文件属性。
  V   完整性属性。
  P   固定属性。
  U   非固定属性。
  [drive:][path][filename] 指定属性要处理的文件。
  /S  处理当前文件夹及其所有子文件夹中的匹配文件。
  /D  也处理文件夹。
  /L  处理符号链接和符号链接目标的属性。
```
