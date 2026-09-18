# Lesson 2 — The UX pair

Goal for this lesson: build the first adversarial pair — a UX writer and a UX reviewer — teach the PM to delegate to it, and use it to turn the accepted brief into a big-picture story breakdown for the gym booking system.

The brief from lesson 1 is the input to this lesson. If you haven't gotten the PM to a brief the user has accepted, go back and finish lesson 1 first — this lesson has nothing to work from without it.

## 1. What's new: a pair, not a single agent

Lesson 1 built one agent. This lesson builds two that only make sense together:

- A **writer** that produces UX/story output.
- A **reviewer** whose only output is a list of problems with what the writer produced — it never writes the content itself.

The loop: the writer drafts, the reviewer reviews, the writer revises against the reviewer's findings, and this repeats until the reviewer has nothing left to flag. That's the whole mechanism — no voting, no negotiation, no debate between them.

Everything else about composing a role file is unchanged from lesson 1: `.claude/agents/`, YAML frontmatter with `name` and a third-person `description`, a second-person body written directly to the agent.

## 2. Scope the pair by domain, not by task

Resist writing "this agent's job is to produce big-picture stories." That would make you rewrite the file again in lesson 3 when detailed stories come up, and again whenever real UX design work starts later.

Instead, scope both agents to the domain they own for the life of the project:

- **UX writer** — owns turning whatever input the pipeline hands it (a brief, later a set of stories) into the UX/story artifact the pipeline is currently asking for.
- **UX reviewer** — owns finding problems with whatever the writer produces in that same domain.

Big-picture stories are simply the first thing this pair is asked to do, because that's what the document pipeline calls for right after a brief is accepted. Nothing in their role files should say "big picture" — that comes from what the PM asks for at invocation time, not from a hardcoded task.

## 3. Compose the two role files

Same process as lesson 1, done twice:

1. Create `.claude/agents/ux-writer.md` and `.claude/agents/ux-reviewer.md`.
2. Write each `description` in third person, stating what the agent owns and when to invoke it (writer: given an approved input, produce the next UX/story artifact; reviewer: given the writer's output, find problems in it).
3. Write each body in second person, addressed to the agent:
   - The writer's body: what it owns, what "done" looks like for a draft (traceable back to the brief, no invented scope), and that it revises based on the reviewer's findings rather than defending its first draft.
   - The reviewer's body: it only reports problems — missing user types, stories that don't trace back to the brief, scope creep into implementation detail, anything vague enough to cause disagreement later. It does not rewrite or suggest polished replacement text; it flags.
4. Neither file should mention "big-picture stories" as a fixed deliverable, and neither should route to the other on its own — the loop between them is driven by the PM, not by the agents themselves.
5. Give the writer permission to stop and escalate. When a detail genuinely isn't answered by the brief and neither the writer nor the PM can reasonably infer it, the writer's body should have it surface that gap to the user directly instead of guessing. A guessed-at assumption that turns out wrong doesn't get caught until much later in the pipeline — after a design, a spec, or code has been built on top of it — and unwinding that is a refactor or a rollback, not a quick fix. Pausing to ask costs one round trip; guessing wrong can cost the whole downstream chain built on that guess.

## 4. Update the PM to delegate

This is the first real edit to `project-manager.md` since lesson 1. Today its body says, in effect, "if the agent doesn't exist, say so instead of doing the work." Now that the UX pair exists, add to its body: when the user asks for a story breakdown (or other UX work), the PM delegates to the UX writer, runs the writer/reviewer loop until it converges, and only then brings the result back to the user.

Keep this addition scoped to delegation behavior — the PM still doesn't write UX content itself, it just knows who to hand it to now.

## 5. Run the loop and watch it converge

With the brief in hand, ask the PM for a big-picture story breakdown. Watch for:

- The PM handing the brief to the UX writer.
- The writer producing a first draft of stories.
- The reviewer responding with specific problems, not a rewrite.
- The writer revising against those specific problems.
- The loop repeating until the reviewer reports nothing left to flag.

If the reviewer approves a first draft with no findings at all, be suspicious — go back to step 3 and check that its role file actually pushes it to look for something, rather than rubber-stamping.

## 6. Save the output and hit the approval gate

Once the loop converges, save the result under `docs/` in the repo — e.g. `docs/stories/big-picture.md` — so it's a real artifact in the project's history, not just chat output. Bring it to the user for the approval gate the document pipeline requires: they approve it as-is, or send it back with changes for the writer/reviewer loop to run again.

Approval here is what unlocks lesson 3 — this same UX pair, asked to expand the approved big-picture stories into detailed, per-story documents.

## What's next

Lesson 3 reuses the UX pair built here — no new agents — to go from big-picture stories to detailed stories, one document per story.
