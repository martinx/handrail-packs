# Contributing a pack

## Before you start

- **One intent per pack.** "Keep data on this machine" is a pack; "my favourite settings" is not.
- **Check the agent's documentation.** Every settings key, permission rule and hook event you use
  must be documented by the agent. Link the page in your pull request.
- **English** for everything in the pack. Translations come later as separate files.

## Write it

```sh
mkdir -p packs/my-pack/claude-code
$EDITOR packs/my-pack/pack.toml packs/my-pack/rules.md packs/my-pack/claude-code/settings.json
handrail check .
```

`pack.toml`:

```toml
id = "my-pack"                 # lowercase, digits, hyphens; must match the directory
version = "1.0.0"
category = "security"
tier = "enforced"              # "advisory": rules.md only, installed in the user's scope
title = "Short title"
summary = "One or two sentences."
protects = ["What it guards against"]
tradeoffs = ["What the user gives up"]
limits = "What it cannot do. Required."

[targets.claude-code]
enforcement = "enforced"       # enforced | partial | advisory | unsupported
min_version = "2.1.242"        # optional: the oldest agent version with the keys you use
settings = "claude-code/settings.json"
hooks = ["claude-code/hooks/guard.sh"]
```

In `settings.json`, `@PACK_DIR@` becomes the directory the pack is installed to, so a hook is
referenced as `"\"@PACK_DIR@/hooks/guard.sh\""`.

## What `handrail check .` enforces

- Required fields, no unknown fields, no unknown placeholders, ids matching directories.
- Settings files are JSON objects.
- **No two packs set the same scalar key to different values** — the agent would silently let
  the later one win.
- Advisory packs ship `rules.md` only.
- Every hook's test vectors pass.
- Shell scripts never put a `$VAR` directly before non-ASCII text (sh reads the bytes as part of
  the variable name; use `${VAR}`).

## Review

- `enforcement` must match reality. `partial` with an honest `limits` beats an `enforced` that
  isn't. A pack that claims more than it enforces is a bug.
- **Packs with executable hooks run as part of every tool call on the user's machine.** They get
  a line-by-line review; keep them short, POSIX `sh`, no network, no dependencies beyond
  `grep`/`sed`, and exit 0 unless they are deliberately blocking.
- Bump `version` whenever a pack's behaviour changes.
