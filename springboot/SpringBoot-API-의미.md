# API 와 REST API

## API ??
- API 는 클라이언트가 처리하고자 하는 요청을 받아서 서버에 전달하고, 그 결과를 클라이언트에 잘 돌려(response)주는 행위를 의미

## REST API ??
- REST : REpresentational State Transfer. '이름(representational)'을 구분해서 '상태(state)'를 '전송(transfer)'하는 것
  - 즉, "명확하고 이해하기 쉬운 API" 지칭
- REST API 는 URL만 보고도 '어떤 행동'을 하는 API 인지 명확히 알 수 있는 것을 의미하며, 그렇게 잘 만들어진 API를 'REST하게 만들어진 API'라는 의미로 RESTful API라 부르기도 함

## REST API 생성 규칙
### 규칙1. URL에는 동사를 쓰지 말고, 자원을 표시해야 한다.
- 자원? 가져오는 데이터를 지칭
### 규칙2. 동사는 HTTP 메서드로.
- HTTP 메서드: POST(create), GET(read), PUT(update), DELETE(delete)
<table>
  <tr>
    <td>설명</td>
    <td>적합한 HTTP 메서드</td>
  </tr>
  <tr>
    <td>조회</td>
    <td>GET</td>
  </tr>
  <tr>
    <td>추가</td>
    <td>POST</td>
  </tr>
  <tr>
    <td>수정</td>
    <td>PUT</td>
  </tr>
  <tr>
    <td>삭제</td>
    <td>DELETE</td>
  </tr>
</table>





