<properties
	 pageTitle="Verwalten von IoT Hubs mit dem Azure-Portal | Microsoft Azure"
	 description="Übersicht über die Erstellung und Verwaltung von Azure IoT Hubs über das Azure-Portal"
	 services="iot-hub"
	 documentationCenter=""
	 authors="nasing"
	 manager="timlt"
	 editor=""/>

<tags
	 ms.service="iot-hub"
	 ms.devlang="na"
	 ms.topic="article"
	 ms.tgt_pltfrm="na"
	 ms.workload="na"
	 ms.date="06/28/2016"
	 ms.author="nasing"/>

# Verwalten von IoT Hubs über das Azure-Portal

## Einführung

In diesem Artikel erhalten Sie Informationen zu den ersten Schritten mit Azure IoT Hub über das Azure-Portal, zum Finden von IoT Hubs sowie zum Erstellen und Verwalten von IoT Hubs.

## Suchen nach IoT Hubs

IoT Hubs finden Sie an vielen Stellen.

1. **+ Neu**: **Azure IoT Hub** ist ein IoT-Dienst, den Sie wie andere Dienste unter der Kategorie **Internet der Dinge** unter **+ Neu** finden.

2. Auf IoT Hubs können Sie auch über den Marketplace zugreifen – wie der Hero-Dienst unter **Internet der Dinge**.

## Erstellen eines IoT Hubs

Sie können einen IoT Hub mit den folgenden Methoden erstellen.

1. Wenn Sie einen IoT Hub über die Option **+ Neu** erstellen, wird das folgende Blatt aus dem nächsten Screenshot angezeigt. Beim Erstellen des IoT Hubs mit dieser Methode gehen Sie genauso vor wie beim Erstellen über den Marketplace.

2. Erstellen eines IoT Hubs über den Marketplace: Wenn Sie auf **Erstellen** klicken, wird ein Blatt geöffnet, das mit dem vorherigen Blatt der Option **+ Neu** identisch ist. Die Erstellung eines IoT Hubs umfasst mehrere Schritte, die in den nächsten Abschnitten aufgeführt sind:

### Auswählen des Namen des IoT Hubs

Sie müssen dem Hub einen Namen geben, um einen IoT Hub zu erstellen. Beachten Sie, dass dieser Name für die Hubs eindeutig sein muss. Da im Back-End keine Duplizierung von Hubs zulässig ist, wird eine möglichst eindeutige Benennung dieses Hubs empfohlen.

### Auswählen des Tarifs

Sie können zwischen 4 Tarifen wählen: **Free**, **Standard 1**, **Standard 2** und **Standard S3**. Mit dem Free-Tarif können nur 500 Geräte mit dem IoT Hub verbunden sein, und bis zu 8.000 Nachrichten pro Tag sind möglich.

**Standard S1**: Die IoT Hub-Edition S1 ist für IoT-Lösungen mit einer großen Anzahl von Geräten ausgelegt, die relativ kleine Datenmengen pro Gerät generieren. Mit jeder S1-Einheit können bis zu 400.000 Nachrichten pro Tag (auf allen verbundenen Geräten) übermittelt werden.

**Standard S2**: Die IoT Hub S2-Edition ist auf IoT-Lösungen ausgelegt, in denen Geräte große Datenmengen generieren. Mit jeder Einheit der Edition S2 können bis zu 6 Millionen Nachrichten pro Tag zwischen allen verbundenen Geräten übermittelt werden.

**Standard S3**: Die IoT Hub-Edition S3 eignet sich für IoT-Lösungen, die große Datenmengen generieren. Mit jeder Einheit der Edition S3 können bis zu 300 Millionen Nachrichten pro Tag zwischen allen verbundenen Geräten übermittelt werden.

![][4]

> [AZURE.NOTE] IoT Hub ermöglicht nur einen Free-Hub pro Abonnement.

### IoT Hub-Einheiten

Eine IoT Hub-Einheit enthält eine bestimmte Anzahl von Nachrichten pro Tag. Die Anzahl der IoT-Einheiten bedeutet also, dass die Gesamtanzahl der für diesen Hub unterstützten Geräte der Anzahl der Einheiten multipliziert mit der Anzahl der Nachrichten pro Tag für diesen Tarif entspricht. Beispiel: Wenn der IoT Hub 700.000 eingehende Nachrichten unterstützen soll, wählen Sie zwei Einheiten des Tarifs S1.

### Gerät für Cloud-Partitionen und Ressourcengruppe

Sie können die Anzahl der Partitionen für einen IoT Hub ändern. Standardmäßig sind 4 Partitionen festgelegt, Sie können jedoch eine andere Anzahl von Partitionen aus einer Dropdownliste auswählen.

Für Ressourcengruppen muss nicht zwingend eine leere Ressourcengruppe erstellt werden. Beim Erstellen einer neuen Ressource können Sie entweder eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden.

![][5]

### Auswählen von Abonnements

Azure IoT Hub zeigt automatisch die Liste der Abonnements an, mit denen das Benutzerkonto verknüpft ist. Sie können eine dieser Optionen auswählen, um den IoT Hub diesem Abonnement zuzuordnen.

### Wählen des Standorts

Die Option „Standort“ enthält eine Liste der Regionen, in denen IoT Hub angeboten wird. IoT Hub ist in den folgenden Regionen zur Bereitstellung verfügbar: Australien, Osten; Australien, Südosten; Asien-Pazifik, Osten; Asien-Pazifik, Südosten; Europa, Norden; Europa, Westen; Japan, Osten; Japan, Westen; USA, Osten; USA, Westen.

### Erstellen des IoT Hubs

Wenn alle vorherigen Schritte abgeschlossen sind, kann der IoT Hub erstellt werden. Klicken Sie auf **Erstellen**, um den Back-End-Prozess zur Erstellung dieses IoT Hubs mit den festgelegten Optionen zu starten und die Bereitstellung am angegebenen Ort durchzuführen.

Bitte beachten Sie, dass die Erstellung des IoT Hubs einige Minuten in Anspruch nehmen kann, da es etwas dauert, bis die Back-End-Bereitstellung in den jeweiligen Servern angezeigt werden

## Ändern der Einstellungen des IoT Hubs

Sie können die Einstellungen eines vorhandenen IoT Hub nach dessen Erstellung ändern. Klicken Sie auf den IoT Hub-Namen, um die Seite mit den Einstellungen zu öffnen.

![][8]

**Freigegebene Zugriffsrichtlinien**: Hierbei handelt es sich um die Richtlinien, die die Berechtigungen für Geräte und Dienste zum Herstellen einer Verbindung mit dem IoT Hub definieren. Sie finden diese Richtlinien, indem Sie unter **Einstellungen** auf **Freigegebene Zugriffsrichtlinien** klicken. Auf diesem Blatt können Sie vorhandene Richtlinien ändern oder eine neue Richtlinie hinzufügen.

### Erstellen einer neuen Richtlinie

- Klicken Sie auf **Hinzufügen**, um ein Blatt zu öffnen, auf dem Sie den Namen der neuen Richtlinie und die Berechtigungen, die dieser Richtlinie zugeordnet werden sollen, wie in der folgenden Abbildung gezeigt eingeben können.

	Diesen freigegebenen Richtlinien können mehrere Berechtigungen zugeordnet werden. Die ersten beiden Richtlinien, **Registry lesen** und **Registry schreiben**, berechtigen zum Lesen und Schreiben im Geräteidentitätsspeicher oder der Identitätsregistrierung. Bitte beachten Sie, dass beim Auswählen der Option mit Schreibzugriff auch die Option mit Lesezugriff automatisch ausgewählt wird.

 	Die Richtlinie für die **Serververbindung** gewährt Berechtigungen zum Zugriff auf die cloudseitigen Endpunkte, z.B. die Consumergruppe für Dienste mit Verbindung zum IoT Hub, während die Richtlinie für die **Geräteverbindung** Berechtigungen zum Senden und Empfangen von Nachrichten auf den geräteseitigen Endpunkten des IoT Hubs gewährt.

- Klicken Sie auf **Erstellen**, um der vorhandenen Liste diese neu erstellte Richtlinie hinzuzufügen.

![][10]

## Nachrichten

Klicken Sie auf die **Messaging**-Richtlinien, um eine Liste der Messaging-Eigenschaften für den IoT Hub anzuzeigen, der verändert wird. Zwei wichtige Eigenschaften können geändert oder kopiert werden: **C2D** (Cloud to Device, Cloud zu Gerät) und **D2C** (Device to Cloud, Gerät zu Cloud).

- **C2D-Einstellungen**: Diese Option hat zwei untergeordnete Einstellungen: **C2D-TTL** (Time to Live, Gültigkeitsdauer) und **Aufbewahrungszeit** für Nachrichten. Bei der ersten Erstellung des IoT Hubs werden diese beiden Einstellungen mit einem Standardwert von 1 Stunde erstellt. Allerdings können Sie diese mithilfe der Schieberegler oder durch Eingeben der Werte anpassen.

- **D2C-Einstellungen**: Diese Option hat mehrere untergeordnete Einstellungen, von denen einige bei Erstellung des IoT Hubs benannt bzw. zugewiesen werden und nur in andere untergeordnete Einstellungen kopiert werden können, die angepasst werden können. Diese Einstellungen sind im nächsten Abschnitt aufgeführt.

**Partitionen**: Dieser Wert wird bei Erstellung des IoT Hubs festgelegt und kann durch diese Einstellung geändert werden.

**Event Hub – Kompatibler Name und Endpunkt**: Bei Erstellung des IoT Hubs wird intern ein Event Hub erstellt, auf den der Benutzer unter bestimmten Umständen zugreifen muss. Name und Endpunkt dieses Event Hubs können nicht angepasst werden, stehen jedoch über die Schaltfläche **Kopieren** zur Verfügung.

**Aufbewahrungszeit**: Standardmäßig auf 1 Tag festgelegt, kann aber mithilfe der Dropdownliste in andere Werte geändert werden. Beachten Sie, dass dieser Wert für D2C in Tagen statt Stunden angegeben ist und die Einstellungen für C2D identisch sind.

**Consumergruppen**: Consumergruppen ähneln als Einstellung anderen Messaging-Systemen, die zum Abrufen von Daten auf bestimmte Weise verwendet werden können, um andere Programme oder Dienste mit IoT Hub zu verbinden. Jeder IoT Hub wird mit einer Standard-Consumergruppe erstellt. Sie können Ihren IoT Hubs jedoch Consumergruppen hinzufügen oder diese daraus löschen.

> [AZURE.NOTE] Die Standard-Consumergruppe kann nicht bearbeitet oder gelöscht werden.

![][11]

## Dateiupload

Damit Sie die Dateiuploadfunktion in IoT Hub verwenden können, müssen Sie Ihrem Hub zunächst ein Azure Storage-Konto zuordnen. Wählen Sie die **Dateiupload**-Einstellungen zum Anzeigen einer Liste der Dateiuploadeigenschaften für den IoT Hub, der geändert wird.

**Speichercontainer**: Verwenden Sie das Portal, um einen Blobcontainer in einem Speicherkonto in Ihrem aktuellen Abonnement Ihrer IoT Hub-Instanz zuzuordnen. Erstellen Sie bei Bedarf auf dem Blatt **Speicherkonten** ein neues Speicherkonto und auf dem Blatt **Container** einen neuen Blobcontainer. IoT Hub generiert automatisch SAS-URIs mit Schreibberechtigungen für diesen Blobcontainer, die Geräte beim Hochladen von Dateien verwenden können.

![][14]

**Benachrichtigungen für hochgeladene Dateien empfangen**: Aktivieren oder deaktivieren Sie Dateiuploadbenachrichtigungen über den Umschalter.

**SAS-TTL**: Diese Einstellung dient zum Festlegen der Gültigkeitsdauer der SAS-URIs, die von IoT Hub an das Gerät zurückgegeben werden. Standardmäßig auf 1 Stunde festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

**Standard-TTL für Dateibenachrichtigungseinstellungen**: Die Gültigkeitsdauer einer Dateiuploadbenachrichtigung (Zeit bis zum Ablauf). Standardmäßig auf 1 Tag festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

**Maximale Zahl gesendeter Dateibenachrichtigungen**: Gibt an, wie oft IoT Hub versucht, eine Dateiuploadbenachrichtigung zu senden. Standardmäßig auf 10 festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

![][13]

## Preise und Skalierung

Die Preise eines vorhandenen IoT Hubs können mit folgenden Ausnahmen über die Einstellungen **Preise** geändert werden:

- In der aktuellen Implementierung kann ein IOT Hub mit Free-SKU nicht in eine der kostenpflichtigen SKUs geändert werden oder umgekehrt.
- Im Abonnement kann nur ein IoT Hub des Free-Tarifs enthalten sein.

![][12]

Der Wechsel von einem höheren Tarif (S2 oder S3) in einen niedrigeren Tarif (S1 oder S2) ist nur zulässig, wenn die Anzahl an gesendeten Nachrichten des Tages nicht dazu in Konflikt steht. Wenn die Anzahl der Nachrichten pro Tag beispielsweise 400.000 überschreitet, kann die Ebene für den IoT Hub geändert werden, aber wenn Sie zur Ebene S1 wechseln, wird der Hub für diesen Tag gedrosselt.

## Löschen des IoT Hubs

Klicken Sie zum Suchen des zu löschenden IoT Hubs auf **Durchsuchen**, und wählen Sie dann den zu löschenden Hub aus. Durch Klicken auf die Schaltfläche **Löschen** unterhalb des Hubnamens wird der Hub gelöscht.

## Nächste Schritte

Folgen Sie diesen Links, um mehr über Azure IoT Hub zu erfahren:

- [Erste Schritte mit IoT Hub (Lernprogramm)][lnk-get-started]
- [Was ist Azure IoT Hub?][]


  [4]: ./media/iot-hub-manage-through-portal/create-iothub.png
  [5]: ./media/iot-hub-manage-through-portal/location1.png
  [8]: ./media/iot-hub-manage-through-portal/portal-settings.png
  [10]: ./media/iot-hub-manage-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-manage-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-manage-through-portal/pricing-error.png
  [13]: ./media/iot-hub-manage-through-portal/file-upload-settings.png
  [14]: ./media/iot-hub-manage-through-portal/file-upload-container-selection.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Was ist Azure IoT Hub?]: iot-hub-what-is-iot-hub.md

<!---HONumber=AcomDC_0706_2016-->