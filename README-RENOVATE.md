# Renovate Bot Setup for Helm values.yaml Image Tags

## What does this do?
This setup enables Renovate Bot to automatically update container image tags in your `values.yaml` files under `cluster-addons/**/values.yaml`.

## How to set up

1. **GitHub Actions Workflow**
   - File: `.github/workflows/renovate.yml`
   - Runs Renovate weekly on Mondays at 07:00 (Asia/Singapore time).
   - You can also trigger it manually via GitHub Actions.

2. **Renovate Config**
   - File: `renovate.json`
   - Uses the base config and applies to all `values.yaml` files under `cluster-addons/`.
   - Custom rule extracts and updates the `tag:` field for Docker images.
   - PRs will be labeled `dependencies` and `renovate` and assigned to `bellyliu`.

## How it works
- Renovate scans all `cluster-addons/**/values.yaml` files.
- It looks for lines like `tag: v1.19.3` and checks for newer tags on Docker Hub or Quay.
- When a new tag is available, Renovate creates a PR to update the tag.
- All PRs are grouped under "Helm values image tags".

## Example PR
If your `values.yaml` contains:
```yaml
image:
  repository: quay.io/jetstack/cert-manager-controller
  tag: v1.19.3
```
Renovate will propose a PR to update `tag: v1.19.3` to the latest available version.

## Notes
- You can customize the schedule, labels, and assignees in `renovate.json`.
- For more advanced matching, see the [Renovate documentation](https://docs.renovatebot.com/).
