# The Church of Ink — Claude Code Context

## Commit Convention

Prefix every commit message with a context tag in brackets, then describe
what changed and why.

```
[landing-page] implement reveal sequence and typography
[logo] add hand-drawn mark SVG to index.html
[WIP] testing purple glow opacity values
[HOTFIX] fix mobile viewport overflow
```

## After Running a Structured Prompt

When you execute a prompt from `docs/prompts/ready/`, the code work is only
HALF the job. You MUST also complete the post-execution checklist:

1. Verify all success criteria in the prompt pass
2. Move the prompt file from `ready/` to `done/`
3. Update the prompt index (`docs/prompts/README.md`)
4. Update `AGENTS.md` if the prompt changed architecture or added features
5. Commit everything together: `[Prompt P0XX complete] summary`

## Key Files to Read Before Any Session

```
AGENTS.md                        — brand context, constraints, architecture
docs/prompts/README.md           — prompt library index
```

## Project-Specific Rules

- This is a single static HTML file. No build step, no framework, no npm.
- All CSS and JS are inline in `index.html`. No external stylesheets or scripts.
- Only external dependency is Google Fonts (preconnect + link tag).
- Test on mobile viewport before committing. The page must not scroll.
- The reveal/load animation sequence is the core UX — do not simplify it
  without explicit approval. Timing values are intentional.
