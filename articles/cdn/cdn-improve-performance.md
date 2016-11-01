<properties
	pageTitle="Verbessern der Leistung durch Komprimieren von Dateien in Azure CDN | Microsoft Azure"
	description="Erfahren Sie, wie Sie die Geschwindigkeit von Dateiübertragungen erhöhen und die Leistung beim Laden von Seiten verbessern, indem Sie Ihre Dateien in Azure CDN komprimieren."
	services="cdn"
	documentationCenter=""
	authors="camsoper"
	manager="erikre"
	editor=""/>

<tags
	ms.service="cdn"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/28/2016"
	ms.author="casoper"/>

# Verbessern der Leistung durch Komprimieren von Dateien

Die Komprimierung ist eine einfache und effektive Methode zur Verbesserung der Dateiübertragungsrate und zum Steigern der Seitenladeleistung, indem die Dateigröße reduziert wird, bevor die Datei vom Server gesendet wird. Sie reduziert die Bandbreitekosten und steigert die Benutzerfreundlichkeit.

Es gibt zwei Methoden zur Aktivierung der Komprimierung:

- Sie können die Komprimierung auf Ihrem Ursprungsserver aktivieren. In diesem Fall leitet CDN die komprimierten Dateien weiter und liefert komprimierte Dateien an Clients, die sie anfordern.
- Sie können die Komprimierung direkt auf CDN-Edgeservern aktivieren. In diesem Fall komprimiert CDN die Dateien und sendet sie an die Endbenutzer, auch dann, wenn sie nicht vom Ursprungsserver aktiviert wurden.

> [AZURE.IMPORTANT] Es dauert eine gewisse Zeit, bis Änderungen an der CDN-Konfiguration im gesamten Netzwerk verteilt sind. Bei <b>Azure CDN von Akamai</b>-Profilen ist die Weitergabe in der Regel in weniger als einer Minute abgeschlossen. Bei <b>Azure CDN von Verizon</b>-Profilen werden Ihre Änderungen in der Regel innerhalb von 90 Minuten übernommen. Wenn Sie die Komprimierung für Ihren CDN-Endpunkt zum ersten Mal einrichten, sollten Sie jedoch 1–2 Stunden warten, um sicherzugehen, dass die Komprimierungseinstellungen an alle POPs verteilt wurden. Erst danach lohnt sich ggf. eine Problembehandlung.

## Aktivieren der Komprimierung

> [AZURE.NOTE] Die CDN-Tarife „Standard“ und „Premium“ bieten die gleiche Komprimierungsmethode, jedoch mit einer anderen Benutzeroberfläche. Weitere Informationen zu den Unterschieden zwischen den CDN-Tarifen „Standard“ und „Premium“ finden Sie unter [Übersicht über Azure CDN](cdn-overview.md).

### Standard-Tarif

> [AZURE.NOTE] Dieser Abschnitt gilt für **Azure CDN Standard von Verizon**- und **Azure CDN Standard von Akamai**-Profile.

1. Klicken Sie auf dem Blatt „CDN-Profil“ auf den CDN-Endpunkt, den Sie verwalten möchten.

	![Blade-Endpunkte für CDN-Profile](./media/cdn-file-compression/cdn-endpoints.png)

	Das Blatt „CDN-Endpunkt“ öffnet sich.

2. Klicken Sie auf die Schaltfläche **Konfigurieren**.

	![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-file-compression/cdn-config-btn.png)

	Es wird das Blatt „CDN-Konfiguration“ geöffnet.

3. Schalten Sie **Komprimierung** an.

	![CDN-Komprimierungsoptionen](./media/cdn-file-compression/cdn-compress-standard.png)

4. Verwenden Sie die Standardtypen, oder ändern Sie die Liste durch Entfernen oder Hinzufügen von Dateitypen.
	
	> [AZURE.TIP] Obwohl es möglich ist, die Komprimierung für komprimierte Formate wie ZIP, MP3, MP4, JPG usw. zu aktivieren, wird dies nicht empfohlen.
	
5. Klicken Sie auf die Schaltfläche **Speichern**, wenn Sie die gewünschten Änderungen vorgenommen haben.

### Premium-Tarif

> [AZURE.NOTE] Dieser Abschnitt gilt für **Azure CDN Premium von Verizon**-Profile.

1. Klicken Sie auf dem Blatt „CDN-Profil“ auf die Schaltfläche **Verwalten**.

	![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-file-compression/cdn-manage-btn.png)

	Das CDN-Verwaltungsportal wird geöffnet.

2. Zeigen Sie auf die Registerkarte **HTTP Groß** und dann auf das Flyout **Cacheeinstellungen**. Klicken Sie auf **Komprimierung**.

	Es werden die Kompressionsoptionen angezeigt.

	![Dateikomprimierung](./media/cdn-file-compression/cdn-compress-files.png)

3. Aktivieren Sie die Komprimierung, indem Sie auf das Optionsfeld **Komprimierung aktiviert** klicken. Geben Sie im Textfeld **Dateitypen** die zu komprimierenden MIME-Typen als durch Trennzeichen getrennte Liste (ohne Leerzeichen) an.
		
	> [AZURE.TIP] Obwohl es möglich ist, die Komprimierung für komprimierte Formate wie ZIP, MP3, MP4, JPG usw. zu aktivieren, wird dies nicht empfohlen.

4. Wenn Sie die gewünschten Änderungen vorgenommen haben, klicken Sie auf die Schaltfläche **Aktualisieren**,


## Komprimierungsregeln

Diese Tabellen beschreiben das Verhalten der Azure CDN-Komprimierung für jedes Szenario.

> [AZURE.IMPORTANT] Für **Azure CDN von Verizon** (Standard und Premium) werden nur zugelassene Dateien komprimiert. Eine Datei ist für die Komprimierung geeignet, wenn sie folgende Bedingungen erfüllt:
>
> - Größer als 128 Bytes
> - Kleiner als 1 MB
> 
> Für **Azure CDN von Akamai** sind alle Dateien zur Komprimierung zugelassen.
>
> Für alle Azure CDN-Produkte muss eine Datei ein MIME-Typ sein, der [für die Komprimierung konfiguriert](#enabling-compression) wurde.
>
> **Azure CDN von Verizon**-Profile (Standard und Premium) unterstützen **gzip**-, **deflate**- oder **bzip2**-Codierung. **Azure CDN von Akamai**-Profile unterstützen nur **gzip**-Codierung.
>
> **Azure CDN von Akamai**-Endpunkte fordern unabhängig von der Clientanforderung stets **gzip**-codierte Dateien beim Ursprung an.

### Komprimierung deaktiviert oder Datei ist nicht für die Komprimierung geeignet

|Vom Client angefordertes Format (über Accept-Encoding-Header)|Zwischengespeichertes Dateiformat|CDN-Antwort an den Client|Hinweise|
|----------------|-----------|------------|-----|
|Komprimiert|Komprimiert|Komprimiert| |
|Komprimiert|Nicht komprimiert|Nicht komprimiert| |	
|Komprimiert|Nicht zwischengespeichert|Komprimiert oder nicht komprimiert|Abhängig von der Reaktion des Ursprungsservers|
|Nicht komprimiert|Komprimiert|Nicht komprimiert| |
|Nicht komprimiert|Nicht komprimiert|Nicht komprimiert| |	
|Nicht komprimiert|Nicht zwischengespeichert|Nicht komprimiert| |

### Komprimierung aktiviert und Datei zur Komprimierung geeignet

|Vom Client angefordertes Format (über Accept-Encoding-Header)|Zwischengespeichertes Dateiformat|CDN-Antwort an den Client|Hinweise|
|----------------|-----------|------------|-----|
|Komprimiert|Komprimiert|Komprimiert|CDN transcodiert zwischen unterstützten Formaten|
|Komprimiert|Nicht komprimiert|Komprimiert|CDN führt die Komprimierung durch|
|Komprimiert|Nicht zwischengespeichert|Komprimiert|CDN führt die Komprimierung durch, wenn der Ursprungsserver die nicht komprimierte Datei zurückgibt. **Azure CDN von Verizon** übergibt die nicht komprimierte Datei bei der ersten Anforderung, komprimiert anschließend die Datei und speichert sie für nachfolgende Anforderungen zwischen. Dateien mit `Cache-Control: no-cache`-Header werden nie komprimiert. 
|Nicht komprimiert|Komprimiert|Nicht komprimiert|CDN führt Dekomprimierung durch|
|Nicht komprimiert|Nicht komprimiert|Nicht komprimiert| |	
|Nicht komprimiert|Nicht zwischengespeichert|Nicht komprimiert| |

## Media Services-CDN-Komprimierung

Für Media Services CDN-aktivierte Streamingendpunkte ist die Komprimierung für die folgenden Inhaltstypen standardmäßig aktiviert: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl,application/f4m+xml. Sie können die Komprimierung für die genannten Typen im Azure-Portal nicht aktivieren/deaktivieren.

## Weitere Informationen
- [Problembehandlung bei der CDN-Dateikomprimierung](cdn-troubleshoot-compression.md)

<!---HONumber=AcomDC_0803_2016-->