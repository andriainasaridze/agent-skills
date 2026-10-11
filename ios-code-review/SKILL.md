---
name: ios-code-review
description: Review iOS, Swift, SwiftUI, UIKit, or other Apple-platform implementations, features, pull requests, diffs, and completed coding tasks for meaningful correctness, architecture, concurrency, maintainability, performance, and testing issues.
---

# iOS Code Review

Find actionable problems supported by the actual code. Prioritize impact over cosmetic preferences. Adapt to the repository and preserve its architecture unless a concrete problem justifies a change. Review and report without modifying code unless explicitly asked to fix findings; preserve unrelated user work.

## Establish context

- Read applicable repository agent instructions, such as `AGENTS.md` or `CLAUDE.md`, and relevant project documentation.
- Establish the requested review scope. For recent work, inspect the relevant diff, including staged, unstaged, or branch changes as appropriate. Identify the intended comparison base rather than assuming all working-tree changes belong to the task.
- Understand the existing architecture and inspect enough surrounding code, callers, dependencies, and tests to evaluate behavior. Account for supported platforms, deployment targets, Swift/toolchain versions, and repository conventions.
- Use available code inspection, build, and test tools when they can resolve a meaningful uncertainty. Record what was actually checked and any limitations; code inspection alone does not establish runtime correctness. For uncertain language or framework behavior, consult version-relevant official documentation when available.

## Review applicable areas

Apply only the areas relevant to the change. Trace concrete failure paths and distinguish confirmed issues from unresolved questions.

- **Correctness:** Incorrect behavior, edge cases, state transitions, error paths, assumptions, crashes, force unwrap risks, races, stale state, and lifecycle issues.
- **Swift:** Value/reference semantics, mutability, optional safety, error handling, generics/existentials, protocol usage, access control, and unnecessary complexity. Report a concrete consequence, not a personal stylistic preference.
- **Concurrency:** Actor isolation, `@MainActor`, `Sendable`, structured/unstructured tasks, cancellation, task lifetime, races, duplicate work, shared mutable state, continuation correctness, and legacy GCD or locking. Consider who owns work and when it should stop. Recommend abstractions only to solve a demonstrated problem.
- **Memory management:** Retain cycles, closure captures, delegates, long-lived tasks, Combine subscriptions, observers, timers, and resource cleanup. Trace ownership and lifetime before claiming a leak.
- **SwiftUI:** State ownership, Observation/observable models, bindings, view and `ForEach` identity, expensive body work, navigation state, lifecycle use, render triggers, task modifiers, accessibility, and layout behavior.
- **UIKit:** Lifecycle, ownership, delegation, layout, threading, cell reuse, memory management, and state handling.
- **Architecture:** Fit with existing design, misplaced business logic, excessive coupling, dependency boundaries, duplication, unnecessary abstractions, god objects, global state, and testability. Prefer the simplest solution that fits the repository; an alternative architecture being possible is not a finding.
- **Error handling:** Silently swallowed errors, unclear user-facing failures, inappropriate handling of recoverable errors, and logging that leaks sensitive data.
- **Testing:** Coverage of important behavior, regression scenarios, edge cases, implementation-specific assertions, flakiness, and mocks that hide real behavior. Do not demand tests for trivial code with no meaningful behavior.
- **Performance:** Plausible real costs such as expensive SwiftUI body work, repeated network requests or decoding, excessive main-thread work, unstable collection identity, memory growth, or image processing on the main actor. Explain the triggering workload and impact; avoid speculative micro-optimizations.

## Assess findings

Before reporting an issue, confirm its location, triggering conditions, consequence, and a practical recommendation against surrounding code. For a diff review, distinguish newly introduced problems from pre-existing ones and keep findings within the requested scope. Do not inflate severity because evidence is incomplete; identify material uncertainty explicitly.

Combine duplicate symptoms of one root cause. Omit generic best-practice advice and cosmetic comments unless they materially affect consistency or readability. Do not invent findings to fill categories.

## Report and completion

Lead with findings grouped by these headings, highest impact first. Omit empty severity groups:

- **CRITICAL** — Likely crashes, data corruption, security issues, major concurrency bugs, or clearly broken behavior.
- **HIGH** — Important correctness or architecture issues that should be fixed.
- **MEDIUM** — Real maintainability, robustness, performance, or testability concerns.
- **LOW** — Useful improvements genuinely worth considering.

For every finding include the file and relevant line or symbol, what is wrong, why it matters (including the triggering scenario), and a concrete recommendation. Link locations when supported by the review environment. If no meaningful issues were found, state **No significant findings.**

After findings, include:

- **GOOD DECISIONS** — Briefly mention important choices the implementation made well when useful; avoid filler praise.
- **TEST GAPS** — Meaningful missing tests and the behavior they should verify, or state that none were identified. Reference findings rather than repeating them.
- **VERDICT** — Whether the change looks ready, the issues preventing readiness, and the checks performed or material verification limitations. Do not imply builds or tests passed unless they ran and passed.

The review is complete when the requested scope and relevant surrounding behavior have been inspected, findings have been checked against evidence, and the report gives an actionable verdict with meaningful test gaps and verification limits.
