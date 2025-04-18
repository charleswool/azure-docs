---
title: Deploy the Azure Monitor Agent with auto provisioning
description: Learn how to deploy the Azure Monitor Agent on your Azure, multicloud, and on-premises servers with auto provisioning to support Microsoft Defender for Cloud protections.
author: bmansheim
ms.author: benmansheim
ms.topic: how-to
ms.date: 08/03/2022
ms.custom: template-how-to
---

# Auto provision the Azure Monitor Agent to protect your servers with Microsoft Defender for Cloud

To make sure that your server resources are secure, Microsoft Defender for Cloud uses agents installed on your servers to send information about your servers to Microsoft Defender for Cloud for analysis. You can use auto provisioning to quietly deploy the Azure Monitor Agent on your servers.

In this article, we're going to show you how to use auto provisioning to deploy the agent so that you can protect your servers.

## Availability

[!INCLUDE [azure-monitor-agent-availability](includes/azure-monitor-agent-availability.md)]

## Prerequisites

Before you enable auto provisioning, you must have the following prerequisites:

- Make sure your multicloud and on-premises machines have Azure Arc installed.
  - AWS and GCP machines
    - [Onboard your AWS connector](quickstart-onboard-aws.md) and auto provision Azure Arc.
    - [Onboard your GCP connector](quickstart-onboard-gcp.md) and auto provision Azure Arc.
  - Other clouds and on-premises machines
    - [Install Azure Arc](/azure/azure-arc/servers/learn/quick-enable-hybrid-vm).
- Make sure the Defender plans that you want the Azure Monitor Agent to support are enabled:
  - [Enable Defender for Servers Plan 2 on Azure and on-premises VMs](enable-enhanced-security.md)
  - [Enable Defender plans on the subscriptions for your AWS VMs](quickstart-onboard-aws.md)
  - [Enable Defender plans on the subscriptions for your GCP VMs](quickstart-onboard-gcp.md)

## Deploy the Azure Monitor Agent with auto provisioning

To deploy the Azure Monitor Agent with auto provisioning:

1. From Defender for Cloud's menu, open **Environment settings**.
1. Select the relevant subscription.
1. Open the **Auto provisioning** page.

    :::image type="content" source="./media/auto-deploy-azure-monitoring-agent/select-auto-provisioning.png" alt-text="Screenshot of the auto provisioning menu item for enabling the Azure Monitor Agent.":::

1. Enable deployment of the Azure Monitor Agent:

    1. For the **Log Analytics agent/Azure Monitor Agent**, select the **On** status.

        In the Configuration column, you can see the enabled agent type. When you enable auto provisioning, Defender for Cloud decides which agent to provision based on your environment. In most cases, the default is the Log Analytics agent.

        :::image type="content" source="./media/auto-deploy-azure-monitoring-agent/turn-on-azure-monitor-agent-auto-provision.png" alt-text="Screenshot of the auto provisioning page for enabling the Azure Monitor Agent." lightbox="media/auto-deploy-azure-monitoring-agent/turn-on-azure-monitor-agent-auto-provision.png":::

    1. For the **Log Analytics agent/Azure Monitor Agent**, select **Edit configuration**.

        :::image type="content" source="./media/auto-deploy-azure-monitoring-agent/configure-azure-monitor-agent-auto-provision.png " alt-text="Screenshot of editing the Azure Monitor Agent configuration." lightbox="media/auto-deploy-azure-monitoring-agent/configure-azure-monitor-agent-auto-provision.png":::

    1. For the Auto-provisioning configuration agent type, select **Azure Monitor Agent**.

        :::image type="content" source="./media/auto-deploy-azure-monitoring-agent/select-azure-monitor-agent-auto-provision.png" alt-text="Screenshot of selecting the Azure Monitor Agent." lightbox="media/auto-deploy-azure-monitoring-agent/select-azure-monitor-agent-auto-provision.png":::

    By default:

    - The Azure Monitor Agent is installed on all existing machines in the selected subscription, and on all new machines created in the subscription.
    - The Log Analytics agent isn't uninstalled from machines that already have it installed. You can [leave the Log Analytics agent](#impact-of-running-with-both-the-log-analytics-and-azure-monitor-agents) on the machine, or you can manually [remove the Log Analytics agent](/azure/azure-monitor/agents/azure-monitor-agent-migration) if you don't require it for other protections.
    - The agent sends data to the default workspace for the subscription. You can also [configure a custom workspace](#configure-custom-destination-log-analytics-workspace) to send data to.
    - You can't enable [collection of additional security events](#additional-security-events-collection).

## Impact of running with both the Log Analytics and Azure Monitor Agents

You can run both the Log Analytics and Azure Monitor Agents on the same machine, but you should be aware of these considerations:

- Certain recommendations or alerts are reported by both agents and appear twice in Defender for Cloud.
- Each machine is billed once in Defender for Cloud, but make sure you track billing of other services connected to the Log Analytics and Azure Monitor, such as the Log Analytics workspace data ingestion.
- Both agents have performance impact on the machine.

When you enable auto provisioning, Defender for Cloud decides which agent to provision. In most cases, the default is the Log Analytics agent.

Learn more about [migrating to the Azure Monitor Agent](/azure/azure-monitor/agents/azure-monitor-agent-migration).

## Custom configurations

### Configure custom destination Log Analytics workspace

When you install the Azure Monitor Agent with auto-provisioning, you can define the destination workspace of the installed extensions. By default, the destination is the “default workspace” that Defender for Cloud creates for each region in the subscription: `defaultWorkspace-<subscriptionId>-<regionShortName>`. Defender for Cloud automatically configures the data collection rules, workspace solution, and additional extensions for that workspace.

If you configure a custom Log Analytics workspace:

- Defender for Cloud only configures the data collection rules and additional extensions for the custom workspace. You'll have to configure the workspace solution on the custom workspace.
- Machines with Log Analytics agent that report to a Log Analytics workspace with the security solution are billed even when the Defender for Servers plan isn't enabled. Machines with the Azure Monitor Agent are billed only when the plan is enabled on the subscription. The security solution is still required on the workspace to work with the plans features and to be eligible for the 500-MB benefit.

To configure a custom destination workspace for the Azure Monitor Agent:

1. From Defender for Cloud's menu, open **Environment settings**.
1. Select the relevant subscription.
1. Open the **Auto provisioning** page.
1. For the **Log Analytics agent/Azure Monitor Agent**, select **Edit configuration**.
1. Select **Custom workspace**, and select the workspace that you want to send data to.

### Log analytics workspace solutions

The Azure Monitor Agent requires Log analytics workspace solutions. These solutions are automatically installed when you auto-provision the Azure Monitor Agent with the default workspace. 

The required [Log Analytics workspace solutions](/azure/azure-monitor/insights/solutions) for the data that you're collecting are:

  - Security posture management (CSPM) – **SecurityCenterFree solution**
  - Defender for Servers Plan 2 – **Security solution**

### Additional extensions for Defender for Cloud

The Azure Monitor Agent requires additional extensions. The ASA extension, which supports endpoint protection recommendations and fileless attack detection, is automatically installed when you auto-provision the Azure Monitor Agent.

### Additional security events collection

When you auto-provision the Log Analytics agent in Defender for Cloud, you can choose to collect additional security events to the workspace. When you auto-provision the Azure Monitor agent in Defender for Cloud, the option to collect additional security events to the workspace isn't available. Defender for Cloud doesn't rely on these security events, but they can be helpful for investigations through Microsoft Sentinel.

If you want to collect security events when you auto-provision the Azure Monitor Agent, you can create a [Data Collection Rule](/azure-monitor/essentials/data-collection-rule-overview) to collect the required events.

Like for Log Analytics workspaces, Defender for Cloud users are eligible for [500-MB of free data](enhanced-security-features-overview.md#faq---pricing-and-billing) daily on defined data types that include security events.

## Next steps

Now that you enabled the Azure Monitor Agent, check out the features that are supported by the agent:

- [Endpoint protection assessment](endpoint-protection-recommendations-technical.md)
- [Adaptive application controls](adaptive-application-controls.md)
- [Fileless attack detection](defender-for-servers-introduction.md#plan-features)
