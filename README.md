[**ut-sap-eventmonitor-linux**](https://github.com/stonebranch-marketplace/ut-sap-eventmonitor-linux)
=====================================================================================================

|     |     |
| --- | --- |
| **Associated Activities** |     |
| **Date** | 26 June 2018 |
| **Author** | Nils Buer |
| **Revision** | 01  |

![](https://stonebranch.atlassian.net/wiki/images/icons/grey_arrow_down.png)Revision History

| **Revision** | **Date** | **Author** | **Changes** |
| --- | --- | --- | --- |
| 00  | 20180626 | Nils Buer | Initial Document (WIP) |
| 01  | 20180628 | Nils Buer | Final Document |

* * *

### Abstract

The here described Universal Tasks Queries the SAP Event history table for the selected SAP Event & Parameter. If the Event is found, it gets confirmed, so that it is not triggered again. Optionally a task can be launched, based on the occurrence of an Event & Parameter.

* * *

### Table of Contents

![](https://stonebranch.atlassian.net/wiki/images/icons/grey_arrow_down.png)TOC

/\*<!\[CDATA\[\*/ div.rbtoc1598884596824 {padding: 0px;} div.rbtoc1598884596824 ul {list-style: disc;margin-left: 0px;} div.rbtoc1598884596824 li {margin-left: 0px;padding-left: 0px;} /\*\]\]>\*/

*   [ut-sap-eventmonitor-linux](#Documentation:SAPEventMonitoringUniversalTask(Linux)-ut-sap-eventmonitor-linux)
    *   [Abstract](#Documentation:SAPEventMonitoringUniversalTask(Linux)-Abstract)
    *   [Table of Contents](#Documentation:SAPEventMonitoringUniversalTask(Linux)-TableofContents)
*   [1 Disclaimer](#Documentation:SAPEventMonitoringUniversalTask(Linux)-1Disclaimer)
*   [2 Scope](#Documentation:SAPEventMonitoringUniversalTask(Linux)-2Scope)
*   [3 Introduction](#Documentation:SAPEventMonitoringUniversalTask(Linux)-3Introduction)
*   [4 Installation](#Documentation:SAPEventMonitoringUniversalTask(Linux)-4Installation)
    *   [4.1 Software Requirements](#Documentation:SAPEventMonitoringUniversalTask(Linux)-4.1SoftwareRequirements)
    *   [4.2 Installation Steps](#Documentation:SAPEventMonitoringUniversalTask(Linux)-4.2InstallationSteps)
*   [5 Universal Task Configuration](#Documentation:SAPEventMonitoringUniversalTask(Linux)-5UniversalTaskConfiguration)
*   [6 Universal Tasks for SAP Event monitoring](#Documentation:SAPEventMonitoringUniversalTask(Linux)-6UniversalTasksforSAPEventmonitoring)
    *   [6.1 Task Monitor Trigger mode](#Documentation:SAPEventMonitoringUniversalTask(Linux)-6.1TaskMonitorTriggermode)
*   [7 Test Cases](#Documentation:SAPEventMonitoringUniversalTask(Linux)-7TestCases)
*   [8 Document References](#Documentation:SAPEventMonitoringUniversalTask(Linux)-8DocumentReferences)

1 Disclaimer
============

No support and no warranty are provided by Stonebranch GmbH for this document and the related Universal Task. The use of this document and the related Universal Task is on your own risk.

Before using this task in a production system, please perform extensive testing.

Stonebranch GmbH assumes no liability for damage caused by the performance of the Universal Tasks

* * *

2 Scope
=======

This document provides a documentation how to install and use the Universal Tasks for SAP Event Monitoring.

* * *

3 Introduction
==============

Some details about the Universal Tasks for SAP Event Monitoring

*   The Universal Task will monitor the SAP Event history table for a certain event and related parameter.
    
*   For the Parameters wildcards “\*” are supported
    
*   The Event is retrieved by calling the USAP command: “display event\_history”
    
*   Once the Event and Parameter have been identified the Event is confirmed in SAP, so that the monitor will not trigger a second time (optional setting)
    
*   Once the Event and Parameter have been identified and confirmed the assigned task is started using the CLI command: ops-task-launch
    
*   If no task is specified, the UT runs in Monitoring mode and goes to success in case an Event has been identified
    
*   Currently only SAP connections via the sapnwrfc.ini destination file is supported. Please contact Stonebranch, if a Version for standard SAP Application Server connection is required (minimal efforts).
    
*   You can create a Task Monitor trigger out of this task by adding an action to the Universal Task, which re-started the task in case of success.
    
*   You can set different log-levels for the Universal task, providing you more information in case of issues
    
*   This Universal Task been added to the CTK-Conversion tool to map automatically Tivoli Workload Scheduler Sap Monitors called: eventProvider="SapMonitor" towards this UT.
    

|     |     |     |
| --- | --- | --- |
| **Command** | **UT Name** | **Description** |

* * *

4 Installation
==============

4.1 Software Requirements
-------------------------

**Universal Task name:** _ut\_sap\_eventmonitor\_linux_  
**Related UAC XML Files for template and task:** _Github repository_  
**Software used:**  
For the set-up you need:

1.  Universal Connector for SAP installed on a Linux Server
    
2.  An SAP destination configured in sapnwrfc.ini (/opt/universal/uagsrv)
    
3.  The Universal Task is based on a bash shell script and awk
    

4.2 Installation Steps
----------------------

The following describes the installation steps:

**1\. Import the Universal Task including the Universal Template to your Controller**

Go to “All Tasks” and load via the Import functionality the Universal Task configuration into the Controller.

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image1%20(2).png?api=v2)![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image2.png?api=v2)

* * *

5 Universal Task Configuration
==============================

**1\. Activate: Resolvable Credentials in Universal Automation Center:**

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image3.png?api=v2)

**2\. Fill Out the Universal Task for each SAP Event and Parameter to monitor:**

In the example the SAP Event: UAC\_TEST with Parameter: UAC is monitored.  
If the Event occurs in the SAP Event history table with status “NEW” the Task “Sleep 600” is launched and the Event is confirmed in SAP.

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image4.png?api=v2)

Fill out or select the required Credentials for SAP and the Universal Controller cli (oms credentials)  
Example: Universal Controller cli (oms credentials) credentials:

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image5.png?api=v2)

Example: SAP Credentials:

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image6.png?api=v2)

* * *

6 Universal Tasks for SAP Event monitoring
==========================================

The following chapter describes the provided SAP Event monitoring UT.

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image7.png?api=v2)

**Field Description:**

|     |     |     |
| --- | --- | --- |
| **Field** | **Required** | **Description** |
| Agent | Yes | Linux Universal Agent to run the USAP commands and bash shell script. |
| Agent Cluster | optional | Optional Agent Cluster for load balancing |
| Client | Yes | SAP Client to connect to e.g. 100 |
| dest | Yes | SAP destination in the file: /opt/universal/uagsrv/sapnwrfc.ini |
| Usapdir | Yes | Directory where the USAP binary is stored default is: /opt/universal/usap/bin |
| opsclidir | Yes | Directory where the CLI is stored default is: /opt/universal/opscli/bin/ |
| Event ID | Yes | Name of the Event to Scan for |
| Event Parameter | optional | Name of the parameter to scan for. Note: wildcard “_” is supported ua_ searches for all event parameters beginning with ua. If no Event Parameters is provided any Event Parameter will match |
| Event Status to Select | Yes | NEW – scan only for new Events (default) Confirmed – scan only for confirmed Events Any Status – scan for any status of Events |
| oms port | Yes | Default is 7878 |
| Taskname | optional | Name of the task to start in case a new event has been identified. If no task is specified, the UT runs in Monitoring mode and goes to success in case an Event has been identified. E.g. you can add the Event Monitor to a Workflow (Note: in that case remove the action, which automatically re-starts the UT in case of status “success”) |
| oms hostname | Yes | Hostname or IP of the OMS |
| Confirm Events or Leave status as new | Yes | Default is “confirm events”. This ensures that the same event is only triggers the event monitor once. |

**Event configuration in SAP**

The SAP Task Monitor scans for Events in the SAP Event history. An Event only shows up in the Event history if an appropriate event criteria profile has been set-up in SAP by using transaction SM62. (Note: optionally a criteria profile can also be set-up via an SAP Task of command group “Set CM Profile” in the Universal Controller). The following screen shows an example of the set-up in SAP using SM62:

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image9.png?api=v2)

**Event History table:**

The following provides and example of the Event history table in SAP (SM62).  
Only Event showing up here can trigger the SAP Event Monitor UT.

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image10.png?api=v2)

### 6.1 Task Monitor Trigger mode

You can create a Task Monitor trigger out of this task by adding an action to the Universal Task, which re-started the task in case of success.

![](https://stonebranch.atlassian.net/wiki/download/attachments/1659994117/image11.png?api=v2)

* * *

7 Test Cases
============

The following basic test cases has been performed:

|     |     |     |
| --- | --- | --- |
| **Case#** | **Assumed behaviour** | **Result** |
| **Scan for Event and Parameter**<br><br>Event: UAC\_TEST  <br>Parameter: UAC  <br>Task to Launch: Sleep 600 | #INFO# ### Monitoring for SAP EVENT:UAC\_TEST and Parameter:UAC<br><br>##INFO #EVENT:UAC\_TEST and Parameter:UAC in STATUS N found.<br><br>Task Type parameter not specified, defaulting to ALL.<br><br>Successfully launched the Timer task "Sleep 600" with task instance sys\_id 1530192249641046777S2VLE5WXSRS4Z.<br><br>opscmd-complete<br><br>##INFO# Task: Sleep 600 launched | Correct |
| **Scan for Event only**<br><br>Event: UAC\_TEST  <br>Parameter: -  <br>Task to Launch: Sleep 600 | #INFO# ### Monitoring for SAP EVENT:UAC\_TEST and Parameter:<br><br>##INFO #EVENT:UAC\_TEST and Parameter: in STATUS N found.<br><br>Task Type parameter not specified, defaulting to ALL.  <br>Successfully launched the Timer task "Sleep 600" with task instance sys\_id 153019224964120177770IUD521Q1F1I.<br><br>opscmd-complete<br><br>##INFO# Task: Sleep 600 launched | Correct |
| Scan for Event and Parameter with wildcard<br><br>Event: UAC\_TEST  <br>Parameter: U\*  <br>Task to Launch: Sleep 600 | ##INFO# ### Monitoring for SAP EVENT:UAC\_TEST and Parameter:UAC<br><br>##INFO# EVENT:UAC\_TEST and Parameter:UAC in STATUS N found.<br><br>##INFO# Task launched: Sleep\_${env}\_600 | Correct |
| **Scan for Event and Parameter with wildcard**<br><br>Event: UAC\_TEST  <br>Parameter: K\*  <br>Task to Launch: Sleep 600 | Task Monitor should stay in running mode because K\* as Parameter does not exist.<br><br>#INFO# ### Monitoring for SAP EVENT:UAC\_TEST and Parameter:K\* | Correct |
| Scan for Event and Parameter no Task “Monitoring Mode”<br><br>Event: UAC\_TEST  <br>Parameter: UAC  <br>Task to Launch: - | #INFO# ### Monitoring for SAP EVENT:UAC\_TEST and Parameter:UAC<br><br>##INFO #EVENT:UAC\_TEST and Parameter:UAC in STATUS N found.<br><br>##INFO #Monitoring Mode, No task to launch specified | Correct |
| **Scan for Event with wildcard**<br><br>Event: UAC\_\*  <br>Parameter: UAC  <br>Task to Launch: - | #INFO# ### Monitoring for SAP EVENT:UAC\_\* and Parameter:UAC<br><br>##INFO #EVENT:UAC\_\* and Parameter:UAC in STATUS N found.<br><br>##INFO #Monitoring Mode, No task to launch specified | Correct |
| Force finish the Monitor UT | Monitor has stopped and no new usap process is started. | Correct |
| Wrong: Client | UNV3087I Universal Connector component 1530192277 registered with local Broker.<br><br>UNV0668I Establishing RFC connection.<br><br>UNV0629E SAP logon error: Client 003 is not available in this system<br><br>UNV0616E Universal Connector ending unsuccessfully with exit code 202. | Correct |
| Wrong: dest |     | Correct |
| Wrong: Client | UNV3087I Universal Connector component 1530192982 registered with local Broker.<br><br>UNV0668I Establishing RFC connection.<br><br>UNV0629E SAP logon error: Client 003 is not available in this system<br><br>UNV0616E Universal Connector ending unsuccessfully with exit code 202.<br><br>##ERROR# ### exitcode =202 |     |
| Wrong: Usapdir | ##ERROR# /opt/universal/usap/bins does not exists. | Correct |
| Wrong: opsclidir | ##ERROR# /opt/universal/opscli/bin/s does not exists. | Correct |
| launch task name that does not exists | ##ERROR#in function launch\_task No tasks found matching with name: Sleep\_${env}\_6000 | Correct |
| Wrong: oms port | Not yet tested |     |
| Wrong: oms hostname | Not yet tested |     |
| Confirm Events or | Not yet tested |     |
| Leave status as new | Not yet tested |     |
| NEW – scan only for new Events (default) | Not yet tested |     |
| Confirmed – scan only for confirmed Events | Not yet tested |     |
| Any Status – scan for any status of Events | Not yet tested |     |

* * *

8 Document References
=====================

There are no document references.

* * *
