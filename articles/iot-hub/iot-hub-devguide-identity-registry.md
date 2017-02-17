---
title: "Informationen zur Azure IoT Hub-Identitätsregistrierung | Microsoft Docs"
description: "Entwicklerhandbuch: Beschreibung der Identitätsregistrierung von IoT Hub und Anleitung zum Verwalten von Geräten mithilfe der Identitätsregistrierung. Enthält Informationen zum Importieren und Exportieren von Geräteidentitäten in einem Massenvorgang."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 9ded95283b52f0fc21ca5b99df8e72e1e152fe1c
ms.openlocfilehash: 08ebf8f6f5a2adab90c7de1084bfb5e4aed77fd3


---
# <a name="identity-registry"></a>Identitätsregistrierung
## <a name="overview"></a>Übersicht
Jeder IoT-Hub verfügt über eine Identitätsregistrierung, in der Informationen zu den Geräten gespeichert werden, die eine Verbindung mit dem IoT-Hub herstellen dürfen. Damit ein Gerät eine Verbindung mit einem IoT-Hub herstellen kann, muss in der Identitätsregistrierung des IoT-Hubs ein Eintrag für dieses Gerät vorhanden sein. Ein Gerät muss sich beim IoT-Hub zudem mit Anmeldeinformationen authentifizieren, die in der Identitätsregistrierung gespeichert sind.

Ganz allgemein handelt es sich bei der Identitätsregistrierung um eine REST-fähige Sammlung von Identitätsressourcen. Wenn Sie der Identitätsregistrierung einen Eintrag hinzufügen, erstellt IoT Hub im Dienst eine Gruppe gerätebezogener Ressourcen, wie z.B. die Warteschlange laufender Cloud-zu-Gerät-Nachrichten.

### <a name="when-to-use"></a>Einsatzgebiete
Verwenden Sie die Identitätsregistrierung, wenn Sie Geräte bereitstellen möchten, die eine Verbindung mit Ihrem IoT-Hub herstellen, und wenn Sie den gerätespezifischen Zugriff auf die geräteseitigen Endpunkte Ihres Hubs steuern möchten.

> [!NOTE]
> Die Identitätsregistrierung enthält keine anwendungsspezifischen Metadaten.
> 
> 

## <a name="identity-registry-operations"></a>Identitätsregistrierungsvorgänge
Die IoT Hub-Identitätsregistrierung ermöglicht folgende Vorgänge:

* Erstellen einer Geräteidentität
* Aktualisieren einer Geräteidentität
* Abrufen von Geräteidentitäten nach ID
* Löschen einer Geräteidentität
* Auflisten von bis zu 1.000 Identitäten
* Exportieren aller Identitäten in Azure-Blobspeicher
* Importieren von Identitäten aus Azure-Blobspeicher

Alle diese Vorgänge erlauben die Verwendung von optimistischer Nebenläufigkeit gemäß [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> Die einzige Möglichkeit zum Abrufen aller Identitäten in der Identitätsregistrierung eines Hubs besteht in der Verwendung der Funktion [Export][lnk-export].
> 
> 

Für IoT Hub-Identitätsregistrierungen gilt Folgendes:

* Enthält keinerlei Anwendungsmetadaten.
* Der Zugriff kann wie bei einem Wörterbuch über die **deviceId** als Schlüssel erfolgen.
* Ausdrucksbasierte Abfragen werden nicht unterstützt.

Eine IoT-Lösung verfügt in der Regel über einen lösungsspezifischen Speicher mit anwendungsspezifischen Metadaten. Der lösungsspezifische Speicher in einer Lösung für intelligente Gebäude würde beispielsweise den Raum enthalten, in dem ein Temperatursensor bereitgestellt wird.

> [!IMPORTANT]
> Verwenden Sie die Identitätsregistrierung nur für die Geräteverwaltung und -bereitstellung. Vorgänge mit hohem Durchsatz zur Laufzeit dürfen nicht von Vorgängen in der Identitätsregistrierung abhängen. Das Überprüfen des Verbindungsstatus eines Geräts vor dem Senden eines Befehls ist z. B. kein unterstütztes Muster. Überprüfen Sie die [Drosselungsraten][lnk-quotas] für die Identitätsregistrierung und das Muster des [Gerätetakts][lnk-guidance-heartbeat].
> 
> 

## <a name="disable-devices"></a>Deaktivieren von Geräten
Sie können Geräte deaktivieren, indem Sie die **status**-Eigenschaft für eine Identität in der Identitätsregistrierung aktualisieren. Typischerweise wird diese Eigenschaft in zwei Szenarien verwendet:

* Während eines Vorgangs zur Bereitstellungsorchestrierung. Weitere Informationen finden Sie unter [Gerätebereitstellung][lnk-guidance-provisioning].
* Wenn aus einem beliebigen Grund die Sicherheit eines Geräts als gefährdet eingestuft oder das Gerät nicht mehr als autorisiert betrachtet wird.

## <a name="import-and-export-device-identities"></a>Importieren und Exportieren von Geräteidentitäten
Sie können einen Massenexport von Geräteidentitäten aus der Identitätsregistrierung eines IoT-Hubs durchführen. Hierbei werden asynchrone Vorgänge im [Endpunkt des IoT Hub-Ressourcenanbieters][lnk-endpoints] verwendet. Exportvorgänge sind Aufträge mit langer Ausführungszeit, die einen vom Kunden bereitgestellten Blobcontainer zum Speichern von Geräteidentitätsdaten verwenden, die aus der Identitätsregistrierung gelesen werden.

Sie können einen Massenimport von Geräteidentitäten in die Identitätsregistrierung eines IoT-Hubs durchführen. Hierbei werden asynchrone Vorgänge im [Endpunkt des IoT Hub-Ressourcenanbieters][lnk-endpoints] verwendet. Importvorgänge sind Aufträge mit langer Ausführungszeit, die Daten in einem vom Kunden bereitgestellten Blobcontainer verwenden, um Geräteidentitätsdaten in die Identitätsregistrierung zu schreiben.

* Ausführliche Informationen zu den Import- und Export-APIs finden Sie unter den [REST-APIs für den IoT Hub-Ressourcenanbieter][lnk-resource-provider-apis].
* Weitere Informationen zum Ausführen von Import- und Exportaufträgen finden Sie unter [Massenverwaltung von IoT Hub-Geräte-Identitäten][lnk-bulk-identity].

## <a name="device-provisioning"></a>Gerätebereitstellung
Die Gerätedaten, die von einer bestimmten IoT-Lösung gespeichert werden, richten sich nach den jeweiligen Anforderungen der Lösung. Von einer Lösung müssen aber mindestens die Geräteidentitäten und Authentifizierungsschlüssel gespeichert werden. Azure IoT Hub enthält eine Identitätsregistrierung, die Werte für jedes Gerät speichern kann, z.B. IDs, Authentifizierungsschlüssel und Statuscodes. Eine Lösung kann andere Azure-Dienste wie Azure Table Storage, Azure Blob Storage oder Azure DocumentDB nutzen, um zusätzliche Gerätedaten zu speichern.

*Gerätebereitstellung* ist der Prozess des Hinzufügens der ersten Gerätedaten zu den Speichern in Ihrer Lösung. Damit ein neues Gerät eine Verbindung mit Ihrem Hub herstellen kann, müssen Sie der IoT Hub-Identitätsregistrierung eine neue Geräte-ID und Schlüssel hinzufügen. Im Rahmen des Bereitstellungsprozesses müssen Sie unter Umständen gerätespezifische Daten in anderen Lösungsspeichern initialisieren.

## <a name="device-heartbeat"></a>Gerätetakt
Die IoT Hub-Identitätsregistrierung enthält das Feld **connectionState**. Verwenden Sie das Feld **connectionState** nur bei der Entwicklung und beim Debuggen. IoT-Lösungen sollten das Feld zur Laufzeit nicht abfragen (um beispielsweise eine Geräteverbindung zu prüfen und dann zu bestimmen, ob eine C2D-Nachricht oder eine SMS gesendet werden soll).

Wenn Ihre IoT-Lösung wissen muss, ob ein Gerät verbunden ist (entweder zur Laufzeit oder mit höherer Genauigkeit als von der **connectionState**-Eigenschaft bereitgestellt), sollten Sie das *Taktmuster* implementieren.

Beim Taktmuster sendet das Gerät D2C-Nachrichten mindestens einmal pro festgelegtem Zeitraum (z. B. mindestens einmal pro Stunde). Selbst wenn ein Gerät keine zu sendenden Daten hat, sendet es daher trotzdem eine leere D2C-Nachricht (in der Regel mit einer Eigenschaft, die sie als Takt identifiziert). Auf der Dienstseite verwaltet die Lösung eine Karte mit dem letzten für jedes Gerät empfangenen Takt. Die Lösung geht davon aus, dass ein Problem mit einem Gerät vorliegt, wenn innerhalb der erwarteten Zeit keine Taktnachricht empfangen wird.

Eine komplexere Implementierung kann die Informationen aus der [Vorgangsüberwachung][lnk-devguide-opmon] enthalten, um Geräte zu ermitteln, die erfolglos versuchen, eine Verbindung herzustellen oder zu kommunizieren. Wenn Sie das Taktmuster implementieren, informieren Sie sich über [IoT Hub-Kontingente und -Drosselungen][lnk-quotas].

> [!NOTE]
> Wenn für eine IoT-Lösung der Geräteverbindungsstatus ausschließlich dazu benötigt wird, um zu bestimmen, ob C2D-Nachrichten gesendet werden müssen, und wenn Nachrichten nicht an große Gruppen von Geräten übertragen werden, sollten Sie in Betracht ziehen, eine kurze Ablaufzeit zu verwenden. Dieses Muster erzielt dasselbe Ergebnis wie beim Aufrechterhalten der Registrierung des Geräteverbindungsstatus mit dem Taktmuster, dies jedoch effizienter. Es ist auch durch die Anforderung von Nachrichtenbestätigungen möglich, vom IoT Hub darüber benachrichtigt zu werden, von welchen Geräten Nachrichten empfangen werden können und welche nicht online oder ausgefallen sind.
> 
> 

## <a name="reference-topics"></a>Referenzthemen:
Die folgenden Referenzthemen enthalten weitere Informationen zur Verwendung der Identitätsregistrierung.

## <a name="device-identity-properties"></a>Geräteidentitätseigenschaften
Geräteidentitäten werden als JSON-Dokumente mit den folgenden Eigenschaften dargestellt:

| Eigenschaft | Options | Beschreibung |
| --- | --- | --- |
| deviceId |erforderlich, bei Aktualisierungen schreibgeschützt |Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`besteht. |
| generationId |erforderlich, schreibgeschützt |Eine vom IoT-Hub generierte Zeichenfolge mit Berücksichtigung der Groß-/Kleinschreibung und einer Länge von bis zu 128 Zeichen. Dieser Wert dient zur Unterscheidung von Geräten mit derselben **deviceId**, wenn diese gelöscht und neu erstellt wurden. |
| etag |erforderlich, schreibgeschützt |Eine Zeichenfolge, die gemäß [RFC7232][lnk-rfc7232] ein schwaches ETag für die Geräteidentität darstellt. |
| auth |optional |Ein zusammengesetztes Objekt, das Authentifizierungsinformationen und Sicherheitsdaten enthält. |
| auth.symkey |optional |Ein zusammengesetztes Objekt, das einen primären und einen sekundären Schlüssel enthält, die im Base64-Format gespeichert sind. |
| status |erforderlich |Zugriffsanzeige. Kann **Aktiviert** oder **Deaktiviert** lauten. Sofern der Status **Aktiviert**lautet, kann das Gerät eine Verbindung herstellen. Lautet die Einstellung **Deaktiviert**, kann dieses Gerät auf keinen geräteseitigen Endpunkt zugreifen. |
| statusReason |optional |Eine 128 Zeichen lange Zeichenfolge, die die Ursache des Geräteidentitätsstatus speichert. Alle UTF-8-Zeichen sind zulässig. |
| statusUpdateTime |schreibgeschützt |Eine temporale Anzeige, die Datum und Uhrzeit der letzten Statusaktualisierung anzeigt. |
| connectionState |schreibgeschützt |Ein Feld, das den Verbindungsstatus anzeigt: entweder **Verbunden** oder **Getrennt**. Dieses Feld stellt den Geräteverbindungsstatus aus IoT Hub-Sicht dar. **Wichtig**: Dieses Feld darf nur für Entwicklungs-/Debuggingzwecke verwendet werden. Der Verbindungszustand wird nur für Geräte aktualisiert, die MQTT oder AMQP verwenden. Er basiert außerdem auf Pings auf Protokollebene (MQTT- oder AMQP-Pings) und kann eine Verzögerung von maximal 5 Minuten haben. Aus diesen Gründen sind falsch positive Rückmeldungen möglich, z.B. als verbunden gemeldete Geräte, die jedoch getrennt sind. |
| connectionStateUpdatedTime |schreibgeschützt |Eine temporale Anzeige, die Datum und Uhrzeit der letzten Aktualisierung des Verbindungsstatus angezeigt. |
| lastActivityTime |schreibgeschützt |Eine temporale Anzeige, die anzeigt, an welchem Datum und zu welcher Uhrzeit das Gerät sich zuletzt verbunden und eine Nachricht empfangen oder gesendet hat. |

> [!NOTE]
> Der Verbindungsstatus kann nur den Status der Verbindung aus Sicht von IoT Hub widerspiegeln. Aktualisierungen dieser Statusanzeige können sich je nach Netzwerkbedingungen und -konfigurationen verzögern.
> 
> 

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT-SDKs für Geräte und Dienste][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit der Verwendung der IoT Hub-Identitätsregistrierung vertraut sind, sind möglicherweise die folgenden Themen im IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Verwalten des Zugriffs auf IoT Hub][lnk-devguide-security]
* [Grundlegendes zu Gerätezwillingen – Vorschau][lnk-devguide-device-twins]
* [Aufrufen einer direkten Methode auf einem Gerät][lnk-devguide-directmethods]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, ist möglicherweise das folgende IoT Hub-Tutorial für Sie interessant:

* [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]

<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md



<!--HONumber=Feb17_HO3-->


