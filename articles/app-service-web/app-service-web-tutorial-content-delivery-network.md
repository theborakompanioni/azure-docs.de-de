---
title: "Hinzufügen eines Content Delivery Network (CDN) zu einer Azure App Service-Instanz | Microsoft-Dokumentation"
description: "Fügen Sie einer Azure App Service-Instanz ein Content Delivery Network (CDN) hinzu, um Ihre statischen Dateien über Server zwischenzuspeichern und bereitzustellen, die von Ihren weltweiten Kunden jeweils nicht weit entfernt sind."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 05/01/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 7208abc0e6eaa9067c5bb36a09e1bfd276fe0b0c
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---
# <a name="add-a-content-delivery-network-cdn-to-an-azure-app-service"></a>Hinzufügen eines Content Delivery Network (CDN) zu einer Azure App Service-Instanz

Das [Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) speichert statische Webinhalte an strategisch platzierten Standorten zwischen, um beim Bereitstellen von Inhalten für Benutzer einen maximalen Durchsatz zu ermöglichen. Durch das CDN wird auch die Serverlast auf Ihrer Web-App verringert. In diesem Tutorial wird veranschaulicht, wie Sie Azure CDN einer [Web-App in Azure App Service](app-service-web-overview.md) hinzufügen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines CDN-Endpunkts
> * Aktualisieren von zwischengespeicherten Objekten
> * Verwenden von Abfragezeichenfolgen zum Steuern von zwischengespeicherten Versionen
> * Verwenden einer benutzerdefinierten Domäne für den CDN-Endpunkt

Hier ist die Startseite der statischen HTML-Beispielwebsite dargestellt, mit der Sie arbeiten:

![Startseite der Beispiel-App](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

## <a name="create-the-web-app"></a>Erstellen der Web-App

Befolgen Sie zum Erstellen der Web-App, die Sie verwenden werden, die Anleitung unter [Erstellen einer statischen HTML-Web-App in Azure in fünf Minuten](app-service-web-get-started-html.md), aber führen Sie dabei nicht den Schritt **Bereinigen von Ressourcen** aus.

Lassen Sie die Befehlseingabeaufforderung nach Abschluss des Tutorials geöffnet, damit Sie später im Rahmen dieses Tutorials weitere Änderungen für die Web-App bereitstellen können.

### <a name="have-a-custom-domain-ready"></a>Bereithalten einer benutzerdefinierten Domäne

Zum Ausführen des Schritts für die benutzerdefinierte Domäne in diesem Tutorial benötigen Sie Zugriff auf Ihre DNS-Registrierung für Ihren Domänenanbieter (z.B. GoDaddy). Um beispielsweise DNS-Einträge für `contoso.com` und `www.contoso.com` hinzuzufügen, benötigen Sie Zugriff für die Konfiguration der DNS-Einstellungen für die Stammdomäne `contoso.com`.

Wenn Sie nicht bereits einen Domänennamen besitzen, können Sie beispielsweise das [Tutorial zur App Service-Domäne](custom-dns-web-site-buydomains-web-app.md) absolvieren, um eine Domäne über das Azure-Portal zu erwerben. 

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Öffnen Sie einen Browser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>Erstellen eines CDN-Profils und -Endpunkts

Wählen Sie im Navigationsbereich auf der linken Seite die Option **App Services** und dann die App aus, die Sie unter [Erstellen einer statischen HTML-Web-App in Azure in fünf Minuten](app-service-web-get-started-html.md) erstellt haben.

![Auswählen der App Service-App im Portal](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

Wählen Sie auf der Seite **App Service** im Abschnitt **Einstellungen** die Option **Netzwerk > Azure CDN für Ihre App konfigurieren**.

![Auswählen des CDN im Portal](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

Geben Sie auf der Seite **Azure Content Delivery Network** die Einstellungen für **Neuer Endpunkt** gemäß den Angaben in der Tabelle an.

![Erstellen des Profils und Endpunkts im Portal](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Einstellung | Empfohlener Wert | Beschreibung |
| ------- | --------------- | ----------- |
| **CDN-Profil** | myCDNProfile | Wählen Sie die Option **Neu erstellen**, um ein neues CDN-Profil zu erstellen. Ein CDN-Profil ist eine Sammlung von CDN-Endpunkten mit demselben Tarif. |
| **Tarif** | Standard Akamai | Über den [Tarif](../cdn/cdn-overview.md#azure-cdn-features) werden der Anbieter und die verfügbaren Features angegeben. In diesem Tutorial verwenden wir Standard Akamai. |
| **CDN-Endpunktname** | Beliebiger Name, der in der Domäne „azureedge.net“ eindeutig ist | Sie greifen auf Ihre zwischengespeicherten Ressourcen in der Domäne *\<Endpunktname>.azureedge.net* zu.

Klicken Sie auf **Erstellen**.

Azure erstellt das Profil und den Endpunkt. Der neue Endpunkt wird in der Liste **Endpunkte** auf derselben Seite angezeigt. Nach seiner Bereitstellung lautet der Status **Wird ausgeführt**.

![Neuer Endpunkt in der Liste](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a>Testen des CDN-Endpunkts

Wenn Sie den Tarif „Verizon“ gewählt haben, dauert die Endpunktverteilung normalerweise etwa 90 Minuten. Für Akamai dauert die Verteilung nur wenige Minuten.

Die Beispiel-App enthält die Datei `index.html` und die Ordner *css*, *img* und *js* mit anderen statischen Objekten. Die Inhaltspfade für alle diese Dateien sind auf dem CDN-Endpunkt gleich. Beispielsweise wird mit beiden folgenden URLs auf die Datei *bootstrap.css* im Ordner *css* zugegriffen:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Navigieren Sie in einem Browser zur unten angegebenen URL. Sie gelangen auf dieselbe Seite, die Sie zuvor in einer Azure-Web-App ausgeführt haben, aber nun wird sie über das CDN bereitgestellt.

```
http://<endpointname>.azureedge.net/index.html
```

![Startseite der Beispiel-App – Bereitstellung per CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

Dies verdeutlicht, dass Azure CDN die Originalobjekte der Web-App abgerufen hat und sie über den CDN-Endpunkt bereitstellt. 

Aktualisieren Sie die Seite, um sicherzustellen, dass diese Seite im CDN zwischengespeichert ist. Es kann sein, dass für ein Objekt zwei Anforderungen erforderlich sind, damit der angeforderte Inhalt vom CDN zwischengespeichert wird.

Weitere Informationen zum Erstellen von Azure CDN-Profilen und -Endpunkten finden Sie unter [Erste Schritte mit Azure CDN](../cdn/cdn-create-new-endpoint.md).

## <a name="purge-the-cdn"></a>Bereinigen des CDN

Das CDN aktualisiert basierend auf der Gültigkeitsdauer (Time To Live, TTL) in regelmäßigen Abständen seine Ressourcen der ursprünglichen Web-App. Die Standardgültigkeitsdauer beträgt sieben Tage.

Es kann vorkommen, dass Sie das CDN vor Ablauf der Gültigkeitsdauer aktualisieren müssen, z.B. beim Bereitstellen von aktualisiertem Inhalt für die Web-App. Sie können die CDN-Ressourcen manuell bereinigen, um eine Aktualisierung auszulösen. 

In diesem Abschnitt des Tutorials stellen Sie eine Änderung für die Web-App bereit und bereinigen das CDN, um für das CDN die Aktualisierung des Caches auszulösen.

### <a name="deploy-a-change-to-the-web-app"></a>Bereitstellen einer Änderung für die Web-App

Öffnen Sie die Datei `index.html`, und fügen Sie der Überschrift H1 den Zusatz „- V2“ hinzu. Dies ist im folgenden Beispiel dargestellt: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Übernehmen Sie die Änderung, und stellen Sie sie für die Web-App bereit.

```bash
git commit -am "version 2"
git push azure master
```

Navigieren Sie nach Abschluss der Bereitstellung zur Web-App-URL. Die Änderung wird angezeigt.

```
http://<appname>.azurewebsites.net/index.html
```

![„V2“ im Titel der Web-App](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Wenn Sie zur CDN-Endpunkt-URL der Startseite navigieren, ist die Änderung noch nicht zu sehen, weil die zwischengespeicherte Version im CDN noch nicht abgelaufen ist. 

```
http://<endpointname>.azureedge.net/index.html
```

![„V2“ nicht im Titel des CDN enthalten](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-the-cdn-in-the-portal"></a>Bereinigen des CDN im Portal

Bereinigen Sie das CDN, um für das CDN die Aktualisierung der zwischengespeicherten Version auszulösen.

Wählen Sie im Navigationsbereich links im Portal die Option **Ressourcengruppen** und dann die Ressourcengruppe aus, die Sie für Ihre Web-App (myResourceGroup) erstellt haben.

![Auswählen der Ressourcengruppe](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

Wählen Sie in der Liste mit den Ressourcen Ihren CDN-Endpunkt aus.

![Auswählen des Endpunkts](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

Klicken Sie oben auf der Seite **Endpunkt** auf **Bereinigen**.

![Auswählen von „Bereinigen“](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Geben Sie die Inhaltspfade ein, die Sie bereinigen möchten. Sie können einen vollständigen Dateipfad übergeben, um eine einzelne Datei zu bereinigen, oder ein Pfadsegment, um den gesamten Inhalt eines Ordners zu bereinigen und zu aktualisieren. Da Sie `index.html` geändert haben, sollten Sie sich vergewissern, dass dies einer der Pfade ist.

Wählen Sie am unteren Rand der Seite die Option **Bereinigen**.

![Seite „Bereinigen“](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a>Sicherstellen des aktualisierten Zustands für das CDN

Warten Sie, bis die Verarbeitung der Bereinigungsanforderung abgeschlossen ist. Dies dauert normalerweise einige Minuten. Wählen Sie zum Anzeigen des aktuellen Status oben auf der Seite das Glockensymbol. 

![Bereinigungsbenachrichtigung](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Navigieren Sie zur CDN-Endpunkt-URL für `index.html`. Der Zusatz „V2“, den Sie dem Titel der Startseite hinzugefügt haben, wird nun angezeigt. Dies bedeutet, dass der CDN-Cache aktualisiert wurde.

```
http://<endpointname>.azureedge.net/index.html
```

![„V2“ im Titel des CDN enthalten](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Weitere Informationen finden Sie unter [Löschen eines Azure CDN-Endpunkts](../cdn/cdn-purge-endpoint.md). 

## <a name="use-query-strings-to-version-content"></a>Verwenden von Abfragezeichenfolgen zum Versehen von Inhalt mit einer Version

Azure CDN verfügt über die folgenden Optionen für das Zwischenspeicherverhalten:

* Ignorieren von Abfragezeichenfolgen
* Umgehen der Zwischenspeicherung für Abfragezeichenfolgen
* Zwischenspeichern jeder eindeutigen URL 

Die erste Option ist die Standardeinstellung. Dies bedeutet, dass nur eine zwischengespeicherte Version eines Objekts vorhanden ist – unabhängig von der Abfragezeichenfolge, die in der URL für den Zugriff verwendet wird. 

In diesem Abschnitt des Tutorials ändern Sie das Zwischenspeicherverhalten, sodass jede eindeutige URL zwischengespeichert wird.

### <a name="change-the-cache-behavior"></a>Ändern des Cacheverhaltens

Wählen Sie im Azure-Portal auf der Seite **CDN-Endpunkt** die Option **Cache**.

Wählen Sie in der Dropdownliste **Verhalten für das Zwischenspeichern von Abfragezeichenfolgen** die Option **Jede eindeutige URL zwischenspeichern**.

Wählen Sie **Speichern** aus.

![Auswählen des Zwischenspeicherverhaltens für Abfragezeichenfolgen](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Sicherstellen der separaten Zwischenspeicherung eindeutiger URLs

Navigieren Sie in einem Browser zur Startseite des CDN-Endpunkts, aber binden Sie dabei eine Abfragezeichenfolge ein: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

Das CDN gibt den aktuellen Inhalt der Web-App zurück, der in der Überschrift den Zusatz „V2“ enthält. 

Aktualisieren Sie die Seite, um sicherzustellen, dass diese Seite im CDN zwischengespeichert ist. 

Öffnen Sie die Datei `index.html`, ändern Sie „V2“ in „V3“, und stellen Sie die Änderung bereit. 

```bash
git commit -am "version 3"
git push azure master
```

Navigieren Sie in einem Browser mit einer neuen Abfragezeichenfolge, z.B. `q=2`, zur CDN-Endpunkt-URL. Das CDN ruft die aktuelle Datei `index.html` ab und zeigt „V3“ an.  Wenn Sie dagegen mit der Abfragezeichenfolge `q=1` zum CDN-Endpunkt navigieren, wird „V2“ angezeigt.

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![„V3“ im Titel des CDN, Abfragezeichenfolge 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![„V2“ im Titel des CDN, Abfragezeichenfolge 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

In dieser Ausgabe ist zu sehen, dass jede Abfragezeichenfolge anders behandelt wird: „q=1“ wurde bereits verwendet, und der zwischengespeicherte Inhalt wird zurückgegeben (V2), aber „q=2“ ist neu, sodass der aktuelle Web-App-Inhalt abgerufen und zurückgegeben wird (V3).

Weitere Informationen finden Sie unter [Steuern des Azure CDN-Zwischenspeicherverhaltens mit Abfragezeichenfolgen](../cdn/cdn-query-string.md).

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a>Zuordnen einer benutzerdefinierten Domäne zu einem CDN-Endpunkt

Sie ordnen Ihre benutzerdefinierte Domäne Ihrem CDN-Endpunkt zu, indem Sie einen CNAME-Eintrag erstellen. Ein CNAME-Eintrag ist eine DNS-Funktion, die eine Quelldomäne einer Zieldomäne zuordnet. Beispielsweise können Sie `cdn.contoso.com` oder `static.contoso.com` der Adresse `contoso.azureedge.net` zuordnen.

Wenn Sie keine benutzerdefinierte Domäne besitzen, können Sie das [Tutorial zur App Service-Domäne](custom-dns-web-site-buydomains-web-app.md) durcharbeiten, um eine Domäne über das Azure-Portal zu erwerben. 

### <a name="find-the-hostname-to-use-with-the-cname"></a>Ermitteln des Hostnamens für die Verwendung mit dem CNAME-Eintrag

Stellen Sie im Azure-Portal auf der Seite **Endpunkt** sicher, dass im Navigationsbereich auf der linken Seite die Option **Übersicht** ausgewählt ist. Wählen Sie anschließend oben auf der Seite die Schaltfläche **+ Benutzerdefinierte Domäne**.

![Auswählen von „Benutzerdefinierte Domäne hinzufügen“](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

Auf der Seite **Benutzerdefinierte Domäne hinzufügen** wird der Endpunkthostname angezeigt, der für die Erstellung eines CNAME-Eintrags verwendet wird. Der Hostname wird von Ihrer CDN-Endpunkt-URL abgeleitet: **&lt;Endpunktname>.azureedge.net**. 

![Seite „Benutzerdefinierte Domäne hinzufügen“](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a>Konfigurieren des CNAME-Eintrags bei Ihrer Domänenregistrierungsstelle

Navigieren Sie zur Website der Domänenregistrierungsstelle, und suchen Sie den Abschnitt für die Erstellung von DNS-Einträgen. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.

Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Öffnen Sie ggf. die Seite mit erweiterten Einstellungen, und suchen Sie nach den Wörtern CNAME, Alias oder Unterdomänen.

Erstellen Sie einen neuen CNAME-Eintrag, mit dem Ihre gewählte Unterdomäne (z.B. **static** oder **cdn**) dem zuvor im Portal angezeigten **Endpunkthostnamen** zugeordnet wird. 

### <a name="enter-the-custom-domain-in-azure"></a>Eingeben der benutzerdefinierten Domäne in Azure

Kehren Sie zur Seite **Benutzerdefinierte Domänen hinzufügen** zurück, und geben Sie den Namen der benutzerdefinierten Domäne einschließlich Unterdomäne in das Dialogfeld ein. Geben Sie z. B. Folgendes ein: `cdn.contoso.com`.   
   
Azure überprüft, ob der CNAME-Eintrag für den eingegebenen Domänennamen vorhanden ist. Wenn der CNAME-Eintrag korrekt ist, wird Ihre benutzerdefinierte Domäne überprüft.

Es kann eine Weile dauern, bis der CNAME-Eintrag an alle Namensserver im Internet verteilt wurde. Wenn die Domäne nicht sofort erkannt wird, Sie aber sicher sind, dass der CNAME-Eintrag richtig ist, sollten Sie einige Minuten warten und die Domäne dann erneut überprüfen.

### <a name="test-the-custom-domain"></a>Testen der benutzerdefinierten Domäne

Navigieren Sie in einem Browser über Ihre benutzerdefinierte Domäne (z.B. `cdn.contoso.com/index.html`) zur Datei `index.html`, um sicherzustellen, dass das Ergebnis mit dem Ergebnis des direkten Zugriffs auf `<endpointname>azureedge.net/index.html` identisch ist.

![Startseite der Beispiel-App per URL der benutzerdefinierten Domäne](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

Weitere Informationen finden Sie unter [Zuordnen von CDN-Inhalt (Content Delivery Network) zu einer benutzerdefinierten Domäne](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines CDN-Endpunkts
> * Aktualisieren von zwischengespeicherten Objekten
> * Verwenden von Abfragezeichenfolgen zum Steuern von zwischengespeicherten Versionen
> * Verwenden einer benutzerdefinierten Domäne für den CDN-Endpunkt

Die folgenden Artikel enthalten Informationen zur Optimierung der CDN-Leistung.

> [!div class="nextstepaction"]
> [Verbessern der Leistung durch Komprimieren von Dateien in Azure CDN](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [Vorabladen von Assets auf einen Azure CDN-Endpunkt](../cdn/cdn-preload-endpoint.md)


