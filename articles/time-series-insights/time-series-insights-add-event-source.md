---
title: "Hinzufügen einer Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: In diesem Tutorial verbinden Sie eine Ereignisquelle mit Ihrer Time Series Insights-Umgebung.
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: ffa2eaf3680e68ac14aabf49b6308caeb173fd43
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---

# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-ibiza-portal"></a>Erstellen einer Ereignisquelle für Ihre Time Series Insights-Umgebung über das Ibiza-Portal

Die Time Series Insights-Ereignisquelle leitet sich von einem Ereignisbroker (beispielsweise Azure Event Hubs) ab. Time Series Insights stellt eine direkte Verbindung mit Ereignisquellen her, und der Datenstrom wird erfasst, ohne dass Benutzer eine einzige Codezeile schreiben müssen. Zurzeit unterstützt Time Series Insights Azure Event Hubs und Azure IoT Hubs. In Zukunft werden weitere Ereignisquellen hinzugefügt.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Schritte zum Hinzufügen einer Ereignisquelle zu Ihrer Umgebung

1.  Melden Sie sich beim [Ibiza-Portal](https://portal.azure.com) an.
2.  Klicken Sie im Ibiza-Portal im Menü auf der linken Seite auf „Alle Ressourcen“.
3.  Wählen Sie Ihre Time Series Insights-Umgebung aus.

  ![Erstellen der Time Series Insights-Ereignisquelle](media/add-event-source/getstarted-create-event-source-1.png)

4.  Wählen Sie „Ereignisquellen“ aus, und klicken Sie anschließend auf „Hinzufügen“.

  ![Erstellen der Time Series Insights-Ereignisquelle – Details](media/add-event-source/getstarted-create-event-source-2.png)

5.  Geben Sie den Namen der Ereignisquelle an. Dieser Name wird allen Ereignissen aus dieser Ereignisquelle zugeordnet und steht bei Abfragen zur Verfügung.
6.  Wählen Sie im aktuellen Abonnement in der Liste der Event Hub-Ressourcen einen Event Hub aus. Wählen Sie andernfalls die Importoption „Event Hub-Einstellungen manuell angeben“, um einen Event Hub in einem anderen Abonnement anzugeben. Ereignisse müssen im JSON-Format veröffentlicht werden.
7.  Wählen Sie eine Richtlinie mit Leseberechtigung im Event Hub aus.
8.  Geben Sie eine Event Hub-Consumergruppe an.

  > [!IMPORTANT]
  > Achten Sie darauf, dass diese Consumergruppe nicht von einem anderen Dienst (beispielsweise durch einen Stream Analytics-Auftrag oder durch eine andere Time Series Insights-Umgebung) verwendet wird. Wenn die Consumergruppe von anderen Diensten verwendet wird, wirkt sich das negativ auf Lesevorgänge für diese Umgebung und andere Dienste aus. Wenn Sie „$Default“ als Consumergruppe verwenden, wird sie unter Umständen von anderen Lesern wiederverwendet.

9.  Klicken Sie auf „Erstellen“.

Nach der Erstellung der Ereignisquelle beginnt Time Series Insights automatisch damit, Daten in Ihre Umgebung zu streamen.

## <a name="next-steps"></a>Nächste Schritte

* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Portal](https://insights.timeseries.azure.com)

