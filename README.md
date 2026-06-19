<div align="center">

# 🥋 Sensei

*The code is ground truth. Stay sharp.*

**A Claude Code skill that quizzes you on the codebase you're working in.**

[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-7C3AED)](https://docs.claude.com/en/docs/claude-code)
[![Language agnostic](https://img.shields.io/badge/language-agnostic-2ea44f)](#)
[![Git required](https://img.shields.io/badge/requires-git-F05032)](#)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow)](./LICENSE)

</div>

---

The more I let Claude write code, the more I worry about losing my skills and project understanding. I plan, implement, review, ship, and repeat, but with the volume of code I see, I can't keep it all in my head. I need a way to stay sharp, and to make sure I'm not just following the flow of the repo without understanding it.

Sensei is my answer to that. It reads the repo you're in, recent commits, docs, ADRs, whatever the project uses, and then turns it around on you: it asks the questions instead of answering them. If you get something wrong, it explains it (pointing at the real file), and remembers to ask you again next time if you don't remember (flashcard's style).

It doesn't care what language you write in. It figures out the stack from the repo itself, so it behaves the same in a Go service as in a React monorepo.

## Table of Contents

- [What it does](#what-it-does)
- [Install](#install)
- [Using it](#using-it)
- [A session, start to finish](#a-session-start-to-finish)
- [How it remembers](#how-it-remembers)
- [Layout](#layout)
- [Hacking on it](#hacking-on-it)
- [License](#license)

## What it does

- **Asks, doesn't tell.** Every question and correction comes from files and commits it read this session, not from what a "typical" project usually looks like. If it can't find it in the code, it won't claim it.
- **Tracks the git history.** It knows what changed since you last sat down with it, and tends to quiz the *why* behind those changes.
- **Remembers your weak spots.** Uses a flashcard-like system.
- **Trusts code over docs.** An ADR marked `proposed` that never got built won't be taught as fact, it checks the status and verifies against the actual code.
- **Stays out of your repo.** Your progress lives outside the project, so nothing leaks into the codebase.

## Install

It's a [Claude Code](https://docs.claude.com/en/docs/claude-code) skill, so it just needs to live in your skills directory:

```bash
git clone git@github.com:guilbill/sensei.git ~/.claude/skills/sensei
```

Claude Code picks it up on its own. Restart the session if one's already open.

## Using it

From inside any git repo:

```
/sensei
```

Or just say what you want:

- *"Quiz me on this project."*
- *"What changed since last time?"*
- *"Keep me sharp."*

You can ask to focus on something specific if you want:

```
/sensei focus on the auth flow
```

## A session, start to finish

It always runs the same four beats: **brief → quiz → teach → record.**

First a short brief, what's new since last time and what you're about to be tested on. Then the quiz, leaning on *why* more than *what*. When you miss one, it explains it with a real reference and asks a simpler version to make sure it stuck. At the end you get a quick read on what's solid, what's still shaky, and your progress file gets updated.

## How it remembers

Progress is kept per project in `~/.claude/skills/sensei/state/<project>.md`, outside the repo. Each file holds a cache of slow-moving facts (stack, layout, the big decisions), a table of how well you know each topic, and a few notes for next time. Lowest score + oldest test date wins the next question. Answer well and the score goes up; blank on it and it comes back sooner.

## Layout

```
sensei/
├── SKILL.md      # the whole tutor lives here
├── README.md
├── .gitignore    # ignores state/
└── state/        # your progress, per project (git-ignored)
    └── <project>.md
```

## Hacking on it

All the behaviour is in [`SKILL.md`](./SKILL.md), change it there and run `/sensei` in any repo to see the difference. Two things worth keeping if you do: don't assume a stack (let it discover one), and don't let it teach anything it hasn't verified in the code.

## License

[MIT](./LICENSE)
