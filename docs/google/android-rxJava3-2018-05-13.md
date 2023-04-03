---
layout: default
title: Hello RxJava 3
parent: Android
grand_parent: Google
nav_order: 4
---


소개
---



이 페이지는 rxJava 2 버전을기반으로 설명하며, Scheduler를 공부하기 위해 정리했습니다. 
<br/>
<br/>
<br/>

사용 설명
<hr/>

__기본__{: style="color: #1b557a"} <br >

_newThread_{: style="color: #e26716"} -  구독자가 추가 될때 마다 스레드를 새로 생성한다는 의미를 갖습니다. 
<br />


_single_{: style="color: #e26716"} - 단일 스레드를 별도로 생성하여 구독작업합니다. 여러번 구독 요청이 와도 단일 스레드를 공통으로 사용하게 됩니다. <br >

_computation_{: style="color: #e26716"} - 일반적인 계산 작업을 할때 사용하는 스케줄러 입니다. interval()함수는 기본적으로 computation스케줄에서 돌아갑니다. 
CPU에 대응하여 계산하는 스케줄러입니다. io작업은 수행할수 없고, 프로세스 수만큼 스레드 풀을 증가 할수 있습니다. 
~~~ java
//코드
public static Observable<Long> interval(long perid, TimeUnit unit){
    return interval(perid, period, unit, Scheduler.computation());
}
~~~
<br/>

_io_{: style="color: #e26716"} - 네트워크상의 요청, 파일 입출력, DB쿼리등을 처리 할때 사용하는 스케줄러 입니다.  
<br >

_trampoline_{: style="color: #e26716"} -  새로운 스레드를 생성하지 않고 현재 스레드에서 무한한 크기의 대기행령 큐를 생성하는 스케줄러입니다. 새로운 스레드를 생성하지 않는다는 것과 대기 행령을 자동으로 만들어 준다는 것이 뉴 스레드 스케줄러, 계산 스케줄러, IO스케줄러와 다릅니다. 

<hr/>

_subscribeOn_{: style="color: #e26716"} - subscribe() 데이터 발행할때 처리하는 스레드를 지정해줍니다.  <br >
_observeOn_{: style="color: #e26716"} - Observable에서 데이터를 처리 할때 사용 되는 스레드를 지정해줍니다.<br >