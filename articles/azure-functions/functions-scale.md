---
title: "Verbrauchs- und App Service-Pläne für Azure Functions | Microsoft-Dokumentation"
description: "Erfahren Sie, wie sich Azure Functions skalieren lässt, um die Anforderungen Ihrer ereignisgesteuerten Workloads zu erfüllen."
services: functions
documentationcenter: na
author: lindydonna
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 0e677fb35279d155241a95cd5f33b63e8294fad2
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="azure-functions-consumption-and-app-service-plans"></a>Verbrauchs- und App Service-Pläne für Azure Functions 

## <a name="introduction"></a>Einführung

Sie können Azure Functions in zwei verschiedenen Modi ausführen: als Verbrauchsplan und als App Service-Plan. Der Verbrauchsplan weist automatisch Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wird der Code nicht mehr ausgeführt, wird die Leistung wieder herunterskaliert. Deshalb müssen Sie für VMs im Leerlauf nicht bezahlen und auch keine Kapazitäten im Voraus reservieren. Dieser Artikel konzentriert sich auf den Verbrauchsplan. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Wenn Sie mit Azure Functions nicht vertraut sind, informieren Sie sich in der [Übersicht über Azure Functions](functions-overview.md).

Wenn Sie eine Funktions-App erstellen, müssen Sie einen Hostingplan für die in der App enthaltenen Funktionen konfigurieren. In beiden Modi führt eine Instanz des *Azure Functions-Hosts* die Funktionen aus. Der Plantyp steuert:

* Wie Hostinstanzen horizontal hochskaliert werden.
* Die Ressourcen, die für jeden Host verfügbar sind.

Derzeit müssen Sie den Plantyp während der Erstellung der Funktions-App auswählen. Sie können ihn anschließend nicht ändern. 

Sie können zwischen verschiedenen Stufen im App Service-Plan skalieren. Im Verbrauchsplan verarbeitet Azure Functions automatisch alle Ressourcenzuordnungen.

## <a name="consumption-plan"></a>Verbrauchsplan

Bei Verwendung eines Verbrauchsplans werden Instanzen des Azure Functions-Hosts dynamisch, basierend auf der Anzahl der eingehenden Ereignisse hinzugefügt und entfernt. Dieser Plan wird automatisch skaliert, sodass Ihnen nur dann Computeressourcen berechnet werden, wenn Ihre Funktionen ausgeführt werden. In einem Verbrauchsplan kann eine Funktion für maximal 10 Minuten ausgeführt werden. 

> [!NOTE]
> Das Standardtimeout für Funktionen in einem Verbrauchsplan beträgt 5 Minuten. Der Wert kann durch Ändern der Eigenschaft `functionTimeout` in [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) für die Funktions-App auf 10 Minuten erhöht werden.

Die Abrechnung basiert auf der Ausführungszeit und dem verwendeten Arbeitsspeicher. Sie wird für alle Funktionen innerhalb einer Funktions-App zusammengefasst. Weitere Informationen finden Sie unter [Preisseite für Azure Functions].

Der Verbrauchsplan ist der Standardplan. Er bietet folgende Vorteile:
- Sie bezahlen nur, wenn Ihre Funktionen ausgeführt werden.
- Das horizontale Hochskalieren erfolgt automatisch – selbst in Zeiten hoher Lasten.

## <a name="app-service-plan"></a>App Service-Plan

In einem App Service-Plan werden Ihre Funktions-Apps ähnlich wie Web-Apps auf dedizierten virtuellen Computern für Basic-, Standard- und Premium-SKUs ausgeführt. Die dedizierten virtuellen Computer werden Ihren App Service-Apps zugeordnet, sodass der Functions-Host immer ausgeführt wird.

Entscheiden Sie sich in den folgenden Fällen für einen App Service-Plan:
- Sie verfügen über nicht ausgelastete virtuelle Computer, auf denen bereits andere App Service-Instanzen ausgeführt werden.
- Sie gehen davon aus, dass Ihre Funktionen-Apps kontinuierlich oder nahezu kontinuierlich ausgeführt werden. In diesem Fall kann ein App Service-Plan kostengünstiger sein.
- Sie benötigen weitere CPU- oder Arbeitsspeicheroptionen zusätzlich zu den für den Verbrauchsplan bereitgestellten.
- Sie benötigen eine längere Ausführungsdauer als die für den Verbrauchsplan zulässige Höchstdauer.
- Sie benötigen Funktionen, die nur mit einem App Service-Plan verfügbar sind, etwa Unterstützung für App Service-Umgebung, VNET/VPN-Konnektivität und größere virtuelle Computer. 

Mit einem virtuellen Computer werden Kosten von der Laufzeit und der Arbeitsspeichergröße entkoppelt. Daher wird Ihnen nicht mehr berechnet als die Kosten für die VM-Instanz, die Sie belegen. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Mit einem App Service-Plan können Sie manuell horizontal hochskalieren, indem Sie weitere Instanzen von virtuellen Computern hinzufügen, oder Sie können die automatische Skalierung aktivieren. Weitere Informationen finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Sie können auch zentral hochskalieren, indem Sie einen anderen App Service-Plan auswählen. Weitere Informationen finden Sie unter [Zentrales Hochskalieren einer App in Azure](../app-service-web/web-sites-scale.md). Wenn Sie beabsichtigen, JavaScript-Funktionen für einen App Service-Plan zu verwenden, sollten Sie einen Plan mit einer kleineren Anzahl von Kernen auswählen. Weitere Informationen finden Sie in der [JavaScript-Referenz für Funktionen](functions-reference-node.md#choose-single-core-app-service-plans).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### Always On

Wenn Sie einen App Service-Plan verwenden, sollten Sie die Einstellung **Always On** aktivieren, damit Ihre Funktions-App ordnungsgemäß ausgeführt wird. Bei einem App Service-Plan geht die Functions-Runtime nach wenigen Minuten der Inaktivität in den Leerlauf über, sodass nur HTTP-Trigger Ihre Funktionen tatsächlich „reaktivieren“ können. Dies ist ähnlich wie bei WebJobs, bei denen Always On aktiviert sein muss. 

Always On ist nur bei einem App Service-Plan verfügbar. Bei einem Verbrauchsplan aktiviert die Plattform Funktions-Apps automatisch.

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Sowohl bei einem Verbrauchs- als auch bei einem App Service-Plan erfordert eine Funktions-App ein Azure Storage-Konto, das Blob, Queue und Table Storage unterstützt. Intern verwendet Azure Functions Azure Storage für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten (darunter Storage Premium) und allgemeine Speicherkonten mit zonenredundanter Speicherreplikation. Diese Konten werden aus dem Blatt **Speicherkonto** herausgefiltert, wenn Sie eine Funktions-App erstellen.

Weitere Informationen zu Speicherkontentypen finden Sie unter [Einführung in die Azure Storage-Dienste](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Funktionsweise des Verbrauchsplans

Der Verbrauchsplan skaliert CPU- und Arbeitsspeicherressourcen automatisch, indem dem Functions-Host basierend auf der Anzahl der Ereignisse, nach denen die Funktionen ausgelöst werden, weitere Instanzen hinzugefügt werden. Jede Instanz des Functions-Hosts ist auf 1,5 GB Arbeitsspeicher beschränkt.

Wenn Sie den verbrauchsbasierten Hostingplan verwenden, werden die Funktionscodedateien in Azure Files-Freigaben im Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

> [!NOTE]
> Bei Verwendung eines Blobtriggers in einem Verbrauchsplan kann es möglicherweise bis zu 10 Minuten dauern, bis neue Blobs verarbeitet werden, nachdem eine Funktions-App in den Leerlauf gewechselt ist. Sobald die Funktions-App ausgeführt wird, werden die Blobs sofort verarbeitet. Um diese anfängliche Verzögerung zu vermeiden, sollten Sie eine der folgenden Möglichkeiten erwägen:
> - Verwenden Sie einen App Service-Plan mit aktivierter Option „Always On“.
> - Verwenden Sie einen anderen Mechanismus zum Auslösen der Blobverarbeitung, z.B. eine Warteschlangennachricht, die den Blobnamen enthält. Ein Beispiel finden Sie unter [Warteschlangentrigger mit Blobeingangsbindung](functions-bindings-storage-blob.md#input-sample).

### <a name="runtime-scaling"></a>Laufzeitskalierung

Azure Functions verwendet die Komponente *Skalierungscontroller*, um die Rate der Ereignisse zu überwachen und zu bestimmen, ob eine Hoch- oder Herunterskalierung erforderlich ist. Der Skalierungscontroller verwendet Heuristik für jeden Triggertyp. Bei Verwendung eines Triggers für Azure Queue Storage beispielsweise basieren die Skalen auf der Länge der Warteschlange und dem Alter der ältesten Nachricht in der Warteschlange.

Die Skalierungseinheit ist die Funktionen-App. Bei einer horizontalen Hochskalierung der Funktions-App werden zusätzliche Ressourcen zugeordnet, um mehrere Instanzen des Azure Functions-Hosts auszuführen. Umgekehrt entfernt der Skalierungscontroller bei abnehmenden Computeanforderungen Instanzen des Functions-Hosts. Die Anzahl der Instanzen wird schließlich zentral auf null herunterskaliert, wenn in einer Funktionen-App keine Funktionen ausgeführt werden.

![Skalierungscontroller: Überwachen von Ereignissen und Erstellen von Instanzen](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Abrechnungsmodell

Die Abrechnung des Verbrauchsplans wird detailliert auf der [Preisseite für Azure Functions] beschrieben. Der Verbrauch wird auf Ebene der Funktions-App zusammengefasst, wobei nur die Zeit gezählt wird, für die der Funktionscode ausgeführt wird. Folgende Einheiten werden für die Abrechnung verwendet: 
* **Ressourcenverbrauch in Gigabytesekunden (GB-s)** – berechnet als Kombination aus Arbeitsspeichergröße und Ausführungsdauer für alle Funktionen in einer Funktions-App. 
* **Ausführungen** – werden bei jeder Ausführung einer Funktion als Antwort auf ein Ereignis gezählt, das durch eine Bindung ausgelöst wurde.

[Preisseite für Azure Functions]: https://azure.microsoft.com/pricing/details/functions

