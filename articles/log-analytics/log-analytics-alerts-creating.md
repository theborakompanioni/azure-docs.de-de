---
title: Erstellen von Warnungsregeln in OMS Log Analytics | Microsoft-Dokumentation
description: "Mit Warnungen in Log Analytics werden wichtige Informationen in Ihrem OMS-Repository identifiziert, und Sie können proaktiv über Probleme informiert werden oder Aktionen aufrufen, um zu versuchen, die Probleme zu beheben.  In diesem Artikel wird beschrieben, wie Sie eine Warnungsregel erstellen, und es werden die verschiedenen Aktionen vorgestellt, die Sie durchführen können."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fdf22ff85a3a76be5de50632c4948df44c2312df
ms.openlocfilehash: 9778c79ca887e154ad2796ce5d90d953643b8067
ms.lasthandoff: 03/01/2017


---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-the-oms-portal"></a>Erstellen und Verwalten von Warnungsregeln in Log Analytics mit dem OMS-Portal
[Warnungen in Log Analytics](log-analytics-alerts.md) werden mithilfe von Warnungsregeln erstellt, für die in regelmäßigen Abständen automatisch Protokollsuchen durchgeführt werden.  Sie erstellen einen Warnungsdatensatz, wenn die Ergebnisse bestimmten Kriterien entsprechen.  Die Regel kann dann automatisch eine oder mehrere Aktionen ausführen, um Sie proaktiv über die Warnung zu informieren oder einen anderen Prozess aufzurufen.   

Dieser Artikel beschreibt die Prozesse zum Erstellen und Bearbeiten von Warnungsregeln mit dem OMS-Portal.  Weitere Informationen über die unterschiedlichen Einstellungen und das Implementieren des erforderlichen Codes finden Sie unter [Reagieren auf Probleme in Log Analytics mithilfe von Warnungen](log-analytics-alerts.md).

>[!NOTE]
> Derzeit können Sie im Azure-Portal keine Warnungsregel erstellen oder ändern. 

## <a name="create-an-alert-rule"></a>Erstellen einer Warnungsregel

Beim Erstellen einer Warnungsregel mit dem OMS-Portal beginnen Sie mit dem Erstellen einer Protokollsuche für die Datensätze, von denen die Warnung aufgerufen werden soll.  Die Schaltfläche **Warnung** ist dann verfügbar, sodass Sie die Warnungsregel erstellen und konfigurieren können.

1. Klicken Sie auf der Seite mit der OMS-Übersicht auf **Log Search**(Protokollsuche).
2. Erstellen Sie entweder eine neue Protokollsuchabfrage, oder wählen Sie eine gespeicherte Protokollsuche aus. 
3. Klicken Sie oben auf der Seite auf **Warnung**, um den Bildschirm **Warnungsregel hinzufügen** zu öffnen.
4. Konfigurieren Sie die Warnungsregel mithilfe der Informationen im Abschnitt [Details von Warnungsregeln](#details-of-alert-rules) unten.
6. Klicken Sie auf **Speichern** , um die Warnungsregel abzuschließen.  Die Ausführung beginnt sofort.


## <a name="edit-an-alert-rule"></a>Bearbeiten einer Warnungsregel
In den **Einstellungen** von Log Analytics können Sie im Menü **Warnungen** eine Liste mit allen Warnungsregeln abrufen.  

![Warnungen verwalten](./media/log-analytics-alerts/configure.png)

1. Wählen Sie in der OMS-Konsole die Kachel **Einstellungen** aus.
2. Wählen Sie **Warnungen**.

In dieser Ansicht können Sie mehrere Aktionen ausführen.

* Deaktivieren Sie eine Regel, indem Sie daneben **Aus** auswählen.
* Bearbeiten Sie eine Warnungsregel, indem Sie daneben auf das Stiftsymbol klicken.
* Entfernen Sie eine Warnungsregel, indem Sie daneben auf das Symbol **X** klicken. 

## <a name="details-of-alert-rules"></a>Details von Warnungsregeln
Wenn Sie eine Warnungsregel im OMS-Portal erstellen oder bearbeiten, arbeiten Sie mit der Seite **Warnungsregel hinzufügen** oder **Warnungsregel bearbeiten**.  Die folgenden Tabellen beschreiben die Felder in diesem Bildschirm.

![Warnungsregel](media/log-analytics-alerts/add-alert-rule.png)

### <a name="alert-information"></a>Alert information (Warnungsinformationen)
Dies sind grundlegende Einstellungen für die Warnungsregel und die Warnungen, die sie erstellt.

| Eigenschaft | Beschreibung |
|:--- |:---|
| Name | Eindeutiger Name zum Identifizieren der Warnungsregel Dieser Name wird in alle von der Regel erstellten Warnungen einbezogen.  |
| Beschreibung | Eine optionale Beschreibung der Warnungsregel. |
| Severity |Schweregrad der Warnungen, die mit dieser Regel erstellt werden. |

### <a name="search-query-and-time-window"></a>Suchabfrage- und Zeitfenster
Das Suchabfrage- und Zeitfenster, das die Datensätze zurückgibt, die ausgewertet werden, um festzustellen, ob Warnungen erstellt werden sollten.

| Eigenschaft | Beschreibung |
|:--- |:---|
| Suchabfrage | Diese Abfrage wird ausgeführt.  Mit den von dieser Abfrage zurückgegebenen Datensätzen wird ermittelt, ob eine Warnung erstellt werden muss.<br><br>Wählen Sie die Option **Use current search query** (Aktuelle Suchabfrage verwenden), um die aktuelle Abfrage zu verwenden, oder wählen Sie in der Liste eine vorhandene gespeicherte Suche aus.  Die Abfragesyntax ist im Textfeld angegeben, und Sie können sie darin bei Bedarf ändern. |
| Zeitfenster |Gibt den Zeitraum für die Abfrage an.  Die Abfrage gibt nur Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden.  Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein.  Er sollte größer als oder gleich der Warnungshäufigkeit sein.  <br><br> Wenn das Zeitfenster beispielsweise auf 60 Minuten festgelegt ist und die Abfrage um 13:15 Uhr ausgeführt wird, werden nur Datensätze zurückgegeben, die zwischen 12:15 und 13:15 Uhr erstellt wurden. |

Wenn Sie das Zeitfenster für die Warnungsregel angeben, wird die Anzahl von vorhandenen Datensätzen angezeigt, die mit den Suchkriterien für das Zeitfenster übereinstimmen.  So können Sie die Häufigkeit bestimmen, um die zu erwartende Anzahl von Ergebnissen zu erhalten.

#### <a name="threshold"></a>Schwellenwert

| Eigenschaft | Beschreibung |
|:--- |:---|
| Anzahl der Ergebnisse |Eine Warnung wird erstellt, wenn die Anzahl der von der Abfrage zurückgegebenen Datensätze entweder **größer als** oder **kleiner als** der Wert ist, den Sie angeben.  |

### <a name="alert-frequency"></a>Alert frequency (Warnhäufigkeit)
Definiert, wie oft die Suchabfrage ausgeführt wird.

| Eigenschaft | Beschreibung |
|:--- |:---|
| Alert frequency (Warnhäufigkeit) | Gibt an, wie oft die Abfrage ausgeführt werden soll. Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein. Er sollte kleiner als oder gleich dem Zeitfensterwert sein.  Wenn der Wert größer als das Zeitfenster ist, besteht das Risiko, dass Datensätze ausgelassen werden.<br><br>Angenommen, Sie verwenden ein Zeitfenster von 30 Minuten und eine Häufigkeit von 60 Minuten.  Wenn die Abfrage um 13:00 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 12:30 und 13:00 Uhr zurück.  Wenn die Abfrage dann das nächste Mal um 14 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 13:30 und 14:00 Uhr zurück.  Alle Datensätze, die zwischen 13:00 und 13:30 erstellt werden, werden also nicht ausgewertet. |
| Suppress alerts (Warnungen unterdrücken) | Wenn Sie die Unterdrückung für die Warnungsregel aktivieren, werden Aktionen für die Regel nach dem Erstellen einer neuen Warnung für einen vorher festgelegten Zeitraum deaktiviert. Die Regel wird weiter ausgeführt und erstellt Warnungsdatensätze, wenn die Kriterien erfüllt sind. Dies ist der Fall, damit Sie Zeit haben, das Problem zu beheben, ohne doppelte Aktionen durchzuführen. |


### <a name="actions"></a>Actions
Warnungsregeln erstellen immer einen [Warnungsdatensatz](#alert-records), wenn der Schwellenwert erreicht wird.  Sie können auch eine oder mehrere auszuführende Aktionen definieren, z.B. Senden einer E-Mail oder Starten eines Runbooks.  Unter [Add actions to alert rules in Log Analytics](log-analytics-alerts-actions.md) (Hinzufügen von Aktionen zu Warnungsregeln in Log Analytics) finden Sie weitere Informationen zum Konfigurieren von Aktionen. 


#### <a name="email-actions"></a>E-Mail-Aktionen
Bei E-Mail-Aktionen wird eine E-Mail mit den Details der Warnung an einen oder mehrere Empfänger gesendet.

| Eigenschaft | Beschreibung |
|:--- |:---|
| E-Mail-Benachrichtigung |Geben Sie **Ja** an, falls eine E-Mail gesendet werden soll, wenn die Warnung ausgelöst wird. |
| Betreff |Der Betreff der E-Mail.  Sie können den Haupttext der E-Mail nicht ändern. |
| Empfänger |Adressen aller E-Mail-Empfänger.  Verwenden Sie als Trennzeichen ein Semikolon (;), wenn Sie mehrere Adressen angeben. |

#### <a name="webhook-actions"></a>Webhookaktionen
Mit Webhookaktionen können Sie einen externen Prozess über eine HTTP POST-Anforderung aufrufen.

| Eigenschaft | Beschreibung |
|:--- |:---|
| Webhook |Geben Sie **Ja** an, wenn beim Auslösen der Warnung ein Webhook aufgerufen werden soll. |
| Webhook-URL |Die URL des Webhooks. |
| Include custom JSON payload (Benutzerdefinierte JSON-Nutzlast einbinden) |Wählen Sie diese Option, wenn Sie die Standardnutzlast durch eine benutzerdefinierte Nutzlast ersetzen möchten. |
| Enter your custom JSON payload (Benutzerdefinierte JSON-Nutzlast eingeben) |Benutzerdefinierte Nutzlast, die an den Webhook gesendet wird.  |

#### <a name="runbook-actions"></a>Runbookaktionen
Bei Runbookaktionen wird ein Runbook in Azure Automation gestartet. 

>[!NOTE]
> Damit diese Aktion aktiviert werden kann, muss die Automation-Lösung in Ihrem Arbeitsbereich installiert sein. 


| Eigenschaft | Beschreibung |
|:--- |:---|
| Runbook | Geben Sie **Ja** an, falls ein Azure Automation-Runbook gestartet werden soll, wenn die Warnung ausgelöst wird.  |
| Automation-Konto | Gibt das Automation-Konto an, in dem Runbooks ausgewählt werden.  Dies ist das Aktionskonto, das mit dem Arbeitsbereich verknüpft ist. |
| Runbook auswählen | Wählen Sie das Runbook aus, das Sie starten möchten, wenn eine Warnung erstellt wird. |
| Run on (Ausführen auf) | Wählen Sie **Azure**, um das Runbook in der Cloud auszuführen.  Wählen Sie **Hybrid Worker**, um das Runbook auf einem Agent auszuführen, auf dem [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installiert ist.  |




## <a name="next-steps"></a>Nächste Schritte
* Installieren Sie die [Alert Management-Lösung](log-analytics-solution-alert-management.md) , um die in Log Analytics erstellten Warnungen und die über System Center Operations Manager (SCOM) gesammelten Warnungen zu analysieren.
* Informieren Sie sich weiter über [Protokollsuchen](log-analytics-log-searches.md) , bei denen Warnungen generiert werden können.
* Arbeiten Sie eine exemplarische Vorgehensweise für das [Konfigurieren eines Webooks](log-analytics-alerts-webhooks.md) mit einer Warnungsregel durch.  
* Informieren Sie sich darüber, wie Sie [Runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) schreiben, um von Warnungen identifizierte Probleme zu beheben.


