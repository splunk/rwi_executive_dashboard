# Changelog

* Project: **Remote Work Insights - Executive Dashboard**
* All notable changes to this project will be documented in this file.
* The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1] - 2020-06-15
- Included a **Guided Setup** to assist with the App configuration. This new feature is dependant on the [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/apps/id/Splunk_SA_rwi-executive-dashboard).
  - App Prerequisites check
  - Indexes macros configuration
  - Data collections check
  - Features/Navigation bar configuration
- Updated the **Video Conferencing SPL searches** to leverage the Video Conferencing Data Model from the [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/apps/id/Splunk_SA_rwi-executive-dashboard).
- **Video Conferencing Ops Dashboard** to support the Cisco Webex Meetings, Microsoft Teams and Zoom data collected by the following connector:
  - [Cisco WebEx Meetings Add-on for Splunk](https://splunkbase.splunk.com/app/4991)
  - [Microsoft Teams Add-on for Splunk](https://splunkbase.splunk.com/app/4994)
  - [Splunk Connect for Zoom](https://splunkbase.splunk.com/app/4961/)
- App icon updated

## [1.0.4] - 2020-04-15
- Bump version to 1.0.4 to follow Splunkbase versioning

## [1.0.2] - 2020-04-10

### Added
- Exporting tags.conf globally

### Changed
- Zoom Meetings/Participants/Duration SPL searches updated

## [1.0.1] - 2020-04-01

### Changed
- v1.0.1 Bugfix
- Zoom Active Meetings SPL updated
- Zoom SPL references to `source=` updated to `event=`

## [1.0.0] - 2020-03-31
- v1.0.0 Release

## [1.0.0-alpha.2] - 2020-03-29

### Added
- `props.conf`: New search-time fields
    - Okta [`OktaIM2:log`]: signature
    - PAN [`pan:system`]: action, src
- `eventtypes.conf` 
	- PAN: New eventtypes for Global Protect to Map to Network Sessions and Authentication Data Models

### Changed
- Moved VPN Ops Dashboard to Global Protect VPN Login Activities
- VPN Ops Dashboard Updated to use CIM Network Sessions Data Model
- Authentication Ops Dashboard to use CIM Authentication Data Model
- New Project Name: `Remote Work Insights - Executive Dashboard`

## [1.0.0-alpha.1] - 2020-03-27

### Added

- Splunk `Remote Work Insights (RWI) - Executive Dashboard` Package
- Navigation Menu
- Dashboards
    - RWI - Executive Dashboard
    - VPN Ops
    - Okta Application Activities
    - Authentication Ops
    - Zoom Ops
- Lookup Table
    - Zoom Meeting Type (ID): `zoom_meeting_type.csv`
- Indexes macros
    - VPN indexes: `rw_vpn_indexes`
    - Authentication indexes: `rw_auth_indexes`
    - Video Conferencing indexes: `rw_vc_indexes`
- README.md
- CHANGELOG.md
- RUNBOOK.md
