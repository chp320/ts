# ORM 이란?

## 자바에서의 영속성
- 자바를 포함한 애플리케이션에서는 시스템이 종료되어도 데이터가 사라지지 않고 보관될 수 있는 '영속성(Persistence)'을 부여했음
- 자바는 jdbc를 제공함으로써 DBMS(ex. MySQL, Oracle, etc) 종류에 상관없이 DB 처리를 가능하게 하였음
<img src="https://github.com/chp320/ts/assets/47440517/1319a564-f786-4c9c-96d3-e65f021dc69d" />
- 더불어 별도 xml 쿼리 파일을 생성하고 SQL Mapper 라 지칭하였고, 질의하기 위한 쿼리를 해당 xml 파일로 작성만 하면 되었음

## 한계
- 그러나 쿼리를 하기 위해 xml 파일 작성에 공수가 많이 들고
- 중복된 쿼리가 반복 작성되면서 피로도가 높아짐

## ORM 등장
- 보다 비즈니스 로직 구현 본연의 기능에 충실하고
- 데이터는 객체로 다뤄서 쿼리 질의에 대한 부담을 덜기 위해 ORM 기법이 등장

## ORM 종류
- 다양한 DBMS가 존재하듯이 ORM도 다양한 종류가 존재하며, 자바는 JPA(Java Persistence API) 를 표준으로 사용함
- JPA 는 자바에서 RDB를 사용하기 위한 '인터페이스' 이며,
- 실제 구현은 하이버네이트(hibernate) 가 대표적임
<img src="https://miro.medium.com/v2/0*CzE1_rn0FyFjRJW4.jpg" alt="object relational mapping" />
<img src="https://github.com/chp320/ts/assets/47440517/66afe870-7845-446e-89ed-d864c9dce92d" />

<hr>

## 엔티티?
### 정의
- 엔티티(entity)는 DB의 테이블과 맵핑되는 객체
- 실제로는 자바 객체 그 자체이지만 "DB 테이블과 직접 연결(일치)" 하는 특징 존재
- @Entity 어노테이션으로 물리 테이블과 맵핑
- 엔티티는 '엔티티 매니저(entity manager)'를 통해서 관리되며, 엔티티 매니저는 DB-Applicaion 간 객체 생성, 수정, 삭제 등의 역할 수행함

## 영속성 컨텍스트?? (Persistence Context)
### 정의
- JPA 의 중요한 특징 중 하나로, '엔티티를 관리하는 가상의 공간'
- 특징
  - 1차 캐시: 엔티티 조회 시 우선적으로 '1차 캐시'에서 데이터를 조회하고 없으면 DB에서 값을 조회
  - 쓰기 지연: 트랜잭션 commit 전까지 DML 수행하지 않고, commit 후 한꺼번에 쿼리 전송하여 DB 부하를 줄임
  - 변경 감지: 트랜잭션 commit 시, 1차 캐시에 저장된 값과 비교하여 변경된 값은 자동 반영
  - 지연 로딩(lazy loading): DB 부하를 줄이기 위해서 쿼리 요청 데이터를 즉시 로딩하는 것이 아닌, 필요 시 로딩하는 것


<br><br><br><br>


##### 참고 사이트

https://medium.com/@emccul13/object-relational-mapping-9d84807f5536

https://networkencyclopedia.com/java-database-connectivity-jdbc/

https://antstudy.tistory.com/447
