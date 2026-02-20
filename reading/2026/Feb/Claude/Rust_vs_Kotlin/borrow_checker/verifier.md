# 🔍 Verifier — 事實查核報告

> **查核範圍**：`borrow_checker.md` 全文主要主張，共 **11 項**
> **標準**：🟢 已驗證 / 🟡 部分支持 / 🔴 未 grounding 或矛盾

***

## 一、Data Race 定義

| \# | 主張 | 判定 | 說明 |
| :-- | :-- | :-- | :-- |
| 1 | Data race = 兩個以上執行緒同時存取同一記憶體，至少一個寫入，且無同步機制 | 🟢 **已驗證** | 與 Rust Reference 官方定義及 LogRocket 技術文章完全吻合 [^1][^2] |


***

## 二、Rust Borrow Checker 三條規則

| \# | 主張 | 判定 | 說明 |
| :-- | :-- | :-- | :-- |
| 2 | Rule 1：同作用域內只能有一個 `&mut` | 🟢 **已驗證** | trippy.dev 及 LogRocket 均有一致確認 [^3][^1] |
| 3 | Rule 2：有多個 `&` 時，不得同時存在 `&mut` | 🟢 **已驗證** | Rust Users Forum 與 blog.logrocket.com 完整支持 [^4][^1] |
| 4 | Rule 3：Lifetime 保證 reference 不超過 owner 生命週期 | 🟢 **已驗證** | reintech.io 及官方文件均支持此機制 [^3] |
| 5 | 這三條規則讓 data race **在結構上不可能存在**（safe Rust） | 🟢 **已驗證** | Rust Users Forum 明確指出：「it's impossible to alias a mutable reference, so it's impossible to perform a data race」[^4] |


***

## 三、Compile-Time / Runtime Overhead

| \# | 主張 | 判定 | 說明 |
| :-- | :-- | :-- | :-- |
| 6 | Borrow checker 是靜態分析，**沒有 runtime overhead** | 🟢 **已驗證** | LogRocket 對比表：Rust「Minimal runtime overhead, No background garbage collection」[^1] |
| 7 | **⚠️ 重要補充（文章未明確說明）**：`unsafe` block 中仍可能發生 data race | 🟡 **部分遺漏** | Hacker News 討論及 Rustonomicon 均指出：unsafe Rust 可繞過 borrow checker，data race 在 unsafe 中是 UB [^5][^6]。文章正文未加此 caveat，建議網誌中補充 |


***

## 四、Kotlin 本質障礙

| \# | 主張 | 判定 | 說明 |
| :-- | :-- | :-- | :-- |
| 8 | Kotlin 的 GC 設計允許同一物件被多個變數同時持有（aliasing），編譯器無法追蹤 | 🟢 **已驗證** | LinkedIn（Dave Leeds）明確：「when it comes to shared data, it's still up to the developer to prevent race conditions」[^7] |
| 9 | Kotlin 必須**手動**使用 Mutex、Atomic、confinement 等工具 | 🟢 **已驗證** | typealias.com 與 LinkedIn 文章均列出這些工具，強調需開發者自律 [^8][^7] |
| 10 | Kotlin「本質上無法做到 compile-time 級別完整保證」 | 🟡 **部分支持，需加 nuance** | KotlinConf 2023（Google 工程師 Kevin Bierhoff）的演講顯示，Kotlin **可透過 static analysis 偵測部分 racy code**，但只是 heuristics，非完整保證 [^9]。文章的「本質上做不到」需要加上此例外說明 |
| 11 | Kotlin Coroutines 的 race 偵測主要靠 TSAN（runtime） | 🟢 **已驗證** | KotlinConf 演講明確：Google 使用「TSAN to detect race conditions when running tests」[^9] |


***

## 🔴 Verifier 重點警示（寫網誌前必看）

> **最重要的兩個 nuance，原文未充分強調：**

1. **`unsafe` Rust 例外**：文章在正文中說 Rust「保證無 data race」，但這僅限 **safe Rust**。`unsafe` block 可繞過 borrow checker，仍可能引入 data race（屬於 Undefined Behavior）。網誌中應加此 caveat，否則論述過強 。[^6][^5]
2. **Kotlin static analysis 的部分能力**：Google 已開發 Kotlin 靜態分析工具，能「在 compile time 基於 heuristics 偵測部分 racy code」。Kotlin「做不到」的準確說法應是：**無法做到 Rust 級別的完整、強制、型別系統級保證**，而非完全不能靜態分析 。[^9]

***

**建議下一步**：可呼叫 `Critic:` 進一步挑戰「unsafe block 的邊界問題」，或直接呼叫 `Writer:` 加入這兩個 nuance 後起草網誌。
<span style="display:none">[^10][^11][^12][^13][^14][^15]</span>

<div align="center">⁂</div>

[^1]: https://blog.logrocket.com/introducing-rust-borrow-checker/

[^2]: https://www.youtube.com/watch?v=hmefR4tE_H4

[^3]: https://trippy.dev/blogs/programming/rusts-borrow-checker-and-its-role-in-preventing-data-races

[^4]: https://users.rust-lang.org/t/data-races-in-rust/54627

[^5]: https://news.ycombinator.com/item?id=40365912

[^6]: https://news.ycombinator.com/item?id=46154386

[^7]: https://www.linkedin.com/posts/daveleeds_how-to-prevent-race-conditions-in-kotlin-activity-7398743882827706368-2ksW

[^8]: https://typealias.com/articles/prevent-race-conditions-in-coroutines/

[^9]: https://www.youtube.com/watch?v=SVlY7Mca1xg

[^10]: https://www.reddit.com/r/rust/comments/v3vagi/data_races_explanation_in_the_rust_book/

[^11]: https://users.rust-lang.org/t/extending-the-borrow-checker/133564

[^12]: https://viralinstruction.com/posts/borrowchecker/

[^13]: https://www.reddit.com/r/rust/comments/u2ge5n/is_this_code_considered_a_data_race_or_race/

[^14]: https://stackoverflow.com/questions/69197290/is-rust-style-ownership-and-lifetimes-possible-without-rust-style-borrow-checkin

[^15]: https://discuss.kotlinlang.org/t/kotlin-coroutines-and-race-detection/2405

