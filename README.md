# 基於幾何深度學習之乳癌淋巴結轉移分類系統
# Breast Cancer Lymph Node Metastasis Classification System Based on Geometric Deep Learning

## 概述

本研究提出了一套結合幾何快速資料密度泛函轉化(g-fDDFT)與深度學習的自動分類系統，針對乳癌前哨淋巴結組織病理影像進行分類。乳癌是女性最常見的癌症之一，其預後與前哨淋巴結轉移狀態高度相關，傳統的診斷方法需要病理學家耗費大量時間進行檢查，且可能漏掉體積較小的轉移細胞。本系統旨在提高診斷效率與準確性，降低人為誤差。

**注意**：本專案主要作為研究成果展示，其中g-fDDFT是實驗室開發的專有技術，完整代碼不公開。

## 研究背景

乳癌是婦女常見的癌症之一，盡早發現是否轉移對患者的預後以及存活率非常重要：
- 未轉移的乳癌五年存活率約為96%
- 局部淋巴結轉移的五年存活率降至86%
- 遠端轉移的五年存活率僅剩29%

組織病理學影像分析是診斷淋巴結轉移的黃金標準，但傳統的人工檢查方法耗時且可能存在誤差。本研究通過結合幾何深度學習與電腦視覺技術，提供更高效的自動化分類解決方案。

## 研究方法

### 數據集
**CAMELYON17 Challenge**：包含1000張乳癌前哨淋巴結全玻片影像(WSI)的公開資料庫
- 由5家醫學機構共同提供的H&E染色病理切片數位影像
- 訓練集500張，測試集500張，並由專家提供轉移區域的標記

[CAMELYON17挑戰賽](https://camelyon17.grand-challenge.org/)
![image](https://github.com/user-attachments/assets/98cd23a2-9532-4095-8734-fc5a8ccb44eb)


### 影像預處理

#### 幾何快速資料密度泛函轉化 (g-fDDFT)
- 使整張WSI受到均一且標準化的處理
- 通過動能密度泛函和位能密度泛函將影像映射到能量空間
- 在能量空間中對影像進行特徵強化與萃取

#### 細胞核分割
- 將RGB色彩空間轉為H&E色彩空間
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
  - Precision達到76%，較傳統方法提升30%
  - 系統在降低運算時間的同時，實現了穩定可靠的分類性能
- **實驗發現**：系統在特徵強化和效率提升方面展現出優勢，但在整體準確度方面仍有改進空間

### 參考實現

對於想要重現部分結果的研究者，以下是一些可參考的開源工具：
- 細胞核分割可參考[histomicstk](https://github.com/DigitalSlideArchive/HistomicsTK)
- WSI處理可使用[OpenSlide](https://openslide.org/)
- 深度學習模型架構可參考[Keras Applications](https://keras.io/api/applications/)


## 未來改進方向

- 提高分類準確度的同時保持高效率
- 改進細胞核分割技術，提高邊緣清晰度和準確性
- 探索更有效的色彩增強方法，提高模型的Recall
- 整合多種深度學習模型，進一步提升系統性能
- 擴展系統應用範圍，探索在其他類型病理影像分析中的潛力

## 科學貢獻

本研究的主要科學貢獻包括：

1. **效率提升**：通過創新的g-fDDFT方法，我們將處理時間縮減83.8%，顯著提高了大型WSI處理效率
2. **特徵強化**：提出了一種基於物理能量模型的特徵強化方法，成功提升特徵表現力
3. **分類性能**：在F1 score方面達到75%，較傳統方法提升13%，尤其在Precision方面有顯著提升
4. **分析流程**：建立了從WSI預處理到自動分類的完整數位病理學分析流程
5. **方法學創新**：在幾何深度學習領域提出了創新的演算法結構，為醫學影像分析提供新思路

## 相關文獻

- 本研究基於作者碩士論文：李昕瑜、陳健章. "基於幾何深度學習之乳癌淋巴結轉移分類系統." 國立中央大學生物醫學工程碩士班, 2025.
- Ehteshami Bejnordi, B., et al. "Diagnostic Assessment of Deep Learning Algorithms for Detection of Lymph Node Metastases in Women With Breast Cancer," Jama, 318(22), 2017, pp. 2199-2210.
- Chen, C.-C., et al. "Unsupervised learning and pattern recognition of biological data structures with density functional theory and machine learning," Scientific reports, 8(1), 2018, pp. 557.
- He, K., et al. "Deep residual learning for image recognition," Proceedings of the IEEE conference on computer vision and pattern recognition, 2016, pp. 770-778.

## 研究資源

本研究使用的CAMELYON17數據集是公開的，研究者可以從以下資源獲取更多信息：

- [CAMELYON17挑戰賽官網](https://camelyon17.grand-challenge.org/)
- [CAMELYON17數據集論文](https://doi.org/10.1093/gigascience/giy065)

此外，以下資源可能對相關研究有幫助：
- [數位病理學開源工具集](https://digitalpathologyassociation.org/)
- [組織病理學影像分析資源](https://www.pathologyoutlines.com/)

## 聯絡方式

李昕瑜
生物醫學工程碩士
國立中央大學生醫科學與工程學系

如對本研究有學術交流需求，請通過以下方式聯繫：
- 電子郵件：[your.email@example.com] <!-- 替換為你的電子郵箱 -->
- 研究實驗室：[實驗室網站/聯繫方式] <!-- 替換為你的實驗室聯繫方式 -->
