---
layout: default
title: Coroutine Job
parent: Android
grand_parent: Google
nav_order: 1
---

# Job & SupervisorJob
<hr/>

## __Job__{: style="color: #00c0ef"}
子コルーチンが失敗すると親や他の子コルーチンもキャンセルする。

## __SupervisorJob__{: style="color: #00c0ef"}
子コルーチンが失敗しても他の子コルーチンに影響を与えないようにする。

- 独立した作業の処理
複数の作業を同時に実行し、ある作業が失敗しても他の作業には影響を与えずに続行したい場合に使用します。
例えば、複数のタスクを並行して実行し、1つのタスクがエラーで停止しても他のタスクはそのまま進行できます。

- UIイベント処理
UI上で複数の非同期処理を実行する際に、1つの処理が失敗しても他の処理を継続させたい場合に便利です。
例えば、ボタンのクリックイベントやデータ読み込みの処理が独立している場合、1つが失敗してもアプリ全体が停止しないようにします。

- ネットワークリクエスト処理
複数のネットワークリクエストを並行して実行し、1つのリクエストが失敗しても他のリクエストを正常に処理したい場合に使用します。
例えば、複数のAPIリクエストを同時に行い、1つがタイムアウトしても他のリクエストが成功するようにします。

**viewModelScope == SupervisorJob**

| **コンテキスト**                                     | **実行結果**                                 | **説明**                                                                                              |
|------------------------------------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| **子 `CoroutineScope`**                             | `a`, `b`, `c`, `d`, `exception`             | 子コルーチンの1つで例外が発生すると、そのコルーチンと親がキャンセルされ、以降の処理は実行されない。    |
| **子 `CoroutineScope` + `CoroutineExceptionHandler`** | `a`, `b`, `c`, `d`, `f`, `exception`        | 例外が発生したコルーチンのみキャンセルされ、`CoroutineExceptionHandler`で例外処理後、他は継続される。    |
| **子 `supervisorScope`**                           | `a`, `b`, `c`, `d`, `f`, `exception`        | `supervisorScope`は1つの子コルーチンが失敗しても、他の子には影響せずに処理が継続される。               |
| **子 `supervisorScope` + `CoroutineExceptionHandler`** | `a`, `b`, `c`, `d`, `f`, `exception`, `e`   | 例外は`CoroutineExceptionHandler`で処理され、すべての子コルーチンが正常に実行される。                  |

#### 子コルーチン coroutineScope
~~~java
fun onJob1() {
    viewModelScope.launch {
        println("a")
        launch {
            println("b")
            coroutineScope { 
                launch {
                    println("d")
                    throw Exception("exception")
                }
                launch {
                    println("e")
                }
            }
        }

        launch {
            println("c")
            withContext(Dispatchers.IO) {
                launch {
                    println("f")
                }
            }
        }
    }
}

//Reulst
// a
// b
// c
// d
// exception
~~~

#### 子コルーチン coroutineScope CoroutineExceptionHandler
~~~java
fun onJob1() {
    val handler = CoroutineExceptionHandler { _, exception ->
            println("Caught exception: ${exception.message}")
    }
    viewModelScope.launch(handler) {
        println("a")
        launch {
            println("b")
            coroutineScope { 
                launch {
                    println("d")
                    throw Exception("exception")
                }
                launch {
                    println("e")
                }
            }
        }

        launch {
            println("c")
            withContext(Dispatchers.IO) {
                launch {
                    println("f")
                }
            }
        }
    }
}

//Reulst
// a
// b
// c
// d
// f
// exception
~~~

#### 子コルーチン supervisorScope
~~~java
fun onJob1() {
    viewModelScope.launch {
        println("a")
        launch {
            println("b")
            supervisorScope { 
                launch {
                    println("d")
                    throw Exception("exception")
                }
                launch {
                    println("e")
                }
            }
        }

        launch {
            println("c")
            withContext(Dispatchers.IO) {
                launch {
                    println("f")
                }
            }
        }
    }
}

//Reulst
// a
// b
// c
// d
// f
// exception
~~~

#### 子コルーチン supervisorScope CoroutineExceptionHandler
~~~java
fun onJob1() {
    val handler = CoroutineExceptionHandler { _, exception ->
            println("Caught exception: ${exception.message}")
    }
    viewModelScope.launch(handler) {
        println("a")
        launch {
            println("b")
            supervisorScope { 
                launch {
                    println("d")
                    throw Exception("exception")
                }
                launch {
                    println("e")
                }
            }
        }

        launch {
            println("c")
            withContext(Dispatchers.IO) {
                launch {
                    println("f")
                }
            }
        }
    }
}

//Reulst
// a
// b
// c
// d
// f
// exception
// e
~~~