### 1. 📌 Nested Table + BULK COLLECT

👉 대량 데이터 빠르게 처리

- BULK COLLECT → 여러 행 한 번에 조회
- 컬렉션 → 배열처럼 데이터 저장

```sql
SELECT * BULK COLLECT INTO v_table FROM students;
```
✔ 특징

- 성능 좋음
- 반복문 최소화
---
### 2. 📌 EXECUTE IMMEDIATE

👉 동적 SQL 실행

```sql
EXECUTE IMMEDIATE 'CREATE TABLE temp(id NUMBER)';
```
✔사용

- 테이블 생성/삭제
- 동적 쿼리 실행
---
### 3. 📌 AUTHID CURRENT_USER

👉 실행자 권한으로 실행

- DEFINER → 만든 사람 권한
- CURRENT_USER → 실행하는 사람 권한
---
### 4. 📌 Trigger (핵심 ⭐)

👉 자동 실행 로직

```sql
BEFORE / AFTER INSERT OR UPDATE OR DELETE
```
---
### 5. 📌 :OLD / :NEW

👉 변경 값 비교

- :OLD → 변경 전
- :NEW → 변경 후
---
### 6. 📌 트리거 주요 역할
✔ 3개만 기억하면 끝

- 1.이력 저장
```sql
INSERT INTO history VALUES(:OLD...);
```
- 2.값 자동 변경
```sql
:NEW.name := UPPER(:NEW.name);
```
- 3.제약 조건 검사
```sql
IF 조건 THEN
    RAISE_APPLICATION_ERROR(...);
END IF;
```
---
### 7. 📌 RAISE_APPLICATION_ERROR

👉 강제 에러 발생
```sql
RAISE_APPLICATION_ERROR(-20001, '에러');
```
---
### 8. 📌 BEFORE vs AFTER

| 구분     | 역할        |
| ------ | --------- |
| BEFORE | 검사 / 값 수정 |
| AFTER  | 로그 / 통계   |


