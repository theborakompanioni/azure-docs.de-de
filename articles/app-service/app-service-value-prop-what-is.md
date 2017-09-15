---
title: "Azure App Service für Web-Apps, mobile und API-Apps | Microsoft Docs"
description: Es wird beschrieben, wie Sie Azure App Service verwenden, um Web-Apps und mobile Apps zu entwickeln, bereitzustellen und zu verwalten.
keywords: App Service, Azure App Service, App Service-Kosten, Skalierung, skalierbar, App-Bereitstellung, Azure-App-Bereitstellung, PaaS, Platform as a Service, Website, Web Site, Web, Azure Mobile
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 94869072a38be61819213fbf4f919764fe1849c0
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="what-is-azure-app-service"></a>Was ist Azure App Service?
*App Service* ist ein [Platform-as-a-Service](https://en.wikipedia.org/wiki/Platform_as_a_service) -Angebot (PaaS) von Microsoft Azure. Erstellen Sie Web-Apps und mobile Apps für alle Plattformen oder Geräte. Integrieren Sie Ihre Apps in SaaS-Lösungen, stellen Sie eine Verbindung mit lokalen Anwendungen her, und automatisieren Sie Ihre Geschäftsprozesse. Azure führt Ihre Apps auf vollständig verwalteten virtuellen Computern (VMs) mit den freigegebenen VM-Ressourcen oder dedizierten VMs aus.

App Service umfasst die Webfunktionen und mobilen Funktionen, die wir vorher separat als Azure Websites und Azure Mobile Services bereitgestellt haben. Außerdem sind neue Funktionen zum Automatisieren von Geschäftsprozessen und Hosten von Cloud-APIs enthalten. Als einzelner integrierter Dienst können Sie mit App Service verschiedene Komponenten – Websites, Back-Ends für mobile Apps, RESTful-APIs und Geschäftsprozesse – in einer zentralen Lösung zusammenstellen.

Das folgende vierminütige Video enthält eine kurze Erklärung dazu, wie sich App Service gegenüber früheren Azure-Angeboten verhält und welche neuen Funktionen es gibt.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Gründe für die Verwendung von App Service
Hier sind einige wichtige Features und Funktionen von App Service aufgeführt:

* **Mehrere Sprachen und Frameworks** : App Service verfügt über erstklassige Unterstützung für ASP.NET, Node.js, Java, PHP und Python. Außerdem können Sie [Windows PowerShell und andere Skripts oder ausführbare Dateien](../app-service-web/web-sites-create-web-jobs.md) auf App Service-VMs ausführen.
* **DevOps-Optimierung** : Richten Sie mit Visual Studio Team Services, GitHub oder BitBucket die [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) ein. Stufen Sie Updates über [Test- und Stagingumgebungen](../app-service-web/web-sites-staged-publishing.md)herauf. Verwalten Sie Ihre Apps in App Service mithilfe von [Azure PowerShell](/powershell/azureps-cmdlets-docs) oder der [plattformübergreifenden Befehlszeilenschnittstelle (Command-Line Interface, CLI)](../cli-install-nodejs.md).
* **Globale Skalierung mit hoher Verfügbarkeit** – Skalieren Sie manuell oder automatisch [zentral](../app-service-web/web-sites-scale.md) oder [horizontal](../monitoring-and-diagnostics/insights-how-to-scale.md) hoch. Hosten Sie Ihre Apps überall in der globalen Rechenzentrumsinfrastruktur von Microsoft. Die App Service-[SLA](https://azure.microsoft.com/support/legal/sla/app-service/) garantiert eine hohe Verfügbarkeit.
* **Verbindungen mit SaaS-Plattformen und lokalen Daten** – Wählen Sie aus über 50 [Connectors](../connectors/apis-list.md) für Unternehmenssysteme (z.B. SAP, Siebel und Oracle), SaaS-Dienste (z.B. Salesforce und Office 365) sowie Internetdienste (z.B. Facebook und Twitter). Greifen Sie über [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md) auf lokale Daten zu.
* **Sicherheit und Compliance** – App Service ist [ISO-, SOC- und PCI-konform](https://www.microsoft.com/TrustCenter/).
* **Anwendungsvorlagen** – Wählen Sie im [Azure Marketplace](https://azure.microsoft.com/marketplace/) aus zahlreichen Vorlagen, die die Verwendung eines Assistenten zum Installieren gängiger Open Source-Software wie WordPress, Joomla und Drupal ermöglichen.
* **Visual Studio-Integration** – Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen und Debuggen.

## <a name="app-types-in-app-service"></a>App-Typen in App Service
App Service verfügt über mehrere *App-Typen*, die jeweils zum Hosten einer bestimmten Workload bestimmt sind:

* [**Web-Apps**](../app-service-web/app-service-web-overview.md): Hosten von Websites und Webanwendungen
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md): Hosten von Back-Ends für mobile Apps
* [**API-Apps**](../app-service-api/app-service-api-apps-why-best-platform.md): Hosten von RESTful-APIs
* [**Logik-Apps**](../logic-apps/logic-apps-what-are-logic-apps.md): Automatisierung von Geschäftsprozessen und Integration von Systemen und Daten in Clouds, ohne Code zu schreiben

Der Begriff *App* bezieht sich hier auf die Hostingressourcen, die dediziert für die Ausführung einer Workload bestimmt sind. Wenn wir „Web-App“ als Beispiel nehmen, verstehen Sie darunter wahrscheinlich sowohl die Computeressourcen als auch die Anwendungscodeelemente, die zusammen für einen Browser die Funktionalität bereitstellen. In App Service handelt es sich bei einer *Web-App* aber um die Computeressourcen, die von Azure zum Hosten Ihres Anwendungscodes bereitgestellt werden. 

Die Anwendung kann aus mehreren App Service-Apps unterschiedlicher Art bestehen. Wenn Ihre Anwendung also beispielsweise aus einem Web-Front-End und einem RESTful-API-Back-End besteht, haben Sie folgende Möglichkeiten:

- Sie können beides (Front-End und API) für eine einzelne Web-App bereitstellen.  
- Sie können Ihren Front-End-Code für eine Web-App und Ihren Back-End-Code für eine API-App bereitstellen. 



## <a name="app-service-plans"></a>App Service-Pläne
[App Service-Pläne](azure-web-sites-web-hosting-plans-in-depth-overview.md) stellen die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden.

In App Service-Plänen wird Folgendes definiert:

- **Region** („USA, Westen“, „USA, Osten“ usw.)
- **Skalierung** (Instanzenanzahl)
- **Instanzgröße** (klein, mittel, groß)
- **SKU** (Free, Shared, Basic, Standard, Premium)

Alle einem **App Service-Plan** zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Dies spart Kosten, wenn Sie mehrere Apps hosten.

Ihr **App Service-Plan** kann von den SKUs **Free** und **Shared** auf die SKUs **Basic**, **Standard** und **Premium** skaliert werden, um Zugriff auf weitere Ressourcen und Features zu erhalten. Wenn Ihr App Service-Plan mindestens auf **Basic** festgelegt ist, können Sie auch die **Größe** und die Skalierung der virtuellen Computer steuern.

**SKU** und **Skalierung** des App Service-Plans haben Einfluss auf die Kosten (nicht auf die Anzahl gehosteter Apps). 

Wenn Sie eine bessere Skalierbarkeit und Netzwerkisolation benötigen, können Sie die Apps in einer [App Service-Umgebung](../app-service-web/app-service-app-service-environment-intro.md)ausführen.

## <a name="pricing"></a>Preise
Weitere Informationen zu den Kosten von App Service finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Testen von App Service
[Hier können Sie problemlos eine Beispiel-Web-App, mobile App oder Logik-App erstellen](https://azure.microsoft.com/try/app-service/) und eine Stunde lang ausprobieren – ohne Kreditkarte oder Verpflichtungen.

Oder eröffnen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), und probieren Sie eines unserer Tutorials für die ersten Schritte aus:

* [Tutorial: Erstellen einer Web-App](../app-service-web/app-service-web-get-started.md)
* [Tutorial: Erstellen einer mobilen App](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Tutorial: Erstellen einer API-App](../app-service-api/app-service-api-dotnet-get-started.md)
* [Tutorial: Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)


