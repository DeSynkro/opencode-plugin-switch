# Changelog

## 0.1.0

- Initial release.
- Interactive terminal switch for global OpenCode plugins and skills.
- Moves turned-off items into `~/.config/opencode/.disabled/{plugins,skills}`
  and back, using a single rename per item.
- Creates the live and disabled directories on first run.
- `--list`, `--install`, `--version`, `--help`.
- Optional reload prompt after applying (`opencode service restart`), default no.
