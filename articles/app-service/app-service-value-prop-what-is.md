---
title: Azure App Service für Web-Apps und mobile Apps | Microsoft Docs
description: Es wird beschrieben, wie Sie Azure App Service verwenden, um Web-Apps und mobile Apps zu entwickeln, bereitzustellen und zu verwalten.
keywords: App Service, Azure App Service, App Service-Kosten, Skalierung, skalierbar, App-Bereitstellung, Azure-App-Bereitstellung, PaaS, Platform as a Service
services: app-service
documentationcenter: ''
author: omarkmsft
manager: dwrede
editor: jimbe

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2016
ms.author: omark

---
# Was ist Azure App Service?
*App Service* ist ein [Platform-as-a-Service](https://en.wikipedia.org/wiki/Platform_as_a_service)-Angebot (PaaS) von Microsoft Azure. Erstellen Sie Web-Apps und mobile Apps für alle Plattformen oder Geräte. Integrieren Sie Ihre Apps in SaaS-Lösungen, stellen Sie eine Verbindung mit lokalen Anwendungen her, und automatisieren Sie Ihre Geschäftsprozesse. Azure führt Ihre Apps auf vollständig verwalteten virtuellen Computern (VMs) mit den freigegebenen VM-Ressourcen oder dedizierten VMs aus.

App Service umfasst die Webfunktionen und mobilen Funktionen, die wir vorher separat als Azure Websites und Azure Mobile Services bereitgestellt haben. Außerdem sind neue Funktionen zum Automatisieren von Geschäftsprozessen und Hosten von Cloud-APIs enthalten. Als einzelner integrierter Dienst können Sie mit App Service verschiedene Komponenten – Websites, Back-Ends für mobile Apps, RESTful-APIs und Geschäftsprozesse – in einer zentralen Lösung zusammenstellen.

Das folgende vierminütige Video enthält eine kurze Erklärung dazu, wie sich App Service gegenüber früheren Azure-Angeboten verhält und welche neuen Funktionen es gibt.

\+[AZURE.VIDEO app-service-history-lesson] 

## Gründe für die Verwendung von App Service
Hier sind einige wichtige Features und Funktionen von App Service aufgeführt:

* **Mehrere Sprachen und Frameworks**: App Service verfügt über erstklassige Unterstützung für ASP.NET, Node.js, Java, PHP und Python. Außerdem können Sie [Windows PowerShell und andere Skripts oder ausführbare Dateien](../app-service-web/web-sites-create-web-jobs.md) auf App Service-VMs ausführen.
* **DevOps-Optimierung**: Richten Sie mit Visual Studio Team Services, GitHub oder BitBucket die [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) ein. Stufen Sie Updates über [Test- und Stagingumgebungen](../app-service-web/web-sites-staged-publishing.md) herauf. Führen Sie [A/B-Tests](../app-service-web/app-service-web-test-in-production-get-start.md) aus. Verwalten Sie Ihre Apps in App Service mithilfe von [Azure PowerShell](../powershell-install-configure.md) oder der [plattformübergreifenden Befehlszeilenschnittstelle (Command-Line Interface, CLI)](../xplat-cli-install.md).
* **Globale Skalierung mit hoher Verfügbarkeit** – Skalieren Sie manuell oder automatisch [zentral](../app-service-web/web-sites-scale.md) oder [horizontal](../azure-portal/insights-how-to-scale.md) hoch. Hosten Sie Ihre Apps überall in der globalen Rechenzentrumsinfrastruktur von Microsoft. Die App Service-Vereinbarung zum Servicelevel ([SLA](https://azure.microsoft.com/support/legal/sla/app-service/)) garantiert eine hohe Verfügbarkeit.
* **Verbindungen mit SaaS-Plattformen und lokalen Daten** – Wählen Sie aus über 50 [Connectors](../connectors/apis-list.md) für Unternehmenssysteme (z.B. SAP, Siebel und Oracle), SaaS-Diensten (z.B. Salesforce und Office 365) sowie Internetdiensten (z.B. Facebook und Twitter). Greifen Sie über [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md) auf lokale Daten zu.
* **Sicherheit und Compliance** – App Service bietet [ISO-, SOC- und PCI-Konformität](https://www.microsoft.com/TrustCenter/).
* **Anwendungsvorlagen** – Wählen Sie in [Azure Marketplace](https://azure.microsoft.com/marketplace/) aus zahlreichen Anwendungsvorlagen, die die Verwendung eines Assistenten zum Installieren gängiger Open Source-Software wie WordPress, Joomla und Drupal ermöglichen.
* **Visual Studio-Integration** – Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen, Debuggen und Verwalten von API-Apps.

## App-Typen in App Service
App Service verfügt über mehrere *App-Typen*, die jeweils zum Hosten einer bestimmten Art von Workload bestimmt sind:

* [**Web-Apps**](../app-service-web/app-service-web-overview.md): Hosten von Websites und Webanwendungen
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md): Hosten von Back-Ends für mobile Apps
* [**API-Apps**](../app-service-api/app-service-api-apps-why-best-platform.md): Hosten von Cloud-APIs
* [**Logik-Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md): Automatisieren des Zugriffs und der cloudübergreifenden Nutzung von Daten, ohne Code schreiben zu müssen

Der Begriff *App* bezieht sich hier auf die Hostingressourcen, die dediziert für die Ausführung einer Workload bestimmt sind. Wenn wir „Web-App“ als Beispiel nehmen, verstehen Sie darunter wahrscheinlich sowohl die Computeressourcen als auch die Anwendungscodeelemente, die zusammen für einen Browser die Funktionalität bereitstellen. In App Service handelt es sich bei einer *Web-App* aber um die Computeressourcen, die von Azure zum Hosten Ihres Anwendungscodes bereitgestellt werden. Wenn Ihre Anwendung aus einem Web-Front-End und einem RESTful-API-Back-End besteht, können Sie sowohl die Bereitstellung für eine Web-App durchführen als auch den Front-End-Code für eine Web-App und den Back-End-Code für eine API-App bereitstellen. Die Anwendung kann aus mehreren App Service-Apps unterschiedlicher Art bestehen.

## App Service-Pläne
In [App Service-Plänen](azure-web-sites-web-hosting-plans-in-depth-overview.md) werden die Arten von Computeressourcen angegeben, auf denen die App ausgeführt wird. Wenn Sie ein geringes Aufkommen an Datenverkehr erwarten, können Sie freigegebene virtuelle Computer (VMs) verwenden. Bei höherem Aufkommen können Sie zwischen verschiedenen Größen von dedizierten VMs wählen. Mehrere App Service-Apps können einen Plan gemeinsam nutzen und werden zusammen mit dem Plan zentral hoch- und herunterskaliert.

Wenn Sie eine bessere Skalierbarkeit und Netzwerkisolation benötigen, können Sie die Apps in einer [App Service-Umgebung](../app-service-web/app-service-app-service-environment-intro.md) ausführen.

## Preise
Weitere Informationen zu den Kosten von App Service finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/app-service/).

## Erste Schritte mit App Service
[Hier können Sie schnell und kostenlos eine temporäre Web-App, mobile App oder Logik-App erstellen](http://go.microsoft.com/fwlink/?LinkId=523751) – keine Kreditkarte erforderlich, keine Verpflichtungen, kein Ärger.

Oder eröffnen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), und probieren Sie eines unserer Tutorials für die ersten Schritte aus:

* [Tutorial: Erstellen einer Web-App](../app-service-web/app-service-web-get-started.md)
* [Tutorial: Erstellen einer mobilen App](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Tutorial: Erstellen einer API-App](../app-service-api/app-service-api-dotnet-get-started.md)
* [Tutorial: Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0803_2016-->