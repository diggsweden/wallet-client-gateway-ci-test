<!--
SPDX-FileCopyrightText: 2026 Digg - Agency for Digital Government
SPDX-License-Identifier: CC0-1.0
-->

# Wallet Client Gateway CI Test

This is a standalone test copy intended for `diggsweden/wallet-client-gateway-ci-test`.
All reusable-ci workflow calls and helper refs use v3 candidate `fa23c718c0621c668c8b35666e0d31ea3a3ad357`, including the release-tag fix.
Publish that reusable-ci revision before pushing these consumer updates.
The default local branch is `main`.

## Local State

The tracked source was copied without its Git history, tags, remotes, build outputs or unrelated untracked directories.
No commit, push, GitHub repository creation or CI execution was performed automatically.
The original application's version is retained: `0.6.11`.

The Maven artifact and JAR are named `wallet-client-gateway-ci-test`.
The application/API classes and schema filenames retain their original names to preserve behaviour.
The ecosystem integration workflow retains the logical service role `wallet-client-gateway`, but reads this test repository through `github.repository`.
Historical changelog links still describe the source project's history.

## GitHub Setup

Create a new, empty **public** GitHub repository named `wallet-client-gateway-ci-test` under the intended organisation.
Connect this directory only to that new repository, then commit and push `main` yourself when ready.
No remote is configured locally.

Grant the test repository access to:

- `RELEASE_TOKEN`: release-bot token with Contents read/write and access to the test repo.
- `RELEASE_GPG_PRIVATE_KEY`, `RELEASE_GPG_PASSPHRASE`, `RELEASE_GPG_PUBLIC_KEY`: the matching signing credentials.
- `CODE_SCANNING_TOKEN`: optional Code Scanning upload token with access to the test repo.

The bot account and repository rules must permit the release commit and signed-tag update.
GHCR uses the test repository's automatic `GITHUB_TOKEN`.

## Flows To Exercise

1. **Push to main:** v3 PR-quality checks and the project's Maven tests run.
2. **Pull request:** the same v3 checks and tests run; the existing ecosystem integration workflow also runs.
3. **Dev release:** manually run `Release Workflow Dev` on `main` or a test branch.
4. **Release:** merge the test PR, update local `main` from GitHub, then create and push a new signed version tag on that exact branch tip.
   The workflow sets the project version from the tag name; tags on a pre-merge PR commit are rejected before version changes are pushed.
5. **Scorecard:** run it manually if wanted; public Scorecard API publication is disabled for this test copy.

Release and dev-release are real publishing workflows once triggered.
With the intended GitHub repository name, the container destination is `ghcr.io/diggsweden/wallet-client-gateway-ci-test`.
The JAR and SBOMs are attached to this test repository's GitHub release; Maven Central publishing is not configured.

For this public test repository, the original SLSA defaults are retained.
If you instead create it as a private repository without Enterprise Cloud, set `enable-slsa: false` on the existing container entry and omit Code Scanning token mappings unless Code Security is enabled.

If a previous run pushed a release commit but failed at tag movement, review and update `main` before choosing a new release version and tag.
Rerunning the old pre-merge tag is not an automatic recovery procedure.
