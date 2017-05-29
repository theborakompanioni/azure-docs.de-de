---
title: "Protokollanalyse für Azure CDN | Microsoft-Dokumentation"
description: "Kunden können die Protokollanalyse für Azure CDN aktivieren."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: rli
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: e3be8aced50a2ce330523d8633f524803af8c8a7
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---

# <a name="diagnostics-logs-for-azure-cdn"></a>Diagnoseprotokolle für Azure CDN

Nachdem Sie CDN für Ihre Anwendung aktiviert haben, möchten Sie wahrscheinlich die CDN-Nutzung überwachen, die Integrität der Bereitstellung überprüfen und potenzielle Probleme behandeln. Azure CDN stellt diese Funktionen im Rahmen der [Basisanalyse](cdn-analyze-usage-patterns.md) bereit.

Als aktueller Azure CDN-Benutzer mit Verizon-Standard- oder Premium-Profil können Sie die Basisanalyse bereits im zusätzlichen Portal anzeigen, indem Sie im Azure-Portal die Option „Verwalten“ verwenden. Mit dem neuen Feature [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) können Sie jetzt die Basisanalyse anzeigen und die Daten an einem oder mehreren Zielorten speichern, z.B. Azure Storage-Konto, Azure Event Hub und [Log Analytics-Arbeitsbereich (OMS)](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started). Dieses Feature ist für alle CDN-Endpunkte verfügbar, die zu CDN-Profilen von Verizon (Standard und Premium) und Akamai (Standard) gehören.

Mit Diagnoseprotokollen können Sie Metriken zur grundlegenden Nutzung von Ihrem CDN-Endpunkt auf verschiedene Quellkomponenten exportieren, um sie auf die gewünschte Weise zu nutzen. Sie haben beispielsweise folgende Möglichkeiten:

- Exportieren von Daten in Blobspeicher, Exportieren in eine CSV-Datei und Generieren von Diagrammen in Excel.
- Exportieren von Daten auf Event Hubs und Korrelieren mit Daten von anderen Azure-Diensten
- Exportieren von Daten in Log Analytics und Anzeigen von Daten in Ihrem eigenen OMS-Arbeitsbereich


![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/OMS-workspace.png)

In der unten angegebenen exemplarischen Vorgehensweise werden das Schema der Basisanalysedaten, die Schritte zum Aktivieren des Features und Bereitstellen an unterschiedlichen Zielen und die Nutzung von Daten über diese Ziele beschrieben.

## <a name="enable-logging-with-azure-portal"></a>Aktivieren der Protokollierung über das Azure-Portal

Die Diagnoseprotokolle sind standardmäßig **deaktiviert**. Führen Sie die folgenden Schritte aus, um sie zu aktivieren:


Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an. Wenn Sie CDN für Ihren Workflow nicht bereits aktiviert haben, sollten Sie [Azure CDN vor dem Fortfahren aktivieren](cdn-create-new-endpoint.md).

1. Navigieren Sie im Portal zum **CDN-Profil**.
2. Wählen Sie ein CDN-Profil und dann den CDN-Endpunkt aus, für den Sie die **Diagnoseprotokolle** aktivieren möchten.
    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/Browse-to-Diagnostics-logs.png)
3. Navigieren Sie zum Blatt **Diagnoseprotokolle**, und ändern Sie den Status im Abschnitt **Überwachung** in **Ein**.
    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/Diagnostics-logs-options.png)
4. Wählen und Konfigurieren Sie das gewünschte Archivierungsziel (Speicherkonto, Event Hub, Log Analytics). 
    
    In diesem Beispiel wird Azure Storage zum Speichern der Protokolle verwendet. Wählen Sie die Option **In ein Speicherkonto archivieren**, wählen Sie die Tage für die Aufbewahrungsdauer aus, und klicken Sie unter **Protokoll** auf **CoreAnalytics**. Wir bieten derzeit nur **CoreAnalytics** an, aber in Zukunft werden weitere Protokolltypen verfügbar sein. Unten finden Sie Schema-, Aggregations- und Verzögerungsinformationen zu CoreAnalytics. 

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/Diagnostics-logs-storage.png)
5.  Speichern Sie die neue Diagnosekonfiguration.
    
    Die Verizon-Protokolldaten werden mit einer Stunde Verzögerung bereitgestellt, und es dauert bis zu zwei Stunden, bis die Daten nach Abschluss der Verteilung auf die Endpunkte angezeigt werden.
    Die Akamai-Protokolldaten werden mit einer Verzögerung von 24 Stunden bereitgestellt, und es dauert bis zu zwei Stunden bis zur Anzeige, wenn sie vor mehr als 24 Stunden erstellt wurden. Falls die Daten gerade erst erstellt wurden, kann es bis zu 25 Stunden dauern, bis die Protokolle angezeigt werden.

## <a name="enable-logging-with-powershell"></a>Aktivieren der Protokollierung mit PowerShell

Unten sind zwei Beispiele dazu angegeben, wie Sie Diagnoseprotokolle mit den Azure PowerShell-Cmdlets aktivieren und abrufen.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Aktivieren von Diagnoseprotokollen in einem Speicherkonto

Verwenden Sie den folgenden Befehl, um Diagnoseprotokolle in einem Speicherkonto zu aktivieren:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```

## <a name="consuming-logs-from-storage"></a>Nutzen von Protokollen aus dem Speicher
In diesem Abschnitt werden das Schema der CDN-Basisanalyse und die Organisation innerhalb eines Azure Storage-Kontos beschrieben, und er enthält Beispielcode zum Herunterladen der Protokolle in eine CSV-Datei.

### <a name="using-microsoft-azure-storage-explorer"></a>Verwenden des Microsoft Azure-Speicher-Explorers
Bevor Sie im Azure Storage-Konto auf die Basisanalysedaten zugreifen können, benötigen Sie zuerst ein Tool zum Zugreifen auf den Inhalt in einem Speicherkonto. Auf dem Markt sind verschiedene Tools erhältlich, aber wir empfehlen Ihnen die Verwendung des Microsoft Azure-Speicher-Explorers. Sie können das Tool [hier](http://storageexplorer.com/) herunterladen. Konfigurieren Sie nach dem Herunterladen und Installieren der Software die Verwendung desselben Azure Storage-Kontos, das als Ziel für die CDN-Diagnoseprotokolle konfiguriert wurde.

1.    Öffnen Sie den **Microsoft Azure-Speicher-Explorer**.
2.    Ermitteln Sie das Speicherkonto.
3.    Navigieren Sie zum Knoten **Blobcontainer** unter diesem Speicherkonto, und erweitern Sie ihn.
4.    Wählen Sie den Container mit dem Namen **insights-logs-coreanalytics** aus, und doppelklicken Sie darauf.
5.    Im Bereich auf der rechten Seite werden Ergebnisse angezeigt. Es wird mit der ersten Ebene begonnen: **„resourceId=“**. Klicken Sie weiter, bis die Datei **PT1H.json** angezeigt wird. Eine Beschreibung des Pfads finden Sie unten im Hinweis.
6.    Jede Datei **PT1H.json** des Blobs steht für die Analyseprotokolle für eine Stunde eines bestimmten CDN-Endpunkts oder dessen benutzerdefinierter Domäne.
7.    Das Schema der Inhalte dieser JSON-Datei wird im Abschnitt „Schema der Basisanalyseprotokolle“ beschrieben.


> [!NOTE]
> **Format des Blobpfads**
> 
> Basisanalyseprotokolle werden einmal pro Stunde generiert. Alle Daten einer Stunde werden gesammelt und in einem zentralen Azure-Blob als JSON-Nutzlast gespeichert. Der Pfad zu diesem Azure-Blob wird so angezeigt, als ob eine hierarchische Struktur vorhanden wäre. Dies liegt daran, dass das Speicher-Explorer-Tool „/“ als Verzeichnistrennzeichen interpretiert und die Hierarchie zum besseren Verständnis anzeigt. Eigentlich steht der gesamte Pfad lediglich für den Namen des Blobs. Für den Namen des Blobs wird die folgende Benennungskonvention verwendet:    
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Beschreibung der Felder:**

|value|Beschreibung|
|-------|---------|
|Subscription Id (Abonnement-ID)    |ID des Azure-Abonnements. Hierfür wird das GUID-Format verwendet.|
|Resource Group Name (Ressourcengruppenname) |Name der Ressourcengruppe, der die CDN-Ressourcen angehören.|
|Profile Name (Profilname) |Name des CDN-Profils|
|Endpoint Name (Endpunktname) |Name des CDN-Endpunkts|
|Year (Jahr)|    Vierstellige Jahresangabe, z.B. 2017|
|Month (Monat)|    Zweistellige Monatsangabe: 01 = Januar bis 12 = Dezember|
|Day (Tag)|    Zweistellige Tagesangabe|
|PT1H.json|    JSON-Datei, in der die Analysedaten gespeichert sind|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Exportieren der Basisanalysedaten in eine CSV-Datei

Um den Zugriff auf die Basisanalyse zu vereinfachen, geben wir einen Beispielcode für ein Tool an, mit dem das Herunterladen der JSON-Dateien in einem flachen Dateiformat mit Kommas als Trennzeichen möglich ist. Diese Datei kann dann leicht zum Erstellen von Diagrammen oder Aggregationen verwendet werden.

Sie können das Tool wie folgt verwenden:

1.    Nutzen Sie den GitHub-Link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ).
2.    Laden Sie den Code herunter.
3.    Befolgen Sie die Anleitung zum Kompilieren und Konfigurieren.
4.    Führen Sie das Tool aus.
5.    In der sich ergebenden CSV-Datei werden die Analysedaten in einer einfachen flachen Hierarchie angezeigt.

## <a name="diagnostic-logs-types"></a>Diagnoseprotokolltypen

Wir bieten derzeit nur die Basisanalyseprotokolle an, die Metriken mit HTTP-Antwortstatistiken und -Ausgangsstatistiken für die CDN-POPs/-Edges enthalten. Im Laufe der Zeit fügen wir weitere Protokolltypen hinzu.

### <a name="core-analytics-metrics-details"></a>Details von Basisanalysemetriken
Unten ist eine Liste mit Metriken angegeben, die in den Basisanalyseprotokollen verfügbar sind. Nicht alle Metriken sind für alle Anbieter verfügbar, aber die Unterschiede sind nur minimal. In dieser Tabelle ist jeweils auch angegeben, ob eine Metrik für einen Anbieter verfügbar ist. Beachten Sie, dass die Metriken nur für die CDN-Endpunkte zur Verfügung stehen, für die Datenverkehr anfällt.


|Metrik                     | Beschreibung   | Verizon  | Akamai 
|---------------------------|---------------|---|---|
| RequestCountTotal         |Gesamtzahl von Anforderungstreffern während dieses Zeitraums| Ja  |Ja   |
| RequestCountHttpStatus2xx |Anzahl aller Anforderungen, die zum HTTP-Code 2xx geführt haben (z.B. 200, 202)              | Ja  |Ja   |
| RequestCountHttpStatus3xx | Anzahl aller Anforderungen, die zum HTTP-Code 3xx geführt haben (z.B. 300, 302)              | Ja  |Ja   |
| RequestCountHttpStatus4xx |Anzahl aller Anforderungen, die zum HTTP-Code 4xx geführt haben (z.B. 400, 404)               | Ja   |Ja   |
| RequestCountHttpStatus5xx | Anzahl aller Anforderungen, die zum HTTP-Code 5xx geführt haben (z.B. 500, 504)              | Ja  |Ja   |
| RequestCountHttpStatusOthers |  Anzahl aller anderen HTTP-Codes (außerhalb des Bereichs von 2xx bis 5xx) | Ja  |Ja   |
| RequestCountHttpStatus200 | Anzahl aller Anforderungen, die zu einer Antwort mit dem HTTP-Code 200 geführt haben              |Nein   |Ja   |
| RequestCountHttpStatus206 | Anzahl aller Anforderungen, die zu einer Antwort mit dem HTTP-Code 206 geführt haben              |Nein   |Ja   |
| RequestCountHttpStatus302 | Anzahl aller Anforderungen, die zu einer Antwort mit dem HTTP-Code 302 geführt haben              |Nein   |Ja   |
| RequestCountHttpStatus304 |  Anzahl aller Anforderungen, die zu einer Antwort mit dem HTTP-Code 304 geführt haben             |Nein   |Ja   |
| RequestCountHttpStatus404 | Anzahl aller Anforderungen, die zu einer Antwort mit dem HTTP-Code 404 geführt haben              |Nein   |Ja   |
| RequestCountCacheHit |Anzahl aller Anforderungen, die zu einem Cachetreffer geführt haben. Dies bedeutet, dass das Objekt direkt vom POP für den Client bereitgestellt wurde.               | Ja  |Nein   |
| RequestCountCacheMiss | Anzahl aller Anforderungen, die zu einem Cachefehler geführt haben. Dies bedeutet, dass das Objekt nicht auf dem POP gefunden wurde, der die geringste Entfernung zum Client aufweist, und daher vom Ursprungsserver abgerufen wurde.              |Ja   | Nein  |
| RequestCountCacheNoCache | Anzahl aller Anforderungen eines Objekts, die aufgrund einer Benutzerkonfiguration im Edgebereich nicht zwischengespeichert wurden.              |Ja   | Nein  |
| RequestCountCacheUncacheable | Anzahl aller Anforderungen von Objekten, für die durch die Header „Cache-Control“ und „Expires“ das Zwischenspeichern verhindert wird. Mit diesen Headern wird angegeben, dass keine Zwischenspeicherung auf einem POP oder durch den HTTP-Client erfolgen soll.                |Ja   |Nein   |
| RequestCountCacheOthers | Anzahl aller Anforderungen mit einem Cachestatus, der durch die obigen Metriken nicht abgedeckt ist              |Ja   | Nein  |
| EgressTotal | Datenübertragung in ausgehender Richtung in GB              |Ja   |Ja   |
| EgressHttpStatus2xx | Datenübertragung in ausgehender Richtung* für Antworten mit dem HTTP-Statuscode 2xx in GB            |Ja   |Nein   |
| EgressHttpStatus3xx | Datenübertragung in ausgehender Richtung für Antworten mit dem HTTP-Statuscode 3xx in GB              |Ja   |Nein   |
| EgressHttpStatus4xx | Datenübertragung in ausgehender Richtung für Antworten mit dem HTTP-Statuscode 4xx in GB               |Ja   | Nein  |
| EgressHttpStatus5xx | Datenübertragung in ausgehender Richtung für Antworten mit dem HTTP-Statuscode 5xx in GB               |Ja   |  Nein |
| EgressHttpStatusOthers | Datenübertragung in ausgehender Richtung für Antworten mit anderen HTTP-Statuscodes in GB                |Ja   |Nein   |
| EgressCacheHit |  Datenübertragung in ausgehender Richtung für Antworten, die direkt aus dem CDN-Cache auf den CDN-POPs/-Edges bereitgestellt wurden    |Ja   |  Nein |
| EgressCacheMiss | Datenübertragung in ausgehender Richtung für Antworten, die nicht auf dem nächstgelegenen POP-Server gefunden und daher vom Ursprungsserver abgerufen wurden              |Ja   |  Nein |
| EgressCacheNoCache | Datenübertragung in ausgehender Richtung für Objekte, die aufgrund einer Benutzerkonfiguration im Edgebereich nicht zwischengespeichert wurden                |Ja   |Nein   |
| EgressCacheUncacheable | Datenübertragung in ausgehender Richtung für Objekte, für die durch den Header „Cache-Control“ bzw. „Expires“ das Zwischenspeichern verhindert wird. Mit diesen Headern wird angegeben, dass keine Zwischenspeicherung auf einem POP oder durch den HTTP-Client erfolgen soll.                    |Ja   | Nein  |
| EgressCacheOthers |  Datenübertragungen in ausgehender Richtung für andere Cacheszenarien             |Ja   | Nein  |

*Datenübertragung in ausgehender Richtung bezieht sich hier auf Datenverkehr, der von CDN-POP-Servern auf dem Client bereitgestellt wird.


### <a name="schema-of-the-core-analytics-logs"></a>Schema der Basisanalyseprotokolle 

Alle Protokolle werden im JSON-Format gespeichert, und jeder Eintrag enthält Zeichenfolgenfelder gemäß dem folgenden Schema:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Hierbei steht „time“ für die Startzeit der Stundengrenze, für die die Statistik gemeldet wird. Beachten Sie Folgendes: Wenn eine Metrik von einem CDN-Anbieter nicht unterstützt wird, ist anstelle eines double- oder integer-Werts ein NULL-Wert vorhanden. Mit diesem NULL-Wert wird das Fehlen einer Metrik angegeben. Dies ist ein anderer Wert als der Wert 0. Beachten Sie außerdem, dass auf dem Endpunkt pro Domäne ein Satz mit diesen Metriken konfiguriert ist.

Hier sind Beispiele für Eigenschaften angegeben:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```
## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure-Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Analysieren von Azure CDN-Verwendungsmustern](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)


