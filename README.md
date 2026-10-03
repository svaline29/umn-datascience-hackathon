# hackathon

Agent skills for this project. Each skill is one folder under `skills/`, in the [Agent Skills](https://agentskills.io) layout (`SKILL.md`, plus `references/` and `scripts/` when needed). Cursor, Claude, Codex, and any other agent that reads `SKILL.md` can use them.

## Skills

| Folder | What it does |
| --- | --- |
| `skills/handwritten-slides-a11y/` | Turns a PDF of handwritten slides, one slide per page, into screen-reader HTML with Presentation MathML. Checks Revised Section 508 (WCAG 2.0 Level A and AA) and WCAG 2.1 Level AA. Stops for a person to confirm uncertain handwriting before the transcript is called ready. |

Point the agent at a skill folder and give it the input that skill expects. To install one as a named skill, link that folder:

```bash
# Cursor, for every project
mkdir -p ~/.cursor/skills
ln -s "$(pwd)/skills/handwritten-slides-a11y" ~/.cursor/skills/handwritten-slides-a11y

# Claude Code
mkdir -p ~/.claude/skills
ln -s "$(pwd)/skills/handwritten-slides-a11y" ~/.claude/skills/handwritten-slides-a11y
```
