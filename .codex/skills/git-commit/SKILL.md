---
name: git-commit
description: Create git commits using Conventional Commits with diff-aware message generation, safe staging, and non-destructive commit practices. Use when the user asks to commit changes, create a git commit, or mentions "/commit".
metadata:
  short-description: Create safe conventional commits
---

# Git Commit

Use this skill to turn local changes into a clean, intentional git commit without taking risky shortcuts.

## Goals

- Inspect the real diff before writing a message.
- Stage only the files that belong in the same logical change.
- Generate a Conventional Commit message that matches the change.
- Commit safely without bypassing hooks or rewriting history unless the user explicitly asks.

## Conventional Commit format

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Commit types

- `feat`: new feature
- `fix`: bug fix
- `docs`: documentation-only change
- `style`: formatting or style-only change
- `refactor`: code restructuring without feature or bug behavior change
- `perf`: performance improvement
- `test`: added or updated tests
- `build`: build tooling or dependencies
- `ci`: CI or automation configuration
- `chore`: maintenance or miscellaneous work
- `revert`: revert of a prior commit

## Breaking changes

Use either of these patterns when the change is intentionally breaking:

```text
feat!: remove deprecated endpoint
```

```text
feat: allow config to extend other configs

BREAKING CHANGE: `extends` key behavior changed
```

## Workflow

### 1. Inspect the current state

Start by checking both status and diff:

```bash
git status --short
git diff --staged
git diff
```

- If files are already staged, treat the staged diff as the likely intended commit.
- If nothing is staged, inspect the working tree diff and decide what belongs together.
- Watch for unrelated edits and avoid bundling them into the same commit.

### 2. Stage intentionally

Use the narrowest staging approach that matches the requested change:

```bash
git add path/to/file1 path/to/file2
git add -p
```

- Prefer staging specific files or hunks over staging everything by default.
- Keep one logical change per commit whenever practical.
- Never commit secrets such as `.env` files, credentials, tokens, or private keys.

### 3. Derive the commit message from the diff

Choose:

- `type`: what kind of change this is
- `scope`: the subsystem, package, feature area, or folder when useful
- `description`: short imperative summary under 72 characters

Write descriptions in present tense and imperative mood:

- Good: `fix login redirect loop`
- Avoid: `fixed login redirect loop`
- Avoid: `fixes login redirect loop`

### 4. Commit

For a short message:

```bash
git commit -m "<type>[scope]: <description>"
```

For a longer body or footer:

```bash
git commit -m "$(cat <<'EOF'
<type>[scope]: <description>

<optional body>

<optional footer>
EOF
)"
```

## Best practices

- Prefer one logical change per commit.
- Add a body when the reason behind the change is not obvious from the diff.
- Add issue references when useful, for example `Refs #123` or `Closes #456`.
- Keep the subject line concise and scannable.

## Safety rules

- Never update git config as part of this skill.
- Never use destructive commands such as force push, hard reset, or checkout-based reverts unless the user explicitly requests them.
- Never use `--no-verify` unless the user explicitly asks to skip hooks.
- If hooks fail, inspect the failure and fix forward instead of trying to bypass safeguards.
- Do not amend an existing commit unless the user explicitly asks for an amend.
