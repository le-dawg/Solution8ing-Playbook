# Using Microsoft Defender for Cloud with External Integrations

Ideally our infrastructure is flawless and impenetrable, but much of our work requires us to be aware of unexpected occurrences. We rely on a system using a number of Azure resources and services such as Microsoft Defender for Cloud, Azure Monitor, and Azure Logic Apps with external integrations such as Microsoft Teams and PagerDuty to allow us to stay alert and aware of unforeseen or suspicious activity. This primer should describe our recommended current set up and allow you to set up your own system to achieve similar results. Please familiarize yourself with [Solution8's conception of Azure Enterprise-Scale Architecture](enterprise-scale-architecture.md) before proceeding.

Our intent is to have a system which notifies regarding Microsoft Defender for Cloud alerts on two integration channels (PagerDuty and Microsoft Teams), depending on the kind of alert.

## Information Flow

![Azure Alerting Information Flow](images/azure-alerting-info-flow.png)

## Microsoft Defender for Cloud Setup

Using Microsoft Defender for Cloud with Azure Management Groups allows us to designate a central subscription to "roll up" all findings for member subscriptions, aggregating findings in one place. You should enable Microsoft Defender for Cloud on all subscriptions in your management group.

We do not suggest setting this up in your root management group, as that should not be used for frequent access. Instead, set it up in whichever subscription you use to handle org-wide infrastructure (for this example, we'll call that `infrasec`).

## Azure Monitor and Logic Apps

Azure Monitor is the central service for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments. We will use Azure Monitor to create alert rules that trigger when Microsoft Defender for Cloud detects a threat.

Azure Logic Apps is a cloud service that helps you schedule, automate, and orchestrate tasks, business processes, and workflows when you need to integrate apps, data, systems, and services across enterprises or organizations. We will use Logic Apps to send notifications to Microsoft Teams and PagerDuty when an alert is triggered.

### Logic App for Microsoft Teams

You can create a Logic App that is triggered by an Azure Monitor alert and sends a message to a Microsoft Teams channel. You will need to create a Microsoft Teams incoming webhook and store the webhook URL in Azure Key Vault.

### Logic App for PagerDuty

You can create a Logic App that is triggered by an Azure Monitor alert and creates an incident in PagerDuty. You will need to get a PagerDuty integration key and store it in Azure Key Vault.

## Testing your work

Once you've done all of this set up, you'll want to test your work! You can generate sample alerts in Microsoft Defender for Cloud to test your integrations.

1. In the Azure portal, navigate to Microsoft Defender for Cloud.
1. Go to the **Security alerts** page.
1. Click on the **Sample alerts** button.
1. Select the subscription you want to create the sample alerts in and click **Create sample alerts**.

This will generate a number of sample alerts that should trigger your Logic Apps and send notifications to Microsoft Teams and PagerDuty.
