---
title: "Überwachung und Diagnose von Azure Service Fabric-Containern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Container, die in Microsoft Azure Service Fabric orchestriert werden, mit der Containerlösung von OMS überwachen und diagnostizieren."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/10/2017
ms.author: dekapur
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 874c1a5c4b399ff2254072b7282f05d83a005cc3
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---

# <a name="monitoring-windows-server-containers-with-oms"></a>Überwachen von Windows Server-Containern mit OMS

## <a name="oms-containers-solution"></a>OMS-Containerlösung

Das Operations Management Suite-Team (OMS) hat eine Containerlösung für die Diagnose und Überwachung von Containern veröffentlicht. Zusammen mit der Service Fabric-Lösung des Teams stellt diese Lösung ein großartiges Tool zum Überwachen von Containerbereitstellungen dar, die in Service Fabric orchestriert werden. Hier ist ein einfaches Beispiel für das Dashboard in der Lösung:

![Einfaches OMS-Dashboard](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Es erfasst auch verschiedene Arten von Protokollen, die im OMS-Tool Log Analytics abgefragt und dann zum Visualisieren beliebiger Metriken oder generierter Ereignisse verwendet werden können. Folgende Protokolltypen werden gesammelt:

1. ContainerInventory: zeigt Informationen über Containerspeicherort, -name und -images an
2. ContainerImageInventory: zeigt Informationen zu bereitgestellten Images an, einschließlich IDs und Größen
3. ContainerLog: zeigt bestimmte Fehlerprotokolle, Docker-Protokolle (stdout usw.) und andere Einträge an
4. ContainerServiceLog: zeigt die Docker-Daemon-Befehle an, die ausgeführt wurden
5. Perf: zeigt Leistungsindikatoren einschließlich Container-CPU, Arbeitsspeicher, Netzwerkdatenverkehr, Datenträger-E/A und benutzerdefinierter Metriken von den Hostcomputern an

In diesem Artikel werden die erforderlichen Schritte zum Einrichten der Containerüberwachung für Ihren Cluster beschrieben. Weitere Informationen zur OMS-Containerlösung finden Sie in der [Dokumentation](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Einrichten von Service Fabric-Clustern

Informationen zum Erstellen eines Clusters mithilfe von Azure Resource Manager-Vorlagen finden Sie [hier](https://github.com/dkkapur/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Sample). Achten Sie darauf, einen eindeutigen Namen für den OMS-Arbeitsbereich hinzuzufügen. Diese Vorlage wird standardmäßig auch zum Bereitstellen eines Clusters in der Vorschauversion von Service Fabric (v255.255) verwendet, d.h., sie kann nicht in einer Produktionsumgebung verwendet und nicht auf eine andere Service Fabric-Version aktualisiert werden. Wenn Sie diese Vorlage langfristig oder in Produktionsumgebungen verwenden möchten, ändern Sie die Version in eine stabile Versionsnummer.

Nachdem der Cluster eingerichtet wurde, vergewissern Sie sich, dass Sie das richtige Zertifikat installiert haben und dass Sie eine Verbindung mit dem Cluster herstellen können.

Überprüfen Sie, ob Ihre Ressourcengruppe ordnungsgemäß eingerichtet wurde, indem Sie im [Azure-Portal](https://portal.azure.com/) nach der Bereitstellung suchen. Die Ressourcengruppe sollte alle Service Fabric-Ressourcen und außerdem eine Log Analytics-Lösung und eine Service Fabric-Lösung enthalten.

So ändern Sie einen vorhandenen Service Fabric-Cluster
* Stellen Sie sicher, dass die Diagnose aktiviert ist (aktivieren Sie sie andernfalls durch [Aktualisieren der VM-Skalierungsgruppe](/rest/api/virtualmachinescalesets/create-or-update-a-set)).
* Fügen Sie einen OMS-Arbeitsbereich hinzu, indem Sie eine „Service Fabric-Analyse“-Lösung über den Azure Marketplace erstellen.
* Bearbeiten Sie die Datenquellen der Service Fabric-Lösung, um Daten aus den entsprechenden Azure Storage-Tabellen (durch WAD eingerichtet) in den Ressourcengruppen auszuwählen, denen der Cluster angehört.
* Fügen Sie den Agent als eine [Erweiterung für die VM-Skalierungsgruppe](/powershell/module/azurerm.compute/add-azurermvmssextension) über PowerShell hinzu, oder aktualisieren Sie dafür die VM-Skalierungsgruppe (gleicher Link wie oben für die Änderung der Resource Manager-Vorlage).

## <a name="2-deploy-a-container"></a>2. Bereitstellen eines Containers

Sobald der Cluster bereit ist und Sie sich vergewissert haben, dass Sie darauf zugreifen können, stellen Sie darin einen Container bereit. Wenn Sie gemäß der Vorlage die Vorschauversion verwenden möchten, können Sie sich auch die neue Docker Compose-Funktionalität von Service Fabric ansehen. Bedenken Sie, dass es bei der ersten Bereitstellung eines Containerimages in einem Cluster mehrere Minuten dauern kann, bis das Image heruntergeladen wurde. Dies hängt von der Größe des Images ab.

## <a name="3-add-the-containers-solution"></a>3. Hinzufügen der Containerlösung

Erstellen Sie im Azure-Portal über den Azure Marketplace eine Containerressource (in der Kategorie „Überwachung und Verwaltung“). 

![Hinzufügen der Containerlösung](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

Im Erstellungsschritt wird ein OMS-Arbeitsbereich angefordert. Wählen Sie den Arbeitsbereich aus, der mit der Bereitstellung oben erstellt wurde. Dieser Schritt fügt die Containerlösung Ihrem OMS-Arbeitsbereich hinzu. Sie wird dann automatisch durch den OMS-Agent erkannt, der von der Vorlage bereitgestellt wurde. Der Agent beginnt das Sammeln von Daten für Containerprozesse im Cluster, und nach etwa 10–15 Minuten sollten in der Lösung Daten wie in der Abbildung des Dashboards oben angezeigt werden.

## <a name="4-next-steps"></a>4. Nächste Schritte

OMS stellt verschiedene hilfreiche Tools im Arbeitsbereich bereit. Anhand der folgenden Optionen können Sie die Lösung an Ihre Anforderungen anpassen:
- Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
- Konfigurieren Sie den OMS-Agent für das Abrufen spezifischer Leistungsindikatoren (wechseln Sie zur Startseite des Arbeitsbereichs > „Einstellungen“ > „Daten“ > „Windows-Leistungsindikatoren“).
- Konfigurieren Sie OMS für die Einrichtung von Regeln für [automatisierte Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
