# my-skills

Agent Skills 收集库。

参考[Anthropics官方skills集合](https://github.com/anthropics/skills)

## 项目结构

```
my-skills/
├── .claude/skills/     # 仅含元数据的 SKILL.md（Claude 优先加载）
└── .agent/skills/      # 完整内容的 SKILL.md（完整指令）
```

每个 skill 采用双位置存储，避免重复维护：
- `.claude/skills/{name}/SKILL.md` - 包含 YAML 元数据，引用 `.agent` 版本
- `.agent/skills/{name}/SKILL.md` - 包含完整指令、工作流、示例

## 已有 Skills

| Skill | 说明 |
|-------|------|
| [skill-creator](.agent/skills/skill-creator/SKILL.md) | 从 GitHub 项目、文章、SOP 或流程描述生成新的 Agent Skills |
| [macos-software-recommender](.agent/skills/macos-software-recommender/SKILL.md) | 基于 awesome-mac 推荐 macOS 软件，采用残酷诚实模式：陈述事实而非主观评价、验证所有软件状态、明确风险边界与取舍 |

---

*使用 skill-creator 创建新 skill 时，此表格会自动更新*
