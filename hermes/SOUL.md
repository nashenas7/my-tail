# h-dashboard Git Workflow

## Repository Model

The canonical upstream repository is:

`https://github.com/asgarimehdi/h-dashboard`

Each Hermes server has its own fork/repository and its own dedicated working branch.

The server startup workflow already:

* clones the correct repository
* configures the Git remotes
* checks out the correct server-specific branch

Never hardcode the fork URL or server-specific branch name.

Always inspect and use the Git configuration that already exists on the server.

---

## Current Branch Is the Only Development Branch

The branch that is checked out when Hermes starts is the server's dedicated working branch.

For example, if the current branch is:

```bash
kimya
```

then all normal development must remain on:

```text
kimya
```

and all normal commits must be pushed only to:

```text
origin/kimya
```

Another server must independently push only to its own current branch.

### Absolute rule

**NORMAL WORK MUST ONLY BE PUSHED TO THE CURRENT CHECKED-OUT BRANCH.**

Never push normal development commits to:

```text
origin/beta
```

Never push normal development commits to another server's branch.

Never push normal development commits to any branch other than the current checked-out branch.

---

## Beta Is Read/Synchronization Only

`beta` is a synchronization/source branch.

Hermes may:

* fetch `beta`
* read `beta`
* compare the current branch with `beta`
* merge/rebase/cherry-pick changes from `beta` into the current branch when appropriate
* use `beta` as the source for synchronization

Hermes must **NOT push development changes to beta**.

In particular, do NOT execute:

```bash
git push origin beta
```

as part of normal development.

Do not update `origin/beta` with the current branch's commits.

The current server branch is the destination of development work.

---

## Session Initialization

At the beginning of a session:

```bash
cd h-dashboard

git remote -v

git branch --show-current

git status
```

Determine the current checked-out branch.

Then synchronize the current branch with the appropriate `beta` source according to the existing Git configuration.

The synchronization direction is:

```text
beta
  ↓
current server branch
```

NOT:

```text
current server branch
  ↓
beta
```

After synchronization, remain on the current server branch.

Do not switch to `beta` for normal development.

---

## Normal Work

During normal development:

1. Stay on the current server-specific branch.
2. Make changes only on that branch.
3. Test/verify the changes.
4. Commit the changes.
5. Push the commit only to the current server-specific remote branch.

Before pushing, verify:

```bash
git branch --show-current
git status
git log -1 --oneline
```

The push destination must correspond to the current checked-out branch.

For example:

```text
current branch: kimya
push destination: origin/kimya
```

Never:

```text
origin/beta
```

unless the user explicitly orders a beta push.

---

## Push Safety Rule

Before every `git push`, verify the destination.

If the destination is:

```text
beta
```

STOP.

Do not push.

Only push to `beta` if the user explicitly says to push to beta.

If the destination is another server's branch, STOP.

Do not push.

If the destination is the current checked-out branch, proceed.

---

## Pull Requests

When the user says:

`pr`

create a Pull Request from the current server-specific branch to:

```text
beta
```

of the canonical repository:

`https://github.com/asgarimehdi/h-dashboard`

Use GitHub MCP when available.

Creating a PR does NOT mean pushing to `beta`.

The normal workflow is:

```text
current server branch
        │
        │ push
        ▼
server fork / current branch
        │
        │ PR
        ▼
canonical repository / beta
```

Do not merge the PR unless the user explicitly asks for the merge.

---

## No Automatic Beta Push

Under no circumstances should Hermes automatically do this after completing work:

```bash
git push origin beta
```

The completion of a task means:

```text
modify
  ↓
test
  ↓
commit
  ↓
push current branch only
```

It does NOT mean:

```text
modify
  ↓
commit
  ↓
push current branch
  ↓
push beta
```

---

## Git Final Check

Before reporting the task as completed, verify:

```bash
git branch --show-current
git status
git log -1 --oneline
git remote -v
```

Report the actual branch that was pushed.

Never claim that a PR contains a commit unless the PR/source branch relationship has actually been verified.

---

# Project Instructions

Before development work:

1. Read `AGENTS.md`.
2. Follow all instructions in `AGENTS.md`.
3. Keep its instructions in context throughout the task.

---

# Required Tools

Use these tools whenever relevant and available:

* Laravel Boost
* Context7
* GitHub MCP
* CodeGraph MCP
* `read-the-damn-docs`

If a required tool is unavailable, diagnose and configure it when possible.

Never claim to have used a tool that was not actually available or used.

---

# Documentation

Use `read-the-damn-docs` for applicable work.

If it is missing:

```bash
npx @agent-native/skills@latest add --skill read-the-damn-docs
```

Read the relevant documentation before implementing or changing behavior.
