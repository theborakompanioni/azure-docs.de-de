---
title: Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie einen vorhandenen benutzerdefinierten DNS-Domänennamen einer Web-App, einem Back-End einer mobilen App oder einer API-App in Azure App Service hinzufügen."
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
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: f98b876658c3257ad2b9162dea053f879ba1f1f0
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps

[Azure-Web-Apps](app-service-web-overview.md) bietet einen hochgradig skalierbaren Webhosting-Dienst mit Self-Patching. In diesem Tutorial wird gezeigt, wie Azure-Web-Apps ein vorhandener benutzerdefinierter DNS-Name zugeordnet wird.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Zuordnen einer Unterdomäne (z.B. `www.contoso.com`) per CNAME-Eintrag
> * Zuordnen einer Stammdomäne (z.B. `contoso.com`) per A-Eintrag
> * Zuordnen einer Platzhalterdomäne (z.B. `*.contoso.com`) per CNAME-Eintrag
> * Automatisieren der Domänenzuordnung mithilfe von Skripts

Verwenden Sie für die Zuordnung eines benutzerdefinierten DNS-Namens zu App Service entweder einen **CNAME-Eintrag** oder einen **A-Eintrag**. 

> [!NOTE]
> Es wird empfohlen, einen CNAME-Eintrag für alle benutzerdefinierten DNS-Namen außer für Stammdomänen (z.B. `contoso.com`) zu verwenden. 

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Erstellen Sie eine App Service-App](/azure/app-service/), oder verwenden Sie eine App, die Sie für ein anderes Tutorial erstellt haben.
* Erwerben Sie einen Domänennamen, und stellen Sie sicher, dass Sie Zugriff auf die DNS-Registrierung für Ihren Domänenanbieter (z.B. GoDaddy) haben.

  Um beispielsweise DNS-Einträge für `contoso.com` und `www.contoso.com` hinzuzufügen, müssen Sie die DNS-Einstellungen für die Stammdomäne `contoso.com` konfigurieren können.

  > [!NOTE]
  > Falls Sie nicht über einen vorhandenen Domänennamen verfügen, können Sie erwägen, [über das Azure-Portal einen Domänennamen zu erwerben](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-the-app"></a>Vorbereiten der App

Um einer Web-App einen benutzerdefinierten DNS-Namen zuzuordnen, muss der [App Service-Plan](https://azure.microsoft.com/pricing/details/app-service/) der Web-App einen kostenpflichtigen Tarif (**Shared**, **Basic**, **Standard** oder **Premium**) aufweisen. In diesem Schritt stellen Sie sicher, dass sich die App Service-App innerhalb des unterstützten Tarifs befindet.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Navigieren zur App im Azure-Portal

Wählen Sie im linken Menü **App Services** und anschließend den Namen der App aus.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/select-app.png)

Die Verwaltungsseite der App Service-App wird angezeigt.  

### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs

Scrollen Sie im linken Navigationsbereich der App-Seite zum Abschnitt **Einstellungen**, und wählen Sie **Zentral hochskalieren (App Service-Plan)**.

![Menü „Zentral hochskalieren“](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Der aktuelle Tarif der App wird durch einen blauen Rahmen hervorgehoben. Stellen Sie sicher, dass sich die App nicht im Tarif **Free** befindet. Benutzerdefinierte DNS-Einträge werden im Tarif **Free** nicht unterstützt. 

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Wenn für den App Service-Plan nicht der Tarif **Free** gilt, können Sie die Seite **Preisstufe auswählen** schließen und zu [Zuordnen eines CNAME-Eintrags](#cname) springen.

### <a name="scale-up-the-app-service-plan"></a>Zentrales Hochskalieren des App Service-Plans

Wählen Sie einen der kostenpflichtigen Tarife aus (**Shared**, **Basic**, **Standard** oder **Premium**). 

Klicken Sie auf **Auswählen**.

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wenn die unten angegebene Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

![Bestätigung des Skalierungsvorgangs](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Zuordnen eines CNAME-Eintrags

Im Tutorialbeispiel fügen Sie einen CNAME-Eintrag für die Unterdomäne `www` (z.B. `www.contoso.com`) hinzu.

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu in der Dokumentation des Anbieters. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. 

Häufig finden Sie in Ihren Kontoinformationen die Seite für die Verwaltung von DNS-Einträgen. Suchen Sie anschließend nach einem Link, z.B. **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der beispielsweise den Namen **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

Der folgende Screenshot zeigt ein Beispiel für eine Seite zur Verwaltung von DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie die Option **Add** (Hinzufügen) wählen, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen hierzu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie einen gesonderten Link **Änderungen speichern** wählen. 

### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Fügen Sie einen CNAME-Eintrag hinzu, um dem Standardhostnamen der App (`<app_name>.azurewebsites.net`) eine Unterdomäne zuzuordnen.

Fügen Sie für das Domänenbeispiel `www.contoso.com` einen CNAME-Eintrag hinzu, mit dem der Name `www` dem Element `<app_name>.azurewebsites.net` zugeordnet wird.

Nach dem Hinzufügen des CNAME-Eintrags sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>Aktivieren der Zuordnung von CNAME-Einträgen in Azure

Wählen Sie im linken Navigationsbereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen**. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Fügen Sie auf der Seite **Benutzerdefinierte Domänen** der App der Liste den vollqualifizierten benutzerdefinierten DNS-Namen (`www.contoso.com`) hinzu.

Wählen Sie das **+**-Symbol neben der Option **Hostnamen hinzufügen**.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie einen CNAME-Eintrag hinzugefügt haben, z.B. `www.contoso.com`. 

Wählen Sie **Überprüfen**.

Die Schaltfläche **Hostnamen hinzufügen** wird aktiviert. 

Stellen Sie sicher, dass **Typ des Hostnamenseintrags** auf **CNAME (www.beispiel.com oder eine beliebige Unterdomäne)** festgelegt ist.

Wählen Sie **Hostnamen hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Unter Umständen dauert es eine Weile, bis der neue Hostname auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Zuordnen eines A-Eintrags

Im Beispiel dieses Tutorials fügen Sie einen A-Eintrag für die Stammdomäne (z.B. `contoso.com`) hinzu. 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a>Kopieren der IP-Adresse der App

Für die Zuordnung eines A-Eintrags benötigen Sie die externe IP-Adresse der App. Sie finden diese IP-Adresse im Azure-Portal auf der Seite **Benutzerdefinierte Domänen** der App.

Wählen Sie im linken Navigationsbereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen**. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die IP-Adresse der App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu in der Dokumentation des Anbieters. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. 

Häufig finden Sie in Ihren Kontoinformationen die Seite für die Verwaltung von DNS-Einträgen. Suchen Sie anschließend nach einem Link, z.B. **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der beispielsweise den Namen **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

Der folgende Screenshot zeigt ein Beispiel für eine Seite zur Verwaltung von DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie die Option **Add** (Hinzufügen) wählen, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen hierzu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie einen gesonderten Link **Änderungen speichern** wählen. 

### <a name="create-the-a-record"></a>Erstellen des A-Eintrags

Um einer App einen A-Eintrag zuzuordnen, sind für App Service **zwei** DNS-Einträge erforderlich:

- Ein **A**-Eintrag, um die IP-Adresse der App zuzuordnen.
- Ein **TXT**-Eintrag, um den Standardhostnamen der App `<app_name>.azurewebsites.net` zuzuordnen. App Service nutzt diesen Eintrag nur zur Konfigurationszeit, um zu bestätigen, dass sich die benutzerdefinierte Domäne in Ihrem Besitz befindet. Nachdem Ihre benutzerdefinierte Domäne überprüft und in App Service konfiguriert wurde, können Sie diesen TXT-Eintrag löschen. 

Erstellen Sie für das Beispiel der Domäne `contoso.com` anhand der folgenden Tabelle die A- und TXT-Einträge (`@` stellt in der Regel die Stammdomäne dar). 

| Eintragstyp | Host | Wert |
| - | - | - |
| A | `@` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse der App](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Wenn die Einträge hinzugefügt werden, sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![Seite der DNS-Einträge](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a>Aktivieren der Zuordnung von A-Einträgen in der App

Fügen Sie auf der Seite **Benutzerdefinierte Domänen** der App im Azure-Portal den vollqualifizierten benutzerdefinierten DNS-Namen (z.B. `contoso.com`) der Liste hinzu.

Wählen Sie das **+**-Symbol neben der Option **Hostnamen hinzufügen**.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie den A-Eintrag konfiguriert haben, z.B. `contoso.com`.

Wählen Sie **Überprüfen**.

Die Schaltfläche **Hostnamen hinzufügen** wird aktiviert. 

Stellen Sie sicher, dass die Option **Typ des Hostnamenseintrags** auf **A-Datensatz (beispiel.com)** festgelegt ist.

Wählen Sie **Hostnamen hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Unter Umständen dauert es eine Weile, bis der neue Hostname auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter A-Eintrag](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Zuordnen einer Platzhalterdomäne

Im Beispiel des Tutorials ordnen Sie der App Service-App einen [DNS-Platzhalternamen](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (z.B. `*.contoso.com`) zu, indem Sie einen CNAME-Eintrag hinzufügen. 

### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

Melden Sie sich bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. 

Häufig enthalten Ihre Kontoinformationen eine Seite für die Verwaltung von DNS-Einträgen. Suchen Sie anschließend nach einem Link, z.B. **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der beispielsweise den Namen **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

Der folgende Screenshot zeigt ein Beispiel für eine Seite zur Verwaltung von DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

Im Beispielscreenshot würden Sie die Option **Add** (Hinzufügen) wählen, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen hierzu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie einen gesonderten Link **Änderungen speichern** wählen. 

### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Fügen Sie einen CNAME-Eintrag hinzu, um dem Standardhostnamen der App (`<app_name>.azurewebsites.net`) einen Platzhalternamen zuzuordnen.

Im Domänenbeispiel `*.contoso.com` ordnet der CNAME-Eintrag `<app_name>.azurewebsites.net` den Namen `*` zu.

Wenn der CNAME-Eintrag hinzugefügt wird, sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a>Aktivieren der Zuordnung von CNAME-Einträgen in der App

Sie können jetzt eine beliebige Unterdomäne hinzufügen, mit der der Platzhaltername der App zugeordnet wird (z.B. `sub1.contoso.com` und `sub2.contoso.com` zu `*.contoso.com`). 

Wählen Sie im linken Navigationsbereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen**. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Wählen Sie das **+**-Symbol neben der Option **Hostnamen hinzufügen**.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie einen vollqualifizierten Domänennamen ein, der zur Platzhalterdomäne passt (z.B. `sub1.contoso.com`), und wählen Sie anschließend **Überprüfen**.

Die Schaltfläche **Hostnamen hinzufügen** wird aktiviert. 

Stellen Sie sicher, dass **Typ des Hostnamenseintrags** auf **CNAME (www.beispiel.com oder eine beliebige Unterdomäne)** festgelegt ist.

Wählen Sie **Hostnamen hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Unter Umständen dauert es eine Weile, bis der neue Hostname auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

Wählen Sie das **+**-Symbol, um einen anderen Hostnamen hinzuzufügen, der der Platzhalterdomäne entspricht. Fügen Sie beispielsweise `sub2.contoso.com` hinzu.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testen im Browser

Navigieren Sie zu den DNS-Namen, die Sie zuvor konfiguriert haben, z.B. `contoso.com`, `www.contoso.com`, `sub1.contoso.com` und `sub2.contoso.com`.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a>Automatisieren mit Skripts

Durch die [Azure CLI](/cli/azure/install-azure-cli) oder durch [Azure PowerShell](/powershell/azure/overview) können Sie mithilfe von Skripts die Verwaltung von benutzerdefinierten Domänen automatisieren. 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle 

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt. 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Zuordnen einer Unterdomäne mit einem CNAME-Eintrag
> * Zuordnen einer Stammdomäne mit einem A-Eintrag
> * Zuordnen einer Platzhalterdomäne mit einem CNAME-Eintrag
> * Automatisieren der Domänenzuordnung mithilfe von Skripts

Fahren Sie mit dem nächsten Tutorial fort, um sich über das Anbinden eines benutzerdefinierten SSL-Zertifikats an eine Web-App zu informieren.

> [!div class="nextstepaction"]
> [Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure-Web-Apps](app-service-web-tutorial-custom-ssl.md)

