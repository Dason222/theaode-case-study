[← Back to overview](../README.md)

# The AI-Augmented Engineering Workflow

I built and operate AODE solo, and a large part of how that's possible is that I don't just *use* AI — I engineered a supervised AI development and operations system around the platform. That system is itself a portfolio piece.

The mental model is simple: the AI (Claude Code CLI, running against the production environment) is treated like a capable junior engineer. It can read anything, propose anything, and execute a lot — but it works inside hard guardrails I designed, its output is verified before anything ships, and every lesson it learns is written down so the next session starts smarter. AI autocomplete makes you type faster. A supervised AI operations system changes what one person can run.

## Guardrails: the environment makes dangerous things impossible

An AI agent with shell access to a production server is either a force multiplier or an incident generator. The difference is the guardrail layer, and I treat that layer as a first-class engineering artifact:

- **Permission allowlists.** Read-only diagnostics and known-safe operations (log inspection, container stats, health checks, builds) are pre-approved so routine work flows without friction. Anything outside the allowlist requires explicit human approval.
- **A pre-execution hook that blocks destructive commands.** Before any shell command runs, a hook inspects it and hard-blocks anything that would stop, remove, or destructively mutate protected services or reset the production database. The AI cannot talk its way past this — the command simply never executes.
- **Deny rules as defense in depth.** The same destructive patterns are also blocked at the permission-config level, so a bug in one layer doesn't open a hole. Two independent mechanisms have to fail before a dangerous command reaches the shell.

The governing principle: **the AI can propose anything, but the environment makes the dangerous things impossible.** I'd rather constrain the sandbox than trust the model's judgment on what not to touch. This is the same philosophy as least-privilege IAM or CI environments that can't reach prod secrets — applied to an AI coworker.

## Custom automation: teaching the AI the operations playbook

Generic AI assistance plateaus fast on a real production system. So I built a set of custom automation skills — reusable, invocable workflows that encode how *this* platform is operated:

- **Guided release workflow.** One command walks a release end to end: version bump, production build, update-package assembly, checksum generation, and post-release verification. The AI executes the steps; the workflow guarantees none get skipped or reordered.
- **System health checks.** A fast diagnostic pass over containers, database health, disk, memory, routing, and recent errors — run at the start of sessions so the AI always works from the real current state instead of assumptions.
- **One-command incident diagnostics.** When something breaks, a single command captures a full snapshot — container states, logs, alerts, resource usage — as a triage baseline and a rollback reference before any fix is attempted.
- **Module scaffolding.** New marketplace modules are scaffolded end to end: routes, UI, database tables, and catalog registration, all following the platform's established conventions instead of whatever pattern the model improvises.
- **Deployment verification.** After a deploy, a verification step fetches the live site and greps the actually-served JavaScript bundles for the expected values — proving a change shipped to users, not just that a build succeeded somewhere.
- **Disk-space cleanup routines.** Automated pruning of build caches, stale packages, and old artifacts, because on a self-hosted platform, a full disk is an outage.

Each of these started as a manual procedure I performed, debugged, and then codified. The AI doesn't invent operations knowledge — it executes mine, consistently, at 3 a.m. if needed.

## Persistent knowledge: every incident makes the system smarter

AI sessions are stateless by default; production systems are not. I maintain a persistent knowledge layer that carries across every session:

- **Architecture notes** — how the deployment engine, routing, auth, and update system actually fit together, so the AI never has to rediscover the system from scratch.
- **A troubleshooting knowledge base** — every non-obvious failure I've hit (build quirks, token edge cases, container gotchas) with its root cause and fix. When a known symptom reappears, the fix is a lookup, not an investigation.
- **Release and incident runbooks** — step-by-step procedures with the verification checkpoints baked in.

This is the AI equivalent of onboarding docs, and it compounds: every incident, once solved, is solved forever. A fresh session today starts with the accumulated judgment of every previous one.

## Multi-agent execution

For larger pieces of work, I don't run one long AI conversation. Plans get decomposed into independent tasks and dispatched to parallel subagents — one implementing an API route while another builds the UI and a third writes migration logic. The main session acts as the reviewing engineer: it collects results, checks them against the plan, and integrates. Nothing a subagent produces ships without passing through that central review. It's a small engineering team's structure, with me as the only human in it.

## Verification culture: evidence before claims

The hard rule of this whole workflow is that **AI output is never trusted blind.** "It should work now" is not a state my system recognizes. Instead:

- Builds must actually pass — a green compile is the entry ticket, not the finish line.
- Deployments are verified against the live site, down to grepping served bundles for the expected change.
- Update packages and installer artifacts are confirmed by SHA256 checksum before they're published.
- After incidents, the fix is validated against the original failing symptom, not just against theory.

Every claim of "done" has to be backed by observed evidence. This single discipline is what makes it safe to let an AI operate at speed on production.

## Beyond code

The same toolchain extends past engineering. The product's promotional and tutorial videos are generated programmatically with **Remotion** — video defined as React components, rendered to MP4 — which means marketing assets are versioned, reviewable, and regenerable like any other code. And **Playwright** browser automation does double duty: verifying UI changes against the real running product, and capturing genuine product footage for those videos. One skill set, applied from backend ops to marketing output.

## The honest version

AI multiplies my output roughly 5–10x on routine engineering — CRUD endpoints, UI wiring, migrations, diagnostics, release mechanics. But that multiplier is not a property of the model. It only exists because of the system wrapped around it: guardrails that make destructive mistakes impossible, verification that catches wrong output before it ships, runbooks that turn past incidents into instant fixes, and architecture decisions that a model cannot make for you.

Anyone can prompt an AI. Designing the supervised system that lets an AI safely operate a production platform — that's the skill I'm selling.

---

[← Back to overview](../README.md) · Live demo: [demo.theaode.com](https://demo.theaode.com)
