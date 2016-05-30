<properties
	pageTitle="System Update Assessment-Lösung in Log Analytics | Microsoft Azure"
	description="Mithilfe der Lösung „System Updates“ in Log Analytics können Sie fehlende Updates auf Server in Ihrer Infrastruktur anwenden."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/06/2016"
	ms.author="banders"/>

# System Update Assessment-Lösung in Log Analytics


Mithilfe der Lösung „System Updates“ in Log Analytics können Sie fehlende Updates auf Server in Ihrer Infrastruktur anwenden. Nach der Installation der Lösung können Sie die Updates, die auf den überwachten Servern fehlen, in Operational Insights über die Kachel **System Update Assessment** auf der Seite **Übersicht** in OMS anzeigen.

Wenn ermittelt wird, dass Updates fehlen, werden ausführliche Informationen im Dashboard **Updates** angezeigt. Mithilfe des Dashboards **Updates** können Sie mit fehlenden Updates arbeiten und einen Plan für deren Anwendung auf die betreffenden Server entwickeln.

## Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses die System Update Assessment-Lösung zu Ihrem OMS-Arbeitsbereich hinzu. Es ist keine weitere Konfiguration erforderlich.

## Details zur System Update-Datensammlung

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für System Update Assessment gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Nein](./media/log-analytics-system-update/oms-bullet-red.png)| ![Nein](./media/log-analytics-system-update/oms-bullet-red.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)| Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates|


### So arbeiten Sie mit Updates

1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **System Update Assessment**. ![Bild der Seite „Übersicht“](./media/log-analytics-system-update/oms-updates01.png)
2. Zeigen Sie auf dem Dashboard **Updates** die Updatekategorien an. ![Bild der Seite „Updates“](./media/log-analytics-system-update/oms-updates02.png)
3. Führen Sie auf der Seite einen Bildlauf nach rechts aus, um das Blatt **Typ fehlender Updates** anzuzeigen, und klicken Sie auf **Sicherheitsupdates**. ![Bild der Seite „Updates“](./media/log-analytics-system-update/oms-updates03.png)
4. Auf der Seite „Suche“ wird eine Liste mit Sicherheitsupdates angezeigt, die für die Server Ihrer Infrastruktur als fehlend ermittelt wurden. Klicken Sie auf einen Knowledge Base-Artikel (KBID), um weitere Informationen zum fehlenden Update anzuzeigen. In diesem Beispiel ist dies *KBID 3032323*. ![Bild der Seite „Updates“](./media/log-analytics-system-update/oms-updates04.png)
5. Im Webbrowser wird der Knowledge Base-Artikel geöffnet, in dem das Update beschrieben wird. ![Abbildung des Knowledge Base-Artikels](./media/log-analytics-system-update/oms-updates05.png)
6. Mit den gefundenen Informationen können Sie einen Plan zum Anwenden fehlender Updates erstellen.

## Nächste Schritte

- [Durchsuchen von Protokollen](log-analytics-log-searches.md), um detaillierte System Update-Daten anzuzeigen

<!---HONumber=AcomDC_0518_2016-->