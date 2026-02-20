# Owner - 附件 borrow_checker.md ; 閱讀目標：寫一篇網誌, 描述 為何 Rust 能在 compile time 時檢查出 Data Race , 而 Kotlin 做不到?


---

# 🎯 Coordinator — 初步介紹

## 📄 文章確認

| 項目 | 內容 |
| :-- | :-- |
| **檔案** | `borrow_checker.md` ✅ 可讀（4,277 字） |
| **內容性質** | 已含 Explainer 三層解說、Rust vs Kotlin 對比表、Mermaid 流程圖、參考文獻 15 篇 |
| **閱讀目標** | 撰寫一篇網誌：為何 Rust 能在 compile time 檢查出 Data Race，Kotlin 做不到？ |
| **文章品質** | 內容豐富，已有大眾版類比、專業版規則、程式碼範例，可直接作為網誌素材 [^1] |


***

## 🗺️ 建議閱讀討論順序

由於閱讀目標是**產出網誌**，建議以下流程：


| 步驟 | 角色 | 目的 | 優先度 |
| :-- | :-- | :-- | :-- |
| 1 | **Verifier** | 查核文中 15 篇引用是否真實有效，確認主張可信度 | 🔴 高 |
| 2 | **Affirmer** | 強化「Rust 能做到」的最有力論據，找最新佐證 | 🟡 中 |
| 3 | **Critic** | 找出文章論述的盲點（e.g., `unsafe` block？Kotlin Coroutines 的改善？） | 🟡 中 |
| 4 | **Synthesizer** | 跨域洞見：Borrow Checker 的設計哲學對其他語言的啟示 | 🟢 視需求 |
| 5 | **Writer** | 整合所有材料，直接產出網誌草稿（結構、標題、段落） | 🔴 最終目標 |


***

## 🕹️ 手動操作指南

直接輸入角色名稱切換，例如：

```
Verifier: 請查核文章中 Kotlin 無法追蹤 aliasing 的主張
Critic: 針對「Kotlin 本質上做不到」給出最強反駁或例外情況
Writer: 根據 borrow_checker.md 的內容，產出完整網誌草稿
```

> **💡 建議捷徑**：如果目標純粹是產出網誌，可直接呼叫 `Writer:` 讓它基於現有文件起草，再視需要用 `Critic:` 或 `Verifier:` 補強。

***

**請問你想從哪個角色開始？還是直接讓 `Writer:` 起草網誌？**

<div align="center">⁂</div>

[^1]: borrow_checker.md

