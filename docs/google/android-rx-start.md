---
layout: default
title: Rx
parent: Android
grand_parent: Google
nav_order: 1
---

# Rx [http://reactivex.io](http://reactivex.io)

### RxJava?
Javaでは、並行処理を行う際に手間が多いため、これを解決するためにNetflixがRxJavaを開発しました。
クライアントのリクエストを処理する際、多数の非同期実行フロー（スレッドなど）を生成し、それらの結果を集約して最終的に返す方式で、内部の処理方式を変更できるように開発されました。

1. 並行性を積極的に受け入れる必要があります (Embrace Concurrency)
2. JavaのFutureを組み合わせることが難しいという点を解決します (Java Futures are Expensive to Compose.)
3. コールバック方式の問題点を改善する必要があります(Callback Have Their Own Problems.)

##### RxJava compile : 'io.reactivex.' + rxjava2:rxjava:2.x

<table rules="groups">
  <thead>
    <tr>
      <th style="text-align: left">リアクティブ API</th>
      <th style="text-align: center">説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">RxLifecycle</td>
      <td style="text-align: left">RxJavaを使用するAndroidアプリ用のライフサイクル処理APIとして、メモリリークの管理に使用できます。</td>
    </tr>
  </tbody>

</table>

基本
<hr/>

~~~ java
//コード
dependencies{
    compile : io.reactivex.rxjava2:rxjava:2.x.y
}
//コード
import io.reactivex.*;
~~~

<br/>

#### Observable
基本
> just(),subscribe(),dispose(),create()

FromXXX()
> fromArray, fromIterable, fromCallable, fromFuture, fromPublisher

Class
> Single, Maybe, Subject, ConnectableObservable

##### rxjava 1.x > just, create, fromがある。

<br/>

| rxjava 1.x | rxjava 2.x| 
|:--------|:-------:|
| Observable  | Observable  | 
|----
|  x  | Maybe  | 
|----
|  x  | Flowable  | 
|----
{: rules="groups"}

###### rxjava 1.x と rxjava 2.x 差
#### RxJava オペレーター <br/>
 基本 関数 : map(),filter(),reduce(),flatmap() <br/>
 生成 関数 : interval, timer, range, intervablRange, defer, repeat<br/>
 変換 関数 : concatMap, swithMap, groupBy, scan<br/>
 組み合わせ 関数 : zip, combineLatest, merge, concat<br/>
 条件 関数 : amb, takeUntil, skipUntil<br/> 
 オペレーター 関数 : delay, timeInterval <br/>
 
 
<hr/>
<br/>

#### Schedulers 

| Schedulers |rxjava 1.x | rxjava 2.x| 
|:--------|:-------:|:-------:|
| New Thread  | .newThread()  |.newThread() |
|----
| Single Thread  | サポートなし  | .single()|
|----
| Computation   | .computation()  |.computation()  |
|----
|  IO  | .io()  |.io() |
|----
|  Trampoline  | .trampoline()  | .trampoline()|
|----
|  Main Thread  | .immediate()  | サポートなし |
|----
|  Test  | .test()  | サポートなし |
|----
{: rules="groups"}

_newThread_{: style="color: #e26716"} - 購読者が追加されるたびに、新しいスレッドが生成されるという意味です。
<br />


_single_{: style="color: #e26716"} - 単一のスレッドを別途生成して購読作業を行います。複数回の購読リクエストがあっても、単一のスレッドを共有して使用します。 <br >

_computation_{: style="color: #e26716"} - 一般的な計算作業を行うときに使用するスケジューラです。interval()関数は基本的にcomputationスケジューラで動作します。CPUに対応して計算するスケジューラで、IO作業は実行できません。プロセス数に応じてスレッドプールを増やすことができます。
~~~ java
//コード
public static Observable<Long> interval(long perid, TimeUnit unit){
    return interval(perid, period, unit, Scheduler.computation());
}
~~~
<br/>

_io_{: style="color: #e26716"} - ネットワーク上のリクエスト、ファイルの入出力、DBクエリなどを処理する際に使用するスケジューラです。
<br >

_trampoline_{: style="color: #e26716"} - 新しいスレッドを生成せず、現在のスレッドで無限のサイズの待機キューを生成するスケジューラです。新しいスレッドを生成しないことと、自動的に待機キューを作成してくれる点が、newThreadスケジューラ、computationスケジューラ、IOスケジューラと異なります。


_subscribeOn_{: style="color: #e26716"} - subscribeでデータを発行する際に処理するスレッドを指定します。  <br >
_observeOn_{: style="color: #e26716"} - Observableでデータを処理する際に使用するスレッドを指定します。<br >
<hr/>

~~~ java
String[] strObj = {"K-1","S-2","J-3"};
Observable<String> source = Observalbe.fromArray(strObj)
.doOnNext(data - > Log.v(" log data["+data+"]"))
.subscribeOn( Schedulers.newThread())
.observeOn( Schedulers.newThread())
.map(Shape::flip);
source.subscribe(Log::i);

~~~
subscribeOn() 関数は、Observableを生成した後、実行される際に使用するスレッドを指定する関数です。
observeOn() 関数は、Observableで生成されたデータが処理される全体を回すスレッドを指定する関数です。
##### ~ subscribeOn()とobserveOn()のスレッドは確実に分離するのが良いです。（両方とも宣言するようにしましょう）

_just_{: style="color: #e26716"} - データを順番に発行する関数で、Observableを生成し、1つの値を入れることができ、同じ型の引数を最大10個まで処理可能です。 
<br />

~~~ java
//コード
Observable.just(1,2,3).subscribe(System.out::println);
~~~


~~~ java
//結果
1
2
3
~~~

<br/>

_subscribe_{: style="color: #e26716"} -  動作させるために準備しておいたObservableを実行するときに使用します。 <br >

~~~ java
//원형
Disposeable subscribe()
...
~~~

引数のない**subscribe()**は、onNextやonCompleteイベントを無視し、onErrorイベントが発生したときだけ例外を投げるため、Observableコードをテストやデバッグするときに便利です。

_dispose_{: style="color: #e26716"} - データの発生を解除するときに使用します。Observableと購読者の関係を切るために使用されます。onCompleteが発生するとdisposeが自動的に呼び出されるため、別途disposeを呼び出す必要はありません。
<br/>

~~~ java
//コード
Observable<String> ob = Observable.just("k","s","j")
Disposable d = ob.subscribe(
    v -> System.out.println("onNext : "+ v),
    err -> System.out.println("onError"),
    () -> System.out.println("onComplete")
);
System.out.println("T/F : "+d.isDisposed);
~~~

~~~ java
//結果
onNext : k
onNext : s
onNext : j
onComplete
T/F : true
~~~

_create_{: style="color: #e26716"} - onNext、onComplete、onErrorは開発者が直接呼び出す必要があります。直接呼び出す必要があるため、onErrorやdispose、バックプレッシャーなども考慮することを忘れないようにしましょう。
<br >

~~~ java
//コード
Observable<String> ob = Observable.create(
    (ObservableEmitter<String> em) -> {
    em.onNext("k");
    em.onNext("s");
    em.onNext("j");
    em.onComplete();
});

ob.subscribe(System.out::println);
~~~

~~~ java
//結果
k
s
j
~~~


__FromXXXX__{: style="color: #1b557a"} <br >

_fromArray_{: style="color: #e26716"} - 配列を処理するとき、使用する関数<br >

~~~ java
//コード
String[] arr = {"k","s","j"};
Observable<String> ob = Observable.fromArray(arr);
ob.subscribe(System.out::println);
~~~

~~~ java
//結果
k
s
j
~~~

<strong>int[] arrayを処理するときは fromArray(toIntegerArray(int[]))を使う</strong>

_fromIterable_{: style="color: #e26716"} - イテレーターパターンを実装したもので、データがあるかどうかとデータの値だけを取得します。使用するインターフェースは[List, Set, BlockingQueue]があり、Mapに関しては{key, value}の準備はされていません。


_fromCallable_{: style="color: #e26716"} - Java 5で追加された並行性APIであるCallableインターフェースを使用して、非同期実行後に返却するcall()メソッドを定義します。<br >

~~~ java
//원형
public interface Callable<V>{
    V call() throw Exception;
}
~~~

~~~ java
//コード
Callable<String> callable = () -> {
    return "Hello Java";
}

Observable<String> ob = Observable.fromCallable(callable);
ob.subscribe(System.out::println);
~~~

~~~ java
//結果
Hello Java
~~~

_fromFuture_{: style="color: #e26716"} - Java 5で追加された並行性APIで、非同期計算の結果を取得する際に使用します。通常、Executorインターフェースを実装したクラスにCallableオブジェクトを引数として渡し、Futureオブジェクトを返却します。get()メソッドを呼び出すと、Callableオブジェクトで実装された計算結果が出るまで待機します。

参考までに、Executorsクラスは単一スレッドの実行者だけでなく、さまざまなスレッドプールをサポートしています。しかし、RxJavaのSchedulersを使用することを推奨します。


~~~ java
//コード
Future<String> future = Executors.newSingleThreadExecutor().submit(() ->{
    return "Hello Java";
});
Observable<String> ob = Observable.fromFuture(future);
ob.subscribe(System.out::println);
~~~

~~~ java
//結果
Hello Java
~~~

<br >

<!-- _fromPublisher_{: style="color: #e26716"} - Publisher는 자바9의 표준인 Flow API의 일부입니다. 
아직 정식 오픈이 되지 않았기 때문에 크게 설명하기 보단, create()와 같이 구현시 개발자가 직접 onNext, onComplete를 
호출해야 한다는것만 알고가면 좋겠다.  -->

<br >

__Class__{: style="color: #1b557a"} <br >

_Single_{: style="color: #e26716"} - このクラスは1つのデータのみを発行し、通常、結果が一意なサーバーAPIを呼び出すときに使用されます。

<br />

ライフサイクルはonNextやonCompleteの代わりにonSuccessを使用し、onErrorは同じです。 [onSuccess(T value)、onError]

<br />
_Maybe_{: style="color: #e26716"} - RxJava2で導入されたObservableのもう一つの形態です。Singleクラスと同様に、最大で1つのデータしか持てませんが、データを発行せずにそのままデータ発行の完了を通知することもできます。<br >
<br />

MaybeオブジェクトはMaybeクラスを使用して生成できますが、通常はObservableの特定の演算子を介して生成されることが多いです。そのほか、Maybeオブジェクトを生成できるリアクティブ演算子には、elementAt()、firstElement()、flatMapMaybe()、lastElement()、reduce()、singleElement()関数などがあります。
<br >

ライフサイクルはonSuccess、onComplete、onErrorを使用します。

_Subject_{: style="color: #e26716"} - Observableの属性と購読者の属性を両方持っているという利点があります。<br >


AsyncSubject_{: style="font-size: 14px"} : Observable.subscribe()で発行された最後のデータのみを取得する際に使用します。  <br >

BehaviorSubject : 購読すると、最も最近の値またはデフォルト値を返すクラスです。データを初めて呼び出す場合、開発者が指定したデフォルト値を取得できます。 <br >

~~~ java
//コード
BehavioSubject<String> subject = BehavioSubject.createDefault("RxJava");
subject.subscribe(data -> System.out.println("#1 : " + data));
subject.onNext("k");
subject.onNext("s");
subject.subscribe(data -> System.out.println("#2 : " + data));
subject.onNext("j");
subject.onComplete();
~~~

~~~ java
//結果
#1 : "RxJava"
#1 : "k"
#1 : "s"
#2 : "s"
#1 : "j"
#2 : "j"
~~~
SubjectをcreateDefaultで生成し、最初に呼び出したため、デフォルト値である「RxJava」が返されます。
<br/>

<!-- PublishSubject : 이 Subject는 시간에 영향을 받습니다. 호출시 Subject에서 구독자에게 데이터를 발행중이라면,
발행중에 데이터를 구독자에게 전달해줍니다. <br >

~~~ java
//コード
PublishSubject<String> subject = PublishSubject.create();
subject.subscribe(data -> System.out.println("#1 : " + data));
subject.onNext("k");
subject.onNext("s");
subject.subscribe(data -> System.out.println("#2 : " + data));
subject.onNext("j");
subject.onComplete();
~~~

~~~ java
//結果
#1 : "k"
#1 : "s"
#2 : "j"
#1 : "j"
~~~ -->

<!-- (1)구독자는 그대로 흐르고 중간에 구독을 시작한 (2)는 그 때의 발행한 데이터를 전달받습니다.

ReplaySubject : 다른 Subject속성과 달리 발행한 전체데이터를 가지고 있다가, 추가로 구독한 구독자에게 전체 데이터를 전달해줍니다. 전체 데이터를 새로운 구독자에서 받을 수 있는것은 큰 장점이기도 하나, 메모리 누수의 문제가 있기때문에 많은 사용은 지향 하는 것이 좋을 것 같습니다.<br >

_ConnectableObservable_{: style="color: #e26716"} - 데이터 하나를 여러 구독자에게 전달 할때 사용합니다. 다른 클래스와 다르게 subscribe()으로 데이터를 발행하는것이 아닌 connect() 함수를 호출해야만
데이터가 발행됩니다. 생성 할때는 create()함수가 아닌 publish()함수로 선언합니다. <br > -->



| rxjava 1.x | rxjava 2.x|    | 種類 | リスト | 
|:--------|:-------:|:-------:|:-------:|:-------:|
| Observable  | Observable  | | 基本 | just(),subscribe(),dispose(),create() | 
|----
|  x  | Maybe  |              | FromXXX() | fromArray, fromIterable, fromCallable,<br/> fromFuture, fromPublisher
 | 
|----
|  x  | Flowable  |           | class| Single, Maybe, Subject, ConnectableObservable| 
|----
{: rules="groups"}


<hr/>

購読者にデータを伝達する際に使用する3つの関数<br/>
_onNext_{: style="color: #e26716"} : データを発行する際に使用されます。<br/>
_onComplete_{: style="color: #e26716"} : すべてのデータの発行が完了したときに呼び出され、ここでdispose()が実行されます。<br/>
_onError_{: style="color: #e26716"} : エラーが発生したときに呼び出され、エラーが発生した場合、onNextやonCompleteは呼び出されないため、dispose()を別途実行する必要があります。これはメモリリークを防ぐためです。<br/>

<br/>

<!-- Hot Observable & Cold Observable? <br/>
_Hot Observable_{: style="color: #e26716"} :  Hot영역은 마우스 이벤트, 키보드 이벤트, 시스템 이벤트, 센서 데이터등으로 표현 한다.<br/>

Hot에서 주의해야 할 것은 Back Pressure로 데이터를 발행하는 속도와 구독자가 처리하는 속도의 차이가 클때 발생하기 때문에 Flowable을 유심히 공부해야 한다. 

_Cold Observable_{: style="color: #e26716"} : Cold영역은 웹 요청, 데이터베이스 쿼리,  파일 읽기 등으로 표현 한다. <br/>

_ColdToHot 전환_{: style="color: #e26726"} : Subject객체를 만들거나 ConnectableObservable를 활용 하는 것입니다. 이부분은 Flowable 내용을 다룰때 다시 다룰 생각입니다.  -->

<br/>

