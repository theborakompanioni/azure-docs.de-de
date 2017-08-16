---
title: "Protokollanalyse für Azure CDN | Microsoft-Dokumentation"
description: "Kunden können die Protokollanalyse für Azure CDN aktivieren."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 03ff74ae4e40d3f2279caaf4f73e9b4aac6a2ebb
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---

# <a name="diagnostics-logs-for-azure-cdn"></a>Diagnoseprotokolle für Azure CDN

Nachdem Sie CDN für Ihre Anwendung aktiviert haben, möchten Sie wahrscheinlich die CDN-Nutzung überwachen, die Integrität der Bereitstellung überprüfen und potenzielle Probleme behandeln. Azure CDN stellt diese Funktionen per [CDN-Basisanalyse](cdn-analyze-usage-patterns.md) und über [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bereit.

## <a name="cdn-core-analytics"></a>CDN-Basisanalyse
Als aktueller Azure CDN-Benutzer mit Verizon-Standard- oder Premium-Profil können Sie die Basisanalyse bereits im zusätzlichen Portal anzeigen, indem Sie im Azure-Portal die Option „Verwalten“ verwenden. 

## <a name="azure-diagnostic-logs"></a>Azure-Diagnoseprotokolle

Mit diesem neuen Feature können Sie jetzt Daten der Basisanalyse anzeigen und diese an mindestens einem Ziel speichern, z.B.:

 - Azure-Speicherkonto
 - Azure Event Hubs
 - [OMS Log Analytics-Repository](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 Dieses Feature ist für alle CDN-Endpunkte verfügbar, die zu CDN-Profilen von Verizon (Standard und Premium) und Akamai (Standard) gehören.

Mit Diagnoseprotokollen können Sie Metriken zur grundlegenden Nutzung von Ihrem CDN-Endpunkt auf verschiedene Quellkomponenten exportieren, um sie auf die gewünschte Weise zu verwenden. Sie können beispielsweise die folgenden Arten von Datenexport durchführen:

- Exportieren von Daten in Blobspeicher, Exportieren in eine CSV-Datei und Generieren von Diagrammen in Excel.
- Exportieren von Daten auf Event Hubs und Korrelieren mit Daten von anderen Azure-Diensten
- Exportieren von Daten in Log Analytics und Anzeigen von Daten in Ihrem eigenen OMS-Arbeitsbereich

In der folgenden Abbildung ist eine typische Datenansicht der CDN-Basisanalyse dargestellt.

![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Abbildung 1: Ansicht der CDN-Basisanalyse*

In der unten angegebenen exemplarischen Vorgehensweise werden das Schema der Basisanalysedaten, die Schritte zum Aktivieren des Features und Bereitstellen an unterschiedlichen Zielen und die Nutzung von Daten über diese Ziele beschrieben.

## <a name="enable-logging-with-azure-portal"></a>Aktivieren der Protokollierung über das Azure-Portal

> [!NOTE]
> Die Diagnoseprotokolle sind standardmäßig **deaktiviert**. 

Führen Sie die folgenden Schritte aus, um die Protokollierung mit der CDN-Basisanalyse zu aktivieren:

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an. Wenn Sie CDN für Ihren Workflow nicht bereits aktiviert haben, sollten Sie [Azure CDN vor dem Fortfahren aktivieren](cdn-create-new-endpoint.md).

1. Navigieren Sie im Portal zum **CDN-Profil**.
2. Wählen Sie ein CDN-Profil und dann den CDN-Endpunkt aus, für den Sie die **Diagnoseprotokolle** aktivieren möchten.

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Navigieren Sie zum Blatt **Diagnoseprotokolle**, und ändern Sie den Status im Abschnitt **Überwachung** in **Ein**.

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Aktivieren der Protokollierung mit Azure Storage
    
Wählen Sie zum Speichern der Protokolle mit Azure Storage die Option **In ein Speicherkonto archivieren**, wählen Sie die Tage für die Aufbewahrungsdauer aus, und klicken Sie unter **Protokoll** auf **CoreAnalytics**.

![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*Abbildung 2: Protokollierung mit Azure Storage*

### <a name="logging-with-oms-log-analytics"></a>Protokollierung mit OMS Log Analytics

Führen Sie diese Schritte aus, um OMS Log Analytics zum Speichern der Protokolle zu verwenden:

1. Wählen Sie auf dem Blatt **Diagnoseprotokolle** unter **Überwachung** die Option **An Log Analytics senden**. 

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Konfigurieren Sie die Log Analytics-Protokollierung, indem Sie auf „Konfigurieren“ klicken. Sie gelangen zu einem Dialogfeld, in dem Sie einen vorherigen Arbeitsbereich auswählen oder einen neuen erstellen können.

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Klicken Sie auf **Neuen Arbeitsbereich erstellen**.

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/07_Create-new.png)

4. Als Nächstes müssen Sie Folgendes auswählen: einen neuen Arbeitsbereichsnamen, ein vorhandenes Abonnement, eine Ressourcengruppe (neu oder vorhanden), einen Standort und einen Tarif. Sie können diese Konfiguration auch in Ihrem Dashboard anheften. Klicken Sie auf „OK“, um die Konfiguration abzuschließen.

    Als Nächstes sollte der Arbeitsbereich mit Ihrem OMS-Arbeitsbereich und Ressourcengruppennamen angezeigt werden. Die Namen müssen eindeutig sein und dürfen nur Buchstaben, Zahlen und Bindestriche enthalten. Leerzeichen und Unterstriche sind nicht zulässig. 

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. Als Nächstes erhalten Sie eine kurze Meldung mit dem Hinweis, dass Ihr Arbeitsbereich erstellt wurde, und das Konfigurationsfenster für die Protokollierung wird wieder angezeigt. Darin können Sie den Namen Ihres Log Analytics-Arbeitsbereichs überprüfen.

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    Nachdem Sie die Log Analytics-Konfiguration durchgeführt haben, sollten Sie auch das Feld „CoreAnalytics“ für die CDN-Protokollierung aktivieren.

6. Wenn alle Einstellungen korrekt sind, können Sie oben im Konfigurationsdialogfeld auf die Schaltfläche **Speichern** klicken.

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/10_Save-me.png)

    Die Schaltfläche **Speichern** ist nicht mehr aktiv, und die EIN/AUS-Schaltfläche ist jetzt auf „EIN“ festgelegt und nicht mehr violett, sondern blau.

7. Um Ihren neuen OMS-Arbeitsbereich anzuzeigen, navigieren Sie zu Ihrem Dashboard im Azure-Portal und klicken auf den Namen Ihres Log Analytics-Arbeitsbereichs. Als Nächstes wird Ihr Arbeitsbereich angezeigt. (Stellen Sie sicher, dass der OMS-Arbeitsbereich links hervorgehoben ist.) Klicken Sie auf die Kachel „OMS-Portal“, um Ihren Arbeitsbereich im OMS-Repository anzuzeigen. 

    ![Portal – Diagnoseprotokolle](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Ihr OMS-Repository ist jetzt zum Protokollieren von Daten bereit. Zum Nutzen dieser Daten müssen Sie eine [OMS-Lösung](#consuming-oms-log-analytics-data) verwenden, die später in diesem Artikel behandelt wird.

Weitere Informationen zu Verzögerungen bei der Datenprotokollierung finden Sie [hier](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Aktivieren der Protokollierung mit PowerShell

Unten ist ein Beispiel dazu angegeben, wie Sie Diagnoseprotokolle mit den Azure PowerShell-Cmdlets aktivieren und abrufen.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Aktivieren von Diagnoseprotokollen in einem Speicherkonto

Melden Sie sich zuerst an, und wählen Sie ein Abonnement aus:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


Verwenden Sie den folgenden Befehl, um Diagnoseprotokolle in einem Speicherkonto zu aktivieren:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
Verwenden Sie diesen Befehl, um Diagnoseprotokolle in einem OMS-Arbeitsbereich zu aktivieren:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Nutzen von Diagnoseprotokollen aus Azure Storage
In diesem Abschnitt werden das Schema der CDN-Basisanalyse und die Organisation innerhalb eines Azure Storage-Kontos beschrieben, und er enthält Beispielcode zum Herunterladen der Protokolle in eine CSV-Datei.

### <a name="using-microsoft-azure-storage-explorer"></a>Verwenden des Microsoft Azure-Speicher-Explorers
Bevor Sie im Azure Storage-Konto auf die Basisanalysedaten zugreifen können, benötigen Sie zuerst ein Tool zum Zugreifen auf den Inhalt in einem Speicherkonto. Auf dem Markt sind verschiedene Tools erhältlich, aber wir empfehlen Ihnen die Verwendung des Microsoft Azure-Speicher-Explorers. Sie können das Tool [hier](http://storageexplorer.com/) herunterladen. Konfigurieren Sie nach dem Herunterladen und Installieren der Software die Verwendung desselben Azure Storage-Kontos, das als Ziel für die CDN-Diagnoseprotokolle konfiguriert wurde.

1.  Öffnen Sie den **Microsoft Azure-Speicher-Explorer**.
2.  Ermitteln Sie das Speicherkonto.
3.  Navigieren Sie zum Knoten **Blobcontainer** unter diesem Speicherkonto, und erweitern Sie ihn.
4.  Wählen Sie den Container mit dem Namen **insights-logs-coreanalytics** aus, und doppelklicken Sie darauf.
5.  Im Bereich auf der rechten Seite werden Ergebnisse angezeigt. Es wird mit der ersten Ebene begonnen: **„resourceId=“**. Klicken Sie weiter, bis die Datei **PT1H.json** angezeigt wird. Der Pfad wird im folgenden Hinweis beschrieben.
6.  Jede Datei **PT1H.json** des Blobs steht für die Analyseprotokolle für eine Stunde eines bestimmten CDN-Endpunkts oder dessen benutzerdefinierter Domäne.
7.  Das Schema der Inhalte dieser JSON-Datei wird im Abschnitt „Schema der Basisanalyseprotokolle“ beschrieben.


> [!NOTE]
> **Format des Blobpfads**
> 
> Basisanalyseprotokolle werden einmal pro Stunde generiert. Alle Daten einer Stunde werden gesammelt und in einem zentralen Azure-Blob als JSON-Nutzlast gespeichert. Der Pfad zu diesem Azure-Blob wird so angezeigt, als ob eine hierarchische Struktur vorhanden wäre. Dies liegt daran, dass das Speicher-Explorer-Tool „/“ als Verzeichnistrennzeichen interpretiert und die Hierarchie zum besseren Verständnis anzeigt. Eigentlich steht der gesamte Pfad lediglich für den Namen des Blobs. Für den Namen des Blobs wird die folgende Benennungskonvention verwendet:   
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Beschreibung der Felder:**

|value|Beschreibung|
|-------|---------|
|Abonnement-ID    |ID des Azure-Abonnements. Hierfür wird das GUID-Format verwendet.|
|Ressource |Gruppenname: Name der Ressourcengruppe, der die CDN-Ressourcen angehören.|
|Profile Name (Profilname) |Name des CDN-Profils|
|Endpoint Name (Endpunktname) |Name des CDN-Endpunkts|
|Jahr|  Vierstellige Jahresangabe, z.B. 2017|
|Month (Monat)| Zweistellige Monatsangabe: 01 = Januar bis 12 = Dezember|
|Day (Tag)|   Zweistellige Tagesangabe|
|PT1H.json| JSON-Datei, in der die Analysedaten gespeichert sind|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Exportieren der Basisanalysedaten in eine CSV-Datei

Um den Zugriff auf die Basisanalyse zu vereinfachen, geben wir einen Beispielcode für ein Tool an, mit dem das Herunterladen der JSON-Dateien in einem flachen Dateiformat mit Kommas als Trennzeichen möglich ist. Diese Datei kann dann leicht zum Erstellen von Diagrammen oder Aggregationen verwendet werden.

Sie können das Tool wie folgt verwenden:

1.  Nutzen Sie den GitHub-Link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ).
2.  Laden Sie den Code herunter.
3.  Befolgen Sie die Anleitung zum Kompilieren und Konfigurieren.
4.  Führen Sie das Tool aus.
5.  In der sich ergebenden CSV-Datei werden die Analysedaten in einer einfachen flachen Hierarchie angezeigt.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Nutzen von Diagnoseprotokollen aus einem OMS Log Analytics-Repository
Log Analytics ist ein Dienst in Operations Management Suite (OMS), der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen. Er sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen. 

Für die Verwendung von Log Analytics müssen Sie die [Protokollierung im Azure OMS Log Analytics-Repository aktivieren](#enable-logging-with-azure-storage). Dies wurde weiter oben in diesem Artikel beschrieben.

### <a name="using-the-oms-repository"></a>Verwenden des OMS-Repositorys

 Im folgenden Diagramm ist die Architektur der Ein- und Ausgaben des Repositorys dargestellt:

![OMS Log Analytics-Repository](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Abbildung 3: Log Analytics-Repository*

Sie können die Daten mithilfe von Verwaltungslösungen auf unterschiedliche Weise anzeigen. Sie erhalten die Verwaltungslösungen über den [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Sie können Verwaltungslösungen aus Azure Marketplace installieren, indem Sie unten in jeder Lösung auf den Link **Jetzt herunterladen** klicken.

### <a name="adding-an-oms-cdn-management-solution"></a>Hinzufügen einer OMS-CDN-Verwaltungslösung

Führen Sie diese Schritte aus, um eine Verwaltungslösung hinzuzufügen:

1.   Melden Sie sich mit Ihrem Azure-Abonnement beim Azure-Portal an, sofern Sie noch nicht angemeldet sind, und navigieren Sie zu Ihrem Dashboard.
    ![Azure-Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. Wählen Sie unter **Marketplace** auf dem Blatt **Neu** die Option **Überwachung und Verwaltung**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. Klicken Sie auf dem Blatt **Überwachung und Verwaltung** auf **See all** (Alle anzeigen).

    ![Alle anzeigen](./media/cdn-diagnostics-log/15_See-all.png)

4.  Suchen Sie im Suchfeld nach „CDN“.

    ![Alle anzeigen](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Wählen Sie **Azure CDN-Basisanalyse**. 

    ![Alle anzeigen](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  Nachdem Sie auf **Erstellen** geklickt haben, werden Sie aufgefordert, einen neuen OMS-Arbeitsbereich zu erstellen oder einen vorhandenen zu verwenden. 

    ![Alle anzeigen](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Wählen Sie den Arbeitsbereich aus, den Sie weiter oben erstellt haben. Anschließend müssen Sie ein Automation-Konto hinzufügen.

    ![Alle anzeigen](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Als Nächstes wird das Formular für das Automation-Konto angezeigt, das Sie ausfüllen müssen. 

    ![Alle anzeigen](./media/cdn-diagnostics-log/20_Automation.png)

9. Nachdem Sie das Automation-Konto erstellt haben, können Sie Ihre Lösung hinzufügen. Klicken Sie auf die Schaltfläche **Erstellen** .

    ![Alle anzeigen](./media/cdn-diagnostics-log/21_Ready.png)

10. Ihre Lösung wurde Ihrem Arbeitsbereich hinzugefügt. Wechseln Sie zurück in Ihr Dashboard im Azure-Portal.

    ![Alle anzeigen](./media/cdn-diagnostics-log/22_Dashboard.png)

    Klicken Sie auf den Log Analytics-Arbeitsbereich, den Sie erstellt haben, um darauf zuzugreifen. 

11. Klicken Sie auf die Kachel **OMS-Portal**, um Ihre neue Lösung im OMS-Portal anzuzeigen.

    ![Alle anzeigen](./media/cdn-diagnostics-log/23_workspace.png)

12. Ihr OMS-Portal sollte jetzt wie folgt aussehen:

    ![Alle anzeigen](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Klicken Sie jeweils auf eine der Kacheln, um verschiedene Ansichten Ihrer Daten anzuzeigen.

    ![Alle anzeigen](./media/cdn-diagnostics-log/25_Interior-view.png)

    Sie können nach links oder rechts scrollen, um weitere Kacheln anzuzeigen, die einzelne Ansichten der Daten darstellen. 

    Wenn Sie auf eine Kachel klicken, erhalten Sie jeweils weitere Details zu Ihren Daten.

     ![Alle anzeigen](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Angebote und Tarife

Angebote und Tarife für OMS-Verwaltungslösungen finden Sie [hier](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Anpassen von Ansichten

Sie können die Ansichten für Ihre Daten anpassen, in dem Sie den **Ansicht-Designer** verwenden. Navigieren Sie zu Ihrem OMS-Arbeitsbereich, und beginnen Sie mit dem Entwerfen, indem Sie auf die Kachel **Ansicht-Designer** klicken.

![Ansicht-Designer](./media/cdn-diagnostics-log/27_Designer.png)

Sie können die Diagrammtypen über den linken Bereich ziehen und ablegen und die Datendetails eingeben, die auf der linken Seite analysiert werden sollen.

![Ansicht-Designer](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Protokolldatenverzögerungen

Verizon-Protokolldatenverzögerungen | Akamai-Protokolldatenverzögerungen
--- | ---
Die Verizon-Protokolldaten werden mit einer Stunde Verzögerung bereitgestellt, und es dauert bis zu zwei Stunden, bis die Daten nach Abschluss der Verteilung auf die Endpunkte angezeigt werden. | Die Akamai-Protokolldaten werden mit einer Verzögerung von 24 Stunden bereitgestellt, und es dauert bis zu zwei Stunden bis zur Anzeige, wenn sie vor mehr als 24 Stunden erstellt wurden. Falls die Daten vor Kurzem erstellt wurden, kann es bis zu 25 Stunden dauern, bis die Protokolle angezeigt werden.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Diagnoseprotokolltypen für CDN-Basisanalyse

Wir bieten derzeit nur Basisanalyseprotokolle an, die Metriken mit HTTP-Antwortstatistiken und -Ausgangsstatistiken für die CDN-POPs/-Edges enthalten.

### <a name="core-analytics-metrics-details"></a>Details von Basisanalysemetriken
Die folgende Tabelle enthält eine Liste mit den Metriken, die in den Basisanalyseprotokollen verfügbar sind. Nicht alle Metriken sind für alle Anbieter verfügbar, aber die Unterschiede sind nur minimal. Außerdem ist in der folgenden Tabelle auch angegeben, ob eine Metrik für einen Anbieter verfügbar ist. Beachten Sie, dass die Metriken nur für die CDN-Endpunkte zur Verfügung stehen, für die Datenverkehr anfällt.


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
| EgressCacheHit |  Datenübertragung in ausgehender Richtung für Antworten, die direkt aus dem CDN-Cache auf den CDN-POPs/-Edges bereitgestellt wurden  |Ja   |  Nein |
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

Hierbei steht „time“ für die Startzeit der Stundengrenze, für die die Statistik gemeldet wird. Wenn eine Metrik von einem CDN-Anbieter nicht unterstützt wird, ist anstelle eines double- oder integer-Werts ein NULL-Wert vorhanden. Mit diesem NULL-Wert wird das Fehlen einer Metrik angegeben. Dies ist ein anderer Wert als der Wert 0. Beachten Sie außerdem, dass auf dem Endpunkt pro Domäne ein Satz mit diesen Metriken konfiguriert ist.

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
* [Azure OMS Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [Azure Log Analytics-REST-API](https://docs.microsoft.com/en-us/rest/api/loganalytics)








