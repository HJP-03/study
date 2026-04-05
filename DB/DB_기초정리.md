# 데이터베이스(Database, DB) 정리

## 1. 데이터베이스란?
여러 사람이 공유할 목적으로 체계화하여 통합·관리하는 데이터의 집합

- 스프레드시트와 유사하지만 컴퓨터 언어로 제어 가능
- 웹/앱을 통해 공유 가능
- 전 세계 어디서든 접근 및 수정 가능

---

## 2. 데이터베이스 특징

| 특징 | 설명 |
|------|------|
| 실시간 접근성 | 실시간 처리 및 응답 가능 |
| 계속적인 변화 | 삽입, 삭제, 갱신으로 항상 최신 상태 유지 |
| 동시 공용 | 여러 사용자가 동시에 데이터 사용 |
| 내용에 의한 참조 | 데이터 값(내용)으로 검색 |

---

## 3. 기본 개념

### 관계형 데이터베이스
여러 개의 테이블이 특정 관계로 구성된 데이터베이스

### 엔티티(Entity)
- 사람, 장소, 사물, 사건 등
- 독립적으로 존재하며 식별 가능

### 엔티티 집합(Entity Set)
- 동일한 속성을 가진 엔티티들의 집합

---

## 4. 스키마(Schema)

데이터베이스 구조 정의

| 구분 | 설명 |
|------|------|
| 외부 스키마 | 사용자 관점 |
| 개념 스키마 | 전체 논리 구조 (1개) |
| 내부 스키마 | 물리적 저장 구조 |

---

## 5. 테이블 용어

| 용어 | 의미 |
|------|------|
| Relation | 테이블 |
| Tuple | 행 (Record) |
| Attribute | 열 (Column) |
| Cardinality | 행 개수 |
| Degree | 열 개수 |

---

## 6. 키(Key)

### 특징
- 유일성: 중복 불가
- 최소성: 최소 속성만 사용

### 종류

| 키 | 설명 |
|----|------|
| 후보키 | 유일성 + 최소성 |
| 기본키 | 선택된 후보키 |
| 슈퍼키 | 유일성만 만족 |
| 외래키 | 다른 테이블 PK 참조 |

### 예시

| 학번 | 주민번호 | 이름 |
|------|----------|------|
| 1 | 1111 | 홍길동 |

- 후보키: 학번, 주민번호
- 기본키: 학번
- 슈퍼키: 학번 + 주민번호

---

## 7. 제약조건

- 기본키는 NULL 불가
- 외래키는 존재하는 값만 참조 가능

---

## 8. 무결성(Integrity)

데이터의 정확성과 일관성 유지

### 무결성 규정 구성

- 규정 이름
- 트리거 조건
- 제약조건(Predicate)
- 위반 시 조치

### 무결성 종류

| 종류 | 설명 |
|------|------|
| 개체 무결성 | PK는 NULL/중복 불가 |
| 참조 무결성 | FK는 존재하는 값 |
| 도메인 무결성 | 값의 범위 제한 |
| 고유 무결성 | 값 중복 금지 |
| NULL 무결성 | NULL 허용 여부 제한 |
| 키 무결성 | 최소 하나의 키 존재 |

---

## 9. 관계 대수

절차적 질의 언어

### 순수 관계 연산자

| 연산 | 설명 |
|------|------|
| SELECT | 행 선택 |
| PROJECT | 열 선택 |
| JOIN | 테이블 결합 |
| DIVISION | 특정 조건 만족 |

### 집합 연산자

- 합집합
- 교집합
- 차집합
- 교차곱

---

## 10. 관계 해석

- 비절차적 언어
- 원하는 결과만 정의
- 종류: 튜플 관계 해석, 도메인 관계 해석

---

## 11. SQL 종류

### DDL (정의어)

| 명령어 | 설명 |
|--------|------|
| CREATE | 생성 |
| ALTER | 수정 |
| RENAME | 이름 변경 |
| TRUNCATE | 전체 삭제 |
| DROP | 삭제 |

※ 특징: 자동 COMMIT (ROLLBACK 불가)

---

### DML (조작어)

| 명령어 | 설명 |
|--------|------|
| SELECT | 조회 |
| INSERT | 삽입 |
| UPDATE | 수정 |
| DELETE | 삭제 |

---

### DCL (제어어)

| 명령어 | 설명 |
|--------|------|
| COMMIT | 저장 |
| ROLLBACK | 복구 |
| GRANT | 권한 부여 |

---

## 12. JOIN

### 개념
여러 테이블을 결합하여 데이터 조회

---

### JOIN 종류

| JOIN | 설명 |
|------|------|
| INNER JOIN | 교집합 |
| LEFT JOIN | 왼쪽 기준 전체 + 일치 |
| RIGHT JOIN | 오른쪽 기준 전체 + 일치 |
| FULL JOIN | 합집합 |
| CROSS JOIN | 모든 경우 |
| SELF JOIN | 자기 자신 |

---

### SQL 예시

```sql
-- INNER JOIN
SELECT A.NAME, B.AGE
FROM EX_TABLE A
INNER JOIN EX_TABLE2 B
ON A.NO_EMP = B.NO_EMP;

-- LEFT JOIN
SELECT A.NAME, B.AGE
FROM EX_TABLE A
LEFT JOIN EX_TABLE2 B
ON A.NO_EMP = B.NO_EMP;

-- RIGHT JOIN
SELECT A.NAME, B.AGE
FROM EX_TABLE A
RIGHT JOIN EX_TABLE2 B
ON A.NO_EMP = B.NO_EMP;

-- FULL JOIN
SELECT A.NAME, B.AGE
FROM EX_TABLE A
FULL JOIN EX_TABLE2 B
ON A.NO_EMP = B.NO_EMP;

-- CROSS JOIN
SELECT A.NAME, B.AGE
FROM EX_TABLE A
CROSS JOIN EX_TABLE2 B;

-- SELF JOIN
SELECT A.NAME, B.AGE
FROM EX_TABLE A, EX_TABLE A2;