# 01 — GCR Package Publishing via glossarist Ruby Gem

## Goal

This repository publishes the IEV (IEC Electropedia) GCR package as a GitHub Release asset. The vocabulary-browser downloads this to build www.geolexica.org.

## Repository Info

- **Dataset ID:** `iev`
- **Owner:** IEC TC 1
- **Concept format:** v1 (`concepts/*.yaml` with `termid`) — pre-harmonized
- **Concept count:** 22,228
- **Languages:** 18 (eng, ara, deu, fra, spa, ita, jpn, kor, pol, por, srp, swe, zho, rus, fin, dan, nld, msa)
- **GCR release asset:** `iev.gcr`
- **GCR size:** ~23 MB

## Current State

- Repository was empty. Initialized with README + `publish-gcr.yml` workflow
- No concept data in the repo itself — data is sourced externally
- The workflow uses vocabulary-browser's Node.js scripts to build GCR
- A GCR package was manually uploaded to the `gcr-latest` release

## Special Considerations

Unlike the other glossary repos, this repo does NOT contain concept YAML files in git. The IEV data (22,228 concepts) is sourced from IEC and is too large / restricted for direct git storage. The GCR package must be built from an external source.

### Options for sourcing IEV data:

1. **Manual upload** — Build GCR from external source and upload as release asset (current approach)
2. **Scheduled CI** — Workflow fetches from IEC source, builds GCR, publishes release
3. **Push trigger** — Workflow runs when concept files are pushed to the repo

## Tasks

### 1. Determine IEV data source

Decide where the raw IEV concept data comes from:
- IEC Electropedia API or export?
- Internal glossarist pipeline?
- Manual export from IEC?

### 2. Update publish-gcr.yml based on data source

If concepts are available in the repo:
```yaml
name: publish-gcr

on:
  push:
    paths: ['concepts/**']
  workflow_dispatch:

permissions:
  contents: write

jobs:
  publish-gcr:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'

      - run: gem install glossarist

      - name: Build GCR
        run: |
          glossarist package . -o iev.gcr \
            --title "IEC Electropedia (IEV)" \
            --owner "IEC TC 1"

      - name: Update gcr-latest release
        uses: softprops/action-gh-release@v2
        with:
          tag_name: gcr-latest
          name: "GCR Package (latest)"
          files: iev.gcr
```

If data is external, the workflow needs a fetch step before packaging.

### 3. Remove vocabulary-browser dependency

Delete the current workflow that checks out vocabulary-browser. Replace with glossarist gem.

## GCR Download URL

```
https://github.com/glossarist/glossarist-data-iev/releases/download/gcr-latest/iev.gcr
```

## Acceptance Criteria

- [ ] `publish-gcr.yml` uses `gem install glossarist` (no Node.js)
- [ ] GCR contains 22,228 concepts with 18 languages
- [ ] GCR uploaded to `gcr-latest` release
- [ ] Clear documentation on how IEV data is sourced
