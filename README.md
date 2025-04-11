# BreastCancer-DeepLearning-Classification
深度學習模型用於乳癌全玻片影像分類的研究

# 基於幾何深度學習之乳癌淋巴結轉移分類系統
# Breast Cancer Lymph Node Metastasis Classification System Based on Geometric Deep Learning

![License](https://img.shields.io/badge/license-MIT-blue)
![Python](https://img.shields.io/badge/python-3.7+-green)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)

## 專案概述

本專案提出了一套結合幾何快速資料密度泛函轉化(g-fDDFT)與深度學習的自動分類系統，針對乳癌前哨淋巴結組織病理影像進行分類。乳癌是女性最常見的癌症之一，其預後與前哨淋巴結轉移狀態高度相關。傳統的診斷方法需要病理學家耗費大量時間進行檢查，且可能漏掉體積較小的轉移細胞。本系統旨在提高診斷效率與準確性，降低人為誤差。

## 研究背景

乳癌是全球女性致死的主要原因之一，早期診斷對提高存活率至關重要。在乳癌診斷過程中，前哨淋巴結的狀態是判斷癌症是否轉移的關鍵指標：
- 未轉移的乳癌五年存活率約為96%
- 局部淋巴結轉移的五年存活率降至86%
- 遠端轉移的五年存活率僅剩29%

組織病理學影像分析是診斷淋巴結轉移的黃金標準，但傳統的人工檢查方法耗時且可能存在誤差。本研究通過結合幾何深度學習與電腦視覺技術，提供更高效的自動化分類解決方案。

## 使用的方法與技術

### 數據集
- **CAMELYON17 Challenge**：使用包含1000張乳癌前哨淋巴結全玻片影像(WSI)的公開數據集
- 由5家醫學機構共同提供的H&E染色病理切片數位影像
- 訓練集500張，測試集500張，並由專家提供轉移區域的標記

### 幾何快速資料密度泛函轉化 (g-fDDFT)
- 創新的影像處理方法，可在能量空間中對影像進行特徵強化與萃取
- 通過動能密度泛函和位能密度泛函將影像映射到能量空間
- 使用Fermi正交活化函數進行特徵像素子群範圍的收斂
- 利用幾何穩定性技術解決能量流形邊緣的不連續問題
- 使用2-stage g-fDDFT對大型WSI進行有效處理

### 細胞核分割
- 使用color deconvolution將RGB色彩空間轉為H&E色彩空間
- 對蘇木精通道進行形態學操作（侵蝕、膨脹、填充）
- 使用分水嶺分割區分鄰近細胞核
- 採用level set segmentation技術精確分割細胞核

### 分類模型
- **特徵提取**：使用ResNet-50提取特徵
- **資料預處理與增強**：H&E染色標準化、隨機色彩增強、隨機剪裁
- **特徵池化**：使用3-norm池化對特徵向量進行合併
- **分類器**：採用LightGBM進行最終分類

## 主要發現與結果

- **處理效率**：本系統將處理時間從12720秒縮減至2059秒，效率提升83.8%
- **分類表現**：
  - F1 score達到75%，較傳統方法提升13%
  - Precision提升30%（從46%到76%）
  - 系統在降低運算時間的同時，實現了穩定可靠的分類性能
- **實驗發現**：系統在特徵強化和效率提升方面展現出優勢，但在整體準確度方面仍有改進空間

## 如何使用

### 環境設定
```bash
pip install -r requirements.txt
```

### 數據準備
1. 下載CAMELYON17數據集
2. 將WSI放置於`data/raw`目錄

### 模型訓練
```python
python train.py --config configs/train_config.yaml
```

### 評估與預測
```python
python evaluate.py --model_path models/trained_model.h5 --data_path data/test
```

## 程式碼結構
```
├── data/
│   ├── raw/                # 原始WSI數據
│   └── processed/          # 處理後的數據
├── src/
│   ├── g_fddft/            # g-fDDFT實現
│   │   ├── transform.py    # 密度泛函轉化
│   │   └── stability.py    # 幾何穩定性
│   ├── preprocessing/      # 數據預處理
│   │   ├── normalization.py
│   │   └── augmentation.py
│   ├── segmentation/       # 細胞核分割
│   │   └── nucleus_seg.py
│   ├── models/             # 深度學習模型
│   │   ├── feature_extractor.py
│   │   └── classifier.py
│   └── utils/              # 工具函數
├── configs/                # 配置文件
├── train.py                # 訓練腳本
├── evaluate.py             # 評估腳本
└── README.md
```

## 未來改進方向

- 提高分類準確度的同時保持高效率
- 改進細胞核分割技術，提高邊緣清晰度和準確性
- 探索更有效的色彩增強方法，提高模型的Recall
- 整合多種深度學習模型，進一步提升系統性能
- 擴展系統應用範圍，探索在其他類型病理影像分析中的潛力

## 相關文獻

- 本研究基於作者碩士論文：李昕瑜、陳健章. "基於幾何深度學習之乳癌淋巴結轉移分類系統." 國立中央大學生物醫學工程碩士班, 2025.
- Ehteshami Bejnordi, B., et al. "Diagnostic Assessment of Deep Learning Algorithms for Detection of Lymph Node Metastases in Women With Breast Cancer," Jama, 318(22), 2017, pp. 2199-2210.
- Chen, C.-C., et al. "Unsupervised learning and pattern recognition of biological data structures with density functional theory and machine learning," Scientific reports, 8(1), 2018, pp. 557.
- He, K., et al. "Deep residual learning for image recognition," Proceedings of the IEEE conference on computer vision and pattern recognition, 2016, pp. 770-778.

## 聯絡方式

李昕瑜
生物醫學工程碩士
國立中央大學生醫科學與工程學系

[Email] <!-- 在此添加你的電子郵箱 -->
[LinkedIn] <!-- 在此添加你的LinkedIn連結 -->
