# Changelog
All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
---
## [2.1.5] - 2026-05-31
### Added
- `phone_notify_service` now accepts a comma-separated list of notify services
  allowing multiple devices to be notified independently.
- Each notify target is called via `repeat/for_each` so one failing device
  does not block others.
- `continue_on_error` added to each individual phone notify call.

---
## [2.1.4] - 2026-05-31
### Added
- New `speaker_chime_duration` input to control how long to wait for the chime
  to finish before TTS plays, preventing audio overlap.
- Snapshot switch is now turned off after the snapshot is taken to allow future
  triggers to reset correctly.
- Changed `mode` from `single` to `queued` (max: 2) to handle rapid successive
  triggers without silently dropping events.
### Fixed
- Safe volume restore when Sonos/speakers are off or unavailable. Replaced
  `| default(0.35)` with an explicit `none` check on `state_attr()` result,
  as `default()` does not catch `None` returned for unavailable entities.
- Added `| float(0.35)` fallback on the volume restore action as a secondary
  safety net.
- Volume is now set before chime playback begins rather than after.

---
## [2.1.3] - 2026-05-31
### Added
- New `base_url` input (required) to support dynamic snapshot URLs across all HA setups.
- Dynamic snapshot URL generation with timestamp cache‑buster.
- Safe `continue_on_error` handling for TTS and TV overlay actions.
- Safe volume restore logic for speakers.
- README updated to document new base_url input and v2.1.3 behavior.
### Changed
- Replaced hard‑coded snapshot URL with dynamic URL built from base_url.
- Improved snapshot timing to ensure the file is fully written before notifications.
- Updated TV overlay and phone notification to use the new dynamic URL.
### Fixed
- Removed unsupported `hass` reference that caused runtime failures in blueprints.
- Eliminated stale snapshot issues on phones and TVs.
- Prevented automation crashes when speakers or TV devices are offline.

---
## [2.1.2] - 2026-05-30
### Added
- Multi-trigger support (ding, motion, person).
- Adjustable TV overlay display duration.
- Single-shot TTS to prevent double announcements.
### Changed
- Improved snapshot timing and reliability.
- Simplified variable handling.
- Improved VLAN compatibility for image delivery.
### Removed
- Deprecated PiP overlay requirement (replaced by TvOverlay app).

---
## [Unreleased]
- No changes yet.
