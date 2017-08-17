---
title: 'Analyseplattformen: Vergleich von Apache Storm mit Stream Analytics | Microsoft Docs'
description: "Hier finden Sie anhand eines Vergleichs von Apache Storm mit Stream Analytics Hilfe beim Auswählen einer Cloudanalyseplattform. Lernen Sie die Funktionen und Unterschiede kennen."
keywords: Analyseplattform, Analyseplattformen, Cloudanalyseplattform, Storm-Vergleich
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: b9aac017-9866-4d0a-b98f-6f03881e9339
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/27/2017
ms.author: samacha
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 4c0c7c185943eb292d816e2047de930245a3e1e1
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a>Auswählen einer Stream Analytics-Plattform: Vergleich von Apache Storm und Azure Stream Analytics
Azure umfasst mehrere Lösungen zum Analysieren von Streamingdaten: [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) und [Apache Storm in Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/). Beide Analyseplattformen verfügen über die Vorteile einer PaaS-Lösung. Für die Plattformen gelten aber einige erhebliche Unterschiede bei den Funktionen sowie in Bezug auf die Konfiguration und Verwaltung. 

In diesem Artikel werden die Features nebeneinander gestellt und verglichen, damit Sie leichter zwischen Apache Storm und Azure Stream Analytics als Cloudanalyseplattform wählen können. 

## <a name="general-features"></a>Allgemeine Features

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm in HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Open Source?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nein. Azure Stream Analytics ist ein Microsoft-eigenes Angebot.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja. Apache Storm ist eine lizenzierte Apache-Technologie.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft-Support?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardwareanforderungen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Keine Azure Stream Analytics ist ein Azure-Dienst.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Keine Apache Storm ist ein Azure-Dienst.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Einheit der obersten Ebene</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Benutzer stellen Streamingaufträge bereit und überwachen sie.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer stellen einen ganzen Cluster bereit und überwachen diesen. Im Cluster können mehrere Storm-Aufträge sowie andere Workloads (inkl. Batch) gehostet werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Preise</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Wird nach verarbeiteter Datenmenge und Anzahl von erforderlichen Streamingeinheiten, die pro Ausführungsstunde des Auftrags erforderlich sind, abgerechnet. 
                </p>
                    <p>Weitere Informationen finden Sie unter <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics – Preise</a>.</p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Die Einheit des Kaufs basiert auf einem Cluster und wird in Abhängigkeit der Ausführungszeit des Clusters und unabhängig von den bereitgestellten Aufträgen in Rechnung gestellt.
                </p>
                <p>
Weitere Informationen finden Sie unter <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight – Preise</a>.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a>Erstellen

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm in HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Funktionen: SQL DSL?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja. In Stream Analytics wird eine SQL-ähnliche Sprache zum Erstellen von Transformationen bereitgestellt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nein. Benutzer schreiben Code in Java oder C# oder verwenden Trident-APIs.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Funktionen: Temporale Operatoren?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Fensteraggregate und temporale Joins werden standardmäßig unterstützt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Temporale Operatoren müssen vom Benutzer implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Entwicklungsumgebung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Benutzer können Aufträge über das Azure-Portal erstellen, debuggen und überwachen, indem sie Beispieldaten eines Livestreams verwenden.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer, die .NET nutzen, können die Entwicklung, das Debuggen und die Überwachung mit Visual Studio durchführen. Benutzer, die Java oder andere Sprachen verwenden, können die IDE ihrer Wahl einsetzen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Debugging-Unterstützung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Als Hilfe beim Debuggen stehen einfache Protokolle zum Auftragsstatus und zu den Auftragsvorgängen zur Verfügung. In Stream Analytics können Benutzer derzeit nicht angeben, welcher Inhalte oder wie viel Inhalt in den Protokollen enthalten ist (ausführlicher Modus).
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ausführliche Protokolle sind verfügbar. Benutzer können auf die Protokolle in Visual Studio zugreifen oder sich am Cluster anmelden und direkt auf die Protokolle zugreifen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Unterstützung für benutzerdefinierte Funktionen (UDFs)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Abfragen unterstützen benutzerdefinierte JavaScript-Funktionen. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzerdefinierte Funktionen (UDFs) können in C#, Java oder einer beliebigen anderen Sprache geschrieben werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Erweiterbarkeit durch benutzerdefinierten Code?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nein. Es gibt keine Unterstützung für erweiterbarem Code in Stream Analytics.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja. Benutzer können benutzerdefinierten Code in C#, Java oder einer beliebigen anderen Sprache schreiben, die in Storm unterstützt wird.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a>Datenquellen (Eingaben) und Ausgaben ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm in HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Datenquellen für die Eingabe</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Azure Event Hubs und Azure-Blobspeicher.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Connectors sind für Azure Event Hubs, Azure Service Bus, Kafka und mehr verfügbar. Benutzer können zusätzliche Connectors erstellen, indem sie benutzerdefinierten Code verwenden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Datenformate für die Eingabe</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Avro, JSON, CSV </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer können mit benutzerdefiniertem Code beliebige Formate implementieren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ausgaben</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ein Streamingauftrag kann über mehrere Ausgaben verfügen. Unterstützte Ausgaben sind Azure Event Hubs, Azure-Blobspeicher, Azure-Tabellenspeicher, Azure SQL-Datenbank und Power BI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Storm unterstützt in einer Topologie viele Ausgaben, und jede Ausgabe kann eine benutzerdefinierte Logik für die Downstreamverarbeitung aufweisen. Storm enthält Connectors für Power BI, Azure Event Hubs, Azure-Blobspeicher, Azure Cosmos DB, SQL und HBase. Benutzer können zusätzliche Connectors erstellen, indem sie benutzerdefinierten Code verwenden.    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Daencodierungsformate</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Daten müssen per UTF-8 formatiert werden.
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
Benutzer können ein beliebiges Datencodierungsformat implementieren, indem sie benutzerdefinierten Code verwenden.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a>Verwaltung und Abläufe ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm in HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Auftragsbereitstellungsmodell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure-Portal, PowerShell und REST-APIs.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Azure-Portal, PowerShell, Visual Studio und REST-APIs.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Überwachung (Statistiken, Indikatoren usw.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Die Überwachung wird über das Azure-Portal und REST-APIs implementiert. Benutzer haben auch die Möglichkeit, Azure-Warnungen zu konfigurieren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Die Überwachung wird über die Storm-Benutzeroberfläche und REST-APIs implementiert.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skalierbarkeit</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Die Skalierbarkeit wird durch die Anzahl von Streamingeinheiten (Streaming Units, SUs) eines Auftrags ermittelt. Jede Streamingeinheit verarbeitet bis zu 1 MB/Sekunde, und es können maximal 50 Einheiten genutzt werden. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">Skalieren von Azure Stream Analytics-Aufträgen zur Erhöhung des Durchsatzes bei der Streamingdatenverarbeitung</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Die Skalierbarkeit richtet sich nach der Anzahl von Knoten im HDInsight Storm-Cluster. Der obere Grenzwert für die Anzahl von Knoten wird durch das Azure-Kontingent des Benutzers bestimmt.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Datenverarbeitungslimits</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Benutzer können den Umfang der Datenverarbeitung steigern oder die Kosten optimieren, indem sie die Anzahl von Streamingeinheiten erhöhen oder verringern. Der obere Grenzwert beträgt hierbei 1 GB/Sekunde.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer können die Clustergröße nach oben oder unten skalieren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Anhalten und Fortsetzen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Anhalten und von letzter Stoppstelle aus fortsetzen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Anhalten und fortsetzen basierend auf einem Wasserzeichen von der letzten Stoppstelle aus.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Dienst- und Framework-Update</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatisches Patchen ohne Ausfallzeiten.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatisches Patchen ohne Ausfallzeiten.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Geschäftskontinuität durch einen hoch verfügbaren Dienst mit garantierten SLAs</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li>SLA mit 99,9% Verfügbarkeit</li>
                <li>Automatische Wiederherstellung nach Fehlern</li>
                <li>Integrierte Wiederherstellung von zustandsbehafteten temporalen Operatoren</li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
SLA mit 99,9% Verfügbarkeit des Storm-Clusters. 
                </p>
                <p>
Apache Storm ist eine fehlertolerante Streamingplattform. Aber der Benutzer ist dafür verantwortlich, dass die Streamingaufträge unterbrechungsfrei ausgeführt werden.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a>Erweiterte Funktionen ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm in HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Verspäteter Eingang und Störereignisbehandlung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Mit integrierten konfigurierbaren Richtlinien können Ereignisse neu angeordnet und verworfen werden, oder die Ereigniszeit kann angepasst werden.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer müssen die Logik zum Behandeln dieses Szenarios implementieren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Referenzdaten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Die Referenzdaten sind im Azure-Blobspeicher mit einem maximalen In-Memory-Cache von 100 MB verfügbar. Referenzdaten werden vom Dienst aktualisiert.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Keine Beschränkungen für die Datengröße. Connectors sind für HBase, Azure Cosmos DB, SQL Server und Azure verfügbar. Benutzer können zusätzliche Connectors erstellen, indem sie benutzerdefinierten Code verwenden. Die Referenzdaten müssen mit benutzerdefiniertem Code aktualisiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integration mit Machine Learning</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Veröffentlichte Azure Machine Learning-Modelle können bei der Auftragserstellung als Funktionen konfiguriert werden. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">Skalieren eines Stream Analytics-Auftrags mit Azure Machine Learning-Funktionen</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Über Storm Bolts verfügbar.
                </p>
            </td>
        </tr>
    </tbody>
</table>

