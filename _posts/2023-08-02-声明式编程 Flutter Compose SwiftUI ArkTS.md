---
layout: post
tags: Android Flutter Compose
---

# 声明式编程

## 简介

- 命令式
  
  传统模式，通过手动找到控件进行set

- 响应式

  通过绑定，自动更新View的数据，如双向绑定或观察者模式ViewBinding等

- 声明式

  Flutter Compose SwiftUI ArkTS React 等

---

# Android 与 Flutter Compose 嵌套

---

## Android 作为父项

### Android 嵌套 Flutter
```

```

### Android 嵌套 Compose
```

```

---

## Android 作为子项

### Flutter 嵌套 Android
```

```

### Compose 嵌套 Android
```

```

# 状态

## Flutter

### 本地状态管理

对于较小规模的应用或简单组件，可以使用 StatefulWidget 和 State 类来管理本地状态。
将需要被更新的数据放在 State 对象中，并使用 setState() 方法来通知框架进行重建。这种方法适用于局部的、相对简单的状态管理。

### Provider

#### 依赖

在 pubspec.yaml 文件中添加 provider 依赖项，并运行 flutter pub get 安装依赖包

```yaml
dependencies:
  flutter:
    sdk: flutter
    
  provider: ^5.0.0
```

#### 创建数据模型类

创建 Model 类，并继承 ChangeNotifier，使用 notifyListeners() 方法在状态发生变化时通知相应的侦听器

```dart
import 'package:flutter/foundation.dart';

class CounterModel extends ChangeNotifier {
  int _counter = 0;

  int get counter => _counter;

  void increment() {
    _counter++;
    notifyListeners();
  }
}
```

#### 在顶层使用 Provider

在 MaterialApp 中使用 MultiProvider 将你的数据模型注册为全局可用的状态

```dart
void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider<CounterModel>(create: (_) => CounterModel()),
      ],
      child: MyApp(),
    ),
  );
}
```

#### 在组件中使用状态

使用 Provider.of<T>(context) 来获取状态实例，并在 UI 中使用它

```dart
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final counter = Provider.of<CounterModel>(context);

    return Scaffold(
      appBar: AppBar(title: Text('Flutter Provider Example')),
      body: Center(
        child: Text(
          'Count: ${counter.counter}',
          style: TextStyle(fontSize: 24),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => counter.increment(),
        child: Icon(Icons.add),
      ),
    );
  }
}
```

### BLoC（Business Logic Component）

### Redux

### InheritedWidget

## Compose

## ArkTS


# 路由

## Flutter

- 命名路由

```dart
// 在 MaterialApp 中定义命名路由
routes: {
  '/': (context) => HomePage(),
  '/details': (context) => DetailsPage(),
}

// 导航到命名路由
Navigator.pushNamed(context, '/details');
```
  
- 构建函数路由

```dart
// 导航到构建函数路由
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => DetailsPage(),
  ),
);
```

- 返回上一个页面
```dart
// 返回上一个页面
Navigator.pop(context);
```

- 传递数据

```dart
// 通过构造函数传递数据
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => DetailsPage(data: 'Hello'),
  ),
);

// 在接收数据的页面获取数据
class DetailsPage extends StatelessWidget {
  final String data;
  
  DetailsPage({required this.data});

  // ...
}
```

- 返回数据

```dart
// 在当前页面调用 Navigator.pop 方法
Navigator.pop(context, "Hello from second screen");

// 在上一个页面接收 pop 方法返回的数据
var result = await Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondScreen()),
);

// 处理接收到的数据
if (result != null) {
  // 打印接收到的数据
  print(result);
}
```

## Compose

参考 Android

## ArkTS

todo
