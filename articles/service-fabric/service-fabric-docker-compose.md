---
title: Azure Service Fabric Docker Compose (Vorschau)
description: "Azure Service Fabric akzeptiert das Docker Compose-Format für eine einfachere Orchestrierung vorhandener Container mithilfe von Service Fabric. Diese Unterstützung befindet sich derzeit in der Vorschauphase."
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
ms.date: 07/06/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 91b6e98df5df98bb557d7fac0475354322d95640
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---
# <a name="compose-application-support-in-service-fabric-preview"></a>Compose-Anwendungsunterstützung in Service Fabric (Vorschau)

Docker verwendet die Datei [„docker-compose.yml“](https://docs.docker.com/compose) zum Definieren von Anwendungen mit mehreren Containern.
Damit Kunden, die mit Docker vertraut sind, vorhandene Containeranwendungen einfacher in Service Fabric orchestrieren können, haben wir eine Unterstützung für die Vorschauversion von Docker Compose nativ in die Plattform integriert. Service Fabric akzeptiert Version 3 (und höher) von `docker-compose.yml`-Dateien. Da diese Unterstützung als Vorschauversion verfügbar ist, wird nur ein Teil der Compose-Direktiven unterstützt. So werden beispielsweise Anwendungsupgrades nicht unterstützt. Sie können jedoch jederzeit Anwendungen entfernen und bereitstellen, statt sie zu aktualisieren.

Für die Verwendung dieser Vorschauversion müssen Sie Ihr Cluster mit dem Vorschau-SDK (Version 255.255.x.x) über das Portal erstellen. 

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase, sodass dafür kein Support bereitgestellt wird.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Bereitstellen einer Docker Compose-Datei in Service Fabric

Diese folgenden Befehle erstellen eine Service Fabric-Anwendung (die in vorherigem Beispiel mit `fabric:/TestContainerApp` benannt wurde), die wie jede Service Fabric-Anwendung verwaltet werden kann. Der angegebene Anwendungsname kann für Integritätsabfragen verwendet werden.

### <a name="using-powershell"></a>Verwenden von PowerShell

Erstellen Sie eine Service Fabric-Anwendung aus einer „docker-compose.yml“-Datei, indem Sie den folgenden Befehl in PS ausführen:

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RepositoryUserName <>] [-RepositoryPassword <>] [-PasswordEnctypted]
```

`RepositoryUserName` und `RepoistoryPassword` verweisen auf den Benutzernamen und das Kennwort der Containerregistrierung. Nachdem die Anwendung abgeschlossen wurde, können Sie den folgenden Befehl ausführen, um den Status der Anwendung zu überprüfen:

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

Um die Compose-Anwendung über PS zu löschen, verwenden Sie den folgenden Befehl:

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="using-azure-cli-20"></a>Mithilfe von Azure-CLI 2.0

Verwenden Sie alternativ den folgenden Azure-CLI-Befehl:

```azurecli
az sf compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

Nachdem die Anwendung erstellt wurde, können Sie den Status der Anwendung mithilfe des folgenden Befehls überprüfen:

```azurecli
az sf compose status --application-id TestContainerApp [ --timeout ]
```

Um die Compose-Anwendung zu löschen, verwenden Sie den folgenden Befehl:

```azurecli
az sf compose remove  --application-id TestContainerApp [ --timeout ]
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

Der Cluster muss für das Erzwingen von Ressourcengrenzwerten wie in [Service Fabric-Ressourcenkontrolle](service-fabric-resource-governance.md) beschrieben eingerichtet werden. Alle anderen Docker Compose-Direktiven werden in dieser Vorschauversion nicht unterstützt.

## <a name="servicednsname-computation"></a>ServiceDnsName-Berechnung

Wenn der in der Compose-Datei angegebene Dienstname ein vollqualifizierter Domänenname ist (d.h., er enthält einen Punkt „.“), lautet der in Service Fabric registrierte DNS-Name `<ServiceName>` einschließlich des Punkts. Andernfalls wird jedes Pfadsegment im Anwendungsnamen eine Domänenbezeichnung im DNS-Namen des Diensts. Dabei wird das erste Pfadsegment die Bezeichnung für die Domäne der obersten Ebene.

Wenn also der angegebene Anwendungsname z.B. `fabric:/SampleApp/MyComposeApp` lautet, wird `<ServiceName>.MyComposeApp.SampleApp` als DNS-Name registriert.

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a>Unterschiede zwischen dem Compose- (Instanzdefinition) und dem Service Fabric-Anwendungsmodell (Typdefinition)

Eine „docker-compose.yml“-Datei beschreibt einen bereitstellbaren Satz von Containern, einschließlich ihrer Eigenschaften und Konfigurationen.
Die Datei kann z.B. Umgebungsvariablen und Ports enthalten. Bereitstellungsparameter wie Platzierungsbeschränkungen, Ressourcenlimits und DNS-Namen können ebenfalls in der Datei „docker-compose.yml“ angegeben werden.

Das [Service Fabric-Anwendungsmodell](service-fabric-application-model.md) verwendet Dienst- und Anwendungstypen. Dabei können Sie über viele Anwendungsinstanzen desselben Typs verfügen. Sie können beispielsweise eine Anwendungsinstanz pro Kunde haben. Dieses typbasierte Modell unterstützt die Registrierung mehrerer Versionen desselben Anwendungstyps in der Runtime.
Für Kunde A kann z.B. eine Anwendung mit Typ 1.0 von AppTypeA instanziiert werden, während für Kunde B eine andere Anwendung mit demselben Typ und derselben Version instanziiert wird. Die Anwendungstypen werden in Anwendungsmanifesten definiert. Der Anwendungsname und die Bereitstellungsparameter werden zum Zeitpunkt der Erstellung der Anwendung angegeben.

Während dieses Modell Flexibilität bietet, planen wir auch die Unterstützung eines einfacheren, instanzenbasierten Bereitstellungsmodells, bei dem sich die Typen implizit aus der Manifestdatei ergeben. In diesem Modell erhält jede Anwendung ein eigenes unabhängiges Manifest. Eine Vorschau dieser Vorgehensweise bieten wir durch die Unterstützung von „docker-compose.yml“-Dateien, einem instanzenbasierten Bereitstellungsformat.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Service Fabric-Anwendungsmodell](service-fabric-application-model.md).

## <a name="related-articles"></a>Verwandte Artikel

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Erste Schritte mit Service Fabric und der Azure CLI 2.0)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

