**컨테이너에서 이미지 추출해 이미지 생성 실습**

#### 1. 아파치 컨테이너 생성
`docker run --name apa -d -p 8092:80 httpd`  

#### 2. 컨테이너에서 현재 실행 중인 상태를 그래도 새로운 이미지로 생성
`docker commit apa new_apa`

#### 3. 이미지 목록 확인 
`docker images`  
<img width="518" alt="image" src="https://github.com/user-attachments/assets/1724776e-62f9-4e88-aa3d-4a8fe7ddb5cf">

---
**도커 파일(Dockerfile)로 이미지 생성 실습**

#### 1. apa_folder 디렉토리에 Dockerfile 생성
`nano ./apa_folder/dockerfile`  
```bash
# 사용할 베이스 이미지 지정
FROM httpd

# 재료 디렉토리의 index.html 파일을 이미지 내 /usr/local/apache2/htdocs/ 디렉토리로 복사 -> 이거때문에 "안녕하세요!"로 보여짐
COPY index.html /usr/local/apache2/htdocs/

# /tmp 디렉토리를 생성하고 권한을 설정합니다.
RUN mkdir -p /tmp && chmod 1777 /tmp

# 패키지 관리자를 업데이트하고 패키지를 설치합니다.
RUN apt-get update && apt-get install -y apt-utils
```
#### 2. Dockerfile로 이미지 생성
`docker build -t dockerfile_image ./apa_folder`  

#### 3. 이미지 목록 확인 
`docker images`  
<img width="541" alt="image" src="https://github.com/user-attachments/assets/8be98f65-8197-43e1-b17a-6268972992da">

#### 4. 생성 이미지로 컨테이너 생성
`docker run --name dockerfile -d -p 8099:80 dockerfile_image`  
웹브라우저를 통해 http://localhost:8099 접속하여 업데이트된 초기 화면을 확인  
<img width="277" alt="image" src="https://github.com/user-attachments/assets/c939c93e-5eaa-43c2-b4ba-7e5452a1a5da">
