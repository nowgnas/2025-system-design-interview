## ULID (Universally Unique Lexicographically Sortable Identifier)

- 48비트 타임스탬프 + 80비트 랜덤 값으로 구성된 128비트 ID
- 타임스탬프 덕분에 시간순 정렬이 가능하여 데이터베이스 인덱싱, 로그 정렬에 유리
- Crockford's Base32 인코딩을 사용하여 URL-Safe하며, UUID와 호환
- 분산 환경의 여러 Pod에서 같은 시간에 ID를 생성해도, 80비트 랜덤 값 덕분에 충돌 확률이 매우 낮음 (2^80)
- 중앙 관리 시스템 없이 각 Pod가 독립적으로 고유 ID를 안전하게 생성 가능

---

## snowflake와 pod-index를 이용한 유일키 생성

- snowflake id + eks의 pod-index를 조합하여 유일키를 생성
- 만약 두 pod에서 snowflake id를 생성했을 때 같은 키가 발급되어도 pod-index는 다르기 때문에 유일성 보장
- eks의 apps.kubernetexs.io/pod-index 값을 사용할 수 있음
- 스케일 아웃 되어도 새 pod는 다음 index 값을 가지게 되어 유일성 보장
- 유일키 생성 서비스는 spof 발생 요인이므로 각 pod에서 키를 발급하도록 구성

---

## 유일키는 어떤 곳에 사용될 수 있을까?

- 비회원 식별값
- 멱등성 보장을 위한 이벤트 멱등키
- 회원, 주문 등의 키값
