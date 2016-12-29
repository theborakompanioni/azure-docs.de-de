---
title: Skalieren von Azure Functions | Microsoft Docs
description: "Erfahren Sie, wie sich Azure Functions skalieren lässt, um die Anforderungen Ihrer ereignisgesteuerten Workloads zu erfüllen."
services: functions
documentationcenter: na
author: dariagrigoriu
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2016
ms.author: dariagrigoriu
translationtype: Human Translation
ms.sourcegitcommit: 1a8e8bdb95b1eff6feb992727d28051463710813
ms.openlocfilehash: fce766358c61494e77464a4506dcc0e466e4e1e4


---
# <a name="scaling-azure-functions"></a>Skalieren von Azure Functions

## <a name="introduction"></a>Einführung

Die Azure Functions-Plattform weist Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wenn der Code nicht mehr ausgeführt wird, wird die Leistung wieder horizontal herunterskaliert. Das bedeutet, dass Sie nicht für virtuelle Computer im Leerlauf bezahlen oder Kapazitäten reservieren müssen, bevor diese benötigt werden. Der Mechanismus für diese neue Funktion ist der verbrauchsbasierte Serviceplan. Dieser Artikel bietet eine Übersicht über die Funktionsweise des verbrauchsbasierten Serviceplans. 

Wenn Sie mit Azure Functions noch nicht vertraut sind, informieren Sie sich im Artikel [Übersicht über Azure Functions](functions-overview.md).

## <a name="choose-a-service-plan"></a>Auswählen eines Serviceplans

Wenn Sie eine Funktionen-App erstellen, müssen Sie einen Hostingplan für die in der App enthaltenen Funktionen konfigurieren. Die verfügbaren Hostingpläne sind der **Verbrauchsplan** und der [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Zurzeit muss diese Auswahl während der Erstellung der Funktionen-App getroffen werden. Nach dem Erstellen können Sie nicht mehr zwischen diesen beiden Optionen wechseln. Sie können zwischen verschiedenen Stufen im [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) skalieren. Für den verbrauchsbasierten Verbrauchsplan werden Änderungen derzeit nicht unterstützt, da die Skalierung dynamisch erfolgt.

### <a name="consumption-plan"></a>Verbrauchsplan

Im **Verbrauchsplan** werden Ihre Funktionen-Apps einer Verarbeitungsinstanz zugewiesen. Bei Bedarf werden Instanzen dynamisch hinzugefügt oder entfernt. Darüber hinaus werden Ihre Funktionen parallel ausgeführt. Dadurch wird die Gesamtzeit minimiert, die für die Verarbeitung von Anforderungen benötigt wird. Die Ausführungsdauer für jede Funktion wird von der enthaltenen Funktionen-App aggregiert. Die Kosten basieren auf der Arbeitsspeichergröße und der Gesamtausführungsdauer aller Funktionen in einer Funktionen-App, gemessen in Gigabytesekunden. Dies ist eine hervorragende Möglichkeit, wenn Sie nur von Zeit zu Zeit Rechenleistung benötigen oder die Laufzeiten Ihrer Aufträge in der Regel sehr kurz sind, da Sie nur dann für Serverressourcen bezahlen, wenn diese auch tatsächlich genutzt werden. 

### <a name="app-service-plan"></a>App Services-Plan

In einem **App Service-Plan** werden Ihre Funktionen-Apps auf dedizierten virtuellen Computern ausgeführt – auf die gleiche Weise, wie heute Web-Apps für Basic-, Standard- oder Premium-SKUs funktionieren. Dedizierte virtuelle Computer sind Ihren App Service-Apps und Funktionen-Apps zugeordnet und stehen immer zur Verfügung, unabhängig davon, ob Code aktiv ausgeführt wird oder nicht. Diese Option eignet sich besonders, wenn Sie über nicht ausgelastete virtuelle Computer verfügen, auf denen bereits anderer Code ausgeführt wird, oder wenn Sie Funktionen kontinuierlich oder nahezu kontinuierlich ausführen möchten. Mit einem virtuellen Computer werden Kosten von der Laufzeit und der Arbeitsspeichergröße entkoppelt. Daher können Sie die Kosten für viele Funktionen mit langer Ausführungsdauer auf die Kosten für einen oder mehrere virtuelle Computer begrenzen, auf denen sie ausgeführt werden.

## <a name="consumption-service-plan"></a>Verbrauchsbasierter Serviceplan

Der verbrauchsbasierte Serviceplan skaliert CPU- und Arbeitsspeicherressourcen automatisch, indem basierend auf den Laufzeitanforderungen der Funktionen in der Funktionen-App weitere Verarbeitungsinstanzen hinzugefügt werden. Jeder Verarbeitungsinstanz der Funktionen-App werden bis zu 1,5 GB Arbeitsspeicherressourcen zugewiesen.

### <a name="runtime-scaling"></a>Laufzeitskalierung

Die Azure Functions-Plattform verwendet einen zentralen Listener, um Computeanforderungen basierend auf den konfigurierten Triggern zu berechnen und zu entscheiden, wann horizontal hoch- oder herunterskaliert werden soll. Der zentrale Listener verarbeitet kontinuierlich Hinweise auf Arbeitsspeicheranforderungen und triggerspezifische Datenpunkte. Im Fall eines Triggers für Azure Queue Storage beispielsweise umfassen die Datenpunkte die Länge der Warteschlange und die Uhrzeit des ältesten Eintrags in der Warteschlange.

![](./media/functions-scale/central-listener.png)

Die Skalierungseinheit ist die Funktionen-App. Horizontales Hochskalieren bedeutet in diesem Fall, weitere Instanzen einer Funktionen-App hinzuzufügen. Wenn die Computeanforderungen sinken, bedeutet dies umgekehrt, dass Instanzen der Funktionen-App entfernt werden. Wenn keine Instanzen mehr ausgeführt werden, erfolgt eine Skalierung auf Null. 

### <a name="billing-model"></a>Abrechnungsmodell

Die Abrechnung des verbrauchsbasierten Serviceplans wird detailliert auf der [Preisseite für Azure Functions](https://azure.microsoft.com/pricing/details/functions) beschrieben. Die Nutzung wird pro Funktionen-App gemeldet und nur für den Zeitraum, in dem Code ausgeführt wird. Folgende Einheiten werden für die Abrechnung verwendet: 
* **Ressourcenverbrauch in GB-s (Gigabytesekunden)** – berechnet als Kombination aus Arbeitsspeichergröße und Ausführungsdauer für alle in einer Funktionen-App ausgeführten Funktionen. 
* **Ausführungen** – werden bei jeder Ausführung einer Funktion als Antwort auf ein Ereignis gezählt, das durch eine Bindung ausgelöst wurde.



<!--HONumber=Nov16_HO4-->


