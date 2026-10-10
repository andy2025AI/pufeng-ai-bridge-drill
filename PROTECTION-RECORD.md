# PROTECTION-RECORD — drill-main-protection

- Repo: `andy2025AI/pufeng-ai-bridge-drill` (public, sanitized sandbox)
- Applied: 2026-10-10
- Ruleset: `drill-main-protection` (id 24828923, enforcement=active, bypass_actors=[])
- Rules: `deletion` · `non_fast_forward` · `required_linear_history` · `pull_request` (allowed_merge_methods: squash)
- Repo switches: `allow_auto_merge=false` · `allow_merge_commit=false` · `allow_rebase_merge=false` · `allow_squash_merge=true` · `delete_branch_on_merge=false`

## Verified behavior (2026-10-10)

| Case | Expected | Result |
|:--|:--|:--|
| Direct push to main | reject | ✅ rejected ("push declined due to repository rule violations") |
| Force push to main | reject | ✅ rejected |
| Delete main (API) | reject | ✅ HTTP 422 "Cannot delete the default branch" |
| Push new branch | allow | ✅ allowed |
| PR merge, required_approving_review_count=1 | block (single-account deadlock) | ✅ blocked; documented as known constraint |
| PR merge, required_approving_review_count=0 | allow | ✅ merged (PR #1) |

## Phases

- **Phase A** (`count=1`, `require_last_push_approval=true`): mechanically reproduced the single-account
  approver deadlock. To enable: add a second approver identity first.
- **Phase B** (`count=0`, **current**): PR-only flow that stays usable with a single account.
  Direct pushes / force pushes / deletions remain blocked.

## Notes

- Main branch of this sandbox intentionally contains drill residue (probe files, merged PR #1) — all sanitized.
- History: one force-push was used during initial setup to scrub the committer email, BEFORE protection was enabled.
- This file itself was updated through the protected flow (branch → PR → merge) as an additional end-to-end check.
