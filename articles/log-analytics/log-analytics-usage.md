---
title: Analysieren der Datennutzung in Log Analytics | Microsoft Docs
description: "Im Dashboard „Nutzung“ in Log Analytics können Sie anzeigen, wie viele Daten an den OMS-Dienst gesendet werden."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 668cde23cb717fcad52fd7823561d10735e6df1b


---
# <a name="analyze-data-usage-in-log-analytics"></a>Analysieren der Datennutzung in Log Analytics
Log Analytics erfasst Daten und sendet sie in regelmäßigen Abständen an den OMS-Dienst.  Im Log Analytics-Dashboard zur **Nutzung** können Sie anzeigen, wie viele Daten an den OMS-Dienst gesendet werden. In diesem Dashboard wird auch angezeigt, wie viele Daten von Lösungen gesendet werden und wie häufig Ihre Server Daten senden.

> [!NOTE]
> Wenn Sie ein kostenloses Konto verwenden, ist die Datenmenge, die täglich an den OMS-Dienst gesendet wird, auf 500 MB begrenzt. Ist die Tagesgrenze erreicht, wird die Datenanalyse beendet und zu Beginn des nächsten Tags fortgesetzt. In diesem Fall müssen Sie alle Daten erneut senden, die von OMS nicht akzeptiert oder verarbeitet wurden.

Wenn Sie die tägliche Nutzungsgrenze überschritten haben oder sich diesem Grenzwert nähern, können Sie optional eine Lösung entfernen, um die an den OMS-Dienst gesendete Datenmenge zu reduzieren. Weitere Informationen zum Entfernen von Lösungen finden Sie unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md).

![Dashboard „Nutzung“](./media/log-analytics-usage/usage-dashboard01.png)

Im Log Analytics-Dashboard zur **Nutzung** werden die folgenden Informationen angezeigt:

- Datenvolume
    - Datenvolume im Zeitverlauf (basierend auf dem aktuellen Zeitbereich)
    - Datenvolumen nach Lösung
    - Daten, die keinem Computer zugeordnet sind
- Computer
    - Computer, die Daten senden
    - Computer ohne Daten in den letzten 24 Stunden
- Angebote
    - Knoten vom Typ „Insight & Analytics“
    - Knoten vom Typ „Automation & Control“
    - Knoten vom Typ „Security“
- Leistung
    - Zeit zum Erfassen und Indizieren von Daten
- Liste der Abfragen

## <a name="to-work-with-usage-data"></a>So arbeiten Sie mit Nutzungsdaten
1. Melden Sie sich mit Ihrem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com) an, sofern Sie noch nicht angemeldet sind.
2. Klicken Sie im Menü **Hub** auf **Weitere Dienste**, und geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Log Analytics**.  
    ![Azure-Hub](./media/log-analytics-usage/hub.png)
3. Im Dashboard **Log Analytics** wird eine Liste mit Ihren Arbeitsbereichen angezeigt. Wählen Sie einen Arbeitsbereich aus.
4. Klicken Sie im Dashboard *Arbeitsbereich* auf **Log Analytics Usage** (Log Analytics-Nutzung).
5. Klicken Sie im Log Analytics-Dashboard zur **Nutzung** auf **Zeit: Letzte 24 Stunden**, um das Zeitintervall zu ändern.  
    ![Zeitintervall](./media/log-analytics-usage/time.png)
6. Zeigen Sie die Blätter mit der Nutzungskategorie an, auf denen die für Sie interessanten Bereiche enthalten sind. Wählen Sie ein Blatt aus, und klicken Sie darin dann auf ein Element, um für die [Protokollsuche](log-analytics-log-searches.md) mehr Details anzuzeigen.  
    ![Blatt mit Beispieldaten zur Nutzung](./media/log-analytics-usage/blade.png)
7. Überprüfen Sie im Dashboard „Protokollsuche“ die Ergebnisse, die für die Suche ausgegeben werden.  
    ![Beispiel zur Nutzung für die Protokollsuche](./media/log-analytics-usage/usage-log-search.png)


## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Informationen, die von Features und Lösungen gesammelt und an OMS gesendet werden, finden Sie unter [Protokollsuchen in Log Analytics](log-analytics-log-searches.md).



<!--HONumber=Dec16_HO2-->


