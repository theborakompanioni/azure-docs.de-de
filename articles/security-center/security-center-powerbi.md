<properties
   pageTitle="Gewinnen von Erkenntnissen aus Azure Security Center-Daten mit Power BI| Microsoft Azure"
   description="Das Azure Security Center Power BI-Inhaltspaket macht es Ihnen leicht, Sicherheitswarnungen und -empfehlungen, angegriffene Ressourcen und Trends ausgehend von einem Datensatz zu finden, der für Ihre Berichte erstellt wurde."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/03/2016"
   ms.author="yurid"/>

# Gewinnen von Erkenntnissen aus Azure Security Center-Daten mit Power BI
Das [Power BI-Dashboard](http://aka.ms/azure-security-center-power-bi) für Azure Security Center ermöglicht Ihnen das Visualisieren, Analysieren und Filtern von Empfehlungen und Sicherheitswarnungen über alle Geräte, einschließlich Ihres Mobilgeräts. Erkennen Sie mit dem Power BI-Dashboard Trends und Angriffsmuster, und zeigen Sie Sicherheitswarnungen nach Ressource oder Quell-IP-Adresse und Sicherheitsrisiken ohne Adresse nach Ressource oder Alter an. Sie können die Sicherheitswarnungen und Empfehlungen aus dem Security Center auch mit anderen Daten kombinieren, z. B. mit [Azure-Überwachungsprotokollen](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) und der [Azure SQL-Datenbanküberwachung](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/), die beide Power BI-Dashboards enthalten. Sie können diese Daten aber in Excel exportieren und so einfach Berichte zum Sicherheitszustand Ihrer Cloudressourcen erstellen.

> [AZURE.NOTE] Die Informationen in diesem Dokument gelten für die Vorschauversion von Azure Security Center.


##Verwenden des Azure Security Center-Dashboards für den Zugriff auf Power BI
Auf Power BI-Berichte können Sie auch über die Azure Security Center-Dashboards zugreifen. Gehen Sie folgendermaßen vor, um diese Aufgabe auszuführen:

1. Klicken Sie im **Azure Security Center**-Dashboard auf die Schaltfläche **In Power BI erkunden**.

	![Mithilfe von Power BI mit Azure Security Center verbinden](./media/security-center-powerbi/security-center-powerbi-fig9-new.png)

2. Das Blatt **In Power BI erkunden** wird wie unten angezeigt auf der rechten Seite geöffnet:

	![Mithilfe von Power BI mit Azure Security Center verbinden](./media/security-center-powerbi/security-center-powerbi-fig2-new.png)

3. Wenn Sie das Power BI-Dashboard zum ersten Mal erstellen, können Sie im Blatt „In Power BI erkunden“ eine der unten aufgeführten Optionen wählen:

	- **Security insights dashboard** (Dashboard „Einblicke in die Sicherheit“): Wählen Sie diese Option, wenn Sie ein Dashboard erstellen möchten, das den Sicherheitsstatus, Threads und Erkennungen enthält. Dies ist eine häufigere Option für Personen mit der DevOps-Rolle, die für das abonnementübergreifende Analysieren ihres Schutzstatus und erkannter Warnungen verantwortlich sind.
	- **Policy management dashboard** (Dashboard „Richtlinienverwaltung“): Wählen Sie diese Option, wenn Sie die Verwaltungs- und Durchsetzungsrichtlinie untersuchen möchten. Dies ist eine häufigere Option für die zentrale IT-Abteilung, bei der es eher um Governance (Kontrolle) geht. Dieses Dashboard bietet Transparenz und Einblicke in die Einhaltung von Sicherheitsrichtlinien im gesamten Unternehmen.
	- Klicken Sie auf **Go to your current Power BI dashboard** (Zu aktuellem Power BI-Dashboard wechseln), wenn Sie bereits über ein Power BI-Dashboard verfügen.

4. Klicken Sie in diesem Beispiel auf **Security insights dashboard** (Dashboard „Einblicke in die Sicherheit“). Das unten dargestellte Fenster wird angezeigt:

	![Azure Security Center – Dashboard „Einblicke in die Sicherheit“](./media/security-center-powerbi/security-center-powerbi-fig3-new.png)

5. Stellen Sie sicher, dass die **Authentifizierungsmethode** auf **OAuth2** festgelegt ist, und klicken Sie auf **Anmelden**.
6. Das **Power BI**-Fenster wird geöffnet, und es wird ein Bericht mit einer Struktur der folgenden Art angezeigt:
	
	![Dashboard „Einblicke in die Sicherheit“](./media/security-center-powerbi/security-center-powerbi-fig5.png)

> [AZURE.NOTE] Für den Bericht ist eine tägliche Aktualisierung geplant. Falls bei dieser Aktualisierung ein Fehler auftritt, finden Sie unter [Potential Refresh Issues with the Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/) (Mögliche Aktualisierungsprobleme mit Azure Security Center Power BI) weitere Informationen zur Problembehandlung.

Hier sehen Sie die Anzahl der Sicherheitswarnungen und Empfehlungen sowie die Zahl der virtuellen Computer, Azure SQL-Datenbanken und Netzwerkressourcen, die von Azure Security Center überwacht werden.

Ein Link zu Azure Security Center leitet Sie zum Azure-Portal weiter. Die Diagramme ermöglichen ein einfaches Visualisieren von Informationen zu Sicherheitsempfehlungen und -warnungen, einschließlich:

- Sicherheitsintegrität von Ressourcen
- Insgesamt ausstehende Vorschläge
- Empfehlungen für virtuelle Computer
- Warnungen im Laufe der Zeit
- Angegriffene Ressourcen
- Angegriffene IPs

Jedes Diagramm liefert Ihnen zusätzliche Einblicke. Wählen Sie eine Kachel aus, um weitere Informationen anzuzeigen. Die Kachel „Sicherheitsintegrität von Ressourcen“ zeigt beispielsweise zusätzliche Details zu ausstehenden Empfehlungen nach Ressourcen an:

![Recommendations](./media/security-center-powerbi/security-center-powerbi-fig6.png)

Wenn Sie auf eine beliebige Zeile in diesem Graphen klicken, werden die anderen abgeblendet, und Sie können Sie sich auf Ihre Auswahl konzentrieren. Klicken Sie im linken Bereich dieser Seite unter der Option **Dashboards** auf **Azure Security Center**, um zum Dashboard zurückzukehren.

> [AZURE.NOTE] Sie können den Bericht bearbeiten und ihn anpassen, zusätzliche Felder hinzufügen oder bestehende Grafiken ändern. Weitere Informationen finden Sie unter [Interagieren mit einem Bericht in der Bearbeitungsansicht von Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/).

Wenn Sie auf die einzelnen Kacheln **Warnungen im Laufe der Zeit**, „Angegriffene Ressourcen“ und **Angreifer-IPs** klicken, ist deren Ausgabe ähnlich. Dies liegt daran, dass der Bericht Informationen zu all diesen drei Variablen unter der Bezeichnung **Angegriffene Ressourcen** zusammenführt:

![Angegriffene Ressourcen](./media/security-center-powerbi/security-center-powerbi-fig7.png)

Nun können Sie mit den Optionen im Menü **Datei** eine Kopie dieses Berichts speichern und ihn ausdrucken oder im Internet veröffentlichen.

![Menü "Datei"](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## Untersuchen von Azure Security Center-Daten mit Power BI-Diensten

Stellen Sie eine Verbindung mit [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) in Power BI her, und führen Sie die unten angegebenen Schritte aus:

1. Im Fenster **Content Pack für Power BI** werden wie unten dargestellt zwei Optionen angezeigt.

	![Inhaltspaket für Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

2. Klicken Sie in diesem Beispiel in der Kachel **Azure Security Center Policy Management** (Azure Security Center-Richtlinienverwaltung) auf **Abrufen**.

3. Stellen Sie im Fenster **Connect to Azure Security Center Policy Management** (Mit Azure Security Center-Richtlinienverwaltung verbinden) sicher, dass in der Dropdownliste **Authentifizierungsmethode** wie unten gezeigt die Option **oAuth2** ausgewählt ist, und klicken Sie auf die Schaltfläche **Anmelden**.

	![Fenster „Richtlinienverwaltung“](./media/security-center-powerbi/security-center-powerbi-fig4-new.png)

4. Sie werden zu einer Authentifizierungsseite umgeleitet. Geben Sie dort die Anmeldedaten ein, die Sie zum Verbinden mit Azure Security Center verwenden. Nach Abschluss des Authentifizierungsprozesses beginnt Power BI mit dem Import von Daten, um die Berichte zu erstellen. Während dieses Zeitraums erscheint rechts unten im Browser möglicherweise diese Meldung:

	![Mithilfe von Power BI mit Azure Security Center verbinden](./media/security-center-powerbi/security-center-powerbi-fig4.png)

	>[AZURE.NOTE] Wenn das Dashboard zum ersten Mal erstellt wird, kann der Vorgang länger als üblich dauern. Dies gilt besonders für Szenarien, in denen mehrere Abonnements verwendet werden.

5. Sobald der Prozess abgeschlossen ist, wird das Azure Security Center Power BI-Dashboard mit dem Bericht **Richtlinienverwaltung** geladen.


## Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Power BI in Azure Security Center verwenden können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

- [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md) – erfahren Sie, wie Sie die Einführung von Azure Security Center planen.
- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – erfahren Sie, wie Sie Sicherheitseinstellungen in Azure Security Center konfigurieren.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) – erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
- [Azure Security Center – häufig gestellte Fragen](security-center-faq.md) zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – suchen Sie nach Blogbeiträgen über Azure-Sicherheit und -Compliance.

<!---HONumber=AcomDC_0608_2016-->