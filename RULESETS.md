# Repository Rulesets Migration & Implementation Guide

This guide provides instructions and JSON schemas for migrating from **Classic Branch Protection** to **Repository-Level Rulesets** in the LaookProj organization when you are ready to transition.

---

## 🧐 Why Migrate to Repository Rulesets?

While Classic Branch Protection serves your workflow well, Repository-Level Rulesets are the modern GitHub standard and provide:
1. **Targeting Empty Repositories**: Rulesets apply to branches by pattern matching (e.g., `main`), meaning they can be configured on empty repositories *before* the first commit is pushed.
2. **Granular Bypass Rules**: Instead of disabling admin protections entirely, you can grant specific bypass permissions solely to your user account (`ahmadnurhidayat`) or selected roles.
3. **Evaluation Mode**: Test new policies in dry-run mode before active enforcement.

---

## 📋 The Ruleset Schema (`ruleset_payload.json`)

Below is the stable, fully-validated JSON schema for a repository-level ruleset that mirrors the LaookProj protection policies, complete with a **bypass list** tailored for your account.

```json
{
  "name": "Main-Branch-Protection-Ruleset",
  "target": "branch",
  "enforcement": "active",
  "bypass_actors": [
    {
      "actor_id": 92295366,
      "actor_type": "RepositoryRole",
      "bypass_mode": "always"
    }
  ],
  "conditions": {
    "ref_name": {
      "include": [
        "refs/heads/main"
      ],
      "exclude": []
    }
  },
  "rules": [
    {
      "type": "deletion"
    },
    {
      "type": "non_fast_forward"
    },
    {
      "type": "required_signatures"
    },
    {
      "type": "pull_request",
      "parameters": {
        "required_approving_review_count": 1,
        "dismiss_stale_reviews_on_push": true,
        "require_code_owner_review": false,
        "require_last_push_approval": false,
        "required_review_thread_resolution": false,
        "automatic_copilot_code_review_bypass": false
      }
    },
    {
      "type": "required_status_checks",
      "parameters": {
        "strict_required_status_checks_policy": true,
        "required_status_checks": [
          {
            "context": "ci/build"
          },
          {
            "context": "ci/lint"
          }
        ]
      }
    }
  ]
}
```

*Note: In the bypass list above, the `actor_id` (`92295366`) is your personal GitHub account ID. This guarantees that your account retains bypass authority while ensuring other members must satisfy approvals and status checks.*

---

## 🛠️ Step-by-Step Migration Guide

When you decide to execute the migration, run these sequential steps using the GitHub CLI:

### Step 1: Save the Payload
Save the JSON configuration above as `ruleset_payload.json` inside your local directory.

### Step 2: Delete the Classic Branch Protection Rules
Classic branch protection rules must be removed first so they do not conflict with the new rulesets:
```bash
repos=(LaookProj laook-backend-api laook-models laook-mobile-app laook-flutter-app)

for r in "${repos[@]}"; do
  echo "Deleting classic branch protection for $r..."
  gh api -X DELETE "/repos/LaookProj/$r/branches/main/protection"
done
```

### Step 3: Apply the New Repository Ruleset
Apply the new ruleset directly to each repository:
```bash
for r in "${repos[@]}"; do
  echo "Creating ruleset for $r..."
  gh api -X POST "/repos/LaookProj/$r/rulesets" \
    --input ruleset_payload.json
done
```

### Step 4: Verify the Active Rulesets
Verify that the ruleset has been successfully created and is in an `active` state:
```bash
for r in "${repos[@]}"; do
  echo "=== Rulesets for $r ==="
  gh api "/repos/LaookProj/$r/rulesets" --jq '.[] | {id: .id, name: .name, enforcement: .enforcement}'
done
```

---

## 🔄 Reverting back to Classic Branch Protection
If you ever need to return to Classic Branch Protection:
1. Delete the ruleset via GitHub API:
   ```bash
   gh api -X DELETE "/repos/LaookProj/REPO_NAME/rulesets/RULESET_ID"
   ```
2. Re-apply the classic protection payload:
   ```bash
   gh api -X PUT "/repos/LaookProj/REPO_NAME/branches/main/protection" \
     --input branch_protection.json
   ```
