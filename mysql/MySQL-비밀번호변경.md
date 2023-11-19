# MySQL 비밀번호 변경하기 (on Macbook)
- 나의 경우 비밀번호를 간단하게 설정했다고 생각했지만 가끔 그 쉬운 비밀번호를 까먹을 때가 있다.
- 기 등록된 비밀번호를 다시 한 번 간단한 비밀번호로 변경해보려 한다 ㅠㅠ

## 1. MySQL 서비스 기동하기
- 이미 Homebrew 로 mysql를 설치한 상태이고, brew로 서비스를 기동한다.
```
$ brew services start mysql
```

## 2. root 계정으로 접속하기
- 로컬에서 테스트 용도로 별도 계정 생성 없이 root 계정으로 사용 중이다.
```
$ sudo mysql -u root -p
```

## 3. 비밀번호 변경하기
- 비밀번호를 1234 로 변경하는 케이스 가정
```
mysql> use mysql;
mysql> UPDATE user set password=password("1234") where user = 'root';
Query OK, 1 row affected (0.02 sec)
Rows matched: 4  Changed: 4  Warnings: 0
mysql> flush privileges;
```

## 4. 위의 방법이 안된다면 아래 명령어 참고
```
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
```

## 주의!
- flush privileges; 하지 않으면 나중에 root 계정으로 접속 시 접속 불가하다고 하니. 필수로 수행할 것!


##### 참고 사이트

https://hansoul.tistory.com/21



