# The Papyria fork of pandoc

This is [Papyria](https://github.com/papyria)'s fork of
[jgm/pandoc](https://github.com/jgm/pandoc). We build our own pandoc
(pandoc-server, in papyria-workflows' `Dockerfile.pandoc-server`) from it
because we depend on fixes that upstream has not released yet.

We don't want a long-lived divergent fork. Every patch we carry is also an
upstream pull request, and we only carry it until a pandoc release includes it.

## Branches

| Branch | What it is |
|---|---|
| `main` | Mirrors upstream `main`, plus this file. |
| `fix-*` | One feature branch per patch, branched from upstream `main`. It is the head branch of that patch's upstream PR. |
| `X.Y-papyria` | The latest pandoc release tag `X.Y`, with every patch that release lacks cherry-picked onto it (`git cherry-pick -x`). This is what we build and deploy. |

To sync `main`, run `git fetch upstream && git merge upstream/main` and push.
This never needs a force-push. Upstream's `.gitignore` ignores top-level files with a dot in their
name, so this file was added with `git add -f`. Don't change `.gitignore`.

We never build from upstream `main`, because it ships unreleased changes we
have not asked for. We build from a release with our patches added.

## Workflow

### A new patch

1. Open an issue on jgm/pandoc with a minimal, reproducible example (see
   upstream's [CONTRIBUTING.md](CONTRIBUTING.md)).
2. Branch `fix-<topic>` from `upstream/main`. Make one logical commit, with a
   test under `test/command/<issue>.md`. Keep commit message lines at 78
   characters or less, because upstream CI checks this.
3. Run the full test suite: `make test`, plus the `pandoc-lua-engine` suite,
   which `make test` skips.
4. Push the branch to this fork, and open a PR against jgm/pandoc that says
   `Closes #<issue>`.
5. Cherry-pick the commit with `-x` onto the current `X.Y-papyria` branch,
   push it, and pin the new head SHA in papyria-workflows'
   `Dockerfile.pandoc-server`.
6. Add the patch to the table below.

### A new upstream release

When pandoc `X.Z` is released:

1. `git fetch upstream --tags`
2. `git switch -c X.Z-papyria X.Z`
3. Find out which carried patches `X.Z` already includes. Move those to
   **Accepted upstream** below.
4. Cherry-pick (`-x`) each remaining `fix-*` commit onto `X.Z-papyria`, then
   run the tests and push.
5. Update `Dockerfile.pandoc-server` to the new head SHA.
6. Delete the previous `X.Y-papyria` branch only after nothing pins one of
   its commits any more.

If a release includes every patch, we can drop the fork. papyria-workflows
then goes back to the stock `pandoc/core` image, as described by the
deletion trigger in its `Dockerfile.pandoc-server`.

### When a PR is merged

Delete its `fix-*` branch, here and locally. Keep the cherry-pick on the
current `X.Y-papyria` until a release includes the patch.

## Carried patches

Current release branch: **`3.12-papyria`** (based on pandoc 3.12).

| Branch | Upstream issue / PR | Commit | Status |
|---|---|---|---|
| _(deleted)_ `fix-11862` | jgm/pandoc#11862 / jgm/pandoc#11863 | Markdown writer: escape list markers after a line break. | Merged 2026-10-06 (after 3.12). Carried until 3.13. |
| `fix-fldsimple` | jgm/pandoc#11944 / jgm/pandoc#11946 | Docx reader: read w:fldSimple fields. | Open |
| `fix-wide-grid-tables` | jgm/pandoc#11945 / jgm/pandoc#11947 | Markdown writer: write wide tables as grid tables. | Open |

## Accepted upstream

Patches that a pandoc release includes, so we no longer carry them.

| Upstream issue / PR | Commit | Released in |
|---|---|---|
| _(none yet)_ | | |
