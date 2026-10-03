# handwritten-slides-a11y

Agent skill that turns a PDF of handwritten slides, one slide per page, into screen-reader HTML with Presentation MathML. The checklist is Revised Section 508 (WCAG 2.0 Level A and AA) and WCAG 2.1 Level AA. Uncertain handwriting stops for a person to confirm before the transcript is called ready.

The skill is the `SKILL.md` file in this folder, plus `references/` and `scripts/`. That layout follows the [Agent Skills](https://agentskills.io) format, so Cursor, Claude, Codex, and any other agent that reads `SKILL.md` can use it. Nothing here is tied to one product.

Point the agent at this directory and give it a PDF. To install it as a named skill, link this folder into that product's skills directory:

```bash
# Cursor, for every project
mkdir -p ~/.cursor/skills
ln -s "$(pwd)" ~/.cursor/skills/handwritten-slides-a11y

# Claude Code
mkdir -p ~/.claude/skills
ln -s "$(pwd)" ~/.claude/skills/handwritten-slides-a11y
```
