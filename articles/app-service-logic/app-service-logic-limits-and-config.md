---
title: Logik-App-Grenzwerte und -Konfiguration | Microsoft-Dokumentation
description: "Enthält eine Übersicht über die Dienstgrenzwerte und die Konfigurationswerte, die für Logik-Apps verfügbar sind."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: dwrede
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 47c13d5cbaeb114da18b710c52e2c989df4c0e01
ms.openlocfilehash: 2417d4e3d8033ff8d66ace8b27f398266a7fd101


---
# <a name="logic-app-limits-and-configuration"></a>Logik-App-Grenzwerte und -Konfiguration

Unten sind die aktuellen Grenzwerte und Konfigurationsdetails für Azure Logic Apps angegeben.

## <a name="limits"></a>Grenzen

### <a name="http-request-limits"></a>Grenzwerte für HTTP-Anforderungen

Dies sind die Grenzwerte für eine einzelne HTTP-Anforderung und/oder einen Connectoraufruf.

#### <a name="timeout"></a>Timeout

|Name|Begrenzung|Hinweise|
|----|----|----|
|Anforderungstimeout|90 Sekunden|Bei Bedarf können ein [asynchrones Muster](app-service-logic-create-api-app.md) oder eine [Until-Schleife](app-service-logic-loops-and-scopes.md) zur Kompensation verwendet werden.|

#### <a name="message-size"></a>Nachrichtengröße

|Name|Begrenzung|Hinweise|
|----|----|----|
|Nachrichtengröße|50 MB|Einige Connectors und APIs unterstützen unter Umständen nicht 50 MB. |
|Grenzwert für die Auswertung von Ausdrücken|131.072 Zeichen|`@concat()`, `@base64()`, `string` dürfen nicht länger sein.|

#### <a name="retry-policy"></a>Wiederholungsrichtlinie

|Name|Begrenzung|Hinweise|
|----|----|----|
|Wiederholungsversuche|4|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Maximale Wiederholungsverzögerung|1 Stunde|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Minimale Wiederholungsverzögerung|20 Sekunden|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

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
|ForEach-Elemente|5.000|Bei Bedarf können Sie die [Abfrageaktion](../connectors/connectors-native-query.md) verwenden, um größere Arrays zu filtern.|
|Until-Iterationen|5.000||
|SplitOn-Elemente|5.000||
|ForEach-Parallelität|20|Sie können ein sequenzielles ForEach-Element festlegen, indem Sie `"operationOptions": "Sequential"` der `foreach`-Aktion hinzufügen.|


### <a name="throughput-limits"></a>Durchsatzlimits

Dies sind die Grenzwerte für eine einzelne Instanz der Logik-App. 

|Name|Begrenzung|Hinweise|
|----|----|----|
|Trigger pro Sekunde|100|Bei Bedarf können Workflows auf mehrere Apps verteilt werden.|

### <a name="definition-limits"></a>Definitionslimits

Dies sind die Grenzwerte für eine einzelne Logik-App-Definition.

|Name|Begrenzung|Hinweise|
|----|----|----|
|Aktionen pro Workflow|250|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Zulässige Schachtelungstiefe für Aktion|5|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Flüsse pro Region und Abonnement|1000||
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
|Schema|8 MB|Sie können den [Blob-URI](app-service-logic-enterprise-integration-schemas.md) zum Hochladen von Dateien verwenden, die größer als 2 MB sind. |
|Zuordnung (XSLT-Datei)|2 MB| |

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Nachrichtengröße für B2B-Protokolle (AS2, X12, EDIFACT)

Hier sehen Sie die Grenzwerte für B2B-Protokolle:

|Name|Begrenzung|Hinweise|
|----|----|----|
|AS2|50 MB|Gilt für Decodieren und Codieren.|
|X12|50 MB|Gilt für Decodieren und Codieren.|
|EDIFACT|50 MB|Gilt für Decodieren und Codieren.|

## <a name="configuration"></a>Konfiguration

### <a name="ip-address"></a>IP-Adresse

Aufrufe, die über einen [Connector](../connectors/apis-list.md) durchgeführt werden, stammen von der unten angegebenen IP-Adresse.

Aufrufe, die direkt über eine Logik-App durchgeführt werden (also per [HTTP](../connectors/connectors-native-http.md) oder [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)), können aus jedem [IP-Bereich für ein Azure-Datencenter](https://www.microsoft.com/en-us/download/details.aspx?id=41653) stammen.

|Logik-App-Region|Ausgehende IP-Adresse|
|-----|----|
|Australien (Osten)|40.126.251.213|
|Australien (Südosten)|40.127.80.34|
|Brasilien Süd|191.232.38.129|
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

- Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App](app-service-logic-create-a-logic-app.md) .  
- [Anzeigen allgemeiner Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)




<!--HONumber=Nov16_HO3-->


