---
layout: post
tags: Android Compose
---

[Google 官方文档](https://developer.android.google.cn/jetpack/compose/interop?hl=zh-cn)


## Compose 与 Android 框架
CompositionLocal 用于为 Compose 中的 Android 框架类型（例如 Context、Configuration 或 View）传递值，
其中 Compose 代码托管在相应的 `LocalContext`、`LocalConfiguration` 或 `LocalView` 中。
请注意，CompositionLocal 类的前缀是 Local，IDE 中的自动补全功能可以很轻松地检测到这些类。
```
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    val context = LocalContext.current
    val view = LocalView.current
    val windowInfo = LocalWindowInfo.current
//    val xxx = LocalXxx.current
    
}
```

## Android View 中的 Compose , `ComposeView` 作为桥梁

将 Compose 界面内容并入 fragment 或现有视图布局 `ComposeView.setContent()` (ComposeView也可写在布局)

并入 fragment 关联生命周期
```
override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View {
    return ComposeView(requireContext()).apply {
        setViewCompositionStrategy(ViewCompositionStrategy.DisposeOnViewTreeLifecycleDestroyed)
        setContent {
            Text("HomeFragment")
        }
    }
}
```

如果同一布局中存在多个 `ComposeView` 元素，每个元素必须具有唯一的 ID 才能使 `savedInstanceState` 发挥作用，
```
addView(ComposeView(...).apply {
        id = R.id.compose_view_x
        ...
      }
```

`ComposeView` ID 在 `res/values/ids.xml` 文件中进行定义
```
<resources>
    <item name="compose_view_x" type="id" />
    <item name="compose_view_y" type="id" />
</resources>
```

## Compose 中的 Android View , `AndroidView` 作为桥梁

系统会向 AndroidView 传递一个返回 View 的 lambda。AndroidView 还提供了在视图膨胀时被调用的 update 回调。每当在该回调中读取的 State 发生变化时，AndroidView 都会重组

```
@Composable
fun CustomView() {
    val selectedItem = remember { mutableStateOf(0) }

    // Adds view to Compose
    AndroidView(
        modifier = Modifier.fillMaxSize(), // Occupy the max size in the Compose UI tree
        factory = { context ->
            // Creates custom view
            CustomView(context).apply {
                // Sets up listeners for View -> Compose communication
                myView.setOnClickListener {
                    selectedItem.value = 1
                }
            }
        },
        update = { view ->
            // View's been inflated or state read in this block has been updated
            // Add logic here if necessary

            // As selectedItem is read here, AndroidView will recompose
            // whenever the state changes
            // Example of Compose -> View communication
            view.coordinator.selectedItem = selectedItem.value
        }
    )
}

@Composable
fun ContentExample() {
    Column(Modifier.fillMaxSize()) {
        Text("Look at this CustomView!")
        CustomView()
    }
}
```

如需嵌入 XML 布局

使用 `androidx.compose.ui:ui-viewbinding` 库 提供的 `AndroidViewBinding`  
layout_example.xml 生成的
```
@Composable
fun AndroidViewBindingExample() {
    AndroidViewBinding(ExampleLayoutBinding::inflate) {
        exampleView.setBackgroundColor(Color.GRAY)
    }
}
```

## Compose中的状态 remember API

remember 函数用于在 @Composable 函数内部创建可记忆的状态。如果您不使用remember函数，会导致以下情况发生：

1. 重复创建状态：每次调用包含状态的`@Composable`函数时，都会重新创建该状态对象。这意味着在函数重新执行时，将创建一个新的状态实例，并且之前存储的状态数据将被丢失。
2. 界面不稳定：由于状态对象的重新创建，界面可能会变得不稳定。例如，输入框的文本内容可能会在每次重新构建时被重置为初始值，无法保留用户的输入。
3. 性能问题：频繁地创建和销毁状态对象会引起性能问题。每次函数重新执行时，都需要分配内存和初始化新的状态对象，而不是重用之前的状态。

通过使用 `remember` 函数，可以将状态对象缓存在 `@Composable` 函数外部，从而避免上述问题。它会记住并在函数重新调用时重用之前保存的状态对象，确保界面的稳定性和一致性，同时提供更好的性能。


与Flutter一样 声明式UI 当变量改变时 需要刷新Widget 才能更新UI

Compose中 将变量声明为State类型使其变为Compose可观察的状态，Compose监测到状态变化触发函数重组

mutableStateOf
```
@Composable
fun Counter(){
    var number by remember {
        mutableStateOf(0)
    }
    Column() {
        Text(text = "当前数值:$number")
        Button(onClick = {
            number++
        }) {
            Text(text = "add")
        }
    }
}
```

状态提升 即 将状态移至可组合项的调用方以使可组合项无状态的模式  
说人话就是 把可变参数抽到可组合项函数外 在调用函数的地方传参 你要怎么处理参数 与可组合项参数无关

rememberSaveable 可保存状态 可应对屏幕旋转等

## Effect API 又被翻译为 效应 效果 副作用

```
LaunchedEffect 在某个可组合项的作用域内运行挂起函数
rememberCoroutineScope 获取组合感知作用域，以便可以在组合外启动协程
rememberUpdatedState 在效应中引用某个值，该效应在值改变时不重启
DisposableEffect 需要清理的效应
SideEffect 将 Compose 状态发布为 非 Compose 代码
produceState 将非 Compose 状态转为 Compose 状态
derivedStateOf 如果某个状态是从其他状态对象计算或者派生出来的，请使用 derivedStateOf，使用此函数可以确保当计算中使用的状态之一发生变化时才会进行计算
snapshotFlow 将 Compose 的 State 转为 Flow
```

例如，如果你希望在某个状态值发生变化时执行异步操作，可以将该状态值作为键传递给LaunchedEffect。  
每当该状态值发生变化时，LaunchedEffect会重新启动协程并执行相应的异步操作。

## `compose-bom` Bill of Material 的缩写

```
    // Compose BoM
    def composeBomVersion = "2023.06.01"
    api  platform("androidx.compose:compose-bom:${composeBomVersion}")
    androidTestApi  platform("androidx.compose:compose-bom:${composeBomVersion}")
```

可以统一管理 Compose 库版本，确保应用中的 Compose 库版本兼容，这只是管理版本，还是要自己添加使用的库到项目的
