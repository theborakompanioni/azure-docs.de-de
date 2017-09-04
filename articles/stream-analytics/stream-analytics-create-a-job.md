---
title: "Erstellen eines Auftrags zur Verarbeitung von Datenanalysen für Stream Analytics | Microsoft Docs"
description: "Erstellen eines Auftrags zur Verarbeitung von Datenanalysen für Stream Analytics | Lernpfadsegment."
keywords: Verarbeitung von Datenanalysen
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Erstellen eines Auftrags zur Verarbeitung von Datenanalysen für Stream Analytics
Die Ressource der obersten Ebene in Azure Stream Analytics ist ein Stream Analytics-Auftrag.  Er besteht aus einer oder mehreren Eingabedatenquellen, einer Abfrage zum Ausdrücken der Datentransformation und mindestens einem Ausgabeziel, in das Ergebnisse geschrieben werden. Dadurch kann der Benutzer die Verarbeitung von Datenanalysen für Streamingdatenszenarien durchführen.

Erstellen Sie zuerst einen neuen Stream Analytics-Auftrag, um mit der Verwendung von Stream Analytics zu beginnen.  Beachten Sie, dass diese Aktion keine Auswirkungen auf die Abrechnung hat, bis der Auftrag gestartet wird.

1. Melden Sie sich online beim [klassischen Azure-Portal](http://manage.windowsazure.com) oder dem [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Portal auf **Neu**, dann je nach Portal auf **Datendienste** oder **Datenanalysen**, klicken Sie anschließend auf **Azure Stream Analytics** oder **Stream Analytics** und dann auf **Schnellerfassung**.
   
   ![Assistent für Aufträge zur Verarbeitung von Datenanalysen](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Erstellen eines Auftrags zur Verarbeitung von Datenanalysen](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Geben Sie die gewünschte Konfiguration für den Stream Analytics-Auftrag an.
   
   * Geben Sie im Feld **Auftragsname** einen Namen zur Identifizierung des Stream Analytics-Auftrags ein. Wenn der **Auftragsname** überprüft wurde, wird im Auftragsnamenfeld ein grünes Häkchen angezeigt. Der **Auftragsname** darf nur alphanumerische Zeichen und den Bindestrich „-“ enthalten und muss zwischen 3 und 63 Zeichen lang sein.
   * Verwenden Sie die **Region** oder den **Standort** im Azure-Portal, um den geografischen Standort anzugeben, an dem der Auftrag ausgeführt werden soll.
   * Bei Verwendung des Azure-Portals wählen oder erstellen Sie ein Speicherkonto für die Verwendung als das **regionale Überwachungskonto für Speicher**. Dieses Speicherkonto wird zum Speichern von Überwachungsdaten für alle Stream Analytics-Aufträge verwendet, die in dieser Region ausgeführt werden.
   * Bei Verwendung des Azure-Portals geben Sie eine neue oder vorhandene **Ressourcengruppe** an, um verwandte Ressourcen für Ihre Anwendung zu speichern.
4. Sobald die Optionen für den neuen Stream Analytics-Auftrag konfiguriert sind, klicken Sie auf **Stream Analytics-Auftrag erstellen**. Die Erstellung des Stream Analytics-Auftrags kann einige Minuten dauern. Sie können den Fortschritt im Benachrichtigungshub überwachen, um den Status zu überprüfen.
   
   ![Benachrichtigungshub für Aufträge zur Verarbeitung von Datenanalysen](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure-Portal – Auftrag zur Verarbeitung von Datenanalysen – Auftrag erstellen](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Für den neuen Auftrag wird der Status **Erstellt** angezeigt. Beachten Sie, dass die Schaltfläche **Start** deaktiviert ist. Vor dem Starten des Auftrags müssen Eingabe, Abfrage und Ausgabe des Auftrags konfiguriert werden.
   
   ![Auftrag zur Verarbeitung von Datenanalysen – Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure-Portal – Auftrag zur Verarbeitung von Datenanalysen – Status](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


