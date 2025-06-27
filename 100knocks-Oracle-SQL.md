# Oracle SQL 100本ノック - Oracle Live SQL対応（HRスキーマ）

## 1〜10問：SELECT文の基本

### 問題 1：従業員テーブルの全件取得
従業員情報を保持している `HR.EMPLOYEES` テーブルの内容をすべて表示してください。

```sql
SELECT * FROM HR.EMPLOYEES;
```

### 問題 2：特定の列だけを表示
従業員の「名前（FIRST_NAME）」「姓（LAST_NAME）」「メールアドレス（EMAIL）」を一覧表示してください。

```sql
SELECT FIRST_NAME, LAST_NAME, EMAIL FROM HR.EMPLOYEES;
```

### 問題 3：列に別名（エイリアス）をつけて表示
FIRST_NAME を「名」、LAST_NAME を「姓」として表示してください。

```sql
SELECT FIRST_NAME AS "名", LAST_NAME AS "姓" FROM HR.EMPLOYEES;
```

### 問題 4：文字列の連結
名前と姓を空白区切りで連結し、「氏名: 山田 花子」の形式で表示してください。

```sql
SELECT '氏名: ' || FIRST_NAME || ' ' || LAST_NAME AS FULL_NAME FROM HR.EMPLOYEES;
```

### 問題 5：DISTINCTの使用  
`HR.EMPLOYEES` テーブルから、**重複のない部署ID（DEPARTMENT_ID）** をすべて表示してください。

```sql
SELECT DISTINCT DEPARTMENT_ID FROM HR.EMPLOYEES;
```

### 問題 6：算術演算で歩合給を計算する  
`SALARY`（給与）と `COMMISSION_PCT`（歩合率）を使って、  
**歩合給（SALARY × COMMISSION_PCT）** を計算し、表示してください。

※`COMMISSION_PCT` に NULL が含まれているため、そのまま算術演算が出来ません。
そのため今回は、`FROM ~~~`を書いたらそれに続くように、
`COMMISSION_PCT IS NOT NULL` を使って、NULLを除外してあげてください。
（NULLを含む算術演算は、後ほど扱います。）

```sql
SELECT FIRST_NAME, SALARY, COMMISSION_PCT,
       SALARY * COMMISSION_PCT AS COMMISSION_PAY
FROM HR.EMPLOYEES
WHERE COMMISSION_PCT IS NOT NULL;
```

### 問題 7：WHERE句による条件指定
給与（SALARY）が 5000以上 の従業員のみを抽出してください。

```sql
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
WHERE SALARY >= 5000;
```

### 問題 8：ORDER BYで並べ替え
給与（SALARY）の高い順に従業員を並べ替えて表示してください。

```sql
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
ORDER BY SALARY DESC;
```

### 問題 9：BETWEENによる範囲指定
給与（SALARY）が 3000以上7000以下 の従業員を抽出してください。

```sql
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
WHERE SALARY BETWEEN 3000 AND 7000;
```

### 問題 10：LIKEによる前方一致検索
メールアドレス（EMAIL）が 「A」で始まる 従業員を抽出してください。

```sql
SELECT FIRST_NAME, EMAIL
FROM HR.EMPLOYEES
WHERE EMAIL LIKE 'A%';
```
