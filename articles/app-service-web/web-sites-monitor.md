---
title: Überwachen von Apps in Azure App Service
description: In diesem Thema wird die Überwachung von Apps in Azure App Service mit dem Azure-Portal beschrieben.
services: app-service
documentationcenter: ''
author: btardif
manager: wpickett
editor: mollybos

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal

---
# Gewusst wie: Überwachen von Apps in Azure App Service
[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) stellt integrierte Überwachungsfunktionen im [Azure-Portal](https://portal.azure.com) bereit. Dies beinhaltet auch die Fähigkeit zum Überprüfen von **Kontingenten** und **Metriken** für eine App und des App Service-Plans, das Einrichten von **Warnungen** und sogar das automatische **Skalieren** basierend auf diesen Metriken.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Grundlagen von Kontingenten und Metriken
### Kontingente
In App Service gehostete Anwendungen unterliegen bestimmten *Grenzwerten* in Bezug auf die verwendbaren Ressourcen. Die Grenzwerte werden mit dem **App Service-Plan** definiert, der der App zugeordnet ist.

Wenn die Anwendung unter einem Plan vom Typ **Free** oder **Shared** gehostet wird, werden die Grenzwerte für die Ressourcen, die von der App verwendet werden können, mit **Kontingenten** definiert.

Wenn die Anwendung unter einem Plan vom Typ **Basic**, **Standard** oder **Premium** gehostet wird, werden die Grenzwerte der Ressourcen, die verwendet werden können, über die Elemente **Größe** (Klein, Mittel, Groß) und **Instanzanzahl** (1, 2, 3, ...) des **App Service-Plans** festgelegt.

**Kontingente** für **Free**- oder **Shared**-Apps sind:

* **CPU(Short)** (CPU (kurz))
  * Zulässige CPU-Menge für diese Anwendung in einem 3-Minuten-Intervall. Dieses Kontingent wird alle drei Minuten zurückgesetzt.
* **CPU(Day)** (CPU (Tag))
  * Zulässige CPU-Gesamtmenge für diese Anwendung für einen Tag. Dieses Kontingent wird alle 24 Stunden um Mitternacht (UTC) zurückgesetzt.
* **Memory** (Arbeitsspeicher)
  * Zulässige Gesamtmenge an Arbeitsspeicher für diese Anwendung.
* **Bandwidth** (Bandbreite)
  * Zulässige Gesamtmenge an ausgehender Bandbreite für diese Anwendung für einen Tag. Dieses Kontingent wird alle 24 Stunden um Mitternacht (UTC) zurückgesetzt.
* **Filesystem** (Dateisystem)
  * Gesamtbetrag des zulässigen Speichers.

Das einzige Kontingent, das für Apps gilt, die unter Plänen vom Typ **Basic**, **Standard** und **Premium** gehostet werden, ist **Filesystem**.

Weitere Informationen zu den spezifischen Kontingenten, Grenzwerten und Features, die für die unterschiedlichen App Service-SKUs verfügbar sind, finden Sie hier: [Diensteinschränkungen des Azure-Abonnements](../azure-subscription-service-limits.md#app-service-limits).

#### Kontingenterzwingung
Wenn die Nutzung einer Anwendung das Kontingent für **CPU(Short)**, **CPU(Day)** oder **Bandwidth** überschreitet, wird die Anwendung beendet, bis das Kontingent zurückgesetzt wird. Während dieses Zeitraums führen alle eingehenden Anforderungen zu einem **HTTP 403**-Fehler. ![][http403]

Wenn das Kontingent **Memory** der Anwendung überschritten wird, wird die Anwendung neu gestartet.

Wenn das Kontingent **Filesystem** überschritten wird, schlagen alle Schreibvorgänge fehl. Dies gilt auch für das Schreiben in Protokolle.

Kontingente können erhöht oder aus Ihrer App entfernt werden, indem Sie ein Upgrade für den App Service-Plan durchführen.

### Metriken
**Metriken** liefern Informationen zur App oder zum Verhalten des App Service-Plans.

Für eine **Anwendung** lauten die verfügbaren Metriken:

* **Durchschnittliche Antwortzeit**
  * Die durchschnittliche Zeit in Millisekunden, die die App zum Bereitstellen von Anforderungen benötigt.
* **Durchschnittlicher Arbeitssatz für Arbeitsspeicher**
  * Die durchschnittliche Arbeitsspeichermenge in MiBs, die von der Anwendung verbraucht wird.
* **CPU-Zeit**
  * Die CPU-Menge in Sekunden, die von der App verbraucht wird. Weitere Informationen zu diesen Metriken finden Sie unter [CPU-Zeit und CPU-Prozentsatz](#cpu-time-vs-cpu-percentage).
* **Eingehende Daten**
  * Die Menge an eingehender Bandbreite in MiBs, die von der App verbraucht wird.
* **Ausgehende Daten**
  * Die Menge an ausgehender Bandbreite in MiBs, die von der App verbraucht wird.
* **HTTP 2xx**
  * Anzahl von Anforderungen, die zu einem HTTP-Statuscode >= 200 und < 300 führen.
* **HTTP 3xx**
  * Anzahl von Anforderungen, die zu einem HTTP-Statuscode >= 300 und < 400 führen.
* **HTTP 401**
  * Anzahl von Anforderungen, die zu einem HTTP 401-Statuscode führen.
* **HTTP 403**
  * Anzahl von Anforderungen, die zu einem HTTP 403-Statuscode führen.
* **HTTP 404**
  * Anzahl von Anforderungen, die zu einem HTTP 404-Statuscode führen.
* **HTTP 406**
  * Anzahl von Anforderungen, die zu einem HTTP 406-Statuscode führen.
* **HTTP 4xx**
  * Anzahl von Anforderungen, die zu einem HTTP-Statuscode >= 400 und < 500 führen.
* **HTTP-Serverfehler**
  * Anzahl von Anforderungen, die zu einem HTTP-Statuscode >= 500 und < 600 führen.
* **Arbeitssatz für Arbeitsspeicher**
  * Aktuelle Menge von Arbeitsspeicher in MiBs, die von der App verwendet wird.
* **Anforderungen**
  * Gesamtzahl von Anforderungen, unabhängig vom sich ergebenden HTTP-Statuscode.

Für einen **App Service-Plan** lauten die verfügbaren Metriken:

> [!NOTE]
> App Service-Planmetriken sind nur für Pläne in SKUs vom Typ **Basic**, **Standard** und **Premium** verfügbar.
> 
> 

* **CPU-Prozentsatz**
  * Die durchschnittliche CPU-Nutzung über alle Instanzen des Plans hinweg.
* **Arbeitsspeicherprozentsatz**
  * Die durchschnittliche Arbeitsspeichernutzung über alle Instanzen des Plans hinweg.
* **Eingehende Daten**
  * Die durchschnittliche eingehende Bandbreite, die für alle Instanzen des Plans verwendet wird.
* **Ausgehende Daten**
  * Die durchschnittliche ausgehende Bandbreite, die für alle Instanzen des Plans verwendet wird.
* **Länge der Datenträgerwarteschlange**
  * Die durchschnittliche Anzahl von Lese- und Schreibanforderungen, die im Speicher in die Warteschlange eingereiht wurden. Ein hoher Wert für die Länge der Datenträgerwarteschlange ist ein Anzeichen dafür, dass eine Anwendung aufgrund einer übermäßigen E/A-Aktivität des Datenträgers verlangsamt wird.
* **Länge der HTTP-Warteschlange**
  * Die durchschnittliche Anzahl von HTTP-Anforderungen, die sich in der Warteschlange befinden müssen, bevor sie erfüllt werden. Eine hohe oder zunehmende HTTP-Warteschlangenlänge deutet darauf hin, dass für einen Plan eine hohe Auslastung besteht.

### CPU-Zeit und CPU-Prozentsatz
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Es gibt zwei Metriken, die die CPU-Auslastung widerspiegeln: **CPU-Zeit** und **CPU-Prozentsatz**

**CPU-Zeit** ist für Apps hilfreich, die unter einem Plan vom Typ **Free** oder **Shared** gehostet werden, da eines der Kontingente basierend auf den von der App verbrauchten CPU-Minuten definiert ist.

Der **CPU-Prozentsatz** ist dagegen für Apps hilfreich, die unter Plänen vom Typ **Basic**, **Standard** und **Premium** gehostet werden, da sie horizontal hochskaliert werden können. Diese Metrik liefert nützliche Informationen zur gesamten Nutzung über alle Instanzen hinweg.

## Granularität und Aufbewahrungsrichtlinien für Metriken
Metriken für eine Anwendung und einen App Service-Plan werden vom Dienst mit den folgenden Granularitäten und Aufbewahrungsrichtlinien protokolliert und aggregiert:

* Metriken mit der Granularität **Minute** werden für **48 Stunden** beibehalten.
* Metriken mit der Granularität **Stunde** werden für **30 Tage** beibehalten.
* Metriken mit der Granularität **Tag** werden für **90 Tage** beibehalten.

## Überwachen von Kontingenten und Metriken im Azure-Portal
Sie können den Status der unterschiedlichen **Kontingente** und **Metriken**, die sich auf eine Anwendung auswirken, im [Azure-Portal](https://portal.azure.com) überprüfen.

![][quotas] Die **Kontingente** befinden sich unter „Einstellungen“ > **Kontingente**. Auf der Benutzeroberfläche können Sie Folgendes überprüfen: (1) den Namen der Kontingente, (2) das Zurücksetzungsintervall, (3) die aktuelle Grenze und (4) den aktuellen Wert.

![][metrics] Auf **Metriken** kann direkt über das Ressourcenblatt zugegriffen werden. Sie können das Diagramm auch wie folgt anpassen: (1) **Klicken** auf das Diagramm und (2) Option **Diagramm bearbeiten** wählen. Hier können Sie (3) **Zeitbereich**, (4) **Diagrammtyp** und (5) **Metriken** für die Anzeige auswählen.

Weitere Informationen zu Metriken finden Sie unter [Überwachen von Dienstmetriken](../azure-portal/insights-how-to-customize-monitoring.md).

## Warnungen und automatische Skalierung
Metriken für eine App oder einen App Service-Plan können mit Warnungen verknüpft werden. Weitere Informationen hierzu finden Sie unter [Empfangen von Warnbenachrichtigungen](../azure-portal/insights-receive-alert-notifications.md).

App Service-Apps, die unter App Service-Plänen vom Typ Basic, Standard oder Premium gehostet werden, unterstützen die **automatische Skalierung**. Dies ermöglicht Ihnen das Konfigurieren von Regeln zur Überwachung der App Service-Planmetriken und das Erhöhen und Verringern der Instanzanzahl mit der Bereitstellung zusätzlicher Ressourcen bei Bedarf (bzw. dem Sparen von Kosten, wenn die Bereitstellung für die Anwendung zu hoch ist). Weitere Informationen zur automatischen Skalierung finden Sie unter [Schritte zur Skalierung](../azure-portal/insights-how-to-scale.md) und [Empfohlene Methoden für die automatische Skalierung in Azure Insights](../azure-portal/insights-autoscale-best-practices.md).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714).

[fzilla]: http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]: http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png

<!---HONumber=AcomDC_0914_2016-->