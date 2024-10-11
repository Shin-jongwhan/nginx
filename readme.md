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
### <br/><br/><br/>

## log
### 접속 로그 (Access Log)
#### /var/log/nginx/access.log
### <br/>

### 에러 로그 (Error Log)
#### /var/log/nginx/error.log
### <br/>

### 이 경로는 Nginx 설정 파일(/etc/nginx/nginx.conf 또는 개별 사이트 설정 파일)에서 access_log와 error_log를 통해 변경할 수 있다.
### docker로 되어있을 경우 로그 경로가 container 안쪽에 있어 container가 죽으면 log를 볼 수 없다.
### 그래서 nginx.conf 파일에서 로그 경로를 local 경로로 수정한 후 사용하는 편이 좋다.
### <br/><br/><br/>

## web server config 설정
### /etc/nginx/sites-available/default에서 수정할 수 있다.
### 여기서 어떤 포트로 접속했을 때 자동으로 포워딩해줄지 설정할 수 있다.
### <br/>

### 예를 들어 다음과 같이 되어 있으면 동작하는 방식은 다음과 같다.
1. xxx.com으로 브라우저에 써서 접속한다. :80 포트는 자동으로 http로 인식하여 안 써도 된다.
2. docker container 안에 있는 웹 서버 포트가 8088인 웹 서버로 포트포워딩이 된다.
   - 여기서 http://172.17.0.1은 docker container 안에서 실행된 것을 나타낸다.
   - local이면 http://172.17.0.1 또는 http://localhost를 적는다.
```
server {
    listen 80;
    server_name xxx.com;

    location / {
        proxy_pass http://172.17.0.1:8088;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
### <br/>

###  만약 https 설정이 되어 있다면 443 포트로 하면 된다.
```
server {
    listen 443;
    server_name xxx.com;

    location / {
        proxy_pass https://172.17.0.1:8088;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
### <br/><br/><br/>
