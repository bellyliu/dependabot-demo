# Dependabot / Renovate Demo Repository

This repository demonstrates using GitHub Dependabot and Renovate to manage dependencies and container image tags.

## Included Tooling
- **Dependabot**: `.github/dependabot.yml` — updates Dockerfiles and Python `requirements.txt` in `python-app/`.
- **Renovate**: `renovate.json` + `.github/workflows/renovate.yml` — updates container image `tag:` fields inside Helm `values.yaml` files under `cluster-addons/*/`.

## Setup and Usage
- Renovate uses the default config (`extends: ["config:base"]`).
- For Renovate to work reliably, add a Personal Access Token (PAT) named `RENOVATE_TOKEN` to your repository secrets. The PAT should have `repo` and `workflow` scopes.
- Dependabot works with the default GitHub token.

## Quick Checks
- To run Renovate locally (test):

```bash
docker run --rm -v "$(pwd)":/repos ghcr.io/renovatebot/renovate:latest \
	--config-file=/repos/renovate.json --log-level info --autodiscover=false
```

- To validate Dependabot config, ensure `.github/dependabot.yml` directories point to the correct locations (e.g. `/python-app`).

## Notes
- Dependabot's `helm` ecosystem cannot update image tags inside Helm `values.yaml` (limitation). Renovate is configured to handle those.
- Prefer Renovate's default config and only use custom managers if you need to update non-standard files.
- If you want me to trigger the Renovate workflow or run tests and share logs, just ask!