이 글에서는 멀티 쓰레드 관점에서의 차이점을 알아볼 것이다.

### HashTable vs HashMap

일단 HashTable과 HashMap을 비교해보면 가장 큰 차이는 동기화 여부이다. 구현은 거의 비슷하다고 볼 수 있고, HashTable은 method에 ***synchronized***가 선언되어있어 멀티 쓰레드 환경에서 Thread Safety 하다. 반대로 HashMap은 동기화 처리가 되어있지 않기때문에 멀티 쓰레드 환경에서 Thread Safety 하지 않다.

> 그럼 HashTable이 더 좋을까??

