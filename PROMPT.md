# Dark factory

Build a dark factory for the repository in this workspace, then run it. Intent goes in, shipped software comes out. No human writes or reviews code. I set goals, give access, and approve irreversible actions. Everything else is the factory's job, including operating ELU.

Start by auditing what already exists. Then carry one important user journey all the way to production, and widen from there.

## How the work splits

**Grok Bots run the factory.** They turn goals and evidence into specs, write the code, the scenarios, the twins, and the checks, operate ELU, merge, deploy, and improve the factory itself.

**ELU gives the factory what it can't get from its own computer.** It shows what real users do and where they fail, runs browser agents that use the app the way a real user would, tests each pull request's preview on its exact commit, and compares real users on each release with the previous one.

Anything the Bots can do, they do. ELU is for what they can't.

## Roles

Each role is its own Bot, or its own session with its own instructions. No role grades its own work.

1. **Coordinator.** Owns the queue and the ledger. Assigns work, runs tasks in parallel, enforces budgets and stop rules, and is my single point of contact. Every task has a record in GitHub (an issue or a project item): spec, state, attempts, spend, and links to evidence.
2. **Analyst.** Turns my goals and ELU's real-user evidence into specs, and keeps the backlog ranked by user impact. Intake also covers production errors, dependency and security updates, flaky checks, and performance regressions. A spec states the user problem, a measurable outcome, acceptance criteria, and the critical invariants (authorization, payments, data integrity). Specs live in the repository. Code is generated from specs, never the other way round.
3. **Scenario writer.** Turns each spec into end-to-end user scenarios and creates them in ELU as test agents, so they run against pull requests and production. Writes them from the spec and from real users' failures, never from the code. Builders never see a scenario's steps, only its result and the failure a user would see.
4. **Builders.** Work in parallel, each in its own worktree, one pull request per candidate. Several candidates may compete for the same spec. The gate picks; among passing candidates, the smallest change wins.
5. **Twin builder.** Builds a digital twin of every third-party service the journey touches: payments, email, auth, storage, anything external. A twin reproduces the service's API, errors, rate limits, and edge cases, so scenarios run fast, cheaply, and without touching real accounts. Every twin is checked against the real service's documented behavior and kept current with it.
6. **Verifier.** Runs types, lint, unit tests, property-based tests, and visible scenarios against the twins. If I give you an API key for a model outside the Grok family, every judgment call, such as code review or spec conformance, goes to that model. Without one, the Verifier runs deterministic checks only and says so.
7. **Release operator.** Merges when the gate passes, deploys progressively (a small share of real users first, then everyone), promotes or rolls back on ELU's comparison, and rolls back in one step. Database changes are expand-then-contract, so every release can roll back.
8. **Improver.** Every week, reviews the ledger and ELU's evidence: where tasks stalled, what failed at the gate, what reached users, what each shipped change cost. Changes roles, prompts, routines, and checks through the same pipeline as product code.

## The loop

1. **Intake.** ELU findings, my goals, and maintenance work become specs.
2. **Scenarios.** The Scenario writer adds or updates ELU test agents for the spec.
3. **Build.** Builders open competing pull requests.
4. **Verify.** The Verifier's checks and CI run off the Bots' computer.
5. **Accept.** ELU tests each candidate's preview on its exact commit.
6. **Release.** Progressive deploy; ELU compares real users; promote or roll back.
7. **Learn.** Close the ELU issue once users on the new release stop hitting it, and reopen it if they hit it again. Every failure that reached users becomes a new scenario, so it can't ship twice.

Run on events (a pull request opens, a check finishes, a deploy lands, ELU reports a finding) and on schedules (daily intake and a stall sweep, the weekly review). A task with no progress for a day is resumed or ended as BLOCKED.

## Pass and fail

- A candidate merges only when ELU's verdict on its exact commit is a pass and CI that ran off the Bots' computer is green. Failed, incomplete, pending, or a verdict on another commit is not a pass.
- No score overrides a failed critical invariant.
- Enforce the gate where no Bot can bypass it: protect the default branch and make the gate a required check. Changes to the gate need my approval.
- A release is promoted only on ELU's comparison of real users. A regression rolls it back immediately, and the fix starts as a new spec.

## Evidence

- Grok Bots share one computer. Nothing on it is evidence: not a Bot's report, a local test run, or a file. Evidence is ELU's results, CI that ran elsewhere, and GitHub's record.
- Never invent ELU APIs, credentials, or results. ELU's MCP server describes its tools. Follow their descriptions.
- Secrets live in GitHub, the deploy platform, or your own tool configuration, never in the repository, an issue, or a prompt. Test-account passwords go only in ELU's Settings.

## Stop rules

- Stopping beats a forced pass. BLOCKED and NEEDS_HUMAN are valid results.
- Per task: three attempts, 20 minutes of active work, and $5 of model spend, unless I set other limits. Time spent waiting for ELU, CI, or a deploy does not count. The first limit reached ends the task.
- Conflicting requirements or missing evidence end the task as BLOCKED, never as another attempt.
- Weakening a test, a scenario, a twin, or the gate to get a candidate through is a failed task, reported as one.

## What waits on me

- Goals and priorities.
- Secrets, accounts, and anything that costs money.
- Irreversible actions: deleting data, destructive migrations, making a repository public, and anything sent to customers outside the product.
- Changes to the gate.
- Any verdict ELU does not return. Report NEEDS_HUMAN and wait.

Send these in one message, with the exact step for each. Never ask what you can find out yourself. Nothing else waits on me.

## Set up

Audit first: the ELU tag, previews, branch protection, CI, deploys, and any earlier agents or bots. Reshape what exists instead of stacking a second factory on top. Ask me before pausing earlier automation that could merge or deploy while you rebuild.

1. From the app's directory, run `npx -y @elu-labs/cli --yes --offer=grok` and keep it running. It prints a sign-in link and a code. Send me both. I sign in on that page, or create my ELU account there with the $1,000 credit. The command then adds the ELU tag to the app and connects ELU's MCP server to you. If it names a config file, add that server to your MCP configuration. Confirm by listing ELU's tools. If it times out, run it again.
2. Ship the tag to production through my current release process; it is a tag-only change. Confirm ELU receives a real session.
3. Check pull-request testing through ELU's MCP server. If GitHub isn't connected to ELU, send me the link ELU gives you. Then turn pull-request testing on through ELU's MCP server. Each pull request needs a preview deploy that creates a GitHub deployment for the pull request's head commit and serves the version page ELU describes. Build it if it is missing.
4. Protect the default branch, with the gate as a required check. If GitHub refuses, tell me what it needs.
5. Run CI off your computer on every pull request. Build it if it is missing.
6. Make production deploys progressive, with one-step rollback. Build that if it is missing.

## Deliver

- The running factory: roles, routines, the ledger, twins, the gate, progressive deploys with rollback, the ELU integration, and a README a new Bot can follow.
- One spec carried from an ELU finding to production, with ELU's evidence attached and its ELU issue closed.
- Two sabotage runs. A candidate that passes every visible check but breaks the journey for a user, rejected at the gate. A release that regresses on its first share of users, rolled back.
- Proof that the stop rules end the loop.
- A weekly report: what shipped, time from finding to production, first-pass rate at the gate, rollbacks, spend per shipped change, what is blocked, and every verdict that came from me instead of ELU.
