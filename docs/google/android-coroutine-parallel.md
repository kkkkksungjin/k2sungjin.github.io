---
layout: default
title: Coroutine Parallel
parent: Android
grand_parent: Google
nav_order: 1
---
# Join & async/await
<hr/>

## __Join__{: style="color: #00c0ef"}
- launch と一緒に使用します。
- 結果は返さず、単に処理が完了するのを待つために使われます。
- 例外が発生した場合、join()を呼び出したときに即座に例外が伝播します。

~~~java
val scope = CoroutineScope(Job() + Dispatchers.Default )
viewModelScope.launch {
    val job1 = scope.launch{
        println("job1")
    }

    val job2 = scope.launch{
        throw Exception("excepiton job2")
    }
    val job3 = scope.launch{
        println("job3")
    }

    joinAll(job1, job2, job3)
    println("completed")
}
//Result
//job1
//job3
//exception
~~~


解決方法


~~~java
各launchにTry-catchをする
//Result
//job1
//Error in job2: exception job2
//job3
~~~

~~~java
val handler = CoroutineExceptionHandler { _, exception ->
    println("onJoinAll Caught exception: ${exception.message}")
}
val scope = CoroutineScope(Job() + Dispatchers.Default + handler)
//Result
//job1
//job3
//Caught exception: excepiton job2
~~~

---


## __async/await__{: style="color: #00c0ef"}
- async と一緒に使用します。
- 非同期処理の結果を返すために使われます。
- 例外が発生した場合、await()を呼び出したときに例外が伝播します。

~~~java
viewModelScope.launch {
    val aa = async {
        "aa"
    }
    val bb = async {
        throw Exception("excepiton")
    }
    val cc = async {
        "cc"
    }

    val a = try {
        aa.await()
    } catch (e: Exception) {
        println("Error in aa: ${e.message}")
        null
    }

    val b = try {
        bb.await()
    } catch (e: Exception) {
        println("Error in bb: ${e.message}")
        null
    }

    val c = try {
        cc.await()
    } catch (e: Exception) {
        println("Error in cc: ${e.message}")
        null
    }

    a?.let { println("$it") }
    b?.let { println("$it") }
    c?.let { println("$it") }
    println("complete")
}

//Reulst
//Error in aa: StandaloneCoroutine is cancelling
//Error in bb: excepiton
//Error in cc: Parent job is Cancelling
//complete
//exception
~~~


解決方法


~~~java
val a = aa.await().getOrElse { "Error in aa" }
val b = bb.await().getOrElse { "Error in bb" }
val c = cc.await().getOrElse { "Error in cc" }
//Reulst
//aa
//Error in bb
//cc
//complete
~~~

~~~java
val handler = CoroutineExceptionHandler { _, exception ->
    println("Caught exception: ${exception.message}")
}
viewModelScope.launch(handler) {
    ...
}
//Reulst
//Error in bb: exception in bb
//aa
//cc
//complete
~~~

~~~java
viewModelScope.launch {
    supervisorScope {
        ...
    }
}
//Reulst
//Error in bb: exception in bb
//aa
//cc
//complete
~~~