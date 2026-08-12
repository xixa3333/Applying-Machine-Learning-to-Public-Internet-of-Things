# Machine Learning 應用於公共物聯網災情資料分類

本專案使用 2006–2013 年 EMIS 災情資料，比較多層感知器（MLP）與隨機森林（Random Forest）對「災情類別」的分類表現。原始倉庫中的中文編碼與 Notebook 已損壞；目前版本已修復文字、整理檔案結構，並把重複的資料前處理流程合併。

## 專案內容

```text
.
├─ data/
│  └─ EMIS_2006-2013.xlsx
├─ notebooks/
│  └─ emis_disaster_classification.ipynb
├─ docs/
│  ├─ Project_民生公共物聯網_空污資料分析.pdf
│  ├─ project_20240805-0150.pdf
│  └─ 應用Machine Learning於公共物聯網.docx
├─ .gitignore
├─ README.md
└─ requirements.txt
```

## 資料集

`data/EMIS_2006-2013.xlsx` 包含 1 個工作表（`工作表1`）、33,585 筆資料與 8 個欄位：

| 欄位 | 說明 |
| --- | --- |
| 時間 | 災情通報時間 |
| 縣市 | 災情所在縣市 |
| 地址 | 災情地址或位置描述 |
| X、Y | 座標欄位，部分資料缺值 |
| 災情類別 | 模型預測目標 |
| 災情細項 | 災情的細分類 |
| 災情描述 | 自由文字描述 |

Notebook 使用 `縣市`、`災情細項`、`災情描述` 作為特徵，並以 `災情類別` 作為分類目標。為延續原始實驗，程式會進行 50% 分層抽樣，並只保留抽樣後至少有 100 筆的類別。

## 快速開始

建議使用 Python 3.10 以上版本：

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
pip install -r requirements.txt
jupyter lab notebooks/emis_disaster_classification.ipynb
```

Notebook 會優先讀取本機 `data/EMIS_2006-2013.xlsx`；若在 Google Colab 等環境執行，則會改從本倉庫下載資料。

## 模型與評估

- MLP：單一 100 神經元隱藏層，最多訓練 100 次迭代。
- Random Forest：100 棵決策樹，使用所有可用 CPU 核心。
- 資料切分：70% 訓練集、30% 測試集，並維持類別比例。
- 評估：accuracy、precision、recall 與 F1-score。

原始實驗記錄的 accuracy 約為 MLP 92.67%、Random Forest 93.26%。由於資料類別不平衡，應同時閱讀各類別的 precision、recall 與 F1-score，不能只比較 accuracy。

## 限制

- `災情描述` 是高基數文字欄位，目前以類別型 One-Hot Encoding 處理；更完整的研究可改用 TF-IDF 或語言模型嵌入。
- 部分類別樣本很少，因此原始流程以門檻排除稀少類別；這會限制模型對少數類別的適用性。
- 本專案是課程研究與方法示範，不應直接用於即時防災決策。
- 本機簡報影片約 741 MB，超過 GitHub 一般 Git 儲存庫的單檔限制，因此未納入版本控制。若要公開影片，建議使用 GitHub Release、Git LFS 或外部影音平台。

## 報告

- [專題報告 PDF](docs/Project_民生公共物聯網_空污資料分析.pdf)
- [專題報告 Word](docs/應用Machine%20Learning於公共物聯網.docx)
- [補充 PDF](docs/project_20240805-0150.pdf)
