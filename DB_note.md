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

# 資料存取與檔案管理 (Data Access and File Management)

## 儲存引擎與資料存取 (Storage Engine and Data Access)

### 儲存引擎的主要功能 (Main Functions of Storage Engine)
- **資料存取 (Data Access)**
  - 檔案存取 (File access)：TableInfo、RecordFile
  - 元數據存取 (Metadata access)：CatalogMgr
  - 索引存取 (Index access)：IndexInfo、Index
- **交易管理 (Transaction Management)**
  - 一致性和隔離性 (Consistency and Isolation)：ConcurrencyMgr
  - 原子性和持久性 (Atomicity and Durability)：RecoveryMgr

### 資料存取層 (Data Access Layers) - 由下而上
- **儲存.檔案 (storage.file)**: Page 和 FileMgr
  - 盡可能快速地存取磁碟 (Access disks as fast as possible)
- **儲存.緩衝 (storage.buffer)**: Buffer 和 BufferMgr
  - 緩存頁面 (Cache pages)
  - 與復原管理員協同確保原子性和持久性 (Work with recovery manager for A and D)
- **儲存.記錄 (storage.record)**: RecordPage 和 RecordFile
  - 在頁面中排列記錄 (Arrange records in pages)
  - 釘住/釋放緩衝區 (Pin/unpin buffers)
  - 與復原管理員協同確保原子性和持久性 (Work with recovery manager for A and D)
  - 與並行管理員協同確保一致性和隔離性 (Work with concurrency manager for C and I)
- **索引 (Index)**
- **目錄管理員 (CatalogMgr)**

### 記錄檔案如何映射到實際磁碟檔案 (How RecordFile Maps to Actual Files)
- RecordFile 透過多層映射到實際磁碟上的檔案
- 從 RecordFile → RecordPage → Buffer → Page → 實際檔案塊 (blocks)
- FileMgr 管理實際的磁碟 I/O 操作
- BufferMgr 處理記憶體中的緩存管理

## 磁碟存取 (Disk Access)

### 為什麼需要磁碟? (Why Disks?)
- 資料庫內容必須保存在**持久性儲存裝置 (persistent storages)** 中
  - 確保系統當機時資料不會遺失，確保持久性 (D 特性)
- 記憶體層次結構：CPU → 快取 → 主記憶體 → 大量儲存裝置 (磁碟、磁帶等)

### 速度與成本 (Speed and Cost)
- **主存儲 (Primary storage)** 快但容量小
- **次級儲存 (Secondary storage)** 容量大但速度慢
- 記憶體層次：
  - 頻寬和成本隨著向上層移動而增加
  - 延遲和容量隨著向下層移動而增加

### 速度差異有多大? (How Slow?)
- 存取一個區塊的典型時間：
  - RAM: ~60ns
  - HDD: ~6ms (比 RAM 慢 10萬倍)
  - SSD: ~0.06ms (比 RAM 慢 1000倍)

### 磁碟和檔案管理 (Disk and File Management)
- I/O 操作包括：
  - **讀取 (Read)**: 從磁碟傳輸資料到主記憶體(RAM)
  - **寫入 (Write)**: 從 RAM 傳輸資料到磁碟

### 了解磁性硬碟 (Understanding Magnetic Disks)
- 資料以**磁區 (sectors)** 為單位儲存在磁碟上
- **順序存取 (Sequential access)** 比隨機存取快
  - 磁碟臂移動較慢
- 存取時間 = 尋道時間 (seek time) + 旋轉延遲 (rotational delay) + 傳輸時間 (transfer time)

### 存取延遲 (Access Delay)
- 尋道時間: 1~20ms
- 旋轉延遲: 0~10ms
- 傳輸率約為每 4KB 頁面 1ms
- 尋道時間和旋轉延遲是主要延遲來源

### SSD 特性 (How about SSDs?)
- 隨機存取延遲通常低於 0.1ms
- 順序存取仍可能比隨機存取更快
  - SSD 總是讀/寫整個區塊，即使只需要一小部分
- 如果讀/寫大小與區塊大小相當，性能差異不大

### 作業系統的磁碟存取 API (OS's Disk Access APIs)
- 作業系統提供兩種磁碟存取 API:
  - **區塊級介面 (Block-level interface)**
  - **檔案級介面 (File-level interface)**

## 區塊級介面 (Block-Level Interface)

### 區塊層級抽象 (Block-Level Abstraction)
- 磁碟可能具有不同的硬體特性，特別是不同的磁區大小
- 作業系統使用**區塊 (blocks)** 隱藏磁區
  - 區塊是 OS 之上的 I/O 單位
  - 大小由 OS 決定

### 轉換 (Translation)
- OS 維護區塊和磁區之間的映射
- 單層轉換：OS 將區塊號碼(從 0 開始)轉換為實際磁區地址

### 區塊級介面特性 (Block-Level Interface)
- 無法直接從磁碟存取區塊內容
  - 區塊可能映射到多個磁區
- 必須先將組成區塊的磁區讀入記憶體頁面，然後從中存取
- **頁面 (Page)**: 記憶體中與區塊大小相同的區域

### API
- `readblock(n, p)`: 讀取區塊 n 的內容到記憶體頁面 p
- `writeblock(n, p)`: 將頁面 p 的內容寫入磁碟區塊 n
- `allocate(k, n)`: 找到 k 個連續未使用的磁碟區塊並標記為已使用
- `deallocate(k, n)`: 將從區塊 n 開始的 k 個連續區塊標記為未使用

## 檔案級介面 (File-Level Interface)

### 檔案級抽象 (File-Level Abstraction)
- OS 提供更高級的磁碟介面，稱為**檔案系統 (file system)**
- 檔案是位元組序列
- 客戶端可從檔案任意位置讀取/寫入任意數量的位元組
- 此級別沒有區塊的概念

### 檔案級介面示例 (File-Level Interface Example)
- Java 中的 `RandomAccessFile` 類
- 在偏移量 700 處增加儲存在檔案 "file1" 中的 4 個位元組：
  ```java
  RandomAccessFile f = new RandomAccessFile("file1", "rws");
  f.seek(700);
  int n = f.readInt(); // 讀取後指針移至704
  f.seek(700);
  f.writeInt(n + 1);
  f.close();
  ```

### 區塊存取? (Block Access?)
- 是的！
  - "s" 模式有什麼含義？(同步寫入磁碟)
- OS 隱藏用於檔案 I/O 的頁面 (I/O 緩衝區)
- OS 也隱藏檔案的區塊

### 檔案的隱藏區塊 (Hidden Blocks of a File)
- OS 將檔案視為**邏輯區塊 (logical blocks)** 序列
  - 例如，如果區塊為 4096 位元組
  - 第 700 位元組位於邏輯區塊 0
  - 第 7992 位元組位於邏輯區塊 1
- 邏輯區塊 ≠ 實體區塊 (格式化磁碟)

### 檔案配置方式 (File Allocation Methods)

#### 連續配置 (Continuous Allocation)
- 在連續實體區塊中儲存每個檔案
- 缺點：
  - 內部碎片 (Internal fragmentation)
  - 外部碎片 (External fragmentation)

#### 範圍配置 (Extent-Based Allocation)
- 將檔案儲存為固定長度的**範圍 (extents)** 序列
- 範圍是實體區塊的連續塊
- 只能緩解外部碎片問題，但未完全解決

#### 索引配置 (Indexed Allocation)
- 為每個檔案保留特殊的**索引區塊 (index block)**
- 記錄分配給檔案的實體區塊

### 轉換 (Translation)
- OS 維護邏輯區塊和實體區塊之間的映射
  - 特定於檔案系統實現
- 調用 `seek` 時的層次轉換：
  - 層 1: 位元組位置 → 邏輯區塊
  - 層 2: 邏輯區塊 → 實體區塊
  - 層 3: 實體區塊 → 磁區

## VanillaCore中的檔案管理 (File Management in VanillaCore)

### VanillaCore的選擇 (VanillaCore's Choice)
- 折衷策略：檔案級，但直接存取邏輯區塊
- 優點：
  - 簡單
  - 可管理區塊內的局部性
  - 可管理刷新時間 (為確保正確性)
- 缺點：
  - 需要始終假設隨機磁碟存取
  - 甚至在順序掃描時也是如此
- 快速 → 最小化 I/O 次數
- 許多 DBMS 也採用此方法（如 Microsoft Access, Oracle 等）

### 檔案 (Files)
- VanillaCore 資料庫儲存在資料庫目錄下的多個檔案中
  - 每個表和索引一個檔案
    - 包括目錄檔案 (如 xxx.tbl, tblcat.tbl)
  - 日誌檔案 (如 vanilladb.log)

### 區塊ID (BlockId)
- 不可變 (Immutable)
- 標識特定邏輯區塊
  - 檔案名 + 邏輯區塊號
- 例如：`BlockId blk = new BlockId("std.tbl", 23);`

### 頁面 (Page)
- 保存區塊的內容
  - 由 OS 中的 I/O 緩衝區支持
- 未綁定到特定區塊
- 一次讀取/寫入/附加整個區塊
- 設置的值在 `write()` 前不會刷新到磁碟

### 檔案管理員 (FileMgr)
- 單例，由所有 Page 實例共享
- 處理實際的 I/O
- 保持資料庫所有已打開的檔案
  - 每個檔案只打開一次，由所有工作線程共享

### 使用 VanillaCore 檔案管理員示例
```java
VanillaDb.initFileMgr("studentdb");
FileMgr fm = VanillaDb.fileMgr();

BlockId blk1 = new BlockId("student.tbl", 0);
Page p1 = new Page();
p1.read(blk1);
Constant sid = p1.getVal(34, Type.INTEGER);
Type snameType = Type.VARCHAR(20);
Constant sname = p1.getVal(38, snameType);
System.out.println("student " + sid + " is " + sname);

Page p2 = new Page();
p2.setVal(34, new IntegerConstant(25));
Constant newName = new VarcharConstant("Rob").castTo(snameType);
p2.setVal(38, newName);
BlockId blk2 = p2.append("student.tbl");
```

### I/O 介面 (I/O Interfaces)
- 在 VanillaCore 和 JVM/OS 之間
- 兩種選擇 (都在檔案級)：
  - **Java 新 I/O (Java New I/O)**
  - **Jaydio (O_Direct, 僅 Linux)**
- 通過更改 vanilladb.properties 檔案中 USING_O_Direct 屬性值來切換實現

#### Java 新 I/O (Java New I/O)
- 每個頁面包裝一個 ByteBuffer 實例來存儲位元組
- ByteBuffer 有兩個工廠方法：allocate 和 allocateDirect
  - allocateDirect 告訴 JVM 使用 OS 的 I/O 緩衝區來保存位元組
  - 不在 Java 可編程緩衝區中，沒有垃圾回收
  - 消除了雙重緩衝的冗餘

#### Jaydio
- 提供類似於 Java 新 I/O 的介面
- 具有 O_Direct 功能
  - 某些檔案系統(在 Linux 上)將檔案頁面緩存在其緩衝區中以提高性能
  - O_Direct 告訴這些檔案系統不要緩存檔案頁面，因為我們將實現自己的緩存策略
  - 僅在 Linux 上可用

# 記憶體管理 (Memory Management)

## 概述 (Overview)

* 資料庫系統需要有效的記憶體管理策略，以最小化慢速I/O操作的影響
* 記憶體管理的兩個主要部分：
  * 緩衝使用者資料 (Buffering User Data) → 使用緩衝區管理器 (Buffer Manager)
  * 快取日誌記錄 (Caching Logs) → 使用日誌管理器 (Log Manager)

## 緩衝使用者資料 (Buffering User Data)

### 慢速I/O的影響 (Consequences of Slow I/Os)

* 資料庫系統需要架構來最小化I/O操作：
  * 以區塊為單位存取磁碟 (Block access)
  * 自行管理區塊的快取 (Self-managed caching)
  * 選擇耗費最少區塊I/O的計畫

### 儲存存取模式 (Storage Access Patterns)

* 空間局部性 (Spatial locality)：每個客戶端一次專注於少量區塊
  * 例如：產品掃描 (product scan) 一次只需要兩個區塊（左右兩個表）
* 時間局部性 (Temporal locality)：最近使用的區塊很可能在近期再次被使用
  * 例如：目錄 (catalog) 的區塊

### 透過快取最小化磁碟存取 (Minimizing Disk Access by Caching)

* 核心概念：保留一組頁面 (pages) 池，保存最近使用區塊的內容
  * 只有當池中沒有空頁面時才進行區塊的換入/換出
* 好處：
  * 經濟：只需少量記憶體空間
  * 節省讀取：如果請求的區塊已在頁面中
  * 節省寫入：對一個區塊的所有修改只需在換出時寫入一次

### 為何不使用虛擬記憶體 (Why not virtual memory)

* 虛擬記憶體 (Virtual Memory) 問題：
  * 問題1：頁面替換演算法不佳（如FIFO, LRU等）
    * 作業系統不知道哪些區塊將在近期被使用
  * 問題2：無法控制的延遲寫入 (delayed writes)
    * 自動換頁導致系統斷電時髒頁面可能丟失，破壞資料庫的恢復能力和交易的持久性
* 資料庫自行管理頁面的優點：
  * 更好的替換策略，控制換頁，減少I/O
  * 支援元寫入 (meta-writes)，可以寫入日誌以從當機恢復

### 緩衝哪些區塊？(What Blocks to Cache)

* 使用者資料區塊 (數據庫包含目錄) → 由緩衝區管理器 (Buffer Manager) 管理
* 日誌區塊 → 由日誌管理器 (Log Manager) 管理

### 使用者區塊的存取模式 (Access Pattern to User Blocks)

* 隨機區塊讀取和寫入
* 多個執行緒對多個區塊的並行存取
* 可預測的特定區塊存取
  * 每個掃描一次需要特定的區塊
  * 表格掃描每次需要一個區塊，並可以忘記剛讀過的內容

### 緩衝區管理器 (Buffer Manager)

* 為減少I/O，緩衝區管理器分配一組頁面，稱為緩衝池 (buffer pool)
* 頁面應該是由作業系統持有的直接I/O緩衝區
  * 避免虛擬記憶體的換頁
  * 消除雙重緩衝的冗餘
  * 例如：Java中的`ByteBuffer.allocateDirect()`

### 固定區塊 (Pinning Blocks)

* 每個表格掃描一次需要一個區塊
  * 區塊的語意隱藏在關聯的`RecordFile`後面
* `RecordFile`實例告訴記憶體管理器需要哪些區塊
  * 每個執行緒/客戶端一個實例
* 固定區塊的過程：
  1. `RecordFile`要求緩衝區管理器在某個頁面中固定(pin)一個區塊
  2. 客戶端存取頁面內容
  3. 客戶端完成後，告訴緩衝區管理器取消固定(unpin)該區塊
* 換出時，只有包含未固定區塊的頁面可以被換出

### 固定頁面 (Pinning Pages)

* 固定的結果：
  1. 命中 (hit)：不需I/O
  2. 換頁 (swapping)：緩衝池中至少有一個候選頁面持有未固定區塊
     * 如果頁面是髒的 (dirty)，需要將頁面內容寫回磁碟
     * 需根據替換策略選擇候選頁面
     * 然後讀入所需的區塊
  3. 等待 (waiting)：緩衝池中所有頁面都被固定
     * 等待直到其他人取消固定一個頁面

### 緩衝區 (Buffers)

* 緩衝池中的每個頁面需要關聯額外信息：
  * 包含的區塊是否被固定？
  * 包含的區塊是否被修改 (dirty)？
  * 替換策略所需的資訊
* 緩衝區 (buffer) 包裝頁面並持有這些資訊
* 一個區塊可被多個客戶端固定和存取
  * 緩衝區必須是執行緒安全的
  * 資料庫需要其他機制 (並行控制) 來序列化對緩衝區的衝突操作

### 緩衝區替換策略 (Buffer Replacement Strategies)

* 緩衝池中的所有緩衝區一開始都未分配
* 一旦所有緩衝區都被載入，緩衝區管理器必須替換某個候選緩衝區中未固定的區塊
* 最佳候選者？
  * 包含在最長時間內不會被使用的區塊的緩衝區
  * 最大化固定的命中率

#### 緩衝區替換策略的類型

* 由於無法確定未固定緩衝區中區塊的存取模式，需要啟發式方法：
  * 天真 (Naïve) 策略
  * FIFO (First In First Out) 策略
  * LRU (Least Recently Used) 策略
  * 時鐘 (Clock) 策略
* 一些商業系統為不同緩衝類型使用不同啟發式方法
  * 如目錄緩衝區、索引緩衝區、全表掃描的緩衝區等

#### 天真策略 (Naïve Strategy)

* 從頭開始順序遍歷緩衝池，替換第一個遇到的未固定緩衝區
* 容易實現，但有問題：緩衝區使用不均勻，某些緩衝區可能包含過時資料，導致命中率低

#### FIFO策略 (FIFO Strategy)

* 選擇包含最早讀入區塊的緩衝區
  * 每個緩衝區記錄區塊讀入時間
* 未固定緩衝區可以維護在優先佇列中
  * 在O(1)時間內找到目標未固定緩衝區
* 假設：較舊的區塊在未來不太可能被使用
  * 不一定有效，如目錄區塊經常使用

#### LRU策略 (LRU Strategy)

* 選擇包含最近最少使用區塊的緩衝區
  * 每個緩衝區記錄區塊取消固定的時間
* 假設：近期未使用的區塊在近期不太可能被使用
  * 一般有效
  * 避免替換常用頁面
* 但對全表掃描仍不是最佳
* 大多數商業系統使用LRU的簡單增強版本

##### LRU變體

* 在Oracle DBMS中，LRU佇列有兩個邏輯區域：
  * 冷 (Cold) 區域在熱 (Hot) 區域之前
  * 冷：使用LRU；熱：使用FIFO
  * 對於全表掃描：將剛讀取的頁面放入頭部 (LRU端)

#### 時鐘策略 (Clock Strategy)

* 類似天真策略，但總是從上次替換位置開始遍歷
* 盡可能均勻地使用未固定緩衝區，帶有LRU風格
* 容易實現

### 緩衝池大小 (Pool Size)

* 所有當前被客戶端存取的區塊集合稱為工作集 (working set)
* 理想情況下，緩衝池應該大於工作集
  * 否則，可能發生死鎖 (deadlock)

### 死鎖 (Deadlock)

* 當固定時沒有候選緩衝區怎麼辦？
  * 緩衝區管理器讓客戶端等待
  * 當其他人取消固定一個區塊時通知客戶端再次固定
* 死鎖情況：
  * 客戶端A和B都想使用兩個緩衝區，但只剩兩個候選緩衝區
  * 如果他們都獲得了一個緩衝區並嘗試獲取另一個，就會發生死鎖
* 如何檢測死鎖？
  * 沒有緩衝區在異常長時間內變得可用
* 如何處理死鎖？
  * 強制至少一個客戶端先取消固定它持有的所有區塊，然後一個一個重新固定

### 自死鎖 (Self-Deadlock)

* 客戶端固定的區塊多於緩衝池容量
* 發生情況：
  * 緩衝池太小
  * 客戶端惡意（幸運的是，我們自己編寫客戶端/RecordFile）
* 如何處理？
  * 固定大小的緩衝區管理器別無選擇，只能拋出異常
* 緩衝池應足夠大，至少能容納單個客戶端的工作集
* 好的客戶端應節約固定區塊：
  * 完成時立即取消固定區塊
  * 在JDBC中迭代ResultSet後調用close()

## 快取日誌 (Caching Logs)

### 為何需要日誌？(Why logging?)

* 為確保交易的ACID屬性：
  * 原子性 (Atomicity)：交易中的所有操作要麼全部成功(交易提交)，要麼全部失敗(交易回滾)
  * 一致性 (Consistency)：交易前後，數據不會違反任何設定的規則
  * 隔離性 (Isolation)：多個交易可以並行執行，但不會互相干擾
  * 持久性 (Durability)：一旦交易提交，它所做的任何更改永久保存在數據庫中

### 天真的C和I實現 (Naïve C and I)

* 觀察：沒有跨數據庫存取數據的交易
* 為確保C和I，每個交易可以簡單地鎖定整個數據庫
  * 在開始時獲取鎖
  * 在提交或回滾時釋放鎖
* 數據庫內沒有並行交易
  * CPU和I/O之間沒有流水線處理

### 天真的A和D實現 (Naïve A and D)

* D在有緩衝區的情況下？
* 在提交交易前刷新交易的所有髒緩衝區
  * 交易提交後返回DBMS客戶端
* 系統崩潰然後恢復怎麼辦？
  * 為確保A，DBMS需要在啟動時回滾未提交的交易
* 問題：
  * 如何確定哪些交易要回滾？
  * 如何回滾交易所做的所有動作？

### 預寫日誌 (Write-Ahead-Logging, WAL)

* 記錄每個交易修改的日誌
  * 例如：<SETVAL, <TX>, <BLK>, <OFFSET>, <VAL_TYPE>, <OLD_VAL>>
  * 在記憶體中以節省I/O
* 提交交易：
  1. 在刷新緩衝區前將所有相關日誌寫入日誌文件
  2. 刷新後，將<COMMIT, <TX>>日誌寫入日誌文件
* 交換髒緩衝區：
  * 在刷新用戶區塊前必須刷新所有日誌
* 確定回滾交易：沒有COMMIT日誌的交易
* 如何回滾：撤銷已記錄到磁盤的操作，刷新所有受影響的區塊，然後寫入<ROLLBACK, <TX>>日誌

### WAL的假設

* 每個區塊寫入要麼完全成功，要麼完全失敗，即使電源故障
  * 即崩潰後沒有損壞的日誌區塊
  * 現代磁盤通常在斷電時儲存足夠的電力完成正在進行的扇區寫入
  * 如果區塊大小等於扇區大小或使用日誌文件系統（如EXT3/4, NTFS）則有效

### 快取日誌 (Caching Logs)

* 像用戶區塊一樣，日誌文件的區塊也被快取
  * 每個交易操作被記錄到記憶體中
  * 日誌區塊只在以下情況刷新：
    * 交易提交
    * 緩衝區交換
  * 避免過多的I/O

### 日誌區塊是否需要緩衝池？

#### 存取模式比較

* 用戶區塊：
  * 多個文件
  * 隨機讀取、寫入和追加
  * 多個工作執行緒（每個JDBC客戶端一個執行緒）的並行存取
* 日誌區塊：
  * 單一日誌文件
  * 始終由多個工作執行緒追加
  * 始終由單個恢復執行緒在啟動時順序向後讀取

#### 一個緩衝區足夠 (One Buffer Is Enough)

* 對於（順序前向）追加：
  * 所有工作執行緒"固定"同一文件的尾部區塊
  * 只需一個緩衝區
* 對於順序向後讀取：
  * 恢復執行緒"固定"正在讀取的區塊
  * 只有一個恢復執行緒
  * 只需一個緩衝區
* DBMS需要額外的日誌管理器來實現這種專門的日誌區塊記憶體管理策略

### 日誌序列號 (Log Sequence Number, LSN)

* 每個日誌記錄有一個唯一標識符，稱為日誌序列號
  * 通常是區塊ID + 起始位置
* `flush(lsn)`刷新所有LSN不大於lsn的日誌記錄

### 讀取的快取管理 (Cache Management for Read)

* 提供向後迭代日誌記錄的日誌迭代器
* 內部上，迭代器分配一個頁面，始終保存當前日誌記錄所在的區塊
* 最佳：更多頁面不會幫助節省I/O

### 追加的快取管理 (Cache Management for Append)

* 永久分配一個頁面P來保存日誌文件的尾部區塊
* 當調用`append(rec)`時：
  1. 如果P中沒有空間，則將頁面P寫回磁盤並清除其內容
  2. 將新日誌記錄添加到P
* 當調用`flush(lsn)`時：
  1. 如果該日誌記錄在P中，則將P寫入磁盤
  2. 否則，不做任何事情
* 最佳：更多頁面不會幫助節省I/O

## LogMgr in VanillaCore

* 在`storage.log`包中
* 單例模式
* 在系統啟動期間通過`VanillaDb.initFileAndLogMgr(dbname)`構建
* 通過`VanillaDb.logMgr()`獲取
* `append`方法將日誌記錄追加到日誌文件，並返回記錄的LSN
  * 不保證記錄將寫入磁盤
* 客戶端可以通過調用`flush`強制特定日誌記錄及其所有前任寫入磁盤
* VanillaCore簡化了LSN為區塊號
  * 一個區塊中的所有日誌記錄被分配相同的LSN，因此一起刷新

### BasicLogRecord

* 日誌記錄中值的迭代器
* 日誌管理器只實現記憶體管理策略
  * 不理解日誌記錄的內容
  * 是恢復管理器定義日誌記錄的語義

### LogIterator

* 客戶端可以通過調用LogMgr中的`iterator`方法讀取日誌文件中的記錄
  * 返回一個LogIterator實例
* 調用`next`返回從尾部逆序的下一個BasicLogRecord
* 這是恢復管理器希望看到日誌的方式

## BufferMgr in VanillaCore

* 每個交易都有自己的BufferMgr
* 在創建交易時通過`transactionMgr.newTransaction(...)`構建
* 通過`transaction.bufferMgr()`獲取
* 交易的BufferMgr負責跟踪哪些緩衝區被交易固定，在沒有可用緩衝區時讓交易等待
* `flush()`刷新被指定交易修改的每個緩衝區
* `available()`返回持有未固定緩衝區的緩衝區數量

### BufferPoolMgr

* BufferPoolMgr是一個單例對象，對外部世界隱藏在buffer包中
* 它為所有頁面管理緩衝池，並實現時鐘緩衝區替換策略
  * 磁盤存取的細節對客戶端未知

### Buffer

* 包裝頁面並存儲：
  * 持有區塊的ID
  * 固定計數
  * 修改信息
  * 日誌信息
* 支持WAL：
  * `setVal()`需要一個LSN
    * 必須在`LogMgr.append()`之後
  * `flush()`調用`LogMgr.flush(maxLsn)`
    * 由BufferMgr在交換時調用

### PageFormatter

* BufferMgr的`pinNew(fmtr)`方法向文件追加新區塊
* PageFormatter初始化區塊
  * 在定義記錄語義的包中擴展（如`storage.record`和`storage.index.btree`）

# 資料記錄管理（Record Management）

## 概述（Overview）

- 記錄管理（Record Management）是資料庫系統中的核心組件，負責處理資料儲存和檢索
- 位於資料庫系統架構中的儲存層（Storage Layer）內
- 在 VanillaCore 系統中，Record 模組與其他模組如 Concurrency、Recovery、Metadata、Index、Buffer 等協同工作

## 資料存取層級（Data Access Layers）

- 資料庫系統按照層級組織資料：
  - 頂層：記錄檔案（RecordFile）—— 包含記錄頁（RecordPage）和記錄
  - 中層：緩衝區管理（BufferMgr）—— 處理頁面在記憶體中的緩存
  - 底層：檔案管理（FileMgr）—— 處理檔案和區塊的實際儲存

## 記錄檔案的主要責任（Responsibilities of RecordFile）

1. **決定記錄在檔案中的儲存方式**
   - 設計記錄的物理結構和佈局

2. **決定要固定（pin）哪些區塊**
   - 降低對緩衝區存取的成本
   - 管理記錄頁的載入和卸載

3. **與復原和並行管理器協作**
   - 確保交易（Transaction）的 ACID 特性
   - 處理並行控制和復原機制

## 記錄管理器的設計考量（Design Considerations for Record Manager）

### 檔案同質性（File Homogeneity）

#### 同質檔案 vs. 異質檔案（Homogeneous vs. Heterogeneous Files）
- **同質檔案**：所有記錄都來自相同的資料表
  - 優點：簡化單表查詢
- **異質檔案**：包含來自不同資料表的記錄
  - 優點：可以針對連接（join）查詢進行優化
  - 缺點：單表查詢效率較低，非叢集欄位的連接效率降低
  - 適用於具有層次關係的結構

### 記錄跨區段性（Record Spanning）

#### 跨區段記錄 vs. 非跨區段記錄（Spanned vs. Unspanned Records）
- **跨區段記錄**：記錄的值可以跨越多個區塊
  - 優點：不浪費磁碟空間，記錄大小不受區塊大小限制
  - 缺點：讀取一條記錄可能需要多次區塊存取和重構
- **非跨區段記錄**：每條記錄完全存在於一個區塊內
  - 優點：讀取效率高，實現簡單
  - 缺點：可能因為區塊大小限制導致記錄大小受限

### 欄位長度（Field Length）

#### 固定長度 vs. 可變長度欄位（Fixed-Length vs. Variable-Length Fields）
- **固定長度欄位**：為每個欄位分配固定字節數
  - 優點：隨機存取容易實現
  - 缺點：可能浪費空間
- **可變長度欄位**：欄位佔用空間根據實際內容決定
  - 三種實現方式：
    1. 可變長度表示法：節省空間但修改可能導致記錄重排
    2. 索引表示法：將字串值存在單獨位置
    3. 固定長度表示法：每個記錄分配相同空間

### 資料儲存方向（Data Storage Orientation）

#### 列導向 vs. 行導向儲存（Column-Store vs. Row-Store）
- **行導向儲存**：記錄按行順序存儲
  - 優點：單行存取效率高，寫入優化
  - 適用於 OLTP 工作負載
- **列導向儲存**：單列的值連續存儲
  - 優點：列讀取和計算高效（如分組和聚合）
  - 適用於 OLAP 工作負載

### 空間管理（Free Space Management）

#### 空間管理策略
1. **鏈式（Chaining）**：鏈接所有空閒空間
2. **元頁面（Meta-Pages）**：使用特殊頁面追蹤記錄頁使用情況
3. **元檔案（Meta-File）**：使用額外檔案追蹤所有空閒空間位置和大小

## VanillaCore 記錄管理器的實現（Implementation of VanillaCore Record Manager）

### 記錄儲存方式（How Records are Stored）
- 採用**非跨區段記錄**
- 使用**同質檔案**
- 採用**行導向儲存**
- 使用**固定長度欄位**
- 使用**鏈式空閒空間管理**：搜尋時間 O(1)

### 關鍵組件（Key Components）
1. **記錄頁（RecordPage）**：
   - 管理記錄在頁面中的佈局
   - 分割為槽（slot），每個槽首整數為使用標記（0=空，1=使用中）
   - 最小槽大小：4+4+8 位元組（標記、指向下一個被刪除槽的指針）

2. **記錄 ID（RecordId）**：
   - 記錄的唯一標識符
   - 對於固定長度實現，ID 等於槽號

3. **檔案頭頁（FileHeaderPage）**：
   - 管理檔案頭資訊
   - 包含指向已刪除槽鏈的指針和尾部槽

4. **表資訊（Table Information）**：
   - 儲存記錄長度、欄位名稱、類型、長度和偏移量
   - 透過 `storage.metadata.TableInfo` 和 `sql.Schema` 類管理

### 固定區塊策略（Pinning Blocks）
- 每個 RecordFile 實例僅固定兩個頁面：
  - 當前位置對應的 RecordPage
  - FileHeaderPage
- 在 close() 時解除固定（unpin）
  - 這就是為什麼 JDBC 用戶應該盡快關閉 ResultSet

### 事務支援（Transaction Support）
- **一致性（C）和隔離性（I）**：與並行管理器（ConcurrencyManager）協作
  - 所有讀/寫操作必須透過 `concurrencyMgr.read/modifyXxx()` 獲取適當鎖
- **原子性（A）和持久性（D）**：與復原管理器（RecoveryManager）協作
  - 所有值的設定通過 `recoveryMgr.logXxx()` 進行日誌記錄
  - 藉助記憶體管理層中的 WAL（Write-Ahead Logging）實現

# 交易管理 (Transaction Management) 第一部分：併發控制 (Concurrency Control)

## 1. 交易排程 (Schedules)

### 1.1 交易簡介 (Transaction Introduction)
- 交易確保ACID特性（原子性、一致性、隔離性、持久性）
  - **一致性 (Consistency)**: 交易將使資料庫保持一致狀態，滿足所有完整性約束條件
  - **隔離性 (Isolation)**: 交易間的交錯執行應具有與某種序列執行相同的效果

### 1.2 交易排程 (Transaction Schedules)
- **排程 (Schedule)**: 來自一組交易的操作/動作列表
- **序列排程 (Serial Schedule)**: 不同交易的操作沒有交錯執行的排程
- **等價排程 (Equivalent Schedules)**: 執行第一個排程的效果與執行第二個排程的效果相同
- **可序列化排程 (Serializable Schedule)**: 等價於某些交易序列執行的排程

### 1.3 並行交易 (Concurrent Transactions)
- **優點**:
  - 增加吞吐量（通過CPU和I/O管道）
  - 縮短短交易的響應時間
- 交易交錯執行可提高並行性，但需確保可序列化性

## 2. 異常情況 (Anomalies)

### 2.1 衝突操作 (Conflict Operations)
- 如果兩個操作在同一對象上由不同交易執行，且至少一個是寫操作，則它們衝突
- **類型**:
  - 寫-讀衝突 (Write-Read Conflict)
  - 讀-寫衝突 (Read-Write Conflict)
  - 寫-寫衝突 (Write-Write Conflict)
  - 讀-讀不存在衝突 (No Read-Read Conflict)

### 2.2 寫-讀衝突異常 (Write-Read Conflict Anomalies)
- 讀取未提交數據（髒讀）
- 不可恢復的排程
  - 如果T1中止，會導致連鎖回滾問題

### 2.3 讀-寫衝突異常 (Read-Write Conflict Anomalies)
- 不可重複讀問題 (Unrepeatable Reads)
  - 同一交易內的多次讀取獲得不同結果

### 2.4 寫-寫衝突異常 (Write-Write Conflict Anomalies)
- 丟失更新問題 (Lost Updates)
  - 一個交易的寫入被另一個交易覆蓋

### 2.5 避免異常 (Avoiding Anomalies)
- 確保交易間所有衝突操作按相同順序執行
- 確保衝突可序列化性 (Conflict Serializability)
- 也需要確保可恢復性 (Recoverability)
  - 定義：若每個交易T僅在其讀取變更的所有交易提交後才提交，則排程是可恢復的

## 3. 基於鎖的併發控制 (Lock-based Concurrency Control)

### 3.1 鎖和鎖定 (Locks and Locking)
- **鎖 (Lock)**: 長期、交易級別的
- **鎖存器 (Latch)**: 短期、數據結構/算法級別的
- DBMS應只允許可序列化、可恢復的排程

### 3.2 二階段鎖協議 (Two-Phase Locking Protocol, 2PL)
- 定義兩種鎖:
  - 共享鎖 (Shared Lock, S)
  - 排他鎖 (Exclusive Lock, X)
- **階段1：增長階段**
  - 交易必須在讀取（寫入）對象前獲得S（X）鎖
- **階段2：收縮階段**
  - 交易一旦釋放任何鎖，就不能請求額外的鎖
- 確保衝突可序列化性

### 3.3 嚴格二階段鎖 (Strict Two-Phase Locking, S2PL)
- 交易獲取鎖如2PL增長階段，但保持所有鎖直到完成
- 允許嚴格 (Strict) 和可序列化排程
- 避免級聯回滾 (Cascading Rollbacks)
- 仍存在死鎖問題 (Deadlock Problem)

### 3.4 解決死鎖 (Coping with Deadlocks)
- 交易循環等待彼此釋放鎖
- **檢測**: 等待圖 (Waits-for Graph)
- **其他技術**:
  - 超時和回滾 (Timeout & Rollback)：死鎖檢測
  - 等待死亡 (Wait-Die)：死鎖預防
  - 保守鎖定 (Conservative Locking)：死鎖預防

### 3.5 鎖的粒度 (Granularity of Locks)
- 鎖定什麼"對象"？(What "objects" to lock?)
  - 記錄、塊、表/文件 (Records vs. Blocks vs. Tables/Files)
- 鎖定對象的粒度:
  - 細粒度：高並發，高鎖定開銷
  - 粗粒度：低鎖定開銷，低並發

### 3.6 多粒度鎖 (Multiple-Granularity Locks)
- 允許用戶在包含其他對象的對象上設置鎖
- 新增意向鎖 (Intention Locks):
  - 意向共享鎖 (Intention-Shared, IS)
  - 意向排他鎖 (Intention-Exclusive, IX)
  - 共享意向排他鎖 (Shared and Intention-Exclusive, SIX)
- 鎖相容性矩陣決定哪些鎖模式可以同時持有
- 多粒度鎖定要求根據協議規則自根至葉獲取鎖

## 4. 動態數據庫 (Dynamic Databases)

### 4.1 幻象問題 (Phantom)
- 因插入操作造成的幻象 (Phantoms Caused by Insertion)
- 因更新操作造成的幻象 (Phantoms Caused by Update)
- 解決方法:
  - EOF鎖或多粒度鎖 (EOF Locks or Multi-granularity Locks)
  - 索引（或謂詞）鎖定 (Index or Predicate Locking)

### 4.2 隔離級別 (Isolation Levels)
- ANSI/ISO SQL標準定義的隔離級別:
  - **讀未提交 (Read Uncommitted)**: 可能髒讀、不可重複讀、幻象
  - **讀已提交 (Read Committed)**: 無髒讀、可能不可重複讀和幻象
  - **可重複讀 (Repeatable Read)**: 無髒讀、無不可重複讀、可能幻象
  - **可序列化 (Serializable)**: 無髒讀、無不可重複讀、無幻象
- 隔離級別實現:
  - 通過不同的鎖持有策略實現不同隔離級別
  - 交易特性可由SQL指定:
    - 存取模式 (Access Model): READ ONLY 或 READ WRITE
    - 隔離級別 (Isolation Level): 通過 Connection.setTransactionIsolation()

## 5. 元結構 (Meta-structures)

### 5.1 元結構簡介 (Meta-structures Introduction)
- DBMS除用戶感知數據外還維護一些元結構
  - 例如：RecordFile中的FileHeaderPage

### 5.2 元結構的併發控制 (Concurrency Control of Meta-structures)
- 文件頭頁的訪問發生在記錄插入/刪除時
- 文件頭頁鎖可提前釋放 (Early Lock Release)
  - 不會泄露"數據"，不損害隔離性
  - 提高插入/刪除操作的並發性
  - 需要特別注意確保原子性和持久性

## 6. VanillaCore中的併發管理器 (Concurrency Manager in VanillaCore)

### 6.1 併發管理器實現 (Concurrency Manager Implementation)
- 在storage.tx.concurrency包中
- 基於鎖的協議:
  - MGL粒度：文件、塊和記錄
  - S2PL實現
  - 死鎖檢測：時間限制策略
- 支持不同隔離級別的交易同時運行:
  - 可序列化 (Serializable)
  - 可重複讀 (Repeatable Read)
  - 讀已提交 (Read Committed)

### 6.2 實際中的鎖模式 (Lock Modes in Practice)
- 根據操作類型和隔離級別選擇不同鎖模式
- 決定什麼鎖要沿著訪問路徑獲取
- 三種隔離級別的併發管理器:
  - SerializableConcurrencyMgr
  - RepeatableRead1ConcurrencyMgr
  - ReadCommittedConcurrencyMgr

### 6.3 鎖表 (Lock Table)
- 實現鎖的相容性表
- 使用時間限制策略解決死鎖

# 交易管理第二部分：復原（Transaction Management Part II: Recovery）

## 資料庫管理系統中的失敗（Failure in a DBMS）

* 失敗類型：
  * 硬碟損壞（Disk crash）
  * 電源故障（Power outage）
  * 軟體錯誤（Software error）
  * 災難（Disaster），如火災
* 本講義僅考慮以下類型故障：
  * 交易掛起（Transaction hangs）
    * 邏輯掛起（Logical hangs）：例如資料未找到、溢位、錯誤輸入
    * 系統掛起（System hangs）：例如死鎖（deadlock）
  * 系統掛起/當機（System hangs/crashes）
    * 硬體錯誤或使DBMS掛起的軟體錯誤

### 關於失敗的假設（Assumptions about Failure）
* 非揮發性儲存裝置的內容不會損壞
  * 例如通過檔案系統日誌（file-system journaling）保護
* 無拜占庭故障（No Byzantine failure）（無殭屍進程）
* 其他類型的失敗將通過其他方法處理
  * 例如通過複製（replication）、仲裁（quorums）等

## 簡單的原子性與持久性（Naïve A and D）

* 當交易要提交（commit）時：
  * 在提交交易並返回給客戶端前，先將交易的所有髒緩衝區（dirty buffers）刷寫到硬碟
* 系統當機後重啟時：
  * 為確保原子性（Atomicity），DBMS需要在啟動時回滾（rollback）未提交的交易
  * 問題：
    * 如何確定哪些交易需要回滾？
    * 如何回滾交易所做的所有操作？

### 預寫日誌（Write-Ahead-Logging, WAL）方法

* 記錄交易所做的每次修改
  * 例如：`<SETVAL, <TX>, <BLK>, <OFFSET>, <VAL_TYPE>, <OLD_VAL>>`
  * 儲存在記憶體中以節省I/O
* 提交交易時：
  1. 在刷寫任何緩衝區之前，將所有相關日誌寫入日誌檔案
  2. 刷寫後，將`<COMMIT, <TX>>`日誌寫入日誌檔案
* 交換髒緩衝區時（在BufferMgr中）：
  * 在刷寫緩衝區之前，必須先刷寫所有日誌

### 回滾機制（Rollback）

* 確定需要回滾的交易：
  * 觀察：有COMMIT日誌的交易必已將所有髒區塊刷寫到硬碟
  * 回滾沒有COMMIT日誌的交易
* 如何回滾交易：
  * 將日誌記錄的舊值寫回，刷寫所有受影響的區塊，再寫入`<ROLLBACK, <TX>>`日誌
* WAL的假設：即使在電源故障情況下，每個區塊寫入要麼完全成功要麼完全失敗
  * 現代硬碟通常在斷電時有足夠電力完成正在進行的扇區寫入
  * 當區塊大小等於扇區大小或使用日誌檔案系統時，此假設有效

### 日誌快取（Caching Logs）

* 與使用者區塊類似，日誌檔案的區塊也會被快取
  * 每個交易操作都記錄在記憶體中
  * 避免過多I/O
* 日誌區塊只有在以下情況才會刷寫：
  * 交易提交時
  * 刷寫資料緩衝區時

## 與復原相關的系統組件（System Components related to Recovery）

* 日誌管理器（Log Manager）：管理日誌的快取
  * 不理解日誌的語義
* 緩衝區管理器（Buffer Manager）：確保每個刷寫的資料緩衝區都遵循WAL
* 復原管理器（Recovery Manager）：確保原子性和持久性，決定：
  * 記錄什麼（從語義上）
  * 何時刷寫緩衝區（和日誌尾部）
  * 如何回滾交易
  * 如何從當機中恢復資料庫

### 復原管理器的操作（Actions of Recovery Manager）

* 正常交易處理期間的操作：
  * 將日誌記錄添加到快取
  * 在COMMIT時刷寫日誌尾部和緩衝區
  * 或者回滾交易（通過撤銷交易所做的變更）
* 系統重啟後的操作：
  * 將資料庫恢復到一致狀態
  * 撤銷所有未完成交易所做的變更
  * 在專用恢復交易中進行（在所有正常交易啟動前）

## 日誌記錄（Log Records）

* 復原管理器在日誌中保存資訊以便能夠回滾交易
* 每當發生可記錄活動時，復原管理器向日誌快取添加日誌記錄：
  * 開始（Start）
  * 提交（Commit）
  * 回滾（Rollback）
  * 更新記錄（Update record）
  * 檢查點（Checkpoint）

### 為何需要COMMIT/ROLLBACK日誌

* 用於在恢復期間識別未完成的交易
* 未完成交易：例如沒有COMMIT/ROLLBACK日誌在硬碟上的交易

### 刷寫COMMIT

* 提交交易時，必須在返回用戶之前刷寫COMMIT日誌
* 如果系統返回給客戶端但在寫入提交日誌之前當機：
  * 復原管理器會將其視為未完成交易並撤銷其所有變更
  * 危及持久性（Durability）

## 區塊鎖存（Latching on Blocks）

* 修改區塊時：
  1. 獲取該區塊的鎖存（latch）
  2. 記錄更新（在記憶體中，由LogMgr完成）
  3. 執行變更
  4. 釋放鎖存
* 刷寫包含區塊的緩衝區時：
  1. 在pin()後獲取該區塊的鎖存
  2. 刷寫相應的日誌記錄
  3. 刷寫緩衝區
  4. 釋放鎖存
* 鎖存與以下無關：
  * S2PL中的鎖（Locks）
  * BufferMgr中的釘住/釋放（Pinning/unpinning）

## 只撤銷恢復（UNDO-only Recovery）

### 未完成交易的定義

* 在硬碟上的日誌檔案中沒有COMMIT或ROLLBACK記錄的交易
* 當機發生時可能處於以下任何狀態：
  1. 活躍狀態（Active）
  2. 提交中（但尚未完成）
  3. 回滾中（Rolling back）

### 只撤銷恢復算法

* 逐個處理日誌記錄（從尾部向頭部）
* 識別已完成的交易（有COMMIT或ROLLBACK記錄）
* 對於未完成的交易的每個更新記錄，撤銷變更
* 最後刷寫所有髒緩衝區並寫入檢查點記錄

## 強制與非強制（Force vs. No-Force）

* 強制方法：
  * 提交交易時，所有修改需要在返回用戶前寫入硬碟
* 非強制方法：
  * 僅寫入日誌而不刷寫所有髒區塊
  * 優點：更快的提交
  * 問題：已提交交易可能未反映到硬碟上
  * 解決方案：在恢復中添加一個新的重做（REDO）階段

## 撤銷-重做恢復（UNDO-REDO Recovery）

* 對於每個完成的交易執行重做
* 對於每個未完成的交易執行撤銷
* 撤銷和重做操作是冪等的（idempotent）
  * 執行同一撤銷操作多次=執行一次

### 快速回滾（Fast Rollback）

* 非強制策略：
  * 回滾期間不刷寫髒頁
  * 甚至不需要保留ROLLBACK記錄在快取中
* 已中止交易將在啟動恢復期間再次回滾
  * 由於撤銷操作是冪等的，不會損害一致性（C）

### 先撤銷還是先重做？

* 撤銷階段必須在重做階段之前
* 否則，一致性（C）可能會因已中止交易而損壞

## 偷取與非偷取（Steal vs. No Steal）

* 當前，髒緩衝區可以在交易提交前刷寫到硬碟
  * 由於交換（swapping）
  * 稱為偷取方法
* 如果不偷取，則不需要撤銷階段！
  * 只重做恢復
* 實現方法：
  * 將所有修改過的緩衝區釘住直到交易結束？
  * 非偷取方法在實際中不可行

## 在恢復期間再次當機（Failures during Recovery）

* 如果系統在恢復過程中再次當機，可以在重啟後簡單地重新運行恢復（從頭開始）
* 由於每個撤銷/重做是冪等的，這是安全的
* 不需要記錄撤銷/重做操作
  * 對於因撤銷/重做而進行的每個資料修改，復原管理器向緩衝區管理器傳遞-1作為LSN號

## 檢查點（Checkpointing）

* 隨著系統持續處理請求，日誌檔案可能變得非常大
  * 運行恢復過程耗時
  * 能否只讀取部分日誌？
* 檢查點是DBMS狀態的一致快照
  * 所有較早的日誌記錄都由"已完成"的交易寫入
  * 這些交易的修改已刷寫到硬碟
* 恢復時，復原管理器可以忽略檢查點之前的所有日誌記錄

### 靜態檢查點（Quiescent Checkpointing）

1. 停止接受新交易
2. 等待現有交易完成
3. 刷寫所有修改過的緩衝區
4. 將靜態檢查點記錄附加到日誌並刷寫到硬碟
5. 開始接受新交易

### 非靜態檢查點（Nonquiescent Checkpointing）

* 靜態檢查點簡單但可能使系統在檢查點過程中太長時間不可用
* 非靜態檢查點允許系統在檢查點處理期間繼續運行
* 工作機制：
  * 在步驟3和4之間，不允許任何交易附加日誌或修改緩衝區
  * 檢查點交易在步驟3之前獲取日誌檔案的鎖存和BufferMgr中所有區塊的鎖存
  * 在步驟4之後釋放這些鎖存

### 何時進行檢查點？

* 定期進行檢查點可使恢復過程更高效
* 檢查點的好時機：
  * 系統啟動時（恢復完成後且任何交易開始前）
  * 工作負載低的執行時間（例如午夜）

## 邏輯日誌（Logical Logging）

### 提早釋放鎖（Early Lock Release）

* DBMS中存在元結構（meta-structures）
  * 例如，RecordFile中的FileHeaderPage
  * 索引（Indices）
* 如果以嚴格方式鎖定，性能會較差
  * 例如，FileHeaderPage上的S2PL使所有插入和刪除序列化
* 元結構上的鎖通常提早釋放

### 邏輯操作（Logical Operations）

* 向RecordFile進行邏輯插入：
  * 按順序獲取FileHeaderPage和目標對象的鎖
  * 執行插入
  * 釋放FileHeaderPage的鎖（但不釋放對象的鎖）
* 提高並發性
* 不損害一致性
* 需要特別注意以確保原子性和持久性

### 邏輯操作的問題

* 當兩個交易修改同一元結構（如FileHeaderPage）時，如果使用物理撤銷記錄回滾一個交易，可能會丟失另一個交易的修改
* 解決方案：邏輯操作需要邏輯撤銷

### 撤銷部分邏輯操作

* 如果交易在邏輯操作中途中止：
  * 記錄邏輯操作期間執行的每個物理操作
  * 部分邏輯操作可以通過撤銷物理操作來撤銷

## 重複歷史恢復（Recovery by Repeating History）

* 基本思想：
  1. 重複歷史：按照確切發生順序重放所有依賴的物理操作（從最後一個檢查點開始）
     * 包括正在進行的回滾/撤銷
     * 這樣記憶體狀態可以正確重建
  2. 恢復所有未完成交易的回滾
     * 對每個已完成的邏輯操作進行邏輯回滾
* 這導致了最先進的恢復算法ARIES
* 步驟1/2在ARIES中稱為REDO/UNDO階段

### 補償日誌（Compensation Logs）

* 重放歷史包括重放以前的撤銷
* 如何在單一階段（日誌掃描）中重放歷史？
* 當撤銷物理操作時，在LogMgr中添加該撤銷的重做日誌，稱為補償日誌
* 在恢復過程中，RecoveryMgr可以通過重做物理和補償日誌來重放歷史
  * 按照它們在日誌檔案中出現的順序（從檢查點到尾部）

### REDO-UNDO恢復算法（假設無邏輯操作）

* 在REDO階段識別未完成交易並保存到撤銷列表中
* 可以處理恢復期間重複的當機

### 支援邏輯操作

* 重複歷史（REDO）按照確切發生順序執行物理操作
* 適用於依賴的物理操作

### REDO-UNDO恢復算法（支援邏輯操作）

* REDO：重複歷史
  * 重放物理和補償日誌
* UNDO：
  * 對物理和未完成的邏輯操作進行物理撤銷
  * 對已完成的邏輯操作進行邏輯撤銷
  * 跳過所有已中止的邏輯操作
* 在UNDO階段繼續記錄：
  * 物理撤銷的補償日誌
  * 邏輯撤銷的物理日誌

### 非冪等邏輯操作

* 邏輯操作及其邏輯撤銷不是冪等的
* 已完成的邏輯操作和邏輯撤銷使用物理日誌重複
  * 在REDO階段
  * "歷史"增長
* UNDO階段必須跳過已完成的邏輯撤銷

### 更快的檢查點

* 活躍交易列表
  * 每個交易的最近LSN
* 髒頁列表
  * RecLSN：反映到硬碟的最新日誌
  * PageLSN
* 無需刷寫頁面

### 恢復回滾（Resume Rollbacks）

* 如何在UNDO階段恢復所有未完成交易的回滾？
* 對每個未完成交易：
  * 已完成的邏輯撤銷必須跳過
  * 已完成的物理撤銷可以跳過（可選；僅為提高性能）

### PrevLSN和UndoNextLSN指針

* 記錄：
  * 每個物理日誌保存PrevLSN
  * 每個補償日誌保存UndoNextLSN
* RecoveryMgr：
  * 記住撤銷列表中每個交易的最後UndoNextLSN
  * UNDO階段處理的下一個LSN是UndoNextLSNs的最大值
* 交易回滾可以恢復

## 生理日誌（Physiological Logging）

### 物理日誌的問題

* 物理日誌數量龐大！
* 例如，如果系統想要將記錄插入到排序檔案中（維護索引時常見）

### 生理日誌的優點

* 觀察：在排序操作期間，對同一區塊的所有物理操作將在一次刷寫中寫入硬碟
* 為何不將所有這些物理操作記錄為一個邏輯操作？
  * 只要這個邏輯操作可以邏輯撤銷
* 生理日誌的特點：
  * 跨區塊為物理的
  * 在每個區塊內為邏輯的
* 顯著節省物理日誌的成本
* 但進一步複雜化恢復算法
  * 因為REDOs不再是冪等的
  * 需要避免重複重放

### REDO-UNDO恢復算法（支援生理日誌）

* 在REDO期間，跳過先前已重放的所有生理操作及其補償
* 在UNDO期間，將每個生理操作視為物理操作
  * 寫入也是生理操作的補償日誌

### 避免重複重放

* 為每個區塊保留PageLSN
  * 該區塊的最新日誌
* REDO階段：
  * 僅當LSN大於目標區塊的PageLSN時才重放生理日誌
* 在ARIES中進一步優化

## VanillaDB復原管理器（The VanillaDB Recovery Manager）

* 日誌粒度：值（values）
* 實現ARIES恢復算法
  * 偷取（steal）和非強制（non-force）
  * 生理日誌
  * 無優化
* 非靜態檢查點（定期）
* 相關包：
  * storage.tx.recovery
* 公共類：
  * RecoveryMgr
  * 每個交易有自己的復原管理器

# 資料庫查詢優化 (Database Query Optimization)

## 概述 (Overview)

- 查詢優化是資料庫系統中的關鍵組件，負責尋找最有效率的方式執行使用者查詢
- 一個好的查詢計畫可以比低效的計畫快上數個數量級
- 查詢優化的目標：
  - 理想情況：找到代價最低的執行計畫
  - 實際情況：避免非常糟糕的執行計畫
- 優化過程基本步驟：
  1. 生成候選計畫
  2. 估計每個計畫的執行代價
  3. 選擇並執行代價最低的計畫

## 代價估計 (Cost Estimation)

- 代價度量標準主要是查詢延遲 (query delay)，同時較低的查詢延遲也意味著更好的系統吞吐量
- I/O 延遲通常主導查詢延遲
- 對每個計畫 p，我們估計 B(p)：執行相應查詢所需存取的塊數
- 估計 B(p) 通常需要更多資訊：
  - R(p)：輸出的記錄數
  - 索引的搜尋代價 (如使用索引)
  - V(p,f)：在 p 中欄位 f 的不同值數量

### 基本計畫類型代價估計

| 計畫類型              | 代價估計 B(p)                                      |
| --------------------- | -------------------------------------------------- |
| TablePlan             | 由 StatMgr 快取的實際塊數 (透過定期的表掃描)       |
| ProjectPlan(c)        | B(c)                                               |
| SelectPlan(c)         | B(c)                                               |
| IndexSelectPlan(t)    | IndexSearchCost(R(t), R(p)) + R(p)                 |
| ProductPlan(c1, c2)   | B(c1) + (R(c1) * B(c2))                            |
| IndexJoinPlan(c1, t2) | B(c1) + (R(c1) * IndexSearchCost(R(t2), 1)) + R(p) |

- B(c) 會遞歸地計算到表層級

### 基數估計 (Cardinality Estimation)

- 估計 R(p) 的過程稱為基數估計
- 索引搜尋代價可透過特定索引的方法計算：
  - HashIndex.searchCost()
  - BTreeIndex.searchCost()

#### 樸素的估計方法

- 均勻分布假設：欄位中的所有值出現機率相同
- 所需的統計資訊不多：
  - R(c)：子計畫 c 中的記錄數
  - V(c, f)：子計畫 c 中欄位 f 的不同值數量
  - Max(c, f)：子計畫 c 中欄位 f 的最大值
  - Min(c, f)：子計畫 c 中欄位 f 的最小值

#### 選擇算子估計 (Select Estimation)

- 對於 p = Select(c, f=x)：
  - 選擇度 (Selectivity) = $\frac{1}{V(c,f)}$
  - R(p) = 選擇度 * R(c)

- 對於 p = Select(c, f>x)：
  - 選擇度 = $\frac{Max(c,f)-x}{Max(c,f)-Min(c,f)}$
  - R(p) = 選擇度 * R(c)

#### 實際情況的問題

- 在真實世界，欄位的值分布很少是均勻的
- 例如：p = Select(c, f=14)
  - 假設 V(c,f) = 15，估計 R(p) = $\frac{1}{15} * R(c) = 3$
  - 但實際上 R(p) 可能是 9

## 直方圖 (Histograms)

- 直方圖能更準確地近似每個欄位中的值分布
- 將欄位值劃分為一組桶 (buckets)
- 桶數越多，近似越準確，但儲存代價也更高

### 直方圖中的桶 (Buckets)

- 每個桶 b 收集一個值範圍的統計資料
- 假設桶內記錄和值均勻分布
- 收集的統計資料：
  - R(p, f, b)：記錄數
  - V(p, f, b)：不同值數量
  - Range(p, f, b)：值範圍

### 使用直方圖的基數估計

- 不論 p 是什麼，我們都有：
  $R(p) = \sum_{b \in p.hist.buckets(f)} R(p, f, b)$

#### 範圍選擇 (Range Selection)

- 對於 p = Select(c, f in Range)，對於欄位 f 中的每個桶 b：
  - 選擇度 = $\frac{|Range(c,f,b) \cap Range|}{|Range(c,f,b)|}$
  - Range(p,f,b) = Range(c,f,b) ∩ Range
  - V(p,f,b) = V(c,f,b) * 選擇度
  - R(p,f,b) = R(c,f,b) * 選擇度

- 對於欄位 f' ≠ f 中的每個桶 b：
  - 縮減率 = $\frac{\sum_b R(p,f,b)}{R(c)}$
  - Range(p,f',b) = Range(c,f',b)
  - R(p,f',b) = R(c,f',b) * 縮減率
  - V(p,f',b) = min(V(c,f',b), R(p,f',b))

#### 笛卡爾積 (Product)

- 對於 p = Product(c1, c2)：
  - 對於 c1 中的每個 (b,f)：
    - Range(p,f,b) = Range(c1,f,b)
    - V(p,f,b) = V(c1,f,b)
    - R(p,f,b) = R(c1,f,b) * R(c2)
  - 對於 c2 中的每個 (b,f)：
    - Range(p,f,b) = Range(c2,f,b)
    - V(p,f,b) = V(c2,f,b)
    - R(p,f,b) = R(c2,f,b) * R(c1)

#### 連接選擇 (Join Selection)

- 對於 p = Select(c, f=g) 或 Join(a, b, a.f=b.g)：
  - 對於欄位 f 中的桶 b1 和欄位 g 中的桶 b2：
    - Range(p,f,b1) = Range(p,g,b2) = IR = Range(c,f,b1) ∩ Range(c,g,b2)
    - minV = min($\frac{|IR|*V(c,f,b1)}{|Range(c,f,b1)|}$, $\frac{|IR|*V(c,g,b2)}{|Range(c,g,b2)|}$)
    - V(p,f,b1) = V(p,g,b2) = minV
    - R(p,f,b1) = R(p,g,b2) = min(R1, R2)
      - 其中 R1 = R(c,f,b1) * $\frac{minV}{V(c,f,b1)}$ * $\frac{1}{V(c,g,b2)}$ * $\frac{R(c,g,b2)}{R(c)}$
      - 其中 R2 = R(c,g,b2) * $\frac{minV}{V(c,g,b2)}$ * $\frac{1}{V(c,f,b1)}$ * $\frac{R(c,f,b1)}{R(c)}$

  - 對於欄位 f' ∉ {f, g} 中的每個桶 b：
    - 縮減率 = $\frac{\sum_b R(p,f,b)}{R(c)}$
    - R(p,f',b) = R(c,f',b) * 縮減率
    - V(p,f',b) = min(V(c,f',b), R(p,f',b))
    - Range(p,f',b) = Range(c,f',b)

### 直方圖類型

#### 等寬直方圖 (Equi-Width Histogram)

- 劃分策略：所有桶具有相同範圍
- 桶範圍 = $\frac{Max(p,f) - Min(p,f) + 1}{桶數}$
- 問題：某些桶可能被浪費

#### 等深直方圖 (Equi-Depth Histogram)

- 劃分策略：所有桶具有相同記錄數
- 深度 = $\frac{R(p)}{桶數}$
- 問題：桶內的記錄/值可能不均勻分布

#### 最大差異直方圖 (Max-Diff Histogram)

- 劃分策略：在記錄數或面積具有最大差異的值處分割桶
  1. 記錄數 (MaxDiff(F))：每個桶的記錄數均勻
  2. 面積 (MaxDiff(A))：每個桶的記錄數和值均勻

### VanillaCore 中的直方圖

- 表直方圖是統計元數據
- 位於 org.vanilladb.core.storage.metadata.statistics 包中
- 透過 StatMgr.getTableStatInfo() 存取（由 TablePlan 使用）

#### 建立直方圖

- 系統啟動時：
  - StatMgr 掃描表並呼叫 SampledHistogramBuilder.sample()
  - 完成後，呼叫 SampledHistogramBuilder.newMaxDiffHistogram()
  - 使用的直方圖類型：
    - 數值欄位使用 MaxDiff(A)
    - 其他類型使用 MaxDiff(F)

- 運行時：
  - StatMgr 追蹤每個表的更新記錄數
  - QueryPlanner 在執行修改/插入/刪除查詢後呼叫 StatMgr.countRecordUpdates()
  - 當呼叫 StatMgr.getTableStatInfo() 且更新記錄數超過閾值（例如100）時，背景重建直方圖
  - StatisticsRefreshTask 掃描表並呼叫 SampledHistogramBuilder.sample()，然後呼叫 SampledHistogramBuilder.newMaxDiffHistogram()

## 啟發式查詢優化器 (Heuristic Query Optimizer)

### 基本計畫生成器 (Basic Planner)

- 產品/連接順序遵循 SQL 中的寫法
- 查詢優化有兩個常見的啟發式方法：
  1. 下推選擇操作 (Pushing Select Down)
  2. 貪婪連接排序 (Greedy Join Ordering)

### 下推選擇操作 (Pushing Select Down)

- 盡早執行選擇操作，以減少每個連接操作的記錄數 (R)
- 例如轉換：
  ```
  SELECT * FROM t1, t2, t3 WHERE t1.f1 = t2.f2 AND t2.f3 = t3.f4 AND t1.f5 = x
  ```
  轉為：
  ```
  Select(t2.f3 = t3.f4)
    Product
      Select(t1.f1 = t2.f2)
        Product
          Select(t1.f5 = x)
            Table t1
          Table t2
      Table t3
  ```

### 貪婪連接排序 (Greedy Join Ordering)

- B(root) = B(p1) + (R(p1) * ...) + ...
- 降低 B(root) 意味著降低 R(p1)
- B(p1) = B(c1) + (R(c1) * ...) + ...
- 降低 B(root) 也意味著降低 R(c1)
- 因此 B(root) ∝ R(p1) + R(c1) + ...
- 貪婪連接排序：反覆將表加入到「主幹」中，使得 R(trunk) 最小

### VanillaCore 中的啟發式查詢計畫生成器

- 在 HeuristicPlanner 中實現
- 步驟 1：為每個提到的表/視圖創建 TablePlanner 對象
- 步驟 2：選擇大小最小的計畫開始連接主幹
- 步驟 3：反覆將計畫添加到連接主幹上
  - 嘗試找出代價最低的連接計畫
  - 如果沒有適用的連接，則使用笛卡爾積

## Selinger 風格查詢優化器 (Selinger-Style Query Optimizer)

### 啟發式計畫生成器的問題

- 貪婪法的假設：降低 R(c1) 意味著降低 R(p1)
- 但這可能不正確：連接率也很重要
- 窮舉搜索最佳連接順序太昂貴：n 個連接的候選數為 O(n!)

### Selinger 風格優化的原理

- 使用遞歸和子問題最優性：
  - B*({t1, t2, t3}) = min(B*({t1, t2} ⨝ t3), B*({t1, t3} ⨝ t2), B*({t2, t3} ⨝ t1))
- 子最優性：
  - 如果 B*({t1, t2}) = B(t1 ⨝ t2) <= B(t2 ⨝ t1)
  - 則 B*({t1, t2} ⨝ t3) = B(t1 ⨝ t2 ⨝ t3) <= B(t2 ⨝ t1 ⨝ t3)
- 可使用動態規劃避免重複計算

### Selinger 優化器示例

- 考慮連接 3 個關係：X, Y, Z

1. 步驟 1：計算每個表的代價（含適當的選擇操作）
   | 1-集合 | 最佳計畫          | 代價 |
   | ------ | ----------------- | ---- |
   | {X}    | Index Select Plan | 10   |
   | {Y}    | Table Plan        | 30   |
   | {Z}    | Select Plan       | 20   |

2. 步驟 2：計算 2-way 連接的代價
   | 2-集合 | 最佳計畫 | 代價 |
   | ------ | -------- | ---- |
   | {X, Y} | X ⨝ Y    | 159  |
   | {X, Z} | Z ⨝ X    | 98   |
   | {Y, Z} | Z ⨝ Y    | 77   |

3. 步驟 3：計算 3-way 連接的代價
   | 3-集合    | 最佳計畫  | 代價 |
   | --------- | --------- | ---- |
   | {X, Y, Z} | Z ⨝ X ⨝ Y | 100  |

### 複雜度比較

- 對於 n=8 的情況：
  - 窮舉搜索：8! = 40,320 個候選
  - Selinger 風格計畫生成器：2^8 = 256 個候選

### VanillaCore 中的 SelingerLikeQueryPlanner

- 位於 org.vanilladb.core.query.planner.opt 包中
- 使用 AccessPath 類來表示每個子計畫
- AccessPathId 是查找表的鍵，使用 sum of pow(2, tp.id) 來表示訪問路徑中的 k-集合
- 使用 R(p1) + R(c1) + ... 來近似 B(root)
- 逐層構建計畫，每層考慮所有可能的連接
