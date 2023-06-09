# 공연 티켓 예매 서비스

### 프로젝트 개요

사용자들이 공연을 관람하기 위한 티켓을 구매할 수 있는 서비스를 구현합니다. </br>
사용자들은 실시간으로 원하는 좌석을 선택하여 티켓을 예매할 수 있습니다.

### 시스템 요구 사항

- 실시간으로 콘서트 예약 정보를 관리할 수 있어야 한다.
- 시스템은 동시에 여러 사용자의 요청을 처리할 수 있어야 한다.
- 시스템은 대용량 트래픽 상황에서도 안정적으로 작동해야 한다.
- 사용자 인증 및 인가 처리가 가능해야 한다.

### 워크 플로우
![image](https://github.com/BookingSisters/BookingSisters/assets/31529142/3f2d871a-429c-412a-9be7-e0227c61a736)

### 시스템 아키텍처
![image](https://github.com/BookingSisters/BookingSisters/assets/31529142/290fd60c-2a36-4482-b186-af916385d728)

### 시스템 구성 요소
- **API Gateway - REST API**
    - 단일 진입 지점으로 MSA의 서비스 별 API를 경로 기반 라우팅을 통해 통합 관리합니다.
- **Cognito**
    - Cognito로 사용자를 관리하고 google을 통한 회원가입, 로그인을 제공합니다.
    - API Gateway의 인증 제공자로 설정함으로써, 외부로부터의 요청이 Cognito를 통해 검증되도록 구성하였습니다.
- **NLB(Network Loadbalancer)**
    - 등록된 대상을 모니터링하고 정상 대상으로만 트래픽이 라우팅 되도록 하여 고성능 트래픽 분산처리를 담당합니다.
- **ECS - Fargate**
    - 서비스 단위의 컨테이너를 자동으로 스케일링하여 MSA 환경에서 트래픽 변동에 유연하게 대응하도록 설정하였습니다.
- **SQS**
    - 응답 대기 없이 작업을 비동기적으로 처리하기 위해 메시지 브로커(SQS)를 사용하여 통신합니다.
- **Lambda**
    - SQS에 메세지가 push 될 때마다 Lambda 트리거가 활성화되어 즉시 메세지를 처리하도록 합니다. 이 방식은 백프레셔를 효과적으로 관리하고 실시간 처리를 보장합니다.
- **이벤트 브릿지**
    - 이벤트 브릿지를 활용한 스케쥴러 관리로 서버의 제약 없이 글로벌 스케쥴에 따라 작업 처리를 보다 효율적으로 수행하도록 합니다.
- **DynamoDB**
    - 높은 확장성과 처리량으로 대량의 조회 요청을 효율적으로 관리합니다.
- **RDS**
    - 안정적인 트랜잭션 처리로 데이터 일관성을 보장합니다.
 
### 기능 시퀀스 다이어그램
🟢  좌석 예매 성공

![image](https://github.com/BookingSisters/BookingSisters/assets/31529142/7de254f3-6503-4e27-934c-ad9efdf809da)

🔴  좌석 예매 실패 (시간 초과)

![image](https://github.com/BookingSisters/BookingSisters/assets/31529142/34e33d7a-92d7-4b2c-bff1-72a1e4e482f9)


