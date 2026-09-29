# AGENTS.md — layer-xdg-portal

Standalone candy repo for the `xdg-portal` layer — the XDG Desktop Portal
infrastructure (daemon, wlroots backend, GTK fallback) for Sway containers. The
candy lives in `charly.yml` at the repo root: the `require:` list, the
`XDG_CURRENT_DESKTOP` env, the per-distro packages, the `copy:` plan steps, the
`check:` assertions, and the embedded `skill:` entity projected into the
marketplace corpus as `/charly-selkies:xdg-portal`.

Canonical files:

- `charly.yml` — the `xdg-portal:` candy entity and the `xdg-portal-skill:` skill
  entity.
- `portal.conf` — the Sway `config.d` drop-in copied to
  `~/.config/sway/config.d/portal.conf`.
- `xdg-desktop-portal-wlr-config` — the portal config copied to
  `~/.config/xdg-desktop-portal-wlr/config`.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:xdg-portal` — the owning skill. The D-Bus activation model,
  the sway `config.d` environment export, and the portal capability table. Load
  before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, service declarations). Load before editing any entity field or plan
  step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence — the portal
  packages, `slurp`, and both config files at their expected paths.
- The `arch` arm covers both arch and cachyos (the cachyos base declares
  `distro: [cachyos, arch]`); keep the four package names in sync across arms.

## Modify this repo

- Edit the `xdg-portal:` candy entity AND the `xdg-portal-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a behaviour
  change not mirrored in the skill leaves the corpus stale.
- A change to either config file belongs in its file and in the matching `copy:`
  step / `check:` assertion.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
