### 📘 PL/SQL 정리
### 1. 📌 PL/SQL 기본 개념 및 환경 설정
---
🔹 정의

- PL/SQL (Procedural Language/Structured Query Language)
→ SQL에 변수, 조건문, 반복문 등 절차적 기능을 추가한 오라클 확장 언어
---
🔹 환경 설정
| 항목             | 설명           |
| -------------- | ------------ |
| `SERVEROUTPUT` | 실행 결과 출력 활성화 |
| `CREATE USER`  | 사용자 생성       |
| `GRANT`        | 권한 부여        |
| `ALTER USER`   | 비밀번호 변경      |
---
```sql
SET SERVEROUTPUT ON;
```
---
🔹 주석
```sql
-- 한 줄 주석

/* 여러 줄 주석 */
```
---
### 2. 📌 변수와 데이터 타입
---
🔹 변수 선언
- 선언 위치: DECLARE 또는 IS/AS ~BEGIN 사이

- 값 할당: :=
---
```sql
DECLARE
    v_name VARCHAR2(50);
BEGIN
    v_name := '홍길동';
END;
```
---
🔹 특수 데이터 타입
| 타입         | 설명            |
| ---------- | ------------- |
| `%TYPE`    | 특정 컬럼 타입 참조   |
| `%ROWTYPE` | 테이블 한 행 전체 구조 |
---
```sql
DECLARE  
    v_name professors.pname%TYPE;  
    v_row  professors%ROWTYPE;  
BEGIN  
    NULL;  
END;
```
---
### 3. 📌 제어문
🔹 조건문

---
✔ IF문
```sql
IF 조건 THEN  
    실행문;  
ELSIF 조건 THEN  
    실행문;  
ELSE  
    실행문;  
END IF;
```
---
✔ CASE문
```sql
CASE  
    WHEN 조건 THEN 결과  
    WHEN 조건 THEN 결과  
    ELSE 결과  
END CASE;
```
---
🔹 반복문
| 종류      | 설명        |
| ------- | --------- |
| `LOOP`  | 무한 반복     |
| `WHILE` | 조건이 참인 동안 |
| `FOR`   | 범위 반복     |
---
```sql
LOOP  
    EXIT WHEN 조건;  
END LOOP;

WHILE 조건 LOOP  
    실행문;  
END LOOP;

FOR i IN 1..10 LOOP  
    DBMS_OUTPUT.PUT_LINE(i);  
END LOOP;
```
---
🔹 흐름 제어
| 키워드        | 설명         |
| ---------- | ---------- |
| `EXIT`     | 반복문 종료     |
| `CONTINUE` | 다음 반복으로 이동 |
---

### 4. 📌 SQL 연산 및 트랜잭션
🔹 DML 처리

---
```sql
DECLARE  
    v_name VARCHAR2(50);  
BEGIN  
    SELECT name INTO v_name  
    FROM students  
    WHERE id = 1;  
END;
```
---
🔹 결과 확인
```sql
DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT);
```
---
🔹 트랜잭션
| 명령어         | 설명       |
| ----------- | -------- |
| `COMMIT`    | 작업 확정    |
| `ROLLBACK`  | 작업 취소    |
| `SAVEPOINT` | 복구 지점 설정 |

```sql
SAVEPOINT sp1;
ROLLBACK TO sp1;
```
---
### 5. 📌 고급 기능
🔹 커서 (Cursor)

- SELECT 결과를 한 행씩 처리하기 위한 포인터 

✔ 명시적 커서

---
```sql
DECLARE  
    CURSOR c1 IS SELECT name FROM students;  
    v_name students.name%TYPE;  
BEGIN  
    OPEN c1;  
    FETCH c1 INTO v_name;  
    CLOSE c1;  
END;
```
---
✔ CURSOR FOR LOOP
```sql
BEGIN  
    FOR rec IN (SELECT name FROM students) LOOP  
        DBMS_OUTPUT.PUT_LINE(rec.name);  
    END LOOP;  
END;
```
---
🔹 컬렉션 (Nested Table)
```sql
DECLARE  
    TYPE num_table IS TABLE OF NUMBER;  
    v_nums num_table := num_table(1,2,3);  
BEGIN  
    NULL;  
END;
```
---
🔹 함수 vs 프로시저
| 구분  | 함수 (Function)  | 프로시저 (Procedure) |
| --- | -------------- | ---------------- |
| 반환값 | 있음 (RETURN 필수) | 없음               |
| 사용  | SQL 내부 사용 가능   | 실행문에서 호출         |
---

✔ 함수 예시
```sql
CREATE OR REPLACE FUNCTION get_sum(a NUMBER, b NUMBER)  
RETURN NUMBER  
IS  
BEGIN  
    RETURN a + b;  
END;
```
---
🔹 기타 고급 기능

---
| 기능                   | 설명                |
| -------------------- | ----------------- |
| `REF CURSOR`         | 동적 커서             |
| `PIPELINED FUNCTION` | 한 행씩 반환 (메모리 효율적) |
---
```sql
PIPE ROW(데이터);
```
---
### 6. 📌 실전 응용 (학사관리 시스템)
🔹 테이블 구조
| 테이블         | 설명   |
| ----------- | ---- |
| professors  | 교수   |
| students    | 학생   |
| courses     | 강좌   |
| enrollments | 수강신청 |

---
🔹 관계
- 학생 → 수강신청 → 강좌
- 교수 → 강좌
---
🔹 주요 로직
| 기능      | 설명             |
| ------- | -------------- |
| 수강신청 검증 | 학번/강좌 존재 여부 확인 |
| 중복 체크   | 동일 강좌 신청 여부    |
| 인원수 관리  | 신청/취소 시 인원 증감  |
---

✔ 수강신청 예시
```sql
DECLARE  
    v_count NUMBER;  
BEGIN  
    SELECT COUNT(*) INTO v_count  
    FROM enrollments  
    WHERE student_id = 1  
    AND course_id = 101;  

    IF v_count = 0 THEN  
        INSERT INTO enrollments VALUES (1, 101);  

        UPDATE courses  
        SET persons = persons + 1  
        WHERE course_id = 101;  

        COMMIT;  
    ELSE  
        DBMS_OUTPUT.PUT_LINE('이미 신청됨');  
    END IF;  
END;
```
---
🔹 통계 예시
```sql
SELECT dept, COUNT(*)  
FROM students  
GROUP BY dept;
```
---
### 7. 📌 핵심 요약
- PL/SQL = SQL + 프로그래밍
- 변수 선언 → DECLARE / IS ~ BEGIN
- 제어문 → IF / LOOP / FOR
- 트랜잭션 → COMMIT / ROLLBACK
- 커서 → 한 행씩 처리
- 함수 → RETURN 필수
- 프로시저 → 로직 실행 중심
---
🚀 한 줄 핵심

👉 PL/SQL = SQL + 변수 + 제어문 + 트랜잭션 + 커서