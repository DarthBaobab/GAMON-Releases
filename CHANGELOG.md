# Changelog

Dieses Dokument wird aus den versionierten Dateien unter `.github/changelogs/` erzeugt.

## v0.0.6

## New

- Add `start`, `stop`, and `restart` CLI commands for controlling an installed manager service, including `service start|stop|restart` variants on Windows and Linux
- Add a hidden, session-scoped Debug menu that is unlocked by clicking the GAMON logo seven times within five seconds
- Add a dedicated Debug and Diagnostics page for debug-mode configuration, automation dry-runs, accelerated live simulations, and the update-launcher self-test
- Add a configurable live debug stream with Trace, Debug, Information, Warning, and Error thresholds
- Add separate rolling user, diagnostics, error, crash, and configurable debug log files with retention limits
- Add regression coverage for simultaneous rotation/restart scheduling, simulated backup time, user-log exception filtering, debug-log thresholds, and service CLI commands

## Improved

- Separate user-facing logs from technical diagnostics so the dashboard, console, manager log, and Discord receive concise Information-or-higher messages without stack traces
- Move detailed exceptions, source contexts, SteamCMD output, system service details, and other troubleshooting data into dedicated diagnostic logs
- Review and reduce logging levels across startup, polling, synchronization, downloads, service management, update checks, and background services
- Consolidate all experimental and troubleshooting controls into the hidden Debug menu instead of exposing them across the main dashboard, system settings, and update page
- Keep the normal bottom console focused on operationally relevant messages while allowing full technical output on the Debug page
- Use manager/simulation time consistently for backup names, metadata, interval checks, and retention calculations
- Clarify backup timestamps and preserve compatibility with existing backup archives
- Reduce repeated cgroup PID logging by emitting changes only when the resolved process changes
- Log SteamCMD raw output at Trace level instead of flooding regular diagnostics
- Update service CLI help and Windows elevation guidance for manual start, stop, and restart operations
- Correct the documented Linux release archive output path

## Fixes

- Execute all scheduler events that share the same timestamp instead of dropping later events after the first one fires
- Prioritize ArkSA map rotation before a simultaneous global restart so the next rotation instance is enabled and started correctly
- Prevent repeated rotation announcements from continually naming the same next map when rotation and restart use the same schedule time
- Defer starting the newly selected rotation map to a simultaneous global restart, preventing transition-state failures
- Prevent backup interval messages from mixing real filesystem time with accelerated simulation time
- Suppress expected update-check cancellation stack traces during controlled manager shutdown
- Prevent detailed exception messages and stack traces from being exposed in dashboard and Discord logs

## v0.0.5

## New

- Add first-class Ark: Survival Ascended and Conan Exiles game modules with dedicated dashboards, instance settings, automation, scheduled-task views, and shared manager integrations
- Add configurable automatic game-update scheduling, postponing, version skipping/resuming, update notifications, and update-with-restart workflows
- Add ArkSA map rotation management with configurable daily, weekly, and monthly schedules, editable map order, announcements, pause/extend/force-swap controls, and safe disabled defaults for new rotation maps
- Add isolated multi-week automation dry-runs and accelerated live simulations for testing schedules, notifications, recovery, updates, restarts, and map rotations
- Add automatic crash recovery with configurable check interval and a maximum of 1 to 10 restart attempts per continuous crash condition (default: 3)
- Add dashboard diagnostics for configuration errors, missing prerequisites/base files, update failures, crashes, stopped instances, duplicate or conflicting ports, and service setup issues
- Add host CPU, memory, disk, and uptime metrics to the dashboard
- Add operational alerts for crashes, resource thresholds, update failures, and failed backup jobs
- Add secure Discord bot token storage, environment-variable support, admin controls, status updates, and configurable logging
- Add global host-wide port validation and automatic allocation of the next free game, peer, query, and RCON ports for new instances
- Add shared automatic backup, emergency snapshot, integrity verification, retention, restore, and dashboard workflows
- Add a first-class force-stop lifecycle action and state-aware instance controls
- Add multilingual German and English UI support
- Add release-package smoke tests, VM integration-test guidance, configuration documentation, troubleshooting guidance, and an expanded xUnit test suite

## Improved

- Replace the previous CoreRCON dependency with the pinned `gorcon/rcon-cli` integration for ArkSA and Conan Exiles; the binary is downloaded on first RCON use and verified with SHA-256
- Generalize dashboard, scheduler, update, notification, recovery, RCON, backup, and game-module integrations instead of hardcoding ArkSA behavior in shared core services
- Make dashboard and instance actions status-aware so start, stop, restart, force-stop, backup, update, and rotation actions are only available in valid states
- Detect unexpected process exits separately from intentional stops; auto-restart now applies only to confirmed crashes and never to intentionally stopped instances
- Stop automatic recovery after the configured attempt limit, emit one visible failure notification, and reset the counter only after a confirmed running state or intentional lifecycle change
- Add maintenance-mode coordination so recovery and background automation do not interfere with updates or planned restarts
- Resolve and cache the actual managed game-server PID instead of relying on the service wrapper process
- Improve periodic dashboard, instance status, metrics, log, notification, and job-queue refresh behavior
- Add log-level filtering, larger history, pagination, and improved scrolling for manager and game logs
- Improve instance ID validation and normalization and resolve the `{instanceId}` server-name variable during creation
- Reload INI configuration automatically when files change on disk
- Validate ports across all enabled game modules and prevent starts when any configured port conflicts
- Prepare WinSW/systemd service files immediately after instance creation and preserve instance-local service/runtime files during base-file synchronization
- Improve Linux support with SteamCMD runtime diagnostics, GE-Proton prefix initialization, required font/runtime checks, native Conan Exiles installation, and native systemd launch support
- Improve release publishing by explicitly publishing `GAMON.csproj`, validating package layouts, and synchronizing release notes and public documentation
- Expand automated coverage for configuration, scheduling, simulation, rotation, crash detection/recovery, updates, backups, RCON, diagnostics, ports, lifecycle policies, and multi-game registration

## Fixes

- Prevent intentionally stopped, disabled, updating, or unknown instances from being restarted by automatic recovery
- Prevent endless automatic restart loops for repeatedly crashing instances
- Correctly mark a previously running or expected process as crashed when it exits unexpectedly while preserving `Stopped` for intentional shutdowns
- Prevent premature crash detection during the process startup grace period
- Make regular ArkSA rotations perform the complete stop, disable, enable, and start transition instead of depending on a later daily restart
- Apply the configured map-rotation order consistently in the dashboard, scheduler, announcements, execution, and simulation
- Ensure newly created or newly marked ArkSA rotation maps remain disabled until rotation management activates them
- Fix instance port persistence and reject duplicate ports both within one instance and across different games
- Improve ArkSA graceful shutdown handling and fall back safely when RCON shutdown fails
- Stop Conan Exiles through its supported process termination path instead of Ark-specific `DoExit`
- Treat expected RCON startup/shutdown timeouts as warnings instead of fatal errors
- Fix Windows PID matching for WinSW descendants, access-denied process checks, and short-lived bootstrap processes
- Fix Linux manager service-state detection and several Proton prefix/environment issues
- Fix SteamCMD update failures being hidden, retry transient bootstrap configuration failures, and keep diagnostics visible for incomplete base files
- Prevent actions during starting, stopping, updating, maintenance, and other invalid transitional states
- Surface failed dashboard background jobs as visible notifications
- Preserve backup integrity metadata and verify emergency snapshots before restore
- Prevent concurrent local secret writes and deletes from removing the shared secret directory during an active write

## v0.0.4

## New

- implement job progress reporting and enhance UI feedback for background jobs

## Improved

- 

## Fixes

- Live Console should now output all log entries

