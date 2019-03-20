### HATEOAS(Hypermedia As The Engine Of Application State)

- Hypermedia : 하나의 컨텐츠가 텍스트, 이미지 등 다양한 컨텐츠를 **Link** 하는 개념
- HATEOAS는 로이필딩이 쓴 REST 관한 논문의 Uniform Interface의 주요 부분 중 하나
- 즉, Http Requeset에 대한 Response에 link에 대한 정보를 제공해줌으로써, Pesponse를 통해 다른 Request를 연결-연결-연결 해서 요청 가능함
  예를들어, 서버에서 "/posts" POST 이라는 요청이 "/post" POST으로 변경되었다고 가정 할 경우, Link를 통해 정보를 제공해주면 클라이언트에서는 소스 변경 없이 정보 제공이 가능함.
- 이를 통한 목적은 Client와 Server의 DeCoupling
- Spring에서는 spring-hateoas를 제공하여 쉽게 REST의 HATEOAS라는 개념을 적용 할 수 있도록 제공하고있음



##### 참고

https://en.wikipedia.org/wiki/HATEOAS

https://spring.io/guides/gs/rest-hateoas

https://www.baeldung.com/spring-hateoas-tutorial



