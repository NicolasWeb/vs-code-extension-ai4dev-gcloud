# Help

## Status-bar controls

- Select the main status item to connect a stopped tunnel or disconnect a connected tunnel.
- Select the ellipsis to open the action list.
- Search the Command Palette for `AI4Dev Gcloud Monitor` to open the same action list.
- Hover over the main item for the current state, supervisor, local port, and last-check time.

## Failure notification

An unhealthy tunnel state triggers one VS Code notification per failure episode, offering retry, authentication renewal, and log viewing. A failed status check (state `unavailable`) is announced only after two consecutive failed probes, and its message reports the failed check instead of claiming the tunnel is down; one transient WSL or transport hiccup therefore never leaves an error toast behind a healthy status bar. A healthy status check re-arms the next notification and, when a failure had been announced, a single information notification reports that the tunnel is connected again. Authentication renewal runs in a visible terminal so interactive sign-in remains available.

## Settings

- `ai4devGcloudMonitor.installPath` points to the Linux installation containing `bashrc/ai4dev-gcloud.bashrc`.
- `ai4devGcloudMonitor.wslDistro` selects a WSL distribution. Leave it empty to derive the distribution from the current WSL workspace or use the WSL default.
- `ai4devGcloudMonitor.refreshIntervalSeconds` controls the polling interval.

Run `AI4Dev Gcloud Monitor: Migrate legacy settings` explicitly to move the six legacy `ai4devMonitor` settings to `ai4devGcloudMonitor` without changing values or scopes. A verified private backup precedes writes; conflicts fail visibly without overwriting the destination. Legacy settings remain readable until migration. Installation paths are never rewritten automatically.

## Language and themes

The display language is selected from VS Code first, then the operating-system locale. Supported languages are French (`fr-FR`), English (`en-US`), and Spanish (`es-ES`). If neither locale is supported, English is used. Status colors use standard VS Code theme tokens and monochrome Codicons.
