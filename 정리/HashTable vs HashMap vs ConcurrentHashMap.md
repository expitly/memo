### HashTable vs HashMap vs CuncurrentHashMap

HashTable과 HashMap을 먼저 보면 HashTable은 더 이상 업데이트 되지않는다. 하위 버전의 호환성을 위해 존재 하고, HashMap은 자바 version이 올라가면서 계속 업데이트가 되고있다. 또한, 가장 큰 차이점은 동기화 여부이다. Source를 열어서 확인해보면 HashTable에는 synchronized가 구현되어 있는 것을 알 수 있다. 즉, HashTable은 MultiThread 환경에서 Thread safety 할 것이고, HashMap은 ThreadSafety 하지 않을 것 이다. 동기화에는 꽤나 큰 비용이 발생하기 때문에 불필요하게 동기화 처리가되면 불필요한 오버헤드가 발생 할 수 있으므로, 항상 ThreadSafety 되는게 좋은건 아니라 상황에 맞는 적절한 처리가 되어야 하는 것이다.

동기화 처리가 되어야 할 경우, HashTable을 사용하는 것 보다 SynchronizedMap이나 CuncurrentMap을 사용하는 것이 더 효율적이다. CuncurrentHashMap을 보면, 일단 이름에서 Cuncurrent가 들어간다. 뭔가 동시성 처리가 되어있을 것 같다. 내부를 살펴보면, CuncurrentHashMap은 부분으로 나누어서 부분부분을 lock 처리한다. HashTable은 전체를 동기화 하게 되는데, 즉, CuncurrentHashMap을 사용하면 lock 되지 않는 부분에 동시에 삽입/삭제 등 처리가 가능 하므로 HashTable보다 더 효율적이다. Java7버전에서 CuncurrentHashMap 소스를 열어보았을 때,  *ReentrantLock* 을 사용하였던 기억이있는데, Java8을 보니 synchronized를 사용한다. 

자꾸만 변한다

아ㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏㅏ