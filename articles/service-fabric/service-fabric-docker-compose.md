---
title: Azure Service Fabric Docker Compose (Vorschau) | Microsoft-Dokumentation
description: "Azure Service Fabric akzeptiert das Docker Compose Format für eine einfachere Orchestrierung vorhandener Container mithilfe von Service Fabric. Diese Unterstützung befindet sich derzeit in der Vorschauphase."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 800f5bacd5197f64968fb1c169ef58330ee75e0d
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---


# <a name="compose-application-support-in-service-fabric-preview"></a>Compose-Anwendungsunterstützung in Service Fabric (Vorschau)

Docker verwendet die Datei [„docker-compose.yml“](https://docs.docker.com/compose) zum Definieren von Anwendungen mit mehreren Containern. Damit Kunden, die mit Docker vertraut sind, vorhandene Containeranwendungen einfacher in Service Fabric orchestrieren können, haben wir eine Unterstützung für die Vorschauversion von Docker Compose nativ in die Plattform integriert. Service Fabric akzeptiert Version 3 (und höher) von `docker-compose.yml`-Dateien. Da diese Unterstützung als Vorschauversion verfügbar ist, wird nur ein Teil der Compose-Direktiven unterstützt. So werden beispielsweise Anwendungsupgrades nicht unterstützt. Sie können jedoch jederzeit Anwendungen entfernen und bereitstellen, statt sie zu aktualisieren.  

Für die Verwendung dieser Vorschauversion müssen Sie das Vorschau-SDK (Version 255.255.x.x) über das Portal installieren. 

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase, sodass dafür kein Support bereitgestellt wird.

## <a name="using-a-docker-composeyml-file-with-service-fabric-preview"></a>Verwenden der Datei „docker-compose.yml“ mit Service Fabric (Vorschau)

Sie erstellen eine Service Fabric-Anwendung aus einer Datei „docker-compose.yml“, indem Sie den folgenden Befehl in PS ausführen:

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RepositoryUserName <>] [-RepositoryPassword <>] [-PasswordEnctypted]
```

RepositoryUserName und RepositoryPassword beziehen sich auf den Benutzernamen und das Kennwort der Containerregistrierung.

Falls Sie die Azure CLI 2.0 verwenden, führen Sie den folgenden Befehl aus:

```bash
az sf compose create --application-id fabric:/TestContainerApp --file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```
Diese Befehle erstellen eine Service Fabric-Anwendung (im vorherigen Beispiel mit dem Namen `fabric:/TestContainerApp`), die über Service Fabric Explorer überwacht werden kann. Der angegebene `ApplicationName` wird für Integritätsabfragen über PS, Azure CLI 2.0 oder SFX verwendet.

Um die Anwendung über PS zu löschen, verwenden Sie den folgenden Befehl:

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

Um die Anwendung über Azure CLI 2.0 zu löschen, verwenden Sie den folgenden Befehl:

```bash
az sf compose remove  --application-id TestContainerApp [ --timeout ]
```

Um den Status der Compose-Anwendung abzufragen, verwenden Sie den folgenden Befehl in PS:

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

Verwenden Sie für Azure CLI 2.0 den folgenden Befehl:

```bash
az sf compose status --application-id TestContainerApp [ --timeout ]
```



## <a name="supported-compose-directives"></a>Unterstützte Compose-Direktiven

In dieser Vorschauversion wird nur ein Teil der verfügbaren Konfigurationsoptionen im Compose V3-Format unterstützt. Die folgenden Grundtypen werden unterstützt:

* Services >Deploy > Replicas    
* Services > Deploy > Placement > Constraints    
* Services > Deploy > Resources > Limits 
*         -cpu-shares    
*         -memory
*         -memory-swap
* Services > Commands    
* Services > Environment
* Services > Ports    
* Services > Image
* Services > Isolation (nur für Windows)
* Services > Logging > Driver    
* Services > Logging > Driver > Options
* Volume & Deploy > Volume    

Der Cluster muss für das Erzwingen von Ressourcengrenzwerten wie in [Service Fabric-Ressourcenkontrolle](service-fabric-resource-governance.md) beschrieben eingerichtet werden.
Alle anderen Docker Compose-Direktiven werden in dieser Vorschauversion nicht unterstützt. 

## <a name="servicednsname-computation"></a>ServiceDnsName-Berechnung

Wenn der in der Compose-Datei angegebene Dienstname ein vollqualifizierter Domänenname ist (d.h., er enthält einen Punkt „.“), lautet der in Service Fabric registrierte DNS-Name `<ServiceName>` einschließlich des Punkts. Andernfalls wird jedes Pfadsegment im Anwendungsnamen eine Domänenbezeichnung im DNS-Namen des Diensts. Dabei wird das erste Pfadsegment die Bezeichnung für die Domäne der obersten Ebene. Wenn also der angegebene Anwendungsname `fabric:/SampleApp/MyComposeApp` lautet, wird `<ServiceName>.MyComposeApp.SampleApp` als DNS-Name registriert.

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a>Unterschiede zwischen dem Compose- (Instanzdefinition) und dem Service Fabric-Anwendungsmodell (Typdefinition)

Die Datei „docker-compose.yml“ beschreibt einen bereitstellbaren Satz von Containern, einschließlich ihrer Eigenschaften und Konfigurationen (z.B. Umgebungsvariablen und Ports). Bereitstellungsparameter wie Platzierungsbeschränkungen, Ressourcenlimits und DNS-Namen werden ebenfalls in der Datei „docker-compose.yml“ angegeben.

Das [Service Fabric-Anwendungsmodell](service-fabric-application-model.md) verwendet Dienst- und Anwendungstypen. Dabei können Sie über viele Anwendungsinstanzen desselben Typs verfügen (z.B. eine Anwendungsinstanz pro Kunde). Dieses typbasierte Modell unterstützt die Registrierung mehrerer Versionen desselben Anwendungstyps in der Runtime. Für Kunde A kann z.B. eine Anwendung mit Typ 1.0 von AppTypeA instanziiert werden, während für Kunde B eine andere Anwendung mit demselben Typ und derselben Version instanziiert wird. Die Anwendungstypen werden in Anwendungsmanifesten definiert. Der Anwendungsname und die Bereitstellungsparameter werden zum Zeitpunkt der Erstellung der Anwendung angegeben.

Während dieses Modell Flexibilität bietet, planen wir auch die Unterstützung eines einfacheren, instanzenbasierten Bereitstellungsmodells, bei dem sich die Typen implizit aus der Manifestdatei ergeben. In diesem Modell erhält jede Anwendung ein eigenes unabhängiges Manifest. Eine Vorschau dieser Vorgehensweise bieten wir durch die Unterstützung von „docker-compose.yml“-Dateien, einem instanzenbasierten Bereitstellungsformat.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Service Fabric-Anwendungsmodell](service-fabric-application-model.md).

