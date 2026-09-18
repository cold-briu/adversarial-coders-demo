# Lesson 1 — The project manager agent

Goal for this lesson: understand what "adversarial" means here, what a sub-agent is, and end up with a working project manager (PM) agent that gives you a first structured output — a scope definition and a set of clarifying questions — for the gym booking system.

No application code gets written in this lesson. There's nothing to code yet, because the agents that will eventually write code don't exist.

## 1. What "adversarial" means in this repo

Adversarial does not mean two agents debating each other. It means a **writer** and an **error-finder**:

- One agent produces an output (a spec, a design, code, tests).
- A second agent's only job is to find problems with that output — nothing it hasn't checked, nothing it assumes is fine.
- The writer revises against those findings.
- This repeats until the error-finder has nothing left to flag.

That pair is the unit of work for each of the four areas this workflow eventually covers: UX design, spec writing, coding, and testing.

## 2. What a sub-agent is

A sub-agent is a role you define for Claude Code: a prompt/skill file that gives an agent a name, a scope of responsibility, and instructions for how it should behave when invoked. It's not a script and it's not hardcoded logic — it's a role that an agent reads and acts from.

The **project manager agent** is the first sub-agent in this workflow. Its job is to talk to the user, interpret what they're asking for, and (later, once the pairs exist) decide which adversarial pair should handle it. In this lesson it has no pairs to route to yet, and no fixed goal beyond understanding the user's intent — you're not going to pre-write "produce a scope doc" into its instructions. You'll compose its role file yourself, a piece at a time, and see what output naturally comes out of talking to it.

## 3. Architecture: agents live in the repo

Every agent this workflow creates is scoped to this repo, not to your personal Claude Code setup. That means the role files live under `.claude/agents/` at the repo root, one Markdown file per agent, and get committed to git alongside everything else. Anyone who clones this repo gets the same agents — nothing lives in a global, machine-specific config.

A repo-scoped agent file has two parts:

- **YAML frontmatter** — `name` (the identifier you'll invoke it by) and `description` (a short sentence saying what it's for; also what Claude Code uses to decide whether to auto-delegate to it).
- **A body** — plain Markdown that becomes the agent's instructions/system prompt.

## 4. Compose the PM agent's role file

Work through this step by step rather than pasting a finished file:

1. Create `.claude/agents/project-manager.md`.
2. Write the frontmatter: `name: project-manager` and a one-line `description` of what it's responsible for (talking to the user, interpreting their message).
3. Write the body as a behavior, not a goal: it asks questions when something is ambiguous, and it does not write code, specs, or designs itself — that belongs to agents that don't exist yet.
4. Leave its output format open for now. Don't specify "write a scope doc" — that's something you'll discover by using it, not something you engineer in advance.

Keep the file short. If you find yourself writing "step 4: route to X agent," delete it — that behavior doesn't exist until the pairs do.

### "You" vs. abstract, third-person tone

Write `description` and the body in different voices, because they're read by different things:

- **`description` is third-person and abstract.** It's metadata Claude Code uses to decide when to route to this agent — it's never fed to the agent as its own instructions, so it should describe the agent from the outside ("Use this agent to...").
- **The body is second-person, addressed directly to the agent.** It becomes the system prompt the model reads as its own instructions when it plays this role. "You are the project manager for this project..." primes it to inhabit that identity directly. An abstract, third-person body ("This agent talks to the user and asks questions...") reads like documentation about the role rather than instructions to act on, and tends to produce weaker adherence.

## 5. List the available agents

Before invoking anything, confirm Claude Code actually picked up your file. There's no `/agents` wizard as of the current version — instead, just ask Claude Code directly, e.g. *"what agents are available in this repo?"* Claude Code scans `.claude/agents/` and reports back what it finds, including `project-manager` sourced from `.claude/agents/project-manager.md`. If it's missing, the frontmatter is probably malformed — fix it before moving on.

## 6. Spin up the PM and get your first output

Writing the file doesn't start anything by itself — an agent definition is just a role sitting on disk until something invokes it. Once `.claude/agents/project-manager.md` exists and you're working in this repo with Claude Code:

- Claude Code notices the file automatically and lists `project-manager` as an available agent for this repo — nothing to install or register.
- To spin it up, ask Claude Code directly to use it, e.g.: *"Use the project-manager agent to interpret this: I want to build a gym booking system for a gym admin."* Claude Code then runs a fresh agent using your role file as its instructions.
- Being explicit like this matters for the lesson — Claude Code can sometimes auto-delegate to a matching agent based on its `description`, but naming it yourself guarantees the PM (and not the general assistant) is the one that responds.

### Running it without losing your current conversation

Invoking the PM this way doesn't derail whatever you were already discussing with Claude Code:

- The PM runs as a fresh, isolated agent — it doesn't inherit or pollute your current conversation's context. Your main thread stays exactly as it was; only the PM's output lands back into it.
- If you want to keep working in your current conversation while the PM runs longer or you want to poke at it separately, ask for it to run in the background (e.g. *"spin up the project-manager agent in the background to interpret this"*). You get notified when it finishes and can keep talking in the meantime.
- For a fully separate thread — e.g. you want to interact with the PM back-and-forth without touching your main conversation at all — open a second Claude Code session in the same repo (a new terminal tab, same directory). Both sessions see the same `.claude/agents/` files, so the PM behaves identically in either one.

Notice what comes back. If the role file is doing its job, you should see the PM:

- Reflect back a big-picture definition of what it understood the project to be.
- Ask a set of clarifying questions about anything left ambiguous (who are the users, what does "booking" cover, what does the admin need to manage, etc.).

If it doesn't produce that on its own, go back to step 4 and adjust the role file until it does — that iteration is the actual lesson. By the end, you should have a PM role file you wrote yourself and a real scope + questions output for the gym booking system to carry into the next lesson.

## What's next

Lesson 2 introduces the first adversarial pair, once there's a scoped piece of work for it to act on.
