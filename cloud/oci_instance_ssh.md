# OCI 에 인스턴스 생성하기 (ssh 접속)

## 작업 순서는 아래와 같다.
1. local PC에서 RSA Private-Public key 생성
2. OCI 인스턴스 생성 시 public key 값 등록
3. 접속 테스트 수행

## 1. public key 생성 (mac)
### 명령어 양식: ssh-keygen -t rsa -f ~/.ssh/{KEY_FILENAME} -C {USER_ACCOUNT}
``` bash
$ ssh-keygen -t rsa -f ~/.ssh/oci-key -C xx@xx.com
# 명령어 수행하면 아래 2개 파일이 생성됨
oci-key   oci-key.pub
```

## 2. public key 등록
### OCI 메인화면 > 'Create VM instance' > Add SSH keys > Paste public keys 선택
### 1에서 생성한 public key의 값을 복사하여 붙혀 넣기
```
$ cat ~/.ssh/oci-key.pub
```

## 3. ssh 접속
### OCI에서는 기본 계정으로 opc 를 부여하며, 해당 계정으로 ssh 접속 필요!
#### https://docs.oracle.com/en/cloud/cloud-at-customer/occ-get-started/log-vm-using-ssh.html#GUID-D45A516E-09EA-4BF6-A5C3-ABD6EAB8CA3D
### 명령어 양식: ssh opc@{IP_ADDRESS} -i {KEY}
``` bash
$ ssh opc@xxx.xxx.xxx.xxx -i ~/.ssh/oci-key
```

## (번외) SSH 접속 시 웰컴메시지 출력
### SSH 접속 시, 웰컴메시지 노출을 위해서는 /etc/motd 파일을 수정하면 된다.
### 단, 사용자 계정이 opc 인데 상기 파일은 root 소유라 sudo 명령어 필요함
### 아스키 문자로 꾸미고 싶다면 하기 url에서 '아스키 아트'를 생성하면 됨
#### https://ko.rakko.tools/tools/68/
``` bash
$ sudo vi /etc/motd
```


##### reference
https://boring-notes.tistory.com/m/entry/OCI-오라클-클라우드-1-가입평생-무료-사용하기

https://boring-notes.tistory.com/entry/GCP-구글클라우드플랫폼-VM머신간-접속-설정하기

https://a-researcher.tistory.com/93



끝.
