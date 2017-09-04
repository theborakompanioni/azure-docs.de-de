---
title: "Gewusst wie: Hinzufügen einer Event Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: "In diesem Tutorial wird beschrieben, wie eine Ereignisquelle hinzugefügt wird, die mit einem Event Hub in Ihrer Time Series Insights-Umgebung verbunden ist."
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.translationtype: HT
ms.sourcegitcommit: 07e5e15f4f4c4281a93c8c3267c0225b1d79af45
ms.openlocfilehash: f6a993b3858cfb94dd9795f5e55f15fa6ec7dcb2
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="how-to-add-an-event-hub-event-source"></a>Gewusst wie: Hinzufügen einer Event Hub-Ereignisquelle

In diesem Tutorial wird beschrieben, wie im Azure-Portal eine Ereignisquelle hinzugefügt wird, die Daten aus einem Event Hub in Ihre Time Series Insights-Umgebung liest.

## <a name="prerequisites"></a>Voraussetzungen

Sie haben einen Event Hub erstellt, in den Sie Ereignisse schreiben. Weitere Informationen zu Event Hubs finden Sie unter <https://azure.microsoft.com/services/event-hubs/>

> [Consumergruppen] Jede Time Series Insights-Quelle benötigt eine eigene dedizierte Consumergruppe, die nicht mit anderen Consumern gemeinsam genutzt wird. Wenn mehrere Leser Ereignisse aus der gleichen Consumergruppe nutzen, werden allen Lesern wahrscheinlich Fehler angezeigt. Beachten Sie, dass darüber hinaus ein Grenzwert von 20 Consumergruppen pro Event Hub gilt. Weitere Informationen finden Sie im [Programmierleitfaden für Event Hubs](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Wählen einer Importoption

Die Einstellungen für die Ereignisquelle können manuell eingegeben werden, oder ein Event Hub kann aus den Event Hubs ausgewählt werden, die Ihnen zur Verfügung stehen.
Wählen Sie unter **Importoption** eine der folgenden Optionen:

* Manuelles Angeben der Event Hub-Einstellungen
* Event Hub aus verfügbaren Abonnements verwenden

### <a name="select-an-available-event-hub"></a>Auswählen eines verfügbaren Event Hubs

Die folgende Tabelle enthält eine Erläuterung der einzelnen Optionen auf der Registerkarte „Neue Ereignisquelle“ sowie eine Beschreibung bei Auswahl eines verfügbaren Event Hubs als Ereignisquelle:

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Name der Ereignisquelle | Der Name der Ereignisquelle. Dieser Name muss in der Time Series Insights-Umgebung eindeutig sein.
| Quelle | Wählen Sie **Event Hub**, um eine Event Hub-Ereignisquelle zu erstellen.
| Abonnement-ID | Wählen Sie das Abonnement, in dem dieser Event Hub erstellt wurde.
| Service Bus-Namespace | Wählen Sie den Service Bus-Namespace, der den Event Hub enthält.
| Event Hub-Name | Wählen Sie den Namen des Event Hubs.
| Event Hub-Richtlinienname | Wählen Sie die SAS-Richtlinie, die auf der Registerkarte "Event Hub-Konfiguration" erstellt werden kann. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung **Lesen** verfügen.
| Event Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem Event Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.

### <a name="provide-event-hub-settings-manually"></a>Manuelles Angeben der Event Hub-Einstellungen

Die folgende Tabelle enthält eine Erläuterung der einzelnen Eigenschaften auf der Registerkarte „Neue Ereignisquelle“ sowie eine Beschreibung bei manueller Eingabe von Einstellungen:

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Name der Ereignisquelle | Der Name der Ereignisquelle. Dieser Name muss in der Time Series Insights-Umgebung eindeutig sein.
| Quelle | Wählen Sie **Event Hub**, um eine Event Hub-Ereignisquelle zu erstellen.
| Abonnement-ID | Das Abonnement, in dem dieser Event Hub erstellt wurde.
| Ressourcengruppe | Das Abonnement, in dem dieser Event Hub erstellt wurde.
| Service Bus-Namespace | Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Service Bus-Namespace erstellt.
| Event Hub-Name | Der Name des Event Hubs. Bei der Erstellung Ihres Event Hubs haben Sie ihm auch einen bestimmten Namen zugewiesen.
| Event Hub-Richtlinienname | Die Richtlinie für den gemeinsamen Zugriff, die auf der Registerkarte "Event Hub-Konfiguration" erstellt werden kann. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung **Lesen** verfügen.
| Event Hub-Richtlinienschlüssel | Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. Geben Sie hier den primären oder sekundären Schlüssel ein.
| Event Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem Event Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

1. Fügen Sie Ihrer Umgebung eine Datenzugriffsrichtlinie hinzu (siehe [Definieren von Datenzugriffsrichtlinien](time-series-insights-data-access.md)).
1. Greifen Sie im [Time Series Insights-Portal](https://insights.timeseries.azure.com) auf Ihre Umgebung zu.
