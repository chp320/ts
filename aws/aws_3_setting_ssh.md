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
```
$ ssh config에등록한'서비스명'
```


