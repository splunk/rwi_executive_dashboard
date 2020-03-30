# Changelog

* Project: **Remote Work Insights - Executive Dashboard**
* Repository: https://github.com/splunk/remote_workforce
* All notable changes to this project will be documented in this file.
* The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
