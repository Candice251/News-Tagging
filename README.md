# 財經新聞多標籤自動分類系統

> 公司內部專案，程式碼因保密協議不便公開，此 PDF 為方法論與結果的完整說明。

## 專案簡介
以公司爬蟲蒐集的十幾萬篇財經新聞為基礎，在零人工標注的前提下，
建立可批次更新的多標籤自動分類系統。
自行定義 30+ 個財經語義標籤，透過 Embedding 篩選、LLM Zero-shot 標注、
LinearSVM 訓練的三段式架構，
最終部署模型對全量新聞進行批次自動標注。

## 技術棧
- **語言**：Python
- **資料庫**：MongoDB
- **Embedding 模型**：paraphrase-multilingual-MiniLM-L12-v2（篩選階段）、BAAI/bge-multilingual-gemma2（訓練／推論階段）
- **LLM**：Groq API
- **分類模型**：LinearSVM（scikit-learn）
- **資料處理**：Pandas, NumPy

## 核心方法
- **資料清洗**：移除 Markdown 語法、網址、圖片標記，篩除過短文章
- **標籤庫設計**：自定義 30+ 個財經語義標籤，並為每個標籤撰寫語義定義
- **候選集篩選**：MiniLM Embedding + Cosine Similarity，每個標籤取相關度最高的前 150 篇
- **Zero-shot 標注**：將標籤定義寫入 Prompt，以 Groq LLM 對候選集進行多標籤標注，產出訓練資料
- **模型訓練**：BGE Multilingual Embedding + LinearSVM 多標籤分類
- **批次部署**：載入模型對全量未標注新聞進行自動標注

## 模型結果

| 指標 | Precision | Recall | F1-score |
|------|-----------|--------|----------|
| Micro avg | 0.72 | 0.81 | 0.76 |
| Macro avg | 0.71 | 0.71 | 0.77 |
| Weighted avg | 0.73 | 0.81 | 0.76 |
| Samples avg | 0.74 | 0.82 | 0.75 |
| **AP** | | | **0.84** |

## 完整說明
詳見 PDF 文件
