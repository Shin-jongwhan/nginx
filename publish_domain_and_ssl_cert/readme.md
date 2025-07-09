# 주소 이동됨
#### https://github.com/Shin-jongwhan/network/tree/main/publish_domain_and_ssl_cert

### 250515
## SSL 인증서 발급하기
### 나는 여기서 하였다.
#### https://www.sslcert.co.kr/
#### ![image](https://github.com/user-attachments/assets/2fd915e0-ba4d-4c16-a9b0-10c8cab91c3c)
### <br/>

### 구매하고 나면 DVC 인증을 받아야 한다. 
#### 도메인 권한 유효성 검증 (DCV) 이란?
- 인증서의 부정,위조 발급을 방지하기 위해서, 도메인에 대해 유효한 권한이 있는지 확인하는 필수 절차 입니다. 발급완료를 위해서 아래 나열된 각 도메인별 인증 절차 수행 완료 또는 고객 서버측의 추가 설정이 필요합니다. (신청 도메인이, 피싱 의심 또는 특정 브랜드 가치 훼손 가능성이 있는 경우에, CA 의 별도 추가 심사 지연 가능)
### 이때 인증 방법으로 아래 4가지가 있는데, 나는 HTTP로 하였다. 다른건 도메인이 이미 있거나 다른 certificate가 있어야 한다.
- HTTP
- HTTPS
- DNS
- email
#### ![image](https://github.com/user-attachments/assets/5958aa00-23bb-4379-9af2-2f8d82cf1480)
#### ![image](https://github.com/user-attachments/assets/ae0bcc67-8a07-40ff-9110-be72fa428d22)
### <br/>

### DVC 검증을 받기 위한 파일이 하나 주어지는데, 이를 다운로드해둔다. 그리고 아래 조건에 맞게 HTTP 도메인 접속을 설정해줘야 한다.
#### ![image](https://github.com/user-attachments/assets/992a5991-ac7d-4457-a1f9-15ad15ad114e)
```
HTTP/s 파일 URL ➜ //(인증도메인)/.well-known/pki-validation/(txt파일)
HTTP/s 파일명 ➜ 62BB07440AD17DB87CF701144CCA67B0.txt 
HTTP/s 파일내용 :
BC0E8A90D83C3ABD92A651D5498421C47A510BF9041C18B40513FB00609381C4
sectigo.com
dcv20250515ae1db
```
### <br/><br/>

## domain 구입 및 record 설정
### 그리고 구매한 도메인 사이트로 가서 관리 페이지에서 A record를 추가한다.
### 이렇게 하면 cognimosyne.com과 www.cognimosyne.com 둘 다 연결되게 하기 위해 다음 2가지를 추가해야 한다.
#### 참고로 구매한 도메인 이름이 들어가는 게 아니다.
#### 1 (cognimosyne.com)
- 유형 : A record
- 호스트 이름 : @
- 값(주소) : public IP
#### 2 (www.cognimosyne.com)
- 유형 : A record
- 호스트 이름 : www
- 값(주소) : public IP
### 설정 후 바로 적용이 되는 게 아니라 DNS server에 전파해서 적용이 될 때까지 1~30분 정도 걸린다.
#### ![image](https://github.com/user-attachments/assets/6ced1c59-d7e7-43c7-add0-1fd52ad086b0)
### <br/><br/>

## 포트포워딩 및 방화벽 설정
### 먼저 windows라면 포트포워딩을 설정해야 한다. 리눅스
### 공유기 관리 홈페이지 가서 한다. 나는 KT 공유기 꺼라서 트래픽 관리 페이지에 가면 할 수 있었다.
#### ![image](https://github.com/user-attachments/assets/02b32ec0-13d0-4617-b50b-1cc50f29e900)
### <br/>

### 그리고 inbound 방화벽을 허용해야 한다.
### 선택에 따라 다르지만 TCP만 허용해도 된다. 나는 그냥 TCP, UDP 둘 다 했다.
#### ![image](https://github.com/user-attachments/assets/e99718cb-5e5d-4423-a36f-72e1124d8776)
#### ![image](https://github.com/user-attachments/assets/43e613b7-040a-44b2-8ef6-eda2af85e624)
#### ![image](https://github.com/user-attachments/assets/0f31ffaf-0600-48e4-914e-2073d9f0f6fd)
#### ![image](https://github.com/user-attachments/assets/3d644ab0-164a-4be7-96c2-e8e7a6c4acb0)
#### ![image](https://github.com/user-attachments/assets/a9e47ac1-b4f8-4ea3-a00d-efa86758d4ad)
#### ![image](https://github.com/user-attachments/assets/de6c6819-12aa-4a35-8d47-4d89648f2a79)
#### ![image](https://github.com/user-attachments/assets/d4baa954-ab5c-49a6-af46-6357b98259c5)
### <br/>

## nginx (web server) 및 web application 테스트
### 나는 이미 세팅한 nginx docker image가 있어서 그걸 사용했다. HTTP 접속을 위해서 80 포트를 연결한다.
```
run -itd -v C:\docker_volume\nginx:/data -p 80:80 --name nginx shinejh0528/nginx:1.1.0
```
### 사용 방법 참고
#### https://hub.docker.com/r/shinejh0528/nginx
### <br/>

### nginx sites-available 수정
```
vi /etc/nginx/sites-available/default
```
#### <br/>

### 다음과 같이 작성
```
server {
    listen 80;
    server_name cognimosyne.com www.cognimosyne.com;

    location / {
        proxy_pass http://[IPv4 address];
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
#### <br/>

### nginx 재시작
```
service nginx restart
# 또는 start 안 되어 있으면
service nginx start
```
### <br/>

### web application 테스트로 하나 구축한다. 나는 python으로 하였다.
```
from flask import Flask, send_from_directory
import os

app = Flask(__name__)

@app.route('/.well-known/pki-validation/<filename>')
def serve_validation_file(filename):
    return send_from_directory('.well-known/pki-validation', filename)

@app.route('/')
def home():
    return "Hello from Flask - cognimosyne.com!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80)  # 또는 포트포워딩으로 공인 80포트 연결
```
#### <br/>

### 그 다음 DVC 인증을 받기 위해 다운로드한 파일을 스크립트와 같은 경로에 저장한다.
- ./.well-known/pki-validation/62BB07440AD17DB87CF701144CCA67B0.txt
#### ![image](https://github.com/user-attachments/assets/e32e2191-337a-4cbc-9202-b4c48d60dca1)
#### <br/>

### web app 시작
```
python .\DCV_validation.py
```
#### ![image](https://github.com/user-attachments/assets/4959798e-cfbc-448a-be15-ac4e39af0427)
### <br/>

### 접속 테스트
### 핸드폰에서도 잘 접속된다.
#### http://cognimosyne.com/
#### ![image](https://github.com/user-attachments/assets/d4553b40-05ef-455f-9787-98a29bfb5b3c)
#### http://cognimosyne.com/.well-known/pki-validation/62BB07440AD17DB87CF701144CCA67B0.txt
#### ![image](https://github.com/user-attachments/assets/a63327bd-22cb-442e-9570-c235db73706f)
#### <br/>

### DVC 인증 테스트
### 아래 사이트에서 다시 돌아와서 인증 테스트를 한다.
#### https://www.sslcert.co.kr/
#### ![image](https://github.com/user-attachments/assets/36ed63c2-6dc9-4e61-9128-a9f996622cd7)
#### ![image](https://github.com/user-attachments/assets/a597965c-27f0-4aad-84ee-7b996af09f6e)
#### ![image](https://github.com/user-attachments/assets/0209c541-2161-4462-87d9-2b975eba07ac)
### <br/>

### 여기까지 진행하면 2일 내로 발급이 다 된다고 하는데, 검증 요청 버튼을 누르면 바로 된다.
#### ![image](https://github.com/user-attachments/assets/aaa7c0cb-30f0-44e6-8384-a62f02442d10)
### 발급되면 HTTPS 적용해서 web 개발하면 된다. 나는 AWS로 배포해볼까 생각도 해본다.
#### ![image](https://github.com/user-attachments/assets/b50a8a3e-36be-4ae9-93cc-05d1591b169f)
#### ![image](https://github.com/user-attachments/assets/33dffbb1-3397-46e1-950e-3bc1f2e24682)

### <br/><br/>

## 로컬에서 도메인 주소 접속이 안 될 때
### windows에서는 hosts 파일 설정을 해줘야 한다.
#### C:\Windows\System32\drivers\etc\hosts 파일을 관리자 권한으로 열고 다음을 추가한다.
```
[IPv4 address] cognimosyne.com www.cognimosyne.com 
```
### <br/>

### 그러면 로컬에서도 도메인으로 접속할 수 있다.
#### ![image](https://github.com/user-attachments/assets/187d6831-a9f5-4e85-8fb2-925711400266)
### <br/><br/>

## 보안 관련 참고사항
### 다양한 국가에서 간헐적으로 자주 접속 시도를 하는데, 보안 관련해서 정말 잘 설정해야겠다는 생각을 하게 된다.
#### ![image](https://github.com/user-attachments/assets/82331fb9-80b9-425a-aa62-7711bbd25378)
#### ![image](https://github.com/user-attachments/assets/98d183c4-80e3-421e-9c94-9adc970045d1)

