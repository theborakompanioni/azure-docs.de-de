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
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 9dc394c0da34a8ee2796c6114e7f2f647c5345a1
ms.contentlocale: de-de
ms.lasthandoff: 04/26/2017

---

# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-azure-portal"></a>Erstellen einer Ereignisquelle für Ihre Time Series Insights-Umgebung über das Azure-Portal

Die Time Series Insights-Ereignisquelle leitet sich von einem Ereignisbroker (beispielsweise Azure Event Hubs) ab. Time Series Insights stellt eine direkte Verbindung mit Ereignisquellen her, und der Datenstrom wird erfasst, ohne dass Benutzer eine einzige Codezeile schreiben müssen. Für Time Series Insights werden derzeit Azure Event Hubs und Azure IoT Hubs unterstützt. Weitere Ereignisquellen werden später hinzugefügt.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Schritte zum Hinzufügen einer Ereignisquelle zu Ihrer Umgebung

1.    Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2.    Klicken Sie im Azure-Portal im Menü auf der linken Seite auf „Alle Ressourcen“.
3.    Wählen Sie Ihre Time Series Insights-Umgebung aus.

  ![Erstellen der Time Series Insights-Ereignisquelle](media/add-event-source/getstarted-create-eventsource1.png)

4.    Wählen Sie „Ereignisquellen“ aus, und klicken Sie anschließend auf „Hinzufügen“.

  ![Erstellen der Time Series Insights-Ereignisquelle – Details](media/add-event-source/getstarted-create-eventsource2.png)

5.    Geben Sie den Namen der Ereignisquelle an. Dieser Name wird allen Ereignissen aus dieser Ereignisquelle zugeordnet und steht bei Abfragen zur Verfügung.
6.    Wählen Sie in der Liste mit Event Hub-Ressourcen im aktuellen Abonnement einen Event Hub aus, oder verwenden Sie die Importoption „Event Hub-Einstellungen manuell angeben“, um einen Event Hub in einem anderen Abonnement anzugeben. Ereignisse müssen im JSON-Format veröffentlicht werden.
7.    Wählen Sie eine Richtlinie mit Leseberechtigung im Event Hub aus.
8.    Geben Sie eine Event Hub-Consumergruppe an.

  > [!IMPORTANT]
  > Achten Sie darauf, dass diese Consumergruppe nicht von einem anderen Dienst (beispielsweise durch einen Stream Analytics-Auftrag oder durch eine andere Time Series Insights-Umgebung) verwendet wird. Wenn die Consumergruppe von anderen Diensten verwendet wird, wirkt sich das negativ auf Lesevorgänge für diese Umgebung und andere Dienste aus. Wenn Sie „$Default“ als Consumergruppe verwenden, wird sie unter Umständen von anderen Lesern wiederverwendet.

9.    Klicken Sie auf „Erstellen“.

Nach der Erstellung der Ereignisquelle beginnt Time Series Insights automatisch damit, Daten in Ihre Umgebung zu streamen.

## <a name="next-steps"></a>Nächste Schritte

* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Portal](https://insights.timeseries.azure.com)

