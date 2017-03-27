---
title: "Auswählen eines Azure Functions-Hostingplans | Microsoft Docs"
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
ms.date: 03/14/2017
ms.author: dariagrigoriu, glenga
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 9b5dabe5e27e68a4a9f140d4f07131caf7306e32
ms.lasthandoff: 03/15/2017


---
# <a name="choose-the-correct-service-plan-for-azure-functions"></a>Auswählen des richtigen Serviceplans für Azure Functions

## <a name="introduction"></a>Einführung

Azure Functions bietet zwei verschiedene Servicepläne: Verbrauchsplan und App Service-Plan. Der Verbrauchsplan weist automatisch Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wenn der Code nicht mehr ausgeführt wird, wird die Leistung wieder horizontal herunterskaliert. Das bedeutet, dass Sie nicht für virtuelle Computer im Leerlauf bezahlen oder Kapazitäten reservieren müssen, bevor diese benötigt werden. Dieser Artikel konzentriert sich auf den verbrauchsbasierten Serviceplan. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Wenn Sie mit Azure Functions noch nicht vertraut sind, informieren Sie sich im Artikel [Übersicht über Azure Functions](functions-overview.md).

## <a name="service-plan-options"></a>Serviceplanoptionen

Wenn Sie eine Funktionen-App erstellen, müssen Sie einen Hostingplan für die in der App enthaltenen Funktionen konfigurieren. Die verfügbaren Hostingpläne sind der **Verbrauchsplan** und der [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Zurzeit muss diese Auswahl während der Erstellung der Funktionen-App getroffen werden. Nach dem Erstellen können Sie nicht mehr zwischen diesen beiden Optionen wechseln. Sie können zwischen verschiedenen Stufen im [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) skalieren. Für den verbrauchsbasierten Verbrauchsplan werden Änderungen derzeit nicht unterstützt, da die Skalierung dynamisch erfolgt.

### <a name="consumption-plan"></a>Verbrauchsplan

Im **Verbrauchsplan** werden Ihre Funktionen-Apps einer Verarbeitungsinstanz zugewiesen. Bei Bedarf werden Instanzen dynamisch hinzugefügt oder entfernt. Darüber hinaus werden Ihre Funktionen parallel ausgeführt. Dadurch wird die Gesamtzeit minimiert, die für die Verarbeitung von Anforderungen benötigt wird. Die Ausführungsdauer für jede Funktion wird von der enthaltenden Funktionen-App aggregiert. Die Kosten basieren auf der Arbeitsspeichergröße und der Gesamtausführungsdauer aller Funktionen in einer Funktionen-App, gemessen in Gigabytesekunden. Dies ist eine hervorragende Möglichkeit, wenn Sie nur von Zeit zu Zeit Rechenleistung benötigen oder die Laufzeiten Ihrer Aufträge in der Regel sehr kurz sind, da Sie nur dann für Serverressourcen bezahlen, wenn diese auch tatsächlich genutzt werden. Der nächste Abschnitt enthält Details zur Funktionsweise des Verbrauchsplans.

### <a name="app-service-plan"></a>App Service-Plan

In einem **App Service-Plan** werden Ihre Funktionen-Apps auf dedizierten virtuellen Computern ausgeführt – auf die gleiche Weise, wie heute Web-Apps für Basic-, Standard- oder Premium-SKUs funktionieren. Dedizierte virtuelle Computer sind Ihren App Service-Apps und Funktionen-Apps zugeordnet und stehen immer zur Verfügung, unabhängig davon, ob Code aktiv ausgeführt wird oder nicht. Diese Option eignet sich besonders, wenn Sie über nicht ausgelastete virtuelle Computer verfügen, auf denen bereits anderer Code ausgeführt wird, oder wenn Sie Funktionen kontinuierlich oder nahezu kontinuierlich ausführen möchten. Mit einem virtuellen Computer werden Kosten von der Laufzeit und der Arbeitsspeichergröße entkoppelt. Daher können Sie die Kosten für viele Funktionen mit langer Ausführungsdauer auf die Kosten für die virtuellen Computer begrenzen, auf denen sie ausgeführt werden. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

## <a name="how-the-consumption-plan-works"></a>Funktionsweise des Verbrauchsplans

Der Verbrauchsplan skaliert CPU- und Arbeitsspeicherressourcen automatisch, indem basierend auf den Laufzeitanforderungen der Funktionen in der Funktionen-App weitere Verarbeitungsinstanzen hinzugefügt werden. Jeder Verarbeitungsinstanz der Funktionen-App werden bis zu 1,5 GB Arbeitsspeicherressourcen zugewiesen.

Wenn bei der Ausführung eines Verbrauchsplans eine App-Funktion im Leerlauf ist, kann es möglicherweise bis zu 10 Minuten dauern, bis neue Blobs erstellt werden. Sobald die Funktionen-App ausgeführt wird, werden die Blobs schneller verarbeitet. Um diese anfängliche Verzögerung zu vermeiden, verwenden Sie entweder einen regulären App Service-Plan mit aktivierter Always On-Option oder einen anderen Mechanismus, um die Blobverarbeitung auszulösen, z.B. eine Warteschlangennachricht mit dem Blobnamen. 

Beim Erstellen einer Funktionen-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Warteschlangen- und Tabellenspeicher unterstützt. Intern verwendet Azure Functions Azure Storage für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten (darunter Storage Premium) und allgemeine Speicherkonten mit Replikation von ZRS. Diese Konten werden aus dem Blatt „Speicherkonto“ herausgefiltert, wenn eine neue Funktionen-App erstellt wird.

Wenn der verbrauchsbasierte Hostingplan verwendet wird, wird der Inhalt von Funktionen-Apps (z.B. Funktionscodedateien und Bindungskonfigurationen) in Azure Files-Freigaben auf dem Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

Weitere Informationen zu Typen von Speicherkonten finden Sie unter [Einführung in die Azure Storage-Dienste] (../storage/storage-introduction.md#introducing-the-azure-storage-services).

### <a name="runtime-scaling"></a>Laufzeitskalierung

Funktionen verwenden einen Skalierungscontroller, um Computeanforderungen basierend auf den konfigurierten Triggern zu bewerten und zu entscheiden, wann horizontal hoch- oder herunterskaliert werden soll. Der Skalierungscontroller verarbeitet kontinuierlich Hinweise auf Arbeitsspeicheranforderungen und triggerspezifische Datenpunkte. Im Fall eines Triggers für Azure Queue Storage beispielsweise umfassen die Datenpunkte die Länge der Warteschlange und die Uhrzeit des ältesten Eintrags in der Warteschlange.

![](./media/functions-scale/central-listener.png)

Die Skalierungseinheit ist die Funktionen-App. Horizontales Hochskalieren bedeutet in diesem Fall, weitere Instanzen einer Funktionen-App hinzuzufügen. Umgekehrt werden bei abnehmenden Computeanforderungen Instanzen der Funktionen-App entfernt. Die Anzahl der Instanzen wird schließlich vertikal auf null herunterskaliert, wenn keine ausgeführt werden. 

### <a name="billing-model"></a>Abrechnungsmodell

Die Abrechnung des Verbrauchsplans wird detailliert auf der [Preisseite für Azure Functions](https://azure.microsoft.com/pricing/details/functions) beschrieben. Die Nutzung wird pro Funktionen-App gemeldet – und zwar nur für den Zeitraum, in dem Code ausgeführt wird. Folgende Einheiten werden für die Abrechnung verwendet: 
* **Ressourcenverbrauch in GB-s (Gigabytesekunden)** – berechnet als Kombination aus Arbeitsspeichergröße und Ausführungsdauer für alle in einer Funktionen-App ausgeführten Funktionen. 
* **Ausführungen** – werden bei jeder Ausführung einer Funktion als Antwort auf ein Ereignis gezählt, das durch eine Bindung ausgelöst wurde.

