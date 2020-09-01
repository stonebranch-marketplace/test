Start-up Guide

Application Automation for OPENSHIFT

Securely Transfer Files to and from Applications Running on OPENSHIFT

Table of Contents {#table-of-contents .list-paragraph}
=================


[Table of Contents](#table-of-contents)

[1. Introduction](#_Toc49848860)

[1.1. Use Case Description](#use-case-description)

[1.2. Key features](#key-features)

[1.3. Solution Architecture](#solution-architecture)

[1.4. General Description of File Transfer Process](#general-description-of-file-transfer-process)

[1.4.1. Graphic Overview](#graphic-overview)

[1.4.2. Process Steps](#process-steps)

[2. How to Get Started](#how-to-get-started)

[2.1. Prerequisites](#prerequisites)

[2.1.1. Universal Automation Center](#universal-automation-center)

[2.1.2. Universal Data Mover License Key](#universal-data-mover-license-key)

[2.1.3. Universal Agent 6.8.x or Above](#universal-agent-6.8.x-or-above)

[2.1.4. OPENSHIFT 4.x](#openshift-4.x)

[2.1.5. Linux Server](#linux-server)

[2.2. Configuration Steps](#configuration-steps)

[2.3. Set Up a File Transfer Task in Universal Controller](#set-up-a-file-transfer-task-in-universal-controller)

[2.3.1. Define a new Agent Cluster for the OPENSHIFT application "newsflash"](#define-a-new-agent-cluster-for-the-openshift-application-newsflash)

[2.3.2. Configure the File Transfer Task](#configure-the-file-transfer-task)

[2.4. Add the Universal Sidecar Container to the Application Deployment Script](#add-the-universal-sidecar-container-to-the-application-deployment-script)

[2.5. Deploy the OPENSHIFT Application](#deploy-the-openshift-application)

[2.6. Running the File Transfer](#running-the-file-transfer)

[2.7. Options to Trigger the File Transfer Scenario](#options-to-trigger-the-file-transfer-scenario)

[2.8. Integration of File Transfer into Microservices Architectures](#integration-of-file-transfer-into-microservices-architectures)

[2.9. Security and Auditability](#security-and-auditability)

[3. Summary & Benefits](#_Toc49848884)

[4. Document References](#document-references)

[5. About Stonebranch](#about-stonebranch)

Introduction
============

This start-up guide will describe a use case for how to securely
transfer business data located on an on-premise Linux server to an
application running on OPENSHIFT, and vice versa, in real-time. As a
result, the applications on OPENSHIFT will always have the most up to
date business data.

The solution also enables the delivery and reception of data from a
Windows server, mainframe or any cloud storage to an application running
on OPENSHIFT, and vice versa. For more info, refer to the solution paper
[Hybrid Cloud File Transfer Solution
Paper](https://www.stonebranch.com/solutions/hybrid-cloud-file-transfers/)
\[8\].

Use Case Description
--------------------

To improve scalability, reduce resource consumption and shorten the time
to develop, test, and roll-out new applications, many IT companies are
currently creating new applications or migrating their applications from
their internal core IT environment into an OPENSHIFT environment hosted
on-premise and on public clouds.

These new or migrated applications are running in containers in an
OPENSHIFT pod. In many cases, they require business data from multiple
sources, including on-premise business applications, mainframe, and
various public cloud storage systems. Additionally, they both receive
and provide data to connected systems, such as SAP business warehouse.

The following use case demonstrates how to securely transfer business
data located on a LINUX server to a web application running on
OPENSHIFT, in real-time.

**USE CASE: "news-flash application"**

The sample use case demonstrates how all started instances of a web
server for "breaking news" (one pod per web server) are updated in
OPENSHIFT with a new webpage. The new webpage html files and related
pictures are sent from an on-premise server to all running pods
containing a web server instance started for the news-flash application
in OPENSHIFT. As a result, all users connected to the new-flash
application will see the new published webpage information.

Key features
------------

This start-up use case focuses on file transfer from an on-premise
server to a group of pods. The solution can also support many additional
scenarios. The following are its key features:

-   Transfer files from any (virtual) Linux/Windows server to an
    application on OPENSHIFT (and vice versa)

-   Transfer files from the mainframe to an application on OPENSHIFT
    (and vice versa)

-   Transfer a file from any cloud storage platform to an application on
    OPENSHIFT (and vice versa)

-   Trigger either a time-based or an event-based file transfer (e.g.,
    from a web application using REST APIs)

-   End-to-end monitoring of all file transfers (including monitoring of
    log files)

-   Cloud (SaaS) or on-premise solution

-   Enhanced security -- validated by regular penetration testing

-   High availability

Solution Architecture
---------------------

The Universal Automation Center (UAC) is a web-based enterprise
scheduler, generally available as SaaS in the Cloud or on-premise.

The UAC consists of:

a)  the **Universal Controller**, a web-based workflow, reporting and
    orchestration engine

b)  the **Universal Agent,** a workload execution component

c)  the **Universal OPENSHIFT Agent**, a workload execution component
    that runs in an OPENSHIFT pod as docker container

The Universal Agent specifically built for the OPENSHIFT environment is
called the **Universal OPENSHIFT Agent**.

As soon as an agent is installed on a server, it automatically connects
to the middleware message bus **OMS** of the Universal Automation
Center, and is ready to execute remote commands/scripts and file
transfers, regardless of whether the agent runs on a server, mainframe,
or inside an OPENSHIFT pod.

For applications that provide an API like SAP, databases, cloud storage
services, etc., no agent is required, as they are scheduled via their
corresponding API.

General Description of File Transfer Process
--------------------------------------------

The architecture below outlines how data is transferred from an
on-premise server to all instances of an application running on
OPENSHIFT. A transfer from the mainframe is performed in a similar way.

### Graphic Overview

![](media/image4.png){width="7.266452318460193in"
height="5.931619641294838in"}

Figure : Provide Business Data from an On-Premise Server to an
Application Running in a Pod

### Process Steps


| &nbsp; | **Component** | **Description** |
| --- | --- | --- |
|**Customer Data Centre**| &nbsp; | &nbsp; |
| 1 | **Linux Server** | A Universal Agent needs to be installed on the server that sends/receives files from an application distributed in an OPENSHIFT pod. This Agent can send files to any other agent installed in the Data Center. The Agent can also connect to any public could storage for file transfer. |
|**OPENSHIFT PaaS**| &nbsp; | &nbsp; |
| 2 | **Reverse Proxy** | Due to security regulations, all communication from and to OPENSHIFT should be sent via reverse https proxy. |
| 3 | **Application Instance cluster** | An application is deployed in a pod.<br/>If the application load increases, the OPENSHIFT orchestration platform allows users to dynamically scale up the number of application instances by starting an additional pod.<br/>In this example, the number of pods is increased in OPENSHIFT from 2 to 4, and the Universal Agents installed in the pods are added dynamically to the Universal Controller agent cluster related to the application.<br/>If the load decreases, application instances, i.e. pods can be stopped in OPENSHIFT. |
| 4 | **Pod with Sidecar Container** | All pods contain a sidecar container with a Universal OPENSHIFT Agent. Each sidecar container is based on the Red Hat UBI image with a Universal OPENSHIFT Agent installed inside. The latest version of the image can be retrieved from the docker registry.<br/> A detailed documentation of the image can be found [here](https://docs.stonebranch.com/confluence/display/UA68/Docker+Containers)\[1\]. <br/> Once a pod, and respectively the sidecar container, are started, the Universal OPENSHIFT Agent of the container automatically registers to a Universal Controller agent cluster dedicated to the application.<br/>  Only a single outbound port is opened from the pod to the OMS (Controller message bus). No inbound port to OPENSHIFT is required. For each application, one pre-configured Universal Agent cluster is created, containing the Universal OPENSHIFT agents of all started instances of the application.</br>The example in Figure 1 shows two applications:<br/>  1.newsflash<br/>  2.MyXYZ - Web<br/>  Each instance of an application is represented by one pod.<br/>  As soon as the Universal OPENSHIFT Agent of the sidecar container is assigned to the related Universal Controller agent cluster, all related pods can send and receive files from/to any other Universal Agent installed on any server. In addition, the application running in the pod can be scheduled like any other application, enabling it to be included in any automated business process.<br/>  The Universal Agent cluster supports file transfers to just one agent (pod), or to all agents (i.e. started pods related to an application) in the Universal Agent cluster.<br/> |

* * *

How to Get Started
==================

This section outlines how this solution can be deployed in the form of a
basic file transfer from a local server to a web application running in
OPENSHIFT.

**USE CASE: "news-flash application"**

The sample use case demonstrates how all started instances of a web
server for "breaking news" (one pod per web server) are updated in
OPENSHIFT with a new webpage. The new webpage html files and related
pictures are sent from an on-premise server to all running pods
containing a web servers instance started for the news-flash application
in OPENSHIFT. As a result, all users connected to the new-flash
application will see the newly published webpage information.

The setup consists of only three steps:

1.  Configure the file transfer workflow using the Universal Controller
    Web-GUI.

2.  Add the Universal Sidecar container to the application deployment
    script.

3.  Deploy the pod to OPENSHIFT and scale up or down as required.

Prerequisites
-------------

In order to set up this sample use case, the following prerequisites are
required:

-   Universal Automation Center 6.8.x or above

-   Universal Data Mover License Key

-   Universal Agent 6.8.x or Above

-   OPENSHIFT 4.x

-   Linux Server

### Universal Automation Center

This use case requires either an on-premise Universal Automation Center,
or Universal Automation Center as SaaS in the cloud. A free trial
version can be requested
[here](https://www.stonebranch.com/products/universal-automation-center/)
\[2\].

An automation process can be set up using the drag and drop workflow
definition tool of the Universal Controller. Each workflow can define
dependencies between numerous tasks, independent of the operating system
on which they are executed.

In the sample scenarios described here, only a single file transfer task
is required. This task can be manually configured, or a pre-configured
task can be uploaded from GitHub.

### Universal Data Mover License Key 

This solution requires a license key for the Universal Data Mover. This
license key will be entered as configuration parameter in the OPENSHIFT
application deployment script.

A free demo license key can be requested from Stonebranch \[5\].

### Universal Agent 6.8.x or Above

File transfers are performed between Universal Agents. Universal Agents
can be deployed on any platform (Linux, Windows, z/OS, OPENSHIFT, etc.).
For this scenario, a Universal Agent needs to be installed on an
on-premise Linux server. That agent can then transfer data to a
Universal Agent installed as sidecar container in an application running
in OPENSHIFT. The Universal Agent in OPENSHIFT is installed
automatically by OPENSHIFT during the deployment of the pod.

For the installation of a Universal Agent on a Linux server, please
refer to the Universal Agent installation guide found
[here](https://docs.stonebranch.com/confluence/display/UA68/Universal+Agent+for+UNIX+Installation)
\[4\].

### OPENSHIFT 4.x

This scenario uses OPENSHIFT 4 deployed via a [Red Hat CodeReady
Container](https://developers.redhat.com/products/codeready-containers/overview)
(CRC) \[3\]. A CRC enables OPENSHIFT to run on a local laptop or server.
However, any OPENSHIFT 4 deployment could be used.

### Linux Server 

This server is used to send and receive data from the OPENSHIFT
application. Any Linux Server can be used. A Universal Agent needs to be
installed on this server in order to send and receive files from the
Universal Agent installed on OPENSHIFT.

For information on the installation of a Universal Agent on a Linux
server, please refer to the Universal Agent installation guide, found
[here](https://docs.stonebranch.com/confluence/display/UA68/Universal+Agent+for+UNIX+Installation)
\[4\].

Configuration Steps
-------------------

The installation consists of only three steps:

1.  Configure the file transfer workflow using the Universal Controller
    web GUI.

2.  Add the Universal Sidecar container to the application deployment
    script.

3.  Deploy the pod to OPENSHIFT and scale up or down as required.

Set Up a File Transfer Task in Universal Controller
---------------------------------------------------

This scenario requires an update to the homepage of a very simple
application (newsflash) consisting of NGNIX web servers. The new
homepage html files and related pictures, therefore, should be sent from
the on-premise Linux server to all running pods containing a web server
instance started for the news-flash application in OPENSHIFT.

Setting up the file transfer task in Universal Controller requires two
steps:

a)  Define a new Agent cluster for the OPENSHIFT application
    "newsflash".

b)  Configure the file transfer task.

###  Define a new Agent Cluster for the OPENSHIFT application "newsflash"

An agent cluster must be configured in Universal Controller for each
application in OPENSHIFT. When a pod is started for an application in
OPENSHIFT, the Universal OPENSHIFT Agent, which is deployed in a
different pod as a sidecar container, will automatically register to the
Universal Controller agent cluster related to the application of the
started pod.

The agent cluster where all newsflash related OPENSHIFT agents will
register is called:

-   *AGENT_CLUSTER_APP_NEWSFLASH*

![](media/image6.png){width="6.294444444444444in"
height="2.1180555555555554in"}

### Configure the File Transfer Task

A new task needs to be configured for transferring the files from the
on-premise Linux server to all agents assigned to the Universal
Controller agent cluster.

![](media/image7.png){width="6.294444444444444in" height="3.9125in"}

File transfer task script:

![](media/image8.png){width="6.294444444444444in"
height="1.7986111111111112in"}

Source Linux Server: *192.168.88.40* (adjust according to your Linux
server)

Source Folder Linux Server: */home/nils/demo/out*

Files to Transfer: *index.html, RedHat.png*

Destination Agent Cluster: *AGENT_CLUSTER_APP_NEWSFLASH*

Destination Folder in the Pod: */ss/ (this is the mounted pod
directory)*

The export files of the file transfer task can be found here \[6\].

Add the Universal Sidecar Container to the Application Deployment Script
------------------------------------------------------------------------

**Add the Universal OPENSHIFT Agent as a Sidecar Container**

To add the Universal OPENSHIFT Agent as sidecar container to your
application, you need to add the following section to your application
deployment YAML file:

![](media/image9.png){width="6.427308617672791in"
height="1.8752679352580928in"}

Configure the following three parameters in the deployment file:

  Parameter          Description
  ------------------ ------------------------------------------------------------------------------------------------------------------------------
  UAGAENGTCLUSTERS   Name of the OPENSHIFT Application ( needs to be the same name as the Universal Controller Agent cluster configured in 2.3.1.
  UAGOMSSERVERS      IP and PORT of the Universal Controller Message Middleware OMS
  UDM_LICENSE        Universal Agent License Key for File Transfer\*

\*Note: A 30 day demo license key can be obtained from
[Stonebranch](https://www.stonebranch.com/contact/) \[5\].

**\
**

**Configure a Shared Folder for Application and Sidecar Containers**

To make the files received by the Universal OPENSHIFT Agent available to
the application in the pod, you need to create a shared folder between
the application and the sidecar container with the Universal OPENSHIFT
Agent.

![](media/image10.png){width="7.259739720034996in"
height="6.656404199475066in"}

The following screenshot shows the complete deployment script,
consisting of the OPENSHIFT web application based on an NGINX web server
and the Universal Agent as a sidecar container.

![](media/image11.png){width="7.280240594925634in"
height="7.006493875765529in"}

The file can be downloaded from GITHUB \[6\].

**Configure Service to Access the Newsflash Application**

To make the newsflash application available outside OPENSHIFT, you need
to create a Service with a NodePort.

![](media/image12.png){width="6.445352143482065in"
height="5.655844269466317in"}

![](media/image13.png){width="6.272726377952756in"
height="3.6144969378827647in"}

The Newsflash application will then be accessible via the following IP:

(Note the host IP can be retrieved on a CRC-based OPENSHIFT install via
the command:

*crc ip*)

In the described example it is: http://192.168.130.11:31510/

Deploy the OPENSHIFT Application
--------------------------------

To deploy the application on OPENSHIFT, you only need to deploy the
deployment file in the 'Deployment Configs' screen (see below).

![](media/image14.png){width="6.1298698600174975in"
height="8.075218722659667in"}

In the deployment config file, we defined three replicas. This means
that once we press the 'Create Deployment Config' button, three
instances of our application will be started (= 3 pods).

These three Universal OPENSHIFT Agents will connect to the Universal
Controller agent cluster: "*AGENT_CLUSTER_APP_NEWSFLASH,*" which was
configured in the deployment script as a parameter and during the set-up
in the Universal Controller.

In the following screenshot, you can see the three started pods in
OPENSHIFT:

![](media/image15.png){width="6.233010717410323in"
height="3.2077930883639545in"}

...as well as the related Universal OPENSHIFT Agents, which have
registered automatically to the Universal Controller agent cluster:

![](media/image16.png){width="6.294444444444444in"
height="1.9361111111111111in"}

**Auto Scaling of Pod:**

If the number of pods in OPENSHIFT is scaled up, more agents will
automatically register for each new pod. Conversely, if the number of
pods is scaled down, the excess agents will automatically be removed
from the Universal Controller agent cluster.

Running the File Transfer
-------------------------

Once the Universal OPENSHIFT Agents have registered in the Universal
Controller agent cluster, they are ready to send and receive files.

The file transfer task in Universal Controller is then launched,
initiating the transfer of homepage html-files*,* including related
pictures from the on-premise Linux server to all started pods of the
newsflash application. In this example, we triggered the file transfer
manually.

![](media/image17.png){width="6.294444444444444in"
height="3.9402777777777778in"}

![](media/image18.png){width="6.294444444444444in"
height="0.8611111111111112in"}

In the screenshot above, you can see that three file transfers were
launched. This is because the Universal Controller agent cluster
contained three agents, one for each started pod.

As result, the files are transferred from the Linux Server to all
assigned pods and the homepage is updated with the new data:

![](media/image19.png){width="4.493506124234471in"
height="2.05588801399825in"}

The new data can also be viewed in all PODs directly:

![](media/image20.png){width="6.294444444444444in"
height="3.6444444444444444in"}

Options to Trigger the File Transfer Scenario
---------------------------------------------

In the example above, we triggered the file transfer manually. However,
the Universal Controller can trigger the file transfers in numerous ways
to ensure that the business data the application requires is always
consistent and up to date.

Additional triggers include (but are not limited to):

-   File arrival

-   Time-based (with support for internal and external calendar like an
    SAP calendar)

-   Email arrival

-   Web services

-   Event in a message queue (e.g. MQ, JMS)

-   The status of another task/workflow (e.g. the start of a new file
    transfer, or if the transfer from last night was successful)

Integration of File Transfer into Microservices Architectures
-------------------------------------------------------------

Any file transfer can be triggered by calling the REST API of the
Universal Controller.

Essentially, a file transfer workflow can be started from any
application, independent of the platform it runs on (e.g. virtual
server, mainframe or pod). This single API enables a loosely coupled
integration with a microservices architecture.

Security and Auditability
-------------------------

The file transfer protocol outlined above is based on Stonebranch UDM
protocol, which encrypts all data and communication channels using
TLS1.2 (e.g. AES 256 / SHA 384).

Universal Controller's web GUI real-time reporting functionality
provides full auditability through detailed information of all data
transfers, including log files.

Summary & Benefits
==================

The Universal Automation Center, with the introduction of the newly
developed Universal OPENSHIFT Agent, enables the secure and reliable
transfer of business data located on the mainframe, on cloud storage
platforms, or any server, to an application running on OPENSHIFT (and
vice versa).

As this example has shown, only three steps are required to configure a
secure file transfer to or from any of your OPENSHIFT applications.

Document References
===================

+----------------------------------+----------------------------------+
| Ref\#                            | Description                      |
+==================================+==================================+
| 1.  *https:/                     | OPENSHIFT Agent documentation    |
| /docs.stonebranch.com/confluence |                                  |
| /display/UA68/Docker+Containers* |                                  |
+----------------------------------+----------------------------------+
| 2.  *ht                          | Link to Free Trial for Universal |
| tps://www.stonebranch.com/produc | Automation Center                |
| ts/universal-automation-center/* |                                  |
+----------------------------------+----------------------------------+
| 3.  *https                       | RED HAT webpage to retrieve an   |
| ://developers.redhat.com/product | OPENSHIFT Code Ready Container   |
| s/codeready-containers/overview* | (CRC)                            |
+----------------------------------+----------------------------------+
| 4.  *https://docs.stonebranch.co | Universal Agent Installation     |
| m/confluence/display/UA68/Univer |                                  |
| sal+Agent+for+UNIX+Installation* |                                  |
+----------------------------------+----------------------------------+
| 5.  *https                       | Contact address to get a 30days  |
| ://www.stonebranch.com/contact/* | license key for Universal Data   |
|                                  | Mover                            |
+----------------------------------+----------------------------------+
| 6.  *.github link*               | Universal Controller File        |
|                                  | Transfer task export ( xml)      |
+----------------------------------+----------------------------------+
| 7.  *. github link*              | POD deployment YAML file         |
+----------------------------------+----------------------------------+
| 8.  *htt                         | Hybrid Cloud File Transfer       |
| ps://www.stonebranch.com/solutio | Solution Paper                   |
| ns/hybrid-cloud-file-transfers/* |                                  |
+----------------------------------+----------------------------------+

About Stonebranch
=================

  Company                                                                              About
  ------------------------------------------------------------------------------------ ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ![](media/image21.png){width="1.4784722222222222in" height="0.4128018372703412in"}   Stonebranch builds dynamic IT automation solutions that transform business IT environments from simple IT task automation into sophisticated, real-time business service automation, helping organizations achieve the highest possible Return on Automation. No matter the degree of automation, Stonebranch software is simple, modern and secure. Using its universal automation platform, enterprises can seamlessly orchestrate workloads and data across technology stacks and ecosystems. Headquartered in Atlanta, Georgia with points of contact and support throughout the Americas, Europe, and Asia, Stonebranch serves some of the world\'s largest financial, manufacturing, healthcare, travel, transportation, energy, and technology institutions. 
