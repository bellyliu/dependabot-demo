# Dependabot / Renovate demo repository

This repository demonstrates a small example of using GitHub Dependabot and Renovate to manage dependencies and container image tags.

### Included tooling
- Dependabot configuration: `.github/dependabot.yml` — updates Dockerfiles and Python `requirements.txt` in `python-app/`.
- Renovate configuration: `renovate.json` + `.github/workflows/renovate.yml` — updates container image `tag:` fields inside Helm `values.yaml` files under `cluster-addons/*/`.

### Quick checks
- To run Renovate locally (test):

```bash
docker run --rm -v "$(pwd)":/repos ghcr.io/renovatebot/renovate:latest \
	--config-file=/repos/renovate.json --log-level info --autodiscover=false
```

- To validate Dependabot config, ensure `.github/dependabot.yml` directories point to the correct locations (e.g. `/python-app`).

**Notes**
- Dependabot's `helm` ecosystem cannot update image tags inside Helm `values.yaml` (limitation). Renovate is configured to handle those.
- If you want me to trigger the Renovate workflow or run tests and share logs, say so and I'll run them.