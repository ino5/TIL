# ๐ MERGE๋ฌธ ์จ๋ณด๋ฉด์ ์ดํดํด๋ณด๊ธฐ
merge๋ฌธ ์์ ์ ํ์ ํ๋ก์ ํธํ  ๋๋ ์ผ์๋ ๊ฑฐ ๊ฐ์๋ฐ.. ๋ค์๋ณด๋๊น ๋ญ๊ฐ ํท๊ฐ๋ฆฌ๋ ๋ถ๋ถ์ด ์๋ ๊ฑฐ ๊ฐ์ ์ง์  ์จ๋ณด๊ธฐ๋ก ํ๋ค!!

์ง์  ์จ๋ณด๋ฉด์ ํ์คํธํด๋ณด๋ ์ MERGE๋ฌธ ํท๊ฐ๋ ธ๋์ง ์๊ฒ ๋ค.. ์ ํํ๊ฒ ์ดํดํ์ง ๋ชปํ๊ณ  ์์๋ค.


## ๐ MERGE ์ ๋ฆฌ

์๋์ฒ๋ผ ์ง์  ์จ๋ณด๊ณ  ๋ด๋ฆฐ ์ ๋ฆฌ

```sql
MERGE INTO tb1 a
USING tb2 b
ON (a.col1 = b.col2)
WHEN MATCHED THEN
UPDATE ~
WHEN NOT MATCHED THEN
INSERT ~
```

USING ๋ค์ ๋ถ๋ถ b ํ์ด๋ธ์ row๋ฅผ ํ๋์ฉ ๊บผ๋ด์
ON ์์ b์ ์ปฌ๋ผ ๋ถ๋ถ์ ๋ฃ๋๋ค.
๊ทธ๋ฆฌ๊ณ  ํด๋น ์กฐ๊ฑด์ผ๋ก a ํ์ด๋ธ์ ์กฐํํ๋ค.
์ด ๋ ์กด์ฌํ๋ฉด UPDATEํ๊ณ  ์กด์ฌํ์ง ์์ผ๋ฉด INSERTํ๋ค.

์ฆ, USING์ ์ฌ์ฉํ ํ์ด๋ธ ๋๋ ๋ทฐ์ ๊ฐ์๋งํผ ์คํํ๋ค.

UPDATE๋ INSERT ๊ฐ์๋ b.~ ์ ๊ฐ์ด ๋ฃ์ด์ b ํ์ด๋ธ์์ ๊บผ๋ธ row์ ๋ํ ์ปฌ๋ผ ๊ฐ์ ์ฌ์ฉํ  ์ ์๋ค. 


## ๐ DUAL ์์ 

### empoyyes ํ์ด๋ธ

`SELECT * FROM empployees;`

![](md-images/sql-merge/2022-07-09-15-47-18.png)


### INSERT

```sql
MERGE INTO employees a
USING dual b
ON (a.employee_id = '1000')
WHEN MATCHED THEN
UPDATE SET a.salary = 2601
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id)
VALUES('1000', 'test', 'test', TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK');
```

![](md-images/sql-merge/2022-07-09-15-47-31.png)

### UPDATE

```sql
MERGE INTO employees a
USING dual b
ON (a.employee_id = '1000')
WHEN MATCHED THEN
UPDATE SET a.salary = 2601
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id)
VALUES('1000', 'test', 'test', TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK');
```

![](md-images/sql-merge/2022-07-09-15-47-39.png)


์ ์ฟผ๋ฆฌ๋ฅผ ํ๋ฒ ๋ ์คํํ๋ฉด a.employee_id๊ฐ 1000์ธ ๊ฐ์ด ์กด์ฌํ๋ฏ๋ก UPDATE ์คํ

## ๐ ํ์ด๋ธ ์์ 1 - USING์ ํ์ด๋ธ ์ฌ์ฉ

`SELECT * FROM employees;`

![](md-images/sql-merge/2022-07-09-15-47-51.png)


<br>

`SELECT * FROM departments;`

![](md-images/sql-merge/2022-07-09-15-48-00.png)


<br>

`SELECT COUNT(*) FROM departments;`

![](md-images/sql-merge/2022-07-09-15-48-07.png)


### INSERT
```sql
MERGE INTO employees a
USING (SELECT department_id FROM departments) b
ON (a.department_id = b.department_id AND a.employee_id = '10100')
WHEN MATCHED THEN
UPDATE SET a.first_name = 'update'
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id, a.department_id)
VALUES(b.department_id+10000, 'test2', b.department_id, TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK', 10);
```

![](md-images/sql-merge/2022-07-09-15-48-16.png)

![](md-images/sql-merge/2022-07-09-15-48-20.png)

### UPDATE

```sql
MERGE INTO employees a
USING (SELECT department_id FROM departments) b
ON (a.department_id = b.department_id AND a.employee_id = '10100')
WHEN MATCHED THEN
UPDATE SET a.first_name = 'update'
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id, a.department_id)
VALUES(b.department_id+100000, 'test2', b.department_id + 100000, TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK', 10);

```

![](md-images/sql-merge/2022-07-09-15-48-27.png)

![](md-images/sql-merge/2022-07-09-15-48-32.png)

`employee_id`๊ฐ 10100์ธ ๊ฒฝ์ฐ์๋ง update

๋๋จธ์ง์ ๊ฒฝ์ฐ์๋ insert


## ๐ ํ์ด๋ธ ์์ 2 - USING์ WHERE ์กฐ๊ฑด ์ถ๊ฐ

์์ ์์ ์ ์ฟผ๋ฆฌ๋ค์ ๋ชจ๋ ๋กค๋ฐฑ ํ ์ด๊ธฐ ์ํ์์ ์คํ

### USING ์ฌ์ฉ ํ์ด๋ธ

`SELECT COUNT(*) FROM departments WHERE department_id >= 100;`

![](md-images/sql-merge/2022-07-09-15-48-39.png)

### INSERT

```sql
MERGE INTO employees a
USING (SELECT department_id FROM departments WHERE department_id >= 100) b
ON (a.department_id = b.department_id AND a.employee_id = '10100')
WHEN MATCHED THEN
UPDATE SET a.first_name = 'update'
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id, a.department_id)
VALUES(b.department_id+10000, 'test2', b.department_id, TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK', 10);
```

![](md-images/sql-merge/2022-07-09-15-48-44.png)


![](md-images/sql-merge/2022-07-09-15-48-49.png)


### UPDATE

```sql
MERGE INTO employees a
USING (SELECT department_id FROM departments WHERE department_id >= 100) b
ON (a.department_id = b.department_id AND a.employee_id = '10100')
WHEN MATCHED THEN
UPDATE SET a.first_name = 'update'
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id, a.department_id)
VALUES(b.department_id+100000, 'test2', b.department_id+10000, TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK', 10);
```

![](md-images/sql-merge/2022-07-09-15-48-59.png)

![](md-images/sql-merge/2022-07-09-15-49-03.png)

USING์ department_id >= 100 ์กฐ๊ฑด์ด ์์ผ๋ฏ๋ก, a.employee_id๊ฐ 10100์ธ row๊ฐ ์กด์ฌํ๋๋ผ๋ ํด๋น row์ department_id๋ 10์ด๋ฏ๋ก ์กฐ๊ฑด์ ๋ง์กฑํ์ง ๋ชปํ๊ฒ ๋๋ค. ๋ฐ๋ผ์ 18๊ฐ ํ ๋ชจ๋ ์๋ก ์ถ๊ฐ๋๋ค.

๋ง์ฝ INSERTํ  ๋ a.department_id๋ฅผ 100์ด์์ผ๋ก ํ๋ค๋ฉด a.eployee_id๊ฐ 10100์ธ row๊ฐ update๋  ๊ฒ์ด๋ค. ๋ค์๊ณผ ๊ฐ์ด ๋ง์ด๋ค.

### INSERT๋ฌธ์์ a.department_id ๊ฐ์ 100์ผ๋ก ํ๊ณ  ์ ์ํ์ ๋ฐ๋ณตํ์ ๋

```sql
MERGE INTO employees a
USING (SELECT department_id FROM departments WHERE department_id >= 100) b
ON (a.department_id = b.department_id AND a.employee_id = '10100')
WHEN MATCHED THEN
UPDATE SET a.first_name = 'update'
WHEN NOT MATCHED THEN
INSERT (a.employee_id, a.last_name, a.email, a.hire_date, a.job_id, a.department_id)
VALUES(b.department_id + 100000, 'test2', b.department_id + 10000, TO_DATE('1999-01-01','YYYY-MM-DD'), 'SH_CLERK', 100);

```

![](md-images/sql-merge/2022-07-09-15-49-10.png)