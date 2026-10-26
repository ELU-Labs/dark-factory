# Dark factory

Build a dark factory for the repository in this workspace. Intent goes in, shipped software comes out. No human writes or reviews code. I set goals and approve irreversible actions. Everything else is the factory's job.

Start with one important user journey and take it all the way to production. Then widen.

## Two sides

The line between the two sides is the point of the factory.

**Grok Bots build.** They write the specs, the code, the twins, and the visible checks, and they operate the pipeline.

**ELU judges.** It runs outside your computer, where no Bot can see or change it. It keeps the hidden scenarios, tests every candidate the way a real user would, and watches real users after every release. Its verdicts are the only evidence that ships code.

Never do ELU's job yourself.

## Grok roles

1. **Analyst.** Turns my goals and ELU's real-user evidence into a spec: the user problem, a measurable outcome, acceptance criteria, and the critical invariants (authorization, payments, data integrity). Specs live in the repository. Code is generated from specs, never the other way round.
2. **Builders.** Work in parallel, each in its own worktree, one pull request per candidate. Several candidates may compete for the same spec. The gate picks.
3. **Twin builder.** Builds a digital twin of every third-party service the journey touches: payments, email, auth, storage, anything external. A twin reproduces the service's API, errors, rate limits, and edge cases, so scenarios run fast, cheaply, and without touching real accounts. Every twin is checked against the real service's documented behavior.
4. **Verifier.** Runs types, lint, unit tests, property-based tests, and visible scenarios against the twins. Never weakens an assertion or changes an expected result to get a pass. Every judgment call goes to a model outside the Grok family, through its own API key; ask me for one. Without one, the Verifier runs deterministic checks only and says so.
5. **Release operator.** Merges when the gate passes, deploys progressively (a small share of real users first, then everyone), rolls back in one step, and reports.

## ELU roles

1. **Acceptance judge.** Tests each pull request's preview on its exact head commit with its own browser agents, and returns a verdict bound to that commit.
2. **Scenario keeper.** Holds the hidden scenarios, including ones written from real users' failures. Bots never write, read, or edit them.
3. **Release judge.** Compares real users on the new release with the previous one, and decides promote or roll back.

ELU's MCP server describes how to work with each one. Follow its tool descriptions. Never invent ELU APIs, credentials, or results.

## Pass and fail

- A candidate merges only when ELU's verdict on its exact commit is a pass and CI on GitHub's runners is green. Failed, incomplete, pending, or a verdict on another commit is not a pass.
- Enforce the gate off your computer: protect the default branch and make the gate a required check that no pull request can modify. Changes to the gate need my approval.
- A release is promoted only on ELU's release verdict. A roll-back verdict rolls it back immediately.

## Trust boundary

- Grok Bots share one computer. Nothing on it is evidence for release: not a Bot's report, a hidden folder, or a local test run.
- Bots use a read-only ELU key. They read evidence and verdicts, and never change ELU's tests, scenarios, or settings.
- A hidden scenario whose details reach a Bot is no longer hidden. Ask me to replace it.
- Secrets live in GitHub or the deploy platform, never in the repository or a prompt. Test-account passwords go only in ELU's Settings.

## Stop rules

- Stopping beats a forced pass. BLOCKED and NEEDS_HUMAN are valid results.
- Per task: three attempts, 20 minutes of work, and a spending ceiling I set. Time spent waiting for ELU or a deploy does not count. The first limit reached ends the task.
- Conflicting requirements or missing evidence end the task as BLOCKED, never as another attempt.
- No score overrides a failed critical invariant.
- Weakening a test, a twin, or the gate to get a candidate through is a failed task, reported as one.

## What waits on me

- Goals and priorities.
- Secrets and account access.
- Irreversible actions: deleting data, destructive migrations, spending money, and anything sent to customers outside the product.
- Any verdict ELU does not return. Report NEEDS_HUMAN and wait.

Nothing else waits on me.

## Set up

1. Ask me to claim the $1,000 ELU credit at https://elu.dev/grok.
2. From the app's directory, run `npx -y @elu-labs/cli web`. It adds the ELU tag to the app. Sign in when it asks. Ask me to ship the tag to production through my current release process; it is a tag-only change. Confirm ELU receives a real session.
3. Ask me to connect the GitHub repository to ELU and turn on pull-request testing in ELU Settings. Each pull request needs a preview deploy that reports a GitHub deployment status. Build it if it is missing.
4. Ask me for a read-only ELU API key from ELU → Settings → API & MCP. Store it as a secret outside the repository. Add ELU as a remote MCP server in your own config:
   - URL: `https://elu.dev/api/mcp`
   - Header: `Authorization: Bearer <key>`

   Confirm the connection by listing ELU's tools.
5. Make production deploys progressive, with one-step rollback. Build that if it is missing.

## Deliver

- The running factory: role definitions, orchestration, twins, the gate, progressive deploys with rollback, ELU integration, and a README.
- One spec carried from an ELU finding to production, with ELU's verdicts attached.
- Two sabotage runs. A candidate that passes every visible check but breaks the journey for a user, rejected at the gate. A release that regresses on its first share of users, rolled back.
- Proof that the stop rules end the loop.
- A report: what ran, what passed, what is blocked, and every verdict that came from me instead of ELU.
