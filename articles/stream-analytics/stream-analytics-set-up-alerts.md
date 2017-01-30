---
title: "Einrichten von Warnungen für Abfragen in Stream Analytics | Microsoft Docs"
description: Grundlegendes zu Stream Analytics-Warnungen
keywords: Einrichten von Warnungen
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/24/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c8463e68600edefecd0f3bdd19c13b13f676b82b


---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Einrichten von Warnungen für Azure Stream Analytics-Aufträge
## <a name="introduction-monitor-page"></a>Einführung: Seite „Überwachen“
Sie können Warnungen einrichten, die eine Warnung auslösen, wenn eine Metrik eine von Ihnen angegebene Bedingung erreicht.

Beispiel: „Wenn Ausgabeereignisse in den letzten 15 Minuten < 100, E-Mail-Benachrichtigung an diese E-Mail-ID senden: xyz@company.com“.

Regeln können für Metriken über das Portal eingerichtet oder [programmgesteuert](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) über Vorgangsprotokolldaten konfiguriert werden.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Einrichten von Warnungen über das klassische Azure-Portal
Es gibt zwei Möglichkeiten, Warnungen im klassischen Azure-Portal einzurichten:  

1. Die Registerkarte **Überwachen** in Ihrem Stream Analytics-Auftrag  
2. Das Vorgangsprotokoll in den Verwaltungsdiensten  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Einrichten von Warnungen über die Registerkarte „Überwachen“ des Auftrags im Portal
1. Wählen Sie die Metrik auf der Registerkarte „Überwachen“ aus, klicken Sie auf die Schaltfläche **Regel hinzufügen** im unteren Teil des Dashboards, und richten Sie dann die Regeln ein.  
   
   ![Dashboard](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  
2. Definieren Sie den Namen und die Beschreibung der Warnung  
   
   ![Erstellen einer Regel](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  
3. Geben Sie die Schwellenwerte, das Warnungsauswertungsfenster und die Aktionen für die Warnung ein  
   
   ![Definieren von Bedingungen](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Einrichten von Warnungen über die Vorgangsprotokolle
1. Wechseln Sie zur Registerkarte **Warnungen** in den Verwaltungsdiensten im [klassischen Azure-Portal](https://manage.windowsazure.com).  
2. Klicken Sie auf **Regel hinzufügen**  
   
   ![Kriterien](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  
3. Definieren Sie den Namen und die Beschreibung der Warnung Wählen Sie 'Stream Analytics' als Diensttyp und den Namen des Auftrags als Dienstnamen.  
   
   ![Definieren einer Warnung](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Einrichten von Warnungen im Azure-Portal
Navigieren Sie im Azure-Portal zum Stream Analytics-Auftrag, für dessen Metriken Sie Warnungen einrichten möchten, und klicken Sie auf den Abschnitt **Überwachung** .  Klicken Sie im dann geöffneten Blatt **Metrik** auf den Befehl **Benachrichtigung hinzufügen**.

  ![Azure-Portal, „Einrichtung“](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Sie können einen Namen für die Warnungsregel eingeben und eine Beschreibung auswählen, die in der Benachrichtigungs-E-Mail angezeigt wird.

Bei Auswahl von „Metrics“ geben Sie eine Bedingung und einen Schwellenwert für die Metrik an.

  ![Azure-Portal, „Metrik auswählen“](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Weitere Informationen zum Konfigurieren von Warnungen im Azure-Portal finden Sie unter [Empfangen von Warnbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Nov16_HO3-->


