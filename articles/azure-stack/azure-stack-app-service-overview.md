---
title: "App Service-Übersicht: Azure Stack | Microsoft-Dokumentation"
description: "Übersicht über App Service in Azure Stack"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: anwestg
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 13928744e7d2fc145662c2a0d5c26d512cf02150
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="app-service-on-azure-stack-overview"></a>Übersicht über App Service in Azure Stack

Azure App Service in Azure Stack ist das Azure-Angebot für Azure Stack. Mit dem Installationsprogramm für App Service in Azure Stack wird die folgende Gruppe von Rolleninstanzen erstellt:

*  Controller
*  Management (zwei Instanzen werden erstellt)
*  FrontEnd
*  Herausgeber
*  Worker (im Modus „Shared“ (Gemeinsam genutzt))

Außerdem wird mit dem Installationsprogramm für App Service in Azure Stack ein Dateiserver erstellt.
    
## <a name="whats-new-in-the-first-release-candidate-of-app-service-on-azure-stack"></a>Was gibt es Neues im ersten Release Candidate von App Service in Azure Stack?
![Portal für App Service in Azure Stack][1]

Der erste Release Candidate von App Service in Azure Stack baut auf der dritten Vorschauversion auf und verfügt über neue Funktionen und Verbesserungen:

* Azure Functions in Azure Stack-Umgebungen basierend auf Active Directory-Verbunddienste (AD FS) 
* Unterstützung des einmaligen Anmeldens für das Functions-Portal und die erweiterten Entwicklertools (Kudu)
* Java-Unterstützung für Web-, API- und mobile Anwendungen
* Verwaltung von Workerebenen mit VM-Skalierungsgruppen zur Verbesserung von Funktionen für das horizontale Hochskalieren für Dienstadministratoren
* Lokalisierung der Administratoroberfläche
* Höhere Stabilität des Diensts
* Mandantenportal-Updates und Installationsprozess-Updates

## <a name="limitations-of-the-technical-preview"></a>Einschränkungen der Technical Preview-Version

Für die Vorschauversionen von App Service in Azure Stack wird kein Support geleistet, aber wir verfolgen den Inhalt des Azure Stack-MSDN-Forums. Ordnen Sie in dieser Vorschauversion keine Produktionsworkloads an. Zwischen den Vorschauversionen von App Service in Azure Stack ist außerdem kein Upgrade möglich. Der Hauptzweck dieser Vorschauversionen besteht darin, zu zeigen, was von uns bereitgestellt wird. Außerdem möchten wir natürlich Feedback erhalten. 

## <a name="what-is-an-app-service-plan"></a>Was ist ein App Service-Plan?

Der App Service-Ressourcenanbieter verwendet den gleichen Code, der für Azure App Service genutzt wird. Daher ist es sinnvoll, einige gemeinsame Konzepte zu beschreiben. In App Service wird der Preiscontainer für Anwendungen als App Service-Plan bezeichnet. Er steht für die Gruppe von dedizierten virtuellen Computern, die zum Vorhalten Ihrer Apps verwendet werden. Ein bestimmtes Abonnement kann mehrere App Service-Pläne enthalten. 

In Azure werden freigegebene und dedizierte Worker verwendet. Ein freigegebener Worker unterstützt das mehrinstanzenfähige App-Hosting mit hoher Dichte, und es gibt nur eine Gruppe von freigegebenen Workern. Dedizierte Server werden nur von einem Mandanten verwendet und können drei Größen haben: Klein, Mittel und Groß. Die Anforderungen von lokalen Kunden können mit diesen Begriffen nicht immer beschrieben werden. In App Service in Azure Stack können Ressourcenanbieter-Administratoren die Workerebenen definieren, die sie verfügbar machen möchten. Administratoren können mehrere Gruppen mit freigegebenen Workern oder verschiedene Gruppen mit dedizierten Workern definieren. Dies richtet sich nach den jeweiligen speziellen Hostinganforderungen. Mit diesen Definitionen der Workerebenen können dann eigene Preis-SKUs definiert werden.

## <a name="portal-features"></a>Features im Portal

Für App Service in Azure Stack wird die gleiche Benutzeroberfläche wie für Azure App Service verwendet. Dies gilt auch für das Back-End. Einige Features sind deaktiviert und funktionieren in Azure Stack nicht. Die Azure-spezifischen Erwartungen bzw. Dienste, die für diese Features erforderlich sind, sind in Azure Stack noch nicht verfügbar. 

## <a name="next-steps"></a>Nächste Schritte

- [Vor den ersten Schritten mit App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md)
- [Hinzufügen eines App Service-Ressourcenanbieters zu Azure Stack](azure-stack-app-service-deploy.md)

Sie können auch andere [PaaS-Dienste (Platform-as-a-Service)](azure-stack-tools-paas-services.md) ausprobieren, z.B. den [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md) und den [MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md).

<!--Image references-->
[1]: ./media/azure-stack-app-service-overview/AppService_Portal.png

