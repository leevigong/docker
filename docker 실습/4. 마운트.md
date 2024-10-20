**바인드 마운트 실습**  
호스트 머신에 디렉토리 생성 후 컨테이너의 내부 경로와 마운트 및 초기화면 변경

#### 1. 홈 디렉토리 아래 마운트 할 디렉토리 생성
`mkdir apa_folder`

#### 2. 디렉토리와 마운트하는 아파치 컨테이너 생성  
'./apa_folder' 디렉토리를 아파치 컨테이너의 '/usr/local/apache2/htdocs' 경로로 연결  
`docker run --name apa000 -d -p 8090:80 -v ./apa_folder:/usr/local/apache2/htdocs httpd`  
웹브라우저를 통해 http://localhost:8090 접속  
<img width="267" alt="image" src="https://github.com/user-attachments/assets/f8213227-da1c-47ec-856a-355ce220040a">  
폴더가 마운트 되면 초기 화면에  Index of/ (해당 폴더 내의 파일 및 디렉토리 목록)

#### 3. 마운트 된 디렉토리 경로에 HTML 파일을 넣어 초기 화면을 실시간으로 변경
~/apa_folder' 디렉토리에 새로운 HTML 파일 생성  
`nano ./apa_folder/inedx.html` 
```html
# html 파일 내용
<html>
    <meta charset="utf-8"/>
    <body>
        <div> 안녕하세요! </div>
    </body>
</html>
```
웹브라우저를 통해 http://localhost:8090 접속  
<img width="287" alt="image" src="https://github.com/user-attachments/assets/192bd873-d943-4f52-8e99-a2f4b2c10aa2">  
업데이트된 초기화면을 볼 수 있음

---
**볼륨 마운트 실습**
#### 1. 볼륨 생성
`docker volume create apa001vol`

#### 2. 볼륨을 마운트하는 아파치 컨테이너 생성
```bash
#볼륨을 마운트하는 아파치 컨테이너를 생성
docker run --name apa001 -d -p 8091:80 -v apa001vol:/usr/local/apache2/htdocs httpd

# 컨테이너 내 파일 권한 변경
docker exec -it apa001 /bin/bash -c "chown -R $(id -u):$(id -g) /usr/local/apache2/htdocs"

# 컨테이너 중지 및 삭제
docker stop apa001
docker rm apa001

# 컨테이너 재생성
docker run --name apa001 -d -p 8091:80 -v apa001vol:/usr/local/apache2/htdocs --user $(id -u):$(id -g) httpd
```  
<img width="284" alt="image" src="https://github.com/user-attachments/assets/821caa6c-d8f3-4bbb-bf87-d101f70918fc">


#### 3. 볼륨 및 컨테이너 상세 정보 확인
볼륨 상세 정보 확인  
`docker volume inspect apa001vol`  
<img width="524" alt="image" src="https://github.com/user-attachments/assets/f28abdd2-0632-4535-9616-41ee3ddc0702">  
- Mountpint : 볼륨이 호스트 머신에서 마운트된 디렉터리 경로

컨테이너 상세 정보 확인  
`docker container inspect apa001`  
<img width="497" alt="image" src="https://github.com/user-attachments/assets/c91adbfe-6b7c-4bd7-83df-138e26c7c924">  
- Source: 볼륨 데이터의 소스 경료 = 호스트 머신의 파일 시스템 경로
- Destination: 컨테이너 내부에서 볼륨 데이터가 마운트될 경로

#### 4. 볼륨 경로에서 index.html 파일 변경
`nano /var/lib/docker/volumes/apa001vol/_data/index.html`

```html
# html 파일 내용
<html>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
```

=> 에러가났는지 index.html에 아무것도 없고 수정도 안되어서 오늘은 여기서 끝,,
