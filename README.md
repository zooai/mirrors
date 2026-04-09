# zooai/mirrors

Image mirror from `ghcr.io/hanzoai/*` to `ghcr.io/zooai/*`.

## Workflow

`mirror.yml` runs on `workflow_dispatch` and every 6h. For each image in the matrix, it pulls from `ghcr.io/hanzoai/{image}:latest` and pushes to `ghcr.io/zooai/{image}:latest` using `crane copy` to preserve all architectures.

Runs on the self-hosted `zoo-build-linux-amd64` runner in the zooai org.

## Mirrored images

| Source (hanzoai) | Destination (zooai) |
|------------------|---------------------|
| `ghcr.io/hanzoai/billing:latest` | `ghcr.io/zooai/billing:latest` |
| `ghcr.io/hanzoai/commerce:latest` | `ghcr.io/zooai/commerce:latest` |
| `ghcr.io/hanzoai/console:latest` | `ghcr.io/zooai/console:latest` |
| `ghcr.io/hanzoai/cloud-site:latest` | `ghcr.io/zooai/cloud:latest` (frontend SPA) |
| `ghcr.io/hanzoai/cloud:latest` | `ghcr.io/zooai/cloud-api:latest` (backend API) |
| `ghcr.io/hanzoai/ingress:latest` | `ghcr.io/zooai/ingress:latest` |
| `ghcr.io/hanzoai/id:latest` | `ghcr.io/zooai/id:latest` |
| `ghcr.io/hanzoai/insights:latest` | `ghcr.io/zooai/insights:latest` |
| `ghcr.io/hanzoai/iam:latest` | `ghcr.io/zooai/iam:latest` |
| `ghcr.io/hanzoai/kms:latest` | `ghcr.io/zooai/kms:latest` |

## Required secrets

Set on this repo (`zooai/mirrors`) under Settings -> Secrets and variables -> Actions:

- `HANZOAI_PULL_TOKEN` -- PAT owned by `hanzo-dev` with `read:packages` on `hanzoai` AND `write:packages` on `zooai`. `hanzo-dev` is an org admin on both, so a single PAT handles both pull and push. (Docker credstore only supports one cred per registry host, so both operations must use the same credential.)

## Trigger manually

```bash
gh workflow run mirror.yml -R zooai/mirrors
gh run watch -R zooai/mirrors
```

## Verify

```bash
crane digest ghcr.io/zooai/billing:latest
```
