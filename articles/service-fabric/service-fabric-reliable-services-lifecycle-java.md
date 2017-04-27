---
title: "Übersicht über den Lebenszyklus von Azure Service Fabric Reliable Services | Microsoft-Dokumentation"
description: Informationen zu den verschiedenen Lebenszyklusereignissen in Service Fabric Reliable Services
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/09/2017
ms.author: pakunapa;
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: ddec69e57e84f33c37831a0da2c21955d78fff98
ms.lasthandoff: 04/03/2017

---

# <a name="reliable-services-lifecycle-overview"></a>Übersicht über den Lebenszyklus von Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java unter Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Bei der Betrachtung der Lebenszyklen von Reliable Services sind vor allem die Grundlagen zum Lebenszyklus relevant. Im Allgemeinen:

* Während des Starts
  * Dienste werden erstellt.
  * Sie haben eine Möglichkeit zum Erstellen und Zurückgeben von null oder mehr Listenern.
  * Alle zurückgegebenen Listener werden geöffnet und ermöglichen so eine Kommunikation mit dem Dienst.
  * Die runAsync-Methode des Diensts wird aufgerufen und ermöglicht dem Dienst die Ausführung langwieriger Aufgaben oder von Hintergrundarbeit.
* Während des Herunterfahrens
  * Das an runAsync übergebene Abbruchtoken wird abgebrochen, und die Listener werden geschlossen.
  * Sobald dies abgeschlossen ist, wird das Dienstobjekt selbst zerstört.

Es gibt Informationen zur genauen Reihenfolge dieser Ereignisse. Insbesondere kann sich die Reihenfolge der Ereignisse geringfügig ändern, je nachdem, ob der zuverlässigen Dienst zustandslos oder zustandsbehaftet ist. Für zustandsbehaftete Dienste müssen wir uns außerdem mit dem Tauschszenario für primäre Replikate befassen. Während dieser Sequenz wird die Rolle des primären Replikats auf ein anderes Replikat (oder wieder zurück) übertragen, ohne dass der Dienst heruntergefahren wird. Schließlich müssen wir uns mit Fehlerbedingungen beschäftigen.

## <a name="stateless-service-startup"></a>Start zustandsloser Dienste
Der Lebenszyklus eines zustandslosen Diensts ist recht einfach. So sieht die Reihenfolge der Ereignisse aus:

1. Der Dienst wird erstellt.
2. Gleichzeitig geschehen zwei Dinge:
    - `StatelessService.createServiceInstanceListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. (`CommunicationListener.openAsync()` wird auf jedem Listener aufgerufen.)
    - Die runAsync-Methode des Diensts (`StatelessService.runAsync()`) wird aufgerufen.
3. Falls vorhanden, wird die eigene onOpenAsync-Methode des Diensts aufgerufen. (Insbesondere wird `StatelessService.onOpenAsync()` aufgerufen. Diese Außerkraftsetzung ist ungewöhnlich, aber verfügbar.)

Beachten Sie unbedingt, dass es keine Reihenfolge zwischen den Aufrufen zum Erstellen und Öffnen der Listener und runAsync gibt. Die Listener werden möglicherweise vor dem Start von runAsync geöffnet. Ebenso wird runAsync möglicherweise aufgerufen, bevor die Kommunikationslistener geöffnet sind oder überhaupt erstellt wurden. Wenn eine Synchronisierung erforderlich ist, wird diese dem Implementierer als Übung überlassen. Gängige Lösungen:

* In einigen Fällen funktionieren Listener erst dann, wenn andere Informationen erstellt oder Aufgaben durchgeführt wurden. Für zustandslose Dienste kann diese Arbeit normalerweise im Konstruktor des Diensts, während des `createServiceInstanceListeners()`-Aufrufs oder im Rahmen der Listenererstellung selbst durchgeführt werden.
* In einigen Fällen lässt sich der Code in runAsync erst starten, wenn die Listener geöffnet sind. In diesem Fall ist zusätzliche Koordination erforderlich. Eine gängige Lösung besteht in einem Flag innerhalb der Listener, das angibt, wann diese abgeschlossen wurden, und das in runAsync vor dem Fortfahren mit der eigentlichen Arbeit überprüft wird.

## <a name="stateless-service-shutdown"></a>Herunterfahren zustandsloser Dienste
Beim Herunterfahren eines zustandslosen Diensts wird dasselbe Muster in umgekehrter Reihenfolge befolgt:

1. Gleichzeitig:
    - Alle offenen Listener werden geschlossen. (`CommunicationListener.closeAsync()` wird für jeden Listener aufgerufen.)
    - Das an `runAsync()` übergebene Abbruchtoken wird abgebrochen. (Es wird überprüft, ob die `isCancelled`-Eigenschaft des Abbruchtokens TRUE zurückgibt, und die `throwIfCancellationRequested`-Methode gibt bei Aufruf eine `CancellationException` zurück.)
2. Sobald auf jedem Listener `closeAsync()` und auch `runAsync()` abgeschlossen wurden, wird die `StatelessService.onCloseAsync()`-Methode des Diensts aufgerufen, sofern vorhanden (auch dies ist eine ungewöhnliche Außerkraftsetzung).
3. Nach Abschluss von `StatelessService.onCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="notes-on-service-lifecycle"></a>Hinweise zum Dienstlebenszyklus
* Sowohl die `runAsync()`-Methode als auch die `createServiceInstanceListeners`-Aufrufe sind optional. Ein Dienst kann eine dieser Optionen, beide oder keine von beiden aufweisen. Wenn der Dienst beispielsweise alle Vorgänge infolge von Benutzeraufrufen ausführt, muss `runAsync()` nicht implementiert werden. Nur die Kommunikationslistener und der zugehörige Code sind erforderlich. Ebenso ist das Erstellen und Zurückgeben von Kommunikationslistenern optional, da der Dienst möglicherweise nur Hintergrundaufgaben ausführt und daher nur `runAsync()` implementieren muss.
* Ein Dienst kann `runAsync()` erfolgreich abschließen und dann zurückkehren. Dies wird nicht als Fehlerbedingung betrachtet, sondern stellt die Hintergrundarbeit für den Abschluss des Diensts dar. Für zustandsbehaftete zuverlässige Dienste wird `runAsync()` erneut aufgerufen, wenn der Dienst vom primären Replikat tiefer gestuft und dann wieder auf das primäre Replikat höher gestuft wird.
* Wenn ein Dienst durch eine unerwartete Ausnahme von `runAsync()` aus beendet wird, ist dies ein Fehler. Das Dienstobjekt wird heruntergefahren, und ein Integritätsfehler wird gemeldet.
* Zwar gibt es keine zeitliche Begrenzung für die Rückgabe dieser Methoden, aber Sie verlieren sofort die Möglichkeit zum Schreiben und können daher Ihre eigentlichen Arbeiten nicht abschließen. Es wird empfohlen, dass Sie sie so schnell wie möglich nach dem Empfang der Abbruchanforderung zurückgeben. Wenn der Dienst nicht in einem angemessenen Zeitraum auf diese API-Aufrufe reagiert, kann Service Fabric das Beenden des Diensts erzwingen. Dies geschieht normalerweise nur während Anwendungsupgrades oder beim Löschen eines Diensts. Das Timeout beträgt standardmäßig 15 Minuten.
* Fehler im `onCloseAsync()`-Pfad führen zu einem Aufruf von `onAbort()`. So erhält der Dienst eine letzte Gelegenheit zum Bereinigen und Freigeben aller beanspruchten Ressourcen.

> [!NOTE]
> Zustandsbehaftete zuverlässige Dienste werden in Java noch nicht unterstützt.
>
>

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Reliable Services](service-fabric-reliable-services-introduction.md)
* [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
* [Erweiterte Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md)

