<properties
	pageTitle="CDN – Problembehandlung bei der Dateikomprimierung"
	description="Behandeln Sie Probleme mit der CDN-Dateikomprimierung."
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
	ms.date="04/28/2016" 
	ms.author="casoper"/>
    
# Problembehandlung bei der CDN-Dateikomprimierung

Dieser Artikel unterstützt Sie bei der Behandlung von Problemen mit der [CDN-Dateikomprimierung](cdn-improve-performance.md).

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Ihre Frage im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) stellen, um dort Hilfe von Azure-Experten zu erhalten. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und klicken Sie auf **Support erhalten**.

## Symptom

Die Komprimierung für Ihren Endpunkt ist aktiviert, die Dateien werden aber nicht komprimiert zurückgegeben.

## Ursache

Es gibt mehrere mögliche Ursachen, darunter folgende:

- Der angeforderte Inhalt eignet sich nicht zur Komprimierung.
- Die Komprimierung ist für den angeforderten Dateityp nicht aktiviert.
- Die HTTP-Anforderung enthielt keinen Header, um einen gültigen Komprimierungstyp anzufordern.

## Schritte zur Problembehandlung

> [AZURE.TIP] Genauso wie beim Bereitstellen neuer Endpunkte dauert es eine gewisse Zeit, bis Änderungen an der CDN-Konfiguration im gesamten Netzwerk verteilt sind. In den meisten Fällen werden Ihre Änderungen innerhalb von 90 Minuten übernommen. Wenn Sie die Komprimierung für Ihren CDN-Endpunkt zum ersten Mal einrichten, sollten Sie jedoch 1–2 Stunden warten, um sicherzugehen, dass die Komprimierungseinstellungen an alle POPs verteilt wurden.

### Überprüfen der Anforderung

Zuerst sollten Sie eine schnelle Integritätsprüfung der Anforderung durchführen. Sie können die [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) Ihres Browsers verwenden, um die Anforderungen anzuzeigen.

- Überprüfen Sie die Anforderung, die an die Endpunkt-URL gesendet wird (`<endpointname>.azureedge.net`), nicht Ihre ursprüngliche Anforderung.
- Überprüfen Sie, ob die Anforderung einen **Accept-Encoding**-Header enthält und ob der Wert für diesen Header **gzip**, **deflate** oder **bzip2** enthält.

![CDN-Anforderungsheader](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### Überprüfen Sie die Komprimierungseinstellungen (Standard-CDN-Profil)

> [AZURE.NOTE] Dieser Schritt gilt nur, wenn Ihr CDN-Profil sich im **Standard**-Tarif befindet.

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Endpunkt, und klicken Sie auf die Schaltfläche **Konfigurieren**.

- Überprüfen Sie, ob die Komprimierung aktiviert ist.
- Überprüfen Sie, ob der MIME-Typ für den zu komprimierenden Inhalt in der Liste der komprimierten Formate enthalten ist.

![CDN-Komprimierungseinstellungen](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### Überprüfen Sie die Komprimierungseinstellungen (Premium-CDN-Profil)

> [AZURE.NOTE] Dieser Schritt gilt nur, wenn Ihr CDN-Profil sich im **Premium**-Tarif befindet.

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Endpunkt, und klicken Sie auf die Schaltfläche **Verwalten**. Das zusätzliche Portal wird geöffnet. Zeigen Sie auf die Registerkarte **HTTP Groß** und dann auf das Flyout **Cacheeinstellungen**. Klicken Sie auf **Komprimierung**.

- Überprüfen Sie, ob die Komprimierung aktiviert ist.
- Überprüfen Sie, ob die Liste **Dateitypen** eine durch Trennzeichen getrennte Liste von MIME-Typen ohne Leerzeichen enthält.
- Überprüfen Sie, ob der MIME-Typ für den zu komprimierenden Inhalt in der Liste der komprimierten Formate enthalten ist.

![CDN-Komprimierungseinstellungen, Premium](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### Überprüfen, ob der Inhalt zwischengespeichert wird

Überprüfen Sie mithilfe der Entwicklertools Ihres Browsers die Antwortheader, um sicherzustellen, dass die Datei in der Region zwischengespeichert wird, in der sie angefordert wird.

- Überprüfen Sie den Antwortheader **Server**. Dieser Header sollte das Format **Plattform (POP-/Server-ID)** aufweisen, wie im Beispiel unten gezeigt.
- Überprüfen Sie den Antwortheader **X-Cache**. Dieser Header sollte **HIT** lauten.  

![CDN-Antwortheader](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### Überprüfen, ob die Datei die Größenanforderungen erfüllt

Um sich für die Komprimierung zu eignen, muss eine Datei folgende Größenanforderungen erfüllen:

- Größer als 128 Bytes.
- Kleiner als 1 MB.

<!---HONumber=AcomDC_0504_2016-->