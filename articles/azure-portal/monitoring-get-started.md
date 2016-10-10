<properties
	pageTitle="Erste Schritte mit Azure Monitor | Microsoft Azure"
	description="Starten Sie mit Azure Monitor, um Einblicke in die Vorgänge Ihrer Ressourcen zu erhalten, und ergreifen Sie basierend auf diesen Daten ggf. entsprechende Maßnahmen."
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/26/2016"
	ms.author="johnkem"/>

# Erste Schritte mit Azure Monitor

Azure Monitor ist ein neuer Plattformdienst, mit dem Sie Ihre Azure-Ressourcen von einer einzigen Quelle aus verwalten können. Mit Azure Monitor können Sie Metriken und Protokolle aus Ihren Azure-Ressourcen visualisieren, abfragen, weiterleiten und archivieren und ggf. notwendige Maßnahmen ergreifen. Sie können im Portal im Blatt „Monitor“, in Insights PowerShell-Cmdlets, in einer plattformübergreifende Befehlszeilenschnittstelle oder in Azure Insights-REST-APIs mit diesen Daten arbeiten. In diesem Artikel zeigen wir Ihnen einige der wichtigsten Komponenten von Azure Monitor.

1. Navigieren Sie im Portal zu **Weitere Dienste**, und suchen Sie die Option **Überwachen**. Klicken Sie auf das Sternsymbol, um diese Option zu Ihrer Favoritenliste hinzuzufügen. So ist sie über die linke Navigationsleiste immer schnell zu erreichen.

    ![Azure Monitor in der Dienstliste](./media/monitoring-get-started/monitor-more-services.png)

2. Klicken Sie auf die Option **Überwachen**, um das Blatt **Überwachen** zu öffnen. Auf diesem Blatt sind alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammengefasst. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

    ![Navigation auf dem Blatt „Monitor“](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] Die Optionen **Dienstbenachrichtigungen** und **Benachrichtigungsgruppen** sind Teil der „privaten Vorschau“ und werden nur angezeigt, wenn Sie an dieser teilnehmen.

    Azure Monitor bietet drei grundlegende Kategorien von Überwachungsdaten: das Aktivitätsprotokoll, Metriken und Diagnoseprotokolle.

3. Klicken Sie auf **Aktivitätsprotokoll**, um sicherzustellen, dass der Abschnitt „Aktivitätsprotokoll“ angezeigt wird.

    ![Blatt „Aktivitätsprotokoll“](./media/monitoring-get-started/monitor-act-log-blade.png)

    Im **Aktivitätsprotokoll** werden alle Vorgänge beschrieben, die in den Ressourcen in Ihrem Abonnement durchgeführt wurden. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Das Aktivitätsprotokoll enthält beispielsweise Informationen darüber, wann und von wem eine Web-App beendet wurde. Aktivitätsprotokollereignisse werden in der Plattform 90 Tage lang gespeichert.
   
    Sie können Abfragen für allgemeine Filter erstellen und speichern und dann die wichtigsten Abfragen an ein Portaldashboard anheften. So können Sie jederzeit sehen, ob Ereignisse aufgetreten sind, die Ihre Kriterien erfüllen.

4. Filtern Sie die Ansicht für eine bestimmte Ressourcengruppe nach der letzten Woche, und klicken Sie dann auf die Schaltfläche **Speichern**.

    ![Aktivitätsprotokollabfrage speichern](./media/monitoring-get-started/monitor-act-log-save.png)

5. Klicken Sie jetzt auf die Schaltfläche **Anheften**.

    ![Aktivitätsprotokoll anheften](./media/monitoring-get-started/monitor-act-log-pin.png)

    Die meisten Ansichten, die in dieser exemplarischen Vorgehensweise erläutert werden, können an ein Dashboard angeheftet werden. So können Sie eine einzige Informationsquelle für operative Daten von Ihren Diensten erstellen.

6. Wechseln Sie zurück zum Dashboard. Sie sehen nun, dass die Abfrage (und die Anzahl von Ergebnissen) auf Ihrem Dashboard angezeigt wird.

    ![Aktivitätsprotokoll wurde an Dashboard angeheftet](./media/monitoring-get-started/monitor-act-log-db.png)

7. Kehren Sie zurück zur Kachel **Überwachen**, und klicken Sie auf den Abschnitt **Metriken**. Zunächst müssen Sie durch Filtern und Auswählen der Optionen oben im Abschnitt eine Ressource auswählen.

    ![Ressourcen für Metriken filtern](./media/monitoring-get-started/monitor-met-filter.png)

    Metriken werden von allen Azure-Ressourcen ausgegeben. In dieser Ansicht werden alle in einer einzigen Konsole zusammengefasst.

8. Sobald Sie eine Ressource ausgewählt haben, werden alle verfügbaren Metriken auf der linken Seite des Blatts angezeigt. Sie können mehrere Metriken auf einmal in einem Diagramm darstellen, indem Sie die gewünschten Metriken auswählen und den Diagrammtyp und Zeitraum ändern. Sie können auch alle Metrikwarnungen anzeigen, die für diese Ressource eingerichtet wurden.

    ![Blatt "Metrik"](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Einige der Metriken sind nur verfügbar, wenn [Application Insights](../application-insights/app-insights-overview.md) für Ihre Ressource aktiviert wurde.

9. Wenn Sie mit dem Diagramm zufrieden sind, können Sie es über die Schaltfläche **Anheften** an Ihr Dashboard anheften.

10. Kehren Sie zum Blatt **Überwachen** zurück, und klicken Sie auf **Diagnoseprotokolle**.

    ![Blatt „Diagnoseprotokolle“](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    Diagnoseprotokolle sind von einer Ressource ausgegebene Protokolle, in denen Daten zum Betrieb dieser bestimmten Ressource erfasst werden. Regelzähler für Netzwerksicherheitsgruppen und Workflowprotokolle für Logik-Apps sind Beispiele für solche Diagnoseprotokolle. Diese Protokolle können für erweiterte Suchvorgänge und Warnungen in einem Speicherkonto gespeichert, per Stream an einen Event Hub gesendet und/oder an [Log Analytics](../log-analytics/log-analytics-overview.md), das Operational Intelligence-Produkt von Microsoft, gesendet werden.
   
    Um zu überprüfen, ob die entsprechenden Diagnoseprotokolle aktiviert sind, können Sie im Portal eine Liste aller Ressourcen in Ihrem Abonnement anzeigen und filtern.

11. Klicken Sie auf dem Blatt „Diagnoseprotokolle“ auf eine Ressource. Wenn Diagnoseprotokolle in einem Speicherkonto gespeichert sind, sehen Sie nun eine Liste mit stündlich erstellten Protokollen, die Sie direkt herunterladen können. Sie können auch auf „Diagnose aktivieren/deaktivieren“ klicken, wodurch Sie die Archivierung in einem Speicherkonto, das Streamen an Event Hubs oder das Senden an einen Log Analytics-Arbeitsbereich einrichten können.

    ![Diagnoseprotokolle für eine Ressource](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Wenn Sie Diagnoseprotokolle in Log Analytics eingerichtet haben, können Sie diese dann im Portal im Abschnitt **Protokollsuche** suchen.

12. Navigieren Sie auf dem Blatt „Überwachen“ zum Abschnitt **Warnungen**.

    ![Blatt „Warnungen“ für öffentliche Vorschau](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Hier können Sie alle Warnungen für Ihre Azure-Ressourcen verwalten. Dies beinhaltet auch Warnungen für Metriken, Aktivitätsprotokollereignisse (Vorschau) sowie Webtests (Speicherorte) und proaktive Diagnosen in Application Insights. Warnungen können das Senden einer E-Mail oder eines POST-Befehls an einen Webhook auslösen.
   
13. Klicken Sie auf **Metrikwarnungen hinzufügen**, um eine Warnung zu erstellen.

    ![Metrikwarnung hinzufügen](./media/monitoring-get-started/monitor-alerts-add.png)

    Sie können dann eine Warnung an Ihr Dashboard anheften, um den Status jederzeit einfach einsehen zu können.

14. Auch der Abschnitt „Überwachen“ enthält Links zu [Application Insights](../application-insights/app-insights-overview.md)-Anwendungen und [Log Analytics](../log-analytics/log-analytics-overview.md)-Verwaltungslösungen. Diese Microsoft-Produkte sind eng in Azure Monitor integriert.

15. Falls Sie Application Insights oder Log Analytics nicht verwenden, ist es gut möglich, dass eine Partnerschaft zwischen Azure Monitor und Ihrem aktuellen Produktanbieter für Überwachung, Protokollierung und Warnungen besteht. Eine vollständige Liste unserer Partner sowie Anleitungen zur Integration finden Sie auf unserer [Partner-Website](./monitoring-partners.md).

Wenn Sie diese Schritte befolgen und alle wichtigen Kacheln an ein Dashboard anheften, können Sie umfassende Ansichten Ihrer Anwendung und Infrastruktur erstellen. Hier ein Beispiel:

![Azure Monitor-Dashboard](./media/monitoring-get-started/monitor-final-dash.png)


## Nächste Schritte
- Lesen Sie die [Übersicht über Azure Monitor](./monitoring-overview.md).

<!---HONumber=AcomDC_0928_2016-->