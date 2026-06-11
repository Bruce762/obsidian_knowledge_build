# obsidian_knowledge_build

一組 Claude Code slash commands，把任意輸入（文章、論文、筆記、對話…）**逐步建構成可累積的個人知識庫（第二大腦）**，並以 Obsidian 作為閱讀／瀏覽介面。

> **致謝**：核心構想來自 Andrej Karpathy 的 gist
> [**karpathy/llm-wiki.md**](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)。
> 本專案把該構想拆成四個可在 Claude Code 直接呼叫的 slash command，並補上中文工作流程。原始概念與文字版權歸原作者所有。

## 這是什麼

傳統用 LLM 讀文件的方式是 RAG：上傳一堆檔案，查詢時即時檢索片段、生成答案——知識不會累積，每次都從頭重新發現。

這個專案採用 Karpathy 提出的另一種模式：LLM **增量地建構並維護一個持久化的 wiki**——一組結構化、互相連結的 markdown 檔案，夾在你與原始來源之間。每加入一份新來源，LLM 不只是索引它，而是讀它、抽取重點、整合進既有 wiki（更新實體頁、修訂主題摘要、標記與舊主張矛盾之處）。知識被「編譯」一次後持續維持最新，而不是每次查詢重新推導。

- **你**負責：蒐集來源、探索、提出好問題。
- **LLM**負責：摘要、交叉引用、歸檔、記帳等所有維護工作。

完整構想說明在 [.claude/commands/wiki-init.md](.claude/commands/wiki-init.md)。

## 架構（三層）

| 層 | 說明 | 誰維護 |
| --- | --- | --- |
| **Raw sources**（`raw/`） | 你蒐集的原始來源（文章、論文、圖片、資料）。不可變，LLM 只讀不改。 | 你 |
| **The wiki**（`wiki/`） | LLM 生成的 markdown：摘要、實體頁、概念頁、筆記。 | LLM |
| **The schema**（`CLAUDE.md`） | 告訴 LLM wiki 怎麼組織、慣例為何、各工作流程怎麼跑。 | 你與 LLM 共同演進 |

三個特殊檔案協助導航：

- `wiki/index.md`——內容導向的目錄（每頁一行連結＋摘要），每次 ingest 更新。
- `wiki/overview.md`——演進中的總論（synthesis）：目前的整體判斷與主論點，整體判斷有變時更新。
- `wiki/log.md`——時序的 append-only 紀錄（ingest／query／lint）。

## 四個指令

| 指令 | 用途 |
| --- | --- |
| [`/wiki-init`](.claude/commands/wiki-init.md) | 初始化知識庫：建立資料夾結構、`CLAUDE.md` schema、`index.md`／`log.md`，並示範第一次 ingest。**只需在一開始跑一次。** |
| [`/ingest <來源>`](.claude/commands/ingest.md) | 把一份新來源讀入 wiki：摘要 → 交叉連結 → 更新 index 與 log。 |
| [`/query <問題>`](.claude/commands/query.md) | 對 wiki 提問：讀 index、帶引用綜合作答，並可把有價值的答案回存成頁面。 |
| [`/lint [範圍]`](.claude/commands/lint.md) | 健檢 wiki：找矛盾、過時主張、孤兒頁、缺頁、缺連結與資料缺口。 |

## 怎麼用

### 1. 初始化（一次）

```
/wiki-init
```

Agent 會先請你提供三項設定（留空時會反問你）：**主題／用途**、**第一份匯入來源**、**wiki 存放位置**。確認後它會建立資料夾結構、`CLAUDE.md`、`index.md`、`log.md`，並示範第一次 ingest。

### 2. 日常循環

```
/ingest raw/some-article.md       # 讀入新來源
/query 這兩份報告的結論有衝突嗎？     # 提問，必要時把答案回存成筆記
/lint                             # 定期健檢
```

## 目錄結構

```
raw/                  # 原始來源（只讀）
  assets/             # 本地圖片
wiki/
  index.md            # 內容目錄
  overview.md         # 演進中的總論（synthesis）
  log.md              # 時序紀錄
  sources/            # 來源摘要頁
  entities/           # 實體頁
  concepts/           # 概念頁
  notes/              # /query 回存的筆記
CLAUDE.md             # schema（由 /wiki-init 產生）
.claude/commands/     # 四個 slash command
```

## 建議搭配

- **[Obsidian](https://obsidian.md/)** 作為閱讀介面：一邊 Claude Code、一邊 Obsidian，即時瀏覽 LLM 的編輯結果；graph view 可看出 wiki 的形狀（哪些是樞紐、哪些是孤兒頁）。
- **Obsidian Web Clipper** 把網頁文章轉成 markdown，快速送進 `raw/`。
- wiki 本身就是一個 git repo，免費獲得版本歷史與分支。

## 授權與出處

核心構想、文字、模式皆源自 Andrej Karpathy 的公開 gist：<https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>。請參考原 gist 以了解作者本人的完整論述。
