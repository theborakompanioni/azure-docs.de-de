---
title: Bereitstellen von Anwendungen auf Azure App Service.
description: Erfahren Sie, wie Sie Anwendungen in App Service bereitstellen
keywords: App Service, Azure App Service, bereitstellen, Bereitstellung
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: 
ms.assetid: de12cd6e-e124-4e48-90bc-c3a3801305da
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: dariagrigoriu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2778057e772fcba7e6021ac11ec5bdcf3720a703
ms.lasthandoff: 11/17/2016


---
# <a name="azure-app-service-deployment-overview"></a>Übersicht zur Azure App-Service Bereitstellung
Azure App Service bietet umfassende  integrierte Funktionen zum Erstellen von leistungsstarken und flexiblen Bereitstellungsworkflows. Die App-Bereitstellung kann Optionen für fortlaufende Integration oder Veröffentlichung über die lokale Quellcodeverwaltung sowie WebDeploy und FTP wiederverwenden. Die empfohlene Methode zum Bereitstellen von Produktions-Apps ist der Austausch eines Bereitstellungsslots. Bereitstellungsslots stellen Staging- und Integrationsumgebungen dar, die Produktions-Apps zugeordnet sind. Bereitstellungsslots können konfiguriert und für den Empfang von Webdatenverkehr zur Überprüfung eingerichtet werden und bei Bedarf auf Produktions-Bereitstellung umgestellt werden. Hierbei entstehen durch automatisiertes Anfahren keine Ausfallzeiten. Die Schritte des Bereitstellungsworkflows können einfach über Release Management-Produkte wie z. B. Visual Studio Release Management automatisiert werden. Dies ist nützlich für die Koordination mit anderen Lösungsressourcen (z. B. Datenspeicher), Wiederholung sowie Replikation über mehrere Einheiten der Bereitstellung. 

[!INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]


