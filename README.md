<p align="center">
  <img src="logo.png" alt="opencode plugin switch" width="480">
</p>

<p align="center">
  <strong>Turn OpenCode plugins and skills on and off.</strong><br>
  One terminal screen, no files deleted, nothing to remember.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT"></a>
  <img src="https://img.shields.io/badge/version-0.1.0-orange.svg" alt="Version 0.1.0">
  <a href="https://ko-fi.com/desynkro"><img src="https://img.shields.io/badge/Ko--fi-support%20me-ff5e5b?logo=kofi&amp;logoColor=white" alt="Support me on Ko-fi"></a>
</p>

<p align="center">
  Built by <a href="https://desynkro.com">DeSynkro</a> with assistance from <a href="https://opencode.ai">Opencode</a>.
</p>

I made this script to switch my OpenCode plugins and skills off without moving things around by hand. I keep my plugins as symlinks inside OpenCode's config directory, so turning one off is just moving the link out and back. This script finds everything in `plugins/` and `skills/`, shows it in one screen, and toggles it for you. Anything you turn off is moved to a `.disabled` folder in the same config directory.

## Why this exists

OpenCode loads every plugin and skill in its config directory. Most of the time that is exactly what I want. Sometimes I want a quieter session: no reverse engineering tools, no extra skills in the model's list. Deleting them felt wrong, and remembering what I had removed felt worse, so I wrote a switch instead. Turn things off, turn them back on, nothing is lost.

## Status

A personal project. It does what I need and keeps improving.

## Requirements

- **Node.js** 18 or newer. It uses `node:` prefixed modules.
- **A terminal.** The switch draws an interactive screen, so it needs a real TTY.

No dependencies, no build step, no `npm install`. The whole tool is one Node script.

## Quick Start

```sh
cd ~/Projects/Code/opencode-plugin-switch
./ops
```

To run it from anywhere, install the symlink once:

```sh
./ops --install
ops
```

That links `~/.local/bin/ops` to this folder. Edit the file and the command picks it up. Remove the symlink to uninstall it.

### Keys

| Key | Action |
|---|---|
| `up` `down`, `j` `k` | move |
| `space` | toggle the highlighted item |
| `a` | turn all on |
| `n` | turn all off |
| `enter` | apply the pending changes |
| `q`, `esc` | quit without changing anything |

Nothing moves until you press `enter`. Quitting drops the pending toggles.

After applying, `ops` prints what moved and asks whether to reload OpenCode. The default is no, so it never restarts a running session by surprise. Say yes, or run `/reload` inside OpenCode yourself.

## How it works

```text
~/.config/opencode/
├── plugins/              live plugins
├── skills/               live skills
└── .disabled/
    ├── plugins/          turned-off plugins
    └── skills/           turned-off skills
```

Turning something off is one `rename()` into `.disabled/`. Turning it back on is the same `rename()` in reverse. The folder layout is the record, so there is no manifest to lose.

`.disabled/` sits beside `plugins/`, never inside it, so a disabled plugin can not be picked up as a plugin OpenCode should load.

On a new machine there is nothing to set up. The first run creates `plugins/`, `skills/` and both `.disabled` folders with `mkdir -p` semantics.

## Features

- **Detects what you have:** plugins as directories, symlinks or `.js` / `.ts` files, and skills as folders with a `SKILL.md` or flat `.md` files.
- **One screen:** Plugins and Skills in separate sections, with live on/off counts.
- **Bulk switches:** `a` turns everything on, `n` turns everything off.
- **Safe moves:** one rename per item, refused if the source is missing or the destination exists, and every refusal is printed.
- **Symlink aware:** links are moved as links, their targets are never touched.
- **Fresh machine friendly:** creates the config folders it needs.
- **Script friendly:** `ops --list` prints a plain on/off list.
- **Optional reload:** asks before running `opencode service restart`.

## Command line

```sh
./ops              # interactive switch
./ops --list       # plain on/off list
./ops --install    # symlink into ~/.local/bin/ops
./ops --version
./ops --help
```

`XDG_CONFIG_HOME` is respected. If it is set, `opencode/` is read from there instead of `~/.config/`.

## What it does not manage

- Skills that a plugin injects at runtime. They are not files, so they appear and disappear with their plugin.
- Plugins installed with `opencode plugin add`. Those live in `opencode.jsonc`, not in `plugins/`.
- Project level folders. This tool only touches the global config directory.

## Troubleshooting

### A plugin is missing from the list

Only immediate entries in `~/.config/opencode/plugins` are managed. Plugins added with `opencode plugin add` live in `opencode.jsonc`.

### A skill is missing from the list

It needs a folder with a `SKILL.md` inside, or to be a flat `.md` file directly in `skills/`.

### Changes do not take effect

Run `/reload` inside OpenCode, or `opencode service restart`. Plugin code changes always need a restart.

### `ops` says it needs an interactive terminal

Use `--list` when piping or running from a script.

## Support

If you find this useful, you can support me on [Ko-fi](https://ko-fi.com/desynkro). You can find more of my work at [desynkro.com](https://desynkro.com).

## License

MIT. See [LICENSE](LICENSE).

## Credits

- Built by [DeSynkro](https://desynkro.com)
- Assisted by [Opencode](https://opencode.ai)
- Made for my own setup and shared for anyone who finds it useful

<p align="center">
  Made with ♥️ for anyone who likes a quieter OpenCode.
</p>
