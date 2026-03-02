# smallpower

[English](./README.md)

smallpower 是一个以内容为核心的可复用 Agent Skills 仓库。
它用于帮助 AI Agent 与工程团队以更稳定的流程产出更清晰的结果。

## 仓库内容

- 位于 `skills/` 下的技能包
- 为技能提供支持的参考资料与模板
- 用于文档整理/分析场景的会议转录示例（`.srt`）

```text
.
├── AGENTS.md
├── skills
│   ├── organizing-meeting-reports
│   │   ├── SKILL.md
│   │   └── templates
│   └── slides-revealjs
│       ├── SKILL.md
│       └── references
├── LICENSE
└── README files
```

## 已包含技能

| 技能                         | 用途                                                                     | 主要路径                                     |
| ---------------------------- | ------------------------------------------------------------------------ | -------------------------------------------- |
| `slides-revealjs`            | 创建或更新 reveal.js 演示文稿，覆盖实用布局模式、插件使用与交付检查。    | `skills/slides-revealjs/SKILL.md`            |
| `organizing-meeting-reports` | 将转录文本/会议纪要/SRT 整理为结构化会议报告，并提取行动项与待确认问题。 | `skills/organizing-meeting-reports/SKILL.md` |

## 语言约定

为保持项目文档一致性：

- `skills/slides-revealjs` 目录下的文档统一使用英文。
- `skills/organizing-meeting-reports` 目录下的文档统一使用简体中文。

## 使用方式

1. 打开目标技能的 `SKILL.md`。
2. 按其中定义的工作流与约束执行。
3. 优先复用本地 `references/` 与 `templates/`，避免重复编写。

该仓库无需构建步骤，可直接作为技能与文档资源使用。

## 贡献说明

- 变更应尽量限定在对应技能目录内。
- 保持 `references/` 与 `templates/` 目录结构稳定。
- 若修改项目级信息，请同步更新中英文 README。

## License

MIT，详见 [LICENSE](./LICENSE)。
