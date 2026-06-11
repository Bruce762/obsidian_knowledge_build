---
description: 把一份新來源讀入 wiki — 摘要、交叉連結、更新 index 與 log
argument-hint: <raw/ 檔案路徑或來源說明>
---

# Ingest：$ARGUMENTS

把 `$ARGUMENTS` 指定的來源整合進知識庫。先快速確認 `CLAUDE.md` 的當前 schema，再依序：

1. **讀來源。** 讀 `$ARGUMENTS`（通常在 `raw/`）。若是含內嵌圖片的 markdown，先讀文字、再視需要單獨檢視 `raw/assets/` 裡被引用的圖片補足脈絡。若使用者只給了主題沒給檔案，先問檔案在哪／請他放進 `raw/`，不要自己編內容。

2. **討論重點。** 用幾句話講這份來源的關鍵 takeaway，並說明你打算建立／更新哪些頁。**等使用者確認或調整後再大量寫入**（除非他說「直接做」）。

3. **寫來源摘要頁。** 在 `wiki/sources/<slug>.md` 建立摘要頁（含 `type: source` frontmatter，`source:` 指回 `raw/` 原檔）。摘要要能獨立閱讀，並用 `[[wikilink]]` 連到相關實體／概念。

4. **更新跨頁。** 找出這份來源涉及的實體與概念：
   - **已存在** → 更新該頁，併入新資訊，更新 `updated:` 與 `sources:`。
   - **不存在但重要** → 在 `wiki/entities/` 或 `wiki/concepts/` 新建頁。
   - **與既有主張衝突** → 在頁面明確標註矛盾與雙方來源，**不要默默覆蓋**。

   一份來源動到 10–15 頁是正常的。

5. **更新總論。** 若這份來源改變了整體判斷或主論點，更新 `wiki/overview.md`（synthesis）；只是補充細節就跳過，不要每次都改。

6. **更新 index。** 在 `wiki/index.md` 把新頁登錄到對應分類（`[[連結]]` + 一句話）。

7. **追加 log。** 在 `wiki/log.md` 追加一筆：`## [<今天日期>] ingest | <來源標題>`，底下列出建立／更新了哪些頁。

8. **回報。** 簡短列出你動過的頁，方便使用者在 Obsidian 裡點開檢查。
