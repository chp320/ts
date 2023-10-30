# MySQL 삭제하기 (on MacBook)
## 1. MySQL 프로세스 죽이기 (homebrew 로 설치한 경우)
```
brew services stop mysql
```
  - launchctl 등록한 경우 내리기
```
sudo launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```
## 2. 관련 파일 삭제하기
* 설치 경로 확인
```
which mysql
```
* 삭제하기 (homebrew)
```
brew uninstall --force mysql
```
  - 다음 명령어로도 삭제 가능
```
brew uninstall mysql --ignore-dependencies
brew remove mysql
brew cleanup
```
* 남아있는 파일 정리를 위해 한 줄씩 수행
```
sudo rm -rf /usr/local/mysql
sudo rm -rf /usr/local/bin/mysql
sudo rm -rf /usr/local/var/mysql
sudo rm -rf /usr/local/Cellar/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /tmp/mysql.sock.lock
sudo rm -rf /tmp/mysqlx.sock.lock
sudo rm -rf /tmp/mysql.sock
sudo rm -rf /tmp/mysqlx.sock
sudo rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
```
* 컴퓨터 재부팅

