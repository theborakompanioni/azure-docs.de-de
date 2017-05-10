---
title: Logik-App-Grenzwerte und -Konfiguration | Microsoft-Dokumentation
description: "Enthält eine Übersicht über die Dienstgrenzwerte und die Konfigurationswerte, die für Logik-Apps verfügbar sind."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: jehollan
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 2a270ba8ae17077c55c6b1473d4955dfb5f79ca1
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="logic-app-limits-and-configuration"></a>Logik-App-Grenzwerte und -Konfiguration

Unten sind die aktuellen Grenzwerte und Konfigurationsdetails für Azure Logic Apps angegeben.

## <a name="limits"></a>Grenzen

### <a name="http-request-limits"></a>Grenzwerte für HTTP-Anforderungen

Dies sind die Grenzwerte für eine einzelne HTTP-Anforderung und/oder einen Connectoraufruf.

#### <a name="timeout"></a>Timeout

|Name|Begrenzung|Hinweise|
|----|----|----|
|Anforderungstimeout|120 Sekunden|Bei Bedarf können ein [asynchrones Muster](../logic-apps/logic-apps-create-api-app.md) oder eine [Until-Schleife](logic-apps-loops-and-scopes.md) zur Kompensation verwendet werden.|

#### <a name="message-size"></a>Nachrichtengröße

|Name|Begrenzung|Hinweise|
|----|----|----|
|Nachrichtengröße|100 MB|Einige Connectors und APIs unterstützen unter Umständen nicht 100 MB. |
|Grenzwert für die Auswertung von Ausdrücken|131.072 Zeichen|`@concat()`, `@base64()`, `string` dürfen nicht länger sein.|

#### <a name="retry-policy"></a>Wiederholungsrichtlinie

|Name|Begrenzung|Hinweise|
|----|----|----|
|Wiederholungsversuche|4|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Maximale Wiederholungsverzögerung|1 Stunde|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Minimale Wiederholungsverzögerung|5 Sekunden|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Ausführungsdauer und Aufbewahrung

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App.

|Name|Begrenzung|Hinweise|
|----|----|----|
|Ausführungsdauer|90 Tage||
|Aufbewahrungsdauer im Speicher|90 Tage|Gilt ab der Startzeit der Ausführung.|
|Min. Wiederholungsintervall|1 Sekunde|| 15 Sekunden für Logik-Apps mit App Service-Plan
|Max. Wiederholungsintervall|500 Tage||


### <a name="looping-and-debatching-limits"></a>Grenzwerte für Schleifen und Auflösen von Batches

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App.

|Name|Begrenzung|Hinweise|
|----|----|----|
|ForEach-Elemente|100.000|Bei Bedarf können Sie die [Abfrageaktion](../connectors/connectors-native-query.md) verwenden, um größere Arrays zu filtern.|
|Until-Iterationen|5.000||
|SplitOn-Elemente|100.000||
|ForEach-Parallelität|20|Sie können ein sequenzielles ForEach-Element festlegen, indem Sie `"operationOptions": "Sequential"` der `foreach`-Aktion hinzufügen.|


### <a name="throughput-limits"></a>Durchsatzlimits

Dies sind die Grenzwerte für eine einzelne Instanz der Logik-App. 

|Name|Begrenzung|Hinweise|
|----|----|----|
|Aktionsausführungen pro 5 Minuten |100.000|Bei Bedarf können Workflows auf mehrere Apps verteilt werden.|
|Aufrufe zum Lesen von Endpunkten zur Laufzeit pro 5 Minuten |60.000|Bei Bedarf können Workflows auf mehrere Apps verteilt werden.|
|Aufrufe zum Aufrufen von Endpunkten zur Laufzeit pro 5 Minuten |45.000|Bei Bedarf können Workflows auf mehrere Apps verteilt werden.|
|Gleichzeitige Aufrufe zum Blockieren von Endpunkten zur Laufzeit |ca. 1.000|Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer|

Wenn Sie davon ausgehen, dass dieses Limit bei der normalen Verarbeitung überschritten wird, oder Auslastungstests ausführen möchten, die dieses Limit für eine bestimmte Zeit überschreiten können, [wenden Sie sich an uns](mailto://logicappsemail@microsoft.com), damit wir Ihnen bei diesen Anforderungen helfen können.

### <a name="definition-limits"></a>Definitionslimits

Dies sind die Grenzwerte für eine einzelne Logik-App-Definition.

|Name|Begrenzung|Hinweise|
|----|----|----|
|Aktionen pro Workflow|250|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Zulässige Schachtelungstiefe für Aktion|5|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Workflows pro Region und Abonnement|1000||
|Trigger pro Workflow|10||
|Maximale Anzahl von Zeichen pro Ausdruck|8.192||
|Max. `trackedProperties`-Größe in Zeichen|16.000|
|`action`/`trigger` -Namensbeschränkung|80||
|`description` |256||
|`parameters` limit|50||
|`outputs` limit|10||

### <a name="integration-account-limits"></a>Grenzwerte für Integrationskonten

Hier sehen Sie die Grenzwerte für Artefakte, die dem Integrationskonto hinzugefügt werden:

|Name|Begrenzung|Hinweise|
|----|----|----|
|Schema|8 MB|Sie können den [Blob-URI](logic-apps-enterprise-integration-schemas.md) zum Hochladen von Dateien verwenden, die größer als 2 MB sind. |
|Zuordnung (XSLT-Datei)|2 MB| |
|Aufrufe zum Lesen von Endpunkten zur Laufzeit pro 5 Minuten |60.000|Bei Bedarf können Workflows auf mehrere Konten verteilt werden.|
|Aufrufe zum Aufrufen von Endpunkten zur Laufzeit pro 5 Minuten |90.000|Bei Bedarf können Workflows auf mehrere Konten verteilt werden.|
|Gleichzeitige Aufrufe zum Blockieren von Endpunkten zur Laufzeit |ca. 1.000|Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer|

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Nachrichtengröße für B2B-Protokolle (AS2, X12, EDIFACT)

Hier sehen Sie die Grenzwerte für B2B-Protokolle:

|Name|Begrenzung|Hinweise|
|----|----|----|
|AS2|50 MB|Gilt für Decodieren und Codieren.|
|X12|50 MB|Gilt für Decodieren und Codieren.|
|EDIFACT|50 MB|Gilt für Decodieren und Codieren.|

## <a name="configuration"></a>Konfiguration

### <a name="ip-address"></a>IP-Adresse

#### <a name="logic-app-service"></a>Logik-App-Dienst

Aufrufe, die direkt aus einer Logik-App erfolgen (d.h. über [HTTP](../connectors/connectors-native-http.md) oder [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)), oder andere HTTP-Anforderungen kommen von der unten angegebenen IP-Adresse:

|Logik-App-Region|Ausgehende IP-Adresse|
|-----|----|
|Australien (Osten)|13.75.153.66, 104.210.89.222, 104.210.89.244, 13.75.149.4, 104.210.91.55, 104.210.90.241|
|Australien (Südosten)|13.73.115.153, 40.115.78.70, 40.115.78.237, 13.73.114.207, 13.77.3.139, 13.70.159.205|
|Brasilien Süd|191.235.86.199, 191.235.95.229, 191.235.94.220, 191.235.82.221, 191.235.91.7, 191.234.182.26|
|Kanada, Mitte|52.233.29.92,52.228.39.241,52.228.39.244|
|Kanada, Osten|52.232.128.155,52.229.120.45,52.229.126.25|
|Indien (Mitte)|52.172.157.194, 52.172.184.192, 52.172.191.194, 52.172.154.168, 52.172.186.159, 52.172.185.79|
|USA (Mitte)|13.67.236.76, 40.77.111.254, 40.77.31.87, 13.67.236.125, 104.208.25.27, 40.122.170.198|
|Asien (Osten)|168.63.200.173, 13.75.89.159, 23.97.68.172, 13.75.94.173, 40.83.127.19, 52.175.33.254|
|USA (Ost)|137.135.106.54, 40.117.99.79, 40.117.100.228, 13.92.98.111, 40.121.91.41, 40.114.82.191|
|USA (Ost) 2|40.84.25.234, 40.79.44.7, 40.84.59.136, 40.84.30.147, 104.208.155.200, 104.208.158.174|
|Japan Ost|13.71.146.140, 13.78.84.187, 13.78.62.130, 13.71.158.3, 13.73.4.207, 13.71.158.120|
|Japan (Westen)|40.74.140.173, 40.74.81.13, 40.74.85.215, 40.74.140.4, 104.214.137.243, 138.91.26.45|
|USA Nord Mitte|168.62.249.81, 157.56.12.202, 65.52.211.164, 168.62.248.37, 157.55.210.61, 157.55.212.238|
|Nordeuropa|13.79.173.49, 52.169.218.253, 52.169.220.174, 40.113.12.95, 52.178.165.215, 52.178.166.21|
|USA (Mitte/Süden)|13.65.98.39, 13.84.41.46, 13.84.43.45, 104.210.144.48, 13.65.82.17, 13.66.52.232|
|Südostasien|52.163.93.214, 52.187.65.81, 52.187.65.155, 13.76.133.155, 52.163.228.93, 52.163.230.166|
|Indien, Süden|52.172.9.47, 52.172.49.43, 52.172.51.140, 52.172.50.24, 52.172.55.231, 52.172.52.0|
|Westeuropa|13.95.155.53, 52.174.54.218, 52.174.49.6, 40.68.222.65, 40.68.209.23, 13.95.147.65|
|Indien, Westen|104.211.164.112, 104.211.165.81, 104.211.164.25, 104.211.164.80, 104.211.162.205, 104.211.164.136|
|USA (West)|52.160.90.237, 138.91.188.137, 13.91.252.184, 52.160.92.112, 40.118.244.241, 40.118.241.243|

#### <a name="connectors"></a>Connectors

Aufrufe, die über einen [Connector](../connectors/apis-list.md) durchgeführt werden, kommen von der unten angegebenen IP-Adresse:

|Logik-App-Region|Ausgehende IP-Adresse|
|-----|----|
|Australien (Osten)|40.126.251.213|
|Australien (Südosten)|40.127.80.34|
|Brasilien Süd|191.232.38.129|
|Kanada, Mitte|52.233.31.197,52.228.42.205,52.228.33.76,52.228.34.13|
|Kanada, Osten|52.229.123.98,52.229.120.178,52.229.126.202,52.229.120.52|
|Indien (Mitte)|104.211.98.164|
|USA (Mitte)|40.122.49.51|
|Ostasien|23.99.116.181|
|USA (Ost)|191.237.41.52|
|USA (Ost) 2|104.208.233.100|
|Japan Ost|40.115.186.96|
|Japan (Westen)|40.74.130.77|
|USA (Mitte/Norden)|65.52.218.230|
|Nordeuropa|104.45.93.9|
|USA (Mitte/Süden)|104.214.70.191|
|Südostasien|13.76.231.68|
|Indien, Süden|104.211.227.225|
|Westeuropa|40.115.50.13|
|Indien, Westen|104.211.161.203|
|USA (West)|104.40.51.248|


## <a name="next-steps"></a>Nächste Schritte  

- Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) .  
- [Anzeigen allgemeiner Beispiele und Szenarien](../logic-apps/logic-apps-examples-and-scenarios.md)
- [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)

