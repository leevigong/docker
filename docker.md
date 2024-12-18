# 도커(Docker) <img src="https://img.shields.io/badge/DOCKER-2496ED?style=flat&logo=Docker&logoColor=white"/>
- 컨테이너 개념을 도입해 데이터/프로그램을 격리시키는 기능을 제공하는 소프트웨어
- 컨테이너 환경을 손쉽게 만들 수 있는 기능, 각 컨테이너를 손쉽게 동작할 수 있는 도커 엔진 제공
- 애플리케이션을 위해 컨테이너로 패키징하고 배포하는 도구(Iaas, Paas에서 활용됨)
- 리눅스 운영체제에서 사용하는 것을 전제로 만들어 졌기 때문에 반드시 리눅스 운영체제가 필요! 컨테이너 안에서 동작하는 프로그램도 리눅스용 프로그램!<br><br>
주 용도) 동일한 개발 환경 제공, 새로운 버전의 테스트, 동일한 서버가 여러 대 필요한 경우<br>
장점) 한 대의 물리 서버에 여러 대의 서버를 띄움, 서버 관리가 용이, 다루기 쉬움<br>
단점) 호스트 서버에 문제가 생기면 모든 컨테이너에 영향을 미침

### Linux 컨테이너 vs VM
IT환경에서 격리된 패키징된 컴퓨팅환경을 제공
특징 | Linux 컨테이너 | VM
|---|---|---|
크기 및 패키징| Megabyte 단위 크기를 가지며, 애플리케이션과 그 실행에 필요한 최소한의 파일만 포함 | - GigaByte 단위 크기를 가지며, 자체 OS를 포함 <br> - 여러 리소스 집약적인 작업을 동시에 수행할 수 있으며, 전체 서버, OS, 데스크탑, DB, 네트워크 등을 추상화, 분할, 복제, 에뮬레이션함
이동성 및 확장성| 경량화 속성과 공유OS로 인해 여러 환경 간 쉽게 이동 가능하며 확장이 상대적으로 쉬움| VM 자체OS를 가지고 있어 이동성이 상대적으로 낮으며 확장을 위해 추가 리소스 필요함
리소스 분리| 호스트 OS커널을 공유하므로 리소스 분리가 상대적으로 덜 격리됨| 자체OS를 가지고 있어 더 격리된 환경을 제공하며, 리소스를 더욱 강력하게 분리할 수 있음
용도| 미세한 서비스 or 컨테이너 오케스트레이션 플랫폼(ex: docker, kubernetes)을 통해 애플리케이션 배포와 관리에 사용| 여러개의 VM을 호스팅하는 가상화 환경에서 서버 가상화, 개발 서버 가상화, 개발 환경 분리, 테스트 및 백업 등 다양한 용도로 활용
**요약**|**경량하고 이동성이 뛰어나며 주로 애플리케이션 수준의 격리(PaaS)를 제공**| **무겁고 격리가 강화되어 다양한 용도로 활용되지만 이동성이 상대적으로 낮음**
<img src="./images/컨테이너 vs VM.png" width="720"/>

### 도커 엔진 vs 컨테이너 vs 이미지
도커 엔진: 도커 소프트웨어의 본체. 도커 엔진이 있어야 컨테이너 생성 및 실행 가능<br>
컨테이너: OS 수준의 가상화 기술로 리눅스 커널을 공유하면서 프로세스를 격리된 환경에서 실행하는 기술<br>
이미지: 컨테이너 실행에 필요한 파일과 설정값들을 포함. 컨테이너 상태가 바뀌거나 삭제되어도 이미지는 변하지 않음.<br>
- 컨테이너 설계도, 컨테이너 생성하는데 사용
- 이미지를 사용하면 동일한 컨테이너를 여러개 생성 가능
- 컨테이너로도 이미지를 만들 수 있음 -> 이 이미지를 이용한 컨테이너 이동 가능
  

**‼️ 데이터나 프로그램을 독립된 환경에 격리해야하는 이유**
- 데이터/프로그램이 격리되지 않으면 서로에게 영향을 줄 수 있고, 서로에게 강한 의존성을 가질 가능성이 있음<br>
- 공유하는 프로그램, 파일, 라이브러리 등을 한 개의 프로그램만을 위해 수정하게 되면 오류 발생 가능성이 있음<br>
=> 프로그램의 격리: 도커 컨테이너를 사용해 프로그램을 격리하면, 여러 프로그램이 한 서버에 실행되면서 발생하게 되는 문제들을 대부분 해결할 수 있음<br>
=> 자유롭게 옮길 수 있는 컨테이너: 운영체제가 달라도 컨테이너를 옮길 수 잇음, 물리적 환경 차이, 서버 구성의 차이를 무시할 수 있으므로, 운영 서버와 개발 서버의 환경 차이로 인한 문제를 방지할 수 있음

### 도커의 구조
물리 서버 > 리눅스 운영체제 > 도커 엔진 > 컨테이너<br>
컨테이너 속에 운영체제의 주변 부분이 들어있어 프로그램의 명령을 전달받고 이를 커널에 전달하는 구조로 되어있음<br>
<img src="./images/도커 구조.png" width="250"/>
<img src="./images/리눅스 구조.png" width="250"/>

* 윈도우 사용시) 윈도우 os > 가상 환경 > 리눅스 운영체제 > 도커 엔진


### 클라우드 서비스의 3가지 주요 모델
#### - IaaS(Infrastructure as a Service)
인프라 자원(서버, 스토리지, 네트워크 등)을 제공 | 사용자: 운영체제와 소프트웨어를 직접 설치하고 관리해야함 <br> ex) AWS EC2, Microsoft Azure, Google Clound Compuer Engine
#### - Paas(Platform as a Service)
애플리케이션 개발에 필요한 플랫폼 + 인프라 제공 | 사용자: 애플리케이션 개발에만 집중 가능 <br> ex) AWS Elastic Beanstalk, Google App Engine, Heroku
#### - SaaS(Softward as a Service)
소프트웨어 (+ 플랫폼 + 인프라)를 인터넷을 통해 서비스 형태로 제공 } 사용자: 소프트웨어 설치나 관리 없이 웹 브라우저에서 사용 <br> ex) Google Workspace, Salesforce, Slack, 한컴독스


---
## 도커 명령어
도커 명령어 기본 형태<br>
`docker [커맨드] (옵션) [이미지(대상)] (명령어) (인자)`

커맨드(상위/하위)
- 상위: '무엇을', 하위: '어떻게'에 해당하는 부분
- 상위 커맨드: 대상 ex) container, image, network ...  
  ** container일때 생략가능
- 하위 커맨드: 행위 ex) start, stop, rm ...  

  
옵션
- 커맨드의 세세한 설정을 지정: 실행 방법 or 전달할 값 ex) -d, --name, -dit ...

이미지(대상)
- 커멘드와 달리 구체적인 이름을 지정 ex) mysql, nginx ...

명령어 인자
- 대상에 전달할 명령어와 인자 **값**을 지정 ex) 문자 코드, 포트번호 ...

<details>
	<summary>docker container run -d -p 8000:8000 python:3.8 python -m http.server</summary>
  	<div markdown="1">
	- 커맨드) container run: 새로운 도커 컨테이너 실행<br>
	- 옵션) -d: 백그라운드 모드<br>
	- 옵션 + 인자) -p 8000:8000: 포트포워딩<br>
	- 이미지(대상)) python:3.8: python3.8 리눅스 이미지<br>
	- 명령어) python -m http.server: 처음 컨테이너 실행시 실행하는 명령어
  	</div>
</details>

### 컨테이너 생성, 실행, 종료, 삭제
#### 컨테이너를 생성하고 실행하는 커맨드: `docker run`
- `docker image pull` + `docker create` + `docker start`
- `docker run (옵션) 이미지 (인자)`
  
옵션|내용|
|--|--|
--name 컨테이너 이름 | 컨테이너 이름 지정
--p 호스트 포트번호 : 컨테이너 포트 번호| 포트포워딩. 호스트 포트와 컨테이너 포트 연결
-v 호스트 디스크 : 컨테이너 디렉터리 | 바인딩 or 볼륨 마운트
--net 네트워크 이름 | 컨테이너를 네트워크에 연결
-e 환경변수 이름 = 값 | 환경변수 설정
-d | 백그라운드로 실행
-i | 컨테이너에 표준 입력,, 키보드 입력 가능
-t | 컨테이너 내부 프로그램이 터미널에서 실행하는 것처럼 작동(입력을 위해 -i와 같이 사용)
-help | 사용방법 안내 메시지

** -dit: -d(백그라운드 실행) + -it(프로세스 종료되지 않게 유지)  
** 컨테이너가 중지되었을때, 자동 재시작 옵션 --restart unless-stopped

#### 컨테이너를 정지 및 삭제: `docker stop`, `docker rm`
- 종료 -> 삭제 (순서 중요!!)

#### 컨테이너 목록 출력: `docker ps` 
- 현재 실행중인 컨테이너 목록
- -a: 모든 컨테이너 목록

#### 컨테이너 정보 확인: `docker inspect`

#### 컨테이너 통신: `-p [호스트 포트]:[컨테이너 포트]`
`-p 8080:80`  
- 포트: 컴퓨터 네트워크에서 통신을 위한 통로
- 포트 포워딩 : 컨테이너 내부 포트와 호스트 포트 매핑하는 기술
- 매핑: 두개의 포트를 연결
0.0.0.0:8080 -> 80/tcp: 호스트 모든 IP주소(0.0.0.0)에서 들어오는 요청을 컨테이너 80번 포트로 전달. 프로토콜은 tcp <br>
:::8080 -> 80/tcp: 호스트의 모든 IPv6(:::)에서 들어오는 요청을 컨테이너 80번 포트로 전달. 프로토콜은 tcp

### 이미지 생성, 삭제
#### 이미지 생성: `docker commit [컨테이너 이름] [새로운 이미지 이름]`
#### 이미지 삭제: `docker image rm [이미지 이름]:(버전)`
- 이미지를 삭제하는 이유: 컨테이너를 삭제해도 이미지는 그래도 남아 스토리지 용량을 압박하게 됨
- 컨테이너 삭제 후 이미지 삭제해야함!!
- 공백으로 구분해 여러 이미지 삭제 가능
#### 이미지 다운로드: `docker pull [이미지 이름]:(버전)`
#### 이미지 목록 출력: `docker image ls`

### 네트워크 생성
#### 네트워크 생성: `docker network create [네트워크 이름]`
#### 네트워크 정보 확인: `docker network inspect [네트워크 이름]`
---
### 파일 복사
컨테이너 내부와 호스트 간에 파일을 복사하는 작업의 필요성
1. 데이터 복원
   - 컨테이너 안의 데이터를 호스트로 백업하거나, 호스트에서 데이터를 컨테이너로 복원할 때 필요
   - 중요한 데이터를 보존하고 컨테이너가 삭제되더라도 안전하게 데이터를 유지
2. 파일 공유
   - 호스트와 컨테이너 간 파일 공유시 사용
   - 컨테이너 내에서 생성된 결과물이나 로그 파일 등을 호스트에서 확인하고 사용해야할 때

#### 파일 복사: `docker cp [호스트 경로] [컨테이너 이름]:[컨테이너 경로]` <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp;  &nbsp; `docker cp [컨테이너 이름]:[컨테이너 경로] [호스트 경로]`
- 양방향 모두 가능

---
### 볼륨 & 마운트
컨테이너의 휘발성
- 컨테이너가 시작되고 실행되는 동안에는 컨테이너 내의 모든 데이터 및 상태 보존
- 컨테이너가 삭제되면 컨테이너 내부의 데이터와 상태는 사라짐(파일, 프로세스 상태, 메모리 내용 등)
- 컨테이너의 휘발성을 극복하기 위해 **볼륨**이라는 저장 공간을 이용해 데이터 영속성을 보장 ‼️

도커 볼륨(Docker Volume)
- 컨테이너와 호스트 머신 간 데이터를 안전하게 저장, 관리, 공유하기 위한 독립적인 저장 공간
- 도커 엔진에 의해 관리되며 호스트 머신의 파일 시스템과는 _분리되어 작동_
- 데이터의 영속성을 보장하고 여러 컨테이너 간에 데이터를 공유 및 백업이 가능

데이터 퍼시스턴시(Data Persistency)
- 데이터를 옮기는 작업 대신 처음부터 컨테이너 외부에 데이터를 보관하고 이를 컨테이너에서 사용하는 개념
- ex) 바인드 마운트, 도커 볼륨

마운트
- 데이터 퍼시스턴스시를 구현하는 방법
- 호스트의 파일 시스템이나 볼륨을 도커 컨테이너 내부의 특정 경로로 연결하는 작업

마운트 종류
1. 바인드 마운트(Bind Mount)
   - 호스트머신의 디렉토리를 컨테이너 내부에 마운트 => 데이터를 실시간으로 동기화하는 방식
   - 주로 개발 중인 소스 코드나 파일을 컨테이너와 컴퓨터 간에 실시간으로 공유하고 변경 내용을 바로 확인할 때 사용
   - **개발 작업을 편리하게 하고 컨테이너와 호스트 간에 데이터를 손쉽게 주고받을 수 있음**
2. 도커 볼륨(Docker Volume)
   - 볼륨은 컨테이너에 디스크 형태로 마운트 => 안전하게 데이터를 저장하고 공유하는 방식
   - 데이터베이스나 중요한 설정 파일과 같은 컨테이너 안의 파일을 안전하게 보관하고 다른 컨테이너와 공유할 때 사용
   - **컨테이너 간 데이터를 안전하게 공유하고 영속성을 확보**

#### 볼륨 생성: `docker volume create [볼륨 이름]`
#### 볼륨 상세 정보: `docker volume inspect [볼륨 이름]`
---
# Dockerfile
- 도커 이미지를 빌드하기 위한 텍스트 기반의 스크립트 또는 설정 파일
- docker 스크립트를 재료 폴더에 컨테이너에 넣을 파일과 함께 두어 이미지 생성

#### dockerfile로 이미지 생성: `docker build -t [이지미 이름] [재료 폴더 경로]`
#### dockerfile 내에 권한 설정

| SetUID (4xxx)                                             | SetGID (2xxx)                                                                                          | Sticky bit (1xxx)                                                |
|------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| 주로 파일에 사용, 파일 사용자가 해당 파일의 소유자의 권한으로 파일을 사용o, 디렉토리에서는 거의 사용x | 파일이나 디렉토리에 사용, 파일에 적용 -> 파일을 사용자가 해당 파일의 소유자의 그룹 권한으로 파일을 사용o, 디렉토리에 적용 -> 그 디렉토리 내에서 생성된 파일들이 그 디렉토리의 그룹 소유권을 물려받음 | 주로 디렉토리에 사용, 디렉토리 내에서 파일을 생성한 사용자만 그 파일을 삭제할 수 있도록 제한 |

---
### 컨테이너 개조
컨테이너를 실행 중인 상태에서 컨테이너 내부의 파일 시스템, 환경 변수, 네티워크 설정 등을 변경하는 작업

#### 컨테이너를 개조하는 방법
- 마운트와 파일 복사를 이용
- 컨테이너에서 리눅스 명령어 실행 => **셀** 필요
  - 기본적으로 bash 셸
  - bash를 실행해 명령을 입력받을 수 있는 상태로 만들어야함
  - bash를 실행시키려면 '/bin/bash' 인자를 전달해야함
  - `docker run / docker exec [커멘드]` 와 함께 사용

##### docker exec
  - 컨테이너 속에서 명령어를 실행하는 커맨드
  - bash 없이 어느 정도 명령을 직접 전달할 수 있지만 초기 설정이 없어 동작하지 않는 경우도 있기에 기본적으로 셸을 통해 명령 실행
  - `docker exec -it [컨테이너 이름] /bin/bash`

##### docker run
- `docker run -dit [이미지 이름] /bin/bash`
- docker run은 기본 사용만 권장. bash 실행이 필요할 경우 docker exec 사용 -> 컨테이너 내부로 접속 권장

** 컨테이너는 실행 중 이지만 기본 패키지(예: Apache 등)는 실행되지 않음  
=> 수동으로 기본 패키지를 실행해야함 `service apache2 start`

---
### 도커 엔진 명령어(도커 엔진 관리 범위)
#### 도커 엔진 시작: `sudo systemctl start docker`
#### 도커 엔진 종료: `sudo systemctl stop docker`

---
## 이미지 생성시 커밋(Commit)과 빌드(Build) 차이
| 항목       | 커밋 (Commit)                                                                                          | 빌드 (Build)                                                                                 |
|------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
|사용 목적| 현재 실행 중인 컨테이너의 상태를 새로운 이미지로 저장하는 과정. | Dockerfile을 사용하여 새로운 이미지를 체계적으로 생성하는 과정. |
| 동작 방식  | 현재 컨테이너의 파일 시스템과 그 상태(설치된 패키지, 생성된 파일 등)를 그대로 이미지에 반영. | Dockerfile에 명시된 명령어들을 순차적으로 실행하여 이미지를 빌드함. |
| 컨테이너 상태 | 컨테이너 내부에서 일어난 변경 사항이 이미지에 포함됨. | 컨테이너의 현재 상태와는 무관하게, Dockerfile에 명시된 내용만으로 이미지가 생성됨. |
| 파일 포함 | 실행 중인 컨테이너에서 발생한 모든 변경 사항이 이미지에 포함됨. | Dockerfile에 명시된 파일 복사(COPY), 패키지 설치(RUN apt-get install) 등의 작업만 포함됨. |
| 관리 가능성 | 변경 사항이 즉시 이미지에 저장되지만, 변경 이력 관리나 기록이 어려움. | Dockerfile을 통해 이미지를 체계적으로 관리하고, 동일한 환경에서 이미지를 재생산할 수 있음. |
| 재현 가능성 | 재현 가능성은 떨어지며, 체계적인 관리가 어렵다. | 빌드 과정이 Dockerfile에 명확히 기록되므로 재현 가능한 이미지 생성이 가능. |
| 주요 사용 상황 | - 컨테이너 내부에서 작업한 내용을 즉시 저장하고 싶을 때.<br>- 컨테이너 상태를 보존하고 이후에 동일 상태로 복원하고 싶을 때. | - 일관된 개발 환경을 유지해야 할 때.<br>- 새롭게 이미지를 만들거나 여러 환경에서 동일한 설정을 배포할 때.|

**주의: 두 방식 모두 마운트 된 파일/폴더는 이미지에 포함시키지않음**

---
# 도커 허브
도커 제작사에서 운영하는 공식 도커 레지스트리(Registy)

도커 레지스트리
- 도커 이미지를 저장하고 관리하는 중앙 저장소로 동작하는 서버
- 도커 이미지를 업로드, 다운로드, 검색, 삭제 등의 작업을 수행
- _도커 이미지를 공유하고 배포하기 위해 사용_

레포지토리
- 도커 이미지의 집합을 나타내는 공간
- 이미지의 다양한 버전을 관리하고 구분
- 레포지토리 옵션
	- public(공개): 레포지토리 내의 이미지들이 공개, 누구나 해당 레포지토리와 이미지를 볼 수 있고 다운로드 가능
	- private(비공개): 레포지토리 내의 이미지들이 비공개, 특정 사용자나 팀에게만 이미지 공유하거나 접근 권한 부여
<img width="500" alt="image" src="https://github.com/user-attachments/assets/014ffb51-eefb-49bc-b387-c1bf608a7b84">

태그 = 버전
- 도커 이미지의 버전을 식별하기 위한 라벨
- 이미지의 특정 버전을 구분하고 관리하는데 사용
- `레지스트리 주소(도커 허브ID)/레포지토리 이름:버전`

#### 이미지에 태그를 부여해 복제: `docker tag [기존 이미지 이름]:(태그) [레지스트리 주소(도커 이름)]/[레포지토리 이름]:[사용자 지정 태그]`
- 명령어 실행 후 기존 이미지와 태그가 부여된 이미지가 둘 다 존재

#### 이미지를 업로드: `docker push [레지스트리 주소(도커 이름)]/[레포지토리 이름]:[버전]`
- 레포지토리는 처음 업로드할 때 존재하지 않고, push 명령어를 실행하여 만들어짐

---
# 도커 컴포즈
시스템 구축과 관련된 명령어를 하나의 텍스트 파일(정의 파일)에 기재해 명령어 한번에 시스템 전체를 실행하고 종료와 폐기까지 한번에 하도록 도와주는 도구  
<img width="720" alt="image" src="https://github.com/user-attachments/assets/0a3dbab2-acbd-4873-813b-c996ac1d03e5">

### 도커 컴포즈의 구조
- 시스템 구죽에 필요한 설정을 YAML 포맷으로 기재한 정의 파일을 이용한 전체 시스템을 일괄 실행(run)|일괄 종료 및 삭제 (down) 할 수 있는 도구
- 정의 파일에는 컨테이너나 볼륨을 '어떠한 설정으로 만들지'에 대한 항목이 기재
- up 명령어
  - docker run 명령어와 비슷
  - 정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행
  - 정의 파일에는 *네트워크*나 *볼륨*에 대한 정의도 기재할 수 있어 주변 환경을 한꺼번에 생성 가능
	
	| 옵션                           | 내용                                                     |
	|-------------------------------|---------------------------------------------------------|
	| `-d`                          | 백그라운드로 실행                                           |
	| `--no-color`                  | 화면 출력 내용을 흑백으로 함                                  |
	| `--no-deps`                   | 링크된 서비스를 실행하지 않음                                  |
	| `--force-recreate`            | 설정 또는 이미지가 변경되지 않더라도 컨테이너를 재생성               |
	| `--no-create`                 | 컨테이너가 이미 존재할 경우 다시 생성하지 않음                     |
	| `--no-build`                  | 이미지가 없어도 이미지를 빌드하지 않음                           |
	| `--build`                     | 컨테이너를 실행하기 전에 이미지를 빌드                           |
	| `--abort-on-container-exit`   | 컨테이너가 하나라도 종료되면 모든 컨테이너를 종료                   |
	| `-t, --timeout`               | 컨테이너를 종료할 때의 타임아웃 설정, 기본은 10초                  |
	| `--remove-orphans`            | 컴포즈 파일에 정의되지 않은 서비스의 컨테이너를 삭제                 |
	| `--scale`                     | 컨테이너의 수를 변경                                         |  

- down 명령어
  - 컨테이너와 네트워크를 정지 및 삭제
  - 볼륨과 이미지는 삭제하지 않음
 
### 도커 컴포즈 사용법
- 호스트 컴퓨터에 폴더를 만들고 이 폴더에 정의 파일(YAML 파일)을 배치
  <img width="840" alt="image" src="https://github.com/user-attachments/assets/f07fba10-aefc-4822-9772-4261cd010e5d">

#### 컴포즈 파일(정의 파일) 작성
| 항목                           | 설명                                   | 예시                        |
|--------------------------------|----------------------------------------|-----------------------------|
| 첫 줄에 도커 컴포즈 버전을 기재   | Docker Compose 파일 버전 기재           | `version: '3'`              |
| 주 항목 services, networks, volumes | 각 항목 아래에 설정 기재                 | `services:`                 |
| 항목 간의 상하 관계는 들여쓰기로 나타냄 | 들여쓰기를 사용하여 계층 구조 표현        | `mysql000ex11:`             |
| 들여쓰기는 일정한 공백 사용        | 2칸 또는 4칸의 공백을 사용               | `image: mysql:5.7`          |
| 각 항목의 키 뒤에 콜론(:)을 붙임    | 키 뒤에 콜론을 붙이고 값을 기재           | `image: mysql:5.7`          |
| 리스트 항목은 줄 앞에 -를 붙임      | 리스트 형태로 여러 값을 나열할 때         | `- wordpress000net1`        |
| 콜론 뒤에는 공백                | 키와 값 사이에 공백 삽입                 | `MYSQL_ROOT_PASSWORD: myrootpass` |
| # 뒤는 주석                    | 주석 작성                              | `# MySQL 설정`              |
| 문자열은 작은따옴표 또는 큰따옴표로 감쌈 | 문자열 값은 따옴표로 감쌈                 | `'mysql'` 또는 `"mysql"`    |

#### 도커 컴포즈 실행: `docker-compose -f [컴포즈 파일 경로] up (옵션)`
#### 도커 컴포즈 리소스 삭제: `docker-compose -f [컴포즈 파일 경로] down (옵션)`
- 컨테이너를 정지하지 않아도 삭제 가능
- 볼륨과 이미지는 데이터 영속성과 이미지 재사용의 이유로 삭제되지 않음
- 옵션
  - `--rmi`: 삭제시 이미지도 삭제
    - all로 지정하면 사용했던 모든 이미지가 삭제
    - local로 지정하면 커스텀 태그가 없는 이미지만 삭제
  - `-v, -volumes`: volumes 항목에 기재된 볼륨을 삭제 (단, external로 지정된 볼륨은 삭제되지 않음)
  - `--remove-orphans`: 컴포즈 파일에 정의되지 않은 서비스의 컨테이너도 삭제
 
#### 도커 컴포즈 stop: `docker-compose -f [컴포즈 파일 경로] stop [옵션]`

#### 기타
- `docker-compose [명령어] -d`: -d 옵션을 붙이고 경로 정의 없이 도커 컴포즈 명령어를 실행하면 현재 작업 디렉터리를 컴포즈용 폴더로 사용
- `docker-compose scale [서비스명]=[컨테이너 갯수]`
  - --scale 옵션 : 한번에 여러개 띄움
  - 예: `docker-compose -f my-docker-compose.yml up --scale filebeat=3`  
** scale 사용시 포드 포워드 중복 에러 유의
	```
	ports:
	  - "8001-8002:80"
	```

<!-- 토글 
<details>
	<summary>토글 접기/펼치기</summary>
  	<div markdown="1">
      안녕
  	</div>
</details>
-->
