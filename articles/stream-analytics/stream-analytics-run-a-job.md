---
title: "Starten von Streamingaufträgen in Stream Analytics | Microsoft Docs"
description: "Ausführen eines Streamingauftrags in Azure Stream Analytics | Lernpfadsegment."
keywords: "Streamingaufträge"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 03d43476c1946221538acccd1251c64ed3f02e13
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Ausführen von Streamingaufträgen in Azure Stream Analytics
Nachdem Eingabe, Abfrage und Ausgabe für den Auftrag angegeben wurden, können Sie den Stream Analytics-Auftrag starten.

So starten Sie den Auftrag:

1. Klicken Sie im klassischen Azure-Portal im Dashboard des Auftrags auf **Starten** am unteren Rand der Seite.
   
   ![Schaltfläche „Auftrag starten“](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   Klicken Sie im Azure-Portal am oberen Rand der Seite mit den Aufträgen auf **Starten** .
   
   ![Azure-Portal – Schaltfläche „Auftrag starten“](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Geben Sie einen Wert für den **Ausgabestart** ein, um zu bestimmen, wann der Auftrag mit der Ausgabe beginnen soll. Die Standardeinstellung für Aufträge, die zuvor noch nicht gestartet wurden, ist die **Startzeit des Auftrags**, was bedeutet, dass der Auftrag sofort mit der Verarbeitung der Daten beginnt. Sie können auch eine **benutzerdefinierte** Zeit in der Vergangenheit (für die Verarbeitung von Verlaufsdaten) oder in der Zukunft (damit die Verarbeitung auf einen späteren Zeitpunkt verschoben wird) angeben. Für Fälle, in denen ein Auftrag bereits zuvor gestartet und beendet wurde, steht die Option **Last Stopped Time** zur Verfügung, um den Auftrag ab dem letzten Zeitpunkt der Ausgabe fortzusetzen und Datenverlust zu vermeiden.  
   
   ![Starten von Streamingaufträgen, Zeit](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure-Portal, Starten von Streamingaufträgen, Zeit](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Bestätigen Sie Ihre Auswahl. Der Status ändert sich in *Starten* und wechselt nach kurzer Zeit zu *Ausführen*, sobald der Auftrag gestartet wurde. Sie können den Fortschritt des **Startvorgangs** im **Notification Hub** überwachen:
   
   ![Streamingauftrag, Fortschritt](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Azure-Portal, Streamingauftrag, Fortschritt](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


