# Renovate Bot — Helm `values.yaml` Image Tags

This repository uses Renovate to update container image tags declared in Helm `values.yaml` files under `cluster-addons/*/values.yaml`.

## What was added
- Workflow: `.github/workflows/renovate.yml` — runs Renovate weekly and can be triggered manually.
- Config: `renovate.json` — uses the default config (`extends: ["config:base"]`).

## How it works
- Renovate scans each `cluster-addons/<chart>/values.yaml` using its built-in managers.
- When a new image tag is available, Renovate creates a PR to update the tag.
- PRs are labeled and assigned as configured in `renovate.json`.

## How to run / test
- The workflow runs automatically in GitHub Actions. To trigger manually, use the Actions tab.
- Local test (run from repo root):

```bash
docker run --rm -v "$(pwd)":/repos ghcr.io/renovatebot/renovate:latest \
  --config-file=/repos/renovate.json --log-level info --autodiscover=false
```

## Permissions and notes
- When running in GitHub Actions, provide a Personal Access Token (PAT) named `RENOVATE_TOKEN` in repository Secrets. This PAT should have at least `repo` and `workflow` scopes to allow Renovate to create branches and run workflows.
- The default config is recommended. Only use `regexManagers` if you need to update dependencies in non-standard files or layouts.
- Renovate will handle registries like Quay or ECR; if you encounter auth errors when querying a registry, check registry access rules and rate limits.

## Customization
- Schedule, labels, assignees, and grouping live in `renovate.json` — edit them to fit your workflow.

## Why the PAT matters
- The GitHub Actions `GITHUB_TOKEN` is limited in some contexts and cannot be reused outside the runner; using a PAT avoids certain permission and rate-limit issues when Renovate creates branches or queries registries.
- For private container registries (or rate-limited public registries), add `hostRules` to `renovate.json` with credentials or set the appropriate registry secrets — Renovate will use those to query tags.

## Reference
- Renovate documentation: https://docs.renovatebot.com/
