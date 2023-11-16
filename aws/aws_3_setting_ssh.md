```
EC2 인스턴스 생성+pem 파일 생성, 탄력적IP 할당까지 진행하였음
이제 콘솔(터미널)에서 EC2 서버로 SSH 접속을 위한 설정을 알아본다.
```
기준: 2023.11.16

# EC2 서버 접속하기
- EC2 접속을 위해 OS별로 상이함
  - Mac/Linux : 터미널
  - Windows : putty
 
## 1. Mac/Linux
### 1-1. 기본 명령어
- SSH 접속을 위해서는 매번 아래 명령어 수행 필요
```
$ ssh -i pem키위치 EC2탄력적IP주소
```

### 1-2. pem 키 파일 복사
- 키페어 pem 파일을 ~/.ssh/ 경로로 이동 후 ssh 실행 시 pem 파일을 자동으로 읽어 접속함
```
$ cp -f pem파일현재위치 ~/.ssh/
$ chmod 600 ~/.ssh/pem키파일명
```

### 1-3. config 파일 작성
- 만일 기존 config 파일이 있다면 수정하면 됨!
```
$ ls ~/.ssh/config
$ vi ~/.ssh/config
```
- 아래 양식에 맞게 작성
```
# 주석
Host 원하는서비스명
    HostName EC2탄력적IP주소
    User ec2-user
    IdentityFile ~/.ssh/pem키파일명
```
- 작성한 config 파일에 실행 권한 없으면 실행 권한 설정 추가
```
chmod 700 ~/.ssh/config
```

### 1-4. SSH 접속
- ssh 명령어로 접속
```
$ ssh config에등록한'서비스명'
```
- 만일 아래처럼 '너 계속 진행할래?' 물어보면 yes 입력하면 됨!
```
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
- 접속했다! 빙고!

## 2. Windows
- 윈도우는 맥/리눅스와 달리 내장 터미널이 없어 별도 putty를 설치하여 진행
  - www.putty.org 에서 하기 파일 다운로드
    - putty.exe
    - puttygen.exe
   
### 2-1. pem 파일 변환
- putty는 pem 키 사용이 불가하여 pem 키를 ppk 파일로 변환해야 이용 가능
- puttygen.exe 실행 > Conversions > Import key > pem 키 선택 > 자동 변환 진행 > Save private key 클릭 (ppk 파일 생성) > 예
- putty.exe 실행 > 좌측 메뉴 > Session
  - HostName: ec2-user@탄력적IP주소
  - Port: 22
  - Connection type: SSH
- 좌측 메뉴 > Connection > SSH > Auth > (우하단) Browse... > ppk 파일 선택
- 좌측 메뉴 > Session > (우측) Saved Sessions에 이름 입력 > Save > Open > 예


## 3. EC2 리눅스 서버 필수 설정
- 자바 기반 application 작동을 위해서 서버에서 필수 진행해야할 설정
  - Java 설치
  - 타임존 변경 : 기본 서버 시간은 '미국 시간대'임
  - 호스트네임 등록
- 위에서 설정한 접속 방식을 통해 EC2 서버로 접속
 
### 3-1. 자바 설치
- 여기서는 자바8 설치할꺼임
```
$ sudo yum install -y java-1.8.0-openjdk-devel.x86_64
```
- 설치 완료 후, 인스턴스의 java 버전을 8로 변경 (현재 기본 자바버전은 7임)


