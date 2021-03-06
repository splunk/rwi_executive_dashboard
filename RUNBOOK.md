# Remote Work Insights - Executive Dashboard: Runbook

## Table of Contents

* [Introduction](#introduction)
    * [Example of Remote Work Insights Data Collection](#example-of-remote-work-insights-data-collection)
* [Upgrade RWI - Executive Dashboard from v1.0.x to v.1.1.x](#upgrade-rwi---executive-dashboard-from-v10x-to-v11x)
    * [Important Note](#important-note)
    * [Navigation Menu](#navigation-menu)
* [Checklist](#checklist)
    * [Splunk Applications](#splunk-applications)
    * [Splunk Infrastructure](#splunk-infrastructure)
    * [Permissions Requirements](#permissions-requirements)
* [Runbook Summary](#runbook-summary)
    * [Install Splunk Apps](#install-splunk-apps)
    * [Create Splunk Indexes](#create-splunk-indexes)
    * [Configure Data Models](#configure-data-models)
    * [Configure Data Collections](#configure-data-collections)
    * [Create Zoom Webhook (Zoom only)](#create-zoom-webhook-zoom-only)
    * [Configure Remote Work Insights - Executive Dashboard](#configure-remote-work-insights---executive-dashboard)
* [Zoom Walkthrough](#zoom-walkthrough)
    * [Configure Splunk Connect for Zoom](#configure-splunk-connect-for-zoom)
    * [Create Zoom Webhook Only App](#create-zoom-webhook-only-app)
* [Configure the Remote Work Insights - Executive Dashboard](#configure-the-remote-work-insights---executive-dashboard)
    * [Version 1.0.x](#version-10x)
    * [Version 1.1.x](#version-11x)
* [Additional Resources](#additional-resources)
    * [Splunk Docs](#splunk-docs)
    * [Splunk Connect for Zoom](#splunk-connect-for-zoom)
    * [Zoom References](#zoom-references)
* [Appendix](#appendix)
    * [Zoom Webhook Data Flow Diagram](#zoom-webhook-data-flow-diagram)
    * [Okta Data Flow Diagram](#okta-data-flow-diagram)
* [Remote Work Insights - Executive Dashboard](#remote-work-insights---executive-dashboard)
    * [Remote Work Insights Executive Home Dashboard](#remote-work-insights-executive-home-dashboard)
    * [VPN Ops Dashboard](#vpn-ops-dashboard)
    * [Zoom Ops Dashboard](#zoom-ops-dashboard)
    * [Authentication Ops Dashboard](#authentication-ops-dashboard)

# Introduction
The purpose of the **Remote Work Insights - Executive Dashboard** is to provide the ability to aggregate information across VPN, authentication, and video conferencing services to provide insights into the connectivity, productivity, and engagement across a remote workforce. An example dashboard that synthesizes information across these services is illustrated below:

![remote work app dashboard](media/remoteWorkExecDashboard.png)

**Dashboard Reference**: [RWI - Executive Dashboard (rw_exec.xml)](default/data/ui/views/rw_exec.xml)

The first row provides real-time information on the number of workers connected via VPN, real-time number of active Zoom video conferencing meetings, and the top application accessed via Okta for the current day. The second row looks at aggregate daily statistics over time for these same mission-critical indicators: number of VPN logins, number of Zoom meetings and average duration, and top 10 apps accessed via Okta. The bottom of the panel shows VPN connectivity counts by geographic location.

This document provides step by step instructions to install and configure your own **Remote Work Insights - Executive Dashboard**. It will allow you to dynamically create dashboards similar to the image above for a specific set of service providers: Palo Alto Network’s GlobalProtect VPN information, Okta authentication services, and Zoom video conferencing services. The instructions begin by highlighting a visual depiction of the data sources by service, a checklist of necessary Splunk Add-ons (commonly known as TAs) that must be installed, a runbook to ensure the proper Splunk Add-ons are correctly in place and finally a summary of steps required to start sending Zoom data to Splunk. 

## Example of Remote Work Insights Data Collection

![data collection flow](media/data_collection_flow.png)

# Upgrade RWI - Executive Dashboard from v1.0.x to v.1.1.x
1. Backup any custom dashboards or reports that were created while on v1.0.x
2. Backup the navigation bar (default.xml), only if you have modified the application menu bar
3. Download and install the latest version of the [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/apps/id/Splunk_SA_rwi-executive-dashboard) from Splunkbase
    * *Installing on Splunk Cloud*: Installing on Splunk Cloud through Self-Service Apps Install requires Splunk Cloud version 7.1.x or later.
    * To install on *Splunk Cloud version 7.0.x or earlier*, submit a case to Splunk Support. See Contact Splunk Support for contact information and how to submit a case.
4. Download and install the latest version of the [RWI - Executive Dashboard](https://splunkbase.splunk.com/app/4952)
    * *Installing on Splunk Cloud*: Installing on Splunk Cloud through Self-Service Apps Install requires Splunk Cloud version 7.1.x or later.
    * To install on *Splunk Cloud version 7.0.x or earlier*, submit a case to Splunk Support. See Contact Splunk Support for contact information and how to submit a case.
5. Restart the *Splunk Search Head* or Initiate a *Search Head Cluster* Rolling Restart
6. Access the RWI - Executive Dashboard app v1.1.x+ for the first time using a Splunk Admin account and proceed with the Guided Setup
    * App Prerequisites check
    * Indexes macros configuration
    * Data collections check
    * Features/Navigation bar configuration
7. Restore any custom dashboards or reports from Step 1
8. ** **See [Important Note](#important-note) Before Proceeding** **
9. Merge any custom navigation menu from Step 2 with the default navigation bar. 

## Important Note

### New App Prequisites for 1.1.x+
- [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/app/5063)
- This Splunk Add-on provides support functions to the RWI - Executive Dashboard v1.1 and above as to the Video Conferencing data model, field search-time extractions for the views, reports provided in the main App.

### Navigation Menu
- If you have modified the Navigation Bar, you may still use the **Guided Setup**. Though, the menu ordering of the navigation bar may changes.
- If you are upgrading from v1.0.x to v1.1.x, you may access the Guided Setup using this link: `http(s)://<your_splunk_hostname>:<port>/en-US/app/rwi_executive_dashboard/guided_setup`

# Checklist
This section provides you the prerequisites to successfully install the **Remote Work Insights - Executive Dashboard**.

## Splunk Applications
Download the following apps from [Splunkbase](https://splunkbase.splunk.com) and deploy them according to your Splunk Environment. For more information on how to deploy Splunk apps and addons refer to the [App Deployment Overview](https://docs.splunk.com/Documentation/Splunk/latest/Admin/Deployappsandadd-ons).

* Splunk
  * [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/app/5063)
  * [Splunk Common Information Model (CIM) Add-on](https://splunkbase.splunk.com/app/1621/)
* Palo Alto Networks 
  * [Palo Alto Networks Add-on for Splunk](https://splunkbase.splunk.com/app/2757/)
  * [Palo Alto Networks App for Splunk](https://splunkbase.splunk.com/app/491/)
* Okta 
  * [Okta Identity Cloud Add-on for Splunk](https://splunkbase.splunk.com/app/3682/)
* Zoom
  * [Splunk Connect for Zoom](https://splunkbase.splunk.com/app/4961)

## Splunk Infrastructure
* Standalone Splunk Instance
  * Any Splunk Enterprise or Splunk Cloud version 7.3 or higher
    * For more information about which Splunk Deployment is right for you see [About Splunk Enterprise deployments](https://docs.splunk.com/Documentation/Splunk/8.0.2/Overview/AboutSplunkEnterprisedeployments)

**OR**
* Distributed Splunk Deployment + Splunk Heavy Forwarder (HF)
  * Any full version Splunk Enterprise version 7.3 or higher with a HF that will act as an independent forwarding agent for your Zoom and/or Okta data source
  * Network and OS Firewall whitelist permissions

**OR**
* Splunk Cloud Environment with an Input Data Manager (IDM) instance
  * Splunk Cloud version 7.3 or higher with an IDM that will act as an independent forwarding agent for your Okta data source

**AND**
* Syslog server for Palo Alto TA
  * For more information on how to configure syslog
    * [Splunk Connect for Syslog](https://splunk-connect-for-syslog.readthedocs.io/en/master/gettingstarted/)
      * [Runbook for Redhat 8](https://splunk-connect-for-syslog.readthedocs.io/en/master/gettingstarted/podman-systemd-general/)

## Permissions Requirements
* Splunk Environment
  * Splunk admin account with ability to install/configure apps and create indexes
  * Splunk CLI (Command Line) access (only required for the Splunk Connect for Zoom)
  * HTTP Event Collector (HEC) Token used by Splunk Connect for Syslog
* Zoom Environment
  * Zoom administrator or developer account
  * Zoom permissions to create and activate a Zoom App
  * Network and OS Firewall whitelist permissions
  * (Optional) Signed Trusted CA SSL Certificate and Private Key

# Runbook Summary
In this runbook, you need to complete the following items:

## Install Splunk Apps
* Splunk Search Head
  * Remote Work Insights - Executive Dashboard
  * [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/app/5063)
  * [Splunk Common Information Model (CIM) Add-on](https://splunkbase.splunk.com/app/1621/)
  * Palo Alto Networks
    * [Palo Alto Networks Add-on for Splunk](https://splunkbase.splunk.com/app/2757/)
    * [Palo Alto Networks App for Splunk](https://splunkbase.splunk.com/app/491/)
  * Okta
    * [Splunk Add-on for Okta](https://splunkbase.splunk.com/app/2806/)
* Splunk Heavy Forwarder
  * Okta
    * [Okta Identity Cloud Add-on for Splunk](https://splunkbase.splunk.com/app/3682/)
  * Zoom
    * [Splunk Connect for Zoom](https://splunkbase.splunk.com/app/4961)
* Syslog
    * [Splunk Connect for Syslog](https://splunkbase.splunk.com/app/4740)

## Create Splunk Indexes
* Palo Alto Networks
  * `index=pan`
* Okta
  * `index=okta`
* Zoom
  * `index=zoom`

## Configure Data Models
* Configure the Splunk Common Information Model (CIM) Data Models
  * Update the [CIM Index Constraints](https://docs.splunk.com/Documentation/CIM/latest/User/Setup#Set_index_constraints) for `Authentication`, `Network Sessions`, `Network Traffic` data models.
* Update Palo Alto Networks Firewall Logs Data Model Schema
  * Prefix `index=pan` in the base search
* Enabled Data Model Acceleration (DMA) (Optional)
  * Palo Alto Networks Add-on for Splunk
    * Palo Alto Networks Firewall Logs

## Configure Data Collections
* Okta
  * Configure Okta Identity Cloud Add-on for Splunk and collecting Okta events
* Zoom
  * Configure the [Splunk Connect for Zoom](https://splunkbase.splunk.com/app/4961) to accept incoming webhooks from Zoom

## Create Zoom Webhook (Zoom only)
* Create Zoom Webhook Only App
* Enable Webhook event subscriptions
* Activate Zoom App

# Configure Remote Work Insights - Executive Dashboard

## Configure Indexes Macros
* Configure the indexes macros to allow the Dashboards to work as per your environment.

| Category | Macro | Definition |
| --- | --- | --- |
| Authentication | rw_auth_indexes | (index=okta) |
| Video Conferencing | rw_vc_indexes | (index=zoom) |
| VPN | rw_vpn_indexes | (index=pan) |

## Configure the CIM Index Constraints
* Follow the [CIM Index Constraints](https://docs.splunk.com/Documentation/CIM/latest/User/Setup#Set_index_constraints) documentation to update the Index Constraints for the `Authentication`, `Network Sessions` and `Network Traffic` CIM data models.

| Category | CIM Data Model | Indexes to add |
| --- | --- | --- |
| Authentication | Authentication | `okta`, `pan` |
| VPN | Network Sessions | `pan` |
| VPN | Network Traffic | `pan` |

# Zoom Walkthrough

## Configure Splunk Connect for Zoom
This section is only applicable to Zoom Data Collection.

* Use the [Splunk Connect for Zoom](https://splunkbase.splunk.com/app/4961) to accept incoming webhooks from Zoom
* Please visit the: [Splunk Connect for Zoom user documentation](https://docs.splunk.com/Documentation/ZoomConnect) to assist you with the installation and configuration.

## Create Zoom Webhook Only App
For this section, you may follow Zoom's documentation: [Create a Webhook-Only App](https://marketplace.zoom.us/docs/guides/getting-started/app-types/create-webhook-only-app)
* Go to: [https://marketplace.zoom.us/](https://marketplace.zoom.us) and login
* On the top right corner, click ***Develop > Build App***
* ***Create*** a Webhook Only App
* **Fill** the App Information and click ***Continue***
  * App Name
  * Short Description
  * Company Name
  * Developer Name
  * Developer Email Address
* Enable ***Event Subscriptions***
* Click on ***Add new event subscription*** button
* **Provide** the following information
  * Subscription Name (eg: Splunk)
  * Event notification endpoint URL (eg: https://example.com:4443)
* Click on ***Add events*** button
* **Subscribe** to any Webhook Events you wish.
  * For more details, please visit the [Zoom Webhook Reference page](https://marketplace.zoom.us/docs/api-reference/webhook-reference).
* Click ***Save***
* Click ***Continue***
* ***Activate*** your newly created Webhook Only App

# Configure the **Remote Work Insights - Executive Dashboard**
## Version 1.0.x
* From the Splunk Search Head, go to the ***RWI - Executive Dashboard*** App

![](media/remote_work_icon.png)

* Go to ***Settings > Advanced Search > Search Macros*** to update the Index Macros

![](media/advanced_search.png)

* Update the following indexes macros

| Category | Macro | Definition |
| --- | --- | --- |
| Authentication | rw_auth_indexes | (index=okta) |
| Video Conferencing | rw_vc_indexes | (index=zoom) |
| VPN | rw_vpn_indexes | (index=pan) |

![](media/search_macros.png)

## Version 1.1.x+
* From the Splunk Search Head, go to the ***RWI - Executive Dashboard*** App
* You should be prompted with the Guided Setup if running version 1.1.x+ for the first time
* The wizard will assist you with the app prerequisites, index macros configuration, incoming data check and navigation bar setup
* The Indexes Macros were moved to the [Splunk Add-on for RWI - Executive Dashboard](https://splunkbase.splunk.com/apps/id/Splunk_SA_rwi-executive-dashboard). You may still configure the Indexes Macros outside of the *Guided Setup* as in the previous section for [Version 1.0.x](#version-10x)

# Additional Resources

## Splunk Docs
* [App deployment overview](https://docs.splunk.com/Documentation/Splunk/latest/Admin/Deployappsandadd-ons)
* [Install an add-on in a single-instance Splunk Enterprise deployment](http://docs.splunk.com/Documentation/AddOns/released/Overview/Singleserverinstall)
* [About securing Splunk Enterprise with SSL](https://docs.splunk.com/Documentation/Splunk/latest/Security/AboutsecuringyourSplunkconfigurationwithSSL)
* [How to get certificates signed by a third-party](https://docs.splunk.com/Documentation/Splunk/latest/Security/Howtogetthird-partycertificates)
* [Splunkbase](https://splunkbase.splunk.com/)
* [Splunk Connect for Syslog](https://splunk-connect-for-syslog.readthedocs.io/en/master/gettingstarted/)
* [Splunk Connect for Syslog - Runbook for redhat 8](https://splunk-connect-for-syslog.readthedocs.io/en/master/gettingstarted/podman-systemd-general/)
* [Splunk CIM Manual](https://docs.splunk.com/Documentation/CIM/Latest/User/Overview)
* [Splunk CIM Index Constraints](https://docs.splunk.com/Documentation/CIM/latest/User/Setup#Set_index_constraints)

## Splunk Connect for Zoom
* [Splunkbase: Splunk Connect for Zoom](https://splunkbase.splunk.com/app/4961)
* [Splunk Connect for Zoom user documentation](https://docs.splunk.com/Documentation/ZoomConnect)

## Zoom References
* [Zoom Create a Webhook-only App](https://marketplace.zoom.us/docs/guides/getting-started/app-types/create-webhook-only-app)
* [Zoom Network Firewall or Proxy Server Settings](https://support.zoom.us/hc/en-us/articles/201362683-Network-Firewall-or-Proxy-Server-Settings-for-Zoom)
* [Zoom Marketplace](https://marketplace.zoom.us/)
* [Zoom Webhook Logs](https://marketplace.zoom.us/user/logs?type=WebhookOnly)
* [Zoom Webhook Documentation](https://marketplace.zoom.us/docs/guides/tools-resources/webhooks)
* [Zoom Developer Forum](https://devforum.zoom.us/)

# Appendix

## Zoom Webhook Data Flow Diagram
![](media/zoom_workflow.png)

## Okta Data Flow Diagram
![](media/okta_dataflow.png)

# Remote Work Insights - Executive Dashboard
## Remote Work Insights Executive Home Dashboard
![](media/remoteWorkExecDashboard.png)

**Dashboard Reference**: [rw_exec.xml](default/data/ui/views/rw_exec.xml)

The first row of the **Remote Work Insights - Executive Dashboard** provides real-time information on the number of workers connected via VPN, real-time number of active Zoom meetings, and the top application accessed via Okta for the current day. The second row enables us to look at aggregate daily statistics over time for these same mission-critical indicators: number of VPN logins, number of Zoom meetings and average duration, and top 10 apps accessed via Okta. The bottom of the panel shows VPN connectivity counts by geographic location. Sudden drops during working hours may indicate connectivity issues.

The combination of VPN, authentication, and video conferencing services will provide insight into the following questions for a remote workforce:
* Is our remote workforce connected?
* Are they able to stay productive and run the business? 
* Are they engaging with each other? 

## VPN Ops Dashboard
![](media/vpn_ops_dashboard.png)

**Dashboard Reference**: [rw_vpn_ops.xml](default/data/ui/views/rw_vpn_ops.xml)

The top panel of the VPN Ops Dashboard shows successful and failed login attempts by location. The middle sequence of pie charts provides more specific information by country and city, as well as an overall indicator of successful and failed login attempts. The bottom row provides a time history of login attempts and insight into the number of unique users logging in to the network, and also a more granular view of users by regions.

## Zoom Ops Dashboard
![](media/zoom_ops_dashboard.png)

**Dashboard Reference**: [rw_vc_zoom_ops.xml](default/data/ui/views/rw_vc_zoom_ops.xml)

The top row of the Zoom Ops dashboard displays real time Zoom statistics: number of current active video conferencing sessions, number of active participants, duration of the longest ongoing meeting, average meeting length, and shortest meeting in the last 1 hour. The middle row shows the number of meetings over time by hour and whether meetings are completed in the scheduled amount of time or run over to provide insight into the distribution of activity over the course of a day. The bottom row shows the number of meetings by type and also indicates the distribution of devices that were used to join Zoom.

## Authentication Ops Dashboard
![](media/auth_ops_dashboard.png)

**Dashboard Reference**: [rw_auth_ops.xml](default/data/ui/views/rw_auth_ops.xml)

The top row of the Authentication Ops dashboard provides real time authentication information for applications accessed via Okta: the success rate and the number of authentication attempts over the last hour. The middle row provides these same metrics over the past seven days, and indicates the reasons for failure. The bottom panel indicates the authentication success rate by application.
