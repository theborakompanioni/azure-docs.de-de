---
title: Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie einen vorhandenen benutzerdefinierten DNS-Domänennamen zu einer Web-App, einem Back-End einer mobilen App oder einer API-App in Azure App Service hinzufügen."
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
ms.date: 04/21/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 08a83bf8dca71846dd06edb9aa253f69c24d253c
ms.lasthandoff: 04/27/2017


---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps

In diesem Tutorial wird gezeigt, wie [Azure-Web-Apps](app-service-web-overview.md) ein vorhandener benutzerdefinierter DNS-Name zugeordnet wird. 

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

Dieses Tutorial zeigt drei gängige Szenarien zur Zuordnung zweier DNS-Namen zu einer App in App Service:

- `www.contoso.com`: Eine Unterdomäne von `contoso.com`. Diese ordnen Sie mithilfe eines CNAME-Eintrags der App zu.
- `contoso.com`: Eine Stammdomäne. Diese ordnen Sie mithilfe eines A-Eintrags der App zu.
- `*.contoso.com`: Eine Platzhalterdomäne. Diese ordnen Sie mithilfe eines CNAME-Eintrags der App zu.

Verwenden Sie für die Zuordnung eines benutzerdefinierten DNS-Namens zu App Service entweder einen **CNAME-Eintrag** oder einen **A-Eintrag**.

> [!NOTE]
> Es wird empfohlen, einen CNAME-Eintrag für alle benutzerdefinierten DNS-Namen außer für Stammdomänen (z.B. „contoso.com“) zu verwenden. 
> 
> 

## <a name="before-you-begin"></a>Voraussetzungen

Um dieses Tutorial absolvieren zu können, benötigen Sie Zugriff auf die DNS-Registrierung für Ihren Domänenanbieter (z.B. GoDaddy) und die Berechtigungen zum Bearbeiten der Konfiguration für Ihre Domäne. 

Um beispielsweise DNS-Einträge für `contoso.com` und `www.contoso.com` hinzuzufügen, benötigen Sie Zugriff für die Konfiguration der DNS-Einstellungen für die Stammdomäne `contoso.com`. 

> [!NOTE]
> Wenn Sie keinen Domänennamen besitzen, sollten Sie eventuell das [Tutorial zur App Service-Domäne](custom-dns-web-site-buydomains-web-app.md) absolvieren, um eine Domäne über das Azure-Portal zu erwerben. 
>
>

## <a name="prepare-your-app"></a>Vorbereiten Ihrer App
Um einen benutzerdefinierten DNS-Namen zuzuordnen, muss Ihr [App Service-Plan](https://azure.microsoft.com/pricing/details/app-service/) einen kostenpflichtigen Tarif (**Shared**, **Basic**, **Standard** oder **Premium**) aufweisen. In diesem Schritt stellen Sie sicher, dass sich Ihre App Service-App im richtigen Tarif befindet.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das Azure-Portal. Melden Sie sich hierzu mit Ihrem Azure-Konto bei [https://portal.azure.com](https://portal.azure.com) an.

### <a name="navigate-to-your-app"></a>Navigieren zu Ihren App
Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/select-app.png)

Dadurch gelangen Sie zum Blatt für die Verwaltung Ihrer App Service-App (_Blatt_: eine Portalseite, die horizontal geöffnet wird).  

### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs

Scrollen Sie im linken Navigationsbereich auf dem Blatt Ihrer App zum Abschnitt **Einstellungen** und wählen Sie **Zentral hochskalieren (App Service-Plan)**.

![Menü „Zentral hochskalieren“](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Stellen Sie sicher, dass sich Ihre App nicht im Tarif **Free** befindet. Der aktuelle Tarif Ihrer App wird durch einen dunkelblauen Rahmen hervorgehoben. 

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Benutzerdefinierte DNS-Einträge werden im Tarif **Free** nicht unterstützt. Wenn Sie Ihre App zentral hochskalieren müssen, führen Sie die Schritte im nächsten Abschnitt aus. Schließen Sie andernfalls das Blatt **Tarif auswählen** und fahren Sie mit [Zuordnen eines CNAME-Eintrags](#cname) oder [Zuordnen eines A-Eintrags](#a) fort.

### <a name="scale-up-your-app-service-plan"></a>Zentrales Hochskalieren Ihres App Service-Plans

Wählen Sie einen der kostenpflichtigen Tarife aus (**Shared**, **Basic**, **Standard** oder **Premium**). 

Klicken Sie auf **Auswählen**.

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wenn die unten stehende Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

![Bestätigung des Skalierungsvorgangs](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Zuordnen eines CNAME-Eintrags

In dem Tutorialbeispiel möchten Sie einen CNAME-Eintrag für die Unterdomäne `www` (`www.contoso.com`) hinzufügen. 

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich zunächst bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Jeder Domänenanbieter hat eine eigene Benutzeroberfläche für DNS-Einträge, daher finden Sie Informationen dazu in der Dokumentation des Anbieters. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. 

Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**. Suchen Sie dann nach einem Link, mit dem Sie DNS-Einträge verwalten können. Dieser Link wird möglicherweise als **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced configuration** (Erweiterte Konfiguration) bezeichnet.

Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie auf **Add** (Hinzufügen) klicken, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen dazu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie auf einen gesonderten Link **Änderungen speichern** klicken. 
>
>

### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Fügen Sie einen CNAME-Eintrag hinzu, um dem Standardhostnamen Ihrer App (`<app_name>.azurewebsites.net`) eine Unterdomäne zuzuordnen.

Für das Beispiel der Domäne `www.contoso.com` sollte Ihr CNAME-Eintrag den Namen `www` auf `<app_name>.azurewebsites.net` verweisen.

Die Seite Ihrer DNS-Einträge sollte in etwa wie auf dem folgenden Screenshot aussehen:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-your-app"></a>Aktivieren der Zuordnung von CNAME-Einträgen in Ihrer App

Jetzt können Sie Ihren konfigurierten DNS-Namen zu Ihrer App hinzufügen.

Klicken Sie im linken Navigationsbereich auf dem Blatt Ihrer App auf **Benutzerdefinierte Domänen**. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Auf dem Blatt **Benutzerdefinierte Domänen** Ihrer App müssen Sie den vollqualifizierten benutzerdefinierten DNS-Namen (`www.contoso.com`) zur Liste hinzufügen.

Klicken Sie auf das **+**-Symbol neben **Hostnamen hinzufügen**.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie zuvor den CNAME-Eintrag konfiguriert haben (z.B. `www.contoso.com`), und klicken Sie dann auf **Überprüfen**.

Andernfalls wird die Schaltfläche **Hostnamen hinzufügen** aktiviert. 

Stellen Sie sicher, dass **Typ des Hostnamenseintrags** auf **CNAME-Eintrag (example.com)** festgelegt ist.

Klicken Sie auf **Hostnamen hinzufügen**, um den DNS-Namen zu Ihrer App hinzufügen.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Möglicherweise dauert es eine Weile, bis der neue Hostnamen auf der Seite **Benutzerdefinierte Domänen** Ihrer App berücksichtigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Zuordnen eines A-Eintrags

Im Beispiel dieses Tutorials fügen Sie einen A-Eintrag für die Stammdomäne `contoso.com` hinzu. 

<a name="info"></a>

### <a name="copy-your-apps-ip-address"></a>Kopieren der IP-Adresse Ihrer App

Für die Zuordnung eines A-Eintrags benötigen Sie die externe IP-Adresse Ihrer App. Sie finden diese IP-Adresse auf dem Blatt **Benutzerdefinierte Domänen**.

Klicken Sie im linken Navigationsbereich auf dem Blatt Ihrer App auf **Benutzerdefinierte Domänen**. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die IP-Adresse der App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich zunächst bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Jeder Domänenanbieter hat eine eigene Benutzeroberfläche für DNS-Einträge, daher finden Sie Informationen dazu in der Dokumentation des Anbieters. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. 

Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**. Suchen Sie dann nach einem Link, mit dem Sie DNS-Einträge verwalten können. Dieser Link wird möglicherweise als **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced configuration** (Erweiterte Konfiguration) bezeichnet.

Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie auf **Add** (Hinzufügen) klicken, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen dazu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie auf einen gesonderten Link **Änderungen speichern** klicken. 
>
>

### <a name="create-the-a-record"></a>Erstellen des A-Eintrags

Um Ihrer App einen A-Eintrag zuzuordnen, erfordert App Service in Wirklichkeit **zwei** DNS-Einträge:

- Einen **A**-Eintrag, um die IP-Adresse Ihrer App zuzuordnen.
- Einen **TXT**-Eintrag, um den Standardhostnamen Ihrer App `<app_name>.azurewebsites.net` zuzuordnen. Mit diesem Eintrag kann App Service überprüfen, ob Sie die benutzerdefinierte Domäne besitzen, die Sie zuordnen möchten.

Erstellen Sie für das Beispiel der Domäne `www.contoso.com` anhand der folgenden Tabelle die A- und TXT-Einträge (`@` stellt in der Regel die Stammdomäne dar). 

| Eintragstyp | Host | Wert |
| - | - | - |
| A | `@` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse Ihrer App](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Die Seite Ihrer DNS-Einträge sollte in etwa wie auf dem folgenden Screenshot aussehen:

![Seite der DNS-Einträge](./media/app-service-web-tutorial-custom-domain/a-record.png)

### <a name="enable-the-a-record-mapping-in-your-app"></a>Aktivieren der Zuordnung von A-Einträgen in Ihrer App

Jetzt können Sie Ihren konfigurierten DNS-Namen zu Ihrer App hinzufügen.

Auf der Seite **Benutzerdefinierte Domänen** Ihrer App im Azure-Portal müssen Sie den vollqualifizierten benutzerdefinierten DNS-Namen (`contoso.com`) zur Liste hinzufügen.

Klicken Sie auf das **+**-Symbol neben **Hostnamen hinzufügen**.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie zuvor den A-Eintrag konfiguriert haben (z.B. `contoso.com`), und klicken Sie dann auf **Überprüfen**.

Andernfalls wird die Schaltfläche **Hostnamen hinzufügen** aktiviert. 

Stellen Sie sicher, dass die Option **Typ des Hostnamenseintrags** auf **A-Datensatz (beispiel.com)** festgelegt ist.

Klicken Sie auf **Hostnamen hinzufügen**, um den DNS-Namen zu Ihrer App hinzufügen.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Möglicherweise dauert es eine Weile, bis der neue Hostnamen auf der Seite **Benutzerdefinierte Domänen** Ihrer App berücksichtigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter A-Eintrag](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Zuordnen einer Platzhalterdomäne

Sie können Ihrer App Service-App auch ein [Platzhalter-DNS](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (z.B. `*.contoso.com`) zuordnen. 

Die hier angegebenen Schritte zeigen, wie die Platzhalterdomäne `*.contoso.com` mittels eines CNAME-Eintrags zugeordnet wird. 

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich zunächst bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Jeder Domänenanbieter hat eine eigene Benutzeroberfläche für DNS-Einträge, daher finden Sie Informationen dazu in der Dokumentation des Anbieters. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. 

Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**. Suchen Sie dann nach einem Link, mit dem Sie DNS-Einträge verwalten können. Dieser Link wird möglicherweise als **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced configuration** (Erweiterte Konfiguration) bezeichnet.

Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie auf **Add** (Hinzufügen) klicken, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen dazu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie auf einen gesonderten Link **Änderungen speichern** klicken. 
>
>

### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Fügen Sie einen CNAME-Eintrag hinzu, um dem Standardhostnamen Ihrer App (`<app_name>.azurewebsites.net`) einen Platzhalternamen zuzuordnen.

Für das Beispiel der Domäne `*.contoso.com` sollte Ihr CNAME-Eintrag den Namen `*` auf `<app_name>.azurewebsites.net` verweisen.

Die Seite Ihrer DNS-Einträge sollte in etwa wie auf dem folgenden Screenshot aussehen:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-your-app"></a>Aktivieren der Zuordnung von CNAME-Einträgen in Ihrer App

Nun können Sie eine beliebige Unterdomäne hinzufügen, die Ihrem Platzhalternamen entspricht.

Für das Beispiel des Platzhalters `*.contoso.com` können Sie nun `sub1.contoso.com` und `sub2.contoso.com` hinzufügen. 

Klicken Sie im linken Navigationsbereich auf dem Blatt Ihrer App auf **Benutzerdefinierte Domänen**. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Klicken Sie auf das **+**-Symbol neben **Hostnamen hinzufügen**.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den vollqualifizierten Domänennamen für eine Unterdomäne ein, die Ihrer Platzhalterdomäne entspricht (z.B. `sub1.contoso.com`), und klicken Sie dann auf **Überprüfen**.

Andernfalls wird die Schaltfläche **Hostnamen hinzufügen** aktiviert. 

Stellen Sie sicher, dass **Typ des Hostnamenseintrags** auf **CNAME-Eintrag (example.com)** festgelegt ist.

Klicken Sie auf **Hostnamen hinzufügen**, um den DNS-Namen zu Ihrer App hinzufügen.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Möglicherweise dauert es eine Weile, bis der neue Hostnamen auf der Seite **Benutzerdefinierte Domänen** Ihrer App berücksichtigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

Klicken Sie erneut auf das **+**-Symbol, um einen anderen Hostnamen hinzuzufügen, der Ihrer Platzhalterdomäne entspricht.

Fügen Sie z.B. `sub2.contoso.com` mithilfe der oben genannten Schritte hinzu.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testen im Browser

Wechseln Sie in Ihrem Browser zu den DNS-Namen, die Sie zuvor konfiguriert haben (`contoso.com` und `www.contoso.com`).

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a>Automatisieren mit Skripts

Durch die [Azure CLI](/cli/azure/install-azure-cli) oder durch [Azure PowerShell](/powershell/azure/overview) können Sie mithilfe von Skripts die Verwaltung von benutzerdefinierten Domänen automatisieren. 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle 

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt. 

```bash 
az appservice web config hostname add --webapp <app_name> --resource-group <resourece_group_name> \ 
--name <fully_qualified_domain_name> 
``` 

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/app-service-cli-configure-custom-domain.md) 

### <a name="azure-powershell"></a>Azure PowerShell 

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt. 

```PowerShell  
Set-AzureRmWebApp -Name <app_name> -ResourceGroupName <resourece_group_name> ` 
-HostNames @(<fully_qualified_domain_name>,"<app_name>.azurewebsites.net") 
```

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="more-resources"></a>Weitere Ressourcen

[Konfigurieren einer App Service-Domäne in Azure App Service](custom-dns-web-site-buydomains-web-app.md)

