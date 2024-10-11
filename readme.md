### 241011
## nginx
### 자세한 개념들과 내용은 아래 블로그 참고
#### https://blog.naver.com/gi_balja/223028077537
#### https://narup.tistory.com/238
#### https://velog.io/@chickenfondue/nginx-docker-compose%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A6%AC%EB%B2%84%EC%8A%A4-%ED%94%84%EB%A1%9D%EC%8B%9C-%EA%B5%AC%EC%84%B1
### <br/><br/><br/>

## 웹 서비스 workflow
### client <-> web server <-> WAS (Web Application Server) <-> DB로 이루어진다.
#### ![image](https://github.com/user-attachments/assets/01d83503-cb63-4a1c-a83e-cf1d5ea99ebf)
### <br/><br/><br/>

## nginx란?
### web server를 말한다. 웹 서버는 클라이언트 요청을 받아서 WAS (Web Application Server)에서 보내준 html, css, javascript를 클라이언트 쪽으로 보내준다.
### <br/><br/><br/>

## nginx 사용하는 이유
### 나는 리버스 프록시 기능이 필요해서 사용하려고 한다.
### 한 서버에서 운영 서버와 dev 서버를 같이 관리하고 있는데 80포트가 하나 뿐인데 도메인에 따라서 포트포워딩 되게끔 만들고 싶었다.
#### ![image](https://github.com/user-attachments/assets/6cb881de-8197-4055-bf21-7d5a07dc89ab)
