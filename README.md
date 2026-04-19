# IELTS Claude Skills

雅思备考 AI 教练系统，适用于支持 Agent Skills 的 AI 编辑器（如 Warp、Cursor、Claude Code 等）。

## v3.0 架构

每个 skill 目录下有两个文件：

- **`SKILL.md`** — 执行流程（<150 行）。顶部有「执行流程总览」锚点，末尾是**强制 Phase：持久化 + 回执**（「不输出回执 = 任务未完成」）。
- **`REFERENCE.md`** — 评分表、题型详表、话题库、模板、陷阱表等参考资料。按需查阅，不常驻上下文。

拆分后 SKILL.md 专注流程，避免长上下文导致 AI 忘记写文件；REFERENCE.md 承载细节，AI 用到时才读。

## Skills 列表

| Skill | 功能 |
|-------|------|
| `ielts` | 总入口：路由 + 进度追踪 + 复盘 + 规划 |
| `ielts-diagnose` | 成绩诊断 + 弱项定位 + 个人备考计划 |
| `ielts-writing` | 写作四维批改 + 句子级标注 + 改写对比 |
| `ielts-reading` | 同义替换提取 + T/F/NG 逻辑拆解 + 段落结构分析 |
| `ielts-speaking` | 话题分组 + 万能故事生成 + Part 3 追问预测 |
| `ielts-vocab` | 间隔重复 + 同义替换训练 + 测试 |

## Quickstart

### 1. 安装
把 6 个 skill 目录（每个含 `SKILL.md` + `REFERENCE.md`）放到 AI 编辑器的 skill 加载路径下。

**Warp / Claude Code** 默认路径：
```bash
git clone https://github.com/LingJingMaster/ielts-claude-skills.git ~/ielts-skills
cp -r ~/ielts-skills/ielts* ~/.agents/skills/
```

**Cursor** 放到 `.cursor/rules/` 或项目级 skills 目录下。

### 2. 初始化数据目录
首次使用任何一个 skill 时会自动创建。你也可以提前手动建：
```bash
mkdir -p ~/.ielts/{writing/submissions,reading/submissions,speaking/stories,vocab}
```
所有进度数据（分数、错题、词汇、作文批改）都保存在 `~/.ielts/` 下，跨会话保留。

### 3. 推荐使用流程

**首次使用（第一次备考）：**
```
你：/ielts
AI：（问你目标分、当前水平、备考时间）
你：目标 7.5，现在 L7 R6.5 W5.5 S6，2 个月后考
AI：（路由到 /ielts-diagnose 生成 8 周计划）
```

**日常训练：**
| 场景 | 说 |
|------|---|
| 写了一篇作文要批改 | 「批改这篇作文」+ 粘贴题目和作文 |
| 阅读错题想分析 | 「这道为什么错」+ 粘贴文章、题目、你的答案 |
| 准备口语 | 「帮我做 Part 2 话题分组」+ 粘贴题库 |
| 背单词 | 「今天背什么」/「考我」 |
| 查看进度 | 「看看进度」 |

**模考后复盘：**
```
你：做完了一套剑 19 Test 3，L32 R28 W5.5 S6.5，发阅读错题分析
AI：（/ielts-diagnose 更新分数 → /ielts-reading 分析错题）
```

### 4. AI 工具搭配建议
| 科目 | 推荐工具 | 理由 |
|------|---------|------|
| 听力 | 无（靠真题精听 + 王陆语料库） | AI 帮不上忙 |
| 阅读 | 本套 skill（Claude / Warp） | 逻辑拆解强项 |
| 写作 | 本套 skill + 交叉验证（UpScore.ai 等） | AI 评分普遍偏高 0.5 |
| 口语 | 本套 skill 生成素材 + Gemini Live / ChatGPT Voice 实战 | 素材生成和口语陪练分开 |
| 词汇 | 本套 skill + Anki | 间隔重复 + 跨设备同步 |

### 5. 常见问题

**Q：数据存在哪？**
A：`~/.ielts/`，纯文本 markdown，你可以直接看、直接改、直接备份。

**Q：换电脑怎么办？**
A：把 `~/.ielts/` 整个目录拷过去就行。

**Q：AI 给的分数准吗？**
A：普遍偏高 0.5 分。建议 2-3 个工具交叉验证。

**Q：可以只用其中几个 skill 吗？**
A：可以，各 skill 独立运行。但 `ielts` 总入口读的是汇总数据，建议保留。
