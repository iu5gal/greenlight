# Greenlight — IU5GAL's build (`iu5gal-build`)

🇮🇹 [Versione italiana](BUILD-IU5GAL.md)

This branch is **my personal build** of [Greenlight](https://github.com/unknownskl/greenlight) 2.4.2:
the original `main-v2` plus the fixes I have proposed to the maintainer that have not been merged yet.
**It is not a separate project and it is never proposed upstream**: every change starts in its own branch,
becomes a pull request to `unknownskl/greenlight`, and is then merged here.

## What it contains

| PR | Branch | Content |
|----|--------|---------|
| [#1705](https://github.com/unknownskl/greenlight/pull/1705) | `chore/remove-dsstore` | `.DS_Store` is no longer tracked |
| [#1706](https://github.com/unknownskl/greenlight/pull/1706) | `feature/italian-language` | Italian translation (it-IT) |
| [#1707](https://github.com/unknownskl/greenlight/pull/1707) | `fix/webui-express5` | The Web UI did not start with Express 5 |
| [#1709](https://github.com/unknownskl/greenlight/pull/1709) | `fix/i18n-keys` | Translation keys aligned between code and language files |
| [#1717](https://github.com/unknownskl/greenlight/pull/1717) | `feature/languages` | French, Brazilian Portuguese, Turkish, Simplified Chinese, Japanese, Korean; Spanish becomes `es-ES.json` |
| [#1718](https://github.com/unknownskl/greenlight/pull/1718) | `fix/settings-polish` | Settings refinements: reset to defaults, validated Web UI port, descriptions |
| [#1719](https://github.com/unknownskl/greenlight/pull/1719) | `feature/i18n-system-language` | Saved language applied at startup, native dialogs translated, system language on first run |
| [#1720](https://github.com/unknownskl/greenlight/pull/1720) | `fix/macos-controller` | Controller input and vibration on macOS since 2.4.2 |
| [#1721](https://github.com/unknownskl/greenlight/pull/1721) | `fix/ui-polish` | Menu entry highlighted from the current page; queue countdown fixed, with a progress bar |
| [#1682](https://github.com/unknownskl/greenlight/pull/1682) | (by vishalrao8) | Controller numbering in Settings → Input. Merged here only |

The current status of each PR is on the PR page itself: once the maintainer merges one, it simply becomes
code that is already in `main-v2`.

## Build and run

You need Node 24 (CI) or 22 (`.nvmrc`) and Yarn 1.22.

```bash
git switch iu5gal-build
yarn
yarn desktop build --no-pack      # build without creating the package
yarn desktop electron . --user-data-dir="$HOME/greenlight-test-profile"
```

- Development: `yarn desktop dev --electron-options="--user-data-dir=$HOME/greenlight-test-profile"`
  (use a path without spaces).
- **Use a test profile**: without `--user-data-dir` the settings are the ones of the installed app.
  A new folder is the same as a first run. Sign-in in development and in production are separate.
- `yarn desktop build` (without `--no-pack`) creates the `.dmg` in `packages/desktop/dist`; it has not been
  verified with this build.
- There are no automated tests: `yarn desktop lint` and `yarn desktop build --no-pack` are the minimum.

## Known macOS limitations (not fixed)

- **Xbox button**: macOS captures it (it opens the Games app) and, with the controller shortcuts disabled,
  it does not reach the app. Pressing **View + Menu** together works as the Xbox button.
- **Vibration over a USB-C cable**: the controller works but does not rumble (`playEffect()` returns
  `not-supported` on macOS). Over Bluetooth it rumbles, but weakly: the console sends low values and the
  controller exposes only two motors.
- **Two controllers**: the second one is announced to the console as controller 2 and only plays in titles
  that support a second player.
- The "Fixes" of #1720 for #1704 covers the controller not being detected in 2.4.2; the 2.4.1 case on
  macOS 27 may be a separate problem.

## Releases

Releases are built by GitHub Actions on demand, not automatically:

```bash
gh workflow run build-iu5gal.yml --repo iu5gal/greenlight --ref iu5gal-build
```

It builds macOS, Linux and Windows and publishes a release named `v<version>-build-iu5gal.<YYYYMMDD>`
(for example `v2.4.2-build-iu5gal.20261009`) with the same files as the official ones, apart from the
Flatpak. If that day's release already exists it does nothing. The builds are unsigned: on macOS use
right-click → Open the first time. The app checks the releases of this fork for updates.

## Keeping the build up to date

```bash
git switch iu5gal-build
git merge --no-ff origin/<new-branch>   # every new improvement
git fetch upstream && git merge upstream/main-v2   # now and then, to follow the original
git push
```

Prefer the command line over the GitHub website: merges involving file renames (Spanish) conflict on the web.
Never open a pull request to upstream **from** this branch.

## License and disclaimer

Greenlight is free software (see [LICENSE](LICENSE)). *Greenlight is not affiliated with Microsoft, Xbox or
Moonlight. All rights and trademarks are property of their respective owners.*
