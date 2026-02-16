# Drift package index

Curated index for Drift user-land packages.

This repository is the discovery/catalog layer for packages in the Drift ecosystem.
It is intentionally separate from `lang-toolchain` (compiler/runtime source).

## Scope

- Curated package metadata (what exists, versions, where to fetch)
- Signature and integrity metadata references (hashes, sidecars, signer ids)
- Policy-friendly organization for namespace ownership and review

Not in scope:

- Compiler/runtime source code
- Trust bypass or signature bypass
- Executing package code at index time

## Security Model

`driftc` remains the final gatekeeper.

This index is advisory/discovery metadata.
Package consumption is accepted only if local toolchain verification passes:

- package hash matches
- signature sidecar is valid
- signer is trusted for namespace
- signer is not revoked
- local trust policy allows it

## Repository Layout (proposed)

```text
index/
	index.v0.json            # top-level catalog
	namespaces/
		drift.foundation.json  # namespace ownership/policy metadata
packages/
	<namespace>/<name>/
		<version>.json         # package release metadata
docs/
	policy.md                # curation and namespace policy
	signing.md               # signing/release requirements

## Metadata Principles

- Deterministic, machine-readable JSON
- No hidden mutable state
- Explicit package identity (package_id, version)
- Explicit artifact location(s)
- Explicit integrity (sha256)
- Explicit signature sidecar path
- Explicit signer IDs (kid) where applicable

## Typical Publish Flow

1. Package author builds .dmp artifact.
2. Author signs artifact (.dmp.sig).
3. Author submits index entry PR with:
		- package id/version
		- artifact URL/path
		- sidecar URL/path
		- hash
		- changelog/notes (optional)
4. Curator reviews policy + metadata consistency.
5. Consumers fetch via configured sources and compile with trust enforcement.

## Consumption Model

Projects configure package sources and trust stores locally.
They may use this curated index, private indexes, or both.

## Relationship to lang-toolchain

- lang-toolchain defines format, verification behavior, and tooling.
- drift-package-index publishes package metadata for discovery.

## Status

Bootstrap repository.
Initial schema and policy docs are expected to evolve during MVP hardening.

