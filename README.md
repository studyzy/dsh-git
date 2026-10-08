# dsh-git

A DSH plugin that adds a **Git** page to the right sidebar: the working tree's changes and the
recent commits on one side, and the diff a click opens on the other — read-only, highlighted,
side by side. The list stays where it is, so two diffs can be put beside each other.

English | [中文](README.zh.md)

![The Git page: changes and commits on the left, the diff a click opens beside them. Click a commit to expand its files.](docs/screenshots/en/01-log.png)

| Two columns, wrapped | Two columns, unwrapped | One column |
| --- | --- | --- |
| ![The default: long lines wrap inside their half](docs/screenshots/en/02-diff-split.png) | ![Each half scrolls its own long lines; line numbers stay put](docs/screenshots/en/03-diff-unwrapped.png) | ![One column, each change as its removal then its insertion](docs/screenshots/en/04-diff-inline.png) |

## Install

Needs Node 22.19+ (or 24+) and the DSH CLI. This plugin follows the `0.2.0` DSH line
(`^0.2.0-rc.2`).

For the Web profile:

```sh
dsh plugin --profile web add @lengmoxxl/dsh-git
dsh --profile web
```

For DSH Desktop: the desktop profile is managed by the Electron app, which does not run a
package manager on demand, so install it the way a manually linked layer is composed — add
a `link:` dependency plus a `dsh.profile.bundles` entry to
`~/.dsh/profiles/desktop/package.json`, and symlink the package into the profile's
`node_modules/@lengmoxxl/`, then restart the app.

```jsonc
// ~/.dsh/profiles/desktop/package.json
"dependencies": { "@lengmoxxl/dsh-git": "link:/path/to/dsh-git" },
"dsh": { "profile": { "bundles": [ /* …, */ "@lengmoxxl/dsh-git" ] } }
```

```sh
ln -s /path/to/dsh-git ~/.dsh/profiles/desktop/node_modules/@lengmoxxl/dsh-git
```

Restart the app afterwards: the Git page is reached from the right Sidebar's add control.

## Requirements, permissions, and limits

- A `git` executable on `PATH`, and a Git working tree as the session's working directory.
- Reads that repository through the Harness filesystem and runs `git` through the Harness subprocess: no network, no credentials, no other path.
- Writes nothing — not the tree, not the index. Commands run with the DSH process's own privileges, and a large repository or diff takes time to gather and draw.

## Release

From a clean `main`:

```sh
npm version patch -m "Cut %s"   # or minor / major
npm publish
```

## License

MIT
