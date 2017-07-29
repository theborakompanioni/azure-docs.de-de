---
title: Service Fabric-Anwendungsupgrade | Microsoft Docs
description: "Dieser Artikel bietet eine Einführung in das Upgrade einer Service Fabric-Anwendung, einschließlich Wahl des Upgrademodus und der Durchführung der Integritätsüberprüfungen."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/27/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: b4637922e7b280b0e9954c9e51788202e784b4f9
ms.openlocfilehash: 743223f78f279fedf33f73ff52b56f4a7358cd51
ms.contentlocale: de-de
ms.lasthandoff: 02/13/2017

---
# <a name="service-fabric-application-upgrade"></a>Service Fabric-Anwendungsupgrade
Eine Azure Service Fabric Anwendung ist eine Sammlung von Diensten. Während eines Upgrades vergleicht Service Fabric das neue [Anwendungsmanifest](service-fabric-application-model.md#describe-an-application) mit der vorherigen Version und ermittelt, welche Dienste in der Anwendung aktualisiert werden müssen. Service Fabric vergleicht die Versionsnummern in den Dienstmanifesten mit den Versionsnummern in der vorherigen Version. Wenn sich ein Dienst nicht geändert hat, wird er nicht aktualisiert.

## <a name="rolling-upgrades-overview"></a>Übersicht über parallele Upgrades
Bei einem parallelen Anwendungsupgrade wird das Upgrade in verschiedenen Phasen durchgeführt. In jeder Phase wird das Upgrade auf eine Teilmenge von Knoten im Cluster angewendet, die als Updatedomäne bezeichnet wird. So bleibt die Anwendung während des gesamten Upgrades immer verfügbar. Während des Upgrades kann der Cluster eine Mischung aus der alten und neuen Version enthalten.

Aus diesem Grund müssen die beiden Versionen aufwärts- und abwärtskompatibel sein. Wenn sie nicht kompatibel sind, muss der Anwendungsadministrator ein mehrstufiges Upgrade bereitstellen, um die Verfügbarkeit aufrechtzuerhalten. Bei einem Upgrade in mehreren Phasen ist der erste Schritt das Aktualisieren einer Zwischenversion der Anwendung, die mit der vorherigen Version kompatibel ist. Der zweite Schritt ist das Upgrade auf die endgültige Version, die mit der Version vor dem Update nicht mehr, jedoch mit der Zwischenversion kompatibel ist.

Updatedomänen werden im Clustermanifest angegeben, wenn Sie den Cluster konfigurieren. Updatedomänen erhalten Updates nicht in einer bestimmten Reihenfolge. Bei einer Updatedomäne handelt es sich um eine logische Bereitstellungseinheit für eine Anwendung. Updatedomänen ermöglichen eine gleichbleibend hohe Verfügbarkeit der Dienste während eines Upgrades.

Nicht parallele Upgrades sind möglich, wenn das Upgrade auf alle Knoten im Cluster angewendet wird. Dies ist der Fall, wenn die Anwendung nur über eine Updatedomäne verfügt. Von diesem Ansatz wird jedoch abgeraten, da der Dienst zum Zeitpunkt des Upgrades nicht aktiv und nicht verfügbar ist. Darüber hinaus übernimmt Azure keine Garantie, wenn ein Cluster mit nur einer Updatedomäne eingerichtet wird.

## <a name="health-checks-during-upgrades"></a>Integritätsprüfungen bei Upgrades
Für ein Upgrade müssen Integritätsrichtlinien festgelegt (oder Standardwerte verwendet) werden. Ein Upgrade wird als erfolgreich bezeichnet, wenn alle Updatedomänen innerhalb der angegebenen Timeouts aktualisiert werden und wenn alle Updatedomänen als fehlerfrei gelten.  Eine fehlerfreie Updatedomäne bedeutet, dass die Aktualisierungsdomäne alle in der Integritätsrichtlinie angegebenen Integritätsprüfungen bestanden hat. Eine Integritätsrichtlinie kann beispielsweise vorgeben, dass alle Dienste in einer Anwendungsinstanz *fehlerfrei*laufen müssen, so wie die Integrität von Service Fabric definiert ist.

Integritätsrichtlinien und -prüfungen während eines Upgrades von Service Fabric sind dienst- und anwendungsunabhängig. Das heißt, es werden keine dienstspezifischen Tests durchgeführt.  Beispiel: Für Ihren Dienst gilt ggf. eine Durchsatzanforderung, doch Service Fabric hat nicht die Informationen zum Überprüfen des Durchsatzes. Nähere Informationen zu den durchgeführten Tests finden Sie in den [Artikeln zur Integrität](service-fabric-health-introduction.md). Die Überprüfungen, die während eines Upgrades durchgeführt werden, beinhalten Tests, die prüfen, ob das Anwendungspaket richtig kopiert wurde, ob die Instanz gestartet wurde, und so weiter.

Die Anwendungsintegrität ist eine Aggregation der untergeordneten Entitäten der Anwendung. Kurz gesagt evaluiert Service Fabric die Integrität der Anwendung über die für die Anwendung gemeldete Integrität. Es wird auch die Integrität aller Dienste für die Anwendung auf diese Weise ausgewertet. Service Fabric wertet ebenso die Integrität der Anwendungsdienste durch Aggregation der Integritätswerte der zugehörigen untergeordneten Elemente wie z. B. der Dienstreplikate aus. Sobald die Anwendungsintegritätsrichtlinie erfüllt ist, kann das Upgrade fortgesetzt werden. Wenn die Integritätsrichtlinie verletzt wird, schlägt die Aktualisierung der Anwendung fehl.

## <a name="upgrade-modes"></a>Upgrademodi
Der Modus, den wir für Upgrades von Anwendungen empfehlen, ist der überwachte Modus, der üblicherweise verwendet wird. Im überwachten Modus wird das Update zunächst in einer Updatedomäne ausgeführt. Wenn alle Integritätsprüfungen erfolgreich durchgeführt wurden (entsprechend der angegebenen Richtlinie), wird automatisch die nächste Updatedomäne aktualisiert.  Wenn die Integritätsprüfungen fehlschlagen und/oder Timeouts überschritten werden, wird das Upgrade für die Updatedomäne entweder zurückgesetzt oder der Modus in den nicht überwachten manuellen Modus geändert. Sie können die Upgrades so konfigurieren, dass einer dieser beiden Modi bei misslungenen Upgrades verwendet wird. 

Der nicht überwachte manuelle Modus benötigt nach jedem Upgrade in einer Updatedomäne einen manuellen Eingriff, um das Upgrade für die nächste Updatedomäne zu starten. Es werden keine Service Fabric-Integritätsprüfungen ausgeführt. Der Administrator überprüft den Zustand oder Status vor dem Upgrade in der nächsten Updatedomäne.

## <a name="upgrade-default-services"></a>Durchführen eines Upgrades von Standarddiensten
Standarddienste in der Service Fabric-Anwendung können während des Upgradevorgangs einer Anwendung aktualisiert werden. Standarddienste werden im [Anwendungsmanifest](service-fabric-application-model.md#describe-an-application) definiert. Für das Aktualisieren von Standarddiensten gelten folgende Standardregeln:

1. Standarddienste im neuen [Anwendungsmanifest](service-fabric-application-model.md#describe-an-application), die im Cluster nicht vorhanden sind, werden erstellt.
> [!TIP]
> [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md#fabric-settings-that-you-can-customize) muss auf TRUE festgelegt werden, um die folgenden Regeln zu aktivieren. Dieses Feature wird ab Version 5.5 unterstützt.

2. Standarddienste, die sowohl im vorherigen [Anwendungsmanifest](service-fabric-application-model.md#describe-an-application) als auch in der neuen Version vorhanden sind, werden aktualisiert. Die im Cluster bereits vorhandenen Dienstbeschreibungen werden durch die der neuen Version überschrieben. Das Anwendungsupgrade führt bei einem Fehler beim Aktualisieren des Standarddiensts automatisch einen Rollback durch.
3. Standarddienste, die im vorherigen [Anwendungsmanifest](service-fabric-application-model.md#describe-an-application), aber nicht in der neuen Version enthalten sind, werden gelöscht. **Beachten Sie, dass dieses Löschen von Standarddiensten nicht rückgängig gemacht werden kann.**

Falls für ein Anwendungsupgrade ein Rollback ausgeführt wird, werden Standarddienste auf den Status vor Beginn des Upgrades zurückgesetzt. Gelöschte Dienste können jedoch nicht erstellt werden.

## <a name="application-upgrade-flowchart"></a>Flussdiagramm eines Anwendungsupgrades
Das Flussdiagramm unter diesem Absatz kann Ihnen dabei helfen, den Upgradevorgang einer Service Fabric-Anwendung zu verstehen. Im Speziellen ist hier schematisch dargestellt, wie die Timeouts, z.B. *HealthCheckStableDuration*, *HealthCheckRetryTimeout* und *UpgradeHealthCheckInterval*, beim Steuern helfen, wenn das Upgrade in einer Updatedomäne als erfolgreich oder fehlgeschlagen gilt.

![Upgradevorgang für eine Service Fabric-Anwendung][image]

## <a name="next-steps"></a>Nächste Schritte
[Ihre Anwendung mit Visual Studio upgraden](service-fabric-application-upgrade-tutorial.md) beschreibt das Upgraden von Anwendungen mit Visual Studio.

[Upgrade Ihrer Anwendung mithilfe von PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) werden Sie schrittweise durch das Upgrade der Anwendung mithilfe von PowerShell geführt.

Steuern Sie die Upgrades von Anwendungen mithilfe von [Upgradeparametern](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Anwendungsupgrades kompatibel, indem Sie sich mit der [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)vertraut machen.

Informieren Sie sich in [weiterführenden Themen](service-fabric-application-upgrade-advanced.md)darüber, wie Sie erweiterte Funktionen beim Upgrade Ihrer Anwendung nutzen.

Informationen zum Beheben gängiger Probleme bei Anwendungsupgrades finden Sie in den Anweisungen unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png

