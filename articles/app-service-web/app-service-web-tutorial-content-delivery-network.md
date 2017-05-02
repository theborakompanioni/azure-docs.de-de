---
title: "Hinzufügen eines Content Delivery Network zu einer Azure App Service-Instanz | Microsoft-Dokumentation"
description: "Fügen Sie ein Content Delivery Network einer Azure App Service-Instanz hinzu, um Ihre statischen Dateien über Edgeknoten bereitzustellen."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 7ba3737566401152a3171e8926beca188045230c
ms.lasthandoff: 04/22/2017

---
# <a name="add-a-content-deliver-network-on-an-azure-app-service"></a>Hinzufügen eines Content Delivery Network zu einer Azure App Service-Instanz

In diesem Tutorial fügen Sie Ihrer Azure App Service-Instanz ein Content Delivery Network (CDN) hinzu, um den statischen Inhalt auf einem Edgeserver verfügbar zu machen. Sie erstellen ein CDN-Profil. Hierbei handelt es sich um eine Sammlung mit bis zu zehn CDN-Endpunkten.

In einem Content Delivery Network werden statische Webinhalte an strategisch platzierten Standorten zwischengespeichert, um beim Bereitstellen von Inhalten für Benutzer einen maximalen Durchsatz zu ermöglichen. Die Verwendung des CDN zum Zwischenspeichern von Websiteobjekten bietet folgende Vorteile:

* Bessere Leistung und höhere Benutzerfreundlichkeit für Endbenutzer, vor allem bei Verwendung von Anwendungen, für die zum Laden von Inhalten mehrere Roundtrips erforderlich sind
* Umfassende Skalierung, um hohe Lasten zu Beginn eines Ereignisses, z.B. bei einer Produkteinführung, besser verarbeiten zu können
* Durch das Verteilen der Benutzeranforderungen und die Bereitstellung von Inhalten über Edgeserver entfällt weniger Datenverkehr auf den Ursprung der Inhalte

> [!TIP]
> Sehen Sie sich unter [Azure CDN-POP-Standorte](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations) die aktuelle Liste an.
>

## <a name="deploy-the-sample"></a>Bereitstellen des Beispiels

Zum Durcharbeiten dieses Tutorials benötigen Sie eine Anwendung, die für die Web-App bereitgestellt wird. Befolgen Sie die Schritte des Schnellstarts unter [Erstellen einer statischen HTML-Web-App in Azure in fünf Minuten](app-service-web-get-started-html.md), um eine Basis für dieses Tutorial zu schaffen.

## <a name="step-1---login-to-azure-portal"></a>Schritt 1: Anmelden beim Azure-Portal

Navigieren Sie in Ihrem bevorzugten Browser zum [Azure-Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Schritt 2: Erstellen eines CDN-Profils

Klicken Sie im linken Navigationsbereich auf die Schaltfläche `+ New` und anschließend auf **Web und mobil**. Wählen Sie unter der Kategorie „Web und mobil“ die Option **CDN** aus.

Füllen Sie die folgenden Felder aus:

| Feld | Beispielwert | Beschreibung |
|---|---|---|
| Name | myCDNProfile | Ein Name für das CDN-Profil. |
| Standort | Westeuropa | Dies ist der Azure-Speicherort, an dem Ihre CDN-Profilinformationen gespeichert werden. Dies hat keine Auswirkung auf die Speicherorte von CDN-Endpunkten. |
| Ressourcengruppe | myResourceGroup | Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups). |
| Tarif  | Standard Akamai | Einen Vergleich der Tarife finden Sie unter [Übersicht über CDN](../cdn/cdn-overview.md#azure-cdn-features). |

Klicken Sie auf **Erstellen**.

Öffnen Sie im linken Navigationsbereich den Ressourcengruppenhub, und wählen Sie **myResourceGroup** aus. Wählen Sie in der Ressourcenliste die Option **myCDNProfile** aus.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Schritt 3: Erstellen eines CDN-Endpunkts

Klicken Sie in den Befehlen neben dem Suchfeld auf **+ Endpunkt**, um das Blatt für die Endpunkterstellung zu öffnen.

Füllen Sie die folgenden Felder aus:

| Feld | Beispielwert | Beschreibung |
|---|---|
| Name |  | Dieser Name wird für den Zugriff auf die zwischengespeicherten Ressourcen in der Domäne `<endpointname>.azureedge.net` verwendet. |
| Ursprungstyp | Web App | Durch die Auswahl eines Ursprungstyps erhalten Sie Kontextmenüs für die restlichen Felder. Wenn Sie den benutzerdefinierten Ursprung wählen, wird ein Textfeld für den Hostnamen des Ursprungs angezeigt. |
| Hostname des Ursprungs | |  In der Dropdownliste werden alle verfügbaren Ursprünge des von Ihnen angegebenen Ursprungstyps aufgeführt. Bei Auswahl von „Benutzerdefinierter Ursprung“ als Ursprungstyp geben Sie die Domäne Ihres benutzerdefinierten Ursprungs ein.  |

Klicken Sie auf **Hinzufügen**.

Der Content Delivery Network-Endpunkt wird erstellt. Nach Abschluss dieses Vorgangs wird der Status auf **Wird ausgeführt** aktualisiert.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---serve-from-azure-cdn"></a>Schritt 4: Bereitstellen per Azure CDN

Nachdem der CDN-Endpunkt nun **ausgeführt** wird, sollten Sie über den CDN-Endpunkt auf den Inhalt zugreifen können.

Da wir den Schnellstart unter [Erstellen einer statischen HTML-Web-App in Azure in fünf Minuten](app-service-web-get-started-html.md) als Basis für dieses Tutorial verwendet haben, sollten in unserem CDN die folgenden Ordner verfügbar sein: `css`, `img` und `js`.

Die Inhaltspfade zwischen der Web-App-URL `http://<app_name>.azurewebsites.net/img/` und der CDN-Endpunkt-URL `http://<endpointname>.azureedge.net/img/` sind gleich. Dies bedeutet, dass Sie die CDN-Endpunktdomäne einfach gegen jeglichen statischen Inhalt austauschen können, der über das CDN bereitgestellt werden soll.

Wir rufen nun unser erstes Bild vom CDN-Endpunkt ab, indem wir in unserem bevorzugten Webbrowser zur folgenden URL navigieren:

```bash
http://<endpointname>.azureedge.net/img/03-enterprise.png
```

Nachdem der statische Inhalt jetzt per CDN verfügbar ist, können Sie Ihre Anwendung aktualisieren, um den CDN-Endpunkt zum Bereitstellen des Inhalts für Endbenutzer zu nutzen.

Je nach der Sprache, mit der Ihre Website erstellt wurde, sind ggf. viele Frameworks zur Unterstützung des CDN-Fallbacks vorhanden. ASP.NET verfügt beispielsweise über Unterstützung für die [Bündelung und Minimierung](https://docs.microsoft.com/en-us/aspnet/mvc/overview/performance/bundling-and-minification#using-a-cdn), bei der auch CDN-Fallbackfunktionen möglich sind.

Falls Ihre Sprache keine integrierte Unterstützung bzw. Bibliothek für die CDN-Fallbackunterstützung aufweist, können Sie ein JavaScript-Framework wie [FallbackJS](http://fallback.io/) verwenden, mit dem das Laden von [Skripts](https://github.com/dolox/fallback/tree/master/examples/loading-scripts), [Stylesheets](https://github.com/dolox/fallback/tree/master/examples/loading-stylesheets) und [Bildern](https://github.com/dolox/fallback/tree/master/examples/loading-images) unterstützt wird.

## <a name="step-5---purge-the-cdn"></a>Schritt 5: Bereinigen des CDN

In bestimmten Fällen kann es erforderlich sein, eine Bereinigung des CDN zu erzwingen, wenn der Inhalt vor Ablauf der Gültigkeitsdauer (Time-To-Live, TTL) ablaufen soll.

Es ist möglich, das Azure CDN manuell zu bereinigen, und zwar entweder über das Blatt „CDN-Profil“ oder „CDN-Endpunkt“. Wenn Sie auf der Seite „Profil“ die Bereinigung wählen, müssen Sie auswählen, welcher Endpunkt bereinigt werden soll.

Geben Sie zum Bereinigen von Inhalt die entsprechenden Inhaltspfade ein. Sie können einen vollständigen Dateipfad übergeben, um eine einzelne Datei zu bereinigen, oder ein Pfadsegment, um den Inhalt eines bestimmten Ordners zu bereinigen und zu aktualisieren.

Klicken Sie auf **Bereinigen**, nachdem Sie alle Inhaltspfade angegeben haben, die bereinigt werden sollen.

![app-service-web-purge-cdn](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

## <a name="step-6---map-a-custom-domain"></a>Schritt 6: Zuordnen einer benutzerdefinierten Domäne

Beim Zuordnen einer benutzerdefinierten Domäne zu Ihrem CDN-Endpunkt wird eine einheitliche Domäne für Ihre Webanwendung bereitgestellt.

Erstellen Sie einen CNAME-Eintrag bei Ihrer Domänenregistrierungsstelle, um Ihrem CDN-Endpunkt eine benutzerdefinierte Domäne zuzuordnen.

> [!NOTE]
> Ein CNAME-Eintrag ist ein DNS-Feature, das eine Quelldomäne wie `www.contosocdn.com` oder `static.contosocdn.com` einer Zieldomäne zuordnet.

Hier fügen wir die Quelldomäne `static.contosocdn.com` hinzu. Sie zeigt auf die Zieldomäne, also auf den CDN-Endpunkt.

| Quelldomäne | Zieldomäne |
|---|---|
| static.contosocdn.com | &lt;Endpunktname&gt;.azureedge.net |

Klicken Sie auf dem Blatt mit der CDN-Endpunktübersicht auf die Schaltfläche `+ Custom domain`.

Geben Sie auf dem Blatt „Benutzerdefinierte Domäne hinzufügen“ Ihre benutzerdefinierte Domäne und die Unterdomäne in das Dialogfeld ein. Geben Sie beispielsweise den Domänennamen im Format `static.contosocdn.com` ein.

Klicken Sie auf **Hinzufügen**.

## <a name="step-7---version-content"></a>Schritt 7: Versionsinhalt

Wählen Sie im linken Navigationsbereich des CDN-Endpunkts unter der Überschrift „Einstellungen“ die Option **Cache**.

Auf dem Blatt **Cache** können Sie konfigurieren, wie das CDN Abfragezeichenfolgen in der Anforderung behandelt.

> [!NOTE]
> Eine Beschreibung der Optionen des Zwischenspeicherverhaltens von Abfragezeichenfolgen finden Sie im Thema [Steuern des Azure CDN-Zwischenspeicherverhaltens mit Abfragezeichenfolgen](../cdn/cdn-query-string.md).

Wählen Sie in der Dropdownliste für das Verhalten der Zwischenspeicherung von Abfragezeichenfolgen die Option **Jede eindeutige URL zwischenspeichern**.

Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

* [Was ist Azure CDN?](../best-practices-cdn.md?toc=%2fazure%2fcdn%2ftoc.json)
* [Aktivieren von HTTPS für eine benutzerdefinierte Azure CDN-Domäne](../cdn/cdn-custom-ssl.md)
* [Verbessern der Leistung durch Komprimieren von Dateien in Azure CDN](../cdn/cdn-improve-performance.md)
* [Vorabladen von Assets auf einen Azure CDN-Endpunkt](../cdn/cdn-preload-endpoint.md)

