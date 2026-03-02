# smallpower

[简体中文](./README.zh-CN.md)

smallpower is a content-first repository of reusable agent skills.
It is designed to help AI agents and engineering teams run repeatable workflows with clearer outputs.

## What is in this repository

- Skill packages under `skills/`
- Supporting references and templates used by those skills
- Real meeting transcript samples (`.srt`) for documentation/analysis scenarios

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

## Included skills

| Skill                        | Purpose                                                                                                       | Main Path                                    |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| `slides-revealjs`            | Build or update reveal.js slide decks with practical patterns, plugin usage, and delivery checklists.         | `skills/slides-revealjs/SKILL.md`            |
| `organizing-meeting-reports` | Convert transcripts/notes/SRT files into structured meeting reports with action items and clarification flow. | `skills/organizing-meeting-reports/SKILL.md` |

## Language conventions

To keep writing consistent across the project:

- All docs in `skills/slides-revealjs` must be written in English.
- All docs in `skills/organizing-meeting-reports` must be written in Simplified Chinese.

## How to use

1. Open the target skill's `SKILL.md`.
2. Follow the workflow and guardrails in that file.
3. Reuse local references/templates instead of rewriting from scratch.

This repository has no required build step and can be used directly as a documentation and skill source.

## Contributing

- Keep changes scoped to the affected skill.
- Preserve existing structure in `references/` and `templates/`.
- Update both language READMEs when project-level information changes.

## License

MIT. See [LICENSE](./LICENSE).
