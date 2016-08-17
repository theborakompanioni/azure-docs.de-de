<properties
	pageTitle="Steuern des Azure CDN-Zwischenspeicherverhaltens von Anforderungen mit Abfragezeichenfolgen | Microsoft Azure"
	description="Das Zwischenspeichern von Azure CDN-Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn diese Abfragezeichenfolgen enthalten."
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

#Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Azure CDN Premium von Verizon](cdn-query-string-premium.md)

##Übersicht

Das Zwischenspeichern von Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn sie Abfragezeichenfolgen enthalten.

> [AZURE.IMPORTANT] Die CDN-Produkte „Standard“ und „Premium“ bieten die gleiche Funktionalität zum Zwischenspeichern von Abfragezeichenfolgen, jedoch mit einer anderen Benutzeroberfläche. Dieses Dokument beschreibt die Benutzeroberfläche für **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon**. Informationen zum Zwischenspeichern von Abfragezeichenfolgen mit **Azure CDN Premium von Verizon** finden Sie unter [Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen – Premium](cdn-query-string-premium.md).

Die folgenden drei Modi sind verfügbar:

- **Abfragezeichenfolgen ignorieren**: Dies ist der Standardmodus. Der CDN-Edgeknoten übergibt die Abfragezeichenfolge bei der ersten Anforderung vom Anforderer an den Ursprung und speichert das Objekt im Cache. Alle nachfolgenden Anforderungen dieses Objekts, die vom Edgeknoten verarbeitet werden, ignorieren die Abfragezeichenfolge bis zum Ablauf des zwischengespeicherten Objekts.
- **Cachingumgehung für URLs mit Abfragezeichenfolgen**: In diesem Modus werden Anforderungen mit Abfragezeichenfolgen nicht auf dem CDN-Edgeknoten zwischengespeichert. Der Edgeknoten ruft das Objekt direkt vom Ursprung ab und übergibt es bei jeder Anforderung an den Anforderer.
- **Cache für eindeutige URL**: In diesem Modus wird jede Anforderung mit einer Abfragezeichenfolge als eindeutiges Objekt mit eigenem Cache behandelt. So wird z. B. die Antwort vom Ursprung für eine Anforderung für *foo.ashx?q=bar* auf dem Edgeknoten zwischengespeichert und für nachfolgende Caches mit der gleichen Abfragezeichenfolge zurückgegeben. Eine Anforderung für *foo.ashx?q=etwasanderes* wird als separates Asset mit eigener Lebensdauer zwischengespeichert.

##Ändern der Einstellungen für das Zwischenspeichern von Abfragezeichenfolgen für CDN-Standardprofile

1. Klicken Sie auf dem Blatt „CDN-Profil“ auf den CDN-Endpunkt, den Sie verwalten möchten.

	![Blade-Endpunkte für CDN-Profile](./media/cdn-query-string/cdn-endpoints.png)

	Das Blatt „CDN-Endpunkt“ öffnet sich.

2. Klicken Sie auf die Schaltfläche **Konfigurieren**.

	![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-query-string/cdn-config-btn.png)

	Es wird das Blatt „CDN-Konfiguration“ geöffnet.

3. Wählen Sie eine Einstellung aus der Dropdown-Liste **Verhalten beim Zwischenspeichern von Abfragezeichenfolgen**.

	![Zwischenspeicherungsoptionen für CDN-Abfragezeichenfolgen](./media/cdn-query-string/cdn-query-string.png)

4. Treffen Sie Ihre Auswahl, und klicken Sie auf **Speichern**.

> [AZURE.IMPORTANT] Diese Einstellungsänderungen sind vielleicht nicht sofort sichtbar, da die Verteilung der Registrierung über das CDN eine Weile dauern kann. Bei <b>Azure CDN von Akamai</b>-Profilen ist die Weitergabe in der Regel innerhalb einer Minute abgeschlossen. Bei <b>Azure CDN von Verizon</b>-Profilen ist die Weitergabe in der Regel in 90 Minuten abgeschlossen, in manchen Fällen kann es aber länger dauern.

<!---HONumber=AcomDC_0803_2016-->