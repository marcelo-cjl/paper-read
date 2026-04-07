# 2026-04-07 向量搜索/ANN 论文日报

> 北京时间：2026-04-07  
> 搜索范围：2026-04-06 新提交论文  
> 关注领域：向量量化(用于搜索)、近似最近邻搜索(graph/disk/GPU/filtered)

---

## 今日新论文：2 篇

### 1. Fiber-Navigable Search: A Geometric Approach to Filtered ANN

**arXiv**: [2604.00102](https://arxiv.org/abs/2604.00102)  
**类别**: 过滤ANN搜索 / 图索引  
**作者**: Thuong Dang

**一句话概述**：  
建立几何框架将过滤ANN搜索失败归为三类模式，通过两阶段搜索+轻量Anchor结构在低选择率场景下召回率提升 ~100%。

**核心贡献**：
- 首次将filtered ANN失败归类为：拓扑切割、几何折叠、真实盆地三种几何模式
- 两阶段算法：完整图探索 + fiber感知邻居下降/锚点重启
- Anchor结构：O(√n) 额外存储，标识满足过滤条件的簇代表点
- 低选择率下对 FAISS HNSW 召回率提升约 +100%

**详细解读**: [fiber_navigable_search_详解.md](./fiber_navigable_search_详解.md)

---

### 2. BBC: Improving Large-k Approximate Nearest Neighbor Search with a Bucket-based Result Collector

**arXiv**: 2604.xxxxx (cs.DB / cs.DS)  
**类别**: 量化ANN加速 / Large-k查询  
**作者**: 待确认

**一句话概述**：  
首次系统解决 Large-k ANN 的两大瓶颈（收集器低效+量化剪枝失效），桶式收集器 + 双重排序算法实现最高 3.8x 加速。

**核心贡献**：
- 识别 Large-k ANN 两大瓶颈：Top-k收集器 O(logk) 开销 + 量化剪枝在大k下失效
- 桶式结果缓冲：将候选插入从 O(logk) 降至 O(1)，提升缓存友好性
- 算法A（对称量化）：候选裁剪，减少精确重排序计算量
- 算法B（非对称量化）：地址排序，消除随机内存访问导致的缓存缺失
- 在 recall@k=0.95 下加速现有量化ANN方法 3.8x
- 即插即用，兼容 IVF-PQ / IVF-SQ / ScaNN / IVF-RaBitQ 等全部量化索引

**详细解读**: [BBC_large_k_ANN_详解.md](./BBC_large_k_ANN_详解.md)

---

## 今日论文主题分布

```
过滤ANN搜索 (Filtered ANN)
  └─ Fiber-Navigable Search (几何框架+图索引)

Large-k ANN 加速
  └─ BBC (桶式收集+量化重排序)
```

## 关键技术词汇

| 术语 | 含义 |
|------|------|
| Fiber | 按谓词过滤后的图子图 |
| Topological Cut | 过滤导致图断连的失败模式 |
| Geometric Fold | 过滤导致几何扭曲的失败模式 |
| Genuine Basin | 过滤导致局部最优的失败模式 |
| Anchor | 标识fiber-present簇的轻量索引结构 |
| Large-k ANN | 需要检索大量(k>>10)近邻的ANN查询 |
| Bucket-based Buffer | 按距离分桶替代最大堆的结果收集结构 |
| Symmetric Quantization | 对称量化（PQ/SQ等，查询和数据共用码本）|
| Asymmetric Quantization | 非对称量化（查询不量化，只压缩数据）|

---

## 与近期工作的关联

```
过滤ANN研究脉络：
ACORN (2024) -> Filtered-DiskANN -> Fiber-Navigable Search (本日)
图索引      -> 运行时过滤       -> 几何感知重启

量化ANN加速脉络：
PQ (2010) -> RaBitQ (2024) -> SAQ (2025) -> BBC (本日)
编码设计   -> 理论误差界    -> 维度分割   -> Large-k专项
```

---

*日报生成时间: 2026-04-07*  
*搜索工具: WebSearch + arXiv RSS*
