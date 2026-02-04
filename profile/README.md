<p align="center">
  <img src="https://artifactkeeper.com/logo-256.png" alt="Artifact Keeper" width="100" />
</p>

<h1 align="center">Artifact Keeper</h1>

<p align="center">
  <strong>The open-source artifact registry. Built for teams that refuse to pay per-seat for package hosting.</strong>
</p>

<p align="center">
  <a href="https://artifactkeeper.com">Website</a> &middot;
  <a href="https://artifactkeeper.com/docs/">Docs</a> &middot;
  <a href="https://demo.artifactkeeper.com">Live Demo</a> &middot;
  <a href="https://github.com/artifact-keeper/artifact-keeper/blob/main/LICENSE">MIT Licensed</a>
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/artifact-keeper/artifact-keeper?style=social" alt="GitHub Stars" />
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License" />
  <img src="https://img.shields.io/badge/package%20formats-45%2B-green.svg" alt="45+ Formats" />
  <img src="https://img.shields.io/badge/rust-1.75%2B-orange.svg" alt="Rust" />
</p>

---

## What is Artifact Keeper?

A full-featured, enterprise-grade artifact registry you can self-host in minutes. Drop-in replacement for JFrog Artifactory and Sonatype Nexus with **zero feature gates** — security scanning, SSO, replication, all 45+ package formats — everything ships in the open-source release.

No open-core. No "enterprise edition." No surprise invoices.

**TODO: Add screenshot of web dashboard**

## Repositories

| Repository | Description | Stack |
|:---|:---|:---|
| [`artifact-keeper`](https://github.com/artifact-keeper/artifact-keeper) | Backend server, CLI, and Docker deployment | Rust, Axum, PostgreSQL, Meilisearch |
| [`artifact-keeper-web`](https://github.com/artifact-keeper/artifact-keeper-web) | Web frontend | Next.js 15, TypeScript, Tailwind CSS, shadcn/ui |
| [`artifact-keeper-ios`](https://github.com/artifact-keeper/artifact-keeper-ios) | iOS & macOS app | SwiftUI, Swift 6, Alamofire |
| [`artifact-keeper-android`](https://github.com/artifact-keeper/artifact-keeper-android) | Android app | Jetpack Compose, Kotlin, Material 3 |
| [`artifact-keeper-api`](https://github.com/artifact-keeper/artifact-keeper-api) | OpenAPI 3.1 spec (165 endpoints) | TypeScript + Rust SDK generation |
| [`artifact-keeper-example-plugin`](https://github.com/artifact-keeper/artifact-keeper-example-plugin) | Example WASM plugin (Unity .unitypackage) | Rust, WIT, Wasmtime |

## Core Features

**45+ Package Formats** — Native protocol support. Not a generic blob store with format labels. Your package managers (`pip install`, `npm install`, `docker pull`, `cargo add`, `helm install`, `go get`, etc.) talk directly to Artifact Keeper using their native protocols.

**Security Scanning** — Automated vulnerability detection with Trivy and Grype. Policy engine with severity thresholds, quarantine workflows, and scan-before-download enforcement.

**WASM Plugin System** — Extend with custom format handlers via WebAssembly. Ship your own package format support without forking the backend.

**Edge Replication** — Mesh-based artifact distribution with swarm sync and P2P transfers between nodes. Put caches close to your build agents.

**SSO & Multi-Auth** — OpenID Connect, LDAP, SAML 2.0, JWT, and API tokens. RBAC with per-repository permissions.

**Artifactory Migration** — Built-in tooling to migrate repositories, artifacts, users, and permissions from JFrog Artifactory. One command.

**Full-Text Search** — Meilisearch-powered search across all repositories, packages, and artifact metadata.

## Mobile Apps

Manage your registries from anywhere. Monitor builds, browse repositories, trigger security scans, and administer users — all from native mobile apps with adaptive layouts.

| iOS / macOS | Android |
|:---:|:---:|
| **TODO: Add iOS app gif** | **TODO: Add Android app gif** |

## Web Dashboard

A full management interface for repositories, packages, security policies, user administration, SSO configuration, replication topology, and operational analytics.

**TODO: Add web dashboard gif**

## Quick Start

```bash
# Clone and start with Docker Compose
git clone https://github.com/artifact-keeper/artifact-keeper.git
cd artifact-keeper
docker compose up -d

# That's it. Visit http://localhost:9080
```

Or pull the pre-built images directly:

```bash
docker pull ghcr.io/artifact-keeper/artifact-keeper-backend:latest
docker pull ghcr.io/artifact-keeper/artifact-keeper-frontend:latest
```

Full deployment guides for [Docker](https://artifactkeeper.com/docs/docs/deployment/docker/), [Kubernetes](https://artifactkeeper.com/docs/docs/deployment/kubernetes/), and [AWS](https://artifactkeeper.com/docs/docs/deployment/aws/) are in the docs.

## Architecture

```
                    ┌──────────────────────────────────────────────┐
                    │              Artifact Keeper                  │
  Package Managers  │                                              │
  ─────────────────>│  Rust Backend (Axum)                         │
  pip, npm, docker, │  45+ native format handlers                  │
  cargo, helm, ...  │  WASM plugin runtime (Wasmtime)              │
                    │                                              │
                    │  ┌────────────┐  ┌─────────────┐  ┌───────┐ │
                    │  │ PostgreSQL │  │ Meilisearch │  │  S3 / │ │
                    │  │            │  │             │  │  Disk  │ │
                    │  └────────────┘  └─────────────┘  └───────┘ │
                    │                                              │
                    │  ┌──────────┐  ┌──────────┐                  │
                    │  │  Trivy   │  │  Grype   │  Security        │
                    │  │  Scanner │  │  Scanner │  Scanning        │
                    │  └──────────┘  └──────────┘                  │
                    └──────────────────────────────────────────────┘
                           │                    │
                     ┌─────┴─────┐        ┌─────┴─────┐
                     │ Edge Node │◄──────►│ Edge Node │  Mesh
                     │  (Peer)   │  P2P   │  (Peer)   │  Replication
                     └───────────┘        └───────────┘
```

## Contributing

Contributions are welcome. Pick an issue, open a PR, or start a discussion. The backend is Rust, the frontend is TypeScript/React, and the mobile apps are native Swift and Kotlin.

## License

MIT. Every feature. No exceptions.
