# Releasing Lyvoxa

This document describes how to create a new GitHub Release (and set it as **Latest**) using the repository automation.

## Release Triggers (Automated)

Lyvoxa publishes releases from **tags**.

### Option A (Recommended): Release commit triggers auto-tag

1. Update versions across the repository (see `docs/MAINTENANCE.md` and `docs/VERSION_MANAGEMENT.md`).
2. Commit with the exact format below (must match):

```
chore(release): bump to X.Y.Z
```

Example:

```
chore(release): bump to 3.2.0
```

3. Push to `main`.

What happens next:

- Workflow **🏷️ Auto Tag Release** creates an annotated tag named `X.Y.Z`.
- Tag push triggers **🔧 CI Pipeline**.
- When CI succeeds, workflow **🌟 Release** builds artifacts and publishes a GitHub Release with:
  - `make_latest: true`
- Workflow **📦 AUR Sync** runs after the release and syncs the AUR package.

### Option B (Manual): Create a tag

If you prefer manual tags:

1. Make sure `main` is green.
2. Create and push a tag:

```bash
git tag -a 3.2.0 -m "Release 3.2.0"
git push origin 3.2.0
```

This triggers the same pipeline: **CI → Release → AUR**.

## Commit Rules (Maintenance)

### Conventional Commits (Required)

Use Conventional Commit subjects for consistent automation:

- `feat(scope): ...`
- `fix(scope): ...`
- `docs(scope): ...`
- `chore(scope): ...`
- `ci(scope): ...`
- `refactor(scope): ...`
- `perf(scope): ...`
- `test(scope): ...`

Scopes commonly used in this repository:

- `tui`
- `monitor`
- `plugin`
- `config`
- `ci`
- `release`
- `deps`
- `docs`

### DCO Sign-off (Required)

Every commit must include a DCO sign-off.

Use:

```bash
git commit -s -m "feat(tui): add new panel"
```

## Notes

- Release tags are **numeric** (`X.Y.Z`) and are used directly as `tag_name`.
- You can also use `vX.Y.Z` tags for humans, but ensure the pipeline still receives the intended tag name.
- If a tag already exists, the auto-tag workflow will skip creating it.
