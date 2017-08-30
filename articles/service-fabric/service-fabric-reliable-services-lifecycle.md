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
ms.date: 08/18/2017
ms.author: masnider
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 3ff04f031b2d170ac63fc1c6f4e2b6ba6a6c46bd
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---

# <a name="reliable-services-lifecycle-overview"></a>Übersicht über den Lebenszyklus von Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java unter Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Bei der Betrachtung der Lebenszyklen von Reliable Services sind vor allem die Grundlagen zum Lebenszyklus relevant. Im Allgemeinen:

- Während des Starts
  - Dienste werden erstellt.
  - Sie haben eine Möglichkeit zum Erstellen und Zurückgeben von null oder mehr Listenern.
  - Alle zurückgegebenen Listener werden geöffnet und ermöglichen so eine Kommunikation mit dem Dienst.
  - Die RunAsync-Methode des Diensts wird aufgerufen und ermöglicht dem Dienst die Ausführung langwieriger Aufgaben oder von Hintergrundarbeit.
- Während des Herunterfahrens
  - Das an RunAsync übergebene Abbruchtoken wird abgebrochen, und die Listener werden geschlossen.
  - Sobald dies abgeschlossen ist, wird das Dienstobjekt selbst zerstört.

Es gibt Informationen zur genauen Reihenfolge dieser Ereignisse. Insbesondere kann sich die Reihenfolge der Ereignisse geringfügig ändern, je nachdem, ob der zuverlässigen Dienst zustandslos oder zustandsbehaftet ist. Für zustandsbehaftete Dienste müssen wir uns außerdem mit dem Tauschszenario für primäre Replikate befassen. Während dieser Sequenz wird die Rolle des primären Replikats auf ein anderes Replikat (oder wieder zurück) übertragen, ohne dass der Dienst heruntergefahren wird. Schließlich müssen wir uns mit Fehlerbedingungen beschäftigen.

## <a name="stateless-service-startup"></a>Start zustandsloser Dienste
Der Lebenszyklus eines zustandslosen Diensts ist recht einfach. So sieht die Reihenfolge der Ereignisse aus:

1. Der Dienst wird erstellt.
2. Gleichzeitig geschehen zwei Dinge:
    - `StatelessService.CreateServiceInstanceListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. `ICommunicationListener.OpenAsync()` wird für jeden Listener aufgerufen.
    - Die Methode `StatelessService.RunAsync()` des Diensts wird aufgerufen.
3. Die Methode `StatelessService.OnOpenAsync()` des Diensts wird aufgerufen (sofern vorhanden). Diese Außerkraftsetzung ist ungewöhnlich, aber verfügbar.

Beachten Sie unbedingt, dass es keine Reihenfolge zwischen den Aufrufen zum Erstellen und Öffnen der Listener und RunAsync gibt. Die Listener werden möglicherweise vor dem Start von RunAsync geöffnet. Ebenso wird RunAsync möglicherweise aufgerufen, bevor die Kommunikationslistener geöffnet sind oder überhaupt erstellt wurden. Wenn eine Synchronisierung erforderlich ist, wird diese dem Implementierer als Übung überlassen. Gängige Lösungen:

  - In einigen Fällen funktionieren Listener erst dann, wenn andere Informationen erstellt oder Aufgaben durchgeführt wurden. Bei zustandslosen Diensten können diese Aufgaben üblicherweise an anderen Orten ausgeführt werden. Beispiele: 
    - Im Konstruktor des Diensts
    - Im Rahmen des `CreateServiceInstanceListeners()`-Aufrufs
    - Bei der Erstellung des eigentlichen Listeners
  - In einigen Fällen lässt sich der Code in RunAsync erst starten, wenn die Listener geöffnet sind. In diesem Fall ist zusätzliche Koordination erforderlich. Eine gängige Lösung besteht in einem Flag innerhalb der Listener, das angibt, wann diese abgeschlossen wurden. Dieses Flag wird dann in RunAsync überprüft, bevor mit der eigentlichen Arbeit fortgefahren wird.

## <a name="stateless-service-shutdown"></a>Herunterfahren zustandsloser Dienste
Beim Herunterfahren eines zustandslosen Diensts wird dasselbe Muster in umgekehrter Reihenfolge befolgt:

1. Gleichzeitig:
    - Alle geöffneten Listener werden geschlossen. `ICommunicationListener.CloseAsync()` wird für jeden Listener aufgerufen.
    - Das an `RunAsync()` übergebene Abbruchtoken wird abgebrochen. Es wird überprüft, ob die Eigenschaft `IsCancellationRequested` des Abbruchtokens „true“ zurückgibt und ob die Methode `ThrowIfCancellationRequested` eine Ausnahme vom Typ `OperationCanceledException` auslöst.
2. Nach Abschluss von `CloseAsync()` auf den einzelnen Listenern und nach Abschluss von `RunAsync()` wird die Methode `StatelessService.OnCloseAsync()` des Diensts aufgerufen (sofern vorhanden). Die Außerkraftsetzung von `StatelessService.OnCloseAsync()` ist nicht üblich.
3. Nach Abschluss von `StatelessService.OnCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="stateful-service-startup"></a>Start zustandsbehafteter Dienste
Zustandsbehaftete Dienste befolgen mit wenigen Änderungen ein ähnliches Muster wie zustandslose Dienste. Beim Starten eines zustandsbehafteten Diensts lautet die Reihenfolge der Ereignisse wie folgt:

1. Der Dienst wird erstellt.
2. `StatefulServiceBase.OnOpenAsync()` wird aufgerufen. Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.
3. Folgende Vorgänge finden parallel statt:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wird aufgerufen. 
      - Falls es sich bei dem Dienst um ein primäres Replikat handelt, werden alle zurückgegebenen Listener geöffnet. `ICommunicationListener.OpenAsync()` wird für jeden Listener aufgerufen.
      - Bei einem sekundären Dienst werden nur als `ListenOnSecondary = true` gekennzeichnete Listener geöffnet. Für sekundäre Dienste geöffnete Listener sind eher selten.
    - Falls es sich bei dem Dienst gerade um ein primäres Replikat handelt, wird die Methode `StatefulServiceBase.RunAsync()` des Diensts aufgerufen.
4. Wenn alle Replikatlisteneraufrufe von `OpenAsync()` abgeschlossen sind und `RunAsync()` aufgerufen wird, wird `StatefulServiceBase.OnChangeRoleAsync()` aufgerufen. Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.

Ähnlich wie bei zustandslosen Diensten wird die Reihenfolge, in der die Listener erstellt und geöffnet werden und RunAsync aufgerufen wird, nicht koordiniert. Wenn Sie eine Koordinierung benötigen, sind die Lösungen nahezu identisch. Es gibt jedoch einen zusätzlichen Fall: Angenommen, die bei den Kommunikationslistenern eingehenden Aufrufe benötigen Informationen aus [zuverlässigen Sammlungen](service-fabric-reliable-services-reliable-collections.md). Da die Kommunikationslistener geöffnet werden könnten, bevor die zuverlässigen Sammlungen gelesen oder geschrieben werden können oder RunAsync gestartet werden kann, ist einige zusätzliche Koordination erforderlich. Die einfachste und gängigste Lösung besteht darin, dass die Kommunikationslistener einen Fehlercode zurückgeben, anhand dessen der Client feststellt, dass die Anforderung wiederholt werden muss.

## <a name="stateful-service-shutdown"></a>Herunterfahren zustandsbehafteter Dienste
Ähnlich wie bei zustandslosen Diensten stimmen die Lebenszyklusereignisse während des Herunterfahrens mit denen während des Starts überein, aber in umgekehrter Reihenfolge. Wenn ein zustandsbehafteter Dienst heruntergefahren wird, treten die folgenden Ereignisse auf:

1. Gleichzeitig:
    - Alle geöffneten Listener werden geschlossen. `ICommunicationListener.CloseAsync()` wird für jeden Listener aufgerufen.
    - Das an `RunAsync()` übergebene Abbruchtoken wird abgebrochen. Es wird überprüft, ob die Eigenschaft `IsCancellationRequested` des Abbruchtokens „true“ zurückgibt und ob die Methode `ThrowIfCancellationRequested` eine Ausnahme vom Typ `OperationCanceledException` auslöst.
2. Sobald `CloseAsync()` auf den einzelnen Listenern und auch `RunAsync()` abgeschlossen ist, wird `StatefulServiceBase.OnChangeRoleAsync()` für den Dienst aufgerufen. (Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.)
    - Auf den Abschluss eines RunAsync-Vorgangs muss nur gewartet werden, wenn es sich bei dem Dienstreplikat um ein primäres Replikat handelt.
3. Nach Abschluss der Methode `StatefulServiceBase.OnChangeRoleAsync()` wird die Methode `StatefulServiceBase.OnCloseAsync()` aufgerufen. Diese Außerkraftsetzung ist ungewöhnlich, aber verfügbar.
3. Nach Abschluss von `StatefulServiceBase.OnCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="stateful-service-primary-swaps"></a>Tausch des primären Replikats bei zustandsbehafteten Diensten
Während ein zustandsbehafteter Dienst ausgeführt wird, werden nur für die primären Replikate dieser zustandsbehafteten Dienste die Kommunikationslistener geöffnet und die RunAsync-Methode aufgerufen. Die sekundären Replikate werden erstellt, erhalten jedoch keine weiteren Aufrufe. Während ein zustandsbehafteter Dienst ausgeführt wird, kann sich jedoch ändern, welches Replikat derzeit das primäre ist. Was bedeutet dies im Hinblick auf die für ein Replikat sichtbaren Lebenszyklusereignisse? Das für das zustandsbehaftete Replikat sichtbare Verhalten hängt davon ab, ob das Replikat während des Tauschs höher oder tiefer gestuft wird.

### <a name="for-the-primary-being-demoted"></a>Für das tiefer gestufte primäre Replikat
Für Service Fabric muss dieses Replikat die Verarbeitung von Nachrichten und jegliche Hintergrundverarbeitung beenden. Folglich ähnelt dieser Schritt dem Herunterfahren des Diensts. Ein Unterschied besteht darin, dass der Dienst nicht zerstört oder geschlossen wird, weil er als sekundäres Replikat erhalten bleibt. Die folgenden APIs werden aufgerufen:

1. Gleichzeitig:
    - Alle geöffneten Listener werden geschlossen. `ICommunicationListener.CloseAsync()` wird für jeden Listener aufgerufen.
    - Das an `RunAsync()` übergebene Abbruchtoken wird abgebrochen. Es wird überprüft, ob die Eigenschaft `IsCancellationRequested` des Abbruchtokens „true“ zurückgibt und ob die Methode `ThrowIfCancellationRequested` eine Ausnahme vom Typ `OperationCanceledException` auslöst.
2. Sobald `CloseAsync()` auf den einzelnen Listenern und auch `RunAsync()` abgeschlossen ist, wird `StatefulServiceBase.OnChangeRoleAsync()` für den Dienst aufgerufen. Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.

### <a name="for-the-secondary-being-promoted"></a>Für das höher gestufte sekundäre Replikat
Analog dazu muss dieses Replikat für Service Fabric mit dem Lauschen auf Nachrichten beginnen und alle relevanten Hintergrundaufgaben starten. Demzufolge ähnelt dieser Prozess dem Erstellen des Diensts, mit der Ausnahme, dass das Replikat selbst bereits vorhanden ist. Die folgenden APIs werden aufgerufen:

1. Gleichzeitig:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. `ICommunicationListener.OpenAsync()` wird für jeden Listener aufgerufen.
    - Die Methode `StatefulServiceBase.RunAsync()` des Diensts wird aufgerufen.
2. Wenn alle Replikatlisteneraufrufe von `OpenAsync()` abgeschlossen sind und `RunAsync()` aufgerufen wurde, wird `StatefulServiceBase.OnChangeRoleAsync()` aufgerufen. Diese Methode wird ungewöhnlicherweise im Dienst außer Kraft gesetzt.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Allgemeine Probleme beim Herunterfahren zustandsbehafteter Dienste und beim Tieferstufen des primären Replikats
Service Fabric ändert das primäre Replikat eines zustandsbehafteten Diensts aus verschiedenen Gründen. Zu den gängigsten Gründen zählen [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md) und [Anwendungsupgrade](service-fabric-application-upgrade.md). Während dieser Vorgänge (sowie beim normalen Herunterfahren des Diensts – etwa, wenn der Dienst gelöscht wird) muss der Dienst das Abbruchtoken (`CancellationToken`) berücksichtigen. Bei Diensten ohne ordnungsgemäße Abbruchbehandlung treten mehrere Probleme auf. Die Vorgänge werden insbesondere verlangsamt, da Service Fabric auf eine ordnungsgemäße Beendigung der Dienste wartet. Dies kann letztendlich zu nicht erfolgreichen Upgrades mit Timeout und Rollback führen. Darüber hinaus kann die Missachtung des Abbruchtokens zu unausgeglichenen Clustern führen, da Knoten ausgelastet werden, die Dienste aber nicht ausgeglichen werden können, weil ihre Verschiebung zu lange dauert. 

Da es sich um zustandsbehaftete Dienst handelt, ist es auch wahrscheinlich, dass sie die [zuverlässigen Sammlungen](service-fabric-reliable-services-reliable-collections.md) verwenden. Beim Tieferstufen eines primären Replikats wird in Service Fabric als eine der ersten Maßnahmen der Schreibzugriff auf den zugrunde liegenden Zustand entzogen. Dies führt zu weiteren Problemen, die den Lebenszyklus des Diensts beeinträchtigen können. Die Sammlungen geben Ausnahmen auf der Grundlage des Timings und abhängig davon zurück, ob das Replikat verschoben oder heruntergefahren wird. Diese Ausnahmen müssen korrekt behandelt werden. Von Service Fabric ausgelöste Ausnahmen können dauerhaft [(`FabricException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) oder vorübergehend [(`FabricTransientException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) sein. Dauerhafte Ausnahmen müssen protokolliert und ausgelöst werden; Vorgänge mit vorübergehenden Ausnahmen können hingegen mit einer Wiederholungslogik wiederholt werden.

Die Behandlung von Ausnahmen, die auf die Verwendung von `ReliableCollections` in Verbindung mit Dienstlebenszyklusereignissen zurückzuführen sind, ist ein wichtiger Bestandteil der Tests und Prüfungen für einen zuverlässigen Dienst. Es empfiehlt sich, den Dienst während der Durchführung von Upgrades und [Chaostests](service-fabric-controlled-chaos.md) unter Last auszuführen, bevor Sie ihn in einer Produktionsumgebung bereitstellen. Mit diesen einfachen Schritten können Sie sicherstellen, dass Ihr Dienst ordnungsgemäß implementiert ist und Lebenszyklusereignisse korrekt behandelt.


## <a name="notes-on-service-lifecycle"></a>Hinweise zum Dienstlebenszyklus
  - Sowohl die `RunAsync()`-Methode als auch die `CreateServiceReplicaListeners/CreateServiceInstanceListeners`-Aufrufe sind optional. Ein Dienst kann eine dieser Optionen, beide oder keine von beiden aufweisen. Wenn der Dienst beispielsweise alle Vorgänge infolge von Benutzeraufrufen ausführt, muss `RunAsync()` nicht implementiert werden. Nur die Kommunikationslistener und der zugehörige Code sind erforderlich. Ebenso ist das Erstellen und Zurückgeben von Kommunikationslistenern optional, da der Dienst möglicherweise nur Hintergrundaufgaben ausführt und daher nur `RunAsync()` implementieren muss.
  - Ein Dienst kann `RunAsync()` erfolgreich abschließen und dann zurückkehren. Der Abschluss ist keine Fehlerbedingung. Der Abschluss von `RunAsync()` gibt an, dass die Hintergrundaufgaben des Diensts abgeschlossen sind. Bei zustandsbehafteten zuverlässigen Diensten wird `RunAsync()` erneut aufgerufen, wenn das Replikat vom primären Replikat zum sekundären Replikat tiefer gestuft und dann wieder zum primären Replikat höher gestuft wird.
  - Wenn ein Dienst die Ausführung von `RunAsync()` mit einer unerwarteten Ausnahme beendet, liegt ein Fehler vor. Das Dienstobjekt wird heruntergefahren, und ein Integritätsfehler wird gemeldet.
  - Zwar gibt es keine zeitliche Begrenzung für die Rückgabe dieser Methoden, aber Sie verlieren sofort die Möglichkeit zum Schreiben in Reliable Collections und können daher Ihre eigentlichen Arbeiten nicht abschließen. Es wird empfohlen, dass Sie sie so schnell wie möglich nach dem Empfang der Abbruchanforderung zurückgeben. Wenn der Dienst nicht in einem angemessenen Zeitraum auf diese API-Aufrufe reagiert, kann Service Fabric das Beenden des Diensts erzwingen. Dies geschieht normalerweise nur während Anwendungsupgrades oder beim Löschen eines Diensts. Das Timeout beträgt standardmäßig 15 Minuten.
  - Fehler im `OnCloseAsync()`-Pfad führen zu einem Aufruf von `OnAbort()`. So erhält der Dienst eine letzte Gelegenheit zum Bereinigen und Freigeben aller beanspruchten Ressourcen.

## <a name="next-steps"></a>Nächste Schritte
- [Einführung in Reliable Services](service-fabric-reliable-services-introduction.md)
- [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
- [Erweiterte Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md)

