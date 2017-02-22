---
title: "Bewährte Methoden für Azure Functions | Microsoft-Dokumentation"
description: "Enthält Beschreibungen der bewährten Methoden und Muster für Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure Functions, Muster, bewährte Methoden, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/09/2016
ms.author: wesmc
translationtype: Human Translation
ms.sourcegitcommit: 182e28e37eb56c547e28524f2a3e13f042238cb4
ms.openlocfilehash: c638bf42b9adf906f195d77268637d056f7b00a9

---

# <a name="best-practices-for-azure-functions"></a>Bewährte Methoden für Azure Functions

##<a name="overview"></a>Übersicht

Dieser Artikel enthält eine Sammlung mit den bewährten Methoden, die Sie beim Implementieren von Funktionen-Apps nutzen können. Beachten Sie, dass Ihre Azure-Funktionen-App ein Azure App Service ist. Diese bewährten Methoden sind also relevant.


## <a name="avoid-large-long-running-functions"></a>Vermeiden von Funktionen mit langer Ausführungsdauer

Umfangreiche Funktionen mit langer Ausführungsdauer können zu unerwarteten Zeitüberschreitungsfehlern führen. Eine Funktion kann umfangreich werden, wenn sie über viele Node.js-Abhängigkeiten verfügt. Das Importieren dieser Abhängigkeiten kann zu längeren Ladezeiten und somit zu unerwarteten Zeitüberschreitungen führen. Node.js-Abhängigkeiten können von mehreren `require()`-Anweisungen in Ihrem Code explizit geladen werden. Sie können auch impliziter Art sein, wenn sie auf einem einzelnen Modul mit eigenen internen Abhängigkeiten basieren, das durch Ihren Code geladen wird.  

Nach Möglichkeit sollten Sie umfangreiche Funktionen immer in kleinere Funktionssätze unterteilen, die zusammenarbeiten und schnelle Reaktionen ermöglichen. Für einen Webhook oder eine HTTP-Triggerfunktion ist unter Umständen eine Bestätigungsantwort innerhalb eines bestimmten Zeitraums erforderlich. Sie können die HTTP-Triggernutzlast an eine Warteschlange übergeben, damit sie von einer Funktion des Warteschlangentriggers verarbeitet wird. Dieser Ansatz ermöglicht es Ihnen, die eigentliche Arbeit zurückzustellen und sofort eine Antwort zurückzugeben. Es ist üblich, dass für Webhooks eine sofortige Antwort erforderlich ist.


## <a name="cross-function-communication"></a>Funktionsübergreifende Kommunikation:

Beim Integrieren von mehreren Funktionen besteht die bewährte Methode im Allgemeinen darin, für die funktionsübergreifende Kommunikation Speicherwarteschlangen zu verwenden.  Der Hauptgrund ist, dass Speicherwarteschlangen kostengünstiger und deutlich einfacher bereitzustellen sind. 

Einzelne Nachrichten in einer Speicherwarteschlange sind auf eine Größe von 64 KB beschränkt. Wenn Sie größere Nachrichten zwischen Funktionen übergeben müssen, kann eine Azure Service Bus-Warteschlange verwendet werden, um Nachrichtengrößen von bis zu 256 KB zu unterstützen.

Service Bus-Themen sind nützlich, wenn die Nachrichten vor der Verarbeitung gefiltert werden sollen.

Event Hubs sind hilfreich, um die Kommunikation mit hohen Volumina zu unterstützen.



## <a name="write-functions-to-be-stateless"></a>Schreiben von zustandslosen Funktionen 

Funktionen sollten nach Möglichkeit zustandslos und idempotent sein. Ordnen Sie Ihren Daten alle erforderlichen Zustandsinformationen zu. Einer Bestellung, die verarbeitet wird, ist beispielsweise meist ein `state`-Member zugeordnet. Eine Funktion kann eine Bestellung basierend auf diesem Zustand verarbeiten, während die Funktion selbst zustandslos bleibt. 

Idempotente Funktionen sind besonders bei Triggern mit Timer zu empfehlen. Wenn bei Ihnen beispielsweise eine bestimmte Komponente immer einmal am Tag ausgeführt werden muss, sollten Sie sie so schreiben, dass sie zu einer beliebigen Tageszeit ausgeführt werden kann und immer die gleichen Ergebnisse liefert. Die Funktion kann beendet werden, wenn für einen bestimmten Tag keine Arbeit vorhanden ist. Falls die letzte Ausführung nicht abgeschlossen werden konnte, sollte die nächste Ausführung am entsprechenden Punkt fortgesetzt werden.


## <a name="write-defensive-functions"></a>Schreiben Sie defensive Funktionen.

Gehen Sie davon aus, dass es für Ihre Funktion jederzeit zu einer Ausnahme kommen kann. Entwerfen Sie Ihre Funktionen so, dass bei der nächsten Ausführung an einem vorherigen Fehlerpunkt angeknüpft werden kann. Stellen Sie sich ein Szenario mit den folgenden Aktionen vor:

1. Abfrage von 10.000 Zeilen in einer Datenbank.
2. Erstellen Sie eine Warteschlangennachricht für jede Zeile, um die spätere Verarbeitung zu ermöglichen.
 
Je nach Komplexität Ihres Systems verfügen Sie ggf. über Folgendes: fehlerhaftes Verhalten von nachgelagerten Diensten, Netzwerkausfälle, Erreichung von Kontingentgrenzen usw. Alle diese Faktoren können sich jederzeit auf Ihre Funktion auswirken. Sie müssen Ihre Funktionen entsprechend entwerfen, um darauf vorbereitet zu sein.

Wie reagiert Ihr Code, wenn nach dem Einfügen von 5.000 dieser Elemente in eine Warteschlange zur Verarbeitung ein Fehler auftritt? Verfolgen Sie, welche Elemente eines Satzes bereits abgeschlossen sind. Andernfalls kann es ein, dass Sie sie beim nächsten Mal erneut einfügen. Dies kann schwerwiegende Auswirkungen auf Ihren Workflow haben. 

Wenn ein Warteschlangenelement bereits verarbeitet wurde, sollte es möglich sein, dass die Funktion eine No-Op-Funktion ist.

Nutzen Sie Verteidigungsmaßnahmen, die für auf der Azure Functions-Plattform verwendete Komponenten bereits bereitgestellt wurden. Informationen hierzu finden Sie beispielsweise unter **Behandeln von Nachrichten in der Warteschlange für nicht verarbeitbare Nachrichten** in der Dokumentation zu [Azure Storage-Warteschlangentriggern](functions-bindings-storage-queue.md#trigger).
 



## <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Vermeiden Sie es, Test- und Produktionscodes in der derselben Funktionen-App zu mischen.

Für Funktionen innerhalb einer Funktionen-App werden Ressourcen gemeinsam genutzt. Dies gilt beispielsweise für den Arbeitsspeicher. Wenn Sie eine Funktionen-App in der Produktion verwenden, sollten Sie ihr keine testbezogenen Funktionen und Ressourcen hinzufügen. Bei der Ausführung des Produktionscodes kann dies zu unerwartetem Mehraufwand führen.

Überlegen Sie sich gut, was Sie in Ihre Funktionen-Apps für die Produktion laden. Der Arbeitsspeicher wird gleichmäßig auf die einzelnen Funktionen der App verteilt.

Wenn Sie eine gemeinsame Assembly nutzen, auf die in mehreren .NET-Funktionen verwiesen wird, sollten Sie sie in einen freigegebenen Ordner einfügen. Verweisen Sie mit einer Anweisung wie im folgenden Beispiel auf die Assembly: 

    #r "..\Shared\MyAssembly.dll". 

Andernfalls ist es leicht möglich, dass Sie versehentlich mehrere Testversionen einer Binärdatei bereitstellen, die sich für die einzelnen Funktionen unterschiedlich verhalten.

Verwenden Sie im Produktionscode keine ausführliche Protokollierung. Dies wirkt sich negativ auf die Leistung aus.



## <a name="use-async-code-but-avoid-taskresult"></a>Verwenden von asynchronem Code bei Vermeidung von „Task.Result“

Die asynchrone Programmierung wird als bewährte Methode empfohlen. Vermeiden Sie es aber stets, auf die `Task.Result`-Eigenschaft zu verweisen. Bei diesem Ansatz wird für eine Sperre eines anderen Threads im Wesentlichen ein Busy-Wait-Vorgang durchgeführt. Die Aufrechterhaltung einer Sperre birgt Potenzial für Deadlocks.




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)




<!--HONumber=Feb17_HO2-->


