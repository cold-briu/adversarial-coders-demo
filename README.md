# Adversarial coders demo

This repo is a step-by-step guide to building an **adversarial agentic workflow** on top of Claude Code.

## The idea

A **project manager agent** talks to the user, interprets each message, and routes the work to one of the following adversarial pairs:

- **UX design** — one agent writes the interaction/visual design, the other finds errors in it
- **Spec writing** — one agent writes the requirements/spec, the other finds errors in it
- **Coding** — one agent writes the implementation, the other finds errors in it
- **Testing** — one agent writes the tests, the other finds errors in them

Each pair works adversarially: one agent writes the output, the other agent's only job is to find errors in it. The writer revises against those findings until the reviewer has nothing left to flag, then the result is handed back to the project manager. The whole workflow runs inside Claude Code, using its native support for custom roles, skills, and project files.

## The document pipeline

This is the most important thing to understand about the workflow: it doesn't happen in one shot. It moves through stages, and every stage produces a document that the user has to approve — or send back for changes — before the next stage starts. Nothing advances silently, and nothing skips ahead of an approval.

A representative path through the pipeline:

1. The user discusses the project with the PM until they reach a shared understanding → the PM writes a **brief**.
2. The user asks for a big-picture breakdown → the PM delegates to the relevant pair, which writes **high-level stories** (the shape of the product, not the detail).
3. Once the user approves the big picture, each story is expanded into **detailed stories** — a document per story.
4. The user approves the detailed stories, or sends them back for changes.
5. A story is picked, and its **spec** is written — this is where the writer/error-finder pair for that area actually kicks in.
6. The same pattern repeats per story as the project moves forward: write a document, run it through its adversarial pair, get user approval, move to the next document (design, then code, then tests).

Every document lives in the repo alongside the code, so the project's history of decisions stays traceable, and the user is a checkpoint at every stage — never optional, never skipped.

## What you'll learn

This repo teaches you how to define the roles, skills, and supporting files needed for these agents to know how to proceed in a granular, predictable way — so the workflow is reliable rather than ad hoc.

## How you'll learn it

The lessons are taught hands-on, through the setup and construction of a real example project: a **gym booking system for a gym admin**. As the system is built out feature by feature, each step introduces the next piece of the adversarial workflow.

No application code gets written until the agent responsible for writing it exists — the workflow is built one role at a time, starting with the project manager agent.

## Lessons

Start at [`lessons/index.md`](lessons/index.md).
