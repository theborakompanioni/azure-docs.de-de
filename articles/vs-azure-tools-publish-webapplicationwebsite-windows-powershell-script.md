---
title: Publish-WebApplicationWebSite (Windows PowerShell-Skript) | Microsoft Docs
description: "Informationen zum Veröffentlichen eines Webprojekts auf einer Azure-Website. Dieses Skript erstellt die erforderlichen Ressourcen in Ihrem Azure-Abonnement, wenn sie noch nicht vorhanden sind."
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 6942ea67050b86dc8f5a4c9d71ebd8ebd83ad0a2


---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publish-WebApplicationWebSite (Windows PowerShell-Skript)
## <a name="syntax"></a>Syntax
Veröffentlicht ein Webprojekt auf einer Azure-Website. Das Skript erstellt die erforderlichen Ressourcen in Ihrem Azure-Abonnement, wenn sie noch nicht vorhanden sind.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguration
Der Pfad zur JSON-Konfigurationsdatei, in der die Details der Bereitstellung beschrieben sind.

| Parameter | Standardwert |
| --- | --- |
| Aliase |(Keine) |
| Erforderlich |true |
| Position |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="subscriptionname"></a>SubscriptionName
Der Name des Azure-Abonnements, in dem Sie die Website erstellen möchten.

| Parameter | Standardwert |
| --- | --- |
| Aliase |(Keine) |
| Erforderlich |false |
| Position |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="webdeploypackage"></a>WebDeployPackage
Der Pfad zum Webbereitstellungspaket für die Veröffentlichung auf der Website. Sie können dieses Paket in Visual Studio mithilfe des Assistenten "Web veröffentlichen" erstellen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure-Clouddiensten und ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parameter | Standardwert |
| --- | --- |
| Aliase |(Keine) |
| Erforderlich |false |
| Position |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Der Benutzername und das Kennwort für die SQL-Datenbank in Azure.

| Parameter | Standardwert |
| --- | --- |
| Aliase |(Keine) |
| Erforderlich |false |
| Position |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Falls "true", werden Nachrichten vom Skript in den Ausgabedatenstrom ausgegeben.

| Parameter | Standardwert |
| --- | --- |
| Aliase |(Keine) |
| Erforderlich |false |
| Position |benannt |
| Standardwert |false |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="remarks"></a>Hinweise
Eine ausführliche Erläuterung der Verwendung des Skripts zum Erstellen von Entwicklungs- und Testumgebungen finden Sie unter [Verwenden von Windows PowerShell-Skripts zum Veröffentlichen in Entwicklungs- und Testumgebungen](vs-azure-tools-publishing-using-powershell-scripts.md).

In der JSON-Konfigurationsdatei sind die Details angegeben, was bereitgestellt werden muss. Dazu zählen die Informationen, die Sie beim Erstellen des Projekts angegeben haben, z. B. den Namen und Benutzernamen für die Website. Sie umfassen auch die bereitzustellende Datenbank, sofern vorhanden. Der folgende Code zeigt ein Beispiel einer JSON-Konfigurationsdatei:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Sie können die JSON-Konfigurationsdatei bearbeiten, um den Umfang der Bereitstellung zu ändern. Der Abschnitt "Website" ist erforderlich, der Abschnitt "Datenbank" optional.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Publish-WebApplicationWebSite (Windows PowerShell-Skript)](vs-azure-tools-publish-webapplicationvm.md)




<!--HONumber=Nov16_HO3-->


