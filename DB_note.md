# 使用 DBMS (Using a DBMS)

## 數據庫管理系統概述 (DBMS Overview)

### 數據庫與 DBMS 的區別 (Difference between Database and DBMS)
- **數據庫 (Database)**: 存儲在計算機中的數據集合
- **DBMS (Database Management System)**: 管理數據庫的軟件系統

## DBMS 主要特性 (Main Features of a DBMS)

### 為什麼不使用文件系統？(Why not file systems?)
文件系統不能提供 DBMS 的高級功能，如查詢優化、事務處理和恢復機制。

### 數據庫系統的優勢 (Advantages of a Database System)
1. **快速查詢 (Fast queries)**
   - 例如：在所有帖子中查找由 Bob 發布且包含 "db" 的帖子
   - 通過索引和查詢優化技術實現

2. **事務處理 (Transaction processing)**
   - 將多個修改操作分組為事務，確保要麼全部成功要麼全部失敗
   - 例如：金錢轉帳場景

3. **故障恢復 (Crash recovery)**
   - 所有修改操作被記錄
   - 恢復後沒有數據損壞

### 查詢處理 (Queries)
1. **結構化數據 (Structured data)**
   - 使用表格組織數據
   - 表格包含行（記錄）和列（字段）

2. **查詢執行過程 (Query execution)**
   ```sql
   SELECT p.id, p.text 
   FROM posts AS p, users AS u
   WHERE u.id = p.authorId
   AND u.name='Bob'
   AND p.text ILIKE '%db%';
   ```
   - 生成聯合表 (p, u)
   - 應用 WHERE 條件篩選
   - 選擇指定的列

### 查詢優化 (Query Optimization)
1. **計劃 (Planning)**
   - DBMS 為每個查詢找到最佳的執行計劃樹

2. **索引 (Indexing)**
   - 為列創建搜索樹
   - 大大提高查詢效率

### 事務與 ACID 特性 (Transactions and ACID Properties)

1. **事務概念 (Transaction Concept)**
   - 每個查詢默認放在事務中
   ```sql
   BEGIN;
   SELECT ...; -- query
   COMMIT;
   ```
   - 可以將多個查詢組合成一個事務

2. **事務示例 (Transaction Example)**
   ```sql
   BEGIN;
   UPDATE users 
   SET karma = karma - 10
   WHERE name='Bob';
   
   UPDATE users 
   SET karma = karma + 10
   WHERE name='John';
   COMMIT;
   ```

3. **ACID 保證 (ACID Guarantees)**
   - **原子性 (Atomicity)**: 操作要麼全部生效，要麼全部不生效
   - **一致性 (Consistency)**: 事務完成後數據處於正確狀態
     - 例如: posts.authorId 必須是有效的 users.id
   - **隔離性 (Isolation)**: 並發事務 = 某種順序的串行事務
   - **持久性 (Durability)**: 事務提交後的變更不會丟失（即使系統崩潰）

## 數據模型 (Data Models)

### 數據模型定義 (Data Model Definition)
- 數據模型是描述 DBMS 中數據庫結構的框架

### 常見數據模型 (Common Data Models)
1. **客戶端 (Client side)**
   - 樹狀模型 (Tree model)，如 JSON 格式

2. **服務器端 (Server side)**
   - ER 模型 (Entity-Relationship model)
   - 關係模型 (Relational model)

### 服務器端使用樹狀模型的問題 (Problems with Tree Model at Server Side)
- 空間複雜度：大量冗餘
- 更新速度慢：需要重複更新相同數據

### 服務器端數據建模 (Data Modeling at Server Side)
1. **識別實體類 (Identify entity groups/classes)**
   - 每個類代表數據的"原子"部分

2. **將相同類的實體存儲在表中 (Store entities in tables)**
   - 行/記錄表示一個實體
   - 列/字段表示一個屬性（如"名稱"）

3. **為每個表定義主鍵 (Define primary keys)**
   - 唯一標識實體的特殊列
   - 例如："ID"

### ER 模型 (ER Model)
1. **識別實體之間的關係 (Identify relationships)**
   - 例如：朋友關係 (N-N)，發帖關係 (1-N)

### 關係模型 (Relational Model)
1. **關係作為外鍵 (Relationships as foreign keys)**
   - 1-1 和 1-N 關係直接用外鍵表示
   - N-N 關係需要中間關係表

### 術語回顧 (Terminology Recap)
- 列 = 字段 = 屬性 (Columns = fields = attributes)
- 行 = 記錄 = 元組 (Rows = records = tuples)
- 表 = 關係 (Tables = relations)
- 關係型數據庫：表的集合（≠ 關係型 DBMS）
- 模式 (Schema)：數據庫中表的列定義
  - 基本上是數據庫的"外觀"
  - 關係/表的模式是字段和字段類型

### 為什麼使用 ER 模型？(Why ER Model?)
- 允許以面向對象的方式思考數據
- **實體 (Entity)**: 對象（或類的實例），具有屬性
- **實體組/類 (Entity group/class)**: 類，必須為每個實體定義 ID 屬性
- **實體間關係 (Relationship)**: 引用（"has-a"關係），可以是 1-1、1-N 或 N-N

### 為什麼使用關係模型？(Why Relational Model?)
- 簡化數據管理和查詢處理
- 利用 SQL 查詢中的"任意表連接"
- 表/關係適用於所有類型的實體類
- 主鍵/外鍵適用於實體間的所有類型關係
- 關係模式是邏輯的，而非物理存儲方式

## 學生數據庫練習 (Student DB Exercise)

### 需求 (Requirements)
- 在學校存儲課程-註冊信息
  - 每個系有多名學生和提供不同課程
  - 每個課程可以有多個部分（如 2018 春季，2019 秋季等）
  - 每個學生可以註冊不同的部分

### 解決方案 (Solution)
1. **關係 (表) (Relations/Tables)**
   - 實現實體組或關係
   - 字段/屬性作為列
   - 記錄/元組作為行

2. **主鍵 (Primary Key)**
   - 通過一組字段實現 ID

3. **外鍵 (Foreign Key)**
   - 實現關係
   - 記錄可以指向其他記錄的主鍵
   - 僅適用於 1-1 和 1-N 關係
   - N-N 關係需要中間關係

# SQL 查詢（SQL Queries）

## PostgreSQL 簡介（PostgreSQL Introduction）

- PostgreSQL 是一個強大的關聯型資料庫管理系統（relational database management system）
- 安裝選項：
  - 一般下載安裝（general download and install）
  - Mac 用戶可以使用 PostgreSQL.app 便捷方式

## 使用 PostgreSQL（Using PostgreSQL）

### 基本命令（Basic Commands）
```
$ createdb <db>         # 建立資料庫
$ psql <db> [user]      # 連接到資料庫
> \h or \?              # 取得幫助資訊
> SELECT now();         # 執行 SQL 命令
```

### 語法特性（Syntax Features）
- SQL 敘述可跨多行編寫，以分號（semicolon, `;`）結尾
- 使用雙連字符（double hyphen, `--`）新增註解（comments）
- SQL 指令預設為大小寫不敏感（case insensitive）
  - 使用雙引號（double quotes, `""`）來區分大小寫
  - 例如：`SELECT "authorId" FROM posts;`

## 結構化查詢語言（Structured Query Language, SQL）

### 資料定義語言（Data Definition Language, DDL）
- 用於操作資料結構（schema）的命令：
  - `CREATE TABLE ...` - 建立新的資料表
  - `ALTER TABLE ...` - 修改資料表結構
  - `DROP TABLE ...` - 刪除資料表

### 資料操作語言（Data Manipulation Language, DML）
- 用於操作資料記錄（records）的命令：
  - `INSERT INTO ... VALUES ...` - 新增資料
  - `SELECT ... FROM ... WHERE ...` - 查詢資料
  - `UPDATE ... SET ... WHERE ...` - 更新資料
  - `DELETE FROM ... WHERE ...` - 刪除資料

## 建立資料表/關聯（Creating Tables/Relations）

### 資料欄位類型（Column Types）
- 數值型態：`Integer`, `bigint`, `real`, `double` 等
- 文字型態：`varchar(n)`, `text` 等
- 非空限制（non-null constraint）可防止欄位值為空

### 範例：建立使用者資料表
```sql
CREATE TABLE users (
    id      serial PRIMARY KEY NOT NULL,
    name    varchar(50) NOT NULL,
    Karma   integer NOT NULL
);
```

### 主鍵（Primary Key）
- 特性：
  - 唯一性（unique）- 每筆記錄的主鍵值不得重複
  - 通常使用 `serial` 型態（自動填入的整數）
  - 系統會自動為主鍵建立索引（index automatically created）

### 外部鍵（Foreign Key）
- 範例：文章資料表連結到使用者資料表
```sql
CREATE TABLE posts (
    id          serial PRIMARY KEY NOT NULL,
    text        text NOT NULL,
    "authorId"  integer NOT NULL
        REFERENCES users ON DELETE CASCADE,
    ts          bigint NOT NULL 
        DEFAULT (extract(epoch from now()))
);
```

- 外部鍵特性：
  - `posts.authorId` 必須對應到有效的 `users.id` 值
  - 刪除關聯資料的處理選項：
    - `NO ACTION`（預設）：不允許刪除被參照的資料，會引發錯誤
    - `CASCADE`：當刪除使用者時，自動刪除該使用者的所有文章

## 資料結構示例（Schema Example）

### 三個資料表的關聯結構
- **users** 資料表：儲存使用者基本資訊
  - `id`：使用者編號（主鍵）
  - `name`：使用者名稱
  - `karma`：使用者評分

- **posts** 資料表：儲存文章內容
  - `id`：文章編號（主鍵）
  - `text`：文章內容
  - `authorId`：作者編號（外部鍵連到 users.id）
  - `ts`：時間戳記

- **friend** 資料表：記錄使用者間的好友關係
  - `uId1`：使用者1編號
  - `uId2`：使用者2編號
  - `since`：成為好友的時間

## 查詢操作（Queries）

### 基本查詢
```sql
SELECT * FROM users;  -- 選取 users 資料表的所有欄位和記錄
```

### 聚合函數（Aggregate Functions）
```sql
SELECT COUNT(*) FROM users;   -- 計算 users 資料表的記錄數量
SELECT AVG(karma) FROM users; -- 計算 users 資料表中 karma 欄位的平均值
SELECT MIN(karma) FROM users;  -- 找出 users 資料表中 karma 欄位的最小值
```
- 這些函數常與 `GROUP BY` 子句一起使用進行分組統計

### 進階查詢語法
```sql
SELECT *
FROM users
WHERE id<5 AND name ILIKE '%User%'
ORDER BY id DESC
LIMIT 2;
```

### 執行計畫分析（Query Plan Analysis）
```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE id<5 AND name ILIKE '%User%'
ORDER BY id DESC
LIMIT 2;
```
- 使用 `EXPLAIN ANALYZE` 可查看查詢的執行計畫和實際執行情況

## 資料更新（Updating Rows）

### 批次更新（Batch Update）
```sql
UPDATE users SET karma = karma + 10 WHERE name = 'Bob';
```
- 此操作會更新所有符合 WHERE 條件的資料列（所有名字為 'Bob' 的使用者）

# Java 並發（Java Concurrency）

## 一、創建新執行緒（Starting a New Thread）

在Java中有兩種基本方式可以創建新的執行緒：

### 1. 實現Runnable介面（Implementing Runnable Interface）

```java
public class HelloRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Hello from a thread!");
    }
    
    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }
}
```

### 2. 繼承Thread類（Extending Thread Class）

```java
public class HelloThread extends Thread {
    @Override
    public void run() {
        System.out.println("Hello from a thread!");
    }
    
    public static void main(String args[]) {
        (new HelloThread()).start();
    }
}
```

## 二、執行緒運作機制（What Happened?）

當新執行緒被啟動時：

- 系統為`run()`方法分配新的堆疊（stack），與`main()`方法的堆疊分開
- CPU會平行執行`run()`方法和`main()`方法
  → 這實現了真正的並行處理
  → 在多核心處理器上，這些執行緒可能真正同時運行
  → 在單核心處理器上，作業系統會透過時間片段（time slicing）來模擬並行性

## 三、進程內的記憶體架構（Memory Scheme in a Process）

一個Java進程（Process）的內存佈局包括：

- **共享區域**：
  - 靜態區域（Static）：存放靜態變量
  - 堆（Heap）：存放對象實例
  - 代碼區（Code）：存放程序代碼

- **每個執行緒（Thread）的私有區域**：
  - 寄存器（Register）：存放執行緒運行狀態
  - 堆疊（Stack）：存放方法調用和局部變量

## 四、多執行緒共享堆記憶體（Multiple Stacks, Single Heap）

關於堆記憶體的特性：

- 堆記憶體存儲所有的物件實例（objects）
- 所有執行緒共享同一個堆記憶體
- 執行緒可以訪問相同的物件 → 通過將同一物件傳遞給它們的建構函數：

```java
public static void main(String args[]) {
    Counter counter = ...;
    (new Thread(new HelloRunnableA(counter))).start(); // thread A
    (new Thread(new HelloRunnableB(counter))).start(); // thread B
}
```

## 五、並發訪問問題（Concurrent Access）

當多個執行緒訪問相同物件時可能出現問題：

```java
class Counter {
    private int c = 0;
    public void set(int c) {
        this.c = c;
    }
    public int get() {
        return c;
    }
}
```

執行緒執行下列代碼時：

```java
int c = counter.get();
c++; // 或 c--;
counter.set(c);
```

可能發生以下問題（假設執行緒A增加計數，執行緒B減少計數）：

1. 執行緒A：獲取c的值（假設為0）
2. 執行緒B：獲取c的值（假設為0）
3. 執行緒A：將獲取的值加1；結果為1
4. 執行緒B：將獲取的值減1；結果為-1
5. 執行緒A：將結果設回c；c現在為1
6. 執行緒B：將結果設回c；c現在為-1

→ 執行緒A的操作結果被執行緒B覆蓋，導致一個執行緒的工作丟失
→ 這是典型的競態條件（race condition）問題

## 六、正確位置的同步化（Synchronization at Right Place）

有兩種解決並發訪問問題的方案：

### 方案1：調用者鎖定（Caller Lock）

調用者在整個增加/減少過程中鎖定計數器：

```java
synchronized(counter) {
    int c = counter.get();
    c++; // 或 c--;
    counter.set(c);
}
```

→ 使用synchronized關鍵字建立同步區塊
→ 保證同一時間只有一個執行緒可以執行該代碼區塊

### 方案2：被呼叫者提供原子方法（Callee Provides Atomic Methods）

被呼叫的類自身提供同步方法：

```java
public class SynchronizedCounter {
    private int c = 0;
    public synchronized void increment() {
        c++;
    }
    public int get() {
        return c;
    }
}
```

→ 使用synchronized關鍵字修飾方法
→ 確保方法的執行是原子性的（不可分割的）
→ 基於方法簽名的同步通常更加優雅且封裝性更好

## 七、阻塞與等待狀態（Blocking and Waiting States）

執行緒狀態轉換的重要概念：

- **阻塞（Blocked）**：執行緒在嘗試進入某個已被其他執行緒佔用的同步區域時被阻塞
  → 當前持有該鎖的執行緒退出同步區域後，阻塞的執行緒才能競爭進入

- **等待（Waiting）**：執行緒A在已進入對象o的同步區域後，可以通過調用`o.wait()`進入等待狀態
  → 執行緒A會釋放鎖，允許其他被阻塞的執行緒B進入同步區域
  → 若執行緒B調用`o.notifyAll()`，等待的執行緒A會再次嘗試獲取鎖並繼續執行

## 八、在循環中包裝wait()（Wrap wait() in a Loop）

良好實踐是將`wait()`調用包裝在循環中以防止錯誤：

**生產者執行緒（Threads A, B - 入隊）**:
```java
// enqueue
synchronized(queue) {
    while(queue.size() == 10) {
        queue.wait();
    }
    queue.add(...);
    queue.notifyAll();
}
```

**消費者執行緒（Threads C, D - 出隊）**:
```java
// dequeue
synchronized(queue) {
    while(queue.size() == 0) {
        queue.wait();
    }
    ... = queue.remove();
    queue.notifyAll();
}
```

為什麼使用循環很重要：
- 防止虛假喚醒（spurious wakeup）：執行緒可能在沒有被明確喚醒的情況下恢復執行
- 確保在條件滿足前執行緒不會繼續執行
- 使得代碼更加健壯，能夠處理各種並發情況
- 特別是在生產者-消費者模式中非常重要

## 補充知識

- **執行緒vs進程**：執行緒是進程內的輕量級執行單位，共享進程的資源
- **Java記憶體模型（JMM）**：定義了執行緒如何與主記憶體交互以及變量可見性的規則
- **死鎖（Deadlock）**：當兩個或更多執行緒互相等待對方持有的資源時發生
- **活鎖（Livelock）**：執行緒不斷響應其他執行緒的操作，但無法向前推進
- **原子操作（Atomic Operations）**：Java提供java.util.concurrent.atomic包支持原子操作
- **執行緒安全（Thread Safety）**：代碼在多執行緒環境中正確執行的特性
- **同步集合（Synchronized Collections）**：java.util.concurrent包提供的執行緒安全集合類

# RDBMS 架構與介面 (RDBMS Architecture and Interfaces)

## 1. 關聯式資料庫管理系統 (RDBMS)

- 定義：關聯式資料庫管理系統 (Relational Database Management System) 是支援關聯式模型的資料庫管理系統
- RDBMS 的發展深受 IBM System R 影響
  - System R 於 1974 年發表，成為許多現代 RDBMS 的基礎
  - 建立了 SQL 語言和關聯式查詢處理的標準

## 2. VanillaDB 專案簡介

VanillaDB 包含三個主要組件：
- **VanillaCore** — 單一伺服器上運行的 RDBMS，以 Java 開發
- **VanillaBench** — 常見的 RDBMS 效能測試工具
- **VanillaComm** — 為分散式 RDBMS 提供通訊基礎架構

## 3. RDBMS 系統架構

### 3.1. 整體架構層次

VanillaCore 架構分為多個層級：
1. **客戶端介面層**：JDBC 介面（Client Side）
2. **通訊層**：Remote.JDBC (Client/Server)
3. **查詢處理層**：Query Interface
   - 交易管理 (Tx)
   - 查詢規劃器 (Planner)
   - 語法分析 (Parse)
   - 代數運算 (Algebra)
4. **儲存引擎層**：Storage Interface
   - 並行控制 (Concurrency)
   - 復原機制 (Recovery)
   - 元數據管理 (Metadata)
   - 索引管理 (Index)
   - 記錄管理 (Record)
   - 日誌系統 (Log)
   - 緩衝池 (Buffer)
   - 檔案管理 (File)
5. **輔助工具**：SQL/Util

### 3.2. 主要組件

- **伺服器與基礎設施**：jdbc、sql、tx 和 utils
- **查詢引擎**：處理 SQL 語句的解析、優化和執行
- **儲存引擎**：管理實際資料的儲存和存取

## 4. 查詢介面 (Query Interfaces)

### 4.1. SQL 介面

- SQL (Structured Query Language) 是標準化的資料庫查詢語言
- VanillaCore 支援 SQL-92 的一個小子集：
  - **DDL**：CREATE \<TABLE | VIEW | INDEX\>
  - **DML**：SELECT, UPDATE, INSERT, DELETE
- 限制：
  - 僅支援 int、long、double 和 varchar 資料型別
  - 只支援單一 SELECT-FROM-WHERE 區塊
  - 不支援 * 萬用字元、AS 別名、NULL 值、明確 JOIN 或 OUTER JOIN
  - 僅在 WHERE 子句中支援 AND，不支援括號和計算值
  - 算術表達式僅支援於 UPDATE 語句
  - INSERT 語句不支援子查詢

### 4.2. JDBC 介面

- JDBC (Java Database Connectivity) 定義於 java.sql 套件
- 主要 Java 介面：
  - **Driver**：資料庫驅動程式
  - **Connection**：與資料庫的連線
  - **Statement**：執行 SQL 語句
  - **ResultSet**：查詢結果集
  - **ResultSetMetaData**：結果集的元數據
- VanillaCore 實現於 org.vanilladb.core.remote.jdbc 套件

### 4.3. JDBC 程式設計流程

1. 連接到資料庫伺服器
2. 執行所需查詢
3. 循環處理結果集（僅適用於 SELECT）
4. 關閉連接

重要建議：
- 結果集會占用伺服器資源（如緩衝區和鎖）
- 客戶端應在不再需要資料庫時盡快關閉連接

### 4.4. 原生查詢介面 (Native Interface)

- 伺服器端直接使用原生介面而非 JDBC
- 主要組件：
  - **Transaction**：處理交易管理
  - **Planner**：SQL 執行效率優化
  - **Plan**：查詢計劃樹的節點
  - **Scan**：記錄集的迭代器

## 5. 儲存介面 (Storage Interface)

### 5.1. 檔案、區塊與頁面

- **區塊 (Block)**：OS 一次讀/寫檔案的最小字節序列
  - 隱藏不同設備間磁區 (Sector) 的差異
- **頁面 (Page)**：區塊必須先讀入記憶體頁面才能處理
  - 多次寫入頁面可使用 flush() 系統呼叫一次性寫回檔案

### 5.2. 記錄檔案 (Record File)

- 提供檔案的隨機存取和循序存取方法
- 隨機存取使用 moveToRecordId() 方法
  - RID（記錄識別碼）= BID（區塊識別碼）+ 區塊內偏移量
  - BID = 檔案名稱 + 檔案內偏移量
- RecordFile 本身是記錄集合的迭代器
- 自動處理快取機制、一次讀寫一個區塊
- getVal() 和 setVal() 存取當前區塊對應頁面的當前紀錄
- 呼叫 next() 可能會刷新頁面

### 5.3. 元數據 (Metadata)

- 定義：元數據是關於資料庫的資訊，而非其內容
- VanillaCore 中的元數據類型：
  1. **資料表元數據**：描述每個資料表的檔案和記錄結構（欄位長度、類型、偏移量）
  2. **視圖元數據**：描述每個視圖的屬性（定義、創建者）
  3. **索引元數據**：描述為每個欄位定義的索引
  4. **統計元數據**：描述每個資料表的統計數據，用於估計計劃成本

- 前三種元數據存儲在特殊的目錄資料表中：
  - tblcat.tbl、fldcat.tbl、idxcat.tbl 和 viewcat.tbl
  - 每次創建資料表/視圖/索引時更新
  - 優點：允許像查詢普通數據一樣查詢元數據

- 統計元數據保存在記憶體中並定期更新：
  - 不需要絕對精確
  - 每個計劃樹都會存取，必須非常快速

### 5.4. 元數據管理

- 儲存引擎提供目錄管理器 (CatalogMgr) 和統計管理器 (StatMgr)
- 相關套件：org.vanilladb.core.storage.metadata
- 資料表元數據使用流程：
  1. 創建資料表時，Planner 呼叫 CatalogMgr.createTable(tbln, sch, tx)
  2. 計算並寫入資料表元數據到目錄
  3. 計劃樹最低層的 TablePlan/Scan 通過 CatalogMgr.getTableInfo(tbln, tx) 提取指定資料表的元數據

## 7. 補充知識：資料儲存架構

- **資料庫**：目錄 (Directory)
- **資料表**：檔案 (File)
- **記錄**：位元組 (Bytes)

記錄在檔案系統中的組織：
- 資料庫目錄包含資料表檔案和系統目錄檔案
- 系統目錄包含元數據：tblcat、fldcat、idxcat、viewcat
- 查詢處理時，TableScan 通過 TableInfo 獲取資料表檔案和記錄結構
- 所有檔案存取都通過 RecordFile 介面，而非直接存取檔案系統

# 服務器與線程（Server and Threads）

## 1. 進程、線程與資源管理（Processes, Threads, and Resource Management）

### 1.1 進程與線程（Processes and Threads）
- **進程（Process）** = 線程（至少一個）+ 全局資源
  - 全局資源：記憶體空間/堆（heap）、已開啟的文件等
- **線程（Thread）** = CPU執行單位 + 本地資源
  - 本地資源：程序計數器（program counter）、暫存器（registers）、函數調用堆棧（function call stack）等
- 單線程進程（single-threaded process）與多線程進程（multithreaded process）：
  - 單線程進程：只有一個執行線程
  - 多線程進程：多個線程共享同一進程的代碼、數據和文件資源，但每個線程有自己的堆棧和寄存器

### 1.2 內核線程與用戶線程（Kernel Threads and User Threads）
- **內核線程（Kernel Threads）**：
  - 由操作系統調度
  - 單核機器上：線程輪流使用CPU時間
  - 多核機器上：線程可以並行執行在不同核心
  - 例如：POSIX Pthreads（UNIX）、Win32 threads

- **用戶線程（User Threads）**：
  - 由用戶應用程序在用戶空間調度（內核之上）
  - 輕量級 → 創建/銷毀速度更快
  - 例如：POSIX Pthreads、Java線程
  - 最終映射到內核線程
  
- **用戶線程到內核線程的映射模式**：
  - **多對一（Many-to-One）**：
    - 優點：簡單、高效線程管理
    - 缺點：一個阻塞系統調用會使所有線程停止；無法在多CPU核心上運行
    - 例子：Solaris中的Green threads（現代OS中很少使用）
    
  - **一對一（One-to-One）**：
    - 優點：避免阻塞問題
    - 缺點：線程管理較慢
    - 大多數OS限制一個進程可映射的內核線程數量
    - 例子：Linux和Windows（從95版開始）
    
  - **多對多（Many-to-Many）**：
    - 結合了一對一和多對一的優點
    - 允許為重負載用戶線程分配更多內核線程
    - 例子：IRIX、HP-UX、ru64和Solaris（9版之前）
    - 可降級為一對一

### 1.3 Java線程（Java Threads）
- 由Java虛擬機（JVM）調度
- 映射取決於JVM實現
  - 通常在UNIX/Windows上一對一映射到Pthreads/Win32線程
- 相比POSIX（一對一）線程的優點：
  - 系統獨立（只要有JVM）

## 2. 支持並發客戶端（Supporting Concurrent Clients）

### 2.1 序列化或交錯操作（Serialized or Interleaved Operations）
- 交錯操作通過流水線CPU和I/O來提高吞吐量
- 通過並行執行不同事務（transactions）的操作來減少CPU和I/O閒置時間

### 2.2 線程與進程選擇（Statements Run by Processes or Threads）
- 數據庫管理系統（DBMS）本質上是關於資源管理
- 如果語句由進程運行，需要進程間通信
  - 例如：當兩個語句訪問同一個表（文件）時
  - 系統相關
- 線程允許直接共享全局資源
  - 例如：通過參數傳遞或靜態變量（static variables）

### 2.3 共享資源（What Resources to Share）
- 已開啟的文件（opened files）
- 緩衝區（buffers）（用於緩存頁面）
- 日誌（logs）
- 對象鎖（包括文件/塊/記錄鎖）
- 元數據（metadata）
- 例如：VanillaCore的架構

## 3. 嵌入式客戶端（Embedded Clients）

- 在與RDBMS相同的機器上運行
- 通常是單線程的
  - 例如：傳感器節點、詞典、手機應用等
- 如果需要高吞吐量，自行管理線程
  - 識別語句之間的因果關係
  - 在一個線程中運行每組因果相關的語句
  - 不同組之間的輸出結果沒有因果關係

## 4. 遠程客戶端（Remote Clients）

- 服務器（線程）創建工作線程
- 每個請求一個工作線程
- 每個客戶端可以是多線程的
  - 例如：Web/應用服務器

### 4.1 請求定義（What is a Request）
- 請求可以是：I/O操作、語句、事務或連接

### 4.2 連接為單位的請求（Request = Connection）
- 在VanillaDB中，一個工作線程處理同一用戶發出的所有語句
- 原因：
  - 用戶發出的語句通常有因果順序 → 確保會話中的因果關係
  - 用戶可能會重新檢查已訪問的數據 → 更容易緩存
- 含義：
  - JDBC連接中發出的所有語句由服務器上的單個線程運行
  - 連接數 = 線程數

### 4.3 線程池（Thread Pooling）
- 每次連接/斷開連接時創建/銷毀線程會帶來大量開銷
- 工作線程池可減少這種開銷
  - 需要時從池中分配線程，不再需要時返回池
  - 當池中沒有可用線程時，客戶端可能需要等待
- 其他好處：
  - 通過限制池大小實現優雅的性能降級

## 5. 實現JDBC（Implementing JDBC）

### 5.1 JDBC編程（JDBC Programming）
1. 連接到服務器
2. 執行所需查詢
3. 循環處理結果集（僅SELECT語句）
4. 關閉連接
   - 結果集會佔用服務器上的寶貴資源，如緩衝區和鎖
   - 客戶端應在不再需要數據庫時盡快關閉連接

### 5.2 Java RMI（Remote Method Invocation）
- Java RMI允許在服務器VM上的對象方法被客戶端VM遠程調用
  - 稱為遠程對象（remote object）
- 存根（Stub）和骨架（Skeleton）機制：
  1. 骨架（由服務器線程運行）綁定遠程對象的接口
  2. 客戶端線程查找並獲取骨架的存根
  3. 客戶端調用方法時，被阻塞並將調用轉發給存根
  4. 存根封裝參數並通過網絡將調用發送到骨架
  5. 骨架接收調用，解封參數，從池中分配工作線程代表客戶端運行遠程對象的方法
  6. 方法返回時，工作線程將結果返回給骨架並返回池
  7. 骨架封裝結果並發送給存根
  8. 存根解封結果並繼續客戶端線程

- **RMI註冊表（Registry）**：
  - 服務器必須先將遠程對象的接口綁定到註冊表，並附加名稱
    - 接口必須擴展java.rmi.Remote接口
  - 客戶端在註冊表中查找名稱以獲取存根

- **注意事項**：
  - 客戶端線程與工作線程是同步的
  - 同一個遠程對象由多個工作線程運行（每個客戶端一個）
    - 綁定到註冊表的遠程對象必須是線程安全的
  - 如果遠程方法返回的是另一個遠程對象，會自動創建該對象的存根並發送回客戶端
    - 該對象可以是線程本地的或線程安全的，取決於是在每次方法調用中創建還是重用
  - 如果有客戶端持有其存根，遠程對象不會被垃圾回收
    - 儘快銷毀客戶端側的存根（例如關閉連接）

### 5.3 在VanillaCore中實現JDBC
- JDBC API定義在客戶端
- 需要客戶端和服務器端實現
  - 在org.vanilladb.core.remote.jdbc包中
  - JdbcXxx是客戶端類
  - RemoteXxx是服務器端類
- 基於Java RMI
  - 處理服務器線程：分派線程、工作線程和線程池
  - 但無法控制池大小
  - 同步客戶端線程與工作線程
    - 客戶端的阻塞方法調用

### 5.4 遠程接口與客戶端包裝器（Remote Interfaces and Client-side Wrappers）
- 服務器端JDBC實現：
  - RemoteXxx類鏡像客戶端的JDBC接口
  - 僅實現最基本的JDBC方法
  - 接口：RemoteDriver、RemoteConnection、RemoteStatement、RemoteResultSet和RemoteMetaData
    - 綁定到註冊表
    - 擴展java.rmi.Remote
    - 拋出RemoteException而非SQLException

- 客戶端JDBC實現：
  - 使用存根的客戶端包裝器實現java.sql接口
    - 例如，JdbcDriver包裝RemoteDriver的存根

### 5.5 遠程實現（Remote Implementations）
- **RemoteDriverImpl**：
  - 服務器入口點
  - 每次調用connect方法（通過存根）時，在服務器上創建新的RemoteConnectionImpl
  - 由多個線程運行，必須是線程安全的

- **RemoteConnectionImpl**：
  - 管理服務器上的客戶端連接
  - 與事務（tx）關聯
  - commit()提交當前事務並立即啟動新事務
  - 線程本地（thread local）

- **RemoteStatementImpl**：
  - 執行SQL語句
  - 創建查找最佳計劃樹的規劃器（planner）
  - 如果連接設置為自動提交，executeUpdate()方法會在結束時調用connection.commit()
  - 線程本地

- **RemoteResultSetImpl**：
  - 提供迭代輸出記錄的方法
  - 從最佳計劃樹打開的掃描（scan）
  - 事務貫穿整個迭代
  - 避免在迭代過程中進行重負載工作
  - 線程本地

- **RemoteMetaDataImpl**：
  - 提供關於查詢結果的模式信息
  - 包含輸出表的Schema對象
  - 線程本地

### 5.6 註冊遠程對象（Registering Remote Objects）
- 只需將RemoteDriver綁定到註冊表
  - 其他對象的存根可以通過方法返回獲得
- 通過JdbcStartUp完成：
  ```java
  /* 為服務器在默認端口1099上創建註冊表 */
  Registry reg = LocateRegistry.createRegistry(1099);
  // 在其中發佈服務器條目
  RemoteDriver d = new RemoteDriverImpl();
  /* 為遠程實現對象d創建存根，保存在RMI註冊表中 */
  reg.rebind("vanilladb-jdbc", d);
  ```

### 5.7 獲取存根（Obtaining Stubs）
- 在客戶端獲取存根：
  ```java
  // url = "jdbc:vanilladb://xxx.xxx.xxx.xxx:1099"
  String host = url.replace("jdbc:vanilladb://", "");
  Registry reg = LocateRegistry.getRegistry(host);
  RemoteDriver rdvr = (RemoteDriver)reg.lookup("vanilladb-jdbc");
  // 創建連接
  RemoteConnection rconn = rdvr.connect();
  // 創建語句
  RemoteStatement rstmt = rconn.createStatement();
  ```
- 可直接通過註冊表或間接通過方法返回獲得

### 5.8 啟動（StartUp）
- StartUp提供main()方法，將VanillaCore作為JDBC服務器運行
  - 調用VanillaDB.init()
    - 通過靜態變量共享全局資源
  - 將RemoteDriver綁定到RMI註冊表
    - 每個連接一個線程

### 5.9 引擎中的線程處理（Threading in Engines）
- 一般而言：
  - 查詢引擎（query engine）中的類是線程本地的
  - 存儲引擎（storage engine）中的類是線程安全的

# 查詢處理 (Query Processing)

## 概述 (Overview)

* 查詢處理是資料庫管理系統的核心功能，負責將 SQL 命令轉換為可執行的計劃
* 查詢評估的輸入和輸出:
  * 輸入: SQL 命令（字串）
  * SELECT 輸出: 輸出資料表的掃描器（Scan，記錄迭代器）
  * 其他命令輸出: 受影響的記錄數

* 規劃器（Planner）的主要工作:
  1. 解析 SQL 命令
  2. 驗證 SQL 命令
  3. 為 SQL 命令找出最佳計劃
  4. 返回計劃或執行計劃並返回受影響的記錄數

## SQL 命令解析與驗證 (Parsing and Validating SQL Commands)

### 語法與語意 (Syntax vs. Semantics)

* **語法（Syntax）**:
  * 描述可能有意義語句的規則集合
  * 例如: `SELECT FROM TABLES t1 AND t2 WHERE b - 3` 在語法上不合法
    * SELECT 子句必須引用某些欄位
    * TABLES 不是關鍵字
    * AND 應該分隔謂詞而非資料表
    * b-3 不是謂詞

* **語意（Semantics）**:
  * 指定語法正確字串的實際含義
  * 例如: `SELECT a FROM t1, t2 WHERE b = 3` 在語法上合法
  * 語意上是否合法取決於:
    * a 是否為欄位名稱
    * t1, t2 是否為資料表名稱
    * b 是否為數值型欄位名稱
  * 語意資訊儲存在資料庫的中繼資料（目錄）中

* **VanillaCore 中的語法與語意處理**:
  * 解析器（Parser）基於語法將 SQL 語句轉換為 SQL 資料
    * 語法錯誤會拋出例外
    * 輸出 SQL 資料，如 QueryData、InsertData、ModifyData、CreateTableData 等
  * 驗證器（Verifier）檢查中繼資料來驗證 SQL 資料的語意

### 詞法分析器、解析器和 SQL 資料 (Lexer, Parser, and SQL Data)

* **解析 SQL 命令**:
  * 解析器使用解析演算法將 SQL 字串轉換為 SQL 資料
  * 使用詞法分析器（也稱為 lexer 或 tokenizer）在讀取時將 SQL 字串分割成詞法單元（tokens）

* **詞法單元（Tokens）**:
  * 每個詞法單元有類型和值
  * VanillaCore 詞法分析器支援五種詞法單元類型:
    * 單字元分隔符，如逗號（,）
    * 數字常數，如 123.6（不支援科學記號）
    * 字串常數，如 'netdb'
    * 關鍵字，如 SELECT、FROM 和 WHERE
    * 識別字，如 t1、a 和 b

* **空白字元（Whitespace）**:
  * SQL 命令在空白字元處分割（如空格、製表符、換行符等）
  * 僅有 '...' 內的空白為例外

* **基於串流的 API**:
  * 只讀取 SQL 字串一次
  * `matchXXX` 方法: 返回下一個詞法單元是否為指定類型
  * `eatXXX` 方法: 如果下一個詞法單元是指定類型，則返回其值，否則拋出 BadSyntaxException

* **實作詞法分析器**:
  * Java SE 提供兩種內建標記器:
    * `java.util.StringTokenizer`: 僅支援兩種標記類型（分隔符和單詞）
    * `java.io.StreamTokenizer`: 具有廣泛的標記類型，包括 VanillaCore 使用的所有五種類型
      * 由 VanillaCore 中的 Lexer 封裝

* **Lexer 設置**:
  * StreamTokenizer 配置:
    * `tok.ordinaryChar('.')` 告訴標記器將點字元解釋為分隔符
    * `tok.lowerCaseMode(true)` 告訴標記器將所有字串標記（非引號字串）轉換為小寫

### 語法與解析 (Grammar and Parsing)

* **文法（Grammar）**:
  * 文法是描述詞法單元如何合法組合的規則集合
  * 每個文法規則指定語法類別及其內容
  * 例如:
    ```
    <Field> := IdTok
    <Constant> := StrTok | NumericTok
    <Expression> := <Field> | <Constant>
    <Term> := <Expression> = <Expression>
    <Predicate> := <Term> [ AND <Predicate> ]
    ```
  * 語法類別是文法規則的左側，表示語言中的特定概念
  * 類別的內容是文法規則的右側，是符合規則的字串集合

* **解析樹（Parse Tree）**:
  * 描述字串如何屬於特定語法類別
  * 語法類別作為內部節點，詞法單元作為葉節點
  * 類別節點的子節點對應於文法規則的應用
  * 解析演算法用於驗證給定字串是否在語法上合法

* **解析演算法**:
  * 解析演算法的複雜性通常與支援文法的複雜性成正比
  * VanillaCore 有簡單的 SQL 文法，因此使用最簡單的解析演算法：遞迴下降（recursive descent）

* **遞迴下降解析器（Recursive-Descent Parser）**:
  * 每個文法規則有一個方法，並遞迴調用這些方法以前序遍歷解析樹
  * 前序遍歷允許 SQL 字串只被讀取一次

* **SQL 資料（SQL Data）**:
  * 解析器返回 SQL 資料
  * 例如，解析查詢語句時，解析器會返回 QueryData 物件
  * 所有 SQL 資料都定義在 query.parse 套件中

### 謂詞 (Predicates)

* **謂詞（Predicate）**:
  * 類別定義在 VanillaCore 的 sql.predicates 中
  * 例如：`(gradyear > 2012 OR gradyear <= 2015) AND majorid = did`

* **表達式（Expression）**:
  * VanillaCore 有三種表達式實作:
    * ConstantExpression（常數表達式）
    * FieldNameExpression（欄位名稱表達式）
    * BinaryArithmeticExpression（二元算術表達式）

* **表達式方法**:
  * `evaluate(rec)` 方法: 返回相對於傳遞記錄的表達式值（Constant 類型）
  * `isConstant`, `isFieldName`, `asConstant`, 和 `asFieldName` 方法: 允許客戶端獲取表達式的內容
  * `isApplicableTo` 方法: 告訴規劃器表達式是否只提及指定結構描述中的欄位

* **項（Term）**:
  * Term 支援五種運算符:
    * OP_EQ（=）、OP_LT（<）、OP_LTE（<=）、OP_GE（>）和 OP_GTE（>=）

* **項的方法**:
  * `isSatisfied(rec)` 方法: 如果給定指定記錄，兩個表達式評估為匹配值，則返回 true
  * `oppositeConstant` 方法: 如果此項為形式 "F<OP>C"，則返回常數
  * `oppositeField` 方法: 如果此項為形式 "F1<OP>F2"，則返回欄位名稱
  * `isApplicableTo` 方法: 告訴規劃器此項的兩個表達式是否適用於指定結構描述

* **謂詞方法**:
  * `isSatisfied` 方法: 評估謂詞
  * `conjoinWith` 方法: 解析器處理 WHERE 子句時連接另一個項
  * `selectPredicate` 方法: 返回僅適用於指定結構描述的子謂詞
  * `joinPredicate` 方法: 返回適用於兩個指定結構描述聯合但不單獨適用於任一結構描述的子謂詞
  * `constantRange` 方法: 決定指定欄位是否受此謂詞中的常數範圍約束
  * `joinFields` 方法: 決定是否有形式為 "F1=F2" 的項

### 驗證器 (Verifier)

* **解析器無法確保的事項**:
  * 解析器無法強制類型相容性，因為它不知道它看到的識別字的類型
  * 解析器也無法強制相容的列表大小
  * 例如: `dname = 'math' AND gradyear = sname` 或 `INSERT INTO dept (did, dname) VALUES ('math')`

* **驗證**:
  * 在將 SQL 資料提供給計劃/掃描之前，規劃器要求驗證器驗證資料的語意正確性
  * 驗證器檢查:
    * 提及的資料表和欄位是否實際存在於目錄中
    * 提及的欄位是否不明確
    * 欄位上的操作是否類型正確
    * 所有常數是否對其相應欄位具有正確的類型和大小

## 掃描和計劃 (Scans and Plans)

### SQL 和關係代數 (SQL and Relational Algebra)

* **關係代數轉換**:
  * SQL 命令可以表示為關係代數中的至少一棵樹
  * SQL 難以直接實作，單個 SQL 命令可能包含多個任務
  * 關係代數相對容易實作，每個運算子表示一個小的、定義明確的任務

* **運算子**:
  * 單資料表運算子: select、project、sort、rename、extend、groupby 等
  * 雙資料表運算子: product、join、semijoin 等
  * 運算元: 資料表、視圖、其他運算子的輸出、謂詞等
  * 輸出: 始終是資料表，可返回或用作下一個運算子的參數

### 掃描 (Scans)

* **掃描（Scan）**:
  * 掃描代表關係代數樹中運算子的輸出（即部分查詢的輸出）
  * VanillaCore 中的所有掃描都實作 Scan 介面
  * 定義在 query.algebra 套件中

* **Scan 介面**:
  * 部分查詢輸出記錄的迭代器
  * 與 RecordFile 區分:
    * RecordFile 是資料表檔案中記錄的迭代器
    * 特定於儲存

* **基本掃描**:
  * `SelectScan(Scan s, Predicate pred)`
  * `ProjectScan(Scan s, Collection<String> fieldList)`
  * `ProductScan(Scan s1, Scan s2)`
  * `TableScan(TableInfo ti, Transaction tx)`

* **可更新掃描（Updatable Scans）**:
  * 掃描預設是唯讀的
  * 需要 TableScan 和 SelectScan 可更新以支援 UPDATE 和 DELETE 命令
  * UpdateScan 提供設定器和允許隨機存取（對索引有用）
  * 由 TableScan 和 SelectScan 實作
  * 僅當掃描中的每個記錄 r 在底層資料庫資料表中都有對應記錄 r' 時，掃描才可更新

* **管道掃描 vs. 實體化掃描**:
  * 管道掃描（Pipelined scanning）:
    * 調用節點的方法導致遞迴調用子節點的相同方法
    * 記錄按需計算一次，不保存中間記錄
  * 實體化掃描（Materialized scanning）:
    * 中間記錄具體化為臨時資料表（檔案）
    * 例如，SortScan 可使用外部排序演算法一次排序所有記錄

### 計劃 (Plans)

* **查詢計劃（Query Plan）**:
  * 部分查詢的成本估算器
  * 每個計劃實例對應關係代數中的一個運算子（也對應一個子樹）
  * 計劃（樹）是評估查詢的藍圖
  * 通過僅訪問統計元數據來估計成本，無實際 I/O，僅內存訪問，非常高效
  * 一旦決定了好的計劃，我們就按照藍圖創建掃描

* **開啟掃描樹**:
  * `open()` 方法構建與當前計劃具有相同結構的掃描樹

## 查詢規劃 (Query Planning)

### 決定性規劃器 (Deterministic Planners)

* **查詢規劃**:
  * 輸入: SQL 資料
  * 輸出: 一個好的計劃樹
  * 由規劃器完成

* **VanillaCore 規劃器**:
  * VanillaCore 中的所有規劃器實作都放在 query.planner 套件中
  * 客戶端可以通過調用 `server.VanillaDb.planner()` 獲取 Planner 物件

* **查詢和更新規劃器**:
  * 驗證解析的 SQL 資料後，Planner 將規劃任務委派給:
    * QueryPlanner 
    * UpdatePlanner 
  * 介面定義在 query.planner 套件中

* **決定性查詢規劃演算法**:
  1. 為 FROM 子句中的每個資料表 T 構建計劃
     * 如果 T 是資料表，則計劃是 T 的資料表計劃
     * 如果 T 是視圖，則計劃是對 T 的定義遞迴調用此演算法的結果
  2. 如果需要，取步驟 1 中計劃的積
  3. 如果需要，對 WHERE 子句中的謂詞進行選擇
  4. 對 SELECT 子句中的欄位進行投影

* **邏輯規劃順序（自下而上）**:
  1. 資料表計劃（FROM）
  2. 積計劃（FROM）
  3. 選擇計劃（WHERE）
  4. 分組計劃（GROUP BY）
  5. 投影（SELECT）
  6. 篩選計劃（HAVING）
  7. 排序計劃（ORDER BY）
  * HAVING 和 ORDER BY 子句中提及的欄位必須出現在投影列表中

* **更新規劃**:
  * DDL 和更新命令通常比 SELECT 簡單
    * 單一資料表
    * 僅 WHERE，無 GROUP BY、HAVING、SORT BY 等
  * 決定性規劃演算法通常足夠
  * BasicUpdatePlanner 實作更新的決定性規劃演算法

* **執行修改**:
  * 修改語句由 `executeModify` 方法處理
  * 過程:
    1. 創建資料表計劃
    2. 應用選擇計劃（WHERE 條件）
    3. 開啟並迭代掃描，更新符合條件的記錄
    4. 返回受影響的記錄數

* **執行插入**:
  * 插入語句由 `executeInsert` 方法處理
  * 過程:
    1. 創建資料表計劃
    2. 開啟更新掃描
    3. 插入新記錄並設置欄位值
    4. 返回插入的記錄數（1）

* **DDL 語句方法**:
  * `executeCreateTable` - 創建新資料表
  * `executeCreateView` - 創建新視圖
  * `executeCreateIndex` - 創建新索引
  * 這些方法主要調用目錄管理器來執行相應操作