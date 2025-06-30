# Oracle SQL 100本ノック - Oracle Live SQL対応

## はじめに
実行環境は以下のURLにございます。   
[Oracle Live SQL](https://livesql.oracle.com/next/)

この100本ノックでは、Oracle Live SQLの`HRスキーマ`を使ってOracleのSQLを学習していこうという試みです。   
![image](https://github.com/user-attachments/assets/b2b3b01f-cde5-4ce9-8e44-1295dff48f71)

また、内容はなるべくOracleのベンダー資格である、[Oracle Silver SQL試験](https://www.oracle.com/jp/education/certification/certification-exam-list/db-sql-1z0-071-exam/)の内容に準拠したものとなるよう心がけています。

### 問題 0：テーブルの構造を確認する  
一番はじめに、以後の演習で使用する `HRスキーマ`の各テーブルの構造を確認しましょう。  
**`DESC` コマンド**を使うことで、テーブルの列名やデータ型を調べることができます。

以下のコマンドを Live SQL 上で順番に実行して、各テーブルの列構成を把握してください。

<pre><code class="language-sql">
DESC HR.EMPLOYEES;
DESC HR.DEPARTMENTS;
DESC HR.JOBS;
DESC HR.LOCATIONS;
DESC HR.REGIONS;
DESC HR.COUNTRIES;
DESC HR.JOB_HISTORY;
</code></pre>

> Oracle Live SQL では `DESC` は SQLとは異なる「コマンド」なので、**複数行の DESC を一度に実行できない**ことがあります。1つずつ実行しましょう。

---

## 1〜10問：SELECT文の基本

### 問題 1：従業員テーブルの全件取得
従業員情報を保持している `HR.EMPLOYEES` テーブルの内容をすべて表示してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT * FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 2：特定の列だけを表示
従業員の「名前（FIRST_NAME）」「姓（LAST_NAME）」「メールアドレス（EMAIL）」を一覧表示してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, LAST_NAME, EMAIL FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 3：列に別名（エイリアス）をつけて表示
FIRST_NAME を「名」、LAST_NAME を「姓」として表示してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME AS "名", LAST_NAME AS "姓" FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 4：文字列の連結
名前と姓を空白区切りで連結し、「氏名: 山田 花子」の形式で表示してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT '氏名: ' || FIRST_NAME || ' ' || LAST_NAME AS FULL_NAME FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 5：DISTINCTの使用  
`HR.EMPLOYEES` テーブルから、**重複のない部署ID（DEPARTMENT_ID）** をすべて表示してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT DISTINCT DEPARTMENT_ID FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 6：算術演算で歩合給を計算する  
`SALARY`（給与）と `COMMISSION_PCT`（歩合率）を使って、  
**歩合給（SALARY × COMMISSION_PCT）** を計算し、表示してください。

※`COMMISSION_PCT` に NULL が含まれているため、そのまま算術演算が出来ません。
そのため今回は、`FROM ~~~`を書いたらそれに続くように、
`COMMISSION_PCT IS NOT NULL` を使って、NULLを除外してあげてください。
（NULLを含む算術演算は、後ほど扱います。）

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY, COMMISSION_PCT,
       SALARY + (SALARY * COMMISSION_PCT) AS COMMISSION_PAY
FROM HR.EMPLOYEES
WHERE COMMISSION_PCT IS NOT NULL;
</code></pre>

</details>

### 問題 7：WHERE句による条件指定
給与（SALARY）が 5000以上 の従業員のみを抽出してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
WHERE SALARY >= 5000;
</code></pre>

</details>

### 問題 8：ORDER BYで並べ替え
給与（SALARY）の高い順に従業員を並べ替えて表示してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
ORDER BY SALARY DESC;
</code></pre>

</details>

### 問題 9：BETWEENによる範囲指定
給与（SALARY）が 3000以上7000以下 の従業員を抽出してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
WHERE SALARY BETWEEN 3000 AND 7000;
</code></pre>

</details>

### 問題 10：LIKEによる前方一致検索
メールアドレス（EMAIL）が 「A」で始まる 従業員を抽出してください。

<details>
<summary>解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, EMAIL
FROM HR.EMPLOYEES
WHERE EMAIL LIKE 'A%';
</code></pre>

</details>

---

## 11〜20問：WHERE句・演算・NULLの扱い
<<<<<<< HEAD

### 問題 11：給与が6000を超える従業員を抽出する  
`SALARY` が 6000 を超える従業員の名前と給与を表示してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
WHERE SALARY > 6000;
</code></pre>

</details>

### 問題 12：給与が3000以上7000以下の従業員を抽出する  
`SALARY` が 3000〜7000 の範囲にある従業員の名前と給与を表示してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
WHERE SALARY BETWEEN 3000 AND 7000;
</code></pre>

</details>

### 問題 13：メールアドレスが「S」で始まる従業員を表示する  
`EMAIL` が「S」で始まる従業員の名前とメールアドレスを表示してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, EMAIL
FROM HR.EMPLOYEES
WHERE EMAIL LIKE 'S%';
</code></pre>

</details>

### 問題 14：給与の昇順で従業員を並べる  
`SALARY` を昇順に並べて従業員の名前と給与を表示してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
ORDER BY SALARY ASC;
</code></pre>

</details>

### 問題 15：給与の降順で上位5人を表示する  
給与（`SALARY`）の高い順に、上位5人の従業員を表示してください。  
※Oracleでは `ROWNUM` を使います。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY
FROM HR.EMPLOYEES
ORDER BY SALARY DESC
FETCH FIRST 5 ROWS ONLY;
</code></pre>

</details>

### 問題 16：歩合給が設定されている従業員のみを表示する  
`COMMISSION_PCT` が NULL でない従業員を抽出してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY, COMMISSION_PCT
FROM HR.EMPLOYEES
WHERE COMMISSION_PCT IS NOT NULL;
</code></pre>

</details>

### 問題 17：給与と歩合給を使って報酬を計算する  
`SALARY` と `COMMISSION_PCT` を用いて、**歩合給（SALARY × COMMISSION_PCT）** を計算してください。  
NULLは除外してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY, COMMISSION_PCT,
       SALARY * COMMISSION_PCT AS COMMISSION_PAY
FROM HR.EMPLOYEES
WHERE COMMISSION_PCT IS NOT NULL;
</code></pre>

</details>

### 問題 18：MOD関数で給与の千円未満を求める  
`SALARY` を 1000 で割った余りを求め、`MOD` 関数を使って「千円未満の端数」を表示してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY, MOD(SALARY, 1000) AS SALARY_MOD
FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 19：ROUND関数で給与を百円単位に丸める  
`SALARY` を 100 で割ってから `ROUND` で丸めて、給与の百円単位を求めてください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY, ROUND(SALARY / 100, 0) * 100 AS SALARY_ROUNDED
FROM HR.EMPLOYEES;
</code></pre>

</details>

### 問題 20：TRUNC関数で給与の千円単位の切り捨て  
`SALARY` を `TRUNC` 関数で千円単位に切り捨てた値を表示してください。

<details>
<summary>▼ 解答を見る</summary>

<pre><code class="language-sql">
SELECT FIRST_NAME, SALARY, TRUNC(SALARY, -3) AS SALARY_TRUNC
FROM HR.EMPLOYEES;
</code></pre>

</details>

---
