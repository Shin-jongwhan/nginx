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
### 먼저 windows라면 포트포워딩을 설정해야 한다.
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


