# 项目记忆（Obsidian 卡片笔记知识库）

## 项目性质

- 这是一个 Obsidian 知识库（vault），不是代码仓库。
- 笔记体系参考 Zettelkasten（卡片盒笔记法）：原子化、双向链接、MOC 结构笔记。
- 版本管理使用 obsidian-git 插件，仓库根目录即 vault 根目录。

## 目录结构

| 目录 | 用途 |
|------|------|
| `02daily/` | 日记，文件名 `YYYY-MM-DD.md`；旧日记在 `02daily/archive/` |
| `03permanent/` | 永久笔记（原子化卡片，一个笔记一个主题） |
| `04moc/` | MOC / 结构笔记，主题入口，用列表聚合相关笔记 |
| `05projects/` | 项目笔记 |
| `06templates/` | 模板（daily、reading_notes 等） |
| `07assets/` | 图片等附件 |
| `08meta/` | 元信息（个人资料等） |
| `.trash/` | Obsidian 回收站，不读取、不修改 |
| `.obsidian/` | Obsidian 配置，除非明确要求，否则不改 |

## 命名与格式约定

- 日记：`02daily/YYYY-MM-DD.md`（由 `.obsidian/daily-notes.json` 指定，格式 `YYYY-MM-DD`）。
- 日记 frontmatter：

  ```yaml
  ---
  tag: [daily]
  date: YYYY-MM-DD
  ---
  ```

- 日记固定结构：`# {{date}} Daily Note` → `## Todo`（`### Schedule` 表格 / `### Review`：`#### Time accounting`、`#### Incomplete tasks`、`#### Summary and Reflection`）。
- 永久笔记通常不加 YAML frontmatter，而是在正文顶部写行内标签，如 `#卡片式笔记法 #学习方法 #ai`。
- 笔记间使用 Obsidian 双链 `[[笔记名]]`；附件使用 `![[文件名]]`。
- 新建日记请基于 `06templates/daily.md`，把 `{{date}}` 替换为实际日期；其他模板中 `{{title}}` 同理。
- 除 `02daily/` 外，多用中文标题与中文正文；专有名词、引用原文可保留原语言。

## 02daily 语言规则

- `02daily/` 下的笔记内容默认使用**英文**书写。
- 即使用户用中文输入，也先翻译成英文再写入日记。
- 例外：人名、中国地名、中文书籍名等专有名词保留原文，不翻译。

## 写作规则

- 新增永久笔记时：遵循原子化原则，用自己的话复述，并至少链接一张相关笔记（写明链接上下文）。
- 新增内容若属于某主题，同步检查对应 `04moc/` 下是否有结构笔记，必要时把新笔记加入其列表。
- 修改时保持其余内容不变，不擅自重排或格式化已有笔记。
- 不要删除笔记；需要清理时移入 `.trash/` 或交给用户确认。

## Git 工作流

- 默认直接提交到 `main` 分支并 push 到 `origin/main`，无需额外确认，也不新建功能分支。
- 提交前先同步远端 main，避免推送被拒。
- 提交信息使用中文，沿用 `feat:` / `fix:` / `docs:` 前缀。
- 只提交与本次任务相关的文件，避免夹带无关改动。

## 已有技能

- `update-daily-file`：把感悟写入当天日记的 `#### Summary and Reflection` 下的 `- Thinking`，按来源分组。相关任务优先使用该技能。
