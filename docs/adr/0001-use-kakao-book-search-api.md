# ADR-0001: 도서 검색 API 선정

- 상태: 채택
- 날짜: 2026-11-04

## 결정

- 다음 책 검색 API(https://developers.kakao.com/docs/ko/daum-search/dev-guide#search-book)
- 추후 필요시 보강용으로 국립중앙도서관 API 병행 검토
- 네이버, 알라딘에서 제공하는 도서 검색 API가 최근 서비스를 종료한 것을 고려하여 추후 국립중앙도서관 API로 변경할 가능성을 두고 API를 변경하기 쉬운 구조로 구현

## 배경
- 도서 검색, 상세 조회 및 리뷰/거래글 작성 시 도서 정보 캐싱에 외부 도서 API가 필요

## 결정 요인

### 1. 비용
  - 무료(쿼터 제한 있음)

### 2. 이용 제한
  - 일간 쿼터 3만건
  - 월간 쿼터 3백만건
  - 링크: https://developers.kakao.com/docs/ko/getting-started/quota

### 3. 제공 정보
  - 프로젝트에서 필요로하는 모든 정보 제공
  - 제공 데이터 목록
    - 도서 제목(title)
    - 도서 소개(contents)
    - 도서 상세 URL(url)
    - ISBN10 or ISBN13(isbn)
    - 도서 출판날짜(datetime)
    - 도서 저자 리스트(authors)
    - 도서 출판사(publisher)
    - 도서 번역자 리스트(translators)
    - 도서 정가(price)
    - 도서 판매가(sale_price)
    - 도서 표지 미리보기 URL(thumbnail)
    - 도서 판매 상태 정보(status)


## 대안

### 국립중앙도서관 API

|| 소장자료 | ISBN 서지정보 |
|---|---|---|
| 비용 | 무료 | 무료 |
| 이용 제한 | 일간 호출 500건 | 없음 |
| 응답 데이터 | 번역자 정보 미제공 | 번역자 정보 미제공 |

- 소장자료 링크: https://www.nl.go.kr/NL/contents/N31101030700.do
- ISBN 서지정보 링크: https://www.nl.go.kr/NL/contents/N31101030500.do


## 영향

- BOOK 테이블의 external_source, external_id 필드 제거 (ISBN을 공통 식별자로 사용)
  - 다음 책 검색 API에서 제공하는 자체 external_id 없음
  - 현재로선 국립중앙도서관 API 도입 가능성 낮음