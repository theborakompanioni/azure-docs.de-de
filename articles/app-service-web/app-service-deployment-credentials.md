---
title: "Anmeldeinformationen für die Azure App Service-Bereitstellung | Microsoft Docs"
description: "Erfahren Sie, wie Sie Anmeldeinformationen für die Azure App Service-Bereitstellung verwenden."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: wpickett
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/21/2016
ms.author: dariagrigoriu
translationtype: Human Translation
ms.sourcegitcommit: 73753bb96d2c9680ed5d5bdf5eb3780e371c08d3
ms.openlocfilehash: 53f971624c2c7a7f64eed257aeb0c402461cc7ba


---
# <a name="azure-app-service-deployment-credentials"></a>Anmeldeinformationen für die Azure App Service-Bereitstellung
Die [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)-Plattform unterstützt zwei Arten von Anmeldeinformationen für die Inhaltsbereitstellung.
* Anmeldeinformationen für die Bereitstellung: Benutzerspezifische Anmeldeinformationen
* Veröffentlichungsprofil: Anwendungsspezifische Anmeldeinformationen 

## <a name="a-nameuserscopeauser-scoped-credentials"></a><a name="userscope"></a>Benutzerspezifische Anmeldeinformationen
Die benutzerspezifischen Anmeldeinformationen werden vom Azure-Benutzer erstellt und direkt einem Microsoft-Konto anstatt einer bestimmten App Service-Anwendung zugewiesen. Die benutzerspezifischen Anmeldeinformationen für die Bereitstellung einer App Service-Anwendung können im [Azure-Portal](https://portal.azure.com) festgelegt oder zurückgesetzt werden. Navigieren Sie hier zu **APP-BEREITSTELLUNG > Anmeldeinformationen für Bereitstellung**. Änderungen an diesen benutzerspezifischen Anmeldeinformationen gelten stets für das gesamte Microsoft-Konto. Diese Anmeldeinformationen werden häufig für die FTP- und Git-Bereitstellung verwendet.

![Anmeldeinformationen für die Azure App Service-Bereitstellung](./media/app-service-deployment-credentials/deployment_credentials.png)
 
Beim Delegieren des Zugriffs auf Azure-Ressourcen über die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) oder Co-Administratorberechtigungen kann jeder Azure-Benutzer, dem Zugriff gewährt wird, seine eigenen benutzerspezifischen Anmeldeinformationen nutzen, bis der Zugriff aufgehoben wird. Diese Anmeldeinformationen für die Bereitstellung dürfen nicht mit anderen Azure-Benutzer geteilt werden.

## <a name="a-nameappscopeaapp-scoped-credentials"></a><a name="appscope"></a>Anwendungsspezifische Anmeldeinformationen
Die anwendungsspezifischen Anmeldeinformationen werden von der App Service-Plattform automatisch erstellt. Ihre Speicherung erfolgt im XML-Veröffentlichungsprofil jeder App Service-Anwendung. Das Veröffentlichungsprofil steht im [Azure-Portal](https://portal.azure.com) auf dem Blatt **Übersicht** der App über die Aktion **Veröffentlichungsprofil abrufen** zur Verfügung. Diese Anmeldeinformationen werden häufig für die WebDeploy-basierte Bereitstellung verwendet. Sie können auch für die FTP- oder Git-Bereitstellung verwendet werden. Visual Studio, das auch ein Einstiegspunkt für die WebDeploy-basierte Bereitstellung ist, kann das Veröffentlichungsprofil für die Authentifizierung analysieren.

![Azure App Service-Veröffentlichungsprofil](./media/app-service-deployment-credentials/publish_profile.png)

Beim Delegieren des Zugriffs auf Azure-Ressourcen über die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) oder Co-Administratorberechtigungen kann jeder Azure-Benutzer, dem Zugriff gewährt wird, dasselbe anwendungsspezifische Veröffentlichungsprofil herunterladen. Das Veröffentlichungsprofil kann jederzeit im Azure-Portal auf dem Blatt **Übersicht** der App zurückgesetzt werden. Das Zurücksetzen der anwendungsspezifischen Anmeldeinformationen empfiehlt sich nach dem Aufheben des delegierten Zugriffs.



<!--HONumber=Jan17_HO1-->


