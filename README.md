# GTM Claude Skills

Claude Skills for sales, marketing and RevOps work, from [Cabot Insights](https://cabot-insights.com).

## gtm-model-picker

Tells you which Claude model (Fable, Opus, Sonnet or Haiku), effort level and subagent setup to use for a go-to-market task, with a rough cost note.

Ask it things like:

- "Which model should I use to tag 10,000 job titles to personas?"
- "What effort level for writing outbound sequences per persona?"
- "Should I use subagents to research signals on 500 accounts?"

It answers in a fixed format:

```
Recommended model: Haiku 5.5
Effort: low; re-run failures at medium
Subagents: Yes, Haiku workers on batches of a few hundred
Why: Narrow, checkable classification at high volume.
Rough cost note: Estimate, assuming ~100 input and 20 output tokens per title: roughly $0.20.
```

### Install

**Claude Code** (all projects):

```bash
git clone https://github.com/cabotinsights/gtm-claude-skills.git
mkdir -p ~/.claude/skills
cp -r gtm-claude-skills/gtm-model-picker ~/.claude/skills/
```

For one project only, copy the folder into that project's `.claude/skills/` instead.

**Claude apps (claude.ai, desktop):** download the `gtm-model-picker` folder, zip it, and upload it as a custom skill in Settings.

### What happens once it is installed

- At the start of a session, Claude sees only the skill's name and one-line description. The rest of the file costs nothing until it is needed.
- When you ask a question that matches the description (which model, effort level or setup for a GTM task), Claude loads the full `SKILL.md` and follows it: it works out task type, volume, reasoning needed and cost sensitivity, then answers in the format above.
- In Claude Code you can also call it directly by typing `/gtm-model-picker`.
- It gives advice only. It does not switch your model, change your effort setting or run anything. You make the change yourself, for example with `/model` in Claude Code.

## Source

Model names, prices, effort defaults and benchmark figures come from Anthropic's guide [Which Claude model should you build with?](https://claude.dev/blog/which-claude-model-should-you-build-with/). The guide's examples are about coding and knowledge work; the GTM examples in this skill are our recommendations, not Anthropic's claims.

Prices and models change. Check the guide before relying on the cost figures, and open an issue if something is out of date.

## Licence

MIT
