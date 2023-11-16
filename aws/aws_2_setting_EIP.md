```
앞서 생성한 인스턴스는 이제 하나의 서버로 볼 수 있고, 따라서 해당 서버에도 IP가 할당된 상태임
그러나 매 서버 기동 시마다 IP도 새로 할당됨
따라서 인스턴스의 IP를 '고정 IP'로 할당이 필요하며, 프리티어 요금제에서는 고정IP(Elastic IP, EIP, 탄력IP)를 1개만 지원됨
```
기준: 2023.11.16


# EIP (고정 IP) 할당

## 1. EIP 페이지 이동
- EC2 대시보드 > (좌측메뉴) 네트워크 및 보안 > 탄력적 IP

## 2. EIP 할당
- 우상단 '탄력적 IP 주소 할당' 클릭

### 2-1. 탄력적 IP 주소 설정
* 네트워크 경계 그룹 : 기본 설정값 (서울 리전인 경우, ap-northeast-2)
* 퍼블릭 IPv4 주소 풀 : Amazon의 IPv4 주소 풀 (기본)
* 우하단 '할당' 클릭

## 3. 할당 IP 확인
- 탄력적 IP 주소 대시보드에 '탄력적 IP주소가 할당되었다'는 메시지가 보인다면 작업이 완료된 것임.
- 앞서 EC2 대시보드의 '퍼블릭 IPv4 주소' 에서 봤던 IP와 상이한 것을 알 수 있음.

## 4. EC2와 EIP 연결
- EIP가 할당 완료되었지만 앞서 생성한 인스턴스와 연결이 필요함
- 우상단 '이 탄력적 IP 주소 연결' 버튼 클릭 or 작업 > '탄력적 IP 주소 연결' 클릭

### 4-1. 탄력적 IP 주소 연결
* 리소스 유형 : 인스턴스 (기본값)
* 인스턴스 : 본인 인스턴스 생성
* 프라이빗 IP 주소 : EC2 대시보드 '프라이빗 IPv4 주소'에서 확인한 IP 주소 선택
* 우하단 '연결' 클릭

### 4-2. 작업 완료 확인
- EIP 대시보드의 '연결된 인스턴스 ID' 컬럼에 본인 선택한 인스턴스 정보 확인
- (좌측메뉴) 인스턴스 > 인스턴스 '클릭' 하여 탄력적 IP에 방금 할당된 IP가 정상 노출되는지 확인
  - 만일 해당 정보가 반영되지 않았다면 '인스턴스 새로고침' 버튼 클릭해보자! 그러면 잘 보일꺼임.

## 🙈 주의!! 🙈
- 탄력적 IP(EIP)는 생성 즉시 EC2 서버에 연결하지 않으면 <u><b>비용이 발생</b></u> 함!!
- 더는 사용할 인스턴스가 없는 경우에도 무조건 <u><b>탄력적 IP를 삭제</b></u> 해야 함!



##### 참고 사이트
https://goddaehee.tistory.com/192
