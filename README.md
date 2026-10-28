# FestPass Docker & Container Security Assessment

- **Name:** Rishabh
- **REGNO:** 257
- **PORT:** 257
- **Docker Hub username:** rishabh010904
- **Docker Hub repository:** https://hub.docker.com/r/rishabh010904/fest-backend
- **AI tools:** GitHub Copilot (assessment implementation, Dockerfile, Compose, Trivy remediation, and documentation).

## Deployment

```powershell
Copy-Item .env.example .env
# Set POSTGRES_PASSWORD in .env, then run:
docker compose up -d
docker compose ps
```

Open `http://localhost:257`. The backend release is `rishabh010904/fest-backend:1.0.0`.

## Image verification

- **1.0.0 digest:** `PENDING_DOCKER_HUB_PUSH`
- **Signature public key:** `cosign.pub`
- **Unsigned release:** `1.0.1` must be rejected by `cosign verify`.

```powershell
cosign verify --key cosign.pub rishabh010904/fest-backend@sha256:<DIGEST>
```

## Trivy comparison

| Image | CRITICAL | HIGH | Size |
|---|---:|---:|---:|
| fest-backend:0.1 | PENDING_SCAN | PENDING_SCAN | PENDING_BUILD |
| fest-backend:0.2 | PENDING_SCAN | PENDING_SCAN | PENDING_BUILD |

The fix moves the image to a pinned Python slim base, runs the application as a non-root user, and upgrades the Python dependencies.

## Scanner gate

```powershell
trivy image --severity CRITICAL --ignore-unfixed --exit-code 1 fest-backend:0.2
if ($LASTEXITCODE -eq 0) { "No fixable CRITICAL vulnerabilities" }
```

## Evidence and incomplete steps

Screenshots are stored in `screenshots/`. Final Docker Hub, registry, Trivy, and cosign evidence must be added after credentials and tools are available in the terminal.

## Security rules

- Never commit `.env`, `*.key`, Docker Hub passwords, or access tokens.
- The `1.0.0` image is signed by digest.
- The `1.0.1` image is intentionally unsigned and must not be used as a release.
