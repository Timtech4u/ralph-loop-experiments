# I Let an AI Agent Run Overnight. Here's What I Learned About Ralph Loop.

Spent the weekend exploring Ralph Loop, the autonomous AI coding technique that's been making rounds in the AI engineering community. The concept is simple: instead of babysitting Claude or GPT through every step, you set up a loop that keeps feeding the same prompt until the task is done.

The original idea comes from Geoffrey Huntley. The core insight? Let the AI see its own mess. Each iteration, it reads the files it modified, sees what worked and what didn't, and iterates toward a solution. No hand-holding required.

## How It Works

The basic loop is almost embarrassingly simple:

```bash
while true; do
  cat PROMPT.md | claude
done
```

Your PROMPT.md contains the task spec, success criteria, and a completion signal. The AI works, tries things, fails, sees the failures in the files, and tries again. When it succeeds, it outputs a completion tag and the loop stops.

State lives in files, not in the conversation. That's the key difference from just chatting with an AI. The filesystem becomes the memory.

## What You Can Build With It

The technique works best for tasks with clear completion criteria:

**Codebase migrations.** "Convert all JavaScript files to TypeScript. Output DONE when `npm run typecheck` passes."

**Test coverage.** "Write tests until coverage hits 80%. Run `npm test` after each change."

**Bug fixes.** "Fix all failing tests. Output COMPLETE when `npm test` exits 0."

**Documentation.** "Add docstrings to all public functions. Verify with `pydoc`."

The pattern is always the same: define success in terms of a command that can be verified, let the AI iterate until that command passes.

## Fresh Context vs Accumulated Context

Here's where it gets interesting. There are two schools of thought.

One approach keeps everything in a single session. The AI accumulates context as it works. Simple to implement, but the transcript grows and old failures can pollute future attempts.

The other approach starts fresh each iteration. New session, clean context, but the AI re-reads the files to understand where it left off. This is closer to how Huntley originally described it.

I found the fresh context approach more reliable for longer tasks. When you're running 50+ iterations overnight, accumulated context becomes a liability. The AI starts hallucinating based on things it tried hours ago.

## Multi-Model Review Gates

One addition I found valuable: using a second model as a reviewer. After the primary agent makes changes, a different model reviews the work before marking the task complete.

Two models catch what one misses. The reviewer doesn't share the implementer's blind spots.

Tools like RepoPrompt (macOS) or OpenAI's Codex CLI work well for this. The review blocks progress until it returns a SHIP verdict. No rubber-stamping.

## Integration With Coding CLIs

Ralph Loop isn't tied to any specific tool. The same pattern works with:

**Claude Code** for complex reasoning tasks
**OpenAI Codex** for code generation with review capabilities
**Cursor** in headless mode
**Aider** for git-aware changes

The key is having a CLI that accepts a prompt and outputs to stdout. Wrap it in a bash loop, point it at your PROMPT.md, and you have an autonomous agent.

Some tools like flow-next bundle this into a complete workflow with task management, dependency tracking, and receipt-based verification. But you can start with just bash and your favorite AI CLI.

## When It Works (and When It Doesn't)

Ralph excels at mechanical tasks with verifiable outcomes. Migrations, test fixes, documentation, refactoring with clear patterns.

It struggles with tasks requiring design judgment, ambiguous requirements, or human feedback loops. Don't expect it to architect your system. Do expect it to implement a well-specified feature while you sleep.

The technique also burns through API credits quickly. Set iteration limits. Monitor costs. A runaway loop can get expensive.

## Getting Started

If you want to try it:

1. Write a PROMPT.md with your task and success criteria
2. Include a verification command the AI can run
3. Define a completion phrase the AI outputs when done
4. Wrap your preferred AI CLI in a bash loop
5. Start with a small task and watch it work before going AFK

The magic isn't in the tooling. It's in writing prompts that converge toward correct solutions. That's a skill worth developing.

## The Takeaway

Ralph Loop shifts the bottleneck from "can the AI do this?" to "can I specify this clearly enough?" Most tasks that seem too complex for AI are actually just underspecified.

When you write a prompt that includes verification commands and clear success criteria, you're doing the hard work upfront. The AI handles the iteration.

That's a trade I'm happy to make.
