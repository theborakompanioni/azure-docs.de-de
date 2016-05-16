<properties
	pageTitle="CDN – Verbessern der Leistung durch Komprimieren von Dateien"
	description="Sie können die Geschwindigkeit von Dateiübertragungen erhöhen und die Leistung beim Laden von Seiten verbessern, indem Sie Ihre Dateien komprimieren."
	services="cdn"
	documentationCenter=".NET"
	authors="camsoper"
	manager="erikre"
	editor=""/>

<tags
	ms.service="cdn"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016" 
	ms.author="casoper"/>

# Verbessern der Leistung durch Komprimieren von Dateien

In diesem Thema wird erläutert, wie Sie die Geschwindigkeit von Dateiübertragungen erhöhen und die Leistung beim Laden von Seiten verbessern, indem Sie Ihre Dateien komprimieren.

CDN bietet zwei Möglichkeiten zur Unterstützung der Komprimierung:

- Sie können die Komprimierung auf Ihrem Ursprungsserver aktivieren. In diesem Fall unterstützt CDN die Komprimierung standardmäßig und sendet komprimierte Dateien an Clients.
- Sie können die Komprimierung direkt auf CDN-Edgeservern aktivieren. In diesem Fall komprimiert CDN die Dateien und sendet sie an die Endbenutzer.

## Aktivieren der Komprimierung

> [AZURE.NOTE] Die CDN-Tarife „Standard“ und „Premium“ bieten die gleiche Komprimierungsmethode, jedoch mit einer anderen Benutzeroberfläche. Weitere Informationen zu den Unterschieden zwischen den CDN-Tarifen „Standard“ und „Premium“ finden Sie unter [Übersicht über Azure CDN](cdn-overview.md).

### Standard-Ebene

1. Klicken Sie auf dem Blatt „CDN-Profil“ auf den CDN-Endpunkt, den Sie verwalten möchten.

	![Blade-Endpunkte für CDN-Profile](./media/cdn-file-compression/cdn-endpoints.png)

	Das Blatt „CDN-Endpunkt“ öffnet sich.

2. Klicken Sie auf die Schaltfläche **Konfigurieren**.

	![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-file-compression/cdn-config-btn.png)

	Es wird das Blatt „CDN-Konfiguration“ geöffnet.

3. Schalten Sie **Komprimierung** an.

	![CDN-Komprimierungsoptionen](./media/cdn-file-compression/cdn-compress-standard.png)

4. Verwenden Sie die Standardtypen, oder ändern Sie die Liste durch Entfernen oder Hinzufügen von Dateitypen.

5. Klicken Sie auf die Schaltfläche **Speichern**, wenn Sie die gewünschten Änderungen vorgenommen haben.

### Premium-Ebene

1. Klicken Sie auf dem Blatt „CDN-Profil“ auf die Schaltfläche **Verwalten**.

	![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-file-compression/cdn-manage-btn.png)

	Das CDN-Verwaltungsportal wird geöffnet.

2. Zeigen Sie auf die Registerkarte **HTTP Groß** und dann auf das Flyout **Cacheeinstellungen**. Klicken Sie auf **Komprimierung**.

	Es werden die Kompressionsoptionen angezeigt.

	![Dateikomprimierung](./media/cdn-file-compression/cdn-compress-files.png)

3. Aktivieren Sie die Komprimierung, indem Sie auf das Optionsfeld **Komprimierung aktiviert** klicken. Geben Sie im Textfeld **Dateitypen** die zu komprimierenden MIME-Typen als durch Trennzeichen getrennte Liste (ohne Leerzeichen) an.

4. Wenn Sie die gewünschten Änderungen vorgenommen haben, klicken Sie auf die Schaltfläche **Aktualisieren**,


## Komprimierungsprozess

### Übersicht

1. Die anfordernde Person sendet eine Inhaltsanforderung.

2. Ein Edgeserver prüft, ob ein **Accept-Encoding**-Header vorhanden ist.
	1. Sofern vorhanden, gibt dieser Header die angeforderte Komprimierungsmethode an.
		> [AZURE.NOTE] Unterstützte Komprimierungsmethoden sind **gzip**, **deflate**, und **bzip2**.
	2. Fehlt der Header, wird die Anforderung in einem nicht komprimierten Format verarbeitet.
	
3.	Der nächstgelegene Edge-POP überprüft den Cachestatus sowie die Komprimierungsmethode und prüft, ob noch eine entsprechende Gültigkeitsdauer (TTL, Time to Live) vorliegt.
	1.	**Cachefehler** („Miss“): Wenn die angeforderte Version nicht zwischengespeichert ist, wird die Anforderung an den Ursprungsserver weitergeleitet.
	2.	**Cachetreffer** („Hit“): Wenn die angeforderte Version mit der angeforderten Komprimierungsmethode zwischengespeichert ist, sendet der Edgeserver den komprimierten Inhalt sofort an den Client.
	3.	**Cachetreffer** („Hit“) Wenn die Datei mit einer anderen Komprimierungsmethode zwischengespeichert wurde, transcodiert der Edgeserver die Ressource in die angeforderte Komprimierungsmethode.
	4.	**Cachetreffer** („Hit“): Wenn die Datei in einem nicht komprimierten Format zwischengespeichert wurde, wird geprüft, ob die Anforderung für die Komprimierung durch den Edgeserver geeignet ist. Sofern möglich, komprimiert der Edgeserver die Datei und sendet sie an den Client. Andernfalls wird der nicht komprimierte Inhalt zurückgegeben.
		
> [AZURE.IMPORTANT] Eine Datei ist für die Komprimierung geeignet, wenn sie folgende Bedingungen erfüllt:
>
> - Größer als 128 Bytes
> - Kleiner als 1 MB
> - Ein MIME-Typ, für den [die Komprimierung konfiguriert wurde](#enabling-compression)

### Tabellen

Die folgenden Tabellen beschreiben das Verhalten der CDN-Komprimierung für jedes Szenario.

#### Komprimierung deaktiviert oder Datei ist nicht für die Komprimierung geeignet

|Angefordertes Format|Zwischengespeicherte Datei|CDN-Reaktion|Hinweise|
|----------------|-----------|------------|-----|
|Komprimiert|Komprimiert|Komprimiert|CDN transcodiert zwischen unterstützten Formaten|
|Komprimiert|Nicht komprimiert|Nicht komprimiert| |	
|Komprimiert|Nicht zwischengespeichert|Komprimiert oder nicht komprimiert|Abhängig von der Reaktion des Ursprungsservers|
|Nicht komprimiert|Komprimiert|Nicht komprimiert|CDN kontaktiert Ursprungsserver, um die nicht komprimierte Version zu erhalten|
|Nicht komprimiert|Nicht komprimiert|Nicht komprimiert| |	
|Nicht komprimiert|Nicht zwischengespeichert|Nicht komprimiert| |

#### Komprimierung aktiviert und Datei zur Komprimierung geeignet

|Angefordertes Format|Zwischengespeicherte Datei|CDN-Reaktion|Hinweise|
|----------------|-----------|------------|-----|
|Komprimiert|Komprimiert|Komprimiert|CDN transcodiert zwischen unterstützten Formaten|
|Komprimiert|Nicht komprimiert|Komprimiert|CDN führt die Komprimierung durch|
|Komprimiert|Nicht zwischengespeichert|Komprimiert|CDN führt die Komprimierung durch, wenn der Ursprungsserver die nicht komprimierte Datei zurückgibt|
|Nicht komprimiert|Komprimiert|Nicht komprimiert|CDN führt Dekomprimierung durch|
|Nicht komprimiert|Nicht komprimiert|Nicht komprimiert| |	
|Nicht komprimiert|Nicht zwischengespeichert|Nicht komprimiert| |	


## Hinweise

1. Genauso wie beim Bereitstellen neuer Endpunkte dauert es eine gewisse Zeit, bis Änderungen an der CDN-Konfiguration im gesamten Netzwerk verteilt sind. In den meisten Fällen werden Ihre Änderungen innerhalb von 90 Minuten übernommen. Wenn Sie die Komprimierung für Ihren CDN-Endpunkt zum ersten Mal einrichten, sollten Sie jedoch 1–2 Stunden warten, um sicherzugehen, dass die Komprimierungseinstellungen an alle POPs verteilt wurden. Erst danach lohnt sich ggf. eine Problembehandlung.
2. Auf dem Edgeserver wird nur eine Dateiversion (komprimiert oder nicht komprimiert) zwischengespeichert. Die Anforderung einer anderen Dateiversion führt dazu, dass der Inhalt vom Edgeserver transcodiert wird.
3. Für Media Services CDN-aktivierte Streamingendpunkte ist die Komprimierung für die folgenden Inhaltstypen standardmäßig aktiviert: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl,application/f4m+xml. Sie können die Komprimierung für die genannten Typen im Azure-Portal nicht aktivieren/deaktivieren.  
4. Obwohl es möglich ist, die Komprimierung für komprimierte Formate wie ZIP, MP3, MP4, JPG usw. zu aktivieren, wird dies nicht empfohlen.

## Weitere Informationen
- [Problembehandlung bei der CDN-Dateikomprimierung](cdn-troubleshoot-compression.md)    

<!---HONumber=AcomDC_0504_2016-->