---
title: Debuggen mithilfe von Vorgangs- und Dienstprotokollen in Stream Analytics | Microsoft Docs
description: "Verwenden von Stream Analytics-Vorgangsprotokollen"
keywords: Dienstprotokolle
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: e694eb8f4d658ff9410ae7be2bd4b69500ecbf20
ms.lasthandoff: 11/17/2016


---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Debuggen von Stream Analytics-Aufträgen mithilfe von Dienst- und Vorgangsprotokollen
Alle Azure-Dienste stellen Benutzern operative Protokollierungsnachrichten bereit, um Details im Zusammenhang mit Verwaltungsvorgängen zu erfassen. In Azure Stream Analytics können diese Informationen für Debugzwecke wie z. B. das Anzeigen des Auftragsstatus, Auftragsfortschritts und von Fehlermeldungen verwendet werden, um den Fortschritt eines Auftrags im Laufe der Zeit von Beginn an, während der Verarbeitung und bis zur Ausgabe nachzuverfolgen.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Suchen von Vorgangsprotokollen im Azure-Verwaltungsportal
Für den Zugriff auf Vorgangsprotokolle gibt es zwei Möglichkeiten:  

* Dashboard des Stream Analytics-Auftrags  
* Verwaltungsdienste im klassischen Azure-Portal  

## <a name="dashboard-of-the-stream-analytics-job"></a>Dashboard des Stream Analytics-Auftrags
Auf der Dashboard-Registerkarte eines Stream Analytics-Auftrags wird eine Verknüpfung mit den zugehörigen Protokollen angezeigt. Wenn Sie auf diese Verknüpfung klicken, werden die Filter so festgelegt, dass aktuelle Protokolle für diesen Auftrag angezeigt werden.

  ![Verwaltungsdienstprotokolle auswählen](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Verwaltungsdienste
So navigieren Sie manuell zu den Vorgangsprotokollen für Stream Analytics und andere Dienste im klassischen Azure-Portal:

1. Klicken Sie im **klassischen Azure-Portal** auf [Verwaltungsdienste](https://manage.windowsazure.com).
2. Wählen Sie **Stream Analytics** als **Typ** und den Namen des Auftrags als **Dienstname** aus.  
   
   ![Stream Analytics auswählen](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Suchen von Überwachungsprotokolle im Azure-Portal
Zum Anzeigen der Betriebsprotokolle für den Stream Analytics-Auftrag klicken Sie im Azure-Portal auf **Durchsuchen**, und wählen Sie dann **Überwachungsprotokolle** aus.

  ![Azure-Portal, „Stream Analytics auswählen“](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Dadurch wird ein Blatt mit den Ereignissen der letzten 7 Tage für alle Ressourcen in Ihrem Abonnement geöffnet.  Sie können Ereignisse eines bestimmten Typs oder Zeitrahmens anzeigen, indem Sie auf den Befehl **Filter** klicken.

  ![Azure-Portal, „Stream Analytics auswählen“](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Abrufen von Protokolldetails
Sie können nach Zeitraum und Status filtern, um die Protokolle für Ihren Auftrag anzuzeigen.

Klicken Sie im Azure-Verwaltungsportal unten im Fenster auf die Schaltfläche **Details** , um weitere Details zu einem ausgewählten Ereignis anzuzeigen. 

  ![Details auswählen](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Klicken Sie im Azure-Portal auf einen Protokolleintrag, um ausführliche Informationen zu den enthaltenen Ereignissen anzuzeigen.

  ![Azure-Portal, „Details auswählen“](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Sie können von dort das Blatt **Detail** öffnen, indem Sie auf das Ereignis klicken.

  ![Azure-Portal, „Details auswählen“](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Debuggen eines fehlgeschlagenen Auftrags
Klicken Sie im Azure-Verwaltungsportal auf das Symbol "Suche", und geben Sie "failed" ein. Dadurch wird ein Ergebnis aller Protokolle mit Fehlern zurück. 

  ![Debuggen eines fehlerhaften Auftrags](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Im Azure-Portal können Sie nach Meldungsebene filtern, um **kritische** Ereignisse anzuzeigen.

  ![Azure-Portal, „Debuggen“](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Wählen Sie einen der Fehler aus, und klicken Sie auf **Details** , um weitere Informationen zu diesem Fehler zu erhalten.  Einige Fehlermeldungen enthalten auch Informationen zur Behebung des Problems. 

  ![Vorgangsdetails](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Wenn Sie den [Support](https://azure.microsoft.com/support/options/) kontaktieren oder dem Team über das [MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) Informationen bereitstellen möchten, beachten Sie die Vorgangsdetails, insbesondere die **Korrelations-ID**. 

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


