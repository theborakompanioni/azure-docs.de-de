---
title: "Nachverfolgen der Integrität von Azure-Diensten mit Azure Monitor-Aktivitätsprotokollen | Microsoft Docs"
description: "Ermitteln Sie den Zeitpunkt von Leistungsabfällen oder Dienstunterbrechungen, die bei Azure aufgetreten sind. "
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 30db9258-98e6-4855-abcd-f06c06277aa8
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d31ee12c783b845e0a8ff1791d21b2ef9013eee3


---
# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Nachverfolgen der Integrität von Azure-Diensten mit Azure Monitor-Aktivitätsprotokollen
Azure informiert jeweils aktuell über Dienstunterbrechungen oder Leistungsabfälle. Sie können solche Ereignisse im Azure-Portal durchsuchen, und Sie können darüber hinaus mithilfe von [REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programmgesteuert auf sämtliche Ereignisse zugreifen.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Durchsuchen der Protokolle zur Dienstintegrität für Ihr Abonnement
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Auf der **Startseite** sehen Sie eine Kachel namens **Dienstintegrität**. Klicken Sie darauf.
   
    ![Privat](./media/insights-service-health/Insights_Home.png)
3. Sie sehen eine Liste aller Regionen in Azure. Klicken Sie auf eine beliebige Region, um die Abfrage von Aktivitätsprotokollen aufzurufen, mit der Sie Dienstereignisse mit Auswirkungen auf Ihre Abonnements in den letzten 24 Stunden anzeigen können.
   
    ![Aktivitätsprotokoll zur Abonnementdienstintegrität](./media/insights-service-health/AzureActivityLogServiceHealth3.png)
4. Außerdem können Sie die Details für einen einzelnen Incident anzeigen, indem Sie in der Tabelle auf das Ereignis klicken.
5. Ändern Sie den **Zeitraum**, um einen größeren Zeitabschnitt anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen der Verfügbarkeit und Reaktionsfähigkeit einer beliebigen Webseite](../application-insights/app-insights-monitor-web-app-availability.md) mit Application Insights, um zu ermitteln, ob eine Seite offline ist.




<!--HONumber=Nov16_HO3-->


