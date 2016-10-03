<properties
 pageTitle="Entwicklungsleitfaden für IoT Hub | Microsoft Azure"
 description="In diesem Entwicklungsleitfaden für Azure IoT Hub werden Themen wie IoT Hub-Endpunkte, Sicherheit, Geräteidentitätsregistrierung und Messaging abgedeckt."
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/02/2016" 
 ms.author="dobett"/>

# Entwicklungsleitfaden für Azure IoT Hub

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht.

Azure IoT Hub hilft, Folgendes bereitzustellen:

* Eine sichere Kommunikation unter Verwendung von Zugriffssteuerung und Sicherheitsanmeldeinformationen auf Gerätebasis.
* Zuverlässiges, hyperskalierbares Messaging zwischen Cloud und Gerät (Cloud-to-Device, C2D) sowie zwischen Gerät und Cloud (Device-to-Cloud, D2C)
* Eine problemlose Geräteanbindung mithilfe von Gerätebibliotheken für die gängigsten Sprachen und Plattformen

In diesem Artikel werden die folgenden Themen behandelt:

- [Endpunkte](#endpoints). In diesem Abschnitt werden die verschiedenen Endpunkte beschrieben, die jeder IoT Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
- [Geräteidentitätsregistrierung](#device-identity-registry). Dieser Abschnitt beschreibt, welche Informationen in jeder IoT Hub-Geräteidentitätsregistrierung gespeichert werden und wie Sie auf die Informationen zugreifen und sie ändern können.
- [Sicherheit](#security). In diesem Abschnitt wird das Sicherheitsmodell beschrieben, mit dem sowohl für Geräte als auch für Cloudkomponenten Zugriff auf IoT Hub-Funktionalität gewährt werden.
- [Messaging](#messaging). In diesem Abschnitt werden die Messagingfunktionen (D2C und C2D) beschrieben, die von IoT Hub verfügbar gemacht werden.
- [Kontingente und Drosselung](#throttling). In diesem Abschnitt werden die Kontingente zusammengefasst, die bei der Verwendung von IoT Hub gelten.

## Endpunkte <a id="endpoints"></a>

Azure IoT Hub ist ein mehrinstanzenfähiger Dienst, der seine Funktionen für zahlreiche Akteure bereitstellt. Das nachstehende Diagramm zeigt die verschiedenen Endpunkte, die von IoT Hub verfügbar gemacht werden.

![IoT Hub-Endpunkte][img-endpoints]

Im Folgenden finden Sie eine Beschreibung der Endpunkte:

* **Ressourcenanbieter**. Der IoT Hub-Ressourcenanbieter macht eine [Azure Resource Manager][lnk-arm]-Schnittstelle verfügbar, mit der Azure-Abonnementbesitzer IoT Hubs erstellen und löschen sowie IoT Hub-Eigenschaften aktualisieren können. IoT Hub-Eigenschaften steuern im Gegensatz zur Zugriffssteuerung auf Geräteebene (siehe Abschnitt [Zugriffssteuerung](#accesscontrol) weiter unten) Sicherheitsrichtlinien auf Hubebene und funktionale Optionen für das C2D- und D2C-Messaging. Der Ressourcenanbieter ermöglicht außerdem das [Exportieren von Geräteidentitäten](#importexport).
* **Geräteidentitätsverwaltung**. Jeder IoT Hub legt einen Satz an HTTP-REST-Endpunkten zum Verwalten von Geräteidentitäten offen (zum Erstellen, Abrufen, Aktualisieren und Löschen). Geräteidentitäten werden zur Geräteauthentifizierung und für die Zugriffssteuerung eingesetzt. Weitere Informationen finden Sie unter [Geräteidentitätsregistrierung](#device-identity-registry).
* **Geräteendpunkte**. Für jedes Gerät, das in der Geräteidentitätsregistrierung bereitgestellt wird, stellt IoT Hub eine Reihe von Endpunkten zur Verfügung, die ein Gerät zum Senden und Empfangen von Nachrichten verwenden kann:
    - *Senden von D2C-Nachrichten*. Verwenden Sie diesen Endpunkt, um D2C-Nachrichten zu senden. Weitere Informationen finden Sie unter [Messaging zwischen Gerät und Cloud](#d2c).
    - *Empfangen von C2D-Nachrichten*. Ein Gerät verwendet diesen Endpunkt, um gezielte C2D-Nachrichten zu empfangen. Weitere Informationen finden Sie unter [Messaging zwischen Cloud und Gerät](#c2d).
    - *Initiieren von Dateiuploads*. Ein Gerät verwendet diesen Endpunkt zum Empfangen eines Azure Storage-SAS-URIs von IoT Hub, um eine Datei hochzuladen. Weitere Informationen finden Sie unter [Dateiuploads](#fileupload).

    Diese Endpunkte werden über die Protokolle [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 und [AMQP 1.0][lnk-amqp] verfügbar gemacht. Beachten Sie, dass AMQP auch über [WebSockets][lnk-websockets] an Port 443 verfügbar ist.
* **Dienstendpunkte**. Jeder IoT Hub legt eine Reihe von Endpunkten offen, die Ihr Anwendungs-Back-End zur Kommunikation mit Ihren Geräten verwenden kann. Diese Endpunkte werden aktuell nur bei Verwendung des [AMQP][lnk-amqp]-Protokolls verfügbar gemacht.
    - *Empfangen von D2C-Nachrichten*. Dieser Endpunkt ist kompatibel mit [Azure Event Hubs][lnk-event-hubs]. Ein Back-End-Dienst kann ihn zum Lesen aller D2C-Nachrichten verwenden, die Ihre Geräte senden. Weitere Informationen finden Sie unter [Messaging zwischen Gerät und Cloud](#d2c).
    - *Senden von C2D-Nachrichten und Empfangen von Übermittlungsbestätigungen*. Diese Endpunkte ermöglichen Ihrem Anwendungs-Back-End das Senden von zuverlässigen C2D-Nachrichten sowie das Empfangen zugehöriger Übermittlungs- oder Ablaufbestätigungen. Weitere Informationen finden Sie unter [Messaging zwischen Cloud und Gerät](#c2d).
    - *Empfangen von Dateibenachrichtigungen*. Dieser Messaging-Endpunkt ermöglicht den Empfang von Benachrichtigungen, wenn Ihre Geräte erfolgreich eine Datei hochgeladen haben.

Im Artikel [Microsoft Azure IoT SDKs][lnk-sdks] werden die verschiedenen Möglichkeiten zum Zugriff auf diese Endpunkte beschrieben.

Schließlich ist es wichtig zu wissen, dass alle IoT Hub-Endpunkte das [TLS][lnk-tls]-Protokoll nutzen und Endpunkte niemals auf unverschlüsselten/unsicheren Kanälen verfügbar gemacht werden.

### Lesen von Daten aus Event Hubs-kompatiblen Endpunkten <a id="eventhubcompatible"></a>

Wenn Sie das [Azure Service Bus-SDK für .NET][lnk-servicebus-sdk] oder den [Event Hubs-Ereignisprozessorhost][lnk-eventprocessorhost] verwenden, können Sie eine beliebige IoT Hub-Verbindungszeichenfolge mit den richtigen Berechtigungen verwenden. Anschließend verwenden Sie **messages/events** als Event Hub-Name.

Wenn Sie SDKs (oder Produktintegrationen) verwenden, die nicht IoT Hub-fähig sind, müssen Sie einen Event Hubs-kompatiblen Endpunkt und den Event Hub-Namen aus den IoT Hub-Einstellungen im [Azure-Portal][lnk-management-portal] abrufen:

1. Klicken Sie auf dem IoT Hub-Blatt auf **Messaging**.
2. Im Abschnitt **Gerät-zu-Cloud-Einstellungen** sehen Sie die Werte **Event Hub-kompatibler Endpunkt**, **Event Hub-kompatibler Name** und **Partitionen**.

    ![D2C-Einstellungen][img-eventhubcompatible]

> [AZURE.NOTE] Wenn das SDK einen Wert für **Hostname** oder **Namespace** benötigt, entfernen Sie das Schema aus dem **Event Hub-kompatiblen Endpunkt**. Wenn es sich bei Ihrem Event Hub-kompatiblen Endpunkt beispielsweise um **sb://iothub-ns-myiothub-1234.servicebus.windows.net/** handelt, lautet der **Hostname iothub-ns-myiothub-1234.servicebus.windows.net** und der **Namespace iothub-ns-myiothub-1234**.

Sie können in diesem Fall eine beliebige Sicherheitsrichtlinie für den gemeinsamen Zugriff mit den **ServiceConnect**-Berechtigungen zur Verbindungsherstellung mit dem angegebenen Event Hub verwenden.

Wenn Sie eine Event Hub-Verbindungszeichenfolge mit den zuvor angegebenen Informationen erstellen müssen, verwenden Sie das folgende Muster:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Nachfolgend finden Sie eine Liste der SDKs und Integrationen, die Sie mit Event Hub-kompatiblen Endpunkten verwenden können, die IoT Hub verfügbar macht:

* [Java-Event Hubs-Client](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm-Spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md), Sie können sich die [Spoutquelle](https://github.com/apache/storm/tree/master/external/storm-eventhubs) bei GitHub ansehen.
* [Apache Spark-Integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## Geräteidentitätsregistrierung

Jeder IoT Hub enthält eine Geräteidentitätsregistrierung. Mit dieser Registrierung können Sie Ressourcen auf Gerätebasis im Dienst erstellen, z.B. eine Warteschlange, die In-Flight-C2D-Nachrichten enthält. Sie können mit der Registrierung auch den Zugriff auf die geräteseitigen Endpunkte zulassen, wie im Abschnitt [Zugriffssteuerung](#accesscontrol) erläutert.

Auf höherer Ebene ist die Geräteidentitätsregistrierung eine RESTful-fähige Sammlung von Geräteidentitätsressourcen. In den folgenden Abschnitten werden die Eigenschaften von Geräteidentitätsressourcen sowie die Vorgänge beschrieben, die über die Registrierung für Identitäten ausgeführt werden können.

> [AZURE.NOTE] Nähere Informationen zum HTTP-Protokoll und den SDKs, die Sie für die Interaktion mit der Geräteidentitätsregistrierung verwenden können, finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

### Geräteidentitätseigenschaften <a id="deviceproperties"></a>

Geräteidentitäten werden als JSON-Dokumente mit den folgenden Eigenschaften dargestellt.

| Eigenschaft | Options | Beschreibung |
| -------- | ------- | ----------- |
| deviceId | erforderlich, bei Aktualisierungen schreibgeschützt | Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}` besteht. |
| generationId | erforderlich, schreibgeschützt | Eine vom Hub generierte Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung, die bis zu 128 Zeichen lang ist. Diese Zeichenfolge dient zur Unterscheidung von Geräten mit derselben **deviceId**, wenn diese gelöscht und neu erstellt wurden. |
| etag | erforderlich, schreibgeschützt | Eine Zeichenfolge, die ein schwaches Etag für die Geräteidentität gemäß [RFC7232][lnk-rfc7232] darstellt.|
| auth | optional | Ein zusammengesetztes Objekt, das Authentifizierungsinformationen und Sicherheitsdaten enthält. |
| auth.symkey | optional | Ein zusammengesetztes Objekt, das einen primären und einen sekundären Schlüssel enthält, die im Base64-Format gespeichert sind. |
| status | erforderlich | Zugriffsanzeige. Kann **Aktiviert** oder **Deaktiviert** lauten. Sofern der Status **Aktiviert** lautet, kann das Gerät eine Verbindung herstellen. Lautet die Einstellung **Deaktiviert**, kann dieses Gerät auf keinen geräteseitigen Endpunkt zugreifen. |
| statusReason | optional | Eine 128 Zeichen lange Zeichenfolge, die die Ursache des Geräteidentitätsstatus speichert. Alle UTF-8-Zeichen sind zulässig. |
| statusUpdateTime | schreibgeschützt | Eine temporale Anzeige, die Datum und Uhrzeit der letzten Statusaktualisierung anzeigt. |
| connectionState | schreibgeschützt | Ein Feld, das den Verbindungsstatus anzeigt: entweder **Verbunden** oder **Getrennt**. Dieses Feld stellt den Geräteverbindungsstatus aus IoT Hub-Sicht dar. **Wichtig:** Dieses Feld darf nur für Entwicklungs-/Debuggingzwecke verwendet werden. Der Verbindungszustand wird nur für Geräte aktualisiert, die MQTT oder AMQP verwenden. Er basiert außerdem auf Pings auf Protokollebene (MQTT- oder AMQP-Pings) und kann eine Verzögerung von maximal 5 Minuten haben. Aus diesen Gründen sind falsch positive Rückmeldungen möglich, z.B. als verbunden gemeldete Geräte, die tatsächlich aber getrennt sind. |
| connectionStateUpdatedTime | schreibgeschützt | Eine temporale Anzeige, die Datum und Uhrzeit der letzten Aktualisierung des Verbindungsstatus angezeigt. |
| lastActivityTime | schreibgeschützt | Eine temporale Anzeige, die anzeigt, an welchem Datum und zu welcher Uhrzeit das Gerät sich zuletzt verbunden und eine Nachricht empfangen oder gesendet hat. |

> [AZURE.NOTE] Der Verbindungsstatus kann nur den Status der Verbindung aus Sicht von IoT Hub widerspiegeln. Aktualisierungen dieser Statusanzeige können sich je nach Netzwerkbedingungen und -konfigurationen verzögern.

### Geräte-Identitätsvorgänge

Die IoT Hub-Geräteidentitätsregistrierung macht die folgenden Vorgänge verfügbar:

* Erstellen einer Geräteidentität
* Aktualisieren einer Geräteidentität
* Abrufen von Geräteidentitäten nach ID
* Löschen einer Geräteidentität
* Auflisten von bis zu 1.000 Identitäten
* Exportieren aller Identitäten in Blobspeicher
* Importieren von Identitäten aus Blobspeicher

Sämtliche der oben aufgeführten Vorgänge erlauben die Verwendung optimistischer Parallelität gemäß [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Die einzige Möglichkeit zum Abrufen aller Identitäten in der Identitätsregistrierung eines Hubs besteht darin, die Funktion [Export](#importexport) zu verwenden.

Für die Identitätsregistrierung eines IoT Hub-Geräts gilt Folgendes:

- Enthält keinerlei Anwendungsmetadaten.
- Der Zugriff kann wie bei einem Wörterbuch über die **deviceId** als Schlüssel erfolgen.
- Ausdrucksbasierte Abfragen werden nicht unterstützt.

Eine IoT-Lösung verfügt in der Regel über einen lösungsspezifischen Speicher mit anwendungsspezifischen Metadaten. Der lösungsspezifische Speicher in einer Lösung für intelligente Gebäude würde beispielsweise den Raum enthalten, in dem ein Temperatursensor bereitgestellt wird.

> [AZURE.IMPORTANT] Verwenden Sie die Geräte-Identitätsregistrierung nur für Geräteverwaltungs- und -bereitstellungsvorgänge. Vorgänge mit hohem Durchsatz zur Laufzeit dürfen nicht von der Ausführung von Vorgängen in der Geräte-Identitätsregistrierung abhängen. Das Überprüfen des Verbindungsstatus eines Geräts vor dem Senden eines Befehls ist z. B. kein unterstütztes Muster. Überprüfen Sie die [Drosselungsraten](#throttling) für die Geräteidentitätsregistrierung und das Muster des [Gerätetakts][lnk-guidance-heartbeat].

### Deaktivieren von Geräten

Sie können Geräte deaktivieren, indem Sie die Eigenschaft **status** für eine Identität in der Registrierung aktualisieren. Typischerweise wird diese Eigenschaft in zwei Szenarien verwendet:

- Während eines Vorgangs zur Bereitstellungsorchestrierung. Weitere Informationen finden Sie unter [Entwerfen der Lösung – Gerätebereitstellung][lnk-guidance-provisioning].
- Wenn aus einem beliebigen Grund die Sicherheit eines Geräts als gefährdet eingestuft oder das Gerät nicht mehr als autorisiert betrachtet wird.

### Importieren und Exportieren von Geräteidentitäten <a id="importexport"></a>

Sie können einen Massenexport von Geräteidentitäten aus der Identitätsregistrierung eines IoT Hubs durchführen. Hierbei werden asynchrone Vorgänge im [Endpunkt des IoT Hub-Ressourcenanbieters](#endpoints) verwendet. Exportvorgänge sind Aufträge mit langer Ausführungszeit, die einen vom Kunden bereitgestellten Blobcontainer zum Speichern von Geräteidentitätsdaten verwenden, die aus der Identitätsregistrierung gelesen werden.

Sie können einen Massenimport von Geräteidentitäten in die Identitätsregistrierung eines IoT Hubs durchführen. Hierbei werden asynchrone Vorgänge im [Endpunkt des IoT Hub-Ressourcenanbieters](#endpoints) verwendet. Importvorgänge sind Aufträge mit langer Ausführungszeit, die einen vom Kunden bereitgestellten Blobcontainer zum Schreiben von Geräteidentitätsdaten in die Geräteidentitätsregistrierung verwenden.

- Ausführliche Informationen zu den Import- und Export-APIs finden Sie unter [Azure IoT Hub – Ressourcenanbieter-APIs][lnk-resource-provider-apis].
- Weitere Informationen zum Ausführen von Import- und Exportaufträgen finden Sie unter [Massenverwaltung von IoT Hub-Geräte-Identitäten][lnk-bulk-identity].

## Sicherheit <a id="security"></a>

In diesem Abschnitt werden die Optionen zum Schützen von Azure IoT Hub beschrieben.

### Zugriffssteuerung <a id="accesscontrol"></a>

IoT Hub verwendet den folgenden Satz an *Berechtigungen*, um Zugriff auf den Endpunkt jedes IoT Hubs zu gewähren. Berechtigungen beschränkten den Zugriff auf einen IoT Hub basierend auf der Funktionalität.

* **RegistryRead**. Diese Berechtigung gewährt Lesezugriff auf die Geräteidentitätsregistrierung. Weitere Informationen finden Sie unter [Geräteidentitätsregistrierung](#device-identity-registry).
* **RegistryReadWrite**. Diese Berechtigung gewährt Lese- und Schreibzugriff auf die Geräteidentitätsregistrierung. Weitere Informationen finden Sie unter [Geräteidentitätsregistrierung](#device-identity-registry).
* **ServiceConnect**. Diese Berechtigung gewährt Zugriff auf die clouddienstseitigen Endpunkte für Kommunikation und Überwachung. Beispielsweise wird Back-End-Clouddiensten die Berechtigung erteilt, D2C-Nachrichten zu empfangen, C2D-Nachrichten zu senden und die zugehörigen Übermittlungsbestätigungen zu empfangen.
* **DeviceConnect**. Diese Berechtigung gewährt Zugriff auf die geräteseitigen Endpunkte für die Kommunikation. Beispielsweise wird die Berechtigung zum Senden von D2C-Nachrichten und das Empfangen von C2D-Nachrichten erteilt. Diese Berechtigung wird von Geräten verwendet.

Sie können Berechtigungen auf folgende Weise festlegen:

* **Richtlinien für den gemeinsamen Zugriff auf Hubebene**. Richtlinien für gemeinsamen Zugriff können eine beliebige Kombination der im vorigen Abschnitt aufgeführten Berechtigungen gewähren. Sie können Richtlinien im [Azure-Portal][lnk-management-portal] oder programmgesteuert mithilfe von [Azure IoT Hub-Ressourcenanbieter-APIs][lnk-resource-provider-apis] definieren. Ein neu erstellter IoT Hub verfügt über die folgenden Standardrichtlinien:

    - **iothubowner**: Richtlinie mit sämtlichen Berechtigungen.
    - **service**: Richtlinie mit ServiceConnect-Berechtigung.
    - **device**: Richtlinie mit DeviceConnect-Berechtigung.
    - **registryRead**: Richtlinie mit RegistryRead-Berechtigung.
    - **registryReadWrite**: Richtlinie mit den Berechtigungen „RegistryRead“ und „RegistryWrite“.


* **Sicherheitsanmeldeinformationen auf Gerätebasis**. Jeder IoT Hub enthält eine [Geräteidentitätsregistrierung](#device-identity-registry). Sie können für jedes Gerät in dieser Registrierung Sicherheitsanmeldeinformationen konfigurieren, die **DeviceConnect**-Berechtigungen erteilen, deren Gültigkeitsbereich auf die entsprechenden Geräteendpunkte festgelegt ist.

Beispielsweise In einer normalen IoT-Lösung:
- Die Geräteverwaltungskomponente verwendet die Richtlinie *registryReadWrite*.
- Die Ereignisprozessorkomponente verwendet die Richtlinie *service*.
- Die Laufzeit-Geschäftslogikkomponente für Geräte verwendet die Richtlinie *service*.
- Einzelne Geräte stellen unter Verwendung von Anmeldeinformationen eine Verbindung her, die in der IoT Hub-Identitätsregistrierung gespeichert sind.

Einen Leitfaden zu IoT Hub-Sicherheitsthemen finden Sie im Abschnitt zur Sicherheit unter [Entwerfen der Lösung][lnk-guidance-security].

### Authentifizierung

Azure IoT Hub gewährt Zugriff auf Endpunkte, indem ein Token zur Verifizierung mit gemeinsam genutzten Zugriffsrichtlinien und Sicherheitsanmeldeinformationen für die Geräteidentität verglichen wird.

Sicherheitsanmeldeinformationen, beispielsweise symmetrische Schlüssel, werden niemals über eine physische Verbindung gesendet.

> [AZURE.NOTE] Der Azure IoT Hub-Ressourcenanbieter wird über Ihr Azure-Abonnement geschützt, ebenso wie alle Anbieter im [Azure Resource Manager][lnk-azure-resource-manager].

Weitere Informationen zur Erstellung und Verwendung von Sicherheitstoken finden Sie im Artikel zu den [IoT Hub-Sicherheitstoken][lnk-sas-tokens].

#### Protokolldetails

Jedes unterstützte Protokoll, z.B. MQTT, AMQP und HTTP, transportiert Token auf unterschiedliche Weise.


HTTP implementiert die Authentifizierung, indem ein gültiges Token in den Anforderungsheader **Authorization** eingeschlossen wird.


Bei Verwendung von [AMQP][lnk-amqp] unterstützt IoT Hub [SASL PLAIN][lnk-sasl-plain] und die auf [AMQP-Ansprüchen basierte Sicherheit][lnk-cbs].

Wenn Sie eine auf AMQP-Ansprüchen beruhende Sicherheit verwenden, gibt der Standard vor, wie die oben aufgeführten Token übertragen werden.

Für SASL PLAIN kann der **Benutzername** Folgendes sein:

* `{policyName}@sas.root.{iothubName}`, wenn Token auf Hubebene verwendet werden.
* `{deviceId}`, wenn Token für Geräte verwendet werden.

In beiden Fällen enthält das Kennwortfeld das Token gemäß der Beschreibung im Artikel zu den [IoT Hub-Sicherheitstoken][lnk-sas-tokens].

Bei Verwenden von MQTT enthält das CONNECT-Paket die deviceId als ClientId, {iothubhostname}/{deviceId} im Feld „Benutzername“ und ein SAS-Token im Feld „Kennwort“. {iothubhostname} muss der vollständige CName des IoT Hubs (z.B. „contoso.azure-devices.net“) sein.

##### Beispiel: #####

Benutzername (bei der Geräte-ID wird Groß-/Kleinschreibung berücksichtigt): `iothubname.azure-devices.net/DeviceId`

Kennwort (SAS mit dem Geräte-Explorer generieren): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] Die [Azure IoT Hub-SDKs][lnk-sdks] generieren automatisch Token, wenn eine Verbindung mit dem Dienst hergestellt wird. In einigen Fällen unterstützen die SDKs nicht alle Protokolle oder Authentifizierungsmethoden.

#### Besonderheiten für SASL PLAIN

Bei Verwendung von SASL PLAIN kann ein Client, der eine Verbindung mit einem IoT Hub herstellt, ein einziges Token für jede TCP-Verbindung verwenden. Wenn das Token abläuft, wird die TCP-Verbindung mit dem Dienst getrennt, und es wird ein Versuch zur erneuten Verbindungsherstellung ausgelöst. Dieses Verhalten ist für eine Komponente im Anwendungs-Back-End unproblematisch, kann für eine geräteseitige Anwendung jedoch sehr schädlich sein. Dies hat folgende Gründe:

*  Gateways stellen eine Verbindung üblicherweise für eine Vielzahl von Geräten her. Bei Verwendung von SASL PLAIN muss für jedes Gerät, das eine Verbindung mit einem IoT Hub herstellen möchte, eine eigene TCP-Verbindung erstellt werden. Bei diesem Szenario erhöht sich der Verbrauch von Rechen- und Netzwerkressourcen erheblich, und es kommt zu einer höheren Latenz für jede Geräteverbindung.
* Der erhöhte Ressourcenverbrauch bei der erneuten Verbindungsherstellung nach jedem Tokenablauf wirkt sich negativ auf Geräte mit Ressourceneinschränkungen aus.

### Gültigkeitsbereich für Anmeldeinformationen auf Hubebene

Sie können den Bereich für Sicherheitsrichtlinien auf Hubebene festlegen, indem Sie Token mit eingeschränktem Ressourcen-URI erstellen. Beispielsweise ist der Endpunkt zum Senden von Gerät-zu-Cloud-Nachrichten von einem Gerät **/devices/{deviceId}/messages/events**. Sie können eine Richtlinie für den gemeinsamen Zugriff auf Hubebene mit **DeviceConnect**-Berechtigungen auch dazu verwenden, um ein Token zu signieren, dessen resourceURI-Element **/devices/{deviceId}** ist. Bei diesem Ansatz wird ein Token erstellt, das nur zum Senden von Gerätenachrichten im Namen des Geräts **deviceId** verwendet werden kann.

Dieser Mechanismus ist mit einer [Event Hubs-Herausgeberrichtlinie][lnk-event-hubs-publisher-policy] vergleichbar und ermöglicht die Implementierung von benutzerdefinierten Authentifizierungsmethoden. Weitere Informationen finden Sie im Sicherheitsabschnitt von [Entwerfen der Lösung][lnk-guidance-security].

## Nachrichten

IoT Hub bietet ein einfaches Messaging für die Kommunikation:

- [Cloud-zu-Gerät (C2D):](#c2d) Von einem Anwendungs-Back-End (*Dienst* oder *Cloud*).
- [Gerät-zu-Cloud (D2C):](#d2c) Von einem Gerät zu einem Anwendungs-Back-End.
- [Dateiuploads](#fileupload) von einem Gerät in ein zugeordnetes Azure Storage-Konto.

Die wichtigsten Eigenschaften beim IoT Hub-Messaging sind eine zuverlässige und stabile Übermittlung von Nachrichten. Diese Eigenschaften ermöglichen Ausfallsicherheit bei zeitweiligen Verbindungsproblemen auf Geräteseite und Lastspitzen bei der Ereignisverarbeitung auf Cloudseite. IoT Hub implementiert *mindestens einmal* Übermittlungsgarantien für das D2C- und C2D-Messaging.

IoT Hub unterstützt mehrere geräteseitige Protokolle (z.B. MQTT, AMQP und HTTP). Um eine nahtlose Interoperabilität zwischen Protokollen zu gewährleisten, definiert IoT Hub ein gemeinsames Nachrichtenformat, das von allen geräteseitigen Protokollen unterstützt wird.

### Nachrichtenformat <a id="messageformat"></a>

IoT Hub-Nachrichten umfassen:

* Einen Satz an *Systemeigenschaften*. Diese Eigenschaften werden von IoT Hub interpretiert oder festgelegt. Dieser Satz ist vordefiniert.
* Einen Satz an *Anwendungseigenschaften*. Dies ist ein Wörterbuch mit Zeichenfolgeneigenschaften, die die Anwendung definieren, und worauf die Anwendung zugreifen kann, ohne den Nachrichtentext deserialisieren zu müssen. IoT Hub ändert diese Eigenschaften nie.
* Ein nicht lesbarer binärer Textkörper.

Weitere Informationen dazu, wie die Nachricht in verschiedenen Protokollen codiert wird, finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

Die folgende Tabelle zeigt den Satz an Systemeigenschaften in IoT Hub-Nachrichten.

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| MessageId | Eine vom Benutzer festgelegte Kennung für die Nachricht; wird für Anforderung-Antwort-Muster verwendet. Format: Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}` besteht. |
| Sequenznummer | Eine Nummer (für jede Gerätewarteschlange eindeutig), die jeder C2D-Nachricht von IoT Hub zugewiesen wird |
| To | Ein Ziel, das in [C2D](#c2d)-Nachrichten angegeben wird. |
| ExpiryTimeUtc | Datum und Uhrzeit des Nachrichtenablaufs. |
| EnqueuedTime | Datum und Uhrzeit des Empfangs der Nachricht durch IoT Hub. |
| CorrelationId | Eine Zeichenfolgeneigenschaft in einer Antwortnachricht, die normalerweise die Nachrichten-ID der Anforderung im Anforderung-Antwort-Muster enthält. |
| UserId | Eine ID zum Festlegen des Ursprungs von Nachrichten. Wenn IoT Hub Nachrichten generiert, wird diese Eigenschaft auf `{iot hub name}` festgelegt. |
| Ack | Ein Feedbacknachrichtengenerator. Diese Eigenschaft wird in C2D-Nachrichten verwendet, um IoT Hub anzuweisen, als Ergebnis der Nachrichtenverarbeitung durch das Gerät Feedbacknachrichten zu generieren. Mögliche Werte: **none** (Standardeinstellung): Es wird keine Feedbacknachricht generiert. **positive**: Feedbacknachricht empfangen, wenn die Nachricht abgeschlossen wurde. **negative**: Feedbacknachricht empfangen, wenn die Nachricht ohne vollständige Verarbeitung durch das Gerät abgelaufen ist (oder die maximale Anzahl von Zustellversuchen erreicht wurde). **full**: Feedback wird sowohl bei erfolgreicher als auch nicht erfolgreicher Nachrichtenverarbeitung generiert. Weitere Informationen finden Sie unter [Nachrichtenfeedback](#feedback). |
| ConnectionDeviceId | Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **deviceId** des Geräts, das die Nachricht sendet. |
| ConnectionDeviceGenerationId | Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **generationId** (gemäß [Geräteidentitätseigenschaften](#deviceproperties)) des Geräts, das die Nachricht gesendet hat. |
| ConnectionAuthMethod | Eine von IoT Hub für D2C-Nachrichten festgelegte Authentifizierungsmethode. Diese Eigenschaft enthält Informationen zu der Methode, die zum Authentifizieren des Geräts verwendet wird, das die Nachricht sendet. Weitere Informationen finden Sie unter [D2C-Anti-Spoofing](#antispoofing).|

### Auswählen des Kommunikationsprotokolls <a id="amqpvshttp"></a>

IoT Hub unterstützt für die geräteseitige Kommunikation die Protokolle MQTT, [AMQP][lnk-amqp], AMQP über WebSockets und HTTP/1. Die folgende Tabelle enthält allgemeine Ratschläge für Ihre Protokollauswahl:

| Protocol | Wann Sie dieses Protokoll auswählen sollten |
| -------- | ------------------------------------ |
| MQTT | Verwenden Sie es auf allen Geräten, die nicht die Verwendung von WebSockets erfordern. |
| AMQPS | Verwenden Sie es für Feld- und Cloudgateways, um die Vorteile geräteübergreifender Multiplexingverbindungen zu nutzen. <br/> Verwenden Sie es, wenn Sie die Verbindung über Port 443 herstellen müssen. |
| HTTPS | Verwenden Sie es für Geräte, die keine anderen Protokolle unterstützen können. |

Sie sollten bei der Auswahl des Protokolls für die geräteseitige Kommunikation folgende Punkte beachten:

* **C2D-Muster**. HTTP/1 verfügt über keine effiziente Methode zum Implementieren von Serverpushvorgängen. Daher fragen Geräte bei Verwendung von HTTP/1 IoT Hub nach C2D-Nachrichten ab. Dieser Ansatz ist sowohl für das Gerät als auch für IoT Hub ineffizient. Unter den aktuellen HTTP/1-Richtlinien sollte jedes Gerät mindestens alle 25 Minuten eine Abfrage nach Nachrichten durchführen. Auf der anderen Seite unterstützen MQTT und AMQP Serverpush beim Empfangen von C2D-Nachrichten. Sie ermöglichen sofortiges Nachrichtenpushen von IoT Hub zum Gerät. Wenn die Übermittlungslatenz eine wichtige Rolle spielt, sind AMQP oder MQTT die zu bevorzugenden Protokolle. Bei nur selten verbundenen Geräten funktioniert auch HTTP/1.
* **Bereichsgateways**. Bei Verwendung von HTTP/1 und MQTT ist es nicht möglich, mehrere Geräte (jedes mit eigenen Anmeldeinformationen pro Gerät) mithilfe der gleichen TLS-Verbindung zu verbinden. Darum sind diese Protokolle für [Bereichsgatewayszenarien][lnk-azure-gateway-guidance] nicht optimal, da sie eine TLS-Verbindung zwischen dem Bereichsgateway und IoT Hub für jedes Gerät benötigen, das mit dem Bereichsgateway verbunden ist.
* **Geräte mit eingeschränkten Ressourcen**. MQTT- und HTTP/1-Bibliotheken haben weniger Speicherbedarf als die AMQP-Bibliotheken. Wenn daher das Gerät über limitierte Ressourcen verfügt (beispielsweise weniger als 1MB RAM), stehen möglicherweise nur diese Protokolle als Protokollimplementierung zur Verfügung.
* **Netzwerkausnahme**. MQTT lauscht standardmäßig an Port 8883. Dies kann Probleme in Netzwerken verursachen, die für Nicht-HTTP-Protokolle geschlossen sind. HTTP und AMQP (über WebSockets) können in diesem Szenario verwendet werden.
* **Größe der Nutzlast**. AMQP und MQTT sind binäre Protokolle, was zu erheblich kompakteren Nutzlasten als bei HTTP/1 führt.

> [AZURE.NOTE] Bei Verwendung von HTTP/1 muss jedes Gerät mindestens alle 25 Minuten eine Abfrage auf C2D-Nachrichten durchführen. In der Entwicklungsphase darf freilich häufiger als alle 25 Minuten eine Abfrage erfolgen.

<a id="mqtt-support">
#### Hinweise zur MQTT-Unterstützung
IoT Hub implementiert die MQTT-Protokollversion 3.1.1 mit den folgenden Einschränkungen und einem bestimmten Verhalten:

  * **QoS 2 wird nicht unterstützt**. Wenn ein Geräteclient eine Nachricht mit **QoS 2** veröffentlicht, schließt IoT Hub die Netzwerkverbindung. Wenn ein Geräteclient ein Thema mit **QoS 2** abonniert, gewährt IoT Hub im **SUBACK**-Paket maximal die QoS-Ebene 1.
  * **Beibehaltungsnachrichten werden nicht beständig speichert**. Wenn ein Geräteclient eine Nachricht mit auf 1 festgelegtem RETAIN-Flag veröffentlicht, fügt IoT Hub der Nachricht die Anwendungseigenschaft **x-opt-retain** hinzu. In diesem Fall speichert IoT Hub die Beibehaltungsnachricht nicht beständig, sondern übergibt sie an die Back-End-Anwendung.

Weitere Informationen finden Sie unter [IoT Hub MQTT-Unterstützung][lnk-mqtt-support].

Schließlich sollte noch der Einsatz eines [Azure IoT-Protokollgateways][lnk-azure-protocol-gateway] erwogen werden, das die Bereitstellung eines hochleistungsfähigen benutzerdefinierten Protokollgateways mit einer direkten Schnittstelle zu IoT Hub ermöglicht. Das Azure IoT-Protokollgateway dient zum Anpassen des Geräteprotokolls zum Unterstützen von Brownfield MQTT-Bereitstellungen oder anderer benutzerdefinierter Protokolle. Dieser Ansatz setzt jedoch voraus, dass Sie ein benutzerdefiniertes Protokollgateway selfhosten und betreiben.

### Gerät an Cloud <a id="d2c"></a>

Wie im Abschnitt [Endpunkte](#endpoints) beschrieben, werden D2C-Nachrichten über einen geräteseitigen Endpunkt (**/devices/{deviceId}/messages/events**) gesendet. Die Nachrichten werden über einen dienstseitigen Endpunkt (**/messages/events**) empfangen, der mit [Event Hubs][lnk-event-hubs] kompatibel ist. Auf diese Weise können Sie standardmäßige Event Hubs-Integration und SDKs zum Empfangen von D2C-Nachrichten verwenden.

IoT Hub implementiert D2C-Messaging in einer Weise, die [Event Hubs][lnk-event-hubs] ähnelt. Die D2C-Nachrichten von IoT Hub ähneln eher Event Hubs-*Ereignissen* als [Service Bus][lnk-servicebus]-*Nachrichten*.

Diese Implementierung hat folgende Auswirkungen:

* Ähnlich wie Event Hubs-Ereignisse sind D2C-Nachrichten dauerhaft und werden in einem IoT Hub maximal sieben Tage aufbewahrt (siehe [Optionen für die D2C-Konfiguration](#d2cconfiguration)).
* D2C-Nachrichten werden für einen festen Satz an Partitionen partitioniert, der zum Zeitpunkt der Erstellung festgelegt wird (siehe [Optionen für die D2C-Konfiguration](#d2cconfiguration)).
* Analog zu Event Hubs müssen Clients, die D2C-Nachrichten lesen, Partitionen und Prüfpunkte verarbeiten können. Siehe [Event Hubs: Ereignisconsumer][lnk-event-hubs-consuming-events].
* Wie Event Hubs-Ereignisse können D2C-Nachrichten maximal 256 KB umfassen und in Batches gruppiert werden, um Sendevorgänge zu optimieren. Batches können maximal 256 KB groß sein und maximal 500 Nachrichten enthalten.

Es gibt jedoch einige wichtige Unterschiede zwischen dem D2C-Messaging von IoT Hub und Event Hubs:

* Wie im Abschnitt [Sicherheit](#security) erläutert, ermöglicht IoT Hub eine Authentifizierung und Zugriffsteuerung auf Gerätebasis.
* IoT Hub unterstützt Millionen von gleichzeitig verbundenen Geräten (siehe [Kontingente und Drosselung](#throttling)), während Event Hubs auf 5.000 AMQP-Verbindungen pro Namespace beschränkt ist.
* IoT Hub ermöglicht keine zufällige Partitionierung mit einem **PartitionKey**. D2C-Nachrichten werden gemäß ihrer ursprünglichen **deviceId** partitioniert.
* Die Skalierung funktioniert in IoT Hub etwas anders als in Event Hubs. Weitere Informationen finden Sie unter [Skalieren von IoT Hub][lnk-guidance-scale].

Hinweis: Dies bedeutet nicht, dass Sie in allen Szenarien IoT Hub durch Event Hubs ersetzen können. In einigen Szenarien mit Ereignisverarbeitung kann es beispielsweise erforderlich sein, Ereignisse in Bezug auf eine andere Eigenschaft oder ein anderes Feld neu zu partitionieren, bevor die Datenströme analysiert werden können. In diesem Szenario können Sie mit einem Event Hub zwei Bestandteile der Pipeline zur Datenstromverarbeitung entkoppeln. Weitere Informationen finden Sie unter *Partitionen* in [Übersicht über Azure Event Hubs][lnk-eventhub-partitions].

Ausführliche Informationen zum Verwenden des D2C-Messaging finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

> [AZURE.NOTE] Bei der Verwendung von HTTP zum Senden von D2C-Nachrichten dürfen Eigenschaftennamen und Eigenschaftswerte nur alphanumerische ASCII-Zeichen sowie die Zeichen ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` enthalten.

#### Datenverkehr ohne Telemetrie

Oftmals senden Geräte nicht nur Telemetriedatenpunkte, sondern auch Nachrichten und Anforderungen, die eine Ausführung und Verarbeitung auf der Anwendungsschicht mit der Geschäftslogik erfordern. Beispiele: Kritische Warnungen, die eine bestimmte Aktion im Back-End auslösen müssen, oder Geräteantworten auf vom Back-End gesendete Befehle.

Weitere Informationen zur besten Verarbeitungsmethode für diese Art von Nachrichten finden Sie unter [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-guidance-d2c-processing].

#### Optionen für die D2C-Konfiguration <a id="d2cconfiguration"></a>

Ein IoT Hub macht die folgenden Eigenschaften zum Steuern des D2C-Messaging verfügbar:

* **Anzahl von Partitionen**. Legen Sie diese Eigenschaft bei der Erstellung fest, um die Anzahl von Partitionen für die D2C-Ereigniserfassung zu definieren.
* **Aufbewahrungsdauer**. Diese Eigenschaft legt die Aufbewahrungszeit für D2C-Nachrichten fest. Als Standardwert ist ein Tag festgelegt, dieser Wert kann jedoch auf sieben Tage erhöht werden.

Analog zu Event Hubs ermöglicht IoT Hub die Verwaltung von Consumergruppen am empfangenden D2C-Endpunkt.

Sie können all diese Eigenschaften sowohl programmgesteuert über die [Azure IoT Hub-Ressourcenanbieter-APIs][lnk-resource-provider-apis] als auch über das [Azure-Portal][lnk-management-portal] ändern.

#### Eigenschaften zum Schutz vor Spoofing <a id="antispoofing"></a>

Um ein Gerätespoofing beim D2C-Messaging zu verhindern, versieht IoT Hub alle Nachrichten mit den folgenden Eigenschaften:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Die ersten beiden Eigenschaften enthalten die Werte für **deviceId** und **generationId** des ursprünglichen Geräts, wie beschrieben unter [Geräteidentitätseigenschaften](#deviceproperties).

Die Eigenschaft **ConnectionAuthMethod** enthält ein serialisiertes JSON-Objekt mit folgenden Eigenschaften:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

### C2D-Nachrichten <a id="c2d"></a>

Wie im Abschnitt [Endpunkte](#endpoints) erläutert, können Sie C2D-Nachrichten über einen dienstseitigen Endpunkt (**/messages/devicebound**) senden. Ein Gerät kann sie über einen gerätespezifischen Endpunkt empfangen (**/devices/{deviceId}/messages/devicebound**).

Jede C2D-Nachricht wird einem bestimmten Gerät zugeordnet, indem die **to**-Eigenschaft auf **/devices/{deviceId}/messages/devicebound** festgelegt wird.

>[AZURE.IMPORTANT] Jede Gerätewarteschlange kann maximal 50 C2D-Nachrichten enthalten. Der Versuch, eine größere Anzahl von Nachrichten an das gleiche Gerät zu senden, führt zu einem Fehler.

> [AZURE.NOTE] Beim Senden von C2D-Nachrichten dürfen Eigenschaftennamen und Eigenschaftswerte nur alphanumerische ASCII-Zeichen sowie die Zeichen ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` enthalten.

#### Nachrichtenlebenszyklus <a id="message lifecycle"></a>

Um die Nachrichtenübermittlung sicher mindestens einmal zu implementieren, werden C2D-Nachrichten in Warteschlangen auf Gerätebasis beibehalten. Geräte müssen explizit den *Abschluss* bestätigen, damit IoT Hub sie aus der Warteschlange entfernen kann. Auf diese Weise wird Ausfallsicherheit bei Verbindungs- und Gerätefehlern gewährleistet.

Die folgende Abbildung zeigt den Lebenszyklus einer C2D-Nachricht und ihren jeweiligen Status.

![Lebenszyklus von C2D-Nachrichten][img-lifecycle]

Wenn der Dienst eine Nachricht sendet, wird diese als *Zur Warteschlange hinzugefügt* betrachtet. Wenn ein Gerät eine Nachricht *empfangen* möchte, *sperrt* IoT Hub die Nachricht (Status wird auf **Nicht sichtbar** gesetzt), um anderen Threads auf dem gleichen Gerät das Empfangen anderer Nachrichten zu ermöglichen. Wenn ein Gerätethread die Verarbeitung einer Nachricht abschließt, wird IoT Hub hierüber durch den *Abschluss* der Nachricht benachrichtigt.

Ein Gerät kann auch Folgendes durchführen:

- *Ablehnen* der Nachricht: IoT Hub versetzt sie in den Status **Unzustellbar**.
- *Verwerfen* der Nachricht: IoT Hub platziert die Nachricht wieder in der Warteschlange mit dem Status **Zur Warteschlange hinzugefügt**.

Bei der Nachrichtenverarbeitung durch den Thread könnte ein Fehler auftreten, ohne dass IoT Hub hierüber benachrichtigt wird. In diesem Fall werden Nachrichten automatisch vom Status **Nicht sichtbar** zurück in den Status **Zur Warteschlange hinzugefügt** versetzt, wenn ein *Timeout für die Sichtbarkeit (oder Sperrung)* abgelaufen ist. Der Standardwert dieses Timeouts ist eine Minute.

Eine Nachricht kann zwischen den Statuswerten **Zur Warteschlange hinzugefügt** und **Nicht sichtbar** maximal so oft wechseln, wie in der Eigenschaft **Anzahl maximaler Zustellungen** in IoT Hub festgelegt wurde. Nachdem diese Anzahl überschritten wurde, legt IoT Hub den Status der Nachricht auf **Unzustellbar** fest. Ebenso kennzeichnet IoT Hub eine Nachricht als **Unzustellbar**, wenn ihre Gültigkeitsdauer abgelaufen ist (siehe [Gültigkeitsdauer](#ttl)).

Ein Tutorial zu C2D-Nachrichten finden Sie unter [Erste Schritte mit Azure IoT Hub-Nachrichten zwischen Cloud und Gerät][lnk-getstarted-c2d-tutorial]. Informationen dazu, wie verschiedene APIs und SDKs die C2D-Funktionalität verfügbar machen, finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

> [AZURE.NOTE] Typischerweise werden C2D-Nachrichten immer dann abgeschlossen, wenn der Verlust der Nachricht keine Auswirkung auf die Anwendungslogik hat. Das ist z.B. der Fall, wenn der Inhalt der Nachricht erfolgreich im lokalen Speicher beibehalten oder ein Vorgang erfolgreich ausgeführt wurde. Die Nachricht könnte auch vorübergehende Informationen übermitteln, deren Verlust der Funktionalität der Anwendung nicht beeinträchtigen würde. In einigen Fällen können Sie für Tasks mit langer Ausführungsdauer die C2D-Nachricht nach Beibehalten der Beschreibung des Tasks im lokalen Speicher abschließen. Dann können Sie das Anwendungs-Back-End mit einer oder mehreren D2C-Nachrichten in verschiedenen Phasen des Taskverlaufs benachrichtigen.

#### Nachrichtenablauf (Gültigkeitsdauer) <a id="ttl"></a>

Jede C2D-Nachricht verfügt über eine Gültigkeitsdauer. Diese Zeit kann (in der Eigenschaft **ExpiryTimeUtc**) explizit durch den Dienst oder durch IoT Hub mithilfe der standardmäßig als IoT Hub-Eigenschaft angegebenen *Gültigkeitsdauer* festgelegt werden. Siehe [Optionen für die C2D-Konfiguration](#c2dconfiguration).

> [AZURE.NOTE] Eine gängige Methode, den Vorteil des Nachrichtenablaufs zu nutzen, ist die Festlegung einer kurzen Gültigkeitsdauer für Werte. So vermeiden Sie, dass Nachrichten an getrennte Geräte gesendet werden. Bei diesem Ansatz wird das gleiche Ergebnis wird erzielt wie beim Aufrechterhalten des Geräteverbindungsstatus, er ist aber deutlich effizienter. Durch die Anforderung von Nachrichtenbestätigungen können Sie von IoT Hub darüber informiert werden, welche der Geräte in der Lage sind, Nachrichten zu empfangen, und welche nicht online sind, bzw. bei welchen bei der Zustellung ein Fehler auftrat.

#### Nachrichtenfeedback <a id="feedback"></a>

Beim Senden einer C2D-Nachricht kann der Dienst das Übermitteln von Feedback auf Nachrichtenbasis anfordern, um über den finalen Status dieser Nachricht informiert zu werden.

- Wenn Sie die **Ack**-Eigenschaft auf **positive** festlegen, generiert IoT Hub nur dann eine Feedbacknachricht, wenn die C2D-Nachricht den Status **Completed** (Abgeschlossen) erreicht hat.
- Wenn Sie die **Ack**-Eigenschaft auf **negative** festlegen, generiert IoT Hub nur dann eine Feedbacknachricht, wenn die C2D-Nachricht den Status **Deadletterd** (Unzustellbar) erreicht.
- Bei Festlegung der **Ack**-Eigenschaft auf **full** generiert IoT Hub in beiden Fällen eine Feedbacknachricht.

> [AZURE.NOTE] Wenn **Ack** auf **full** festgelegt ist und Sie keine Feedbacknachricht erhalten, bedeutet dies, dass die Feedbacknachricht abgelaufen ist. Der Dienst kann nicht wissen, was mit der ursprünglichen Nachricht geschehen ist. In der Praxis sollte ein Dienst sicherstellen, dass Feedback verarbeitet werden kann, bevor es abläuft. Die maximale Ablaufzeit beträgt zwei Tage, daher sollte ausreichend Zeit verbleiben, um den Dienst zu starten, wenn ein Fehler auftritt.

Wie im Abschnitt [Endpunkte](#endpoints) erläutert, übermittelt IoT Hub Feedback in Form von Nachrichten über einen dienstseitigen Endpunkt (**/messages/servicebound/feedback**). Die Semantik für den Empfang von Feedback stimmt mit der für C2D-Nachrichten überein und weist den gleichen [Nachrichtenlebenszyklus](#message lifecycle) auf. Nachrichtenfeedback wird nach Möglichkeit in einer einzigen Nachricht zusammengefasst, die das folgende Format aufweist.

Jede von einem Gerät vom Feedback-Endpunkt empfangene Nachricht umfasst die folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTime | Zeitstempel, der die Erstellung der Nachricht angibt. |
| UserId | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

Der Nachrichtenkörper ist ein serialisiertes JSON-Array aus Datensätzen, von denen jeder die folgenden Eigenschaften aufweist:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTimeUtc | Zeitstempel, der den Zeitpunkt des Nachrichtenergebnisses angibt. Diese Eigenschaft kann beispielsweise angeben, wann das Gerät abgeschlossen wurde oder die Nachricht abgelaufen ist. |
| OriginalMessageId | **MessageId** der C2D-Nachricht, auf die sich das Feedback bezieht. |
| StatusCode | Erforderlicher Ganzzahlwert. In von IoT Hub erzeugten Feedbacknachrichten verwendet. <br/> 0 = Erfolg <br/> 1 = Nachricht abgelaufen <br/> 2 = max. Anzahl von Sendungen überschritten <br/> 3 = Nachricht abgelehnt |
| Beschreibung | Zeichenfolgenwerte für **StatusCode**. |
| DeviceId | **DeviceId** des Zielgeräts für die C2D-Nachricht, auf die sich das Feedback bezieht. |
| DeviceGenerationId | **DeviceGenerationId** des Zielgeräts für die C2D-Nachricht, auf die sich das Feedback bezieht. |


>[AZURE.IMPORTANT] Der Dienst muss eine **MessageId** für die C2D-Nachricht angeben, damit das Feedback der ursprünglichen Nachricht zugeordnet werden kann.

Das folgende Beispiel zeigt den Text einer Feedbacknachricht.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

#### Optionen für die C2D-Konfiguration <a id="c2dconfiguration"></a>

Jeder IoT Hub legt die folgenden Konfigurationsoptionen für das C2D-Messaging offen:

| Eigenschaft | Beschreibung | Bereich und Standardwert |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Standardmäßige Gültigkeitsdauer für C2D-Nachrichten. | ISO\_8601-Intervall bis 2D (mindestens 1 Minute). Standardwert: 1 Stunde. |
| maxDeliveryCount | Maximale Zustellungsanzahl für C2D-Gerätewarteschlangen pro Gerät. | 1 bis 100. Standardwert: 10 |
| feedback.ttlAsIso8601 | Aufbewahrungsdauer für dienstgebundene Feedbacknachrichten. | ISO\_8601-Intervall bis 2D (mindestens 1 Minute). Standardwert: 1 Stunde. |
| feedback.maxDeliveryCount | Maximale Zustellungsanzahl für Feedbackwarteschlangen. | 1 bis 100. Standardwert: 100. |

Weitere Informationen finden Sie unter [Verwalten von IoT Hubs][lnk-portal].

### Dateiuploads <a id="fileupload"></a>

Wie im Abschnitt [Endpunkte](#endpoints) ausführlich beschrieben, können Geräte Dateiuploads initiieren, indem sie eine Benachrichtigung über einen geräteseitigen Endpunkt senden (**/devices/{deviceId}/files**). Wenn ein Gerät IoT Hub über einen abgeschlossenen Upload benachrichtigt, generiert IoT Hub Dateiuploadbenachrichtigungen, die Sie über einen dienstseitigen Endpunkt als Nachrichten empfangen können (**/messages/servicebound/filenotifications**).

Statt Nachrichten über IoT Hub selbst zu übertragen, fungiert IoT Hub als Verteiler für ein zugeordnetes Azure Storage-Konto. Ein Gerät fordert ein Speichertoken von IoT Hub an, das spezifisch für die vom Gerät hochgeladene Datei gilt. Das Gerät verwendet den SAS-URI zum Hochladen der Datei in den Speicher. Wenn der Upload abgeschlossen ist, sendet das Gerät eine Benachrichtigung über den Abschluss an IoT Hub. IoT Hub überprüft, ob die Datei hochgeladen wurde, und fügt dann dem neuen dienstseitigen Messaging-Endpunkt für Dateibenachrichtigungen eine Dateiuploadbenachrichtigung hinzu.

#### Zuweisen eines Azure Storage-Kontos zu IoT Hub

Damit Sie die Funktion zum Hochladen von Dateien verwenden können, müssen Sie IoT Hub zunächst ein Azure Storage-Konto zuweisen. Sie können diesen Schritt entweder über das [Azure-Portal][lnk-management-portal] oder programmgesteuert über [Azure IoT Hub-Ressourcenanbieter-APIs][lnk-resource-provider-apis] ausführen. Nachdem Sie IoT Hub ein Speicherkonto zugewiesen haben, gibt der Dienst einen SAS-URI an ein Gerät zurück, wenn das Gerät eine Anforderung zum Dateiupload initiiert.

> [AZURE.NOTE] Die [Azure IoT Hub-SDKs][lnk-sdks] behandeln automatisch das Abrufen des SAS-URIs, das Hochladen der Datei und das Benachrichtigen von IoT Hub über einen abgeschlossenen Upload.

#### Initialisieren eines Dateiuploads

IoT Hub verfügt über zwei REST-Endpunkte, um den Dateiupload zu unterstützen: einen zum Abrufen des SAS-URIs für den Speicher und einen zum Benachrichtigen von IoT Hub über einen abgeschlossenen Upload. Das Gerät initiiert den Dateiupload, indem es eine GET-Anforderung an IoT Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}` sendet. Der Hub gibt einen SAS-URI, der speziell für die hochzuladende Datei gilt, sowie eine Korrelations-ID zurück, die nach Abschluss des Uploads verwendet werden soll.

#### Benachrichtigen von IoT Hub über einen abgeschlossenen Dateiupload

Das Gerät ist verantwortlich für das Hochladen der Datei in den Speicher mithilfe der Azure Storage-SDKs. Nach Abschluss des Uploads sendet das Gerät eine POST-Anforderung an IoT Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications/{correlationId}`. Dabei verwendet es die mit der ursprünglichen GET-Anforderung empfangene Korrelations-ID.

#### Benachrichtigungen zum Dateiupload

Wenn ein Gerät eine Datei hochlädt und IoT Hub über den Abschluss des Uploads informiert, generiert der Dienst optional eine Benachrichtigung, die den Namen und den Speicherort der Datei enthält.

Wie im Abschnitt [Endpunkte](#endpoints) erläutert, übermittelt IoT Hub Dateiuploadbenachrichtigungen als Nachrichten über einen dienstseitigen Endpunkt (**/messages/servicebound/fileuploadnotifications**). Die Semantik für Dateiuploadbenachrichtigungen stimmt mit der für C2D-Nachrichten überein und weist den gleichen [Nachrichtenlebenszyklus](#message lifecycle) auf. Jede Nachricht, die vom Endpunkt für Dateiuploadbenachrichtigungen abgerufen wird, ist ein JSON-Datensatz mit den folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTimeUtc | Zeitstempel, der die Erstellung der Benachrichtigung angibt. |
| DeviceId | **DeviceId** des Geräts, das die Datei hochgeladen hat. |
| BlobUri | Der URI der hochgeladenen Datei. |
| BlobName | Der Name der hochgeladenen Datei. |
| LastUpdatedTime | Zeitstempel, der die letzte Aktualisierung der Datei angibt. |
| BlobSizeInBytes | Die Größe der hochgeladenen Datei. |

**Beispiel**. Hier sehen Sie einen Beispieltext einer Dateiuploadbenachrichtigung:

```
{
	"deviceId":"mydevice",
	"blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
	"blobName":"mydevice/myfile.jpg",
	"lastUpdatedTime":"2016-06-01T21:22:41+00:00",
	"blobSizeInBytes":1234,
	"enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

#### Konfigurationsoptionen für Dateiuploadbenachrichtigungen <a id="c2dconfiguration"></a>

Jeder IoT Hub legt die folgenden Konfigurationsoptionen für Dateiuploadbenachrichtigungen offen:

| Eigenschaft | Beschreibung | Bereich und Standardwert |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Steuert, ob Dateiuploadbenachrichtigungen in den Endpunkt für Dateibenachrichtigungen geschrieben werden. | Bool. Standardwert: TRUE. |
| **fileNotifications.ttlAsIso8601** | Standardgültigkeitsdauer für Dateiuploadbenachrichtigungen. | ISO\_8601-Intervall bis zu 48 Stunden (mindestens 1 Minute). Standardwert: 1 Stunde. |
| **fileNotifications.lockDuration** | Sperrdauer für die Warteschlange der Dateiuploadbenachrichtigungen. | 5 bis 300 Sekunden (mindestens 5 Sekunden). Standardwert: 60 Sekunden. |
| **fileNotifications.maxDeliveryCount** | Maximale Übermittlungsanzahl für die Warteschlange der Dateiuploadbenachrichtigungen. | 1 bis 100. Standardwert: 100. |

Weitere Informationen finden Sie unter [Verwalten von IoT Hubs][lnk-portal].

## Kontingente und Drosselung <a id="throttling"></a>

Jedes Azure-Abonnement kann maximal 10 IoT Hubs enthalten.

Jeder IoT Hub wird mit einer bestimmten Anzahl von Einheiten in einer spezifischen SKU bereitgestellt (weitere Informationen finden Sie unter [Azure IoT Hub – Preise][lnk-pricing]). Die SKU und Anzahl der Einheiten bestimmen das maximale tägliche Kontingent von Nachrichten, die Sie senden können.

Die SKU legt auch die Drosselungslimits fest, die IoT Hub für alle Vorgänge erzwingt.

### Vorgangsdrosselung

Bei der Vorgangsdrosselung wird die Datenübertragungsrate pro Minute begrenzt, um einen Missbrauch zu verhindern. IoT Hub versucht, möglichst keine Fehler zurückzugeben, es werden jedoch Ausnahmen ausgelöst, wenn die Drosselungsgrenze zu lange überschritten wird.

Die nachfolgende Liste zeigt alle erzwungenen Werte für die Drosselung. Die Werte beziehen sich auf einen einzelnen Hub.

| Drosselung | Wert pro Hub |
| -------- | ------------- |
| Identitätsregistrierungsvorgänge (Erstellen, Abrufen, Aktualisieren, Löschen) | 5000/Minute/Einheit (für S3) <br/> 100/Minute/Einheit (für S1 und S2). |
| Geräteverbindungen | 6\.000/Sekunde/Einheit (für S3), 120/Sekunde/Einheit (für S2), 12/Sekunde/Einheit (für S1). <br/>Mindestens 100/Sekunde. <br/> Zwei S1-Einheiten entsprechen beispielsweise 2*12 = 24/Sekunde. Es sind jedoch mindestens 100/Sekunde auf die Einheiten verteilt vorhanden. Mit neun S1-Einheiten erhalten Sie 108/Sekunde (9*12) über alle Einheiten. |
| Senden von Nachrichten von Geräten an die Cloud | 6\.000/Sekunde/Einheit (für S3), 120/Sekunde/Einheit (für S2), 12/Sekunde/Einheit (für S1). <br/>Mindestens 100/Sekunde. <br/> Zwei S1-Einheiten entsprechen beispielsweise 2*12 = 24/Sekunde. Es sind jedoch mindestens 100/Sekunde auf die Einheiten verteilt vorhanden. Mit neun S1-Einheiten erhalten Sie 108/Sekunde (9*12) über alle Einheiten. |
| C2D-Sendevorgänge | 5\.000/Minute/Einheit (für S3), 100/Minute/Einheit (für S1 und S2). |
| C2D-Empfangsvorgänge | 50\.000/Minute/Einheit (für S3), 1.000/Minute/Einheit (für S1 und S2). |
| Dateiuploadvorgänge | 5\.000 Dateiuploadbenachrichtigungen/Minute/Einheit (S3), 100 Dateiuploadbenachrichtigungen/Minute/Einheit (für S1 und S2). <br/> 10.000 SAS-URIs können gleichzeitig für ein Speicherkonto geöffnet sein.<br/> 10 SAS-URIs/Gerät können gleichzeitig geöffnet sein. | 

Hier muss gesagt werden, dass die Drosselung der *Geräteverbindungen* nicht die maximale Anzahl gleichzeitig verbundener Geräte bestimmt, sondern die Rate, mit der neue Geräteverbindungen mit einem IoT Hub eingerichtet werden können. Die Drosselung ist abhängig von der Anzahl der Einheiten, die für den Hub bereitgestellt werden.

Wenn Sie beispielsweise eine S1-Einheit erwerben, erhalten Sie eine Drosselung von 100 Verbindungen pro Sekunde. Das bedeutet, dass das Herstellen einer Verbindung mit 100.000 Geräten mindestens 1.000 Sekunden (ca. 16 Minuten) dauert. Es können jedoch so viele Geräte gleichzeitig verbunden sein, wie in der Geräteidentitätsregistrierung registriert sind.

Eine ausführliche Erläuterung der IoT Hub-Drosselung finden Sie in dem Blogbeitrag [IoT Hub throttling and you][lnk-throttle-blog] \(Was habe ich mit der IoT Hub-Drosselung zu tun?).

>[AZURE.NOTE] Die Kontingente oder Drosselungsgrenzwerte können jederzeit angehoben werden, indem die Anzahl von bereitgestellten Einheiten in einem IoT Hub erhöht wird.

>[AZURE.IMPORTANT] Identitätsregistrierungsvorgänge sind für die Verwendung zur Laufzeit in Szenarien für die Geräteverwaltung und -bereitstellung vorgesehen. Das Lesen oder Aktualisieren einer großen Anzahl von Geräteidentitäten wird durch [Import-/Exportaufträge](#importexport) unterstützt.

## Nächste Schritte

Nachdem Sie in diesem Dokument einen Überblick über die Entwicklung für IoT Hub erhalten haben, können Sie in den folgenden Artikeln mehr erfahren:

- [Tutorial zum Hochladen von Dateien auf Geräten][lnk-file upload]
- [Programmgesteuertes Erstellen eines IoT Hubs][lnk-create-hub]
- [Einführung in das C SDK][lnk-c-sdk]
- [IoT Hub-SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]
- [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]



[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx

[img-endpoints]: ./media/iot-hub-devguide/endpoints.png
[img-lifecycle]: ./media/iot-hub-devguide/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide/eventhubcompatible.png

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx

[lnk-sas-tokens]: iot-hub-sas-tokens.md
[lnk-azure-gateway-guidance]: iot-hub-guidance.md#field-gateways
[lnk-guidance-provisioning]: iot-hub-guidance.md#provisioning
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-guidance-security]: iot-hub-guidance.md#customauth
[lnk-guidance-heartbeat]: iot-hub-guidance.md#heartbeat

[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-getstarted-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md

[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../resource-group-overview.md
[lnk-azure-resource-manager]: https://azure.microsoft.com/documentation/articles/resource-group-overview/
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-guidance-d2c-processing]: iot-hub-csharp-csharp-process-d2c.md
[lnk-management-portal]: https://portal.azure.com
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-tls]: https://tools.ietf.org/html/rfc5246
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-sdks-summary.md

[lnk-design]: iot-hub-guidance.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md
[lnk-securing]: iot-hub-security-ground-up.md

<!---HONumber=AcomDC_0907_2016-->