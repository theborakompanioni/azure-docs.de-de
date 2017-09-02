---
title: Azure-Containerregistrierungsrepositorys | Microsoft-Dokumentation
description: "Verwenden der Azure-Containerregistrierungsrepositorys für Docker-Images"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: dd4feff057269ed7106990bb63eed7fcffa2dbec
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017

---
# <a name="azure-container-registry-repositories"></a>Azure-Containerregistrierungsrepositorys

Azure-Containerregistrierungen sind mit einer Vielzahl von Diensten und Orchestratoren kompatibel. Damit Sie die Quelldienste und -agents einfacher verfolgen können, von denen aus ACR verwendet wird, haben wir angefangen, das Docker-Headerfeld in der Datei „Docker.config“ zu verwenden.



## <a name="viewing-repositories-in-the-portal"></a>Anzeigen von Repositorys im Portal

Die ACR-Header weisen folgendes Format auf:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Cloud: Azure, Azure Stack oder andere Government- oder länderspezifische Azure-Clouds. Azure Stack- und Government-Clouds werden derzeit noch nicht unterstützt. Dieser Parameter ermöglicht allerdings die zukünftige Unterstützung.
* Service: Der Name des Diensts.
* Optionalservicename: Ein optionaler Parameter für Dienste mit untergeordneten Diensten oder das Angeben einer SKU (Web-Apps korrespondieren z.B. mit Azure/app-service/web-apps).

Es wird empfohlen, dass Partnerdienste und -orchestratoren spezifische Headerwerte verwenden, um unsere Telemetrie zu unterstützen. Benutzer können den Wert, der an den Header übergeben wird, bei Bedarf ändern.

Hier finden Sie die Werte, die ACR-Partner verwenden sollen, um das Feld „X-Meta-Source-Client“ aufzufüllen:

| Service Name              | Header                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | azure/compute/azure-container-service |
| App Service – Web-Apps    | azure/app-service/web-apps            |
| App Service – Logic Apps  | azure/app-service/logic-apps          |
| Batch                     | azure/compute/batch                   |
| Cloud Console             | azure/cloud-console                   |
| Functions                 | azure/compute/functions               |
| Internet der Dinge (IoT) – Hub  | azure/iot/hub                         |
| HDInsight                 | azure/data/hdinsight                  |
| Jenkins                   | azure/jenkins                         |
| Machine Learning          | azure/data/machile-learning           |
| Service Fabric            | azure/compute/service-fabric          |
| VSTS                      | azure/vsts                            |


## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zu Registrierungen und den unterstützten Diensten und Orchestratoren](container-registry-intro.md)

