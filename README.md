# (주문)생산자와 (주문)소비자 간 역할 수행 서비스를 제공하는 서버
gin 프레임워크를 이용한 WAS 서버를 통해 주문 생산과 주문 소비가 이루어진다.
생산자와 소비자는 url의 1차 path에서 buyer / seller로 구분된다.

### 시작하기

toml 파일을 자신의 환경에 맞게 바꿔준다.
*  config/config.toml
  ```toml
  port = "WAS 서버를 실행시킬 포트번호"
  host = "서버와 연동할 mongoDB실행 url"
  ```

실행을 위한 go package를 설치한다.
*  go 의존성 패키지 설치
  ```shell
  go mod init
  go mod tidy
  ```
  
swagger 파일을 만들어준다
*  swagger 패키지 설치
  ```shell
  swag init
  ```
#
*  main 파일 실행
  ```shell
  go run main.go
  ```

* 일별 주문번호의 정상적인 count를 위해선 orderedlist 컬렉션에 아래와 같이 {daycount : 0} 의 데이터를 먼저 넣어주어야 한다.
<img width="233" alt="스크린샷 2022-12-28 13 28 09" src="https://user-images.githubusercontent.com/100397903/209757411-95b2d6be-b243-43b6-9817-aa1241f958f1.png">
<img width="992" alt="스크린샷 2022-12-28 13 26 10" src="https://user-images.githubusercontent.com/100397903/209757348-eee6a96a-af9c-4291-98d2-aa07c86fbb8b.png">
- 이후 controller/buyercontroller.go 파일 상단의 daycountID 상수를 생성된 daycount Document의 ObjectId로 변경해줄 
<img width="578" alt="스크린샷 2023-01-03 15 45 17" src="https://user-images.githubusercontent.com/100397903/210310815-090f3766-eacb-477d-868b-832464c109f7.png">

## 프로그램 실행방법
제공하는 기능의 api 리스트는 아래와 같다.
<img width="1097" alt="스크린샷 2022-12-28 16 16 12" src="https://user-images.githubusercontent.com/100397903/209773726-91f8a9ce-2e89-4a86-9af8-8f127fa90f53.png">


테스트를 위해선 프로그램 실행 후 http://localhost:8080/swagger/index.html#/ 페이지로 접속하면 swagger를 통해 실행할 수 있다.

### seller
<hr />

  - seller - 메뉴 신규 등록
  <img width="721" alt="스크린샷 2022-12-28 13 40 55" src="https://user-images.githubusercontent.com/100397903/209758419-ab05e02f-63be-4c1c-8982-d1efdcee6dc9.png">
  
  - seller - 메뉴 업데이트
  <img width="755" alt="스크린샷 2022-12-28 15 22 15" src="https://user-images.githubusercontent.com/100397903/209767648-b9548157-dfb6-4509-a49c-6b57183a011f.png">
  
  - seller - 메뉴 삭제
  <img width="720" alt="스크린샷 2022-12-28 15 24 16" src="https://user-images.githubusercontent.com/100397903/209767851-bbde25bf-c648-4947-a589-b8e436d8741e.png">
  
  - seller - 주문 상태 업데이트
  <img width="734" alt="스크린샷 2022-12-28 15 43 16" src="https://user-images.githubusercontent.com/100397903/209770013-3d306c54-bcdb-40d4-99b7-f5e882c8b38a.png">
<img width="725" alt="스크린샷 2022-12-28 15 43 27" src="https://user-images.githubusercontent.com/100397903/209770018-570facd3-5bf0-43dc-a363-aacfe199e31e.png">
<img width="718" alt="스크린샷 2022-12-28 15 43 37" src="https://user-images.githubusercontent.com/100397903/209770110-86cf149a-afbc-4972-8521-6e0fc48738a7.png">
<img width="725" alt="스크린샷 2022-12-28 15 43 47" src="https://user-images.githubusercontent.com/100397903/209770129-351fd625-5f19-4f7e-bb31-7df7ba546e71.png">

  - seller - 주문 리스트 가져오기(page parameter 별 5개씩 반환)
  <img width="720" alt="스크린샷 2022-12-28 15 49 05" src="https://user-images.githubusercontent.com/100397903/209770547-9b7fa894-dc67-47e9-a0fa-bb97ba3654e7.png">

  - seller - 추천 메뉴 업데이트
  <img width="721" alt="스크린샷 2022-12-28 15 54 28" src="https://user-images.githubusercontent.com/100397903/209771204-6f33b476-3b23-4065-b551-7e31a97f2479.png">



### buyer
<hr />

  - buyer - 모든 메뉴 가져오기(page parameter 별 5개씩 반환)
  <img width="725" alt="스크린샷 2022-12-28 16 26 56" src="https://user-images.githubusercontent.com/100397903/209774950-8317b95b-4b5f-4111-8f4d-c58a4d88a9b2.png">
  
  - buyer - 카테고리별 정렬된 메뉴 조회(평점, 주문 수, 추천메뉴)
  <img width="724" alt="스크린샷 2022-12-28 16 28 39" src="https://user-images.githubusercontent.com/100397903/209775155-adbe8c45-52d1-439f-bd2d-8ba7df8de7a2.png">

  - buyer - 주문하기(주문이 완료되면 해당 메뉴의 orderedcount + 1, limit - 1이 이루어지고, limit이 0인 상태로 주문을 하게 되면 abort된다.)
  <img width="716" alt="스크린샷 2022-12-28 16 30 37" src="https://user-images.githubusercontent.com/100397903/209775380-ef9052b3-e071-4806-a6c2-525ae555af18.png">

  - buyer - 주문에 메뉴 추가하기(주문의 상태가 배달중/배달완료일 경우 새 주문으로 추가됨)
  <img width="718" alt="스크린샷 2022-12-28 17 27 55" src="https://user-images.githubusercontent.com/100397903/209782481-4ae3e50f-1a2b-4fec-a80d-4aefcce9e9fa.png">
  <img width="726" alt="스크린샷 2022-12-28 17 30 04" src="https://user-images.githubusercontent.com/100397903/209782682-65019cf7-a9ab-4262-9a19-e88b1691175c.png">

  - buyer - 주문의 현재 상태 가져오기
  <img width="978" alt="스크린샷 2022-12-28 17 33 33" src="https://user-images.githubusercontent.com/100397903/209783145-411e12c7-c5a7-4777-8414-028cb7e23134.png">

  - buyer - 주문에서 메뉴 변경하기(주문의 상태가 배달중/배달완료일 경우 변경 불가 메시지 반환)
  <img width="719" alt="스크린샷 2022-12-28 17 36 36" src="https://user-images.githubusercontent.com/100397903/209783518-59aac0f5-b547-44bc-b388-a3d70d8fb04f.png">
  <img width="952" alt="스크린샷 2022-12-28 17 35 59" src="https://user-images.githubusercontent.com/100397903/209783551-efdd6de8-5fb4-48b6-9c31-cf1bc0e51b23.png">

  - buyer - 리뷰 작성하기(이미 후기가 작성되었을 경우 에러 반환), body에 포함된 score는 메뉴의 평점에 반영된다.
  <img width="725" alt="스크린샷 2022-12-28 17 39 26" src="https://user-images.githubusercontent.com/100397903/209783879-155e78ac-f6cc-447c-ba83-5067fa8d2549.png">
    <img width="725" alt="스크린샷 2022-12-28 17 40 13" src="https://user-images.githubusercontent.com/100397903/209783961-fadc7bc9-728e-46ad-9a95-dc0d0fee2d21.png">

  - buyer - 메뉴별 리뷰 확인하기(평점도 함께 반환, 리뷰가 없다면 에러반환)
  <img width="958" alt="스크린샷 2022-12-28 18 28 26" src="https://user-images.githubusercontent.com/100397903/209790149-a8da3382-00e7-4db2-8e43-6c49c5098391.png">
<img width="1087" alt="스크린샷 2022-12-28 18 30 23" src="https://user-images.githubusercontent.com/100397903/209790432-fa194221-2b54-485c-966d-e8cfb936ed34.png">


### 코드 구성
코드 구성에 대한 정보는 우측 페이지에서 확인할 수 있다.
https://quilt-stoplight-9b6.notion.site/Docs-0c6140c809904bdcb46a41fbbea48369


# 피드백 반영 개선사항

  
## 피드백
<img width="608" alt="스크린샷 2023-01-03 15 57 25" src="https://user-images.githubusercontent.com/100397903/210312027-5b390114-d593-40e0-a64f-580c652e046a.png">
<img width="610" alt="스크린샷 2023-01-03 15 57 37" src="https://user-images.githubusercontent.com/100397903/210312054-a3698710-97bf-4cb5-9134-edead7f68b24.png">
  
## buyer - 주문하기
  - 메뉴를 주문, 추가, 변경할 때 수량을 기입할 수 있도록 수정
  - 주문과 관련하여 메뉴 추가, 변경, 새 주문시 각 메뉴의 현재 주문 가능한 Limit을 확인하고 그 이상일 경우 abort하는 로직 전체적으로 추가
  - IsReviewed, Status 값을 각각 false, “주문접수” 로 default 설정을 해 주어 주문시 payload에서 제외
    <img width="744" alt="스크린샷 2023-01-03 15 04 37" src="https://user-images.githubusercontent.com/100397903/210307021-26603a23-af7a-4e03-8ddb-ca3afc14458a.png">
    <img width="723" alt="스크린샷 2023-01-03 15 07 45" src="https://user-images.githubusercontent.com/100397903/210307267-a8511d50-b27f-41d4-a5f5-1d5a84b948f3.png">


## buyer - 주문에 메뉴 추가시 중복 체크
  - 주문에 메뉴를 추가할 경우, 해당 주문에 이미 같은 메뉴가 있는지 확인하는 로직을 추가
    <img width="735" alt="스크린샷 2023-01-03 15 16 59" src="https://user-images.githubusercontent.com/100397903/210308124-3c8799be-f047-4ec9-83a3-86af025ba826.png">
    <img width="725" alt="스크린샷 2023-01-03 15 17 20" src="https://user-images.githubusercontent.com/100397903/210308154-3f73cda5-ce80-44bd-9f8a-7bf7b602b052.png">
    <img width="735" alt="스크린샷 2023-01-03 15 18 23" src="https://user-images.githubusercontent.com/100397903/210308243-bc862bd1-7e22-47a2-a13a-94e326912b2f.png">
    <img width="724" alt="스크린샷 2023-01-03 15 23 32" src="https://user-images.githubusercontent.com/100397903/210308722-8a2c233c-45a1-41eb-8018-9e38f82bb5bd.png">


## buyer - 리뷰 작성시
  - 평점이 5를 초과하면 abort하는 로직 추가
  - 해당 주문의 각 메뉴에 대해 리뷰가 가능하고, 주문의 모든 메뉴에 대한 리뷰가 완료되었을 시, 주문의 전체 리뷰 여부를 true로 업데이트하는 로직 추가
    <img width="730" alt="스크린샷 2023-01-03 15 25 26" src="https://user-images.githubusercontent.com/100397903/210308867-28702b65-67c1-45ae-bd90-0fe198a6a95f.png">
    <img width="727" alt="스크린샷 2023-01-03 15 26 00" src="https://user-images.githubusercontent.com/100397903/210308923-7a6f0ecd-f1d4-4c50-884d-e5f68d649ecf.png">
    <img width="741" alt="스크린샷 2023-01-03 15 26 11" src="https://user-images.githubusercontent.com/100397903/210308946-f477dc4e-4119-4132-9469-14dc220caf3e.png">
    <img width="735" alt="스크린샷 2023-01-03 15 27 29" src="https://user-images.githubusercontent.com/100397903/210309091-3079789d-2c77-42c5-b17a-cc38d7c8e880.png">


## seller - 새로운 메뉴 데이터 추가시
  - OrderedCount, Avg, Suggestion, IsVisible값을 각각 0, 0, false, true로 default 설정을 해 주어 새로운 메뉴 데이터 추가시 payload에서 제외
    <img width="719" alt="스크린샷 2023-01-03 15 30 02" src="https://user-images.githubusercontent.com/100397903/210309335-0717b78b-3885-4c48-969d-e49a2844dd79.png">


## seller - 메뉴 데이터 삭제시
  - 메소드를 POST -> DELETE 로 변경
  - 메뉴 데이터에 IsVisible 멤버변수를 추가하여 삭제시 플래그를 활용하여 visible을 false로 변경 + 데이터를 조회할 때 기본적으로 IsVisible이 true인 데이터만 가져오도록 설정
    <img width="966" alt="스크린샷 2023-01-03 15 39 25" src="https://user-images.githubusercontent.com/100397903/210310262-3a36b8a8-5090-48c2-a464-827c17da885e.png">
    <img width="988" alt="스크린샷 2023-01-03 15 40 36" src="https://user-images.githubusercontent.com/100397903/210310360-0af5c3c7-53ec-4b7f-b641-2e7e63886ff7.png">


