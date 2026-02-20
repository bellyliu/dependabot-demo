# Renovate Bot — Helm `values.yaml` Image Tags

This repo uses Renovate to update container image tags declared in Helm `values.yaml` files under `cluster-addons/*/values.yaml`.

What was added
- Workflow: `.github/workflows/renovate.yml` — runs Renovate weekly and can be triggered manually.
- Config: `renovate.json` — uses a `regexManagers` entry that extracts `repository:` + `tag:` pairs from `cluster-addons/*/values.yaml` and treats them as Docker dependencies.

How it works (short)
- Renovate scans each `cluster-addons/<chart>/values.yaml` and applies the configured regex manager.
- The regex manager extracts the image repository line and the `tag:` line, uses the `docker` datasource, and creates PRs when newer tags exist.
- PRs are grouped under the configured `groupName` ("Helm values image tags") and are labeled and assigned per `renovate.json`.

Regex manager details
- `fileMatch`: `cluster-addons/.*/values.yaml$`
- `matchStrings` (used in `renovate.json`):
   - `repository:\s*(?<depName>[^\n]+)\s*\n\s*tag:\s*(?<currentValue>[^\n]+)`
- This captures the full repository (may include registry, e.g. `quay.io/jetstack/cert-manager-controller`) as `depName` and `tag` as `currentValue`.
- Renovate then queries the `docker` datasource for available tags and proposes updates.

How to run / test
- The workflow runs automatically in GitHub Actions. To trigger manually, use the Actions tab.
- Local test (run from repo root):

```bash
docker run --rm -v "$(pwd)":/repos ghcr.io/renovatebot/renovate:latest \
   --config-file=/repos/renovate.json --log-level info --autodiscover=false
```

Permissions and notes
- When running in GitHub Actions, the `renovatebot/github-action` step uses the provided `GITHUB_TOKEN`. Ensure it has write access to create branches and PRs.
- The regex manager is brittle to unusual formatting; if your `values.yaml` uses different indentation or keys (e.g. `image.tag` instead of `tag:` on the next line), update the `matchStrings` accordingly.
- Renovate will handle registries like Quay or ECR; if you encounter auth errors when querying a registry, check registry access rules and rate limits.

Customization
- Schedule, labels, assignees, and grouping live in `renovate.json` — edit them to fit your workflow.

Reference
- Renovate regex managers: https://docs.renovatebot.com/config-presets/#regexmanager

If you want, I can run a local test and share the logs, or tweak the regex to match alternate `values.yaml` layouts.
