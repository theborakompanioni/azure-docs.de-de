<properties
 pageTitle="Einführung in die Bibliothek der Geräteverwaltung | Microsoft Azure"
 description="Clientbibliothek der Azure IoT Hub-Geräteverwaltung"
 services="iot-hub"
 documentationCenter=""
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Einführung in die Clientbibliothek der Azure IoT Hub-Geräteverwaltung

## Übersicht

Mit der Clientbibliothek der Azure IoT Hub-Geräteverwaltung können Sie Ihre IoT-Geräte mit Azure IoT Hub verwalten. Das „Verwalten“ umfasst Aktionen wie das Neustarten, Zurücksetzen auf die Werkseinstellung und Aktualisieren der Firmware. Derzeit stellen wir eine plattformunabhängige C-Bibliothek bereit, aber wir werden in Kürze Unterstützung für weitere Sprachen hinzufügen. Wie unter [Übersicht über die Azure IoT Hub-Geräteverwaltung][lnk-dm-overview] beschrieben, umfasst die Azure IoT Hub-Geräteverwaltung drei Hauptkonzepte:

- Gerätezwillinge
- Geräteaufträge
- Geräteabfragen

Falls Sie hiermit nicht vertraut sind, empfiehlt es sich, vor dem Fortfahren die Übersicht zu lesen. Alle Konzepte sind eng mit der Clientbibliothek verknüpft.

Die Clientbibliothek der Geräteverwaltung hat zwei Hauptaufgaben:

- Synchronisieren von Eigenschaften auf dem physischen Gerät mit dem entsprechenden Gerätezwilling in IoT Hub
- Choreographieren von Geräteaufträgen, die von IoT Hub an das Gerät gesendet werden

Die Geräteeigenschaften des physischen Geräts (z.B. Akkustand und Seriennummer) werden in regelmäßigen Abständen mit dem cloudbasierten Gerätezwilling synchronisiert, damit IoT Hub jederzeit über eine aktuelle Übersicht über die einzelnen physischen Geräte verfügt (sofern das Gerät verbunden ist).

Der Dienst interagiert mit dem physischen Gerät hauptsächlich über Geräteaufträge und den Gerätezwilling. Die folgenden Auftragstypen werden in IoT Hub bereitgestellt. Die Clientbibliothek der Geräteverwaltung übernimmt den Großteil der Orchestrierung für Geräteaufträge, aber es liegt an Ihnen als Entwickler, die erforderlichen Rückrufe auf Ihrem Gerät zur Unterstützung der einzelnen Auftragstypen zu implementieren.

1.	**Firmwareupdate**: Dient zum Aktualisieren der Firmware (bzw. des Betriebssystemimage) auf dem physischen Gerät.
2.	**Neustart**: Startet das physische Gerät neu.
3.	**Zurücksetzung auf Werkseinstellungen**: Setzt die Firmware (bzw. das Betriebssystemimage) des physischen Geräts auf ein ab Werk bereitgestelltes Backupimage zurück, das auf dem Gerät gespeichert ist.
4.	**Konfigurationsaktualisierung**: Konfiguriert den IoT Hub-Client-Agent, der auf dem physischen Gerät ausgeführt wird.
5.	**Geräteeigenschaft lesen**: Dient zum Abrufen des aktuellsten Werts einer Geräteeigenschaft auf dem physischen Gerät.
6.	**Geräteeigenschaft schreiben**: Ändert eine Geräteeigenschaft auf dem physischen Gerät.

In den folgenden Abschnitten werden Sie durch die Architektur der Clientbibliothek geführt und erhalten Richtlinien dazu, wie Sie die unterschiedlichen Geräteobjekte auf dem Gerät implementieren.

## Entwurfsprinzipien und Funktionskonzepte für die Clientbibliothek der Geräteverwaltung
Beim Entwerfen der Clientbibliothek der Geräteverwaltung wurde auf Portabilität und die plattformübergreifende Integration geachtet. Dies wurde mit den folgenden Entwurfsentscheidungen erreicht:

1.	Die Erstellung wurde per LWM2M mit dem COAP-Standardprotokoll durchgeführt, um die Erweiterbarkeit für einen Bereich von unterschiedlichen Geräten abzudecken.
2.	Sie wurde in ANSI C99 geschrieben, um die Portabilität für viele verschiedene Plattformen zu vereinfachen.
3.	Die Bibliothek ist per TCP/TLS und Azure IoT Hub-Authentifizierung (SAS-Token) geschützt, damit sie auch in Szenarien mit hohen Sicherheitsanforderungen verwendet werden kann.
4.	Basiert auf dem [Eclipse Wakaama][lnk-Wakaama]-OSS-Projekt, um vorhandenen Code zu nutzen und wieder in die Community einfließen zu lassen.

### Relevante LWM2M-Konzepte
Wir haben den LWM2M-Standard gewählt, um die Erweiterbarkeit für einen weiten Bereich unterschiedlicher Geräte abzudecken. Um die Entwicklung zu vereinfachen, haben wir das Protokoll größtenteils abstrahiert. Es ist jedoch wichtig, die grundlegenden Prinzipien der Bibliothek zu verstehen, vor allem das Datenmodell und die Übertragung der Daten.

#### Objekte und Ressourcen: Datenmodell in LWM2M
Mit dem LWM2M-Datenmodell werden die Konzepte von Objekten und Ressourcen eingeführt:

- **Objekte** beschreiben eine Reihe von zusammenhängenden Funktionsentitäten im System, z.B. die Geräte- und Firmwareupdates.
- **Ressourcen** dienen zum Beschreiben von Attributen oder Aktionen, die in diesen Objekten enthalten sind, z.B. Informationen zum Akkustand und die Neustartaktion.

Beachten Sie, dass dieses Modell zwei Beziehungen der Art „1:n“ umfasst:

- **Gerät und Objekte**: Jedes Gerät kann mehrere Objekte umfassen. Ein Contoso-Gerät muss beispielsweise ein „Geräteobjekt“ und ein „Serverobjekt“ aufweisen (Objekte werden im nächsten Abschnitt ausführlicher beschrieben).
- **Objekte und Ressourcen**: Jedes Objekt kann mehrere Ressourcen umfassen. Beispielsweise kann ein Objekt die Ressourcen für das Firmwareupdate des Contoso-Geräts enthalten, z.B. den Paket-URI, unter dem das neue Image gespeichert ist.

#### Muster „Beobachten/Benachrichtigen“: Übertragung von Daten in LWM2M
Zusätzlich zu diesen Konzepten ist es wichtig zu verstehen, wie Daten vom Gerät zum Dienst fließen. Hierzu wird von LWM2M das Muster „Beobachten/Benachrichtigen“ („Observe/Notify“) definiert. Wenn das physische Gerät eine Verbindung mit dem Dienst herstellt, initiiert IoT Hub „Beobachtungen“ der ausgewählten Geräteeigenschaften. Anschließend benachrichtigt das physische Gerät den Dienst über Änderungen an den Geräteeigenschaften.

In unserer Clientbibliothek haben wir das Muster „Beobachten/Benachrichtigen“ implementiert, um Geräteverwaltungsdaten vom Gerät an IoT Hub zu senden. Das Muster wird mit zwei Parametern gesteuert:

- **Mindestzeitraum**: Dies ist der Zeitraum, um den das Gerät das Senden eines Updates für eine beobachtete Eigenschaft verzögert. Er ist auf fünf Minuten festgelegt. Daher sendet das Gerät ein Update für eine beobachtete Eigenschaft maximal alle fünf Minuten. Dies gilt auch, wenn sich der Wert häufiger ändert. Wenn sich die Eigenschaft also jede Minute ändert, ist für den Dienst nur die letzte Änderung in Minute 5 sichtbar.

- **Höchstzeitraum**: Der Zeitraum, nach dem das Gerät – unabhängig davon, ob sich die beobachtete Eigenschaft ändert – ein Update für den Wert der Eigenschaft sendet. Dieser Wert ist auf sechs Stunden festgelegt. Das Gerät sendet also mindestens alle sechs Stunden ein Update für den Wert einer beobachteten Eigenschaft, und zwar auch dann, wenn sich der Wert nicht geändert hat.

> [AZURE.NOTE]  Die einzige Ausnahme bei diesen Parametern ist, dass der Mindestzeitraum für die Eigenschaften, die für den Firmwareupdate-Auftrag verwendet werden, auf 30 Sekunden festgelegt ist. Dies liegt daran, dass sich diese Eigenschaften während der Auftragsausführung sehr häufig ändern. Daher haben wir den Mindestzeitraum reduziert, um den Updateprozess zu beschleunigen.

## Funktionen und Architektur der Clientbibliothek
Wie wir in der Übersicht gesehen haben, übernehmen die Clientbibliotheken zwei Hauptfunktionen:

- Synchronisieren des physischen Geräts mit dem entsprechenden Gerätezwilling in IoT Hub
- Choreographieren von Geräteaufträgen, die vom IoT Hub an das Gerät gesendet werden

In diesem Abschnitt gehen wir auf beide Aufgaben näher ein.

### Synchronisieren des physischen Geräts und seines Gerätezwillings
Für die Clientbibliothek wird das Muster „Beobachten/Benachrichtigen“ verwendet, um den Gerätezwilling aktuell zu halten. Beachten Sie, dass es sich beim Gerätezwilling um die dienstseitige Darstellung des physischen Geräts handelt. Der folgende Prozess läuft ab, um diese Synchronisierung durchzuführen:

1. Das Gerät registriert sich normalerweise während der Initialisierung beim Dienst. Beispiel: „Ich bin ein Gerät, ich habe die Geräte-ID Contoso mit dem Zugriffstoken Y.“
2. Der Dienst beobachtet die Ressourcen auf den Objekten. Beispiel: „Halte mich über den Akkustand meines Contoso-Geräts auf dem Laufenden.“
3. Das Gerät benachrichtigt den Dienst über den Wert der Ressource. Die Häufigkeit der Benachrichtigungen basiert auf dem Mindest- und Höchstzeitraum. Beispiel: „17:00, Akkustand 99%, 17:05, Akkustand 90%, usw.“

### Choreographieren von Geräteaufträgen: Zusammenarbeit von Gerätezwilling und Geräteaufträgen
Um auf einem physischen Gerät eingreifen zu können, muss der Dienst nach dem zugeordneten Gerätezwilling suchen. Hierzu können Eigenschaften abgefragt werden, oder es kann nach einer bestimmten ID gesucht werden. Wenn die Zwillings-ID bekannt ist (und somit die Übereinstimmung mit dem physischen Gerät), kann der Dienst einen Geräteauftrag auf dem physischen Gerät starten.

Der Geräteauftrag stellt eine Reihe von unterschiedlichen Befehlen dar, die für den jeweils durchzuführenden Prozess stehen (z.B. Schritte für das Firmwareupdate). Ein Geräteauftrag kann aus mehreren Schritten bestehen:

1.	Der Gerätezwilling verfügt über Eigenschaften, die den Zustand eines Geräteauftrags darstellen.
2.	Um die verschiedenen Prozesse im Geräteauftrag zu durchlaufen, müssen die Eigenschaften auf dem Gerätezwilling einen bestimmten Wert aufweisen. Daher muss das physische Gerät die richtige Eigenschaft festlegen, damit es mit dem Gerätezwilling synchronisiert werden und der Auftrag fortgesetzt werden kann.

Diese Sequenz wird wiederholt, wenn der Prozess aus mehreren Schritten besteht. (Beispiel: Während eines Firmwareupdates ändert der Gerätezwilling seine Eigenschaften im Laufe der verschiedenen Schritte mehrmals, bevor der Auftrag als abgeschlossen angesehen wird.)

## Richtlinien zur Implementierung der Geräteverwaltung in Ihrem Client
In den vorherigen Abschnitten haben Sie Informationen zu den Funktionen der Clientbibliotheken der Geräteverwaltung, zu den Entwurfsprinzipien und zur Verknüpfung mit LWM2M erhalten. Jetzt wird beschrieben, wie die einzelnen Bestandteile während der Laufzeit zusammengefügt werden.

Im Wesentlichen übernimmt die Clientbibliothek die Kommunikation zwischen dem Gerät und dem Dienst, sodass nur noch die Implementierung der gerätespezifischen Logik durchgeführt werden muss. Diese besteht aus zwei Teilen:

1.	**Implementieren der gerätespezifischen Details**: Umfasst das Schreiben der gerätespezifischen Logik, um die vom Dienst angeforderten Aktionen (z.B. das Herunterladen des Firmwarepakets) in den entsprechenden Rückrufen durchzuführen. Ein Beispiel für diese Art von Rückruf für das Firmwareupdatepaket ist unten angegeben. Die Rückrufe befinden sich in den C-Dateien des Ordners [an diesem Speicherort][lnk-github1].

            object_firmwareupdate *obj = get_firmwareupdate_object(0);
            obj->firmwareupdate_packageuri_write_callback =     start_firmware_download;
            // platform specific code
            obj->firmwareupdate_update_execute_callback = start_firmware_update;
            //platform specific code


2.	**Informieren der Clientbibliothek über die Änderung der Eigenschaften**: Hierfür rufen Sie die entsprechenden festgelegten Funktionen in der H-Datei des Ordners [an diesem Speicherort][lnk-github2] auf.

        IOTHUB_CLIENT_RESULT set_firmwareupdate_state(uint16_t instanceId, int value);

### Objekte, die in der Clientbibliothek der Geräteverwaltung implementiert werden müssen

Es wurde gerade beschrieben, wie Sie die gerätespezifische Logik zum Durchführen von Geräteaufträgen implementieren. Nun wird erläutert, welche Objekte für die Verwendung verfügbar sind.

Einige dieser Objekte sind erforderlich. Dies bedeutet, dass Sie die gerätespezifische Logik implementieren müssen, damit sie Teil der IoT Hub-Geräteverwaltung ist. Andere Objekte sind optional, sodass Sie sie je nach Ihren Dienstanforderungen auswählen können (z.B. können Sie sich dafür entscheiden, keine Firmwareupdates mit IoT Hub durchzuführen). Die Objekte werden im Folgenden beschrieben:

- **Geräteobjekt (erforderlich)**: Stellt gerätespezifische Informationen bereit, z.B. Herstellerinformationen, Modellnummer, Seriennummer, Gerätezeit. Der Dienst kann diese Informationen lesen und in einigen Fällen auch aktualisieren. Außerdem werden zwei Aktionen definiert, die der Dienst auf einem Gerät durchführen kann: Neustart und Zurücksetzung auf die Werkseinstellung.
- **Serverobjekt (erforderlich)**: Enthält Verbindungsparameter und Einstellungen zum Herstellen der Verbindung mit IoT Hub, z.B. die Lebensdauer der Registrierung und die Transportbindung. Der Dienst kann nur diese Informationen lesen.
- **Config-Objekt (optional)**: Enthält benutzerdefinierte Konfigurationsinformationen, die vom Gerät abgefragt oder vom Dienst per Pushvorgang auf ein Gerät übertragen werden können, um die Gerätekonfiguration zu vereinfachen. Der Dienst kann diese Informationen lesen und aktualisieren.
- **Firmwareupdateobjekt (optional)**: Stellt eine Firmwareupdateaktion bereit, die vom Dienst aufgerufen werden kann. Darüber hinaus werden Informationen wie der Speicherort des Firmwarepakets und der Status eines laufenden Firmwareupdatevorgangs bereitgestellt.

Jedes dieser Objekte verfügt über eine Gruppe von zugeordneten Ressourcen (denken Sie an die 1:n-Zuordnung). Die Liste mit den LWM2M-Objekten und allen zugeordneten Ressourcen, die in der Azure IoT Hub-Geräteverwaltung unterstützt werden, finden Sie am Ende dieses Artikels.

> [AZURE.NOTE] Benutzerdefinierte Geräteeigenschaften, mehrere Ressourceninstanzen und mehrere Objektinstanzen werden in der aktuellen Version des Systems nicht unterstützt.

### Zusammenfügen des Gesamtbilds

Unten ist ein Diagramm angegeben, in dem alle Bestandteile zusammengefügt sind. Rechts im blauen Bereich sehen Sie die verschiedenen Komponenten der Clientbibliothek der Geräteverwaltung: Objekte, Handler und Benachrichtigungsmethoden. Links im grünen Bereich ist die Logik dargestellt, die Sie auf Geräteanwendungsebene schreiben müssen. Die Clientbibliothek verbindet die Anwendungslogik mit IoT Hub, um sicherzustellen, dass die Kommunikation und Choreographie richtig durchgeführt wird.

Die folgende Abbildung zeigt die Komponenten, aus denen sich die Clientbibliothek der Geräteverwaltung zusammensetzt.

![][img-library-overview]

## Nächste Schritte: Sammeln von praktischer Erfahrung
In diesem Artikel wurden die Grundlagen beschrieben, die für die Verwendung der Clientbibliothek der IoT Hub-Geräteverwaltung für C gelten.

Um praktische Erfahrungen zu sammeln, können Sie die folgenden Ressourcen verwenden:

- Beispiel für Intel Edison-Firmwareupdate: Dies ist ein Beispiel, bei dem Geräteverwaltungsfunktionen für ein Intel Edison-Gerät aktiviert sind. Weitere Informationen finden Sie unter [iotdm\_edison\_sample][lnk-edison-sample].
- Beispiel für simulierte Geräte: Ein plattformunabhängiges Gerätebeispiel, das auf Linux- und Windows-Geräten ausgeführt werden kann. Weitere Informationen finden Sie unter [iotdm\_simple\_sample][lnk-simple-sample].
- Weitere Informationen zu LWM2M-Objekten finden Sie unter [OMNA Lightweight M2M (LWM2M) Object & Resource Registry][lnk-oma] \(OMNA Lightweight M2M (LWM2M) – Objekt- und Ressourcenregistrierung).

## Anhang: Derzeit unterstützte LWM2M-Objekte und -Ressourcen

### Geräteobjekt

| Ressourcenname | Für Ressource zulässiger Remotevorgang | Typ | Bereich und Einheiten | Beschreibung |
|-----------------|--------------------------------------|---------|-----------------|-------------|
| Hersteller | Lesen | String | | Herstellername |
| ModelNumber | Lesen | String | | Modellbezeichner (vom Hersteller angegebene Zeichenfolge) |
| DeviceType | Lesen | String | | Gerätetyp (vom Hersteller angegebene Zeichenfolge)<br/>Hinweis: Wird der serverseitigen API **SystemPropertyNames.DeviceDescription** zugeordnet. |
| SerialNumber | Lesen | String | | Seriennummer des Geräts |
| FirmwareVersion | Lesen | String | | Aktuelle Firmwareversion des Geräts |
| HardwareVersion | Lesen | String | | Aktuelle Hardwareversion des Geräts |
| Reboot | Ausführen | | | Startet das Gerät neu. |
| FactoryReset | Ausführen | | | Dient zum Durchführen der Zurücksetzung auf die Werkseinstellungen des Geräts, damit das Gerät die gleiche Konfiguration wie bei der anfänglichen Bereitstellung hat. |
| CurrentTime | Lesen<br/>Schreiben | Time | | Aktuelle UNIX-Zeit des Geräts. Der Client sollte für das Erhöhen dieses Zeitwerts jeweils nach Ablauf einer Sekunde zuständig sein.<br/>Der Server für die Geräteverwaltung kann auf diese Ressource schreiben, um den Client mit der Zeit auf dem Server zu synchronisieren. |
| UTCOffset | Lesen<br/>Schreiben | String | | UTC-Abweichung ist wirksam. |
| Zeitzone | Lesen<br/>Schreiben | String | | Gibt an, in welcher Zeitzone sich das Gerät befindet. |
| MemoryFree | Lesen | Integer | KB | Geschätzter aktuell verfügbarer Speicherplatz zum Speichern der Daten und Software des Geräts |
| MemoryTotal | Lesen | Integer | KB | Gesamtmenge des Speicherplatzes zum Speichern von Daten und Software auf dem Gerät |
| BatteryLevel | Lesen | Integer | 0 - 100% | Aktueller Akkustand als Prozentwert (zwischen 0 und 100) |
| BatteryStatus | Lesen | Integer | 0 - 6 | **0**: Akku funktioniert richtig und ist nicht an die Stromversorgung angeschlossen.<br/>**1**: Akku wird geladen.<br/>**2**: Akku ist vollständig geladen und an die Stromversorgung angeschlossen.<br/>**3**: Akku ist beschädigt.<br/>**4**: Akkustand ist niedrig.<br/>**5**: Akku ist nicht vorhanden.<br/> **6**: Keine Akkuinformationen verfügbar. |

### Firmwareupdate-Objekt

| Ressourcenname | Vorgang | Typ | Bereich und Einheiten | Beschreibung |
|----------------|-----------|---------|-----------------|-------------|
| Paket | Schreiben | Nicht transparent | | Firmwarepaket im Binärformat.<br/>Wird der Dienst-API zugeordnet:<br/>**SystemPropertyNames.FirmwarePackage** |
| PackageURI | Schreiben | String | 0 - 255 Byte | URI, unter dem das Gerät das Firmwarepaket herunterladen kann.<br/>Wird der Dienst-API zugeordnet: **SystemPropertyNames.FirmwarePackageUri** |
| Aktualisieren | Ausführen | | | Die Firmware wird aktualisiert, indem das unter „Package“ gespeicherte Firmwarepaket oder die unter dem Paket-URI heruntergeladene Firmware verwendet wird.<br/>Wird der Dienst-API zugeordnet:<br/>**ScheduleFirmwareUpdateAsync** |
| Zustand | Lesen | Integer | 1-3 | Status des Firmwareupdate-Prozesses:<br/>**1**: Leerlauf. Dies kann vor dem Herunterladen des Firmwarepakets oder nach dem Anwenden des Firmwarepakets auftreten.<br/>**2**: Firmwarepaket wird heruntergeladen.<br/>**3**: Firmwarepaket wurde heruntergeladen.<br/> Wird der Dienst-API zugeordnet: **SystemPropertyNames.FirmwareUpdateState** |
| UpdateResult | Lesen | Integer | 0 - 6 | Ergebnis des Downloads oder Updates der Firmware<br/>**0**: Standardwert<br/>**1**: Firmwareupdate erfolgreich<br/>**2**: Nicht genügend Speicher für das neue Firmwarepaket<br/>**3**: Nicht genügend Arbeitsspeicher beim Herunterladen des Firmwarepakets<br/>**4**: Verbindungsverlust beim Herunterladen des Firmwarepakets<br/>**5**: Fehler bei CRC-Prüfung für neu heruntergeladenes Paket<br/>**6**: Nicht unterstützter Typ des Firmwarepakets<br/>**7**: Ungültiger URI Wird der Dienst-API zugeordnet: **SystemPropertyNames.FirmwareUpdateResult** |
| PkgName | Lesen | String | 0 - 255 Byte | Beschreibender Name des Firmwarepakets, auf das von der Ressource **Package** verwiesen wird<br/>Wird der Dienst-API zugeordnet:<br/>**SystemPropertyNames.FirmwarePackageName** |
| PackageVersion | Lesen | String | 0 - 255 Byte | Version des Firmwarepakets, auf das von der Ressource **Package** verwiesen wird<br/>Wird der Dienst-API zugeordnet:<br/>**SystemPropertyNames.FirmwarePackageVersion** |

### LWM2M-Serverobjekt

| Ressourcenname | Vorgang | Typ | Bereich und Einheiten | Beschreibung |
|------------------------|------------|---------|-----------------|---------------|
| Standardmäßiger Mindestzeitraum | Lesen Schreiben | Integer | Sekunden | Dies ist der Zeitraum, um den das Gerät das Senden eines Updates für eine beobachtete Eigenschaft verzögert. Bei einer **DefaultMinPeriod** von fünf Minuten sendet das Gerät beispielsweise maximal alle fünf Minuten ein Update für eine beobachtete Eigenschaft. Dies gilt auch, wenn sich der Wert häufiger ändert. Wird der Dienst-API zugeordnet: **SystemPropertyNames.DefaultMinPeriod** |
| Standardmäßiger Höchstzeitraum | Lesen Schreiben | Integer | Sekunden | Der Zeitraum (in Sekunden), nach dem das Gerät – unabhängig davon, ob sich die beobachtete Eigenschaft ändert – ein Update für den Wert der Eigenschaft sendet. Bei einer **DefaultMaxPeriod** von sechs Stunden sendet eine beobachtete Eigenschaft beispielsweise alle sechs Stunden ein Update für den Wert der Eigenschaft, und zwar unabhängig davon, ob sich die Ressource ändert.<br/>Wird der Dienst-API zugeordnet:<br/>**SystemPropertyNames.DefaultMaxPeriod** |
| Gültigkeitsdauer | Lesen Schreiben | Integer | Sekunden | Die Registrierungslebensdauer des Geräts. Eine neue Registrierungs- oder Updateanforderung muss vom Gerät innerhalb dieser Lebensdauer empfangen werden, da die Registrierung des Geräts für den Dienst ansonsten aufgehoben wird.<br/>Wird der Dienst-API zugeordnet:<br/>**SystemPropertyNames.RegistrationLifetime** |

### Config-Objekt

| Ressourcenname | Vorgang | Typ | Bereich und Einheiten | Beschreibung |
|---------------|------------|--------|-----------------|-------------|
| Name | Lesen Schreiben | String | | Führt eine eindeutige Identifizierung des Namens der Gerätekonfiguration durch, die gelesen oder aktualisiert werden soll. |
| Wert | Lesen Schreiben | String | | Führt eine eindeutige Identifizierung des Konfigurationswerts durch, der gelesen oder aktualisiert werden soll. |
| Anwenden | Ausführen | | | Wendet die Konfigurationsänderung auf das Gerät an. |

## Nächste Schritte

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Entwicklerhandbuch][lnk-devguide]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

[img-library-overview]: media/iot-hub-device-management-library/library.png
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-simple-sample]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c/iotdm_client/samples/iotdm_simple_sample
[lnk-edison-sample]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c/iotdm_client/samples/iotdm_edison_sample
[Azure IoT Hub device SDK]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c
[Azure IoT Hub]: Link%20to%20DM%20Overview
[Lightweight M2M]: http://openmobilealliance.org/about-oma/work-program/m2m-enablers/
[CoAP]: https://tools.ietf.org/html/rfc7252
[Wakaama]: https://github.com/eclipse/wakaama
[OMA LWM2M Object and resource registry]: http://technical.openmobilealliance.org/Technical/technical-information/omna/lightweight-m2m-lwm2m-object-registry

[lnk-Wakaama]: https://github.com/eclipse/wakaama
[lnk-github1]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c/iotdm_client/lwm2m_objects
[lnk-github2]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c/iotdm_client/lwm2m_objects
[lnk-oma]: http://technical.openmobilealliance.org/Technical/technical-information/omna/lightweight-m2m-lwm2m-object-registry

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0810_2016-->