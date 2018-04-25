﻿---
title: Lync Server 2013 Persistent Chat Resource Kit Tools
TOCTitle: Lync Server 2013 Persistent Chat Resource Kit Tools
ms:assetid: 7a34d2ba-eb25-4e22-92d1-b9baf81b102c
ms:mtpsurl: https://technet.microsoft.com/pt-br/library/JJ945599(v=OCS.15)
ms:contentKeyID: 52057785
ms.date: 06/27/2014
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Lync Server 2013 Persistent Chat Resource Kit Tools

 

_**Tópico modificado em:** 2013-02-24_

The Lync Server 2013 Chat Persistente Resource Kit tools help to make routine tasks easier for IT administrators who deploy and manage Lync Server 2013 Servidor de Chat Persistente. In addition to installation instructions, this topic describes the purpose of each tool, and examples of its use.

## Installation of the Resource Kit Tools

To install the Lync Server 2013, Ferramentas do Resource Kit, download **PersistentChatReskit.msi**. Run **PersistentChatReskit.msi** to do a simple installation. The .msi installs all the tools in the following path: \\**Program Files\\ Microsoft Lync Server 2013\\Persistent Chat Server Resource Kit**. Tools that are self-contained executables are in this folder. Tools that also have files are in their own subfolders.

<table>
<thead>
<tr class="header">
<th><img src="images/JJ945587.important(OCS.15).gif" title="important" alt="important" />Importante:</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>After installing the Lync Server 2013, Ferramentas do Resource Kit, you must install <strong>PsExec.exe</strong> and copy <strong>PsExec.exe</strong> to the following path: \<strong>Program Files\ Microsoft Lync Server 2013\Persistent Chat Server Resource Kit\ChatStressTool</strong>. If you do not copy <strong>PsExec.exe</strong>, the Chat Persistente Stress Tool will throw an error exception, and not perform correctly. Make sure that you meet this prerequisite requirement prior to running the tool. For details about installing <strong>PsExec.exe</strong>, see <a href="http://go.microsoft.com/fwlink/p/?linkid=282246">http://go.microsoft.com/fwlink/p/?LinkId=282246</a>.</td>
</tr>
</tbody>
</table>


## Supported Environments

For optimal performance, the Lync Server 2013, Ferramentas do Resource Kit should be installed in the same environment and with the same specifications that are required for Lync Server 2013.

## Resource Kit Tools Overview

Here are the tools that are provided in the Lync Server 2013 Chat Persistente Resource Kit. The following section provides a description of each tool, including requirements and example usage.

  - AffCheck

  - ChatMonitoringSummary

  - ChatStress Tool

  - ChatUpgradeVerifier

  - ChatUsageReport

  - ScheduleADSyncforPrincipal

## AffCheck

## Description

The AffCheck tool confirms that the Chat Persistente back-end database user and group affiliation records match that of Serviços de Domínio Active Directory.

## Requirements

The tool is installed with the PersistentChatResKit installer on a domain joined machine.

The user account under which the tool is run must have Read access to the Chat Persistente back-end database and Active Directory Domain Services.

## Usage

Configure the AffCheck.exe.config file according to the instructions in the config file and run the AffCheck tool without command-line parameters. Following are the contents of the default AffCheck.exe.config.

**AffCheck.exe.config:**

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <!--Domain Controller IP Address-->
        <add key="LDAP" value="LDAP://0.0.0.0/"/>
        
        <!-- Domain DN  This is case sensitive, it must match exactly-->
        <add key="DomainComponent" value ="DC=DOMAIN,DC=COM"/>
        
        <!--Domain Administrator Login and Password-->
        <add key="DomainLogin" value="DOMAIN\Administrator"/>
        <add key="DomainPassword" value ="password"/>
        
        <!-- Connection string to Group Chat Database-->
        <add key="ConnectionString" value="data source=SQL_SERVER\INSTANCE;initial catalog=DATABASE_NAME;integrated security=SSPI"/>
        
        <!--Check group affiliations-->
        <add key="CheckGroups" value="true"/>
        
        <!--Check user affilations-->
        <add key="CheckUsers" value="true"/>
        
        <!--List all affiliations if there is a mismatch between database and active directory-->
        <add key="ListAffiliations" value="true"/>
    
        <!--If you need to offset the results of the number of affilations in AD(can be negative to add to AD parent count)-->
        <add key="Offset" value ="0"/>
    
        <!--If you need to ignore certain parents, provide a semi colon delimitted list.-->
        <add key="Ignore" value ="DC=uatest,DC=test,DC=contoso,DC=com;DC=test,DC=contoso,DC=com"/>
      </appSettings>
    </configuration>

## ChatMonitoringSummary

## Description

The PersistentChatMonitoringSummary tool moves Chat Persistente monitoring information from the monitoring database into a specified CSV log file.

The CSV file will contain a breakdown of Chat Persistente sessions by number of total sessions, successful sessions, unexpected failures, expected failures, and a breakdown of the unexpected failures by diagnostic ID, number of failures, and failure description.

## Requirements

Install the Chat Persistente Resource Kit tools on a domain-joined machine that has access to the Monitoring database.

The user account under which the tool runs must have Read access to the Monitoring database.

The file, PersistentChatMonitoringSummary.exe.config, must contain a \<connectionStrings\> section that defines the connection string to the Monitoring database. It must also contain a key for the PersistentChatEndpointUri that the monitoring data will be gathered for, and a file path to a location for the CSV file that will be generated. Refer to the installed config file for examples. The file must be located in the same directory as the tool.

## Usage

    PersistentChatMonitoringSummary [-StartDateTime <date>] [-EndDateTime <date>]

These parameters define the selection of data:

**StartDateTime:** Optionally specifies the start date of the selection period. Default: 1/1/1753 12:00:00 AM

**EndDateTime:** Optionally specifies the last date of the selection period. Default: Now

## Example

    C:\Users\Administrator.VDOMAIN>Desktop\PersistentChatMonitoringSummary.exe
    Reading database connection information, Persistent Chat endpoint uri, and csv output path information from the application config file...
    Connecting to Monitoring database with connection string specified in the application config file...
    Gathering Persistent Chat Session Summary information between "1/1/1753 12:00:00 AM" and "11/19/2012 10:11:25 AM" for Persistent Chat Endpoint Uri "persistentChatEndpointUri@domain.com"...
    Press enter to continue or hit ctr-c if these settings are incorrect...
    
    The summary information about Persistent Chat sessions from the Monitoring database has been output to C:\PersistentChatMonitoring_dd4ace24-4c8a-4a3d-8fd4-591bdfacf47b.csv
    Press enter to exit...

## Chat Persistente Stress Tool

## Description

The Chat Persistente Stress tool provides an easy way to simulate usage of Chat Persistente to test real-world performance, including varied user models to better fit your expected usage scenarios.

## Requirements

Install the Chat Persistente Resource Kit tools onto a domain-joined machine that has access to the Chat Persistente back-end database.

In addition to this *controller* machine, you will need several *loader* machines. For every 10K users in your user model, you will need at least 4GB of free RAM on a loader machine. For example, a run with 80K users will require about 32GB of RAM spread across all loader machines. We recommend that you have at least three loader machines, regardless of expected load.

Loader machines must have the .NET 4.5 Framework as well as the Visual C++ 2012 Redistributable installed.

## Configuration

Copy ChatStressTool files into a shared folder accessible from all loader machines.

Create users and channels for use in the stress run:

  - Create as many users as your user model calls for, enable them for Lync, and set their Chat Persistente policy to Enabled.

  - Create a category for your stress channels, and then create as many rooms as are needed under that category. The category should have all stress users in its **Allowed** list (by way of adding their OU), and stress rooms should have a privacy setting of **Open**.

  - We recommend creating extra stress rooms. You can create 50,000 rooms with the following Interface da linha de comando do Windows PowerShell command:
    
        for ($i = 0; $i -le 50000; $i++) { New-CsPersistentChatRoom -Category <parent category> -Name "StressChan_$i" -Privacy Open }

Edit the configuration files to fit your topology:

In **LoaderProcess.exe.config**, change “controller.contoso.com” to the controller machine’s fully qualified domain name (FQDN).

In **StressLauncher.exe.config:**

1.  Change the “LoaderBinary” setting value to the shared folder’s path.

2.  Change “AdminUser”/”AdminPassword” to credentials that have admin access to loader machines.

3.  Change “ChannelCategory” to the name of the category that stress channels have been created under.

4.  Change “UserNamePattern” and “UserPasswordPattern” to a template that matches your stress user credentials. {0} is replaced with the user’s index number.

5.  Change “Domain” to the SIP domain of your test topology.

6.  Change “ConnectionString” to a connection string for your Chat Persistente back-end database.

7.  Change “UserIndexStart” to the index of the first stress user.

8.  Change “LyncFQDN” to the FQDN of your Front End pool.

9.  Modify the “Machines” list to include machine names for all of your loader machines.

10. Change the baseAddress of the service endpoint (default is “controller.contoso.com”) to the FQDN of your controller machine.

## Usage

After configuration is complete, open StressLauncher.exe on the controller machine. You can launch StressLauncher as any user. The credentials under which the loader processes start on the loader machines must be specified in the config file. You also must give a connection string that has Read access to the Chat Persistente back-end database. If this connection string uses integrated Windows authentication, you must launch StressLauncher as a user that has this access.

Alter the user model settings as needed. Click **Start Load** to initiate a run. After a minute or so, users will start being signed in, and the progress bar will begin to fill. At this point, you may can the controller machine working and take performance measurements.

## ChatUpgradeVerifier

## Description

ChatUpgradeVerifier is a Chat Persistente specific database comparison tool. The tool compares either the Group Chat 2007 R2 or Group Chat 2010 Database (2007/2010Db) to the Chat Persistente 2013 Database (2013Db).

The tool will check, one by one, each category, Chat Persistente room, and add-in in 2007/2010Db to see if it appears in the 2013Db. The comparison includes checking all settings on the category, chat room, or add-in, any principals in scope on the category, and any principal in a role on either the category or the chat room. If a category or a chat room does not appear correctly in the 2013Db, the differences will be output to a conflicts file. If, after the upgrade has occurred, the 2007/2010Db is changed and then this tool is run, there will be a differences output to the conflicts file. Note that this application is a database comparison tool only and does not validate the upgrade process.

## Requirements

Install the Chat Persistente Resource Kit tools on a domain-joined machine that has access to the Chat Persistente back-end databases (previous and current versions for Chat Persistente).

The user account under which the tool runs must have Read access to the Chat Persistente databases.

The ChatUpgradeVerifier.exe.config file must contain either the GroupChat2007R2Db parameter or the GroupChat2010Db parameter, with a connection string to the appropriate Group Chat database (either Groupchat 2007R2 or 2010). It must also contain a PersistentChat2013Db parameter, with a connection string to the Chat Persistente 2013 database.

## Usage

Run **ChatUpgradeVerifier** without any parameters.

## Example

![Executando ChatUpgradeVerifier.exe.](images/JJ945599.4c273bc3-7926-47c7-ade7-34522721ebf9(OCS.15).jpg "Executando ChatUpgradeVerifier.exe.")

## Chat Persistente Usage Report

## Description

The ChatUsageReport tool generates an HTML report of Chat Persistente service usage.

## Requirements

Install the Chat Persistente Resource Kit tools on a domain-joined machine that has access to the Chat Persistente back-end database.

The user account under which the tool is run must have Read access to the Chat Persistente back-end database.

The file, ChatUsageReport.exe.config, must contain a \<connectionStrings\> section defining the connection string to the Chat Persistente back-end database. The contents of the default config file are included here, for your reference.

## Usage

    ChatUsageReport [-StartDate {date}] [-EndDate {date}] [-TopActiveUsers {n}] [-TopActiveRooms {n}] [-LeastActiveRooms {n}] [-RoomsInactiveSince {Date}] [-OutputFolder {path}]

These parameters define the selection of data:

**StartDate:** Optionally specifies the UTC start date of the selection period. Default: Earliest Date

**EndDate:** Optionally specifies the UTC end date of the selection period. Default: Now

These parameters define how and what data is displayed:

**TopActiveUsers:** If this is specified, the report will include the n most active users in terms of the number of messages the user has posted in the chat room for the selected period. Default: 10

**TopActiveRooms:** If this is specified, the report will include the n most active chat rooms in terms of the number of messages posted in the room for the selected period. Default: 10

**LeastActiveRooms:** If this is specified, the report will include the n least active chat rooms in terms of the number of messages posted in the chat room for the selected period. Rooms will have at least one message posted. Default: 10

**RoomsInactiveSince:** If this is specified, the report will include a list of chat rooms that have been inactive since the specified date. Default: Entire Time

**OutputFolder:** The folder where the ChatUsageReport.html and the graph images will be placed. This must be defined in the config file or on the command line.

All of the command line parameter values can also be specified in the ChatUsageReport.exe.config file that is located in the same directory as the tool. If any value is specified in both the config file and the command line, the command line value will override the config file value.

## Output

The report will always include the following output:

  - Top n most active chat rooms by number of message posts for selected period.

  - Top n most active users by number of message posts for selected period.

  - Top n least active chat rooms by number of message posts for selected period.

  - Chat rooms that are inactive for the entire life of the database, or since the specified date.

  - Daily message post trend for selected period.

  - Weekly message post trend for selected period.

  - Monthly message post trend for selected period.

  - Total message posts for selected period.

  - Total number of enabled rooms.

## Example

The following example generates a usage report for the entire year 2001 and places the report in the OutputFolder specified in the ChatUsageReport.exe.config.

    ChatUsageReport -RoomsInactiveSince 06-20-2010

ChatUsageReport.exe.config:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <connectionStrings>
        <!-- The PersistentChat connection string must be defined in this file. -->
        <add name="PersistentChat" connectionString="Data Source=contoso.com\RTC;Initial Catalog=mgc;Integrated Security=SSPI"/>
      </connectionStrings>
      <appSettings>
        <!-- The OutputFolder must be defined here or on the command line. -->
        <add key="OutputFolder" value="."/>
        <!-- The values below are the same as the application defaults. -->
        <add key="StartDate" value="01/01/0001"/>
        <add key="EndDate" value="12/31/9999"/>
        <add key="TopActiveUsers" value="10"/>
        <add key="TopActiveRooms" value="10"/>
        <add key="LeastActiveRooms" value="10"/>
        <add key="RoomsInactiveSince" value="01/01/0001"/>
      </appSettings>
    </configuration></configuration>

## ScheduleADSyncForPrincipal

## Description

ScheduleADSyncForPrincipal is a Microsoft SQL Server 2012 script that must be run directly from within SQL Server Management Studio when connected to the Chat Persistente back-end database. This script enables you to force Chat Persistente to synchronize its records of a user with those of Active Directory Domain Services, rather than waiting for the scheduled synchronization time.

## Requirements

The user account under which the script is run must have owner access to the Chat Persistente back-end database.

## Usage

Following are the contents of the default script:

    /*
    This script will schedule a principal for a forced AD synchronization cycle
    
    If you're using Sql Server Management Studio, pressing Ctrl+Shift+M will 
    allow you to specify values for the template parameter.
    */
    
        insert into
          tblPrincipalMeta
          (
           prinID
          ,prinAffiliationsDirty
          ,prinAttributesDirty
          ,prinDeleted
          )
          select
            prinID
           ,1
           ,1
           ,0
          from
            tblPrincipal
          where
            prinID not in (select prinID from tblPrincipalMeta) and
            prinID = <PrinID,int,0>
     
        update
          tblPrincipalMeta
        set
          prinAffiliationsDirty = 1
         ,prinAttributesDirty = 1
         ,tryCount = 0
         ,nextTry = null
        where
         prinID = <PrinID,int,0>
