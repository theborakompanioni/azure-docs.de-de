---
title: Echtzeitstatistiken in Azure CDN | Microsoft Docs
description: Echtzeitstatistiken liefern Echtzeitdaten über die Leistung von Azure CDN bei der Übermittlung von Inhalten an Ihre Clients.
services: cdn
documentationcenter: ''
author: camsoper
manager: erikre
editor: ''

ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper

---
# Echtzeitstatistiken in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## Übersicht
Dieses Dokument erläutert Echtzeitstatistiken in Microsoft Azure CDN. Mit dieser Funktionalität werden Echtzeitdaten bereitgestellt, z.B. Bandbreite, Cachestatus und gleichzeitige Verbindungen mit Ihrem CDN-Profil, wenn Inhalte für Ihre Clients bereitgestellt werden. Dies ermöglicht jederzeit eine fortlaufende Überwachung der Integrität Ihres Diensts, einschließlich von Ereignissen bei Freischaltungen.

Die folgenden Diagramme sind verfügbar:

* [Bandbreite](#bandwidth)
* [Statuscodes](#status-codes)
* [Cachestatus](#cache-statuses)
* [Verbindungen](#connections)

## Zugreifen auf Echtzeitstatistiken
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem CDN-Profil.
   
    ![CDN-Profilblatt](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. Klicken Sie auf dem Blatt „CDN-Profil“ auf die Schaltfläche **Verwalten**.
   
    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    Das CDN-Verwaltungsportal wird geöffnet.
3. Zeigen Sie auf die Registerkarte **Analysen** und dann auf das Flyout **Echtzeitstatistiken**. Klicken Sie auf **Großes HTTP-Objekt**.
   
    ![CDN-Verwaltungsportal](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    Die Echtzeitstatistiken werden angezeigt.

Für alle Diagramme werden Echtzeitstatistiken für den ausgewählten Zeitraum angezeigt. Der Zeitraum beginnt, wenn die Seite geladen wird. Die Diagramme werden nach einigen Sekunden automatisch aktualisiert. Mit der Schaltfläche **Diagramm aktualisieren** (falls vorhanden) wird das Diagramm gelöscht, und danach werden nur die ausgewählten Daten angezeigt.

## Bandbreite
![Diagramm „Bandbreite“](./media/cdn-real-time-stats/cdn-bandwidth.png)

Das Diagramm **Bandbreite** zeigt die Menge der für die aktuelle Plattform verwendeten Bandbreite während des ausgewählten Zeitraums an. Der schattierte Teil des Diagramms zeigt die Bandbreitennutzung an. Die genaue Menge der Bandbreite, die gerade verwendet wird, wird direkt unter dem Liniendiagramm angezeigt.

## Statuscodes
![Diagramm „Statuscode“](./media/cdn-real-time-stats/cdn-status-codes.png)

Das Diagramm **Statuscodes** gibt an, wie oft bestimmte HTTP-Antwortcodes im ausgewählten Zeitraum auftreten.

> [!TIP]
> Eine Beschreibung der einzelnen HTTP-Statuscodeoptionen finden Sie unter [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx) (Azure CDN-HTTP-Statuscodes).
> 
> 

Eine Liste mit den HTTP-Statuscodes wird direkt über dem Diagramm angezeigt. Diese Liste gibt jeden Statuscode an, der im Liniendiagramm enthalten sein kann, sowie die aktuelle Anzahl der Vorkommen pro Sekunde für den jeweiligen Statuscode. Standardmäßig wird für jeden Statuscode in dem Diagramm eine Linie angezeigt. Sie können jedoch auch nur die Statuscodes überwachen, die besonderen Bedeutung für Ihre CDN-Konfiguration haben. Überprüfen Sie hierzu die gewünschten Statuscodes, und löschen Sie alle anderen Optionen. Klicken Sie anschließend auf **Diagramm aktualisieren**.

Sie können protokollierte Daten für einen bestimmten Statuscode vorübergehend ausblenden. Klicken Sie in der Legende direkt unterhalb des Diagramms auf den Statuscode, den Sie ausblenden möchten. Der Statuscode wird im Diagramm sofort ausgeblendet. Wenn Sie erneut auf den Statuscode klicken, wird die Option wieder angezeigt.

## Cachestatus
![Diagramm „Cachestatus“](./media/cdn-real-time-stats/cdn-cache-status.png)

Das Diagramm **Cachestatus** gibt an, wie oft bestimmte Typen von Cachestatus während des ausgewählten Zeitraums auftreten.

> [!TIP]
> Eine Beschreibung der einzelnen Cachestatuscode-Optionen finden Sie unter [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) (Azure CDN-Cachestatuscodes).
> 
> 

Eine Liste mit den Cachestatuscodes wird direkt über dem Diagramm angezeigt. Diese Liste gibt jeden Statuscode an, der im Liniendiagramm enthalten sein kann, sowie die aktuelle Anzahl der Vorkommen pro Sekunde für den jeweiligen Statuscode. Standardmäßig wird für jeden Statuscode in dem Diagramm eine Linie angezeigt. Sie können jedoch auch nur die Statuscodes überwachen, die besonderen Bedeutung für Ihre CDN-Konfiguration haben. Überprüfen Sie hierzu die gewünschten Statuscodes, und löschen Sie alle anderen Optionen. Klicken Sie anschließend auf **Diagramm aktualisieren**.

Sie können protokollierte Daten für einen bestimmten Statuscode vorübergehend ausblenden. Klicken Sie in der Legende direkt unterhalb des Diagramms auf den Statuscode, den Sie ausblenden möchten. Der Statuscode wird im Diagramm sofort ausgeblendet. Wenn Sie erneut auf den Statuscode klicken, wird die Option wieder angezeigt.

## Verbindungen
![Diagramm „Verbindungen“](./media/cdn-real-time-stats/cdn-connections.png)

Dieses Diagramm gibt an, wie viele Verbindungen mit den Edgeservern hergestellt wurden. Jede Anforderung für ein Asset, die das CDN passiert, ergibt eine Verbindung.

## Nächste Schritte
* Benachrichtigungen mit [Echtzeitwarnungen in Azure CDN](cdn-real-time-alerts.md)
* Ausführlichere Informationen zu [erweiterten HTTP-Berichten](cdn-advanced-http-reports.md)
* Analyse von [Verwendungsmustern](cdn-analyze-usage-patterns.md)

<!---HONumber=AcomDC_0824_2016-->