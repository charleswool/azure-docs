---
title: Release notes for Microsoft Defender for Cloud
description: A description of what's new and changed in Microsoft Defender for Cloud
ms.topic: reference
ms.date: 08/14/2022
---

# What's new in Microsoft Defender for Cloud?

Defender for Cloud is in active development and receives improvements on an ongoing basis. To stay up to date with the most recent developments, this page provides you with information about new features, bug fixes, and deprecated functionality.

This page is updated frequently, so revisit it often.

To learn about *planned* changes that are coming soon to Defender for Cloud, see [Important upcoming changes to Microsoft Defender for Cloud](upcoming-changes.md).

> [!TIP]
> If you're looking for items older than six months, you'll find them in the [Archive for What's new in Microsoft Defender for Cloud](release-notes-archive.md).

## August 2022

Updates in August include:

- [Auto-deployment of Azure Monitor Agent (Preview)](#auto-deployment-of-azure-monitor-agent-preview)

### Auto-deployment of Azure Monitor Agent (Preview)

The [Azure Monitor Agent](../azure-monitor/agents/agents-overview.md) (AMA) collects monitoring data from the guest operating system of Azure and hybrid virtual machines and delivers it to Azure Monitor for use by features, insights, and other services, such as Microsoft Sentinel and Microsoft Defender for Cloud.

The [Azure Monitor Agent is now integrated](enable-data-collection.md?tabs=autoprovision-ama#tabpanel_1_autoprovision-ama) into Microsoft Defender for Cloud. You can [auto-provision Azure Monitor Agent](auto-deploy-azure-monitoring-agent.md) to all of your cloud and on-premises servers with Defender for Cloud. Also, Defender for Cloud protections can use data collected by the Azure Monitor Agent.

## July 2022

Updates in July include:

- [General availability (GA) of the Cloud-native security agent for Kubernetes runtime protection](#general-availability-ga-of-the-cloud-native-security-agent-for-kubernetes-runtime-protection)
- [Defender for Container's VA adds support for the detection of language specific packages (Preview)](#defender-for-containers-va-adds-support-for-the-detection-of-language-specific-packages-preview)
- [Protect against the Operations Management Infrastructure vulnerability CVE-2022-29149](#protect-against-the-operations-management-infrastructure-vulnerability-cve-2022-29149)
- [Integration with Entra Permissions Management](#integration-with-entra-permissions-management)
- [Key Vault recommendations changed to "audit"](#key-vault-recommendations-changed-to-audit)
- [Deprecate API App policies for App Service](#deprecate-api-app-policies-for-app-service)

### General availability (GA) of the Cloud-native security agent for Kubernetes runtime protection

We're excited to share that the Cloud-native security agent for Kubernetes runtime protection is now generally available (GA)!

The production deployments of Kubernetes clusters continue to grow as customers continue to containerize their applications. To assist with this growth, the Defender for Containers team has developed a cloud-native Kubernetes oriented security agent.

The new security agent is a Kubernetes DaemonSet, based on eBPF technology and is fully integrated into AKS clusters as part of the AKS Security Profile.

The security agent enablement is available through auto-provisioning, recommendations flow, AKS RP or at scale using Azure Policy.

You can [deploy the Defender profile](./defender-for-containers-enable.md?pivots=defender-for-container-aks&tabs=aks-deploy-portal%2ck8s-deploy-asc%2ck8s-verify-asc%2ck8s-remove-arc%2caks-removeprofile-api#deploy-the-defender-profile) today on your AKS clusters.

With this announcement, the runtime protection - threat detection (workload) is now also generally available.

Learn more about the Defender for Container's [feature availability](supported-machines-endpoint-solutions-clouds-containers.md).

You can also review [all available alerts](alerts-reference.md#alerts-k8scluster).

Note, if you're using the preview version, the `AKS-AzureDefender` feature flag is no longer required.

### Defender for Container's VA adds support for the detection of language specific packages (Preview)

Defender for Container's vulnerability assessment (VA) is able to detect vulnerabilities in OS packages deployed via the OS package manager. We have now extended VA's abilities to detect vulnerabilities included in language specific packages.

This feature is in `preview` and is only available for Linux images.

To see all of the included language specific packages that have been added, check out Defender for Container's full list of [features and their availability](supported-machines-endpoint-solutions-clouds-containers.md#registries-and-images).

### Protect against the Operations Management Infrastructure vulnerability CVE-2022-29149

Operations Management Infrastructure (OMI) is a collection of cloud-based services for managing on-premises and cloud environments from one single place. Rather than deploying and managing on-premises resources, OMI components are entirely hosted in Azure.

Log Analytics integrated with Azure HDInsight running OMI version 13 requires a patch to remediate [CVE-2022-29149](https://nvd.nist.gov/vuln/detail/CVE-2022-29149). Review the report about this vulnerability in the [Microsoft Security Update guide](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2022-29149) for information about how to identify resources that are affected by this vulnerability and remediation steps.

If you have Defender for Servers enabled with Vulnerability Assessment, you can use [this workbook](https://github.com/Azure/Microsoft-Defender-for-Cloud/tree/main/Workbooks/OMI%20Vulnerability%20Dashboard) to identify affected resources.

### Integration with Entra Permissions Management

Defender for Cloud has integrated with [Microsoft Entra Permissions Management](../active-directory/cloud-infrastructure-entitlement-management/index.yml), a cloud infrastructure entitlement management (CIEM) solution that provides comprehensive visibility and control over permissions for any identity and any resource in Azure, AWS, and GCP.

Each Azure subscription, AWS account, and GCP project that you onboard, will now show you a view of your [Permission Creep Index (PCI)](../active-directory/cloud-infrastructure-entitlement-management/ui-dashboard.md).

Learn more about [Entra Permission Management (formerly Cloudknox)](other-threat-protections.md#entra-permission-management-formerly-cloudknox)

### Key Vault recommendations changed to "audit"

The effect for the Key Vault recommendations listed here was changed to "audit":

| Recommendation name | Recommendation ID |
| ------- | ------ |
| Validity period of certificates stored in Azure Key Vault should not exceed 12 months | fc84abc0-eee6-4758-8372-a7681965ca44 |
| Key Vault secrets should have an expiration date | 14257785-9437-97fa-11ae-898cfb24302b |
| Key Vault keys should have an expiration date | 1aabfa0d-7585-f9f5-1d92-ecb40291d9f2 |


### Deprecate API App policies for App Service

We deprecated the following policies to corresponding policies that already exist to include API apps:

| To be deprecated | Changing to |
|--|--|
|`Ensure API app has 'Client Certificates (Incoming client certificates)' set to 'On'` | `App Service apps should have 'Client Certificates (Incoming client certificates)' enabled` | 
| `Ensure that 'Python version' is the latest, if used as a part of the API app` | `App Service apps that use Python should use the latest 'Python version` |
| `CORS should not allow every resource to access your API App` | `App Service apps should not have CORS configured to allow every resource to access your apps` |
| `Managed identity should be used in your API App` | `App Service apps should use managed identity` |
| `Remote debugging should be turned off for API Apps` | `App Service apps should have remote debugging turned off` |
| `Ensure that 'PHP version' is the latest, if used as a part of the API app` | `App Service apps that use PHP should use the latest 'PHP version'`|
| `FTPS only should be required in your API App` | `App Service apps should require FTPS only` |
| `Ensure that 'Java version' is the latest, if used as a part of the API app` | `App Service apps that use Java should use the latest 'Java version` |
| `Latest TLS version should be used in your API App` | `App Service apps should use the latest TLS version` |

## June 2022

Updates in June include:

- [General availability (GA) for Microsoft Defender for Azure Cosmos DB](#general-availability-ga-for-microsoft-defender-for-azure-cosmos-db)
- [General availability (GA) of Defender for SQL on machines for AWS and GCP environments](#general-availability-ga-of-defender-for-sql-on-machines-for-aws-and-gcp-environments)
- [Drive implementation of security recommendations to enhance your security posture](#drive-implementation-of-security-recommendations-to-enhance-your-security-posture)
- [Filter security alerts by IP address](#filter-security-alerts-by-ip-address)
- [Alerts by resource group](#alerts-by-resource-group)
- [Auto-provisioning of Microsoft Defender for Endpoint unified solution](#auto-provisioning-of-microsoft-defender-for-endpoint-unified-solution)
- [Deprecating the "API App should only be accessible over HTTPS" policy](#deprecating-the-api-app-should-only-be-accessible-over-https-policy)
- [New Key Vault alerts](#new-key-vault-alerts)

### General availability (GA) for Microsoft Defender for Azure Cosmos DB

Microsoft Defender for Azure Cosmos DB is now generally available (GA) and supports SQL (core) API account types. 

This new release to GA is a part of the Microsoft Defender for Cloud database protection suite, which includes different types of SQL databases, and MariaDB. Microsoft Defender for Azure Cosmos DB is an Azure native layer of security that detects attempts to exploit databases in your Azure Cosmos DB accounts.

By enabling this plan, you'll be alerted to potential SQL injections, known bad actors, suspicious access patterns, and potential explorations of your database through compromised identities, or malicious insiders.  

When potentially malicious activities are detected, security alerts are generated. These alerts provide details of suspicious activity along with the relevant investigation steps, remediation actions, and security recommendations.

Microsoft Defender for Azure Cosmos DB continuously analyzes the telemetry stream generated by the Azure Cosmos DB services and crosses them with Microsoft Threat Intelligence and behavioral models to detect any suspicious activity. Defender for Azure Cosmos DB doesn't access the Azure Cosmos DB account data and doesn't have any effect on your database's performance.

Learn more about [Microsoft Defender for Azure Cosmos DB](concept-defender-for-cosmos.md).

With the addition of support for Azure Cosmos DB, Defender for Cloud now provides one of the most comprehensive workload protection offerings for cloud-based databases. Security teams and database owners can now have a centralized experience to manage their database security of their environments. 

Learn how to [enable protections](enable-enhanced-security.md) for your databases.

### General availability (GA) of Defender for SQL on machines for AWS and GCP environments

The database protection capabilities provided by Microsoft Defender for Cloud, has added support for your SQL servers that are hosted in either AWS or GCP environments.

Defender for SQL, enterprises can now protect their entire database estate, hosted in Azure, AWS, GCP and on-premises machines.

Microsoft Defender for SQL provides a unified multicloud experience to view security recommendations, security alerts and vulnerability assessment findings for both the SQL server and the underlining Windows OS.

Using the multicloud onboarding experience, you can enable and enforce databases protection for SQL servers running on AWS EC2, RDS Custom for SQL Server and GCP compute engine. Once you've enabled either of these plans, all supported resources that exist within the subscription are protected. Future resources created on the same subscription will also be protected.

Learn how to protect and connect your [AWS environment](quickstart-onboard-aws.md) and your [GCP organization](quickstart-onboard-gcp.md) with Microsoft Defender for Cloud.

### Drive implementation of security recommendations to enhance your security posture

Today's increasing threats to organizations stretch the limits of security personnel to protect their expanding workloads. Security teams are challenged to implement the protections defined in their security policies.

Now with the governance experience, security teams can assign remediation of security recommendations to the resource owners and require a remediation schedule. They can have full transparency into the progress of the remediation and get notified when tasks are overdue.

This feature is free while it is in the preview phase.

Learn more about the governance experience in [Driving your organization to remediate security issues with recommendation governance](governance-rules.md).

### Filter security alerts by IP address

In many cases of attacks, you want to track alerts based on the IP address of the entity involved in the attack. Up until now, the IP appeared only in the "Related Entities" section in the single alert pane. Now, you can filter the alerts in the security alerts page to see the alerts related to the IP address, and you can search for a specific IP address.

:::image type="content" source="media/release-notes/ip-address-filter-for-alerts.png" alt-text="Screenshot of filter for I P address in Defender for Cloud alerts." lightbox="media/release-notes/ip-address-filter-for-alerts.png":::

### Alerts by resource group

The ability to filter, sort and group by resource group has been added to the Security alerts page. 

A resource group column has been added to the alerts grid.

:::image type="content" source="media/release-notes/resource-column.png" alt-text="Screenshot of the newly added resource group column." lightbox="media/release-notes/resource-column.png":::

A new filter has been added which allows you to view all of the alerts for specific resource groups.

:::image type="content" source="media/release-notes/filter-by-resource-group.png" alt-text="Screenshot that shows the new resource group filter." lightbox="media/release-notes/filter-by-resource-group.png":::

You can now also group your alerts by resource group to view all of your alerts for each of your resource groups. 

:::image type="content" source="media/release-notes/group-by-resource.png" alt-text="Screenshot that shows how to view your alerts when they're grouped by resource group." lightbox="media/release-notes/group-by-resource.png":::

### Auto-provisioning of Microsoft Defender for Endpoint unified solution

Until now, the integration with Microsoft Defender for Endpoint (MDE) included automatic installation of the new [MDE unified solution](/microsoft-365/security/defender-endpoint/configure-server-endpoints?view=o365-worldwide#new-windows-server-2012-r2-and-2016-functionality-in-the-modern-unified-solution&preserve-view=true) for machines (Azure subscriptions and multicloud connectors) with Defender for Servers Plan 1 enabled, and for multicloud connectors with Defender for Servers Plan 2 enabled. Plan 2 for Azure subscriptions enabled the unified solution for Linux machines and Windows 2019 and 2022 servers only. Windows servers 2012R2 and 2016 used the MDE legacy solution dependent on Log Analytics agent.

Now, the new unified solution is available for all machines in both plans, for both Azure subscriptions and multi-cloud connectors. For Azure subscriptions with Servers Plan 2 that enabled MDE integration *after* June 20th 2022, the unified solution is enabled by default for all machines Azure subscriptions with the Defender for Servers Plan 2 enabled with MDE integration *before* June 20th 2022 can now enable unified solution installation for Windows servers 2012R2 and 2016 through the dedicated button in the Integrations page:

:::image type="content" source="media/integration-defender-for-endpoint/enable-unified-solution.png" alt-text="The integration between Microsoft Defender for Cloud and Microsoft's EDR solution, Microsoft Defender for Endpoint, is enabled." lightbox="media/integration-defender-for-endpoint/enable-unified-solution.png":::

Learn more about [MDE integration with Defender for Servers](integration-defender-for-endpoint.md#users-with-defender-for-servers-enabled-and-microsoft-defender-for-endpoint-deployed).

### Deprecating the "API App should only be accessible over HTTPS" policy

The policy `API App should only be accessible over HTTPS` has been deprecated. This policy is replaced with the `Web Application should only be accessible over HTTPS` policy, which has been renamed to `App Service apps should only be accessible over HTTPS`.

To learn more about policy definitions for Azure App Service, see [Azure Policy built-in definitions for Azure App Service](../azure-app-configuration/policy-reference.md).

### New Key Vault alerts

To expand the threat protections provided by Microsoft Defender for Key Vault, we've added two new alerts.

These alerts inform you of an access denied anomaly, is detected for any of your key vaults.

| Alert (alert type) | Description | MITRE tactics | Severity |
|--|--|--|--|
| **Unusual access denied - User accessing high volume of key vaults denied**<br>(KV_DeniedAccountVolumeAnomaly) | A user or service principal has attempted access to anomalously high volume of key vaults in the last 24 hours. This anomalous access pattern may be legitimate activity. Though this attempt was unsuccessful, it could be an indication of a possible attempt to gain access of key vault and the secrets contained within it. We recommend further investigations. | Discovery | Low |
| **Unusual access denied - Unusual user accessing key vault denied**<br>(KV_UserAccessDeniedAnomaly) | A key vault access was attempted by a user that doesn't normally access it, this anomalous access pattern may be legitimate activity. Though this attempt was unsuccessful, it could be an indication of a possible attempt to gain access of key vault and the secrets contained within it.  | Initial Access, Discovery | Low |

## May 2022

Updates in May include:

- [Multicloud settings of Servers plan are now available in connector level](#multicloud-settings-of-servers-plan-are-now-available-in-connector-level)
- [JIT (Just-in-time) access for VMs is now available for AWS EC2 instances (Preview)](#jit-just-in-time-access-for-vms-is-now-available-for-aws-ec2-instances-preview)
- [Add and remove the Defender profile for AKS clusters using the CLI](#add-and-remove-the-defender-profile-for-aks-clusters-using-the-cli)

### Multicloud settings of Servers plan are now available in connector level

There are now connector-level settings for Defender for Servers in multicloud.

The new connector-level settings provide granularity for pricing and auto-provisioning configuration per connector, independently of the subscription.

All auto-provisioning components available in the connector level (Azure Arc, MDE, and vulnerability assessments) are enabled by default, and the new configuration supports both [Plan 1 and Plan 2 pricing tiers](defender-for-servers-introduction.md#defender-for-servers-plans).

Updates in the UI include a reflection of the selected pricing tier and the required components configured.

:::image type="content" source="media/release-notes/main-page.png" alt-text="Screenshot of the main plan page with the Server plan multicloud settings." lightbox="media/release-notes/main-page.png":::

:::image type="content" source="media/release-notes/auto-provision.png" alt-text="Screenshot of the auto-provision page with the multicloud connector enabled.":::

### Changes to vulnerability assessment

Defender for Containers now displays vulnerabilities that have medium and low severities that aren't patchable.

As part of this update, vulnerabilities that have medium and low severities are now shown, whether or not patches are available. This update provides maximum visibility, but still allows you to filter out undesired vulnerabilities by using the provided Disable rule.

:::image type="content" source="media/release-notes/disable-rule.png" alt-text="Screenshot of the disable rule screen.":::

Learn more about [vulnerability management](deploy-vulnerability-assessment-tvm.md)

### JIT (Just-in-time) access for VMs is now available for AWS EC2 instances (Preview)

When you [connect AWS accounts](quickstart-onboard-aws.md), JIT will automatically evaluate the network configuration of your instance's security groups and recommend which instances need protection for their exposed management ports. This is similar to how JIT works with Azure. When you onboard unprotected EC2 instances, JIT will block public access to the management ports, and only open them with authorized requests for a limited time frame.

Learn how [JIT protects your AWS EC2 instances](just-in-time-access-overview.md#how-jit-operates-with-network-resources-in-azure-and-aws)

### Add and remove the Defender profile for AKS clusters using the CLI

The Defender profile (preview) is required for Defender for Containers to provide the runtime protections and collects signals from nodes. You can now use the Azure CLI to [add and remove the Defender profile](defender-for-containers-enable.md?tabs=k8s-deploy-cli%2Ck8s-deploy-asc%2Ck8s-verify-asc%2Ck8s-remove-arc%2Ck8s-remove-cli&pivots=defender-for-container-aks#use-azure-cli-to-deploy-the-defender-extension) for an AKS cluster.

> [!NOTE]
> This option is included in [Azure CLI 3.7 and above](/cli/azure/update-azure-cli).

## April 2022

Updates in April include:

- [New Defender for Servers plans](#new-defender-for-servers-plans)
- [Relocation of custom recommendations](#relocation-of-custom-recommendations)
- [PowerShell script to stream alerts to Splunk and QRadar](#powershell-script-to-stream-alerts-to-splunk-and-ibm-qradar)
- [Deprecated the Azure Cache for Redis recommendation](#deprecated-the-azure-cache-for-redis-recommendation)
- [New alert variant for Microsoft Defender for Storage (preview) to detect exposure of sensitive data](#new-alert-variant-for-microsoft-defender-for-storage-preview-to-detect-exposure-of-sensitive-data)
- [Container scan alert title augmented with IP address reputation](#container-scan-alert-title-augmented-with-ip-address-reputation)
- [See the activity logs that relate to a security alert](#see-the-activity-logs-that-relate-to-a-security-alert)

### New Defender for Servers plans

Microsoft Defender for Servers is now offered in two incremental plans:

- Defender for Servers Plan 2, formerly Defender for Servers
- Defender for Servers Plan 1, provides support for Microsoft Defender for Endpoint only

While Defender for Servers Plan 2 continues to provide protections from threats and vulnerabilities to your cloud and on-premises workloads, Defender for Servers Plan 1 provides endpoint protection only, powered by the natively integrated Defender for Endpoint. Read more about the [Defender for Servers plans](defender-for-servers-introduction.md#defender-for-servers-plans).

If you have been using Defender for Servers until now no action is required.

In addition, Defender for Cloud also begins gradual support for the [Defender for Endpoint unified agent for Windows Server 2012 R2 and 2016](https://techcommunity.microsoft.com/t5/microsoft-defender-for-endpoint/defending-windows-server-2012-r2-and-2016/ba-p/2783292). Defender for Servers Plan 1 deploys the new unified agent to Windows Server 2012 R2 and 2016 workloads.

### Relocation of custom recommendations

Custom recommendations are those created by users and have no effect on the secure score. The custom recommendations can now be found under the All recommendations tab.

Use the new "recommendation type" filter, to locate custom recommendations.

Learn more in [Create custom security initiatives and policies](custom-security-policies.md).

### PowerShell script to stream alerts to Splunk and IBM QRadar

We recommend that you use Event Hubs and a built-in connector to export security alerts to Splunk and IBM QRadar. Now you can use a PowerShell script to set up the Azure resources needed to export security alerts for your subscription or tenant.

Just download and run the PowerShell script. After you provide a few details of your environment, the script configures the resources for you. The script then produces output that you use in the SIEM platform to complete the integration.

To learn more, see [Stream alerts to Splunk and QRadar](export-to-siem.md#stream-alerts-to-qradar-and-splunk).

### Deprecated the Azure Cache for Redis recommendation

The recommendation `Azure Cache for Redis should reside within a virtual network` (Preview) has been deprecated. We’ve changed our guidance for securing Azure Cache for Redis instances. We recommend the use of a private endpoint to restrict access to your Azure Cache for Redis instance, instead of a virtual network.

### New alert variant for Microsoft Defender for Storage (preview) to detect exposure of sensitive data

Microsoft Defender for Storage's alerts notifies you when threat actors attempt to scan and expose, successfully or not, misconfigured, publicly open storage containers to try to exfiltrate sensitive information.

To allow for faster triaging and response time, when exfiltration of potentially sensitive data may have occurred, we've released a new variation to the existing `Publicly accessible storage containers have been exposed` alert.

The new alert, `Publicly accessible storage containers with potentially sensitive data have been exposed`, is triggered with a `High` severity level, after there has been a successful discovery of a publicly open storage container(s) with names that statistically have been found to rarely be exposed publicly, suggesting they might hold sensitive information.

| Alert (alert type) | Description | MITRE tactic | Severity |
|--|--|--|--|
|**PREVIEW - Publicly accessible storage containers with potentially sensitive data have been exposed** <br>(Storage.Blob_OpenContainersScanning.SuccessfulDiscovery.Sensitive)| Someone has scanned your Azure Storage account and exposed container(s) that allow public access. One or more of the exposed containers have names that indicate that they may contain sensitive data. <br> <br> This usually indicates reconnaissance by a threat actor that is scanning for misconfigured publicly accessible storage containers that may contain sensitive data. <br> <br> After a threat actor successfully discovers a container, they may continue by exfiltrating the data. <br> ✔ Azure Blob Storage <br> ✖ Azure Files <br> ✖ Azure Data Lake Storage Gen2 | Collection  | High |

### Container scan alert title augmented with IP address reputation

An IP address's reputation can indicate whether the scanning activity originates from a known threat actor, or from an actor that is using the Tor network to hide their identity. Both of these indicators, suggest that there's malicious intent. The IP address's reputation is provided by [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684).

The addition of the IP address's reputation to the alert title provides a way to quickly evaluate the intent of the actor, and thus the severity of the threat.  

The following alerts will include this information:

- `Publicly accessible storage containers have been exposed`

- `Publicly accessible storage containers with potentially sensitive data have been exposed`

- `Publicly accessible storage containers have been scanned. No publicly accessible data was discovered`

For example, the added information to the title of the `Publicly accessible storage containers have been exposed` alert will look like this:

- `Publicly accessible storage containers have been exposed`**`by a suspicious IP address`**

- `Publicly accessible storage containers have been exposed`**`by a Tor exit node`**

All of the alerts for Microsoft Defender for Storage will continue to include threat intelligence information in the IP entity under the alert's Related Entities section.

### See the activity logs that relate to a security alert

As part of the actions you can take to [evaluate a security alert](managing-and-responding-alerts.md#respond-to-security-alerts), you can find the related platform logs in **Inspect resource context** to gain context about the affected resource.
Microsoft Defender for Cloud identifies platform logs that are within one day of the alert.

The platform logs can help you evaluate the security threat and identify steps that you can take to mitigate the identified risk.

## March 2022

Updates in March include:

- [Global availability of Secure Score for AWS and GCP environments](#global-availability-of-secure-score-for-aws-and-gcp-environments)
- [Deprecated the recommendations to install the network traffic data collection agent](#deprecated-the-recommendations-to-install-the-network-traffic-data-collection-agent)
- [Defender for Containers can now scan for vulnerabilities in Windows images (preview)](#defender-for-containers-can-now-scan-for-vulnerabilities-in-windows-images-preview)
- [New alert for Microsoft Defender for Storage (preview)](#new-alert-for-microsoft-defender-for-storage-preview)
- [Configure email notifications settings from an alert](#configure-email-notifications-settings-from-an-alert)
- [Deprecated preview alert: ARM.MCAS_ActivityFromAnonymousIPAddresses](#deprecated-preview-alert-armmcas_activityfromanonymousipaddresses)
- [Moved the recommendation Vulnerabilities in container security configurations should be remediated from the secure score to best practices](#moved-the-recommendation-vulnerabilities-in-container-security-configurations-should-be-remediated-from-the-secure-score-to-best-practices)
- [Deprecated the recommendation to use service principals to protect your subscriptions](#deprecated-the-recommendation-to-use-service-principals-to-protect-your-subscriptions)
- [Legacy implementation of ISO 27001 replaced with new ISO 27001:2013 initiative](#legacy-implementation-of-iso-27001-replaced-with-new-iso-270012013-initiative)
- [Deprecated Microsoft Defender for IoT device recommendations](#deprecated-microsoft-defender-for-iot-device-recommendations)
- [Deprecated Microsoft Defender for IoT device alerts](#deprecated-microsoft-defender-for-iot-device-alerts)
- [Posture management and threat protection for AWS and GCP released for general availability (GA)](#posture-management-and-threat-protection-for-aws-and-gcp-released-for-general-availability-ga)
- [Registry scan for Windows images in ACR added support for national clouds](#registry-scan-for-windows-images-in-acr-added-support-for-national-clouds)

### Global availability of Secure Score for AWS and GCP environments

The cloud security posture management capabilities provided by Microsoft Defender for Cloud, has now added support for your AWS and GCP environments within your Secure Score.

Enterprises can now view their overall security posture, across various environments, such as Azure, AWS and GCP.

The Secure Score page has been replaced with the Security posture dashboard. The Security posture dashboard allows you to view an overall combined score for all of your environments, or a breakdown of your security posture based on any combination of environments that you choose.

The Recommendations page has also been redesigned to provide new capabilities such as: cloud environment selection, advanced filters based on content (resource group, AWS account, GCP project and more), improved user interface on low resolution, support for open query in resource graph, and more. You can learn more about your overall [security posture](secure-score-security-controls.md) and [security recommendations](review-security-recommendations.md).

### Deprecated the recommendations to install the network traffic data collection agent

Changes in our roadmap and priorities have removed the need for the network traffic data collection agent. The following two recommendations and their related policies were deprecated.  

|Recommendation |Description |Severity |
|---|---|---|
| Network traffic data collection agent should be installed on Linux virtual machines|Defender for Cloud uses the Microsoft Dependency agent to collect network traffic data from your Azure virtual machines to enable advanced network protection features such as traffic visualization on the network map, network hardening recommendations and specific network threats. |Medium |
| Network traffic data collection agent should be installed on Windows virtual machines |Defender for Cloud uses the Microsoft Dependency agent to collect network traffic data from your Azure virtual machines to enable advanced network protection features such as traffic visualization on the network map, network hardening recommendations, and specific network threats. |Medium |

### Defender for Containers can now scan for vulnerabilities in Windows images (preview)

Defender for Container's image scan now supports Windows images that are hosted in Azure Container Registry. This feature is free while in preview, and will incur a cost when it becomes generally available.

Learn more in [Use Microsoft Defender for Container to scan your images for vulnerabilities](defender-for-containers-usage.md).

### New alert for Microsoft Defender for Storage (preview)

To expand the threat protections provided by Microsoft Defender for Storage, we've added a new preview alert.

Threat actors use applications and tools to discover and access storage accounts. Microsoft Defender for Storage detects these applications and tools so that you can block them and remediate your posture.

This preview alert is called `Access from a suspicious application`. The alert is relevant to Azure Blob Storage, and ADLS Gen2 only.

| Alert (alert type) | Description | MITRE tactic | Severity |
|--|--|--|--|
| **PREVIEW - Access from a suspicious application**<br>(Storage.Blob_SuspiciousApp) | Indicates that a suspicious application has successfully accessed a container of a storage account with authentication.<br>This might indicate that an attacker has obtained the credentials necessary to access the account, and is exploiting it. This could also be an indication of a penetration test carried out in your organization.<br>Applies to: Azure Blob Storage, Azure Data Lake Storage Gen2 | Initial Access | Medium |

### Configure email notifications settings from an alert

A new section has been added to the alert User Interface (UI) which allows you to view and edit who will receive email notifications for alerts that are triggered on the current subscription.

:::image type="content" source="media/release-notes/configure-email.png" alt-text="Screenshot of the new UI showing how to configure email notification.":::

Learn how to [Configure email notifications for security alerts](configure-email-notifications.md).

### Deprecated preview alert: ARM.MCAS_ActivityFromAnonymousIPAddresses

The following preview alert has been deprecated:

|Alert name| Description|
|----------------------|---------------------------|
|**PREVIEW - Activity from a risky IP address**<br>(ARM.MCAS_ActivityFromAnonymousIPAddresses)|Users activity from an IP address that has been identified as an anonymous proxy IP address has been detected.<br>These proxies are used by people who want to hide their device's IP address, and can be used for malicious intent. This detection uses a machine learning algorithm that reduces false positives, such as mis-tagged IP addresses that are widely used by users in the organization.<br>Requires an active Microsoft Defender for Cloud Apps license.|

A new alert has been created that provides this information and adds to it. In addition, the newer alerts (ARM_OperationFromSuspiciousIP, ARM_OperationFromSuspiciousProxyIP) don't require a license for Microsoft Defender for Cloud Apps (formerly known as Microsoft Cloud App Security).

See more alerts for [Resource Manager](alerts-reference.md#alerts-resourcemanager).

### Moved the recommendation Vulnerabilities in container security configurations should be remediated from the secure score to best practices

The recommendation `Vulnerabilities in container security configurations should be remediated` has been moved from the secure score section to best practices section.

The current user experience only provides the score when all compliance checks have passed. Most customers have difficulties with meeting all the required checks. We're working on an improved experience for this recommendation, and once released the recommendation will be moved back to the secure score.

### Deprecated the recommendation to use service principals to protect your subscriptions

As organizations move away from using management certificates to manage their subscriptions, and [our recent announcement that we're retiring the Cloud Services (classic) deployment model](https://azure.microsoft.com/updates/cloud-services-retirement-announcement/), we deprecated the following Defender for Cloud recommendation and its related policy:

|Recommendation |Description |Severity |
|---|---|---|
| Service principals should be used to protect your subscriptions instead of Management Certificates | Management certificates allow anyone who authenticates with them to manage the subscription(s) they're associated with. To manage subscriptions more securely, using service principals with Resource Manager is recommended to limit the blast radius in the case of a certificate compromise. It also automates resource management. <br />(Related policy: [Service principals should be used to protect your subscriptions instead of management certificates](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6646a0bd-e110-40ca-bb97-84fcee63c414)) |Medium |

Learn more:

- [Cloud Services (classic) deployment model is retiring on 31 August 2024](https://azure.microsoft.com/updates/cloud-services-retirement-announcement/)
- [Overview of Azure Cloud Services (classic)](../cloud-services/cloud-services-choose-me.md)
- [Workflow of Microsoft Azure classic VM Architecture - including RDFE workflow basics](../cloud-services/cloud-services-workflow-process.md)

### Legacy implementation of ISO 27001 replaced with new ISO 27001:2013 initiative

The legacy implementation of ISO 27001 has been removed from Defender for Cloud's regulatory compliance dashboard. If you're tracking your ISO 27001 compliance with Defender for Cloud, onboard the new ISO 27001:2013 standard for all relevant management groups or subscriptions.

:::image type="content" source="media/upcoming-changes/removing-iso-27001-legacy-implementation.png" alt-text="Defender for Cloud's regulatory compliance dashboard showing the message about the removal of the legacy implementation of ISO 27001." lightbox="media/upcoming-changes/removing-iso-27001-legacy-implementation.png":::

### Deprecated Microsoft Defender for IoT device recommendations

Microsoft Defender for IoT device recommendations is no longer visible in Microsoft Defender for Cloud. These recommendations are still available on Microsoft Defender for IoT's Recommendations page.

The following recommendations are deprecated:

| Assessment key | Recommendations |
|--|--|
| 1a36f14a-8bd8-45f5-abe5-eef88d76ab5b: IoT Devices | Open Ports On Device |
| ba975338-f956-41e7-a9f2-7614832d382d: IoT Devices | Permissive firewall rule in the input chain was found |
| beb62be3-5e78-49bd-ac5f-099250ef3c7c: IoT Devices | Permissive firewall policy in one of the chains was found |
| d5a8d84a-9ad0-42e2-80e0-d38e3d46028a: IoT Devices | Permissive firewall rule in the output chain was found |
| 5f65e47f-7a00-4bf3-acae-90ee441ee876: IoT Devices | Operating system baseline validation failure |
|a9a59ebb-5d6f-42f5-92a1-036fd0fd1879: IoT Devices | Agent sending underutilized messages |
| 2acc27c6-5fdb-405e-9080-cb66b850c8f5: IoT Devices | TLS cipher suite upgrade needed |
|d74d2738-2485-4103-9919-69c7e63776ec: IoT Devices | Auditd process stopped sending events |

### Deprecated Microsoft Defender for IoT device alerts

All of Microsoft's Defender for IoT device alerts are no longer visible in Microsoft Defender for Cloud. These alerts are still available on Microsoft Defender for IoT's Alert page, and in Microsoft Sentinel.

### Posture management and threat protection for AWS and GCP released for general availability (GA)

- **Defender for Cloud's CSPM features** extend to your AWS and GCP resources. This agentless plan assesses your multi cloud resources according to cloud-specific security recommendations that are included in your secure score. The resources are assessed for compliance using the built-in standards. Defender for Cloud's asset inventory page is a multicloud enabled feature that allows you to manage your AWS resources alongside your Azure resources.

- **Microsoft Defender for Servers** brings threat detection and advanced defenses to your compute instances in AWS and GCP. The Defender for Servers plan includes an integrated license for Microsoft Defender for Endpoint, vulnerability assessment scanning, and more. Learn about all of the [supported features for virtual machines and servers](supported-machines-endpoint-solutions-clouds-servers.md). Automatic onboarding capabilities allow you to easily connect any existing or new compute instances discovered in your environment.

Learn how to protect and connect your [AWS environment](quickstart-onboard-aws.md) and [GCP organization](quickstart-onboard-gcp.md) with Microsoft Defender for Cloud.

### Registry scan for Windows images in ACR added support for national clouds

Registry scan for Windows images is now supported in Azure Government and Azure China 21Vianet. This addition is currently in preview.

Learn more about our [feature's availability](supported-machines-endpoint-solutions-clouds-containers.md).
