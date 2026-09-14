---
name: reddit-morning
description: The daily Reddit routine through the Mentio MCP server. Finds yesterday's Reddit questions and buying signals for the product named in CLAUDE.md with a fit verdict, drafts replies for the ones the user picks under the reply rules, and marks them done or ignored when the user says so. Use it when the user says "reddit morning", "run the routine", or asks what is worth answering on Reddit today.
---

# Reddit morning

Three steps, in order. Every mention comes from Mentio's MCP tools; nothing is
posted to Reddit by this skill, ever. The person posts by hand.

Read `CLAUDE.md` first for the product name, the reply rules and the voice. If
there is no product name, ask for it before step 1.

## 1. Find what is worth answering today

Call `search_mentions` twice, both with `platforms: ["reddit"]`, `relevant: true`,
`since` set to yesterday morning in the user's timezone, newest first:

- once with `intent: "question"`
- once with `intent: "buy_intent"`

Merge and dedupe by mention id. List each one as a numbered line:

```
N. r/<subreddit>: <what the person is actually asking, one line>. Fit: yes | partly | no, <why>.
```

"Fit" is whether the product is an honest answer to that exact ask. Say no
freely: the no verdicts are what keep the routine out of spam. Skip anything
older than a day even if the search returns it. If nothing came back, say so
and stop.

## 2. Draft replies for the ones the user picks

Wait for the user to name numbers. For each, call `get_mention` to read the
full post and the thread context, then draft one reply that follows every rule
in `CLAUDE.md`. If `CLAUDE.md` has no rules, use these:

- Answer the question first and completely.
- Mention the product only if it is genuinely the answer, and when you do, say
  the user works on it.
- No links unless the person asked for one.
- Under 120 words, no bullet points, no marketing words.

Print the drafts with their number. Do not post them anywhere. Remind the user
to paste, tweak and reply from their own account.

## 3. Clean up when the user says "done"

Call `update_mention` for every mention from the step 1 list:

- the ones the user replied to: `status: "done"` with the note `replied <today's date>`
- the rest: `status: "ignored"`

Tomorrow's step 1 then only sees what is new.

## Notes

- Instant alerts for buying signals are set up once, not here: `create_alert`
  with an instant rule filtered to Reddit and `buy_intent`, to a Slack,
  Telegram, email or webhook channel from `list_channels`.
- If the MCP server is not connected, tell the user the one-liner:
  `claude mcp add --transport http mentio https://mcp.mentio.dev/mcp`.
