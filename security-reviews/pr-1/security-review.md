# Security Review – PR #1

This document captures potential security issues introduced or exposed by the changes in https://github.com/gorbagana-dev/gorbagana-gitbook/pull/1. Findings are prioritized by potential impact and likelihood.

## Findings

### 1. Binary downloads lack integrity verification (Medium)
The new laconic-so installation guide repeatedly instructs operators to fetch pre-built binaries and helper scripts directly over HTTPS via `curl` without verifying signatures or even a published checksum. That includes both Docker Compose (`curl -SL https://github.com/docker/compose/...`) and the laconic-so release payload that will later orchestrate validator/RPC deployments.【F:security-reviews/pr-1/installing-laconic-so.md†L14-L75】 A compromise of `git.vdb.to`, GitHub release artifacts, or any on-path TLS termination point would allow attackers to distribute a trojanized orchestrator that operators are told to run with elevated privileges. Mitigation: require checksum/signature verification (and publish official checksums/keys), or install from a package repository that already enforces integrity.

### 2. Documentation encourages exposing RPC ports without authentication (Medium)
The new run-book explicitly tells operators to make RPC, WebSocket, and gossip ports accessible externally (using host networking) and to set `PUBLIC_RPC_ADDRESS` if they want to be discoverable. There is no mention of enabling TLS termination, rate limiting, or authentication to protect the Solana-compatible RPC interface before it is opened to the internet. The troubleshooting guide even directs operators to `sudo ufw allow 8899/tcp` and `sudo ufw allow 8900/tcp`, which fully exposes those unauthenticated services to the world.【F:security-reviews/pr-1/running-your-rpc-node.md†L61-L88】【F:security-reviews/pr-1/troubleshooting.md†L82-L111】 Exposed Solana RPC nodes are routinely abused for free compute, spam, and DoS amplification. Mitigation: document how to restrict access (firewall allow-lists, authenticated proxies), enforce TLS, and mention rate limiting/monitoring guidance before advising operators to open the ports.

### 3. New unpinned `honkit` dependency increases supply-chain risk (Low/Medium)
`package.json` now declares `honkit` with a `^4.0.0` range but the repository still has no lockfile. That means every install in CI or on contributors' machines will pick up the latest `4.x` release, including any compromised versions that attackers may publish. Because HonKit executes a CLI with access to the filesystem when building docs, a malicious release could run arbitrary code during install or build.【F:security-reviews/pr-1/package.json†L5-L20】 Mitigation: commit a `package-lock.json`, pin HonKit to an exact vetted version, and add dependency-audit checks to CI.

## Overall assessment
The PR mostly adds operator documentation, but the instructions currently lower the bar for a supply-chain attack on validator operators and risk exposing RPC nodes to the public internet without hardening steps. Addressing the issues above before merging will reduce the likelihood of compromise or service abuse.
