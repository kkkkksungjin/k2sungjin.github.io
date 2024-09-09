---
layout: default
title: Hello RxJava 2
parent: Android
grand_parent: Google
nav_order: 3
---

소개
<hr/>
이 페이지는 rxJava 2 버전을기반으로 설명하며, Observable를 공부하기 위해 정리했습니다. 
<br/>
<br/>
<br/>

사용 설명
<hr/>

__기본__{: style="color: #1b557a"} <br >

_just_{: style="color: #e26716"} - 데이터를 차례로 발행하는 함수, Observable을 생성하고, 한개의 값을 넣을 수 있고, 같은 타입의 인자로 최대 10개 까지 처리가능하다. 
<br />

~~~ java
//코드
Observable.just(1,2,3).subscribe(System.out::println);
~~~


~~~ java
//결과
1
2
3
~~~

<br/>

_subscribe_{: style="color: #e26716"} - 동작시키기 위해 준비해둔 observable을 실행할때 사용합니다.  <br >

~~~ java
//원형
Disposeable subscribe()
...
~~~

인자가 없는 subscribe() onNext, onComplete 이벤트를 무시하고 onError이벤트가 발생했을때만 throw를 던지기때문에 Observable 코드를 테스트하거나 디버깅 할때 용이합니다.

_dispose_{: style="color: #e26716"} - 데이터 발생을 해지 할때 사용. Observable과 구독자 관계를 끊을때 사용합니다. onComplete가 발생할때 dispose를 호출하기 때문에 따로 dispose를 호출할 필요가 없습니다.
<br/>

~~~ java
//코드
Observable<String> ob = Observable.just("k","s","j")
Disposable d = ob.subscribe(
    v -> System.out.println("onNext : "+ v),
    err -> System.out.println("onError"),
    () -> System.out.println("onComplete")
);
System.out.println("T/F : "+d.isDisposed);
~~~

~~~ java
//결과
onNext : k
onNext : s
onNext : j
onComplete
T/F : true
~~~

_create_{: style="color: #e26716"} -  onNext, onComplete, onError를 개발자가 직접 호출해야 한다. 
직접 호출해야 하기 때문에, onError 또는 dispose, back pressure등도 고려해야 한다는걸 명심하자.
<br >

~~~ java
//코드
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
//결과
k
s
j
~~~


__FromXXXX__{: style="color: #1b557a"} <br >

_fromArray_{: style="color: #e26716"} - 배열를 처리 할때 사용하는 함수<br >

~~~ java
//코드
String[] arr = {"k","s","j"};
Observable<String> ob = Observable.fromArray(arr);
ob.subscribe(System.out::println);
~~~

~~~ java
//결과
k
s
j
~~~

<strong>int[] array를 처리 할때는 fromArray(toIntegerArray(int[]))를 사용하자.</strong>

_fromIterable_{: style="color: #e26716"} - 이터레이터 패턴을 구현한 것으로 데이터가 있는지 와 데이터 값만 가져온다.<br >
사용 인터페이스는 [List, Set, BlockingQueue]가있고, Map-{key,value}에 관해서는 준비되어있지 않다. 


_fromCallable_{: style="color: #e26716"} - 자바5에서 추가된 동시성 API인 Callabel인터페이스로 비동기 실행 후 반환하는 call()를 정의한다.<br >

~~~ java
//원형
public interface Callable<V>{
    V call() throw Exception;
}
~~~

~~~ java
//코드
Callable<String> callable = () -> {
    return "Hello Java";
}

Observable<String> ob = Observable.fromCallable(callable);
ob.subscribe(System.out::println);
~~~

~~~ java
//결과
Hello Java
~~~

_fromFuture_{: style="color: #e26716"} - 자바5에서 추가된 동시성 API로 비동기 계산의 결과를 구할때 사용
보통 Executor 인터페이스를 구현한 클래스에 Callable객체를 인자로 넣어 Future객체를 반환, get()메소드를 호출하면 Callable객체에서 구현한 계산 결과가 나올때까지 기다리게 된다.

참고로, Executors Class는 단일스레드 실행자 뿐만 아니라 다양한 스레드풀을 지원한다. 하지만, RxJava의 Schedulers를 사용
하는것을 권장한다. 


~~~ java
//코드
Future<String> future = Executors.newSingleThreadExecutor().submit(() ->{
    return "Hello Java";
});
Observable<String> ob = Observable.fromFuture(future);
ob.subscribe(System.out::println);
~~~

~~~ java
//결과
Hello Java
~~~

<br >

_fromPublisher_{: style="color: #e26716"} - Publisher는 자바9의 표준인 Flow API의 일부입니다. 
아직 정식 오픈이 되지 않았기 때문에 크게 설명하기 보단, create()와 같이 구현시 개발자가 직접 onNext, onComplete를 
호출해야 한다는것만 알고가면 좋겠다. 

<br >

__Class__{: style="color: #1b557a"} <br >

_Single_{: style="color: #e26716"} - 이 클래스는 오직 1개의 데이터만을 발행, 보통 결과가 유일한 서버의 API를 호출할때 사용한다. 
<br />

라이프사이클은 onNext, onComplete대신 onSuccess를 사용하고 onError는 동일하다 [onSuccess(T value), onError ]

<br />
_Maybe_{: style="color: #e26716"} - RxJava2에서 도입된 Observable의 또다른 형태이다. Single클래스와 마찬가지로
최대 데이터 하나를 가질수 있지만 데이터 발행 없이 바로 테이터 발생 완료를 할수도 있다. <br >
<br />

Maybe 객체는 Maybe 클래스를 이용해 생성할 수 있지만 보통 Observable의 특정 연산자를 통해 생성할때가 많다. 
그외 Maybe객체를 생성할수 있는 리액트 연산자에는 elementAt(),firstElement(),flatMapMaybe(),lastElement(),reduce()
,singleElement() 함수등이 있다. 
<br >

라이프사이클은 onSuccess, onComplete, onError를 사용한다. 

_Subject_{: style="color: #e26716"} - Observable 속성과 구독자의 속성을 모두 가지고 있다는 장점이 있습니다.<br >


AsyncSubject_{: style="font-size: 14px"} : Observable.subscribe()에서 발행한 마지막 데이터만 가져올때 사용합니다.  <br >

BehaviorSubject : 구독을 하면 가장 최근 값을 혹은 기본값을 넘겨주는 클래스입니다. 데이터를 처음 호출할경우
개발자가 지정한 기본값을 받아 올수 있습니다. <br >

~~~ java
//코드
BehavioSubject<String> subject = BehavioSubject.createDefault("RxJava");
subject.subscribe(data -> System.out.println("#1 : " + data));
subject.onNext("k");
subject.onNext("s");
subject.subscribe(data -> System.out.println("#2 : " + data));
subject.onNext("j");
subject.onComplete();
~~~

~~~ java
//결과
#1 : "RxJava"
#1 : "k"
#1 : "s"
#2 : "s"
#1 : "j"
#2 : "j"
~~~
subject를 createDefault로 생성하고 처음 호출하였기에 Default값인 Rxjava를 반환해주게 됩니다. 
<br/>

PublishSubject : 이 Subject는 시간에 영향을 받습니다. 호출시 Subject에서 구독자에게 데이터를 발행중이라면,
발행중에 데이터를 구독자에게 전달해줍니다. <br >

~~~ java
//코드
PublishSubject<String> subject = PublishSubject.create();
subject.subscribe(data -> System.out.println("#1 : " + data));
subject.onNext("k");
subject.onNext("s");
subject.subscribe(data -> System.out.println("#2 : " + data));
subject.onNext("j");
subject.onComplete();
~~~

~~~ java
//결과
#1 : "k"
#1 : "s"
#2 : "j"
#1 : "j"
~~~

(1)구독자는 그대로 흐르고 중간에 구독을 시작한 (2)는 그 때의 발행한 데이터를 전달받습니다.

ReplaySubject : 다른 Subject속성과 달리 발행한 전체데이터를 가지고 있다가, 추가로 구독한 구독자에게 전체 데이터를 전달해줍니다. 전체 데이터를 새로운 구독자에서 받을 수 있는것은 큰 장점이기도 하나, 메모리 누수의 문제가 있기때문에 많은 사용은 지향 하는 것이 좋을 것 같습니다.<br >

_ConnectableObservable_{: style="color: #e26716"} - 데이터 하나를 여러 구독자에게 전달 할때 사용합니다. 다른 클래스와 다르게 subscribe()으로 데이터를 발행하는것이 아닌 connect() 함수를 호출해야만
데이터가 발행됩니다. 생성 할때는 create()함수가 아닌 publish()함수로 선언합니다. <br >



| rxjava 1.x | rxjava 2.x|    | 종류 | 리스트 | 
|:--------|:-------:|:-------:|:-------:|:-------:|
| Observable  | Observable  | | 기본 | just(),subscribe(),dispose(),create() | 
|----
|  x  | Maybe  |              | FromXXX() | fromArray, fromIterable, fromCallable,<br/> fromFuture, fromPublisher
 | 
|----
|  x  | Flowable  |           | class| Single, Maybe, Subject, ConnectableObservable| 
|----
{: rules="groups"}


<hr/>

구독자에게 전달 할때 사용하는 세가지 함수<br/>
_onNext_{: style="color: #e26716"} : 데이터 발행을 할때 사용<br/>
_onComplete_{: style="color: #e26716"} : 모든 데이터의 발행을 완료 했을때 발생하고, 이곳에서 dispose()가 실행됩니다. <br/>
_onError_{: style="color: #e26716"} : 에러가 생겼을때 호출되며, 에러가 생겼을경우는 onNext,onComplete를 호출하지 않기때문에 dispose()처를를 따로 해주어야합니다. 메모리누수를 방지하기 위해서 입니다.<br/>

<br/>

Hot Observable & Cold Observable? <br/>
_Hot Observable_{: style="color: #e26716"} :  Hot영역은 마우스 이벤트, 키보드 이벤트, 시스템 이벤트, 센서 데이터등으로 표현 한다.<br/>

Hot에서 주의해야 할 것은 Back Pressure로 데이터를 발행하는 속도와 구독자가 처리하는 속도의 차이가 클때 발생하기 때문에 Flowable을 유심히 공부해야 한다. 

_Cold Observable_{: style="color: #e26716"} : Cold영역은 웹 요청, 데이터베이스 쿼리,  파일 읽기 등으로 표현 한다. <br/>

_ColdToHot 전환_{: style="color: #e26726"} : Subject객체를 만들거나 ConnectableObservable를 활용 하는 것입니다. 이부분은 Flowable 내용을 다룰때 다시 다룰 생각입니다. 

<br/>
