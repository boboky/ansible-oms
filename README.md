# Operations Management Suite 

Operations Management Suite (also known as OMS) is a collection of management services that were designed in the cloud from the start. Rather than deploying and managing on-premise resources, OMS components are entirely hosted in Azure. Configuration is minimal, and you can be up and running literally in a matter of minutes. Just because OMS services run in the cloud doesn’t mean that they can’t effectively manage your on-premise environment. Put an agent on any Windows or Linux computer in your data center, and it will send data to Log Analytics where it can be analyzed along with all other data collected from cloud or on-premise services.

## OMS services

1. Log Analytics - Monitor and analyze the availability and performance of different resources including physical and virtual machines.
2. Automation - Automate manual processes and enforce configurations for physical and virtual machines.
3. Backup - Backup and restore critical data.
4. Site  - Provide high availability for critical applications.

*We use only Log Analytics OMS services*

### Log Analytics

Provides monitoring services for OMS by collecting data from managed resources into a central repository. This data could include events, performance data, or custom data provided through the API. Once collected, the data is available for alerting, analysis, and export. This method allows you to consolidate data from a variety of sources so you can combine data from your Azure services with your existing on-premise environment. It also clearly separates the collection of the data from the action taken on that data so that all actions are available to all kinds of data.

![Log Analytice Overview](../images/overview-log-analytics.png)

## Collecting data

There are a variety of ways that you can get data into the repository for Log Analytics to analyze.

1. Windows or Linux Computers and Virtual Machines
2. Azure services
3. Data Collector API

*We use Windows or linux computers and virtual machines, Azure services*

### Windows or Linux computers and virtual machines

You install the Microsoft Monitoring Agent on Windows and Linux computers or virtual machines that you want to collect data from. The agent will automatically download from Log Analytics configuration that defines events and performance data to collect. You can easily install the agent on virtual machines running in Azure using the Azure portal. If you have an existing Operations Manager environment, you can connect the management group to Log Analytics and automatically start collecting data from all existing agents.

### Azure services

Log Analytics collects telemetry from Azure Diagnostics and Azure Monitoring into the repository so that you can monitor Azure resources.

## Reporting and analyzing data

Log Analytics includes a powerful query language to extract data stored in the repository. Since data from all sources are stored as records, you can analyze data from multiple sources in a single query.
In addition to ad hoc analysis, Log Analytics provides multiple ways to report and analyze data from a query.

1. Views and dashboards
2. Export
3. Log Search API

*We use Views and dashboards*

### Views and dashboards

Views and dashboards visualize the results of a query in the portal. Management solutions will typically include views that analyze the data from the solution. You can also create your own custom views to analyze data and make it readily available in your custom portal.

### Export

You have the option to export the results of any query so that you can analyze it outside of Log Analytics. You can even schedule a regular export to Power BI which provides significant visualization and analysis capabilities.

## Alerting

Log Analytics can proactively alert you or take corrective action when it detects an issue. Like all other analysis in Log Analytics, this is done with a log search. This search runs on a regular schedule, and an alert is created if the results match particular criteria.

![Alert Overview](../images/overview-alerts.png)

In addition to creating an alert record in the Log Analytics repository, alerts can take the following actions.

1. Email
2. Runbook
3. Webhook

*We use Email*

### Email

Send an email to proactively notify you of a detected issue.

## Management Solutions

Management Solutions are prepackaged sets of logic that implement a particular management scenario leveraging one or more OMS services. Different solutions are available from Microsoft and from partners that you can easily add to your Azure subscription to increase the value of your investment in OMS from the Solutions Gallery or Azure Marketplace.

Ref: https://azuremarketplace.microsoft.com/marketplace/
Ref: https://azure.microsoft.com/resources/templates/

Management solutions add functionality to OMS, providing additional data and analysis tools to Log Analytics. They may also define new record types to be collected that can be analyzed with Log Searches or by additional user interface provided by the solution in the dashboard.

## Using Log Analytics

You can access Log Analytics through the OMS portal or the Azure portal which run in any browser and provide you with access to configuration settings and multiple tools to analyze and act on collected data. From the portal you can leverage log searches where you construct queries to analyze collected data, dashboards which you can customize with graphical views of your most valuable searches, and solutions which provide additional functionality and analysis tools.

## Data Sources

Log Analytics collects data from the Connected Sources in your OMS workspace and stores it in OMS repository. The data that is collected from each is defined by the Data Sources that you configure. Data in the OMS repository is stored as a set of records. Each data source creates records of a particular type with each type having its own set of properties.

![Alert Overview](../images/overview-data-sources.png)

Data Sources are different than OMS Solutions which also collect data from Connected Sources and create records in the OMS repository. Solutions can be added to your workspace from the Solutions Gallery and will typically provide additional analysis tools in the OMS portal.

The data sources that are currently available in Log Analytics are listed in the following table. Each has a link to a separate article providing detail for that data source.

1. Custom logs - Text files on Windows or Linux agents containing log information.
2. Windows Event logs - Events collected from the event log on Windows computers.
3. Windows Performance counters - Performance counters collected from Windows computers.
4. Linux Performance counters - Performance counters collected from Linux computers.
5. IIS logs - Internet Information Services logs in W3C format.
6. Syslog - Syslog events on Windows or Linux computers.

*We use all the data sources, except IIS logs*

## Manage Log Analytics using Azure Resource Manager templates

You can use Azure Resource Manager templates to create and configure Log Analytics workspaces. Below are the tasks we have automated using ARM templates which can be deployed using Ansible or Azure CLI

* Deployment of Log Analystics and creation of workspace
* Add a Gallery and Custom Solutions
* Create Custom Dashboard 
* Collect performance counters from Linux and Windows computers
* Collect events from syslog on Linux computers
* Collect events from Windows event logs
* Configure OMS workspace to pull azure activity logs
* Create saved searches
* Create Alert and sechdules

*custom event logs are configured manually via OMS portal and pushed all computers*

### Requirment

Automation account need to be created with run as previlage which is required to create custom solutions. The account should be created within the same resource group as OMS workspace/Log Analystics

### Known Limitation 

OMS deployment and configuration is automated using azure resource manager templates, except Custom log Data source, which need to be configured manually, which dosnt support automation as of yet


### List of Solutions Deployed for OMS Gallary/MarketPlace

* Log Search - Provides ability to collect and analyze logs and events from all your computers, deployed by default as a part of Log analytics/OMS workspace creation
* AntiMalware Assessment - View status of antivirus and antimalware scans across your servers.
* Security and Audit - Provides the ability to explore security related data and helps identify security breaches.
* Agent Health - The Agent Health solution gives customers insight into the health, performance and availability of their Windows and Linux agents
* Azure Log Analytics - Track all create, update and delete activities occurring in your Azure subscriptions.
* Alert Management - View your Operations Manager and OMS alerts to easily triage alerts and identify the root causes of problems in your environment.
* Azure Network Security Group Analytics - Gain insight into your Azure Network Security Group logs
* Key Vault Analytics - Understand your Key Vault usage through Analysis of Key Vault logs
* Network Performance Monitoring - Offers near real time monitoring of network performance parameters like loss and latency.
* Update Management - Identify and orchestrate the installation of missing system updates.

### Custom Soltuions Deployed

* Azure Storage Analytics - Utilizes native Storage REST API capabilities and Storage Analytics to enable inventory and monitoring of storage accounts.

### Custom Dashboard

All solutions deployed form Galary come with Pre-configuired Dashboards

* Server Performance - https://gallery.technet.microsoft.com/scriptcenter/Server-Performance-3d767ab1
* Free Tier Data Consumption Tracker - https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2017/03/21/oms-free-tier-data-consumption-tracker-dashboard/

### Custom Logs

* ClamAV log - /var/log/clamd.log
* ClamAV DB log - /var/log/freshclam.log

*Need to have read permission for OMS agent user*

### Known Limitations 

AntiMalware Assessment - Works only for Windows, uses Windows Defender
Azure Network Security Group Analytics - Alerts can be performed only based on API names
Network Performance Monitoring - Works only for Windows, requires minimum of 2 machines per subnet

### Custom Alerts

Few alerts are configured by default as part of the solutions. They can extended as per the requirment. 
