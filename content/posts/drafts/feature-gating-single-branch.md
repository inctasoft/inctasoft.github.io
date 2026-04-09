---
title: "It's OK to Ship Broken Code — If It's Feature-Gated"
date: 2026-04-09
author: "Inctasoft"
description: "How feature gating replaces branch management when AI agents ship code in parallel — and why production is just a different config."
draft: true
---

AI agents write code fast. Faster than any branching strategy was designed to handle.

The natural reaction is to create more branches — one per agent, one per feature, one per experiment. But branch management quickly becomes the bottleneck. Merge conflicts pile up. Context is lost between sessions. The coordination overhead eats the productivity gain.

There's a simpler model: **one branch, always. Feature gating does the rest.**

<!--more-->

## The insight

It is perfectly fine to push unfinished, experimental, or even broken code to production — as long as it is **feature-gated off**.

A feature gate is a config flag checked at runtime. If the flag is off, the code path is never reached. From the user's perspective, the feature doesn't exist yet. From the agent's perspective, the work landed safely.

This means:

- Agents ship to `main` continuously, without coordination
- No feature branch ever lives longer than a single session
- Broken code in production is not a crisis — it's just gated

## The branch model that follows

With feature gating, the branching strategy simplifies to its logical minimum:

**One `main` branch. Forever.**

Short-lived `feat/`, `fix/`, and `chore/` branches exist only to satisfy the PR review step — they're created, reviewed, squash-merged, and deleted within minutes. They never accumulate, never conflict, and never require rebasing against a drifting `main`.

The branch is not the unit of isolation. **The flag is.**

## Production is just a different config

This is the key reframe: staging vs. production is not a matter of which code runs. It's a matter of **which flags are on**.

Same codebase. Same container. Different `config.json`.

This means:

- You can run the "full feature" experience for internal testing by flipping flags on
- External users see only the stable, validated subset
- Promoting a feature to production means editing one value, not merging a branch
- Rolling back means editing it back — no `git revert`, no redeploy

## Who owns the promote decision?

**Humans.**

Agents ship code. Humans validate behavior. When a feature is ready, a human flips the flag. This keeps the human in the loop at the only moment that matters — not during every commit, but at the moment of exposure.

The workflow becomes:

```
agent ships → gates off → human tests → flag on → production
```

No code review bottleneck. No deployment ceremony. No release manager.

## A real example

The Valko voice-assistant runs this exact model. One `main` branch. One codebase deployed to three environments (prod, beta, cloud). The environments differ by a single `VALKO_PROFILE` environment variable that selects an `InstallationProfile`.

Each profile is a plain object of booleans and strings:

```ts
export interface InstallationProfile {
  byokEnabled: boolean;
  githubEnabled: boolean;
  tts_provider: 'piper' | 'gcp' | 'none';
  // ...
}
```

Prod has `byokEnabled: false`. Beta has it `true`. Same binary. Same build. Different config.

Multiple AI agents contributed features to this codebase in parallel — some finished, some not. Everything lands on `main`. What reaches users is controlled entirely by which profile they're on.

## The mental shift

Traditional deployment thinking: *"Is the code ready to ship?"*

Feature-gated thinking: *"Is the code safe to land? Is it ready to be turned on?"*

These are different questions. Code can land the moment it compiles and passes tests. It can be turned on days or weeks later, after human validation. The two concerns are fully decoupled.

This is especially important in agentic workflows, where the agent that wrote the code and the human who validates behavior may never interact in the same session. The flag is the handoff point.

---

The bottleneck in AI-assisted development is rarely the code. It's the coordination. Feature gating removes most of that coordination overhead by turning deployment into a config decision and keeping the branch clean.

One branch. All features. Different configs. That's it.

---

*We use this model across all our projects at Inctasoft. If you're building with AI agents and running into branch management pain, reach out: [office@inctasoft.com](mailto:office@inctasoft.com)*
