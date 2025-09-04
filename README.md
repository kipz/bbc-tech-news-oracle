# BBC Technology News Oracle

A GitHub Actions-based oracle system that creates and verifies attestations for the BBC Technology RSS feed content. This project ensures the integrity and authenticity of BBC Technology news content through cryptographic attestations.

## Overview

This oracle system monitors the BBC Technology RSS feed (`http://newsrss.bbc.co.uk/rss/newsonline_uk_edition/technology/rss.xml`) and creates cryptographic attestations for the content. It uses GitHub Actions workflows to:

1. **Create Attestations**: Generate attestations for RSS feed content and create GitHub releases
2. **Verify Attestations**: Download and verify existing attestations from releases

## Features

- **Automated Attestation Creation**: Creates attestations on every push to main branch or manual trigger
- **Content Digest Tracking**: Uses content digests to prevent duplicate releases
- **Release Management**: Automatically creates GitHub releases with attestation files
- **Verification Workflow**: Allows manual verification of existing attestations
- **Integration with URL Oracle**: Uses the [`kipz/url-oracle`](https://github.com/kipz/url-oracle) reusable workflows

## Workflows

### 1. Attest BBC Technology RSS Feed (`attest-bbc.yml`)

**Triggers:**
- Push to main branch
- Manual workflow dispatch

**Process:**
1. Creates attestation using the URL Oracle workflow (requires `id-token: write`, `contents: write`, `actions: read` permissions)
2. Downloads the generated attestation.json
3. Checks for existing releases with the same content digest
4. Creates a new release only if content has changed
5. Tags releases with format: `content-{digest}`

### 2. Verify BBC Technology RSS Feed Attestation (`verify-bbc.yml`)

**Triggers:**
- Manual workflow dispatch with release name input

**Process:**
1. Downloads attestation.json from specified release
2. Verifies the attestation using the URL Oracle verification workflow

## Usage

### Creating Attestations

Attestations are automatically created when:
- Code is pushed to the main branch
- The workflow is manually triggered

### Verifying Attestations

To verify an existing attestation:

1. Go to the Actions tab in GitHub
2. Select "Verify BBC Technology RSS Feed Attestation"
3. Click "Run workflow"
4. Enter the release name (format: `content-{digest}`)
5. The workflow will download and verify the attestation

### Finding Release Names

Release names follow the pattern `content-{digest}` where `{digest}` is the content digest of the RSS feed content. You can find available releases in the GitHub Releases section of this repository.

## Configuration

The system requires the following GitHub secrets:

- `ORACLE_TOKEN`: Token for accessing the URL Oracle service

## Dependencies

This project uses reusable workflows from:
- `kipz/url-oracle/.github/workflows/create-attestation.yml@main`
- `kipz/url-oracle/.github/workflows/verify-attestation.yml@main`

## RSS Feed

The oracle monitors: `http://newsrss.bbc.co.uk/rss/newsonline_uk_edition/technology/rss.xml`

This feed contains the latest BBC Technology news articles and is updated regularly by the BBC.