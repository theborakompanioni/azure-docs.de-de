---
title: Erweiterte Verwendung von Reliable Services | Microsoft Docs
description: Erfahren Sie mehr über die erweiterte Verwendung der Reliable Services von Service Fabric für mehr Flexibilität für Ihre Dienste.
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider

ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2016
ms.author: vturecek

---
# Erweiterte Verwendung des Reliable Services-Programmiermodells
Azure Service Fabric vereinfacht das Schreiben und Verwalten zuverlässiger zustandsloser und zustandsbehafteter Dienste (Reliable Services). In diesem Handbuch wird die erweiterte Verwendung von Reliable Services erläutert, die Ihnen mehr Kontrolle über und Flexibilität für Ihre Dienste ermöglicht. Machen Sie sich vor dem Lesen dieses Handbuchs mit dem [Reliable Services-Programmiermodell](service-fabric-reliable-services-introduction.md) vertraut.

Sowohl zustandsbehaftete als auch zustandslose Dienste weisen zwei primäre Einstiegspunkte für Benutzercode auf:

* `RunAsync` ist ein allgemein gültiger Einstiegspunkt für Ihren Dienstcode.
* `CreateServiceReplicaListeners` und `CreateServiceInstanceListeners` dienen dazu, Kommunikationslistener für Clientanforderungen zu öffnen.

Für die meisten Dienste sind diese zwei Einstiegspunkte ausreichend. In den seltenen Fällen, in denen mehr Kontrolle über den Lebenszyklus eines Diensts erforderlich ist, sind zusätzliche Lebenszyklusereignisse verfügbar.

## Lebenszyklus der Instanz eines zustandslosen Diensts
Der Lebenszyklus eines zustandslosen Diensts ist sehr einfach. Ein zustandsloser Dienst kann nur geöffnet, geschlossen oder abgebrochen werden. `RunAsync` wird in einem zustandslosen Dienst ausgeführt, wenn eine Dienstinstanz geöffnet wird, und abgebrochen, wenn eine Dienstinstanz geschlossen oder abgebrochen wird.

Obwohl `RunAsync` in fast allen Fällen ausreichen sollte, sind die Ereignisse „Öffnen“, „Schließen“ und „Abbrechen“ auch in zustandslosen Diensten verfügbar:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)` "OnOpenAsync" wird zur Verwendung der zustandslosen Dienstinstanz aufgerufen. Erweiterte Serviceinitialisierungsaufgaben können zu diesem Zeitpunkt gestartet werden.
* `Task OnCloseAsync(CancellationToken)` OnCloseAsync wird aufgerufen, wenn die zustandslose Dienstinstanz ordnungsgemäß beendet wird. Dies kann der Fall sein, wenn Code für den Dienst aktualisiert, die Dienstinstanz aufgrund des Lastenausgleichs verschoben oder ein vorübergehender Fehler erkannt wird. "OnCloseAsync" kann verwendet werden, um Ressourcen sicher zu schließen, die Hintergrundverarbeitung anzuhalten, das Speichern des externen Status zu beenden oder bestehende Verbindungen zu deaktivieren.
* `void OnAbort()` OnAbort wird aufgerufen, wenn das Herunterfahren der zustandslosen Dienstinstanz erzwungen wird. Diese Methode wird im Allgemeinen verwendet, wenn auf dem Knoten ein dauerhafter Fehler erkannt wird oder Service Fabric den Lebenszyklus der Dienstinstanz aufgrund von internen Fehlern nicht zuverlässig verwalten kann.

## Lebenszyklus des zustandsbehafteten Dienstreplikats
Der Lebenszyklus eines zustandsbehafteten Dienstsreplikats ist viel komplizierter als der einer zustandslosen Dienstinstanz. Zusätzlich zu den Ereignissen „Öffnen“, „Schließen“ und „Abbrechen“ durchläuft ein zustandsbehafteter Dienst während seiner Lebensdauer Rollenänderungen. Wenn ein zustandsbehaftetes Dienstreplikat die Rolle wechselt, wird das Ereignis `OnChangeRoleAsync` ausgelöst:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)` OnChangeRoleAsync wird immer dann aufgerufen, wenn das zustandsbehaftete Dienstreplikat die Rolle wechselt und beispielsweise ein primäres oder sekundäres Replikat wird. Primäre Replikate erhalten Schreibstatus (mit Erlaubnis zum Erstellen und Schreiben in Reliable Collections). Sekundäre Replikate erhalten Lesestatus (können nur aus vorhandenen Reliable Collections lesen). Die meisten Aufgaben in einem zustandsbehafteten Dienst werden im primären Replikat ausgeführt. Sekundäre Replikate können schreibgeschützte Überprüfungen durchführen, Berichte generieren und Data Mining oder andere schreibgeschützte Aufträge ausführen.

In einem zustandsbehafteten Dienst verfügt nur das primäre Replikat über Schreibzugriff auf den Zustand. Daher ist dies in der Regel wo der Dienst die eigentliche Arbeit ausführt. Die `RunAsync`-Methode wird in einem zustandsbehafteten Dienst nur ausgeführt, wenn das zustandsbehaftete Dienstreplikat primär ist. Die `RunAsync`-Methode wird abgebrochen, wenn ein primäres Replikat seine Rolle ändert und kein primäres Replikat mehr ist, oder während der Ereignisse „Schließen“ oder „Abbrechen“.

Die Verwendung des Ereignisses `OnChangeRoleAsync` erlaubt Ihnen die Ausführung der Arbeitsschritte, abhängig von der Replikatsrolle sowie als Reaktion auf Rollenwechsel.

Ein zustandsbehafteter Dienst bietet außerdem die gleichen vier Lebenszyklusereignisse wie ein zustandsloser Dienst, mit derselben Semantik und identischen Anwendungsfällen:

* `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
* `Task OnCloseAsync(CancellationToken)`
* `void OnAbort()`

## Nächste Schritte
Erweiterte Themen im Zusammenhang mit Service Fabric finden Sie in den folgenden Artikeln.

* [Konfigurieren zustandsbehafteter Reliable Services](service-fabric-reliable-services-configuration.md)
* [Einführung in Service Fabric-Integrität](service-fabric-health-introduction.md)
* [Verwenden von Systemintegritätsberichten für die Problembehandlung](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Konfigurieren von Diensten mit dem Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-configure-services.md)

<!---HONumber=AcomDC_0713_2016-->