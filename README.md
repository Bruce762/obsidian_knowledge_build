# obsidian_knowledge_build

一個 Claude Code 的 `/wiki` slash command，用來把任意輸入（文章、論文、筆記、對話…）**逐步建構成可累積的個人知識庫（第二大腦）**，並以 Obsidian 作為閱讀／瀏覽介面。

> **致謝**：本 command 的核心構想完全來自 Andrej Karpathy 的 gist
> [**karpathy/llm-wiki.md**](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)。
> 本專案把該構想包裝成一個可在 Claude Code 直接呼叫的 slash command，並補上中文的啟動指令模板。原始概念與文字版權歸原作者所有。

## 這是什麼

傳統用 LLM 讀文件的方式是 RAG：上傳一堆檔案，查詢時即時檢索片段、生成答案——知識不會累積，每次都從頭重新發現。

這個 command 採用 Karpathy 提出的另一種模式：LLM **增量地建構並維護一個持久化的 wiki**——一組結構化、互相連結的 markdown 檔案，夾在你與原始來源之間。每加入一份新來源，LLM 不只是索引它，而是讀它、抽取重點、整合進既有 wiki（更新實體頁、修訂主題摘要、標記與舊主張矛盾之處）。知識被「編譯」一次後持續維持最新，而不是每次查詢重新推導。

- **你**負責：蒐集來源、探索、提出好問題。
- **LLM**負責：摘要、交叉引用、歸檔、記帳等所有維護工作。

詳細的構想說明寫在 command 本體裡：[.claude/commands/wiki.md](.claude/commands/wiki.md)。

## 架構（三層）

| 層 | 說明 | 誰維護 |
| --- | --- | --- |
| **Raw sources** | 你蒐集的原始來源（文章、論文、圖片、資料）。不可變，LLM 只讀不改。 | 你 |
| **The wiki** | LLM 生成的 markdown 檔案：摘要、實體頁、概念頁、總結。 | LLM |
| **The schema** | `CLAUDE.md`——告訴 LLM wiki 怎麼組織、慣例為何、各工作流程怎麼跑。 | 你與 LLM 共同演進 |

兩個特殊檔案協助導航：

- `index.md`——內容導向的目錄（每頁一行連結＋摘要），每次 ingest 更新。
- `log.md`——時序的 append-only 紀錄（ingest／query／lint）。

## 怎麼用

在 Claude Code 中輸入：

```
/wiki
```

command 末尾的「啟動指令」會請你提供三項設定（留空時 Agent 會先反問你）：

1. **主題／用途**——這個 wiki 是關於什麼的。
2. **第一份匯入來源**——要 ingest 的檔案路徑或內容。
3. **wiki 存放位置**——預設為專案根目錄。

確認後，Agent 會逐步引導你建立 `CLAUDE.md` 架構文件、設置 `index.md` 與 `log.md`、定義資料夾命名規範，並示範第一次完整的 ingest。之後每一次互動都遵循該架構。

### 三種日常操作

- **Ingest**——丟一份新來源、請 Agent 處理：讀取 → 討論重點 → 寫摘要頁 → 更新 index → 更新相關頁 → 附一筆 log。
- **Query**——對 wiki 提問，Agent 檢索相關頁、綜合出帶引用的答案；好的答案可回存成新頁，讓探索同樣累積。
- **Lint**——定期請 Agent 健檢 wiki：找矛盾、過時主張、孤立頁、缺漏的交叉引用與資料缺口。

## 建議搭配

- **[Obsidian](https://obsidian.md/)** 作為 IDE：一邊是 Claude Code、一邊是 Obsidian，即時瀏覽 LLM 的編輯結果。graph view 可看出 wiki 的形狀（哪些是樞紐、哪些是孤立頁）。
- **Obsidian Web Clipper** 把網頁文章轉成 markdown，快速送進 raw 收藏。
- wiki 本身就是一個 git repo，免費獲得版本歷史與分支。

## 授權與出處

核心構想、文字、模式皆源自 Andrej Karpathy 的公開 gist：<https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>。請參考原 gist 以了解作者本人的完整論述。
