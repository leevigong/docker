**컨테이너와 호스트 간에 파일 복사 실습**  
: 호스트의 html 파일을 아파치 컨테이너 내부로 복사하여 초기화면 변경

#### 1. 디렉토리 및 html 파일 생성
```bash
# 현재 디렉토리 아래 'example_html' 디렉토리 생성
mkdir example_html

# example_html 디렉토리에 새로운 HTML 파일 생성
nano ./example_html/index.html
```
```html
# html 파일 내용
<html>
    <meta charset="utf-8"/>
    <body>
        <div> 안녕하세요! </div>
    </body>
</html>
```

#### 2. 아파치 컨테이너 생성
`docker run --name apa000 -d -p 8089:80 httpd`  
웹브라우저를 통해 http://localhost:8089 접속  
<img width="268" alt="image" src="https://github.com/user-attachments/assets/26545a3d-1fb6-4ad0-8e1e-59fe1b9dd256">

#### 3. 호스트에서 컨테이너로 index.html 파일 복사
'./example_html/index.html' 파일을 아파치 컨테이너인 apa000의 '/usr/local/apache2/htdocs/' 경로로 복사  
** 아파치 컨테이너의 경로는 아파치 서버가 웹 브라우저에 제공하는 웹 루트 디렉토리  
`docker cp ./example_html/index.html apache000:/usr/local/apache2/htdocs`  
<img width="268" alt="image" src="https://github.com/user-attachments/assets/aa3021d2-afd9-470e-a135-4ba4b7433f9b">
