<properties
	pageTitle="Überwachen von Anforderungen, Nutzung und Speicher in DocumentDB | Microsoft Azure"
	description="Erfahren Sie, wie Sie Ihr DocumentDB-Konto anhand von Leistungsmetriken (z. B. Anforderungen und Serverfehler) und Nutzungsmetriken (z. B. Speicherbelegung) überwachen."
	services="documentdb"
	documentationCenter=""
	authors="mimig1"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/10/2016"
	ms.author="mimig"/>

# Überwachen von Anforderungen, Nutzung und Speicher in DocumentDB

Sie können Ihre DocumentDB-Konten im [Microsoft Azure-Portal](https://portal.azure.com/) überwachen. Für jedes DocumentDB-Konto sind sowohl Leistungsmetriken (wie Anforderungen und Serverfehler) als auch Nutzungsmetriken (wie belegter Speicher) verfügbar.

## Anzeigen von Leistungsmetriken im Portal 
1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com/) in einem neuen Fenster, und klicken Sie nacheinander auf **Durchsuchen** und **DocumentDB-Konten**. Klicken Sie anschließend auf den Namen des DocumentDB-Kontos, dessen Leistungsmetriken Sie anzeigen möchten.
2.	Innerhalb des Bereichs **Überwachung** sehen Sie standardmäßig Folgendes:
	*	Gesamtanforderungen des aktuellen Tags.
	*	Gesamtanzahl der [Anforderungseinheiten](documentdb-request-units.md) (Request Units, RUs), die am aktuellen Tag verwendet wurden.
	*	Verwendeter Speicher.

	Wenn in der Tabelle die Meldung **Keine Daten verfügbar** angezeigt wird, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) entsprechende Informationen.

	![Screenshot des Überwachungsbereichs, in dem die Gesamtzahl der Anforderungen des aktuellen Tages und die Speichernutzung angezeigt werden](./media/documentdb-monitor-accounts/documentdb-total-requests-and-usage.png)


3.	Wenn Sie auf die Kachel **Anforderungen**, **RUs gesamt** oder **Speicher** klicken, wird das detaillierte Blatt **Metrik** geöffnet.
4.	Das Blatt **Metrik** zeigt Details zu den ausgewählten Metriken. Im oberen Bereich des Blatts befindet sich ein Diagramm der pro Stunde erfassten Anforderungen. Darunter befindet sich eine Tabelle, die Aggregierungswerte für gedrosselte Anforderungen und Anforderungen gesamt zeigt. Das Fenster „Metrik“ zeigt auch eine Liste von Warnung, die definiert wurden. Sie ist gefiltert nach den im aktuellen Fenster „Metrik“ anzeigten Metriken (wenn Sie eine Reihe von Warnungen haben, dann werden auf diese Weise hier nur die relevanten gezeigt).   

	![Screenshot des Blatts „Metrik“ mit gedrosselten Anforderungen](./media/documentdb-monitor-accounts/documentdb-metric-blade.png)


## Anpassen von Leistungsmetrikansichten im Portal

1.	Klicken Sie zum Anpassen der in einem bestimmten Diagramm angezeigten Metriken mit der rechten Maustaste auf das Diagramm, um es auf dem Blatt **Metrik** zu öffnen, und klicken Sie dann auf **Diagramm bearbeiten**. ![Screenshot der Steuerelemente auf dem Blatt „Metrik“ mit hervorgehobener Option „Diagramm bearbeiten“](./media/documentdb-monitor-accounts/madocdb3.png)

2.	Das Blatt **Diagramm bearbeiten** bietet Optionen zum Ändern der in diesem Diagramm angezeigten Metriken sowie des zugehörigen Zeitraums. ![Screenshot des Blatts "Diagramm bearbeiten"](./media/documentdb-monitor-accounts/madocdb4.png)

3.	Sie können die im Detail angezeigten Metriken ändern, indem Sie einfach die verfügbaren Leistungsmetriken auswählen oder löschen und dann unten auf dem Blatt auf **OK** klicken.
4.	Um den Zeitraum zu ändern, wählen Sie einen anderen Zeitraum (z.B. **Benutzerdefiniert**) und klicken unten auf dem Blatt auf **OK**.  

	![Screenshot des Details "Zeitraum" auf dem Blatt "Diagramm bearbeiten" mit Eingabe eines benutzerdefinierten Zeitraums](./media/documentdb-monitor-accounts/madocdb5.png)


## Erstellen von nebeneinander stehenden Diagrammen im Portal
Im Azure-Portal können Sie nebeneinander stehende Metrikdiagramme erstellen.

1.	Klicken Sie zuerst mit der rechten Maustaste auf das Diagramm, das Sie kopieren möchten, und wählen Sie **Anpassen** aus.

	![Screenshot des Diagramms "Anforderungen insgesamt" mit hervorgehobener Option "Anpassen"](./media/documentdb-monitor-accounts/madocdb6.png)

2.	Klicken Sie im Menü auf **Klonen**, um das Detail zu kopieren, und klicken Sie dann auf **Anpassung fertig**.

	![Screenshot des Diagramms "Anforderungen insgesamt" mit hervorgehobenen Optionen "Klonen" und "Anpassung fertig"](./media/documentdb-monitor-accounts/madocdb7.png)


Sie können diesen Bereich jetzt wie jeden anderen Metrikbereich behandeln und ihn sowie die dort angezeigte Zeitspanne anpassen. Auf diese Weise können Sie zwei verschiedene Metrikdiagramme gleichzeitig nebeneinander anzeigen. ![Screenshot des Diagramms "Anforderungen insgesamt" und des Diagramms "Anforderungen insgesamt – letzte Stunde"](./media/documentdb-monitor-accounts/madocdb8.png)

## Einrichten von Warnungen im Portal
1.	Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Durchsuchen**, dann auf **DocumentDB-Konten** und anschließend auf den Namen des DocumentDB-Kontos, für das Sie Warnungen zu Leistungsmetriken einrichten möchten.

2.	Wenn das Blatt **Alle Einstellungen** nicht geöffnet ist, klicken Sie oben links auf den Befehl **Einstellungen**, um das Blatt zu öffnen. ![Screenshot des Befehls „Einstellungen“ auf dem Datenbankkontoblatt](./media/documentdb-monitor-accounts/madocdb10.png)

3.	Klicken Sie auf dem Blatt **Alle Einstellungen** auf **Warnungsregeln**, um das Blatt „Warnungsregeln“ zu öffnen. ![Screenshot des ausgewählten Warnungsregelnteils](./media/documentdb-monitor-accounts/madocdb10.5.png)

4.	Klicken Sie auf dem Blatt "Warnungsregeln" auf **Warnung hinzufügen**.![Screenshot des Blatts "Warnungsregeln" mit hervorgehobener Schaltfläche "Warnung hinzufügen"](./media/documentdb-monitor-accounts/madocdb11.png)

5.	Legen Sie im Fenster **Warnungsregel hinzufügen** Folgendes fest:
	*	Den Namen der Warnungsregel, die Sie einrichten möchten.
	*	Eine Beschreibung der neuen Warnungsregel.
	*	Die Metrik für die Warnungsregel.
	*	Bedingung, Schwellenwert und Zeitraum die festlegen, wann die Warnung aktiviert wird. Zum Beispiel mehr als 5 Serverfehler im Verlauf der letzten 15 Minuten.
	*	Ob der Dienstadministrator oder Coadministratoren bei Auslösung der Warnung eine E-Mail erhalten.
	*	Weitere E-Mail-Adressen für Warnbenachrichtigungen. ![Screenshot des Blatts "Warnungsregel hinzufügen"](./media/documentdb-monitor-accounts/madocdb12.png)

## Programmgesteuertes Überwachen von DocumentDB
Die im Portal für Konten verfügbaren Metriken, z. B. für die Speichernutzung von Konten und die Gesamtzahl der Anforderungen, stehen über die DocumentDB-APIs nicht zur Verfügung. Sie können jedoch mithilfe der DocumentDB-APIs die Nutzungsdaten auf Sammlungsebene abrufen. Gehen Sie zum Abrufen von Daten auf Sammlungsebene wie folgt vor:

- Führen Sie zur Verwendung der REST-API [einen GET-Befehl für die Sammlung](https://msdn.microsoft.com/library/mt489073.aspx) aus. Die Kontingent- und Nutzungsinformationen für die Sammlung werden in den Headern „x-ms-resource-quota“ und „x-ms-resource-usage“ in der Antwort zurückgegeben.
- Verwenden Sie für das .NET SDK die Methode [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx), die eine [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) mit mehreren Nutzungseigenschaften zurückgibt, z.B. **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage** sowie weitere Eigenschaften.

Um auf weitere Metriken zuzugreifen, verwenden Sie das [Azure Insights SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Verfügbare Metrikdefinitionen können folgendermaßen aufgerufen werden:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08 

Abfragen zum Abrufen einzelner Metriken verwenden das folgende Format:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Weitere Informationen finden Sie unter [Retrieving Resource Metrics via the Azure Insights API](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/) (Abrufen von Ressourcenmetriken über die Azure Insights-API).

## Problembehandlung
Wenn in den Überwachungskacheln die Meldung **Keine Daten verfügbar** angezeigt wird und Sie unlängst in der Datenbank Anforderungen durchgeführt oder Daten hinzugefügt haben, können Sie die Kacheln so ändern, dass die letzte Nutzung angezeigt wird.

### Bearbeiten einer Kachel zum Aktualisieren der aktuellen Daten
1.	Klicken Sie zum Anpassen der in einem bestimmten Detail angezeigten Metriken mit der rechten Maustaste auf das Diagramm, um das Blatt **Metrik** zu öffnen, und klicken Sie dann auf **Diagramm bearbeiten**. ![Screenshot der Steuerelemente auf dem Blatt „Metrik“ mit hervorgehobener Option „Diagramm bearbeiten“](./media/documentdb-monitor-accounts/madocdb3.png)

2.	Klicken Sie auf dem Blatt **Diagramm bearbeiten** im Abschnitt **Zeitbereich** auf **letzte Stunde** und dann auf **OK**. ![Screenshot des Blatts „Diagramm bearbeiten“ mit ausgewählter Option „letzte Stunde“](./media/documentdb-monitor-accounts/documentdb-no-available-data-past-hour.png)


3.	In der aktualisierten Kachel sollten nun die aktuellen Daten und Angaben zur Nutzung angezeigt werden. ![Screenshot der aktualisierten Kachel „Anforderungen insgesamt – letzte Stunde“](./media/documentdb-monitor-accounts/documentdb-no-available-data-fixed.png)

## Nächste Schritte
Weitere Informationen zur Kapazität von DocumentDB finden Sie unter [Verwalten der Kapazität von DocumentDB](documentdb-manage.md).

<!---HONumber=AcomDC_0615_2016-->