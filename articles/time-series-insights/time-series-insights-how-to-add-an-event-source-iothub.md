---
title: "Gewusst wie: Hinzufügen einer IoT Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: "In diesem Tutorial wird beschrieben, wie eine Ereignisquelle hinzugefügt wird, die mit einem IoT Hub in Ihrer Time Series Insights-Umgebung verbunden ist."
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
ms.openlocfilehash: 3b98728ced81256d05b1bed2c92fc66c5ca61b98
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="how-to-add-an-iot-hub-event-source"></a>Gewusst wie: Hinzufügen einer IoT Hub-Ereignisquelle

In diesem Tutorial wird beschrieben, wie im Azure-Portal eine Ereignisquelle hinzugefügt wird, die Daten aus einem IoT Hub in Ihre Time Series Insights-Umgebung liest.

## <a name="prerequisites"></a>Voraussetzungen

Sie haben einen IoT Hub erstellt, in den Sie Ereignisse schreiben. Weitere Informationen zu IoT Hubs finden Sie unter <https://azure.microsoft.com/services/iot-hub/>

> [Consumergruppen] Jede Time Series Insights-Quelle benötigt eine eigene dedizierte Consumergruppe, die nicht mit anderen Consumern gemeinsam genutzt wird. Wenn mehrere Leser Ereignisse aus der gleichen Consumergruppe nutzen, werden allen Lesern wahrscheinlich Fehler angezeigt. Weitere Details finden Sie im [IoT Hub-Entwicklerhandbuch](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Wählen einer Importoption

Die Einstellungen für die Ereignisquelle können manuell eingegeben werden, oder ein IoT Hub kann aus den IoT Hubs ausgewählt werden, die Ihnen zur Verfügung stehen.
Wählen Sie unter **Importoption** eine der folgenden Optionen:

* Manuelles Angeben der IoT Hub-Einstellungen
* Verwenden eines IoT Hubs aus verfügbaren Abonnements

### <a name="select-an-available-iot-hub"></a>Auswählen eines verfügbaren IoT Hubs

Die folgende Tabelle enthält eine Erläuterung der einzelnen Optionen auf der Registerkarte „Neue Ereignisquelle“ sowie eine Beschreibung bei Auswahl eines verfügbaren IoT Hubs als Ereignisquelle:

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Name der Ereignisquelle | Der Name der Ereignisquelle. Dieser Name muss in der Time Series Insights-Umgebung eindeutig sein.
| Quelle | Wählen Sie **IoT Hub**, um eine IoT Hub-Ereignisquelle zu erstellen.
| Abonnement-ID | Wählen Sie das Abonnement, in dem dieser IoT Hub erstellt wurde.
| IoT Hub-Name | Wählen Sie den Namen des IoT Hubs.
| IoT Hub-Richtlinienname | Wählen Sie die SAS-Richtlinie aus, die sich auf der Registerkarte „IoT Hub-Einstellungen“ befindet. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung zum Herstellen einer **Dienstverbindung** verfügen.
| IoT Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem IoT Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.

### <a name="provide-iot-hub-settings-manually"></a>Manuelles Angeben der IoT Hub-Einstellungen

Die folgende Tabelle enthält eine Erläuterung der einzelnen Eigenschaften auf der Registerkarte „Neue Ereignisquelle“ sowie eine Beschreibung bei manueller Eingabe von Einstellungen:

| Eigenschaftenname | Beschreibung |
| --- | --- |
| Name der Ereignisquelle | Der Name der Ereignisquelle. Dieser Name muss in der Time Series Insights-Umgebung eindeutig sein.
| Quelle | Wählen Sie **IoT Hub**, um eine IoT Hub-Ereignisquelle zu erstellen.
| Abonnement-ID | Das Abonnement, in dem dieser IoT Hub erstellt wurde.
| Ressourcengruppe | Das Abonnement, in dem dieser IoT Hub erstellt wurde.
| IoT Hub-Name | Der Name Ihres IoT Hubs. Bei der Erstellung Ihres IoT Hubs haben Sie ihm auch einen bestimmten Namen zugewiesen.
| IoT Hub-Richtlinienname | Die SAS-Richtlinie, die sich auf der Registerkarte „IoT Hub-Einstellungen“ befindet. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung zum Herstellen einer **Dienstverbindung** verfügen.
| IoT Hub-Richtlinienschlüssel | Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. Geben Sie hier den primären oder sekundären Schlüssel ein.
| IoT Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem IoT Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

1. Fügen Sie Ihrer Umgebung eine Datenzugriffsrichtlinie hinzu (siehe [Definieren von Datenzugriffsrichtlinien](time-series-insights-data-access.md)).
1. Greifen Sie im [Time Series Insights-Portal](https://insights.timeseries.azure.com) auf Ihre Umgebung zu.
