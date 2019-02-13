문득 3 way handshaking 을 직접 보고 싶어서 wireshark를 통해 테스트 해보고 결과를 기록하였다.

Test 서버 구성은 간단하게 NGINX를 앞에 두고 reverse proxy를 통해 NodeJs를 바라보게 만들어두고 "Hello World"를 출력하도록 구성하였다.

![](C:\Users\JinYoung\Desktop\wireshark test\wiresharkTest.png)

결과는 위 화면과 같다. 빨간색으로 줄을 그은 부분은 내 서버 ip이므로 가려보았다.

결과는 이론상 아는 바와 같았다.

먼저 1~3줄을 보면, SYN과 ACK를 서로 주고 받으며 3way handshaking 과정이 나타난다. Source에서는 내 Client의 Public IP가 아닌 private IP가 나왔고, Destination은 서버의 public IP가 표출됨을 알 수 있다.

또한, INFO 부분을 보면 알 수 있듯이 client에서 임의의 포트(49158)을 만들고, 서버의 80 포트로 요청을 보냄을 알 수 있다. 

1~3줄의 3way handshaking과정이 끝나면 비로소 4번째 줄에 http 요청을 보낸다.

그럼 5번째 줄에 서버로부터 Request를 받았다는 TCP ACK 신호를 받고, 6번째줄에 4번째줄의 Response인 HTTP 응답을 받는다.

마지막으로 7번째 줄에서 클라이언트는 HTTP응답을 받았다는것을 서버에 알려주기위해 TCP ACK 신호를 보낸다.



그냥.. 눈으로 한 번쯤 보고 싶었다.

끝.





