# IELTS Claude Skills

面向 Agent Skills 工作流的雅思备考技能集。适用于 Claude Code、Warp、Cursor 等支持本地 skill 目录的环境。

仓库目标很明确：
- 用一个总入口 `ielts` 做诊断、路由和进度追踪
- 用 5 个子 skill 处理阅读、写作、口语、词汇和成绩诊断
- 把用户进度写入 `~/.ielts/`，让训练跨会话持续

## Skills

| Skill | 作用 |
|-------|------|
| `ielts` | 总入口：摸底、路由、进度报告、计划调度 |
| `ielts-diagnose` | 根据模考/真考成绩生成诊断和备考计划 |
| `ielts-writing` | 写作四维批改、句子级诊断、目标分改写 |
| `ielts-reading` | 阅读错题拆解、T/F/NG 逻辑分析、同义替换提取 |
| `ielts-speaking` | 口语话题分组、万能故事、Part 3 追问框架 |
| `ielts-vocab` | 词汇推送、测试、难词池、间隔重复 |

## 结构约定

每个 skill 目录优先采用两层结构：

- `SKILL.md`：只放执行流程、模式切换、写入规则
- `REFERENCE.md`：只放评分表、题库、词表、搭配库、报告模板等参考资料

这样做的目的不是美观，是为了降低模型漏步骤和无关展开的概率。

## 写入契约

所有会写文件的 skill 都遵守同一条硬约定：

- 只要本轮实际写入了文件，最终回复末尾必须逐行打印 `✅ 已写入 /真实/绝对/路径`
- 不允许只写 `~/.ielts/...`
- 写入失败必须明确报错
- 没有回执，流程就算没完成

这条约定是整个仓库最重要的行为约束之一。

## 安装

把仓库里的 `ielts*` 目录复制到你的 skill 加载路径。

Claude Code / Warp 常见路径：

```bash
git clone https://github.com/LingJingMaster/ielts-claude-skills.git ~/ielts-skills
cp -r ~/ielts-skills/ielts* ~/.agents/skills/
```

Cursor：

- 放到 `.cursor/rules/`
- 或项目自己的 skills 目录

## 数据目录

所有用户进度默认写到：

```bash
~/.ielts/
```

首次运行任一 skill 时会自动创建，也可以手动预创建：

```bash
mkdir -p ~/.ielts/{writing/submissions,reading/submissions,speaking/stories,vocab}
```

这个目录里保存：
- 档案和分数历史
- 写作批改记录和错误模式
- 阅读分析记录和同义替换库
- 口语故事和话题分组
- 词汇进度、难词池、已掌握词表

## 推荐工作流

首次使用：

```text
你：/ielts
AI：问目标分、当前水平、考试时间
你：目标 7.5，现在 L7 R6.5 W5.5 S6，2 个月后考
AI：路由到 /ielts-diagnose，生成计划
```

日常训练：

| 场景 | 建议输入 |
|------|---------|
| 写作批改 | 批改这篇作文 + 题目 + 作文全文 |
| 阅读分析 | 这道为什么错 + 文章 + 题目 + 你的答案 |
| 口语准备 | 帮我做 Part 2 话题分组 + 当季题库 |
| 词汇训练 | 今天背什么 / 考我 |
| 查看进度 | 看看进度 |

模考后复盘：

```text
你：L32 R28 W5.5 S6.5，帮我分析
AI：/ielts-diagnose 更新分数，再路由到对应专项 skill
```

## 维护建议

如果你继续扩展这个仓库，优先保持下面三条：

1. 新增参考数据时，先放 `REFERENCE.md`，不要直接塞进 `SKILL.md`
2. 新增任何写文件逻辑时，必须补写入回执规则
3. 避免把会过时的政策、价格、考试安排写进静态参考文件

## FAQ

Q：数据存在哪？
A：`~/.ielts/`，纯文本 Markdown，可以直接看、改、备份。

Q：换电脑怎么办？
A：把 `~/.ielts/` 整个目录迁过去即可。

Q：AI 给的分数准吗？
A：通常偏高约 0.5 分，建议交叉验证。

Q：可以只用其中几个 skill 吗？
A：可以，但如果你想保留完整进度链，建议保留 `ielts` 总入口。
