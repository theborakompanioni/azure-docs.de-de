---
title: Zuordnen eines benutzerdefinierten DNS-Namens zu einer Azure-Web-App | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie einen benutzerdefinierten DNS-Domänennamen zu einer Web-App, einem mobilen App-Back-End oder einer API-App in Azure App Service hinzufügen."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/29/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 25ddbd0c89ec7eee8eb240cf111245bb8b5add48
ms.lasthandoff: 04/04/2017


---
# <a name="map-a-custom-dns-name-to-an-azure-web-app"></a>Zuordnen eines benutzerdefinierten DNS-Namens zu einer Azure-Web-App

In diesem Tutorial erfahren Sie, wie Sie Ihrer Web-App, Ihrem mobilen App-Back-End oder Ihrer API-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) einen benutzerdefinierten DNS-Namen zuordnen. 

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

Dieses Tutorial folgt dem Beispielszenario der Zuordnung zweier DNS-Namen zu einer App in App Service:

- `contoso.com`: Eine Stammdomäne. Sie verwenden einen [A-Eintrag](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) zum Zuordnen zu Azure. 
- `www.contoso.com`: Eine Unterdomäne von `contoso.com`. Sie können entweder einen [A-Eintrag](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) oder einen [CNAME-Eintrag](https://en.wikipedia.org/wiki/CNAME_record) verwenden.

Das Tutorial veranschaulicht auch, wie Sie einen [Platzhalter-DNS-Eintrag](https://en.wikipedia.org/wiki/Wildcard_DNS_record) zu App Service zuordnen (z.B. `*.contoso.com`).

## <a name="before-you-begin"></a>Voraussetzungen

Da dieses Tutorial veranschaulicht, wie Sie einen DNS-Namen zu Azure App Service zuordnen, müssen Sie über Administratorzugriff auf die DNS-Konfigurationsseite Ihres jeweiligen Domänenanbieters verfügen (z.B. GoDaddy). Um beispielsweise eine Zuordnung für `contoso.com` und `www.contoso.com` hinzuzufügen, müssen Sie DNS-Einträge für `contoso.com` konfigurieren können.

Wenn Sie noch keine Domäne bei einem Domänenanbieter registriert haben, können Sie jederzeit einfach [eine Domäne direkt bei Azure kaufen und Ihrer App zuordnen](custom-dns-web-site-buydomains-web-app.md).

## <a name="step-1---prepare-your-app"></a>Schritt 1: Vorbereiten Ihrer App
Um einen benutzerdefinierten DNS-Namen zuzuordnen, muss Ihr [App Service-Plan](https://azure.microsoft.com/pricing/details/app-service/) den Tarif **Shared** oder höher aufweisen. In diesem Schritt stellen Sie sicher, dass Ihre Azure-App sich im richtigen Tarif befindet.

### <a name="log-in-to-azure"></a>Anmelden an Azure

Öffnen Sie das Azure-Portal. 

Melden Sie sich hierzu mit Ihrem Azure-Konto bei [https://portal.azure.com](https://portal.azure.com) an.

### <a name="navigate-to-your-app"></a>Navigieren zu Ihren App
Klicken Sie im linken Menü auf **App Service** und anschließend auf den Namen Ihrer Azure-App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/select-app.png)

Dadurch gelangen Sie auf das _Blatt_ Ihrer App (eine Portalseite, die horizontal geöffnet wird).  

### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs
Stellen Sie auf der Seite **Übersicht**, die standardmäßig geöffnet wird, sicher, dass sich Ihre App nicht im Tarif **Free** befindet.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Benutzerdefinierte DNS-Einträge werden im Tarif **Free** nicht unterstützt. Wenn Sie Ihre App zentral hochskalieren müssen, führen Sie die Schritte im nächsten Abschnitt aus. Andernfalls fahren Sie mit [Schritt 2]() fort.

### <a name="scale-up-your-app-service-plan"></a>Zentrales Hochskalieren Ihres App Service-Plans

Um Ihren Plan zentral hochzuskalieren, klicken Sie im linken Bereich auf **Zentral hochskalieren (App Service-Plan)**.

Wählen Sie den Tarif aus, auf den Sie hochskalieren möchten. Wählen Sie z.B. **Shared**. Wenn Sie fertig sind, klicken Sie auf **Auswählen**.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wenn die unten stehende Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="info"></a>

## <a name="step-2---get-mapping-information"></a>Schritt 2: Abrufen der Zuordnungsinformationen

In diesem Schritt rufen Sie die Informationen ab, die Sie später benötigen, um den DNS-Eintrag bzw. die DNS-Einträge zu erstellen. 

### <a name="open-the-custom-domain-ui"></a>Öffnen der Benutzeroberfläche der benutzerdefinierten Domäne

Klicken Sie auf dem Blatt Ihrer App im Menü auf **Benutzerdefinierte Domänen**. 

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

### <a name="copy-the-mapping-information"></a>Kopieren der Zuordnungsinformationen

Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die **IP-Adresse** der App und ihren Standardhostnamen unter **Der Website zugewiesene Hostnamen**.

Sie benötigen die IP-Adresse für die Zuordnung des A-Eintrags und den Standardhostnamen für die Zuordnung des CNAME-Eintrags.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

## <a name="step-3---add-dns-records"></a>Schritt 3: Hinzufügen von DNS-Einträgen 

In diesem Schritt fügen Sie die DNS-Einträge hinzu, die Sie benötigen, um Ihren benutzerdefinierten DNS-Namen zu Ihrer App zuzuordnen. Diesen Schritt führen Sie mit Ihrem eigenen Domänenanbieter durch.

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich zunächst bei der Website Ihres Domänenanbieters an.

Suchen Sie dann die Seite für die Verwaltung von DNS-Einträgen. Jeder Domänenanbieter hat eine eigene Benutzeroberfläche für DNS-Einträge, daher finden Sie Informationen dazu in der Dokumentation des Anbieters. Im Allgemeinen sollten Sie nach Links oder Bereichen der Website suchen, die als **Domain Name** (Domänenname), **DNS** oder **Name Server Management** (Namenserververwaltung) bezeichnet werden. Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**. 

Suchen Sie dann nach einem Link, mit dem Sie DNS-Einträge verwalten können. Dieser Link wird möglicherweise als **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced configuration** (Erweiterte Konfiguration) bezeichnet.

Der folgende Screenshot zeigt beispielhaft, wie die Seite für DNS-Einträge aussehen könnte:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie auf **Add** (Hinzufügen) klicken, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen dazu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern, wie z.B. GoDaddy, werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie nach dem Erstellen der Einträge auf einen gesonderten Link zum Speichern von Änderungen (**Save Changes**) klicken. 
>
>

<a name="a"></a>

### <a name="create-an-a-record"></a>Erstellen eines A-Eintrags

Im Beispiel dieses Tutorials fügen Sie einen A-Eintrag für die Stammdomäne `contoso.com` hinzu. 

Um Ihrer App einen A-Eintrag zuzuordnen, erfordert App Service in Wirklichkeit _zwei_ DNS-Einträge:

- Einen **A**-Eintrag, um die IP-Adresse Ihrer App zuzuordnen.
- Einen **TXT**-Eintrag, um den Standardhostnamen Ihrer App zuzuordnen. Mit diesem Eintrag kann App Service überprüfen, ob Sie die benutzerdefinierte Domäne besitzen, die Sie zuordnen möchten.

Die folgende Tabelle zeigt die Konfiguration der Zuordnung eines A-Eintrags für die unterstützten Domänentypen (`@` repräsentiert in der Regel die Stammdomäne). 

<table cellspacing="0" border="1">
  <tr>
    <th>Domänentyp</th>
    <th>Eintragstyp</th>
    <th>Host</th>
    <th>Wert</th>
  </tr>
  <tr>
    <td rowspan="2" align="left">Stammdomäne<br>(z.B. <code>contoso.com</code>)</td>
    <td>A</td>
    <td>@</td>
    <td>IP-Adresse aus <a href="#info">Schritt 2</a></td>
  </tr>
  <tr>
    <td>TXT</td>
    <td>@</td>
    <td>Standardhostname aus <a href="#info">Schritt 2</a></td>
  </tr>
  <tr>
    <td rowspan="2" align="left">Unterdomäne<br>(z.B. <code>www.contoso.com</code>)</td>
    <td>A</td>
    <td>www</td>
    <td>IP-Adresse aus <a href="#info">Schritt 2</a></td>
  </tr>
  <tr>
    <td>TXT</td>
    <td>www</td>
    <td>Standardhostname aus <a href="#info">Schritt 2</a></td>
  </tr>
  <tr>
    <td rowspan="2" align="left">Platzhalterdomäne<br>(z.B. <code>*.contoso.com</code>)</td>
    <td>A</td>
    <td>\*</td>
    <td>IP-Adresse aus <a href="#info">Schritt 2</a></td>
  </tr>
  <tr>
    <td>TXT</td>
    <td>\*</td>
    <td>Standardhostname aus <a href="#info">Schritt 2</a></td>
  </tr>
</table>

Für das Beispiel der Domäne `contoso.com` sollte die Seite mit den DNS-Einträgen in etwa wie der folgende Screenshot aussehen:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="cname"></a>

### <a name="create-a-cname-record"></a>Erstellen eines CNAME-Eintrags
Im Beispiel dieses Tutorials fügen Sie einen CNAME-Eintrag für die Stammdomäne `contoso.com` hinzu. 

Im Gegensatz zum [Zuordnen eines A-Eintrags](#a) ist für das Zuordnen eines CNAME-Eintrags zu Ihrer App kein zusätzlicher Eintrag erforderlich.

> [!IMPORTANT]
> Es wird empfohlen, eine CNAME-Zuordnung zu verwenden. Wenn Sie Ihre App löschen und neu erstellen oder von einem dedizierten Hostingtarif zurück in den Tarif **Shared** wechseln, kann sich die virtuelle IP-Adresse Ihrer App ändern. Ein CNAME-Eintrag bleibt während eines solchen Wechsels erhalten, eine A-Zuordnung wird möglicherweise durch die neue IP-Adresse ungültig. 
>
> Erstellen Sie jedoch _keinen_ CNAME-Eintrag für die Stammdomäne (d.h. den „Stammeintrag“). Weitere Informationen finden Sie unter [Why can't a CNAME record be used at the root domain](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain) (Warum kann ein CNAME-Eintrag nicht für die Stammdomäne verwendet werden?).
> Verwenden Sie stattdessen zum Zuordnen einer Stammdomäne zu Ihrer Azure-App einen [A-Datensatz](#a).
> 
> 

Die folgende Tabelle zeigt die Konfiguration der Zuordnung eines CNAME-Eintrags für die unterstützten Domänentypen.

Konfigurieren Sie Ihren CNAME-Eintrag wie folgt:

<table cellspacing="0" border="1">
  <tr>
    <th>Domänentyp</th>
    <th>CNAME-Host</th>
    <th>CNAME-Wert</th>
  </tr>
  <tr>
    <td>Unterdomäne<br>(z.B. <code>www.contoso.com</code>)</td>
    <td>www</td>
    <td>Standardhostname aus <a href="#info">Schritt 2</a></td>
  </tr>
  <tr>
    <td>Platzhalterdomäne<br>(z.B. <code>*.contoso.com</code>)</td>
    <td>\*</td>
    <td>Standardhostname aus <a href="#info">Schritt 2</a></td>
  </tr>
</table>

Für das Beispiel der Domäne `www.contoso.com` sollte die Seite mit den DNS-Einträgen in etwa wie der folgende Screenshot aussehen:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record.png)

<a name="enable"></a>

## <a name="step-4---enable-the-custom-dns-in-your-app"></a>Schritt 4: Aktivieren des benutzerdefinierten DNS-Namens in Ihrer App

Jetzt können Sie Ihre konfigurierten DNS-Namen (z.B. `contoso.com` und `www.contoso.com`) zu Ihrer App hinzufügen.

Auf der Seite **Benutzerdefinierte Domänen** im Azure-Portal (siehe [Schritt 1](#info)) müssen Sie den vollqualifizierten Domänennamen (Fully-Qualified Domain Name, FQDN) Ihrer benutzerdefinierten Domäne zur Liste hinzufügen.

### <a name="add-the-a-hostname-to-your-app"></a>Hinzufügen des A-Hostnamens zu Ihrer App

Klicken Sie auf das **+**-Symbol neben **Hostnamen hinzufügen**.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Geben Sie den FQDN ein, für den Sie zuvor den A-Eintrag konfiguriert haben (z.B. `contoso.com`), und klicken Sie dann auf **Überprüfen**.

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/verification-error.png)

Andernfalls wird die Schaltfläche **Hostnamen hinzufügen** aktiviert. 

Stellen Sie sicher, dass die Option **Typ des Hostnamenseintrags** auf **A-Datensatz (beispiel.com)** festgelegt ist.

Klicken Sie auf **Hostnamen hinzufügen**, um den DNS-Namen zu Ihrer App hinzufügen.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Möglicherweise dauert es eine Weile, bis der neue Hostnamen auf der Seite **Benutzerdefinierte Domänen** Ihrer App berücksichtigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<a name="add-cname"></a>
### <a name="add-the-cname-hostname-to-your-app"></a>Hinzufügen des CNAME-Hostnamens zu Ihrer App

Klicken Sie auf das **+**-Symbol neben **Hostnamen hinzufügen**.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den FQDN ein, für den Sie zuvor den CNAME-Eintrag konfiguriert haben (z.B. `www.contoso.com`), und klicken Sie dann auf **Überprüfen**.

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

Andernfalls wird die Schaltfläche **Hostnamen hinzufügen** aktiviert. 

Stellen Sie sicher, dass die Option **Typ des Hostnamenseintrags** auf **A-Datensatz (beispiel.com)** festgelegt ist.

Klicken Sie auf **Hostnamen hinzufügen**, um den DNS-Namen zu Ihrer App hinzufügen.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Möglicherweise dauert es eine Weile, bis der neue Hostnamen auf der Seite **Benutzerdefinierte Domänen** Ihrer App berücksichtigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

### <a name="step-5---test-in-browser"></a>Schritt 5: Testen im Browser

Wechseln Sie in Ihrem Browser zu den DNS-Namen, die Sie zuvor konfiguriert haben (`contoso.com` und `www.contoso.com`).

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="more-resources"></a>Weitere Ressourcen

[Kaufen und Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](custom-dns-web-site-buydomains-web-app.md)

