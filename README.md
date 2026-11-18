# AI4Dev Gcloud Monitor

`AI4Dev Gcloud Monitor` adds a compact VS Code status-bar monitor and controls for the existing Linux tunnel installation. It runs locally on Windows through WSL or directly in a Remote WSL extension host.

## Use

- The main status-bar item shows connection state, supervisor, local port, and last-check time in its tooltip. Click it to connect or disconnect.
- The adjacent ellipsis opens the localized action list. The same list is available through the `AI4Dev Gcloud Monitor` Command Palette entry.
- When tunnel trouble is detected, VS Code shows one notification per failure episode with retry, authentication renewal, and log actions. A check failure (state `unavailable`, e.g. a transient WSL outage) must be confirmed by two consecutive failed probes before it is announced, and its message says the status check failed rather than asserting the tunnel is down. A single failed probe only recolors the status bar. When a healthy check follows an announced failure, one information notification reports the recovery.
- Notification buttons perform their work in the background without opening a terminal, success dialog or log pane. If the action and its follow-up status check fail, one new error notification combines their safe error descriptions. Button-based renewal uses the bounded cached-credential refresh (`ai4dev-gcloud auth-renew --non-interactive`); when sign-in is required it fails visibly rather than opening an interactive login.
- Mirrored tunnel-status/authentication announcements belong to one acknowledged, focused VS Code window; otherwise the OS notification channel owns them. Before acquiring ownership, the monitor closes both existing OS channels; either close failing prevents acquisition. An existing owner does not repeat those native close calls on every poll. The configured installation stores private expiring per-window leases, refreshed on status/focus changes and released on deactivation. The lease lifetime is three refresh intervals plus the command timeout, capped at one day. Old installations without the acknowledgment mechanism retain OS presentation while the monitor still renders its status bar. Arbitrary manual notifications and per-click errors are not suppressed by this status lease.
- Authentication renewal explicitly selected from the action menu or Command Palette keeps the visible terminal flow, including its authentication prompt-prefill preference. This manual flow is distinct from a notification-button click.

## Configuration

- `ai4devGcloudMonitor.installPath`: Linux path to the existing `ai4dev-gcloud` installation; defaults to `~/repositories/nicolas/ai4dev/ai4dev-gcloud/ai4dev-gcloud-cli`. An explicitly configured path remains authoritative and is not rewritten automatically.
- `ai4devGcloudMonitor.authenticationPromptPrefill`: whether an outstanding authentication alert pre-fills `ai4dev_auth_renew` in the next interactive shell prompt; defaults to `true`, matching the tunnel default. Changing the setting synchronizes it through `ai4dev_prefill`; the extension also reapplies it before opening authentication renewal. Merely activating the extension does not overwrite an existing tunnel preference.
- `ai4devGcloudMonitor.wslDistro`: optional WSL distribution name. When empty, the extension derives it from a WSL workspace or uses the default distribution.
- `ai4devGcloudMonitor.refreshIntervalSeconds`: status polling interval in seconds; defaults to `5` and cannot be less than `1`.
- `ai4devGcloudMonitor.commandTimeoutSeconds`: maximum duration of a tunnel command; defaults to `60` seconds.
- `ai4devGcloudMonitor.logLines`: number of recent log lines shown by the extension; defaults to `50`.

The extension reuses `bashrc/ai4dev-gcloud.bashrc` and its tunnel commands. The status check verifies the configured supervisor and local listening port; it does not call the remote application.

Command, setting and status-item identifiers use `ai4devGcloudMonitor`. Existing explicit `ai4devMonitor` settings remain readable at their original scope until the explicit `AI4Dev Gcloud Monitor: Migrate legacy settings` command is run. Startup does not migrate settings or rewrite installation paths.

Migration covers only the six documented settings, preserving user, workspace and each workspace-folder scope. Conflicting destination values or language overrides refuse before writes. An extension-private settings backup is persisted and read back before copying; every destination is verified before legacy keys are removed. Changed sources, failed updates or readback errors stop with an explicit failure and retain the backup for recovery; this is optimistic concurrency protection, not an atomic multi-file transaction. Repeating a completed migration changes nothing. No editor reload, extension installation, authentication or service action is performed by migration.

## Language and display

The status bar uses monochrome Codicons. Status colors use VS Code theme tokens, so they adapt to light and dark themes. Runtime labels and notifications support `fr-FR`, `en-US`, and `es-ES`; VS Code's display language takes priority, followed by the operating-system locale, then `en-US`.

The status tooltip also shows the configured service window when enabled. Its days, times, and time zone come from `AI4DEV_GCLOUD_SERVICE_WINDOW_*` in the tunnel configuration and are informational only; they never gate tunnel operations. When supported by the installed tunnel, the tooltip also reports whether authentication prompt prefill is currently enabled.

Diagnostics and explicitly requested tunnel logs are shown in the local `AI4Dev Gcloud Monitor` Output channel. The extension does not collect telemetry or persist credentials.

## Distribution

The shared production, packaging, Marketplace, and installation process for both extensions is documented in [VS Code Extensions - Producing process Best practices](https://github.com/NicolasWeb/ai-agents-md/blob/main/vs-code-extensions/home-made/VS%20Code%20Extensions%20-%20Producing%20process%20Best%20practices.md).

## Published identity and migration (added on 2026-11-04)

- This extension is published as `nitronicolasoft.ai4dev-gcloud-monitor`; the former published ids, most recent first, are `nitronicolasoft.ai4dev-monitor`, `nitronicolasoft.a4d-watcher` and `nicolas-web-local.ai4dev-tunnel`. The AI4Dev repository mounts this extension at `ai4dev-gcloud/ai4dev-gcloud-vs-code-extension/`; the extensions repository mounts it at `home-made/ai4dev-monitor/`.
- A renamed identity is, for VS Code, a different extension: there is no in-place upgrade path from the old id.
- A selected profile that still holds `nitronicolasoft.ai4dev-monitor`, `nitronicolasoft.a4d-watcher` or `nicolas-web-local.ai4dev-tunnel` must uninstall those former ids before this package is installed there; otherwise several copies stay active and the old ones stop receiving updates. A temporary verification profile is separate from the habitual profile and must not remove its extensions.
- `scripts/install.ps1` checks that precondition against `code --list-extensions` on both hosts and refuses with `OLD_EXTENSION_INSTALLED` while the old extension is still listed.
