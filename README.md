# 🏥 Medicare Part A & B Data Pipeline

Automated data cleaning and restructuring pipeline for CMS Medicare statistics (2018–2023).

---

## 📌 Project Overview

This project reshapes hierarchical Excel reports from the US **CMS (Centers for Medicare & Medicaid Services)** into a standardized, analysis-ready **Long Format (Tidy Data)** CSV file.

---

## 🛠️ Data Processing Workflow

### 1. Source Data
* **Data Origin**: [CMS Program Statistics - Medicare Part A & Part B](https://data.cms.gov/summary-statistics-on-use-and-payments/medicare-medicaid-service-type-reports/cms-program-statistics-medicare-part-a-part-b-all-types-of-service)
* **Input Format**: Excel (`.xlsx`) with multi-tab beneficiary structures.

### 2. Data Ingestion
* Loaded raw `.xlsx` files directly via `pandas`.
* Specified `header=3` to correctly align multi-year columns (2018–2023).

### 3. Hierarchical Feature Extraction
* **Identify Metrics**: Detected top-level metric headers where year values were empty (`NaN`).
* **Forward Fill**: Created a `Metric_Type` column using `ffill()` to propagate category tags down to sub-items.
* **Data Cleaning**: Filtered out empty rows (`BLANK`) and redundant header entries.

### 4. Wide-to-Long Transformation
* Applied `pd.melt()` to convert year columns into structured `Year` and `Value` variables for easier visualization and SQL querying.

### 5. Multi-Tab Consolidation
Merged three target beneficiary tabs into a single unified dataset:
* **`MDCR SUMMARY AB 1_CPS_11SAB`** ➡️ Tagged as `All`
* **`MDCR SUMMARY AB 2_CPS_11SAB`** ➡️ Tagged as `Aged`
* **`MDCR SUMMARY AB 3_CPS_11SAB`** ➡️ Tagged as `Disabled`

---

## 📊 Dataset Schema

| Column Name | Description | Example |
| :--- | :--- | :--- |
| **`Beneficiary_Group`** | Beneficiary population type | `All`, `Aged`, `Disabled` |
| **`Metric_Type`** | Primary metric category | `Number of Original Medicare Enrollees` |
| **`Type of Coverage and Service`** | Specific service or coverage type | `Part A and/or Part B` |
| **`Year`** | Data record year | `2018` |
| **`Value`** | Statistical count/numeric value | `38665082.0` |

---

## 📁 Output File
* **Filename**: `Medicare_2018_2023_Cleaned.csv` (UTF-8 Encoded)


---


# 🏥 Medicare Part A & B 資料處理管道 (Data Pipeline)

針對美國 CMS（Medicare & Medicaid 服務中心）醫療保險統計數據（2018–2023）進行資料清洗與結構重組的 Python 資料處理管道。

---

## 📌 專案概述

將美國 CMS 官方提供的階層式 Excel 統計報告，自動化轉換與重組為符合 **Tidy Data（長格式/Long Format）** 標準的 CSV 資料集，以利後續進行 SQL 數據查詢、統計分析與資料視覺化。

---

## 🛠️ 資料處理流程 (Workflow)

### 1. 原始資料來源 (Source Data)
* **資料來源**：[CMS Program Statistics - Medicare Part A & Part B](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/CMSProgramStatistics)
* **輸入格式**：包含多個受益人分頁（Tabs）的階層式 `.xlsx` 檔案。

### 2. 資料載入 (Data Ingestion)
* 使用 Pandas 載入原始 `.xlsx` 檔案。
* 設定 `header=3` 以準確對齊跨年份（2018–2023）的欄位結構。

E (Extract - 資料擷取/載入)：
使用 Pandas 讀取原始 .xlsx 檔案與指定 header=3。
跨多個分頁（Tabs）載入全體、年長者、身障者等不同族群資料。

### 3. 階層特徵提取與清洗 (Hierarchical Feature Extraction)
* **識別主要指標**：自動偵測年份數值為空（`NaN`）的大項目標題。
* **向下填補 (Forward Fill)**：建立 `Metric_Type` 欄位並利用 `ffill()` 將大項目類別標籤向下傳播至細項。
* **資料過濾**：剔除空白列（`BLANK`）與重複的標題列。

### 4. 寬表格轉長表格 (Wide-to-Long Transformation)
* 透過 `pd.melt()` 將原本橫向排版的年份欄位，轉換為直向結構的 `Year`（年份）與 `Value`（數值）變數。

### 5. 多分頁整合 (Multi-Tab Consolidation)
將三個主要的受益人分頁整合為單一資料集，並新增標籤區分：
* `MDCR SUMMARY AB 1_CPS_11SAB` ➡️ 標記為 **All**（全體受益人）
* `MDCR SUMMARY AB 2_CPS_11SAB` ➡️ 標記為 **Aged**（年長者）
* `MDCR SUMMARY AB 3_CPS_11SAB` ➡️ 標記為 **Disabled**（身障者）

T (Transform - 資料轉換與清洗) (這是這份專案發揮最多技巧的地方)：
階層結構拆解：識別 NaN 標頭並使用 ffill()（向下填補）進行特徵提取與傳播。
資料清洗：過濾空白列（BLANK）與重複表頭。
寬轉長（Unpivot/Reshaping）：使用 pd.melt() 將原本橫向的年份資料轉換為標準的 Tidy Data (Long Format) 結構。
資料合併（Consolidation）：將三個 Tab 的資料打散後重新整合並建立變數標籤 (Beneficiary_Group)。


---

## 📊 資料集結構 (Dataset Schema)

| 欄位名稱 (Column Name) | 說明 (Description) | 範例 (Example) |
| :--- | :--- | :--- |
| **Beneficiary_Group** | 受益人族群分類 | `All`, `Aged`, `Disabled` |
| **Metric_Type** | 主要指標類別名稱 | `Number of Original Medicare Enrollees` |
| **Type of Coverage and Service** | 具體服務與承保類型 | `Part A and/or Part B` |
| **Year** | 數據紀錄年份 | `2018` |
| **Value** | 統計數量/數值 | `38665082.0` |

---

## 📁 輸出檔案 (Output File)

* **檔名**：`Medicare_2018_2023_Cleaned.csv`（UTF-8 編碼）

L (Load - 資料載入/輸出)：
將清洗與結構重組完畢的資料，輸出成適合 SQL / BI 工具進行分析的 Medicare_2018_2023_Cleaned.csv。
