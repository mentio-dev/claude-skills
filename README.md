# Claude Code skills for Mentio

Skills that turn [Mentio](https://mentio.dev) mentions into a routine inside
Claude Code. Mentio watches Reddit, Hacker News, X, GitHub, Bluesky, LinkedIn,
Stack Overflow, DEV, YouTube and news for your keywords and scores every match
for relevance, sentiment and intent; its MCP server gives Claude those mentions
as tools. These skills are the procedures on top.

## Skills

| Skill | What it does |
| --- | --- |
| [`reddit-morning`](./reddit-morning/SKILL.md) | The five-minute Reddit routine: find yesterday's questions and buying signals with a fit verdict, draft replies under your rules, clean up when you are done. Nothing is posted for you. |

The routine and the rules behind it are written up in
[How to find customers on Reddit with Claude Code](https://mentio.dev/blog/find-customers-on-reddit/).

## Install

1. Connect the Mentio MCP server once (an account starts with $5.80 of credit, no card):

   ```bash
   claude mcp add --transport http mentio https://mcp.mentio.dev/mcp
   ```

2. Copy a skill into your project (or into `~/.claude/skills/` for every project):

   ```bash
   git clone https://github.com/mentio-dev/claude-skills
   mkdir -p .claude/skills && cp -r claude-skills/reddit-morning .claude/skills/
   ```

3. Put your product and reply rules in the project's `CLAUDE.md`. [`templates/CLAUDE.md`](./templates/CLAUDE.md) is the shape; fill in the placeholders.

4. In Claude Code: `/reddit-morning`.

## The rules the skills keep

Only threads you would answer without a product to sell. Answer first, disclose,
no pitch. No DMs, no duplicate replies. A person posts every reply by hand.
The skills find, sort and draft; you decide and click.

## Contributing

Issues and skill ideas are welcome here. The source of truth is the Mentio
monorepo, which publishes this repository; a pull request here is read and
carried over, not merged in place.

MIT.
