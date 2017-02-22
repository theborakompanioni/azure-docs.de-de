---
title: "Übersicht über den Lebenszyklus von Azure Service Fabric Reliable Services | Microsoft-Dokumentation"
description: Informationen zu den verschiedenen Lebenszyklusereignissen in Service Fabric Reliable Services
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider;
translationtype: Human Translation
ms.sourcegitcommit: 1472bd1647b620d1f0489cdabbed58e72585aef7
ms.openlocfilehash: ad1f713f2b12af4f778b9b14edf33a568d2aa8b1

---

# <a name="reliable-services-lifecycle-overview"></a>Übersicht über den Lebenszyklus von Reliable Services
Bei der Betrachtung der Lebenszyklen von Reliable Services sind vor allem die Grundlagen zum Lebenszyklus relevant. Im Allgemeinen:

* Während des Starts
  * Dienste werden erstellt.
  * Sie haben eine Möglichkeit zum Erstellen und Zurückgeben von null oder mehr Listenern.
  * Alle zurückgegebenen Listener werden geöffnet und ermöglichen so eine Kommunikation mit dem Dienst.
  * Die RunAsync-Methode des Diensts wird aufgerufen und ermöglicht dem Dienst die Ausführung langwieriger Aufgaben oder von Hintergrundarbeit.
* Während des Herunterfahrens
  * Das an RunAsync übergebene Abbruchtoken wird abgebrochen, und die Listener werden geschlossen.
  * Sobald dies abgeschlossen ist, wird das Dienstobjekt selbst zerstört.

Es gibt Informationen zur genauen Reihenfolge dieser Ereignisse. Insbesondere kann sich die Reihenfolge der Ereignisse geringfügig ändern, je nachdem, ob der zuverlässigen Dienst zustandslos oder zustandsbehaftet ist. Für zustandsbehaftete Dienste müssen wir uns außerdem mit dem Tauschszenario für primäre Replikate befassen. Während dieser Sequenz wird die Rolle des primären Replikats auf ein anderes Replikat (oder wieder zurück) übertragen, ohne dass der Dienst heruntergefahren wird. Schließlich müssen wir uns mit Fehlerbedingungen beschäftigen.

## <a name="stateless-service-startup"></a>Start zustandsloser Dienste
Der Lebenszyklus eines zustandslosen Diensts ist recht einfach. So sieht die Reihenfolge der Ereignisse aus:

1. Der Dienst wird erstellt.
2. Gleichzeitig geschehen zwei Dinge:
    - `StatelessService.CreateServiceInstanceListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. (`ICommunicationListener.OpenAsync()` wird auf jedem Listener aufgerufen.)
    - Die RunAsync-Methode des Diensts (`StatelessService.RunAsync()`) wird aufgerufen.
3. Falls vorhanden, wird die eigene OnOpenAsync-Methode des Diensts aufgerufen. (Insbesondere wird `StatelessService.OnOpenAsync()` aufgerufen. Diese Außerkraftsetzung ist ungewöhnlich, aber verfügbar.)

Beachten Sie unbedingt, dass es keine Reihenfolge zwischen den Aufrufen zum Erstellen und Öffnen der Listener und RunAsync gibt. Die Listener werden möglicherweise vor dem Start von RunAsync geöffnet. Ebenso wird RunAsync möglicherweise aufgerufen, bevor die Kommunikationslistener geöffnet sind oder überhaupt erstellt wurden. Wenn eine Synchronisierung erforderlich ist, wird diese dem Implementierer als Übung überlassen. Gängige Lösungen:

* In einigen Fällen funktionieren Listener erst dann, wenn andere Informationen erstellt oder Aufgaben durchgeführt wurden. Für zustandslose Dienste kann diese Arbeit normalerweise im Konstruktor des Diensts, während des `CreateServiceInstanceListeners()`-Aufrufs oder im Rahmen der Listenererstellung selbst durchgeführt werden.
* In einigen Fällen lässt sich der Code in RunAsync erst starten, wenn die Listener geöffnet sind. In diesem Fall ist zusätzliche Koordination erforderlich. Eine gängige Lösung besteht in einem Flag innerhalb der Listener, das angibt, wann diese abgeschlossen wurden, und das in RunAsync vor dem Fortfahren mit der eigentlichen Arbeit überprüft wird.

## <a name="stateless-service-shutdown"></a>Herunterfahren zustandsloser Dienste
Beim Herunterfahren eines zustandslosen Diensts wird dasselbe Muster in umgekehrter Reihenfolge befolgt:

1. Gleichzeitig:
    - Alle offenen Listener werden geschlossen. (`ICommunicationListener.CloseAsync()` wird für jeden Listener aufgerufen.)
    - Das an `RunAsync()` übergebene Abbruchtoken wird abgebrochen. (Es wird überprüft, ob die `IsCancellationRequested`-Eigenschaft des Abbruchtokens TRUE zurückgibt, und die `ThrowIfCancellationRequested`-Methode gibt bei Aufruf eine `OperationCanceledException` zurück.)
2. Sobald auf jedem Listener `CloseAsync()` und auch `RunAsync()` abgeschlossen wurden, wird die `StatelessService.OnCloseAsync()`-Methode des Diensts aufgerufen, sofern vorhanden (auch dies ist eine ungewöhnliche Außerkraftsetzung).
3. Nach Abschluss von `StatelessService.OnCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="stateful-service-startup"></a>Start zustandsbehafteter Dienste
Zustandsbehaftete Dienste befolgen mit wenigen Änderungen ein ähnliches Muster wie zustandslose Dienste. Beim Starten eines zustandsbehafteten Diensts lautet die Reihenfolge der Ereignisse wie folgt:

1. Der Dienst wird erstellt.
2. `StatefulServiceBase.OnOpenAsync()` wird aufgerufen. (Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.)
3. Wenn es sich beim fraglichen Dienstreplikat um das primäre handelt, geschehen die folgenden Dinge parallel. Andernfalls fährt der Dienst mit Schritt 4 fort.
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. (`ICommunicationListener.OpenAsync()` wird auf jedem Listener aufgerufen.)
    - Die RunAsync-Methode des Diensts (`StatefulServiceBase.RunAsync()`) wird aufgerufen.
4. Sobald alle `OpenAsync()`-Aufrufe des Replikatlisteners abgeschlossen sind und `RunAsync()` gestartet wurde (bzw. diese Schritte übersprungen wurden, da es sich bei diesem Replikat zurzeit um ein sekundäres handelt), wird `StatefulServiceBase.OnChangeRoleAsync()` aufgerufen. (Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.)

Ähnlich wie bei zustandslosen Diensten gibt es keine Koordination der Reihenfolge, in der die Listener erstellt und geöffnet werden und RunAsync aufgerufen wird. Die Lösungen sind abgesehen von einem zusätzlichen Fall nahezu identisch: Nehmen Sie beispielsweise an, dass die bei den Kommunikationslistenern eingehenden Aufrufe Informationen in [zuverlässigen Sammlungen](service-fabric-reliable-services-reliable-collections.md) benötigen, um zu funktionieren. Da die Kommunikationslistener geöffnet werden könnten, bevor die zuverlässigen Sammlungen gelesen oder geschrieben werden können oder RunAsync gestartet werden kann, ist einige zusätzliche Koordination erforderlich. Die einfachste und gängigste Lösung besteht darin, dass die Kommunikationslistener einen Fehlercode zurückgeben, anhand dessen der Client feststellt, dass die Anforderung wiederholt werden muss.

## <a name="stateful-service-shutdown"></a>Herunterfahren zustandsbehafteter Dienste
Ähnlich wie bei zustandslosen Diensten stimmen die Lebenszyklusereignisse während des Herunterfahrens mit denen während des Starts überein, aber in umgekehrter Reihenfolge. Wenn ein zustandsbehafteter Dienst heruntergefahren wird, treten die folgenden Ereignisse auf:

1. Gleichzeitig:
    - Alle offenen Listener werden geschlossen. (`ICommunicationListener.CloseAsync()` wird für jeden Listener aufgerufen.)
    - Das an `RunAsync()` übergebene Abbruchtoken wird abgebrochen. (Es wird überprüft, ob die `IsCancellationRequested`-Eigenschaft des Abbruchtokens TRUE zurückgibt, und die `ThrowIfCancellationRequested`-Methode gibt bei Aufruf eine `OperationCanceledException` zurück.)
2. Sobald `CloseAsync()` auf den einzelnen Listenern abgeschlossen ist und auch `RunAsync()` abgeschlossen wurde (was nur bei einem primären Dienstreplikat erforderlich sein sollte), wird `StatefulServiceBase.OnChangeRoleAsync()` für den Dienst aufgerufen. (Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.)
3. Nach Abschluss der `StatefulServiceBase.OnChangeRoleAsync()`-Methode wird die `StatefulServiceBase.OnCloseAsync()`-Methode aufgerufen. (Auch diese Außerkraftsetzung ist ungewöhnlich, aber verfügbar.)
3. Nach Abschluss von `StatefulServiceBase.OnCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="stateful-service-primary-swaps"></a>Tausch des primären Replikats bei zustandsbehafteten Diensten
Während ein zustandsbehafteter Dienst ausgeführt wird, werden nur für die primären Replikate dieser zustandsbehafteten Dienste die Kommunikationslistener geöffnet und die RunAsync-Methode aufgerufen. Die sekundären Replikate werden erstellt, erhalten jedoch keine weiteren Aufrufe. Während ein zustandsbehafteter Dienst ausgeführt wird, kann sich jedoch ändern, welches Replikat derzeit das primäre ist. Was bedeutet dies im Hinblick auf die für ein Replikat sichtbaren Lebenszyklusereignisse? Das für das zustandsbehaftete Replikat sichtbare Verhalten hängt davon ab, ob das Replikat während des Tauschs höher oder tiefer gestuft wird.

### <a name="for-the-primary-being-demoted"></a>Für das tiefer gestufte primäre Replikat
Für Service Fabric muss dieses Replikat die Verarbeitung von Nachrichten und jegliche Hintergrundverarbeitung beenden. Folglich ähnelt dieser Schritt dem Herunterfahren des Diensts. Ein Unterschied besteht darin, dass der Dienst nicht zerstört oder geschlossen wird, weil er als sekundäres Replikat erhalten bleibt. Die folgenden APIs werden aufgerufen:

1. Gleichzeitig:
    - Alle offenen Listener werden geschlossen. (`ICommunicationListener.CloseAsync()` wird für jeden Listener aufgerufen.)
    - Das an `RunAsync()` übergebene Abbruchtoken wird abgebrochen. (Es wird überprüft, ob die `IsCancellationRequested`-Eigenschaft des Abbruchtokens TRUE zurückgibt, und die `ThrowIfCancellationRequested`-Methode gibt bei Aufruf eine `OperationCanceledException` zurück.)
2. Sobald `CloseAsync()` auf den einzelnen Listenern und auch `RunAsync()` abgeschlossen ist, wird `StatefulServiceBase.OnChangeRoleAsync()` für den Dienst aufgerufen. (Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.)

### <a name="for-the-secondary-being-promoted"></a>Für das höher gestufte sekundäre Replikat
Auf ähnliche Weise muss dieses Replikat für Service Fabric mit dem Lauschen auf Nachrichten beginnen (falls es diese Funktion ausführt) und alle relevanten Hintergrundaufgaben starten. Demzufolge ähnelt dieser Prozess dem Erstellen des Diensts, mit der Ausnahme, dass das Replikat selbst bereits vorhanden ist. Die folgenden APIs werden aufgerufen:

1. Gleichzeitig:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. (`ICommunicationListener.OpenAsync()` wird auf jedem Listener aufgerufen.)
    - Die RunAsync-Methode des Diensts (`StatefulServiceBase.RunAsync()`) wird aufgerufen.
4. Sobald alle `OpenAsync()`-Aufrufe des Replikatlisteners abgeschlossen sind und `RunAsync()` gestartet wurde (bzw. diese Schritte übersprungen wurden, da es sich um ein sekundäres Replikat handelt), wird `StatefulServiceBase.OnChangeRoleAsync()` aufgerufen. (Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.)

## <a name="notes-on-service-lifecycle"></a>Hinweise zum Dienstlebenszyklus
* Sowohl die `RunAsync()`-Methode als auch die `CreateServiceReplicaListeners/CreateServiceInstanceListeners`-Aufrufe sind optional. Ein Dienst kann eine dieser Optionen, beide oder keine von beiden aufweisen. Wenn der Dienst beispielsweise alle Vorgänge infolge von Benutzeraufrufen ausführt, muss `RunAsync()` nicht implementiert werden. Nur die Kommunikationslistener und der zugehörige Code sind erforderlich. Ebenso ist das Erstellen und Zurückgeben von Kommunikationslistenern optional, da der Dienst möglicherweise nur Hintergrundaufgaben ausführt und daher nur `RunAsync()` implementieren muss.
* Ein Dienst kann `RunAsync()` erfolgreich abschließen und dann zurückkehren. Dies wird nicht als Fehlerbedingung betrachtet, sondern stellt die Hintergrundarbeit für den Abschluss des Diensts dar. Für zustandsbehaftete zuverlässige Dienste wird `RunAsync()` erneut aufgerufen, wenn der Dienst vom primären Replikat tiefer gestuft und dann wieder auf das primäre Replikat höher gestuft wird.
* Wenn ein Dienst durch eine unerwartete Ausnahme von `RunAsync()` aus beendet wird, ist dies ein Fehler. Das Dienstobjekt wird heruntergefahren, und ein Integritätsfehler wird gemeldet.
* Zwar gibt es keine zeitliche Begrenzung für die Rückgabe dieser Methoden, aber Sie verlieren sofort die Möglichkeit zum Schreiben in Reliable Collections und können daher Ihre eigentlichen Arbeiten nicht abschließen. Es wird empfohlen, dass Sie sie so schnell wie möglich nach dem Empfang der Abbruchanforderung zurückgeben. Wenn der Dienst nicht in einem angemessenen Zeitraum auf diese API-Aufrufe reagiert, kann Service Fabric das Beenden des Diensts erzwingen. Dies geschieht normalerweise nur während Anwendungsupgrades oder beim Löschen eines Diensts. Das Timeout beträgt standardmäßig 15 Minuten.
* Für zustandsbehaftete Dienste gibt es auf ServiceReplicaListeners eine zusätzliche Option, die ihnen das Starten auf sekundären Replikaten ermöglicht. Dies ist ungewöhnlich, aber die einzige Änderung in den Lebenszyklen besteht darin, dass `CreateServiceReplicaListeners()` auch dann aufgerufen wird (und die resultierenden Listener geöffnet werden), wenn das Replikat ein sekundäres Replikat ist. Wenn das Replikat später in ein primäres konvertiert wird, werden in ähnlicher Weise die Listener geschlossen und zerstört. Im Rahmen der Änderung in ein primäres Replikat werden neue erstellt und geöffnet.
* Fehler im `OnCloseAsync()`-Pfad führen zu einem Aufruf von `OnAbort()`. So erhält der Dienst eine letzte Gelegenheit zum Bereinigen und Freigeben aller beanspruchten Ressourcen.

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Reliable Services](service-fabric-reliable-services-introduction.md)
* [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
* [Erweiterte Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md)



<!--HONumber=Jan17_HO1-->


