Publisher
====
아이템의 발행자.
이 메시지들은 하나의 흐름에 실려서 나가고, 같은 아이템이 같은 순서로 나가는 것을 보장

Subscriber
====
메시지의 구독자. 메시지들을 정상적으로 수신하면, OnNext 함수로 하나하나 볼 수 있고,
publisher가 메시지를 정상 발행하지 못하는 경우, OnError가 호출된다.

-   Subscription<br>
publisher와 subscriber를 잇는 메시지 컨트롤을 한다. 
subscription의 request(갯수) 메소드를 사용하면 current에서 갯수만큼 메시지를 가져온다.


