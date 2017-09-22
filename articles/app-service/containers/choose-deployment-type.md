---
title: "Bereitstellung von Azure-Web-App unter Linux – benutzerdefiniertes Image oder integriertes Plattformimage?  | Microsoft Docs"
description: "Erwägungen bei der Entscheidung zwischen benutzerdefinierter Docker-Containerbereitstellung und einem integrierten Anwendungsframework"
keywords: Azure App Service, Web-App, Linux, OSS
services: app-service
documentationCenter: 
authors: nickwalk
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: nickwalk
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 322533a9dce54222943f8b2a3d653780a674c8a4
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="custom-image-or-built-in-platform-image"></a>Benutzerdefiniertes Image oder integriertes Plattformimage?

[Web-App unter Linux](app-service-linux-intro.md) bietet zwei verschiedene Pfade für die Veröffentlichung Ihrer Anwendung im Web:

- **Bereitstellung mit benutzerdefinierten Images:** Wandeln Sie Ihre App in ein Docker-Image um, das alle Dateien und Abhängigkeiten in einem ausführbereiten Paket enthält.
- **App-Bereitstellung mit einem integrierten Plattformimage:** Unsere integrierten Plattformimages enthalten allgemeine Web-App-Runtimes und Abhängigkeiten wie Node und PHP. Verwenden Sie eine der [Azure App Service-Bereitstellungsmethoden](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) zum Bereitstellen Ihrer App im Speicher Ihrer Web-App, und führen Sie sie dann mithilfe eines integrierten Plattformimages aus.

Welche Methode ist für Ihre App geeignet? Hauptsächlich sind folgende Faktoren zu berücksichtigen:

- **Verfügbarkeit von Docker in Ihrem Entwicklungsworkflow:** Für die Entwicklung benutzerdefinierter Images sind grundlegende Kenntnisse des Docker-Entwicklungsworkflows erforderlich. Zur Bereitstellung eines benutzerdefinierten Images für eine Web-App muss das benutzerdefinierte Image auf einem Repositoryhost wie Docker Hub veröffentlicht werden. Wenn Sie mit Docker vertraut sind und Ihrem Buildworkflow Docker-Aufgaben hinzufügen können oder wenn Sie Ihre App bereits als Docker-Image veröffentlichen, ist ein benutzerdefiniertes Image nahezu immer die beste Wahl.
- **Besondere Laufzeitanforderungen:** Die integrierten Plattformimages sind dafür vorgesehen, die Anforderungen der meisten Web-Apps zu erfüllen, können aber nur begrenzt angepasst werden. Möglicherweise weist Ihre App besondere Abhängigkeiten oder andere Laufzeitanforderungen auf, die die Möglichkeiten integrierter Images überschreiten.
- **Buildanforderungen:** Mit [Continuous Deployment](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) können Sie Ihre App in Azure direkt aus dem Quellcode ausführen. Es ist kein externer Build- oder Veröffentlichungsprozess erforderlich. Die Anpassbarkeit und die Verfügbarkeit von Buildtools innerhalb des [Kudu](https://github.com/projectkudu/kudu/wiki)-Bereitstellungsmoduls sind jedoch begrenzt. Ihre App kann die Möglichkeiten von Kudu überschreiten, wenn ihre Abhängigkeiten oder Anforderungen an benutzerdefinierte Buildlogik steigen.
- **Anforderungen für Lese-/Schreibzugriff auf den Datenträger:** Allen Web-Apps wird ein Speichervolume für Webinhalte zugeordnet. Dieses durch Azure Storage abgesicherte Volume wird im App-Dateisystem unter `/home` eingebunden. Im Gegensatz zu den Dateien im Containerdateisystem kann auf Dateien im Inhaltsvolume über alle Skalierungsinstanzen einer App zugegriffen werden, und Änderungen bleiben nach einem Neustart der App erhalten. Die Datenträgerlatenz des Inhaltsvolumes ist jedoch größer und variabler als die Latenz des lokalen Containerdateisystems, und der Zugriff kann durch Plattformupgrades, ungeplante Ausfallzeiten und Probleme mit der Netzwerkkonnektivität beeinträchtigt werden. Für Apps, die umfassenden Lesezugriff auf Inhaltsdateien erfordern, ist eine Bereitstellung über ein benutzerdefiniertes Images möglicherweise von Vorteil, da hierbei die Dateien im Imagedateisystem platziert werden und nicht auf dem Inhaltsvolume.
- **Nutzung von Buildressourcen:** Wenn eine App aus dem Quellcode bereitgestellt wird, verwenden die von Kudu ausgeführten Bereitstellungsskripts dieselben Compute- und Speicherressourcen des App Service-Plans wie die laufende App. Große App-Bereitstellungen verbrauchen möglicherweise mehr Ressourcen oder Zeit als gewünscht. Insbesondere erzeugen viele Bereitstellungsworkflows starke Datenträgeraktivität auf dem App-Inhaltsvolume, das für Aktivitäten dieser Art nicht optimiert ist. Ein benutzerdefiniertes Image stellt alle Dateien und Abhängigkeiten Ihrer App in einem einzelnen Paket für Azure bereit, ohne dass weitere Dateiübertragungen oder Bereitstellungsaktionen erforderlich sind.
- **Anforderung für schnelle Iteration:** Das Umwandeln einer App in ein Docker-Image erfordert zusätzliche Schritte. Damit die Änderungen wirksam werden, müssen Sie das neue Image bei jedem Update mithilfe von Push in ein Repository übertragen. Diese Updates werden dann per Pull in die Azure-Umgebung übernommen. Wenn einer der integrierten Container die Anforderungen Ihrer App erfüllt, kann die Bereitstellung aus dem Quellcode einen schnelleren Entwicklungsworkflow bieten.
