# layer-xdg-portal

XDG Desktop Portal infrastructure for OpenCharly Sway containers.

The `xdg-portal` candy installs `xdg-desktop-portal` with the wlroots
(`xdg-desktop-portal-wlr`) and GTK backends plus `slurp` (the region selector),
and drops the Sway `config.d` and portal config that wire screencasting through
PipeWire on a sway session. It enables screen sharing, portal screenshots, and
file dialogs for applications inside the container.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `xdg-portal` |
| Packages | `xdg-desktop-portal`, `xdg-desktop-portal-wlr`, `xdg-desktop-portal-gtk`, `slurp` (fedora / arch) |
| Env | `XDG_CURRENT_DESKTOP=sway` |
| Requires | `pod-dbus`, `pod-sway`, `pod-pipewire` |
| Config | `~/.config/sway/config.d/portal.conf`, `~/.config/xdg-desktop-portal-wlr/config` |
| Service / port | none (D-Bus activation on demand) |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list — typically
transitively through the `sway-desktop` metalayer:

```yaml
my-desktop-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-xdg-portal:v2026.243.2105'
```

The portal daemons start on demand when an application calls a portal D-Bus
method. The wlr config installs `chooser_type=none` so screen sharing is
auto-approved for headless automation.

The candy's `plan:` asserts the `xdg-desktop-portal` and `xdg-desktop-portal-wlr`
packages, the `slurp` binary at `/usr/bin/slurp`, the wlroots portal config file,
and the Sway `config.d` drop-in.

## Layout

- `charly.yml` — the `xdg-portal:` candy entity (the `require:` list, the
  `XDG_CURRENT_DESKTOP` env, the per-distro packages, the `copy:` plan steps, the
  `check:` assertions) and the embedded `xdg-portal-skill:` skill entity.
- `portal.conf` — the Sway `config.d` drop-in.
- `xdg-desktop-portal-wlr-config` — the portal config with `chooser_type=none`.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:xdg-portal`
- `/charly-selkies:sway-desktop` — desktop metalayer that includes this candy
- `/charly-infrastructure:dbus-layer` — D-Bus session bus (required)
- `/charly-selkies:pipewire` — PipeWire (required for ScreenCast)
- `/charly-selkies:sway` — Sway compositor (required)
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
