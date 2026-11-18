# Changelog

## 0.2.1

- Use the `ai4devGcloudMonitor` command and configuration namespace while reading existing explicit legacy settings until the optional, backed-up migration is requested.
- Route mirrored status notifications through one acknowledged focused editor window, with the system notification channel retaining ownership otherwise.
- Run notification-button actions in the background and report combined action/status errors without opening terminals or changing authentication automatically.
- Keep the package experimental with `preview` enabled.

## 0.2.0

- Renamed the published identity to `nitronicolasoft.ai4dev-monitor`: `name` is `ai4dev-monitor`, `displayName` is `AI4Dev Monitor`, and the project folder is `vs-code-extensions/home-made/ai4dev-monitor/`. The former published ids were `nitronicolasoft.a4d-watcher` and, before that, `nicolas-web-local.ai4dev-tunnel`; a renamed id is a different extension id with no in-place upgrade, so `scripts/install.ps1` now refuses with `OLD_EXTENSION_INSTALLED` while either former id is still listed on any host.
- User-visible labels follow the new name: the Command Palette entry, the Settings page title, the status-bar item and the Output channel now read `AI4Dev Monitor`. Command ids and configuration keys keep the `ai4devTunnel.` prefix, so existing user settings and keybindings stay valid.

## Unreleased

- Add distribution guidance and a verified local installer for Windows and Remote WSL extension hosts.
- Publish the extension as `nitronicolasoft.a4d-watcher` (`publisher` `nitronicolasoft`, readable identity `Nitro Nicola SOFT`), the renamed identity of `nicolas-web-local.ai4dev-tunnel`, and move the project to `vs-code-extensions/home-made/a4d-watcher/`. The id change is visible in `package.json`; the extension id is what VS Code matches, so the renamed package does not replace the old one: `scripts/install.ps1` now checks for `nicolas-web-local.ai4dev-tunnel` on the Windows profile and every Remote WSL store and refuses with `OLD_EXTENSION_INSTALLED` until it is uninstalled. Settings keys and commands keep the existing `ai4devTunnel.` prefix.

## 0.1.2

- Keep failure notifications coherent with the status bar: a single failed status probe (state `unavailable`) no longer raises the sticky error toast; two consecutive failed probes are required before announcing, and the announcement says the status check failed instead of asserting the tunnel is not working.
- Announce recovery once with an information notification when a healthy check follows an announced failure, since an error toast cannot be retracted.

## 0.1.1

- Add the `ai4devTunnel.authenticationPromptPrefill` setting, live-synced to the tunnel installation.
- Fix tilde expansion of the install path so the default `~/agents/ai4dev/ai4dev_tunnel` resolves (bash applies tilde expansion to `${VAR#~/}` patterns, leaving a broken path).
- Correct and translate the invalid-configuration error for boolean settings.

## 0.1.0

- Add a theme-aware tunnel status bar with click-based controls and a localized action list.
- Add failure notifications with retry, authentication renewal, and log actions.
- Support Windows/WSL and Remote WSL with French, English, and Spanish runtime localization.
