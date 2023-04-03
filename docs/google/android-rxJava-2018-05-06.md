---
layout: default
title: Hello RxJava 1
parent: Android
grand_parent: Google
nav_order: 2
---

# RxJava [[http://reactivex.io](http://reactivex.io)]

소개
<hr/>
최근에 트랜드로 떠오르고 있는 rx를 공부하려고보니 개념정리는 어느정도 되지만, 다른 사람들에게 
설명하려 하면 확신히 서지 않아서, 구글링하면 다 나오겠지만 전체정의를 한번 하려고 합니다. 
너무 많은정보가 있더라도 양해부탁드립니다. 만약, 제가 정리중에 잘못된부분이 있으면 커맨트 부탁 드립니다.

### 나온 배경
<hr/>
자바는 동시성 처리를 하는데 번거로움이 많기 때문에 이를 해결하기 위해 넷플릭스에서 RxJava를 개발, 클라이언트 요청을 
처리할 때 다수의 비동기 실행 흐름(스레드 등)을 생성하고 그것의 결과를 취합하여 최종 리턴하는 방식으로 내부방식에서 변경할수 있도록 개발 

### 개선점
+ 동시성을 적극적으로 끌어안을 필요가 있다 (Embrace Concurrency)
+ 자바 Future를 조합하기 어렵다는 점을 해결한다. (Java Futures are Expensive to Compose.)
+ 콜백 방식의 문제점을 개선해야 한다 (Callback Have Their Own Problems.)

> ##### - 동시성과 Future [이해참조](http://hamait.tistory.com/748)
<hr/>
<br/><br/>



기본
<hr/>

~~~ java
//코드
dependencies{
    compile : io.reactivex.rxjava2:rxjava:2.x.y
}
//코드
import io.reactivex.*;
~~~

<br/>

#### Observable
기본
> just(),subscribe(),dispose(),create()

FromXXX()
> fromArray, fromIterable, fromCallable, fromFuture, fromPublisher

Class
> Single, Maybe, Subject, ConnectableObservable

##### rxjava 1.x > just, create, from만 있다. 

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

###### rxjava 1.x 과 rxjava 2.x 클래스 비교
<hr/>
<br/>

#### RxJava 연산자 <br/>
 기본 함수 : map(),filter(),reduce(),flatmap() <br/>
 생성 함수 : interval, timer, range, intervablRange, defer, repeat<br/>
 변환 함수 : concatMap, swithMap, groupBy, scan<br/>
 결합 함수 : zip, combineLatest, merge, concat<br/>
 조건 함수 : amb, takeUntil, skipUntil<br/> 
 수학 및 기타연산 함수 : delay, timeInterval <br/>
 
 
<hr/>
<br/>

#### Schedulers 

| Schedulers |rxjava 1.x | rxjava 2.x| 
|:--------|:-------:|:-------:|
| 뉴스레드 스케줄러  | .newThread()  |.newThread() |
|----
| 싱글 스레드 스케줄러  | 지원안함  | .single()|
|----
| 계산 스케줄러   | .computation()  |.computation()  |
|----
|  IO 스케줄러  | .io()  |.io() |
|----
|  트램펄린 스케줄러  | .trampoline()  | .trampoline()|
|----
|  메인 스레드 스케줄러  | .immediate()  | 지원안함 |
|----
|  테스트 스레드 스케줄러  | .test()  | 지원안함 |
|----
{: rules="groups"}

~~~ java
String[] strObj = {"K-1","S-2","J-3"};
Observable<String> source = Observalbe.fromArray(strObj)
.doOnNext(data - > Log.v(" log data["+data+"]"))
.subscribeOn( Schedulers.newThread())
.observeOn( Schedulers.newThread())
.map(Shape::flip);
source.subscribe(Log::i);

~~~
subscribeOn()함수는 Observable를 생성후 실행 될때 사용할 스레드를 지정하는 함수.
observeOn()함수는 Observable에서 생성된 data가 처리되는 전체를 돌릴 스레드를 지정 하는 함수.
##### ~ subscribeOn() 과 observeOn() 스레드는 확실히 분리하는것이 좋다(둘다 선언 해주도록 하자)