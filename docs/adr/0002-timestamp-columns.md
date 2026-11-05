# ADR-0002: 생성·수정 시각 기록 방식

- 상태: 채택
- 날짜: 2026-11-04

## 결정
1. 모든 엔티티는 BaseTimeEntity를 상속해 created_at, updated_at 필드를 가진다.
2. 생성·수정 시각 값은 Hibernate의 @CreationTimestamp, @UpdateTimestamp로 자동 기록한다. Spring Data JPA Auditing은 사용하지 않는다.

## 이유

### 1. 모든 테이블에 updated_at 컬럼
- 구현 단계에서 엔티티마다 updated_at 필드의 필요 여부가 바뀔 수 있다. 컬럼 하나를 더하는 비용보다 엔티티마다 해당 필드의 필요 여부를 판단하고, 추후에 없애거나 추가하는 비용이 더 크다.
- 나중에 컬럼을 추가하는 경우 이미 생성된 인스턴스의 updated_at 값을 알 수 있는 방법이 없다.
- 모든 엔티티가 updated_at 값을 갖기 때문에 created_at과 updated_at 값을 모두 갖는 경우와 created_at 값만을 갖는 경우를 구분할 필요가 없다. 상속 클래스 구조가 단순해진다.

### 2. Hibernate 애노테이션
- 설정 클래스 없이 BaseTimeEntity 클래스만 있어도 된다.
- 일반적으로 많이 사용하는 Spring Data JPA의 Auditing(@EnableJpaAuditing, @EntityListeners)의 추가적인 기능인 @CreatedBy와 @LastModifiedBy를 사용하지 않는다.
- 다른 JPA 구현체를 사용할 일이 없기 때문에 Hibernate에 묶이는 것을 걱정하지 않아도 된다.

## 영향
- ERD 명세에서 도메인마다 제각각이었던 created_at, updated_at 포함 여부를 모두 포함으로 변경