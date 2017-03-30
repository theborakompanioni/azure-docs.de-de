---
title: "Konfigurieren von Datenausgaben für Stream Analytics-Aufträge | Microsoft Docs"
description: "Konfigurieren von Ausgaben für Stream Analytics-Aufträge | Lernpfadsegment."
keywords: Datenausgabe, Datenverschiebung
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aebad3dc5b6fda757713b53edfe9a6d474b7f4cb
ms.lasthandoff: 11/17/2016


---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Konfigurieren von Datenausgaben für Stream Analytics-Aufträge
Azure Stream Analytics-Aufträge können mit einer oder mehreren Datenausgaben verknüpft werden, die eine Verbindung mit einer vorhandenen Datensenke definieren. Während Ihr Stream Analytics-Auftrag eingehende Daten verarbeitet und umwandelt, wird ein Datenstrom von Datenausgabeereignissen in die Ausgabe Ihres Auftrags geschrieben.

Stream Analytics-Datenausgaben können als Quelle für Echtzeitdashboards oder Warnungen, zum Auslösen von Workflows für Datenverschiebungen oder einfach zum Archivieren von Daten für die spätere Batchverarbeitung verwendet werden. Stream Analytics bietet eine erstklassige Integration in verschiedene Azure-Diensten, die hier ausführlich dokumentiert sind.

So fügen Sie Ihrem Stream Analytics-Auftrag eine Ausgabe hinzu:

1. Klicken Sie im klassischen Azure-Portal auf **Ausgaben** und dann in Ihrem Stream Analytics-Auftrag auf **Ausgabe hinzufügen**.
   
    ![Ausgaben hinzufügen](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
    Klicken Sie im Azure-Portal in Ihrem Stream Analytics-Auftrag auf die Kachel **Ausgaben** .
   
    ![Azure-Portal – Ausgaben hinzufügen](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)
2. Geben Sie den Typ der Ausgabe an:
   
    ![Auswählen eines Datenverschiebungstyps](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
    ![Azure-Portal – Datenverschiebungstyp auswählen](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)
3. Geben Sie im Feld **Ausgabealias** einen Anzeigenamen für diese Ausgabe ein. Dieser Name kann später in der Abfrage Ihres Auftrags zum Verweisen auf die Ausgabe verwendet werden.  
   
    Geben Sie den Rest der erforderlichen Verbindungseigenschaften ein, um eine Verbindung mit Ihrer Ausgabe herzustellen.  Diese Felder unterscheiden sich je nach Ausgabetyp und sind hier im Detail definiert.  
   
    ![Hinzufügen von Eigenschaften für die Datenausgabe](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  
4. Je nach Ausgabetyp müssen Sie möglicherweise angeben, wie die Daten serialisieren oder formatiert werden. Die spezifischen Serialisierungseinstellungen für die einzelnen Ausgabetypen sind hier dokumentiert.
   
    Geben Sie den Rest der erforderlichen Verbindungseigenschaften ein, um eine Verbindung mit Ihrer Datenquelle herzustellen. Diese Felder unterscheiden sich je nach Eingabe- und Quelltyp und sind [hier](stream-analytics-create-a-job.md)im Detail definiert.  
   
    ![Hinzufügen der Datenausgabe zum Event Hub](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  
   
    ![Azure-Portal – Datenausgabe für den Event Hub](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [!Note]
> Jedes Ausgabeelement, das dem Auftrag hinzugefügt wird, muss vorhanden sein, bevor der Auftrag gestartet wird und Ereignisse übertragen werden. Wird beispielsweise ein Blob Storage als Ausgabe verwendet, erstellt der Auftrag nicht automatisch ein Speicherkonto. Es muss vom Benutzer erstellt werden, bevor der ASA-Auftrag gestartet wird.
> 
> 

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


