# homebrew-unravel

Homebrew tap for [unravel](https://github.com/roo-oliv/unravel) — an AI-powered CLI that decomposes PR diffs into causal threads for human code reviewers.

## Install

```bash
brew tap roo-oliv/unravel
brew install unravel-review
```

`brew install unravel` also works — it's a tap-level alias for `unravel-review` (see `Aliases/unravel`). The installed CLI binary is `unravel` regardless of which name you install under.

## Upgrade

```bash
brew update
brew upgrade unravel-review
```

## Uninstall

```bash
brew uninstall unravel-review
brew untap roo-oliv/unravel
```

## About this tap

This repository contains the Homebrew formula for `unravel-review` — matching the PyPI package name for consistency across channels. The formula vendors all transitive Python dependencies as `resource` blocks so installs are reproducible and don't pull from PyPI at install time.

## Layout

```
Formula/
  unravel-review.rb     # canonical formula; class UnravelReview
Aliases/
  unravel -> ../Formula/unravel-review.rb   # tap-level alias
```

The alias is a plain symlink. Homebrew resolves it transparently: `brew info unravel` and `brew info unravel-review` return the same formula.

## Releasing

The formula is intended to be regenerated on each release of the main repo. Until the automation is in place (see the TODO block at the top of `Formula/unravel-review.rb`), the process is:

1. Publish a new release of `unravel-review` to PyPI.
2. In a scratch directory:
   ```bash
   uv venv --python 3.12 .poet && source .poet/bin/activate
   uv pip install 'setuptools<81' homebrew-pypi-poet unravel-review==<new-version>
   poet -f unravel-review > new-formula.rb
   ```
3. Copy the generated `url`/`sha256` and every `resource` block into `Formula/unravel-review.rb`, preserving the `desc`, `homepage`, `depends_on`, `install`, and `test` sections.
4. Commit and push. Users get the update on their next `brew update`.

## Audit

Before publishing a formula update:

```bash
brew style Formula/unravel-review.rb
brew audit --strict --online roo-oliv/unravel/unravel-review
brew test roo-oliv/unravel/unravel-review
```

## License

The formula itself is BSD-2-Clause (matching Homebrew convention). The `unravel` software it installs is AGPL-3.0-or-later.
