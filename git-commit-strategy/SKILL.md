---
name: git-commit-strategy
description: Prepare, organize, review, or create Git commits by inspecting changes, grouping them into focused commits, validating them, and writing concise messages. Use whenever the user requests work on Git commits.
---

# Git Commit Strategy

Keep commits small, focused, and independently understandable across repositories and languages. Match the requested scope: preparing or reviewing commits does not by itself authorize creating them or pushing them.

## Inspect and organize

- Read applicable repository instructions. Inspect `git status`, the full unstaged diff (`git diff`), and the full staged diff (`git diff --cached`). Read relevant untracked files separately; Git diffs do not include them. Inspect binary changes with suitable tools when needed.
- Understand each change's purpose and dependencies before choosing commit boundaries. Group changes by logical outcome, separating unrelated work even when it shares a file. Keep implementation, necessary tests, and supporting documentation together when they form one coherent change; avoid splitting coupled changes into broken intermediate states.
- Preserve existing user changes and staging choices. If existing staged changes conflict with the requested grouping and intent is unclear, clarify before changing their staging. Stage explicit paths or selected hunks rather than sweeping in the entire working tree.
- Exclude secrets, credentials, build artifacts, DerivedData, temporary files, and unrelated files. Review actual content as well as filenames, including already tracked files; ignore rules alone do not protect them. Do not print secret values. Leave excluded files intact and uncommitted.

## Validate and commit

- Choose relevant tests, builds, or checks from repository guidance and the affected behavior. Run them before committing when appropriate. If checks fail or cannot run, report the limitation; do not claim success or repeatedly retry unchanged failures. Resolve relevant failures within the authorized scope, or seek direction before committing with unresolved failures.
- Before **each** commit, inspect `git diff --cached` in full and its file list (`git diff --cached --name-status`). Confirm the staged content matches that commit's purpose and contains no excluded or unrelated material. Use `git diff --cached --check` when applicable. Reinspect after any staging changes.
- Write a concise imperative subject describing the outcome. Use Conventional Commit-style prefixes when appropriate to the repository: `feat:`, `fix:`, `refactor:`, `test:`, `docs:`, or `chore:`. Add a body only when context or rationale would help a future reader.
- Create only the authorized commits. Never amend, rebase, force push, or rewrite existing shared history unless the user explicitly requests it. Commit authorization does not imply push authorization.
- After each commit, verify its contents and inspect status before proceeding. If a hook changes files or a commit fails, inspect the new state before retrying; avoid blindly repeating commands or automatically amending.

## Report

At the end, report:

- Commits created, with short hashes and messages, or state that none were created.
- Files included in each commit.
- Checks run and their results, including checks skipped or unavailable when relevant.
- Anything intentionally left uncommitted and why, based on a final `git status` inspection.

For preparation or review only, report the proposed commit groups and messages instead of implying they were created.
