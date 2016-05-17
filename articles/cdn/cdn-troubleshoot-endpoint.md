<properties
	pageTitle="CDN – Problembehandlung bei CDN-Endpunkten mit Status 404"
	description="Problembehandlung beim Statuscode 404 bei CDN-Endpunkten"
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
    
# Problembehandlung bei CDN-Endpunkten mit Status 404

Dieser Artikel unterstützt Sie bei der Behandlung von Problemen mit [CDN-Endpunkten](cdn-create-new-endpoint.md), die 404-Fehler ausgeben.

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Ihre Frage im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) stellen, um dort Hilfe von Azure-Experten zu erhalten. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und klicken Sie auf **Support erhalten**.

## Symptom

Sie haben ein CDN-Profil und einen Endpunkt erstellt, Ihre Inhalte sind auf dem CDN jedoch anscheinend nicht verfügbar. Benutzer, die über die CDN-URL auf die Inhalte zugreifen möchten, erhalten den HTTP-Statuscode 404.

## Ursache

Es gibt mehrere mögliche Ursachen, darunter folgende:

- Den Ursprung der Datei ist für das CDN nicht sichtbar.
- Der Endpunkt ist falsch konfiguriert, weshalb das CDN an der falschen Stelle sucht.
- Der Host lehnt den Host-Header aus dem CDN ab.
- Der Endpunkt konnte noch nicht über das CDN verteilt werden.

## Schritte zur Problembehandlung

> [AZURE.IMPORTANT] Ein CDN-Endpunkt kann nach seiner Erstellung nicht sofort verwendet werden, da die Verteilung der Registrierung über das CDN eine Weile dauern kann. In der Regel ist er innerhalb von 90 Minuten verfügbar, in einigen Fällen kann es jedoch länger dauern. Wenn Sie die in diesem Dokument beschriebenen Schritte ausgeführt haben und dennoch weiterhin der Code 404 zurückgegeben wird, warten Sie unter Umständen ein paar Stunden, und versuchen Sie es dann erneut, bevor Sie ein Supportticket öffnen.

### Überprüfen der Ursprungsdatei

Überprüfen Sie zunächst, dass die Datei, die zwischengespeichert werden soll, im Ursprung verfügbar und öffentlich zugänglich ist. Das geht am schnellsten, wenn Sie eine InPrivate- oder Inkognito-Browsersitzung öffnen und die Datei direkt aufrufen. Wenn Sie die URL in das Adressfeld eingeben oder einfügen, können Sie prüfen,ob die gewünschte Datei angezeigt wird. In diesem Beispiel verwenden wir eine Datei, die in einem Azure-Speicherkonto gespeichert ist und unter `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt` abgerufen werden kann. Wie Sie sehen können, hat hier alles geklappt.

![Erfolg!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Dies ist die schnellste und einfachste Möglichkeit, um zu prüfen, ob Ihre Datei öffentlich zugänglich ist. Bei manchen Netzwerkkonfigurationen von Organisationen kann unter Umständen der Eindruck entstehen, dass die Datei öffentlich zugänglich ist, während sie jedoch nur Benutzern des Netzwerks angezeigt wird (selbst wenn sie auf Azure gehostet wird). Daher ist es am besten, wenn Sie den Test über einen externen Browser durchführen, z.B. über ein Mobilgerät, das nicht an das Netzwerk Ihrer Organisation angeschlossen ist, oder einen virtuellen Computer in Azure.

### Überprüfen der Ursprungseinstellungen

Nachdem wir geprüft haben, ob die Datei im Internet öffentlich verfügbar ist, sollten wir nun die Ursprungseinstellungen überprüfen. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem CDN-Profil, und klicken Sie auf den betreffenden Endpunkt. Klicken Sie auf dem Blatt **Endpunkt** auf den Ursprung.

![Endpunktblatt mit hervorgehobenem Ursprung](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Das Blatt **Ursprung** wird angezeigt.

![Ursprungsblatt](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### Ursprungstyp und Hostname

Stellen Sie sicher, dass der **Ursprungstyp** korrekt ist, und prüfen Sie auch den **Hostnamen des Ursprungs**. In unserem Beispiel, `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, ist `cdndocdemo.blob.core.windows.net` der Teil der URL, der den Hostnamen darstellt. Wie Sie im Screenshot sehen können, ist dies richtig. Wenn Azure Storage, Web-App oder Clouddienst als Ursprung ausgewählt ist, wird das Feld **Hostname des Ursprungs** als Dropdownliste angezeigt. So können keine Schreibfehler im Hostnamen entstehen. Bei einem benutzerdefinierten Ursprung muss der Hostname jedoch *unbedingt* richtig geschrieben sein.

#### HTTP- und HTTPS-Ports

Prüfen Sie als Nächstes Ihre **HTTP-** und **HTPS-Ports**. Die Einstellungen 80 und 443 stimmen meistens und erfordern keine Änderungen. Wenn der Ursprungsserver jedoch an einem anderen Port lauscht, muss dies hier angegeben werden. Wenn Sie sich in Bezug darauf nicht sicher sind, sehen Sie sich die URL Ihrer Ursprungsdatei an. Die HTTP- und HTTPS-Spezifikationen legen die Ports 80 und 443 als Standard fest. In unserer URL (`https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`) ist kein Port festgelegt. Daher wird der Standardport 443 vorausgesetzt. Unsere Einstellungen sind daher richtig.

Angenommen, die URL für die zuvor getestete Ursprungsdatei ist `http://www.contoso.com:8080/file.txt`. Am Ende des Hostnamensegments sehen Sie `:8080`. Dies weist den Browser an, Port `8080` für die Verbindung mit dem Webserver unter `www.contoso.com` zu verwenden. Geben Sie daher „8080“ im Feld **HTTP-Port** ein. Beachten Sie, dass sich diese Porteinstellungen nur darauf auswirken, welchen Port der Endpunkt zum Abrufen von Informationen vom Ursprung verwendet.
  
### Überprüfen der Endpunkteinstellungen

Klicken Sie auf dem Blatt **Endpunkt** auf die Schaltfläche **Konfigurieren**.

![Endpunktblatt mit hervorgehobener Schaltfläche „Konfigurieren“](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Das Blatt **Konfigurieren** des Endpunkts erscheint.

![Blatt „Konfigurieren“](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### Protokolle

Prüfen Sie für die **Protokolle**, dass das von den Clients verwendete Protokoll ausgewählt ist. Das vom Client verwendete Protokoll wird für den Zugriff auf den Ursprung genutzt. Daher müssen die Ursprungsports wie im letzten Abschnitt beschrieben richtig konfiguriert sein. Der Endpunkt lauscht nur an den Standard-HTTP- und -HTTPS-Ports (80 und 443), unabhängig von den Ursprungsports.

Kehren wir nun zu unserem Beispiel (`http://www.contoso.com:8080/file.txt`) zurück. Wie Sie wissen hat Contoso `8080` als HTTP-Port festgelegt. Nehmen wir nun einmal an, dass `44300` als HTTPS-Port festgelegt wurde. Wenn ein Endpunkt namens `contoso` erstellt wurde, ist `contoso.azureedge.net` der CDN-Endpunkt-Hostname. Eine Anforderung für `http://contoso.azureedge.net/file.txt` ist eine HTTP-Anforderung. Somit verwendet der Endpunkt HTTP auf Port 8080 für den Abruf vom Ursprung. Bei einer sicheren Anforderung über HTTPS (`https://contoso.azureedge.net/file.txt`) verwendet der Endpunkt HTTPS auf Port 44300 beim Abruf der Datei vom Ursprung.

#### Header des Ursprungshosts

Der **Header des Ursprungshosts** ist der Hostheaderwert, der bei jeder Anforderung an den Ursprung übermittelt wird. In den meisten Fällen entspricht dieser Wert dem **Ursprungshostnamen**, den wir zuvor bereits geprüft haben. Wenn in diesem Feld ein falscher Wert eingegeben ist, wird in der Regel kein 404-Status ausgegeben. Je nachdem, was der Ursprung erwartet, werden wahrscheinlich jedoch andere 4xx-Statuscodes zurückgegeben werden.

#### Ursprungspfad

Als Letztes sollte noch der **Ursprungspfad** geprüft werden. Standardmäßig ist dieses Feld leer. Verwenden Sie dieses Feld nur, wenn Sie den Umfang der Ressourcen vom Ursprungshost, die auf dem CDN verfügbar sein sollen, einschränken möchten.

In unserem Endpunkt sollen beispielsweise alle Ressourcen im Speicherkonto verfügbar sein, weshalb ich das Feld **Ursprungspfad** leer gelassen habe. Bei einer Anforderung an `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` wird somit vom Endpunkt eine Verbindung mit `cdndocdemo.core.windows.net` mit der Anforderung `/publicblob/lorem.txt` hergestellt. Dementsprechend fordert der Endpunkt bei einer Anforderung an `https://cdndocdemo.azureedge.net/donotcache/status.png` vom Ursprung `/donotcache/status.png` an.

Aber wie gehe ich vor, wenn das CDN nicht für jeden Ursprungspfad verwendet werden soll? Angenommen, ich möchte nur den Pfad `publicblob` bereitstellen. Durch Eingabe von */publicblob* im Feld **Ursprungspfad** fügt der Endpunkt vor jeder Anforderung an den Ursprung */publicblob* ein. Dadurch wird `/publicblob` bei jeder Anforderung an `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` vor dem Anforderungsteil der URL (`/publicblob/lorem.txt`) eingefügt. Somit wird eine Anforderung für `/publicblob/publicblob/lorem.txt` vom Ursprung erstellt. Wenn dieser Pfad zu keiner Datei führt, gibt der Ursprung den Status 404 zurück. Die richtige URL zum Abruf von „lorem.txt“ wäre in diesem Beispiel `https://cdndocdemo.azureedge.net/lorem.txt`. Wie Sie sehen, ist der Pfad */publicblob* hier nicht angegeben, da der Anforderungsteil der URL `/lorem.txt` lautet und der Endpunkt `/publicblob` hinzufügt, wodurch `/publicblob/lorem.txt` die Anforderung ist, die an den Ursprung weitergegeben wird.

<!---HONumber=AcomDC_0504_2016-->