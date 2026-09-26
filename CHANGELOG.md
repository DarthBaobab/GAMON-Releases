# Changelog

Dieses Dokument wird aus `changelog/WIP.md` und den versionierten Dateien unter `changelog/releases/` erzeugt.

## v1.0.0

## New

- Add `GAMON update --version VERSION` for confirmed CLI installs of a specific older or newer manager release.
- Add persistent last-player-seen tracking for instances and show the timestamp in dashboard instance tables.
- Add a standalone dry-run-first migration script for ASA-Dedi-Manager-Linux ArkSA instances, saves, configuration, and cluster data
- Add the `/wdk` Discord slash command with Ark-only autocomplete to destroy wild dinos on one or all running ArkSA instances
- Add the `/players` Discord slash command to list connected players across all instances or one selected instance
- Add the complete official ASA `ServerSettings` and `ShooterGameMode` INI catalog with documented types, defaults, descriptions, and sections
- Add confirmed manual backup deletion from the instance backup history
- Add global and instance-specific ArkSA EOS whitelists with optional inheritance and `-exclusivejoin` support
- Replace ArkSA instance INI tabs with searchable, localized schema-driven ASA settings, per-mod sections, validated manual editing, previews, and lossless saving
- Add an ArkSA CurseForge mod manager with server-side search, per-instance assignments, manual mod IDs, and release-embedded API-key support
- Add a manually triggered Alpha debug release workflow that publishes symbol-bearing debug packages for Linux and Windows
- Add editable file-based UI localization under `Localization/Original` and `Localization/Custom`, including typed `UiKeys` and dashboard validation tooling

## Improved

- Allow cluster-chat join/leave notifications to be enabled separately for in-game chat and Discord.
- Increase gorcon/rcon-cli RCON timeouts for busy game servers and group repeated endpoint timeouts before warning.
- Show ArkSA map rotation status in the dashboard with current, previous, and next map runtime details, and add dashboard notifications for scheduled game updates, successful updates, and completed map rotations.
- Add a configurable ArkSA map-rotation minimum runtime percentage so manual starts near the cycle boundary do not double the effective rotation length.
- Send scheduled restart, update, map-rotation, and WDK announcements to the Discord status channel, and announce all ArkSA WDK executions in chat before running the wipe command.
- Replace the ArkSA Daily WDK manual instance-ID field with an all-instances toggle and per-instance checkboxes, and add a per-instance Daily WDK automation switch.
- Show the offending INI line content in ArkSA manual INI editor validation errors.
- Complete English XML summaries for production code types, replace generic descriptions, remove duplicate summaries, and explain complex workflow decisions inline.
- Centralize connected-player polling in a shared presence cache used by metrics, Discord `/players`, and cluster-chat join/leave detection.
- Fall back from missing, empty, broken, or placeholder-incompatible translations through base language and English before showing the raw key
- Refresh dashboard player counts in the background and render from status snapshots so expanded dashboard panels no longer block on repeated RCON and process-status calls.
- Render the game dashboard instance panel from cached status snapshots and start it collapsed without blocking on live instance checks.
- Allow Debug Tools to reinstall the normal manager release for the current or newest version and clean up files left by expanded Alpha debug packages.
- Create new backups in the platform-default archive format (`.zip` on Windows, `.tar.gz` on Linux) while keeping list, delete, cleanup, and restore support for both formats
- Group the instance backup history by retention period and split backup timestamps into date, calendar week, time, and archive format columns
- Split dashboard disk usage into Core GAMON files, game installations, game backups, and remaining system usage with a wide stacked usage view and expandable game/instance details
- Match the compact disk usage card to the CPU and memory cards and move the detailed disk breakdown into a dedicated System storage page
- Cache dashboard snapshots for quick page switches, stretch host and instance resource metric refresh intervals, and show loading indicators during dashboard refreshes
- Align the game dashboard instance section with the global dashboard table, collapsed by default, and move automatic updates into a full-width section below it
- Rework dashboard instance rows with actions first, RAM in gigabytes, process start time and runtime, clearer host CPU labeling, and empty metrics for disabled instances
- Convert emoji and Latin special characters to readable ASCII for Ark RCON chat, broadcasts, and cluster relays while preserving Ark text in Discord mirrors
- Keep waiting for active ArkSA shutdowns beyond the initial grace period by monitoring CPU, write I/O, and save-file activity, with configurable inactivity and maximum limits
- Add game-specific RCON quick actions and a localized command reference to the instance dashboard console
- Define all ArkSA settings through the typed instance-setting catalog, localize their names and descriptions through the standard UI language files, and show the original INI or command-line name above each description
- Start, stop, and back up all game instances in parallel with a ten-second stagger while preserving per-instance status tracking and error handling
- Integrate ArkSA general server settings, instance whitelist, instance automation, and manual startup parameters into the searchable settings groups, with general settings expanded by default
- Keep undocumented optional INI values absent until configured and allow clearing them without writing empty entries
- Unify Ark instance settings into general server, automation, whitelist, and schema-driven game settings; build startup parameters from catalog values plus a dedicated manual field
- Show and highlight each instance setting's default value, support configurable numeric step sizes, omit command-line parameters at their defaults, support value-less boolean presence switches, and organize Ark settings by target and group with consistent IDs
- Show the current browser-local date, time, time-zone ID, and UTC offset in the top app bar, with warnings when browser and server time differ
- Add an instance-specific ArkSA mod dialog with CurseForge search, ordered priorities, enable/disable controls, deletion, platform and pricing filters, logos, project links, and ratings
- Use per-instance ArkSA mod directories, remove legacy shared mod-directory links automatically, and reset the local mod directory before each server start
- Read ArkSA crossplay compatibility and premium pricing directly from CurseForge mod metadata
- Consolidate app, Discord, secrets, and game-wide configuration under `Config/`, with one subdirectory per game and automatic migration from legacy locations
- Allow ArkSA Game.ini and GameUserSettings.ini defaults to be edited under Instance Defaults and reused for new instances
- Inject the CurseForge API key from a GitHub Actions repository secret into alpha and stable release builds
- Restore ArkSA general, network, and startup settings for existing instances; include schema-based Game/GUS settings during creation and stage validated INI changes under `_gamon_Config` until the next server start
- Group ArkSA INI editors by every section, including `ServerSettings`, and move full-file editing into a validated save/cancel dialog
- Add whitelist search, sortable columns, and per-entry activation controls
- Show instance backup history beside settings, RCON, and logs in the existing tab bar
- Support `{user}` and `{instanceId}` variables in cluster-chat join and leave messages
- Group Discord status entries by game and show each instance with a status emoji and player count
- Publish Discord slash command results in the configured admin channel instead of showing them only to the invoking user
- Standardize repository line endings to prevent files from appearing modified only because of EOL conversion
- Show localized weekdays and ISO calendar weeks in backup history timestamps
- Cover tiered backup retention and manual deletion with automated tests
- Limit automatic backup cleanup to once every 24 hours per instance using a persistent UTC marker
- Run Alpha release builds only when manually triggered
- Allow Debug Tools to find and install Alpha debug packages for the current manager version or newer
- Update GitHub Actions checkout and .NET setup actions to Node 24-compatible major versions

## Fixes

- Warn about ongoing RCON timeouts only after two minutes, repeat at most every two minutes, reset after successful responses, and identify the affected instance instead of its IP address.
- Keep temporary systemd status-query failures from crashing the player-presence background service.
- Delay immediate automatic game updates with announcements by two minutes after the longest warning window so the first warning is sent.
- Refresh dashboard instance memory metrics when an instance transitions to running or stopped.
- Prevent the dashboard from freezing when an instance is enabled or disabled by avoiding async-over-sync configuration saves in instance setters.
- Suppress stale cluster-chat leave notifications after game-server restart RCON gaps by resetting per-instance join/leave baselines when player presence becomes unavailable.
- Stop cluster-chat join/leave detection from treating stale player snapshots with RCON errors as current player data after scheduled restarts.
- Prevent ArkSA schema-driven settings from crashing when known numeric INI values are empty or malformed, and show the field validation error instead.
- Warn in the ArkSA INI editor when deprecated `ActiveMods` entries are present so they can be removed in favor of the mod manager or `ModIds`.
- Send an ArkSA `serverchat` fallback after each `broadcast` RCON command while filtering server-authored chat from cluster relay loops.
- Keep ArkSA weekly map rotations from being skipped when the scheduler recalculates shortly after the exact rotation time.
- Prevent corrupted or interleaved manager log lines by opening Serilog file sinks for shared access, avoiding runtime logger rebuilds for Discord logging, and suppressing service-mode console logging.
- Break the startup DI cycle between Discord logging, player presence, monitoring, and game handlers with an independent player-presence snapshot store.
- Preserve section boundaries when manually editing ArkSA Game.ini and GameUserSettings.ini sections, including Windows CRLF files.
- Clean up expanded Alpha debug package leftovers, including Linux native runtime files like `lib*.so` and `createdump`, on normal single-file startup and after standard manager updates.
- Publish Alpha debug packages as expanded app-host deployments so Visual Studio can load .NET debug services during SSH remote attach.
- Scale the dashboard and storage detail disk bars against total drive capacity so free space remains visible.
- Keep the storage detail disk bar full-width when wrapped by its tooltip.
- Stop reporting ArkSA daily task cancellation as an unhandled background-service exception during shutdown.
- Keep Discord warning/error log entries buffered until the bot is ready and send large batches across multiple flushes instead of dropping entries
- Limit dashboard loading indicators to initial page loads and reduce expensive work during instance panel expand/collapse rendering
- Suppress CA1873 logging analyzer warnings during MSBuild compilation
- Align game handler constructor cancellation tokens with .NET parameter ordering conventions
- Suppress design-time P/Invoke analyzer noise for the local secret store DPAPI calls
- Delay RCON commands for 30 seconds after a game server reports startup completion, avoiding authentication timeouts while RCON is still initializing
- Normalize whitespace in ArkSA mod-ID lists during ASA-Dedi-Manager-Linux migration so GAMON recognizes every migrated mod
- Stop showing dashboard diagnostics for intentionally stopped enabled instances or service environment files that GAMON generates automatically on start
- Keep the ArkSA map and URL startup argument visibly enclosed in quotes while leaving dash-prefixed arguments unquoted
- Store ArkSA cluster data under `_cluster`, migrate the legacy `clusters` directory, and exclude all underscore-prefixed game directories from instance discovery
- Build the ArkSA map URL as one argument followed only by dash-prefixed flags, preserve manual `-NoHangDetection`, remove conflicting port variants, and pass Linux arguments through a lossless systemd array
- Show readable German and English labels instead of raw INI keys for official ArkSA settings without hand-written translations
- Remove the legacy Ark `NoHangDetection` default, manage Crossplay through the command-line catalog, and stop persisting the derived peer port
- Display update checks, notifications, schedules, and live-console timestamps in the browser's local time zone
- Delete game-server instances cleanly when Wine/Proton prefixes contain symbolic links, including dangling links
- Keep instance-specific ArkSA mod lists synchronized with active ModIds assigned through the global manager
- Pass the actual ArkSA ModIds and saved mod configuration into the instance mod dialog instead of literal field names
- Pass ArkSA create/default modes correctly to Game.ini and GameUserSettings.ini editors and materialize missing default files
- Initialize new ArkSA instances with schema-based Game.ini and GameUserSettings.ini defaults instead of empty files
- Prevent mouse-wheel scrolling over manual ArkSA mod-ID fields from changing the entered ID

## v0.1.0

## New

- Add Stable and Alpha manager update channels to the dashboard
- Publish private Alpha builds for Linux and Windows automatically after each commit to the main branch
- Support locally stored or environment-provided GitHub tokens for private Alpha release downloads

## Improved

- Compare manager versions using semantic version precedence, including prerelease identifiers
- Consider public stable releases together with private Alpha releases so a stable version supersedes its prereleases
- Maintain unreleased notes in a visible, version-independent WIP changelog instead of requiring a versioned file before tagging
- Archive the WIP changelog under the actual stable tag and reset it automatically after a successful release
- Require repository-aware AI assistants to update the WIP changelog alongside user-visible changes

## Fixes

- Prevent differently formatted or older prerelease versions from being treated as updates solely because their version strings differ
- Read MinVer's plain-text version output correctly when resolving automated Alpha release versions

## v0.0.7

## Fixes

- Defer starting a newly selected ArkSA rotation map to a simultaneous global restart, preventing transition-state failures

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

