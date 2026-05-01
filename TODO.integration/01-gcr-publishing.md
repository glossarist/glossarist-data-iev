# 01 — Versioned GCR Publishing via glossarist Ruby Gem

## Goal

This repository publishes versioned GCR packages as GitHub Releases, triggered by version tags.

## Repository Info

| Field | Value |
|-------|-------|
| **Dataset ID / shortname** | `iev` |
| **Owner** | IEC TC 1 |
| **Format** | v1 (`concepts/concept-*.yaml` with `termid`) |
| **Concepts** | 22,228 |
| **Languages** | 18 |
| **GCR filename** | `iev-{version}.gcr` |
| **GCR size** | ~23 MB |

## Release Convention

| Trigger | Tag | Assets |
|---------|-----|--------|
| Push tag `v1.0.0` | `v1.0.0` | `iev-1.0.0.gcr` + `iev.gcr` |
| workflow_dispatch | `v{version}` | `iev-{version}.gcr` + `iev.gcr` |

### Download URLs
```
https://github.com/glossarist/glossarist-data-iev/releases/latest/download/iev.gcr
https://github.com/glossarist/glossarist-data-iev/releases/download/v1.0.0/iev-1.0.0.gcr
```

## How to publish

```bash
git tag v1.0.0
git push origin v1.0.0
```

## Acceptance Criteria

- [ ] `publish-gcr.yml` triggers on `v*` tag push only
- [ ] GCR contains 22,228 concepts, 18 languages
- [ ] `metadata.yaml` has `shortname: iev` and `version`
- [ ] Release has both versioned and unversioned GCR assets
- [ ] No checkout of vocabulary-browser
