---
layout: post
tags: Harmony Flutter Kotlin
---

# 异步结果，TS 的 Promise，Dart 的 Future，Kotlin 的 Deferred

都表示一个尚未完成的计算，需要等待，使用 await 或者 then 来处理

await 是在异步环境里同步返回结果，then 是在同步环境回调里异步返回结果。差不多

## TS 的 Promise

```TypeScript
function fetchData(): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('Hello from TypeScript!');
    }, 2000);
  });
}

// 使用 then
fetchData().then((data) => {
  console.log(data);
});

// 使用 async/await
async function fetchDataAsync() {
  const data = await fetchData();
  console.log(data);
}

fetchDataAsync();
```

## flutter 的 Future，async + await

```Flutter
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Hello from Flutter!';
}

// 使用 then
fetchData().then((data) {
  print(data);
});

// 使用 async/await
void fetchDataAsync() async {
  final data = await fetchData();
  print(data);
}

fetchDataAsync();
```

## kotlin 的协程，async + await

```Kotlin
import kotlinx.coroutines.*

fun fetchData(): Deferred<String> = GlobalScope.async {
    delay(2000)
    "Hello from Kotlin!"
}

// 使用 await
fun main() = runBlocking {
    val data = fetchData().await()
    println(data)
}

// 使用 then 风格的方式（invokeOnCompletion 不能获取到结果，只能进行最终回调）
fun fetchDataWithThen(callback: (String) -> Unit) {
    GlobalScope.launch {
        val data = fetchData().await()
        callback(data)
    }
}

fun main() {
    fetchDataWithThen { data ->
        println(data)
    }
    Thread.sleep(3000) // 等待异步任务完成
}
```
