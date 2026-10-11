---
name: app-store-preflight
description: Audit Apple-platform apps for App Store submission readiness and App Review rejection risks. Use when the user asks whether an app is ready for App Store Review, requests an App Store compliance audit, or wants to reduce rejection risk.
---

# App Store Preflight

Audit the app's applicable requirements and report evidence-backed findings. By default, inspect and report; if the user explicitly requests fixes, make targeted changes and verify them afterward. Do not modify unrelated code, submit a build, or change App Store Connect configuration as part of an audit.

## Current requirements and evidence

Apple requirements change. During an actual audit, if internet access is available, verify current requirements against official Apple sources. Prioritize the [App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/), [App Store Connect documentation](https://developer.apple.com/help/app-store-connect/), and [submission requirements](https://developer.apple.com/news/upcoming-requirements/), plus relevant Apple privacy, StoreKit, subscription, Sign in with Apple, account deletion, and platform documentation. Follow relevant links and confirm effective dates. Prefer official Apple sources over third-party explanations.

Apply rules to the actual platform, distribution context, storefronts, OS versions, and relevant entitlements; do not assume purchase or external-link rules are universal. Cite the Apple source and guideline section supporting a finding, and record the audit date. If a rule is uncertain, verify it; if access is unavailable, disclose the limitation and leave currency-dependent conclusions unverified rather than relying only on memory.

Distinguish **code/project inspection**, **runtime testing** (with environment/build), and **App Store Connect inspection** throughout. Record evidence for each passed check and finding. Code indicating an intended behavior does not prove that behavior works at runtime. Do not invent missing information, mark untested behavior as passed, or claim App Store Connect settings are correct without inspecting them. Request missing information only when it materially affects the audit; otherwise record a manual check.

## Phase 1 — Understand the app

Read repository instructions and inspect the project, configuration, dependencies, and relevant product flows. Identify supported Apple platforms and the intended submission build. Determine which of these apply:

- Authentication, account creation/deletion, and Sign in with Apple.
- StoreKit, IAP, subscriptions, external purchases, and external links.
- Permissions, tracking, analytics, advertising, crash reporting, and third-party SDKs.
- User-generated content, moderation, networking, web content, sensitive functionality, and hardware or service dependencies.

Create a brief applicability map and note gaps in available evidence. Audit applicable requirements only; mark irrelevant topics as not applicable with a reason rather than flagging them as missing.

## Phase 2 — App Review Guidelines audit

Compare implementation and observable product behavior with current relevant guidelines. Look for incomplete or broken functionality, misleading UI or metadata, payment/subscription and external-purchase violations, Sign in with Apple and account-deletion gaps, permission/privacy/tracking issues, user-generated content and moderation gaps, unsupported or hidden functionality, placeholders, inaccessible features, reviewer login barriers, and hardware dependencies.

Support each risk with an affected flow or file and the applicable Apple requirement. Distinguish confirmed issues from potential risks needing runtime or configuration evidence. When tooling permits, run relevant builds and exercise critical flows; report unavailable testing explicitly.

## Phase 3 — Privacy audit

Where applicable, inspect Privacy Manifests, required reason API use and declared reasons, Info.plist usage descriptions, ATT implementation, actual tracking, collected user data, and analytics/advertising/crash-reporting SDK behavior. Inspect dependencies and bundled SDK privacy information; do not infer all data practices from first-party code alone.

Compare declarations with observed implementation and available runtime evidence. Identify corresponding App Store Connect privacy disclosures, including relevant third-party collection and tracking. Inspect those declarations if available; otherwise list reconciliation as a manual check. Verify current Apple requirements for applicable SDK manifests/signatures and privacy APIs.

## Phase 4 — StoreKit / subscriptions

Skip this phase with a reason if there are no purchases or subscriptions. Otherwise check, where relevant:

- Product loading and mapping to intended entitlements; purchase success, cancellation, failure, and pending states.
- Transaction verification, entitlement persistence, restoration, revocation, expiration, and refund handling.
- Subscription management; pricing and billing periods; trial/introductory-offer presentation and eligibility.
- Required Terms of Use/privacy links, paywall behavior, and misleading purchase UX.

Separate code findings from StoreKit sandbox/device testing and App Store Connect configuration checks. Local StoreKit configuration tests do not prove production product availability. Record tested scenarios and environments; leave untested lifecycle scenarios as manual checks.

## Phase 5 — App Store Connect readiness

Inspect available App Store Connect evidence, or request manual verification of applicable fields:

- App name, subtitle, description, keywords, category, and age rating.
- Screenshots, app previews, support URL, privacy policy URL, and copyright.
- Review notes, demo credentials, contact information, and selected build.
- IAP/subscription configuration and required agreements.

Check metadata against actual app behavior and the intended build. Verify current Xcode/SDK submission requirements against the build's toolchain evidence. Mark fields passed only when actually inspected; identify the source and any limits of screenshots or exports. Do not expose credential values in the report.

## Phase 6 — Reviewer experience

Walk through the app as an App Review reviewer when runtime access is available; otherwise assess the flow from available evidence and label that limitation. Determine whether all important features are accessible, credentials or special instructions are needed, purchases/subscriptions need explanation, external hardware/services are required, or any feature could appear broken or unfinished.

Identify necessary review notes, usable demo access, and setup instructions without assuming they have been supplied in App Store Connect. Check that reviewer access does not depend on unavailable services or unexplained prerequisites.

## Phase 7 — Final report and completion

Finish when every applicable phase has been assessed or explicitly marked unverified, relevant checks are recorded, and remaining gaps have actionable next steps. Group findings under these exact headings, using “None” when empty:

- **BLOCKERS** — Issues likely to prevent submission or cause rejection.
- **HIGH PRIORITY** — Issues that should be addressed before review.
- **RECOMMENDED** — Improvements unlikely to block review.
- **VERIFIED** — Requirements actually checked and passed, with evidence and verification method.
- **MANUAL CHECKS** — Checks requiring developer action, inaccessible App Store Connect data, or additional runtime testing.

For each issue, state the evidence, applicable Apple source, impact, and next action. Keep uncertainty explicit and avoid duplicate findings across groups. Include checks performed, their results, and the brief readiness reason before the verdict.

Use YES only when applicable submission/rejection requirements have been verified and no blockers, unresolved high-priority issues, or material verification gaps remain. Otherwise use NO and explain whether this reflects a confirmed issue or insufficient evidence. Readiness is an evidence-based assessment, not a guarantee of Apple's decision.

End with exactly one of these lines, with nothing after it:

APP STORE READY: YES

or

APP STORE READY: NO
