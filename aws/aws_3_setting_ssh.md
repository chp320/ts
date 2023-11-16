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
# Amazon Linux 2 AMI인 경우 (AL2)
$ sudo yum install -y java-1.8.0-openjdk-devel.x86_64
# Amazon Linux 2023 AMI인 경우 (AL2023)
$ sudo dnf install java-1.8.0-amazon-corretto-devel
```
- 참고로 AL2와 AL2023의 차이는.. AL2는 yum 명령어를 사용하며, AL2023은 dnf 명령어를 사용해서 install 관리를 한다고 한다.
  - 더불어 AL2는 python2.7 기반인데 AL2023은 python3 기반이라고 함
- 자바 버전 확인
```
$ java -version
```
- 자바 버전 변경: 만일 자바8이 아니라 다른 버전도 함께 설치되어 있는 경우, 아래 명령어를 통해 쉽게 버전 변경이 가능함
```
$ alternatives --config java
```
  - 상기 명령어 수행하면 설치된 자바 버전이 넘버링되어 나타나며, 변경코자 하는 버전의 번호를 입력하면 해당 버전으로 변경됨

### 3-2. 타임존 변경
- EC2 서버 기본 타임존은 UTC로 한국 시간대로 변경 필요
- 아래 명령어를 수행하면 현재 UTC로 지정된 것을 확인할 수 있음
```
$ cat /etc/localtime
TZif2UTCTZif2UTC
UTC0
```
- 하기 명령어를 차례로 수행
```
$ sudo rm /etc/localtime
$ sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
# 변경된 타임존 확인
$ date
```

### 3-3. 호스트네임 등록
- 현재 정보 확인
```
$ uname -n
```
- hostname 변경
```
$ sudo vim /etc/hostname
원하는호스트명등록
# 서버 재부팅 (이후에 변경내용 확인)
$ sudo reboot
```
- 호스트 주소 검색 시 가장 먼저 확인하는 /etc/hosts 에 변경한 hostname 등록
  - hostname이 /etc/hosts 에 등록되지 않아 발생한 장애는 아래 우와기술블로그 참고
```
$ sudo vim /etc/hosts

# /etc/hosts 파일에 방금 등록한 hostname 추가
127.0.0.1 등록한hostname

# 아래 명령어로 정상 등록 여부 확인
$ curl 등록한hostname
```

- 🙈 주의 🙈
  - OS버전에 따라 수정해야할 파일이 다름
    - CentOS 5,6버전은 /etc/sysconfig/network
    - CentOS 7버전은 /etc/hostname
   

##### 참고 사이트
https://linux.how2shout.com/how-to-install-java-on-amazon-linux-2023/
https://techblog.woowahan.com/2517/
https://m.blog.naver.com/jsky10503/220743935499

