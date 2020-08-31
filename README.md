|     |     |
| --- | --- |
| **Associated Activities** | ??? |
| **Date** | 18 August 2020 |
| **Author** | Dan Moran |
| **Revision** | 1   |
| **GitHub Name and Link** | [**ut-microsoft-teams-messaging**](https://github.com/stonebranch-marketplace/ut-microsoft-teams-messaging) |

![](https://stonebranch.atlassian.net/wiki/images/icons/grey_arrow_down.png)Revision History

| **Revision** | **Date** | **Author** | **Changes** |
| --- | --- | --- | --- |
| 1   | 20200818 | Dan Moran | First draft |

* * *

Table of Contents
=================

![](https://stonebranch.atlassian.net/wiki/images/icons/grey_arrow_down.png)TOC

/\*<!\[CDATA\[\*/ div.rbtoc1598884828543 {padding: 0px;} div.rbtoc1598884828543 ul {list-style: disc;margin-left: 0px;} div.rbtoc1598884828543 li {margin-left: 0px;padding-left: 0px;} /\*\]\]>\*/

*   [Disclaimer](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-Disclaimer)
*   [Introduction](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-Introduction)
*   [Overview](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-Overview)
*   [Software Requirements](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-SoftwareRequirements)
    *   [Software Requirements for Linux Agent](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-SoftwareRequirementsforLinuxAgent)
    *   [Python Integration](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-PythonIntegration)
        *   [Check the Current Python Version](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-ChecktheCurrentPythonVersion)
        *   [Add the Required Python Modules](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-AddtheRequiredPythonModules)
    *   [Software Requirements for Windows Agent](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-SoftwareRequirementsforWindowsAgent)
*   [Incoming Webhooks in MS Teams](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-IncomingWebhooksinMSTeams)
    *   [Adding an Incoming Webhook to an MS Teams Channel](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-AddinganIncomingWebhooktoanMSTeamsChannel)
    *   [Incoming Webhooks Key Features](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-IncomingWebhooksKeyFeatures)
    *   [Feature](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-Feature)
*   [Import Microsoft Teams Notifications Forwarding Universal Template](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-ImportMicrosoftTeamsNotificationsForwardingUniversalTemplate)
*   [Configure Microsoft Teams Notifications Forwarding Universal Tasks](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-ConfigureMicrosoftTeamsNotificationsForwardingUniversalTasks)
    *   [Field Descriptions for MS Teams Notifications Forwarding](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-FieldDescriptionsforMSTeamsNotificationsForwarding)
*   [Test Cases](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-TestCases)
*   [Document References](#UniversalAutomationCenterSupportforMicrosoftTeamsNotificationsForwarding-NEW-DocumentReferences)

Disclaimer
==========

No support or warranty is provided by Stonebranch GmbH for this document and the herein described Universal Template / Universal Tasks. The use of this document and the Universal Template / Universal Tasks is at your own risk.

Before using this information in a production system, please perform extensive testing. Stonebranch GmbH assumes no liability for damage caused by the performance of the Universal Tasks.

* * *

Introduction
============

This document tells you how to use Universal Tasks for Microsoft Teams Notifications Forwarding, which allow you to send messages to an existing channel of Microsoft Teams. As a result, you can integrate this solution in Universal Automation Center to notify users for UAC result on Microsoft Teams.

If more tasks are created in the future, this document will be updated accordingly.

* * *

Overview
========

Some details about Universal Tasks for Microsoft Teams Notifications Forwarding:

*   Supports logging functionality, by selecting the log level by yourself (INFO, DEBUG, etc)
    
*   Uses pymsteams python library
    
*   Accepts as input parameters the log level, an incoming webhook a title and a text of the message
    
*   Set up the connection from UAC to MS Teams channel using webhooks
    
*   Set up the message is about to send and forwards it to the channel
    
*   No authentication is supported
    
*   This Universal Task supports both Universal Agent for Linux/Unix and Windows and has been tested in both systems:
    

* * *

Software Requirements
=====================

Software Requirements for Linux Agent
-------------------------------------

*   [Python 3.6](https://stonebranch.atlassian.net/wiki/spaces/MAR/pages/1619951628/Universal+Automation+Center+Support+for+Microsoft+Teams+Notifications+Forwarding+-+NEW#Python-Integration)
    
*   For Python, the following modules are required:
    
    *   _sys, for system-specific parameters and functions_
        
    *   _pymsteams, to interact with a Microsoft Teams channel_
        
    *   _logging, for python loglevel support_  
        _**Note: Only the module pymsteams needs to be added via python installer**_  
        _➔ pip install pymsteams_
        
*   Universal Controller V6.4.7.0 or higher
    
*   Universal Agent V6.5.0.0 or higher installed on a Linux/Windows Server
    

Python Integration
------------------

_pymsteams_ \[2\] is a Python Wrapper Library to send requests to Microsoft Teams Webhooks. Microsoft refers to these messages as Connector Cards. A message can be sent with only the main Connector Card, or additional sections can be included into the message. This library uses Webhook Connectors for Microsoft Teams.

### Check the Current Python Version

_python -V (Note: Capital “V”)_

The Linux Agent must have Python version 3.6 or later. If it has a lower version of Python, or no Python at all, either:

*   Upgrade your Python to version 3.6.
    
*   Install the Universal Agent with the Python binding option (--python yes). This option will install Python 3.6. and the agent.
    

For example:

```
sudo sh ./unvinst --network_provider oms --oms_servers 7878@192.168.88.12 --oms_port 7878 --oms_autostart no --ac_netname OPSAUTOCONF --opscli yes --python yes
```

### Add the Required Python Modules

For Python, the following modules are required:

*   _pip install pymsteams or in case of universal Agent with python binding: /opt/universal/python3.6/bin/python3 -m pip install pymsteams_  
    Only run these if not already available:
    
    *   _pip install sys_
        
    *   _pip install logging_
        

In a command shell, run as sudo or root

Software Requirements for Windows Agent
---------------------------------------

(If there are no requirements, the document should say so.)

* * *

Incoming Webhooks in MS Teams
=============================

Incoming webhooks are special type of Connectors in MS Teams that provide a simple way for an external app to share content in team channels. They are often used as tracking and notification tools.

MS Teams provides a unique URL to which you send a JSON payload with the message that you want to POST, typically in a card format. Cards are user-interface (UI) containers that contain content and actions related to a single topic and are a way to present message data in a consistent way.

MS Teams uses cards within three capabilities:

*   Bots
    
*   Messaging extensions
    
*   Connectors
    

Adding an Incoming Webhook to an MS Teams Channel
-------------------------------------------------

If your MS Team's Settings => Member permissions => Allow members to create, update, and remove connectors is selected, any team member can add, modify, or delete a connector.\[1\]

1.  Navigate to the channel where you want to add the webhook and select (•••) More Options from the top navigation bar.
    
2.  Choose Connectors from the drop-down menu and search for Incoming Webhook.
    
3.  Select the Configure button, provide a name, and, optionally, upload an image avatar for your webhook.
    
4.  The dialog window will present a unique URL that will map to the channel. Make sure that you copy and save the URL—you will need to provide it to the outside service.
    
5.  Select the Done button. The webhook will be available in the team channel.
    

Incoming Webhooks Key Features
------------------------------

| **Feature**<br>----------- | **Description** |
| --- | --- |
| Scoped Configuration | Incoming webhooks are scoped and configured at the channel level (e.g., outgoing webhooks are scoped and configured at the team level). |
| Secure resource definitions | Messages are formatted as JSON payloads. This declarative messaging structure prevents the injection of malicious code as there is no code execution on the client. |
| Actionable messaging support | If you choose to send messages via cards, you must use the actionable message card format. Actionable message cards are supported in all Office 365 groups including Teams. |
| Independent HTTPS messaging support | Cards are a great way to present information in a clear and consistent way. Any tool or framework that can send HTTPS POST requests can send messages to Teams via an incoming webhook. |
| Markdown support | All text fields in actionable messaging cards support basic Markdown. Don't use HTML markup in your cards. HTML is ignored and treated as plain text. |

* * *

Import Microsoft Teams Notifications Forwarding Universal Template
==================================================================

You must import the XML file provided to you by Stonebranch onto your Universal Controller.

This is the file containing the Universal Template (and one or more Universal Tasks) that Stonebranch exported from their Universal Controller.

1.  Log in to your Controller.
    
2.  Go to Automation Center > Tasks > All Tasks.
    
3.  Right-click the Task Name header at the top of the list and click Import.
    
4.  On the Import pop-up dialog, enter the name of the directory containing the file(s) to import, and then click OK.
    

![](https://raw.githubusercontent.com/stonebranch-marketplace/ut-microsoft-teams-messaging/master/images/image2.png)![](https://raw.githubusercontent.com/stonebranch-marketplace/ut-microsoft-teams-messaging/master/images/image3.png)

The imported Universal Template will appear on your list of Universal Templates (Configuration > Universal Templates).

Under Automation Center > Universal Tasks, a Universal Task type - based on the Universal Template - will appear.

* * *

Configure Microsoft Teams Notifications Forwarding Universal Tasks
==================================================================

For the new Universal Task type, create a new task and enter the task-specific Details that were created in the Universal Template.

![](https://raw.githubusercontent.com/stonebranch-marketplace/ut-microsoft-teams-messaging/master/images/image4.png)

* * *

Field Descriptions for MS Teams Notifications Forwarding
--------------------------------------------------------

| **Field** | **Description** |
| --- | --- |
| Agent | The Agent that runs the Python script assigned to the Universal Task |
| Logging Level | log level: DEBUG, INFO, WARNING, ERROR, CRITICAL |
| Incoming Webhook | The incoming web hook of Microsoft Teams channel |
| Message Title | The title of the message sent to Microsoft Teams channel |
| Message Text | The text of the message sent to Microsoft Teams channel |

* * *

Test Cases
==========

The following basic test cases has been performed:

| **Case#** | **Assumed behavior** | **Result** |
| --- | --- | --- |
| Set as Incoming Webhook an invalid uri | 1.  The application should stop and exit with message UAC failed to forward a message in this MS Teams channel.<br>    <br>2.  No message should be sent. | Correct |
| Set as Incoming Webhook a valid uri | 1.  A message should be sent. | Correct |

* * *

Document References
===================

This document references the following documents:

| **Ref#** | **Description** |
| --- | --- |
| \[1\] Microsoft Teams Webhooks | [https://docs.microsoft.com/enus/microsoftteams/platform/webhooks-andconnectors/how-to/add-incoming-webhook](https://docs.microsoft.com/enus/microsoftteams/platform/webhooks-andconnectors/how-to/add-incoming-webhook) |
| \[2\] pymsteams | [https://pypi.org/project/pymsteams/](https://pypi.org/project/pymsteams/) |
