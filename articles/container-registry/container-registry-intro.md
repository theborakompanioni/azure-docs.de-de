---
title: Private Docker-Containerregistrierungen in Azure | Microsoft-Dokumentation
description: "Enthält eine Einführung in den Dienst „Azure-Containerregistrierung“ und die Bereitstellung von cloudbasierten, verwalteten, privaten Docker-Registrierungen."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: bc0c8ccad64166582dcb3f7162280a8772abc6b3
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---
# <a name="introduction-to-private-docker-container-registries"></a>Einführung in private Docker-Containerregistrierungen


Die Azure-Containerregistrierung ist ein verwalteter Dienst vom Typ [Docker-Registrierung](https://docs.docker.com/registry/), der auf Version 2.0 der Open Source-Docker-Registrierung basiert. Erstellen und verwalten Sie Azure-Containerregistrierungen, um Ihre privaten [Docker-Container](https://www.docker.com/what-docker)images zu speichern und zu verwalten. Verwenden Sie Containerregistrierungen in Azure mit Ihren vorhandenen Containerentwicklungs- und Bereitstellungspipelines, und nutzen Sie das umfangreiche Wissen der Docker-Community.

Hintergrundinformationen zu Docker und Containern finden Sie unter:

* [Docker-Benutzerhandbuch](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Anwendungsfälle
Rufen Sie Images aus einer Azure-Containerregistrierung für verschiedene Bereitstellungsziele ab:

* **Skalierbare Orchestrierungssysteme** zum Verwalten von Anwendungen in Containern über Cluster mit Hosts hinweg, z.B. [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) und [Kubernetes](http://kubernetes.io/docs/).
* **Azure-Dienste**, die die bedarfsorientierte Erstellung und Ausführung von Anwendungen unterstützen, z.B. [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](../service-fabric/index.md) und andere.

Entwickler können im Rahmen eines Workflows der Containerentwicklung auch eine Pushübertragung in eine Containerregistrierung durchführen. Sie können Daten beispielsweise mit einem Tool für Continuous Integration und Entwicklung an eine Containerregistrierung wie z.B. [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) oder [Jenkins](https://jenkins.io/) übertragen.





## <a name="key-concepts"></a>Wichtige Begriffe
* **Registrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine oder mehrere Containerregistrierungen. Jede Registrierung verfügt über ein standardmäßiges Azure-[Speicherkonto](../storage/storage-introduction.md) an demselben Standort. Nutzen Sie den lokalen Speicher in räumlicher Nähe zu Ihren Containerimages, indem Sie eine Registrierung an demselben Azure-Standort wie Ihre Bereitstellungen erstellen. Ein vollständig qualifizierter Registrierungsname hat folgendes Format: `myregistry.azurecr.io`.

  Sie [steuern den Zugriff](container-registry-authentication.md) auf eine Containerregistrierung mit einem auf Azure Active Directory basierenden [Dienstprinzipal](../active-directory/active-directory-application-objects.md) oder einem bereitgestellten Administratorkonto. Führen Sie den Standardbefehl `docker login` aus, um die Authentifizierung für eine Registrierung durchzuführen.

* **Repository**: Eine Registrierung enthält mindestens ein Repository, also eine Gruppe von Containerimages. Die Azure-Containerregistrierung unterstützt Repositorynamespaces mit mehreren Ebenen. Mit dieser Funktion können Sie Sammlungen mit Images für eine bestimmte App oder eine Sammlung von Apps für bestimmte Entwicklungs- oder Betriebsteams gruppieren. Beispiel:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` stellt ein unternehmensweites Image dar.
  * `myregistry.azurecr.io/warrantydept/dotnet-build` stellt ein Image dar, das zum Erstellen von .NET-Apps verwendet wird, die für die Garantieabteilung freigegeben werden.
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web` stellt ein Webimage dar, das in der App „customersubmissions“ gruppiert und im Besitz der Garantieabteilung ist.

* **Image**: Jedes Image wird in einem Repository gespeichert und ist eine schreibgeschützte Momentaufnahme eines Docker-Containers. Azure-Containerregistrierungen können sowohl Windows- als auch Linux-Images enthalten. Sie steuern Imagenamen für alle Containerbereitstellungen. Verwenden Sie [Docker-Standardbefehle](https://docs.docker.com/engine/reference/commandline/), um Images in ein Repository zu übertragen (Push) oder ein Image aus einem Repository abzurufen (Pull).

* **Container**: Ein Container definiert eine Softwareanwendung und ihre Abhängigkeiten innerhalb eines vollständigen Dateisystems, einschließlich Code, Laufzeit, Systemtools und Bibliotheken. Führen Sie Docker-Container basierend auf Windows- oder Linux-Images aus, die Sie aus einer Containerregistrierung abrufen. Für Container, die auf demselben Computer ausgeführt werden, wird derselbe Betriebssystemkernel genutzt. Docker-Container sind für alle wichtigen Linux-Distributionen und Macintosh- und Windows-Betriebssysteme vollständig portabel.




## <a name="next-steps"></a>Nächste Schritte
* [Erstellen einer Containerregistrierung mit dem Azure-Portal](container-registry-get-started-portal.md)
* [Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md)
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)
* Informationen zum Erstellen eines Continuous Integration- und Bereitstellungsworkflows mithilfe von Visual Studio Team Services, Azure Container Service und der Azure-Containerregistrierung finden Sie in [diesem Tutorial](../container-service/container-service-docker-swarm-setup-ci-cd.md).
* Informationen zum Einrichten Ihrer privaten Docker-Registrierung in Azure (ohne öffentlichen Endpunkt) finden Sie unter [Bereitstellen einer privaten Docker-Registrierung in Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).

