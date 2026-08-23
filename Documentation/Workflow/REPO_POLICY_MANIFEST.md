# REPO_POLICY_MANIFEST.md

Repository: `Atomovvy/UiPath_skills`
Policy mode: Atomovvy fork overlay
Central policy repository: `Atomovvy/Atomovvy`
Repository role: `public third-party-derived skill development fork`

## Scope

This file adds Atomovvy repository-write coordination to this fork. It does not replace the repository's existing `AGENTS.md`/`CLAUDE.md`, contribution rules, skill architecture, flavor rules, hook-twin requirements, test framework, packaging semantics, publishing rules, or upstream project governance.

When central Atomovvy policy and repository-local rules both apply, use the strictest compatible rule. If required central policy cannot be resolved for an Atomovvy-managed repository-changing task, fail closed as `CENTRAL_POLICY_UNAVAILABLE`.

## Repository writer coordination

Ordinary hosted GitHub mutations in this fork use the current P2-C Simple Lock v2 coordination route:

```text
P2C_SIMPLE_LOCK_V2_STATUS=ACTIVE
COORDINATION_MODE=P2C_SIMPLE_LOCK_V2
LOCK_REF=coordination/write-lock-v2
LOCK_FILE=LOCK.json
CLAIM_PRIMITIVE=GITHUB_CONTENTS_FILE_SHA_CAS
READ_ONLY_LOCK_REQUIRED=NO
P2C_LEGACY_STRONG_STATUS=FROZEN_REFERENCE
LEGACY_STRONG_DEFAULT_FOR_ORDINARY_WRITES=NO
RUNTIME_ACTIVATION_COMMIT=8eed3d99eb165d6690795b9934e546634bd9b390
```

A conforming hosted writer MUST freshly fetch and validate `LOCK.json`, retain the exact returned file SHA, acquire by exact file-SHA compare-and-swap, and verify exact ownership before crossing the target-mutation boundary. Coordination failure fails closed and MUST NOT silently fall back to Legacy Strong.

The lock coordinates conforming repository writers only. It does not grant skill publication, package publication, release, workflow, deployment, credential, network, external service, or merge authority and does not replace Git freshness, target freshness, review, validation, or the repository's existing contribution rules.

## Public-fork boundary

Do not place private Atomovvy data, secrets, credentials, host paths, unpublished internal evidence, or private central-policy contents into this public repository. References to central policy identify the controlling source but do not copy its private body here.

## Handoff

Completed Atomovvy-managed repository-changing work is recorded in `Documentation/Workflow/AGENT_LAST_RUN.md`. Live coordination state is read only from `coordination/write-lock-v2:LOCK.json`.
