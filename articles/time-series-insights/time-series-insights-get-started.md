---
title: Erstellen einer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: "In diesem Tutorial erfahren Sie, wie Sie eine Time Series-Umgebung erstellen, eine Verbindung mit einer Ereignisquelle herstellen und innerhalb weniger Minuten für die Analyse Ihrer Ereignisdaten bereit sind."
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
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.contentlocale: de-de
ms.lasthandoff: 04/26/2017

---

# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Erstellen einer neuen Time Series Insights-Umgebung über das Azure-Portal

Eine Time Series Insights-Umgebung ist eine Azure-Ressource mit Erfassungs- und Speicherfunktionen. Kunden stellen Umgebungen über das Azure-Portal mit der erforderlichen Kapazität bereit.

## <a name="steps-to-create-the-environment"></a>Schritte zum Erstellen der Umgebung

Führen Sie zum Erstellen der Umgebung die folgenden Schritte aus:

1.    Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2.    Klicken Sie in der linken oberen Ecke auf das Pluszeichen („+“).
3.    Suchen Sie mithilfe des Suchfelds nach „Time Series Insights“.

  ![Erstellen der Time Series Insights-Umgebung](media/get-started/getstarted-create-environment1.png)

4.    Wählen Sie „Time Series Insights“ aus, und klicken Sie anschließend auf „Erstellen“.

  ![Erstellen der Time Series Insights-Ressourcengruppe](media/get-started/getstarted-create-environment2.png)

5.    Geben Sie den Namen der Umgebung an. Unter diesem Namen wird die Umgebung im [Time Series-Explorer](https://insights.timeseries.azure.com) dargestellt.
6.    Wählen Sie ein Abonnement aus. Verwenden Sie ein Abonnement, das Ihre Ereignisquelle enthält. Time Series Insights kann Azure IoT Hub- und Event Hub-Ressourcen im gleichen Abonnement automatisch erkennen.
7.    Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie sie. Eine Ressourcengruppe ist eine Sammlung von Azure-Ressourcen, die zusammen verwendet werden.
8.    Wählen Sie einen Hostingstandort aus. Verwenden Sie einen Standort, der Ihre Ereignisquelle enthält, um die Verschiebung von Daten zwischen Rechenzentren zu vermeiden.
9.    Wählen Sie einen Tarif.
10.    Wählen Sie eine Kapazität aus. Die Kapazität einer Umgebung kann nach der Erstellung geändert werden.
11.    Erstellen Sie Ihre Umgebung. Sie können Ihre Umgebung auch an das Dashboard anheften, um nach der Anmeldung komfortabel darauf zugreifen zu können.

  ![Erstellen der Time Series Insights-Anheftung an das Dashboard](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Nächste Schritte

* [Definieren von Datenzugriffsrichtlinien](time-series-insights-data-access.md) für den Zugriff auf Ihre Umgebung im [Time Series-Insights-Portal](https://insights.timeseries.azure.com)
* [Erstellen einer Ereignisquelle](time-series-insights-add-event-source.md)
* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle

