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
ms.date: 09/25/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: e05d1a3d6111e3bbc34008226bcd1fdf35935450
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a>Docker Compose-Anwendungsunterstützung in Service Fabric (Vorschauversion)

Docker verwendet die Datei [„docker-compose.yml“](https://docs.docker.com/compose) zum Definieren von Anwendungen mit mehreren Containern. Damit Kunden, die mit Docker vertraut sind, vorhandene Containeranwendungen einfacher in Service Fabric orchestrieren können, haben wir eine Unterstützung für die Vorschauversion von Docker Compose nativ in die Plattform integriert. Service Fabric akzeptiert `docker-compose.yml`-Dateien ab Version 3 (und höher). 

Da diese Unterstützung als Vorschauversion verfügbar ist, wird nur ein Teil der Compose-Direktiven unterstützt. So werden beispielsweise Anwendungsupgrades nicht unterstützt. Sie können jedoch jederzeit Anwendungen entfernen und bereitstellen, statt sie zu aktualisieren.

Wenn Sie diese Vorschauversion verwenden möchten, erstellen Sie Ihren Cluster mindestens mit der Version 5.7 der Service Fabric-Laufzeit über das Azure-Portal (zusammen mit dem entsprechenden SDK). 

> [!NOTE]
> Das Feature befindet sich derzeit in der Vorschauphase und wird in Produktionsumgebungen nicht unterstützt.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Bereitstellen einer Docker Compose-Datei in Service Fabric

Die folgenden Befehle erstellen eine Service Fabric-Anwendung (die in vorherigem Beispiel mit `fabric:/TestContainerApp` benannt wurde), die wie jede andere Service Fabric-Anwendung überwacht und verwaltet werden kann. Sie können den jeweiligen Anwendungsnamen für Integritätsabfragen verwenden.

### <a name="use-powershell"></a>Verwenden von PowerShell

Erstellen Sie eine Service Fabric-Anwendung aus einer „docker-compose.yml“-Datei, indem Sie den folgenden Befehl in PowerShell ausführen:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName` und `RegistryPassword` verweisen auf den Benutzernamen und das Kennwort der Containerregistrierung. Nachdem Sie die Anwendung fertiggestellt haben, können Sie ihren Status mit folgendem Befehl überprüfen:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName fabric:/TestContainerApp -GetAllPages
```

Um die Compose-Anwendung über PowerShell zu löschen, verwenden Sie den folgenden Befehl:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Verwenden der Azure Service Fabric CLI (sfctl)

Verwenden Sie alternativ den folgenden Service Fabric CLI-Befehl:

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

Nachdem Sie die Anwendung erstellt haben, können Sie ihren Status mit folgendem Befehl überprüfen:

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

Um die Compose-Anwendung zu löschen, verwenden Sie den folgenden Befehl:

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a>Unterstützte Compose-Direktiven

Diese Vorschauversion unterstützt nur einen Teil der verfügbaren Konfigurationsoptionen im Compose V3-Format, einschließlich der folgenden Grundtypen:

* Dienste > Bereitstellen > Replikate
* Dienste > Bereitstellen > Platzierung > Einschränkungen
* Dienste > Bereitstellen > Ressourcen > Grenzwerte
    * -cpu-shares
    * -memory
    * -memory-swap
* Dienste > Befehle
* Dienste > Umgebung
* Dienste > Ports
* Dienste > Image
* Dienste > Isolation (nur für Windows)
* Dienste > Protokollieren > Treiber
* Dienste > Logging > Treiber > Optionen
* Volumen & Bereitstellen > Volumen

Der Cluster muss für das Erzwingen von Ressourcengrenzwerten wie in [Service Fabric-Ressourcengovernance](service-fabric-resource-governance.md) beschrieben eingerichtet werden. Alle anderen Docker Compose-Direktiven werden in dieser Vorschauversion nicht unterstützt.

## <a name="servicednsname-computation"></a>ServiceDnsName-Berechnung

Wenn der in der Compose-Datei angegebene Dienstname ein vollqualifizierter Domänenname ist (d.h., er enthält einen Punkt [.]), lautet der in Service Fabric registrierte DNS-Name `<ServiceName>` einschließlich des Punkts. Andernfalls wird jedes Pfadsegment im Anwendungsnamen eine Domänenbezeichnung im DNS-Namen des Diensts. Dabei wird das erste Pfadsegment die Bezeichnung für die Domäne der obersten Ebene.

Wenn also der angegebene Anwendungsname z.B. `fabric:/SampleApp/MyComposeApp` lautet, wird `<ServiceName>.MyComposeApp.SampleApp` als DNS-Name registriert.

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a>Unterschiede zwischen dem Compose- (Instanzdefinition) und dem Service Fabric-Anwendungsmodell (Typdefinition)

Eine „docker-compose.yml“-Datei beschreibt einen bereitstellbaren Satz von Containern, einschließlich ihrer Eigenschaften und Konfigurationen.
Die Datei kann z.B. Umgebungsvariablen und Ports enthalten. Bereitstellungsparameter wie Platzierungsbeschränkungen, Ressourcengrenzwerte und DNS-Namen können ebenfalls in der Datei „docker-compose.yml“ angegeben werden.

Das [Service Fabric-Anwendungsmodell](service-fabric-application-model.md) verwendet Dienst- und Anwendungstypen. Dabei können Sie über viele Anwendungsinstanzen desselben Typs verfügen. Sie können beispielsweise eine Anwendungsinstanz pro Kunde haben. Dieses typbasierte Modell unterstützt die Registrierung mehrerer Versionen desselben Anwendungstyps in der Runtime.

Für Kunde A kann z.B. eine Anwendung mit Typ 1.0 von AppTypeA instanziiert werden, während für Kunde B eine andere Anwendung mit demselben Typ und derselben Version instanziiert wird. Die Applikationstypen werden im Applikationsmanifest festgelegt. Bei der Erstellung der Anwendung werden auch der Applikationsname und die Bereitstellungsparameter angegeben.

Obwohl dieses Modell Flexibilität bietet, planen wir auch die Unterstützung eines einfacheren, instanzbasierten Bereitstellungsmodells, bei dem sich die Typen implizit aus der Manifestdatei ergeben. In diesem Modell erhält jede Anwendung ein eigenes unabhängiges Manifest. Eine Vorschau dieser Vorgehensweise bieten wir durch die Unterstützung von „docker-compose.yml“-Dateien, einem instanzenbasierten Bereitstellungsformat.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Service Fabric-Anwendungsmodell](service-fabric-application-model.md).
* [Erste Schritte mit der Service Fabric CLI](service-fabric-cli.md)

