# Multi-Algorithm-Study-on-Boundary-Based-Image-Segmentation-and-Evaluation-on-Natural-Images
---
title: "BSDS500 图像分割算法对比系统 / BSDS500 Image Segmentation Comparative Study"
permalink: /projects/bsds500-segmentation-comparison/
categories: ["图像处理 / Image Processing"]
layout: single
classes: wide
---

## 🧠 项目名称 | Project Title  
**BSDS500 图像分割算法对比系统 / BSDS500 Image Segmentation Comparative Study**

---

### 👤 项目角色 | Role  
独立完成数据读取、算法实现、调参、评估可视化与工程搭建  
Sole contributor: responsible for data loading, algorithm development, tuning, evaluation, and pipeline design

---

### 📆 项目周期 | Duration  
2025年11月 – 12月  
November – December 2025

---

### 📚 所属课程 | Course  
[EECE 5626 – Image Processing & Pattern Recognition](/courses/fall-2025/)  
东北大学《图像处理与模式识别》课程项目

---

### 🎯 项目目标 | Objective  
本项目基于 **BSDS500 图像分割基准数据集**，对六种经典图像分割算法进行系统性比较，包括：  
Otsu、K-Means、Active Contours（Snake）、形态学分割、图论分割（Graph-based segmentation）等。  

目标是构建一套 **模块化、可扩展、可调参** 的评估框架，对每种算法进行：  
- 参数调优（Grid Search）  
- 跨图像验证（Cross-image validation）  
- 区域与边界两类指标评价（IoU / Dice / Boundary F-measure）  
- 运行时间分析与可视化对比  

This project evaluates six classical image segmentation algorithms on the BSDS500 dataset using a modular and extensible architecture.  
The framework supports parameter tuning, cross-validation, region-based and boundary-based metrics, and comparative visualization.

---

### 🔧 技术工具 | Technologies  
- Python（NumPy, OpenCV, scikit-image, Matplotlib）  
- 图像分割算法实现（Otsu / K-Means / Active Contours / Morphology / Graph-based）  
- BSDS500 `.jpg` 图像与 `.mat` 人工标注解析  
- Grid Search 参数调优  
- Evaluation metrics（IoU / Dice / Boundary F-measure）  
- 跨图像验证（Train/Val/Test & K-fold）  
- 可视化与对比图生成  

---

## 🧱 工程结构 | Project Structure
本项目采用模块化结构，确保算法实现、调参、评估与可视化彼此独立：

```text
Project/
│
├── bsds_loader.py              
│
├── algorithms/
│   ├── seg_otsu.py             
│   ├── seg_kmeans.py           
│   ├── seg_snake.py            
│   ├── seg_morph.py            
│   ├── seg_graph.py            
│   └── __init__.py
│
├── metrics/
│   ├── iou.py                  
│   ├── dice.py                 
│   ├── boundary_f.py           
│   ├── timing.py               
│   └── __init__.py
│
├── tuner/
│   ├── grid_search.py          
│   ├── tuner.py                
│   └── __init__.py
│
├── evaluation/
│   ├── cross_validation.py     
│   ├── evaluate_one.py         
│   ├── evaluate_dataset.py     
│   └── __init__.py
│
├── visualization/
│   ├── overlay.py              
│   ├── plot_results.py         
│   ├── display_gt.py           
│   └── __init__.py
│
├── compare.py                  
│
├── utils/
│   ├── helpers.py              
│   ├── image_processing.py     
│   └── __init__.py
│
└── main.py

---

## 🔍 模块内容简介 | Module Descriptions

### 📌 **1. bsds_loader.py**
负责加载 **BSDS500 数据集**：  
- 读取 `.jpg` 图像（train / val / test）  
- 读取 `.mat` 人工标注（5 个 annotator）  
- 输出结构化数据供算法直接使用  

---

### 📌 **2. algorithms/**  
存放所有分割算法的独立实现。

- **seg_otsu.py**  
  全局大津阈值法（无须调参）

- **seg_kmeans.py**  
  K-Means 聚类分割（可调 `K`）

- **seg_snake.py**  
  Active Contours（可调 `alpha, beta, gamma`）

- **seg_morph.py**  
  形态学操作（opening/closing/gradient/Watershed）

- **seg_graph.py**  
  基于图论的分割（Felzenszwalb / Normalized Cut）

这些模块只负责 **给定图像 → 输出 segmentation mask**。

---

### 📌 **3. metrics/**  
基于分割结果和 ground truth 进行评价：

- **IoU**（区域重叠度）  
- **Dice**（区域相似度）  
- **Boundary F-measure**（BSDS500 官方指标，用边界图评估）  
- **运行时间统计**

这些指标用于算法比较与调参。

---

### 📌 **4. tuner/**  
用于 **参数调优**：  
- `grid_search.py`：遍历所有参数组合  
- `tuner.py`：统一调参流程，输出 `best_params` 与 `best_score`  

完全符合教授提出的：

> lack of parameter tuning strategies

---

### 📌 **5. evaluation/**  
用于 **跨图像验证与全数据评估**：

- `cross_validation.py`：Train/Val/Test 分离 & K-Fold  
- `evaluate_one.py`：评估单张图像  
- `evaluate_dataset.py`：对整个数据集计算平均指标  

回应教授的：

> cross-image validation  
> results scale across different image types

---

### 📌 **6. visualization/**  
用于展示与绘图：

- segmentation overlay  
- 算法对比柱状图 / 折线图 / 雷达图  
- 显示 5 个 annotator 的 ground truth  

---

### 📌 **7. compare.py**  
使用 **最优参数** 在 **test set** 上运行全部算法，生成：

- metrics 表格  
- 可视化对比图  
- CSV/Markdown 汇总文件  

---

### 📌 **8. main.py**  
整个工程的入口，统一执行完整流程：
1. 加载 BSDS500 数据集
2. 对训练集进行参数调优
3. 在验证集上做 cross-image 验证
4. 使用最佳参数评估测试集
5. 输出最终比较表
6. 生成可视化图表

---

## 🚀 主流程 | Main Pipeline Overview

完整的实验流程如下：

Step 1 — Load Dataset (bsds_loader)

Step 2 — Run Segmentation Algorithms (algorithms/*)

Step 3 — Parameter Tuning on Train Set (tuner/grid_search)

Step 4 — Cross-Image Validation on Val Set (evaluation/cross_validation)

Step 5 — Final Evaluation on Test Set (evaluation/evaluate_dataset)

Step 6 — Compare All 6 Algorithms (compare.py)

Step 7 — Visualize Results (visualization/plot_results)

---

### 📈 项目成果 | Results  
- 完成六类经典图像分割算法的模块化实现  
- 构建了可重用、可调参、可验证的 segmentation pipeline  
- 通过 IoU、Dice、Boundary-F 等指标对算法进行深入对比  
- 使用网格搜索实现各算法的最优参数选择  
- 可视化展示六种算法在不同图像上的表现差异  

The pipeline successfully evaluates six segmentation algorithms under consistent settings, performs parameter tuning, and visualizes both region-based and boundary-based results across the BSDS500 dataset.

---

