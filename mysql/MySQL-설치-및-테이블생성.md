# MySQL 설치하기 (on MacBook)
## 1. homebrew 로 설치
* 설치하기 (설치완료까지 약 10분 정도 소요)
```
brew install mysql
```
* mysql 서비스 시작하기
```
brew services start mysql
```
* root 비밀번호 로그인
  - 아래 명령어 입력 후 별도 비밀번호 입력 필요
```
  mysql -u root -p
```
* root 비밀번호 없이 로그인
  - brew 설치 시 root 패스워드 없이 완료됨 (We've installed your MySQL database without a root password.)
```
  mysql -uroot
```
  - 만일 root 비밀번호 설정을 위해서는 아래 명령어 수행
```
mysql_secure_installation
```

## 2. 사용하기
* Database 생성
```
CREATE DATABASE [database명] ;
```
<img width="699" alt="image" src="https://github.com/chp320/ts/assets/47440517/e5bb9666-30cf-4443-ac23-171cd96b9b97">

  - 생성된 database 확인
```
show databases ;
```
<img width="699" alt="image" src="https://github.com/chp320/ts/assets/47440517/1999af04-58cb-4052-920a-f30cb5eb7725">

  - 사용할 database 선택
```
use [database명] ;
```
<img width="699" alt="image" src="https://github.com/chp320/ts/assets/47440517/d1afbfc0-9292-422d-8e88-cac34a120433">

* Table 생성
```
create table [table명] (
`[컬럼명]` [속성], 
`[컬럼명]` [속성], 
...
) ;
```
<img width="699" alt="image" src="https://github.com/chp320/ts/assets/47440517/0902e678-74ca-4e2b-892d-c1e3dca2cdcb">

  - 생성된 table 확인
```
show tables ;
```
<img width="699" alt="image" src="https://github.com/chp320/ts/assets/47440517/60c6d435-fe4a-4e66-b291-8e84014466bf">

  - 생성된 테이블 구조 확인
```
desc [table명] ;
```
<img width="699" alt="image" src="https://github.com/chp320/ts/assets/47440517/0ae03049-1272-49f3-aef7-28b21d49df7d">

## 3. 확인하기
- 데몬 실행 확인
```
brew services list
```
<img width="514" alt="image" src="https://github.com/chp320/ts/assets/47440517/9271d580-b221-4e68-bb6a-d0fc3e34a88d">

