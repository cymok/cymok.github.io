---
layout: post
tags: Android Compose
---

可组合项

```

Button

Text
(JetPackCompose之Text使用指北)[https://juejin.cn/post/7064595591220690951]

TextField

Image

ProgressIndicator  
  CircularProgressIndicator  
  LinearProgressIndicator  


Column (垂直列表)  
  对其方式 
  horizontalAlignment = Alignment.CenterHorizontally  
  verticalArrangement = Arrangement.SpaceEvenly  
  verticalArrangement 可选对齐 EqualWeight SpaceBetween SpaceAround SpaceEvenly Center Top Bottom
  
  // 类似 ScrollView 的滚动  
  val scrollState = rememberScrollState(0)
  Column(
      modifier = Modifier.verticalScroll(scrollState),
  ) {}

Row (水平)  
  verticalAlignment  
  horizontalArrangement 可选对齐 EqualWeight SpaceBetween SpaceAround SpaceEvenly Center Start End  

Box (层叠 FramLayout)
  子Widget要控制位置 添加参数即可 modifier = Modifier.align(Alignment.TopStart)

ConstraintLayout 在Compose中 优势已不明显 在View中是为了解决嵌套性能问题 而Compose嵌套不存在太大性能问题

LazyColumn (类似 RecyclerView 的可滚动列表)
  LazyColumn() {
      items(items = getDatas()) { data ->
          ItemWidget(title = data)
      }
  }

LazyRow

LazyGrid (网格布局)

LazyHorizontalGrid (流布局)

LazyVerticalStaggeredGrid

Divider

Spacer

TabRow

Tab

ScrollableTabRow (超出屏幕可滚动的TabRow)

Pager 接口
  实现类:
  
  HorizontalPager
  
  VerticalPager

```

---

UI - Modifier

// Modifier 是有序的, 例如 clickable padding background 设置不同的顺序 效果是不一样的

- size(): 设置组件的固定大小。
- height() 和 width(): 设置组件的固定高度和宽度。
- aspectRatio() 设置宽高比例

- align(): 设置组件在父容器中的对齐方式。

- offset(): 将组件在父容器中的位置偏移指定的距离。
- padding(): 在组件的周围添加内边距。
- paddingFromBaseline(): 在组件的基线周围添加内边距。

- fillMaxSize(): 将组件的大小设置为父容器的最大尺寸。
- wrapContentSize(): 将组件的大小调整为其内容的尺寸。可设置居中等
- requiredWidth(): 强制组件具有指定的宽度。
- requiredHeight(): 强制组件具有指定的高度。

- background(): 设置组件的背景颜色或绘制背景样式。
- border(): 在组件周围绘制边框。
- clip(): 将组件裁剪为指定形状。

- focusable(): 将组件设置为可获取焦点，并在获取焦点时执行指定的操作。
- clickable(): 将组件设置为可点击，并在点击时执行指定的操作。
- combinedClickable(): 将多个点击事件合并为一个。可设置 单击 双击 长按
- draggable(): 将组件设置为可拖动，并在拖动时执行指定的操作。

- scale(): 缩放组件的大小。
- rotate(): 旋转组件。
- alpha(): 设置组件的透明度。

- scrollable(): 将组件设置为可滚动，并指定滚动行为。
- horizontalScroll() 和 verticalScroll()
- pointerInput(): 处理触摸和手势事件。
- indication(): 设置组件的触摸反馈效果。

- keyboardActions(): 在键盘事件发生时执行指定的操作。

- shadow(): 添加阴影效果。

- testTag(): 给组件设置测试标签，用于自动化测试。

---

Compose 第三方库

图片  
(coil-compose)[https://github.com/coil-kt/coil]
```
# dependencies 
implementation("io.coil-kt:coil-compose:2.4.0")
// implementation("io.coil-kt:coil:2.4.0")

# usage
AsyncImage(
	model = "https://img-blog.csdnimg.cn/20200401094829557.jpg",
	contentDescription = null
)

# or
Image(
    painter = rememberAsyncImagePainter(
    model = ImageRequest.Builder(LocalContext.current)
        .data("https://example.com/image.jpg")
        .size(Size.ORIGINAL) // Set the target size to load the image at.
        .build()
    ),
    contentDescription = null
)
```

官方导航库
navigation-compose
```
# dependencies
implementation 'androidx.navigation:navigation-compose:2.6.0'
```

列表 下拉刷新 等 多种自定义可组合项
[Compose中的列表](https://blog.csdn.net/lyabc123456/article/details/128510336)
