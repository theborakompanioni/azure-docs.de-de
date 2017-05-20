---
title: "Hinzufügen eines SSL-Zertifikats zu Ihrer Azure App Service-App | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie ein SSL-Zertifikat zu Ihrer App Service-App hinzufügen."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo;aelnably
ms.translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: c4e7ee86ad9dc7a51fff9e948757faaf5ca9b9c4
ms.contentlocale: de-de
ms.lasthandoff: 04/22/2017

---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service

In diesem Tutorial schützen Sie Ihre Web-App, indem Sie ein SSL-Zertifikat für Ihre Instanz von **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)** erwerben, sicher in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) speichern und einer benutzerdefinierten Domäne zuordnen.

## <a name="step-1---log-in-to-azure"></a>Schritt 1: Anmelden bei Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="step-2---place-an-ssl-certificate-order"></a>Schritt 2: Bestellen eines SSL-Zertifikats

Ein SSL-Zertifikat können Sie bestellen, indem Sie im **Azure-Portal** ein neues [App Service-Zertifikat](https://portal.azure.com/#create/Microsoft.SSL) erstellen.

![Zertifikaterstellung](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Geben Sie einen aussagekräftigen **Namen** für Ihr SSL-Zertifikat sowie den **Domänennamen** ein.

> [!NOTE]
> Dies ist einer der wichtigsten Teile des Einkaufsvorgangs. Achten Sie darauf, dass Sie den richtigen Hostnamen (benutzerdefinierte Domäne) eingeben, den Sie mit diesem Zertifikat schützen möchten. Fügen Sie dem Hostnamen **NICHT** WWW an. 
>

Wählen Sie Ihr **Abonnement**, Ihre **Ressourcengruppe** und Ihre **Zertifikat-SKU** aus.

> [!WARNING]
> App Service-Zertifikate können nur von anderen App-Diensten innerhalb des gleichen Abonnements verwendet werden.  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Schritt 3: Speichern des Zertifikats in Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) ist ein Azure-Dienst zum Schutz von kryptografischen Schlüsseln und Geheimnissen, die von Cloudanwendungen und -diensten verwendet werden.
>

Öffnen Sie nach Erwerb des SSL-Zertifikats das Ressourcenblatt [App Service-Zertifikate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders).

![Bild von Bereitschaft zum Speichern in KV einfügen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Der Zertifikatstatus lautet **Ausstehende Ausstellung**, da Sie noch ein paar Schritte ausführen müssen, bevor Sie das Zertifikat verwenden können.

Klicken Sie auf dem Blatt „Zertifikateigenschaften“ auf **Zertifikatkonfiguration** und anschließend auf **Schritt 1: Speichern**, um dieses Zertifikat in Azure Key Vault zu speichern.

Klicken Sie auf dem Blatt **Key Vault-Status** auf **Key Vault-Repository**, um einen bereits vorhandenen Schlüsseltresor zum Speichern dieses Zertifikats auszuwählen, ODER klicken Sie auf **Neuen Schlüsseltresor erstellen**, um einen neuen Schlüsseltresor im gleichen Abonnement und in der gleichen Ressourcengruppe zu erstellen.

> [!NOTE]
> Die Kosten für die Speicherung des Zertifikats sind bei Azure Key Vault sehr gering.
> Weitere Informationen finden Sie in der **[Preisübersicht zu Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Nach der Wahl des Key Vault-Repositorys für die Speicherung des Zertifikats sollte der Schritt **Speichern** als erfolgreich angezeigt werden.

![Bild von der erfolgreichen Ausführung des Schritts „Speichern“ einfügen](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Schritt 4: Überprüfen des Domänenbesitzes

> [!NOTE]
> Von App Service-Zertifikaten werden drei Arten von Domänenüberprüfungen unterstützt: Domänenüberprüfung, Überprüfung per E-Mail und manuelle Überprüfung. Diese werden im Abschnitt [Erweitert](#advanced) ausführlicher erläutert.

Klicken Sie auf dem Blatt **Zertifikatkonfiguration**, das Sie in Schritt 3 verwendet haben, auf **Schritt 2: Überprüfen**.

Die **Domänenüberprüfung** ist **NUR DANN** der zweckmäßigste Prozess, wenn Sie **[Ihre benutzerdefinierte Domäne von Azure App Service erworben](custom-dns-web-site-buydomains-web-app.md)** haben.
Klicken Sie auf die Schaltfläche **Überprüfen**, um diesen Schritt abzuschließen.

![Bild von der Domänenüberprüfung einfügen](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Klicken Sie nach dem Klicken auf **Überprüfen** auf die Schaltfläche **Aktualisieren**. Daraufhin sollte der Schritt **Überprüfen** als erfolgreich angezeigt werden.

![Bild von der erfolgreichen Ausführung des Schritts „Überprüfen“ einfügen](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Schritt 5: Zuweisen des Zertifikats zur App Service-App

> [!NOTE]
> Bevor Sie die Schritte in diesem Abschnitt ausführen, müssen Sie einen benutzerdefinierten Domänennamen mit Ihrer App verknüpft haben. Weitere Informationen finden Sie unter **[Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App](web-sites-custom-domain-name.md)**.
>

Klicken Sie im **[Azure-Portal](https://portal.azure.com/)** auf der linken Seite auf **App Service**.

Klicken Sie auf den Namen Ihrer App, der Sie dieses Zertifikat zuweisen möchten.

Klicken Sie in den **Einstellungen** auf **SSL-Zertifikate**.

Klicken Sie auf **App Service-Zertifikat importieren**, und wählen Sie das soeben erworbene Zertifikat aus.

![Bild von Zertifikatimport einfügen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

Klicken Sie im Abschnitt **SSL-Bindungen** auf **Add bindings** (Bindungen hinzufügen), und wählen Sie mithilfe der Dropdownlisten den Domänennamen, der mit SSL geschützt werden soll, sowie das zu verwendende Zertifikat aus. Sie können auch auswählen, ob SSL auf der **[Servernamensanzeige](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Server Name Indication, SNI) oder IP basieren soll.

![Bild von SSL-Bindungen einfügen](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klicken Sie auf **Bindung hinzufügen** , um die Änderungen zu speichern und SSL zu aktivieren.

> [!NOTE]
> Wenn Sie **IP-basiertes SSL** ausgewählt haben und Ihre benutzerdefinierte Domäne einen A-Eintrag verwendet, sind folgende Zusatzschritte erforderlich. Diese werden im Abschnitt [Erweitert](#Advanced) ausführlicher erläutert.

Ihre App sollte nun über `HTTPS://` (anstelle von `HTTP://`) erreichbar sein. So können Sie überprüfen, ob das Zertifikat richtig konfiguriert wurde.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Schritt 6: Verwaltungsaufgaben

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App")]

## <a name="advanced"></a>Erweitert

### <a name="verifying-domain-ownership"></a>Überprüfen des Domänenbesitzes

Von App Service-Zertifikaten werden noch zwei andere Arten der Domänenüberprüfung unterstützt: Überprüfung per E-Mail und manuelle Überprüfung.

#### <a name="mail-verification"></a>Überprüfung per E-Mail

Die Überprüfungs-E-Mail wurde bereits an die E-Mail-Adresse(n) gesendet, die dieser benutzerdefinierten Domäne zugeordnet ist/sind.
Öffnen Sie die E-Mail, und klicken Sie auf den Link für die Überprüfung, um den Schritt für die Überprüfung per E-Mail abzuschließen.

![Bild von der Überprüfung per E-Mail einfügen](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Wenn Sie die Überprüfungs-E-Mail erneut senden müssen, klicken Sie auf die Schaltfläche **E-Mail erneut senden**.

#### <a name="manual-verification"></a>Manuelle Überprüfung

> [!IMPORTANT]
> HTML-Webseitenüberprüfung (funktioniert nur mit Standardzertifikat-SKU)
>

1. Erstellen der HTML-Datei **starfield.html**

1. Der Inhalt dieser Datei muss exakt dem Namen des Domänenüberprüfungstokens entsprechen. (Sie können das Token vom Blatt mit dem Domänenüberprüfungsstatus kopieren.)

1. Aktualisieren Sie diese Datei im Stammverzeichnis des Webservers, der Ihre Domäne hostet: `/.well-known/pki-validation/starfield.html`

1. Klicken Sie nach Abschluss der Überprüfung auf **Aktualisieren**, um den Zertifikatstatus zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.

> [!TIP]
> Vergewissern Sie sich in einem Terminal mithilfe von `curl -G http://<domain>/.well-known/pki-validation/starfield.html`, dass die Antwort das `<verification-token>` enthält.

#### <a name="dns-txt-record-verification"></a>Überprüfung des DNS-TXT-Eintrags

1. Erstellen Sie mithilfe des DNS-Managers einen TXT-Eintrag in der Unterdomäne `@` mit einem Wert, der dem Domänenüberprüfungstoken entspricht.
1. Klicken Sie nach Abschluss der Überprüfung auf **Aktualisieren**, um den Zertifikatstatus zu aktualisieren.

> [!TIP]
> Sie müssen einen TXT-Eintrag unter `@.<domain>` mit dem Wert `<verification-token>` erstellen.

### <a name="assign-certificate-to-app-service-app"></a>Zuweisen des Zertifikats zur App Service-App

Wenn Sie **IP-basiertes SSL** ausgewählt haben und Ihre benutzerdefinierte Domäne einen A-Datensatz verwendet, müssen Sie die folgenden zusätzlichen Schritte ausführen:

Nach der Konfiguration einer IP-basierten SSL-Bindung wird Ihrer App eine dedizierte IP-Adresse zugewiesen. Diese IP-Adresse befindet sich auf der Seite **Benutzerdefinierte Domäne** unter den Einstellungen Ihrer App (direkt über dem Abschnitt **Hostnamen**). Sie ist als **Externe IP-Adresse** angegeben.

![Bild von IP-SSL einfügen](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Diese IP-Adresse unterscheidet sich von der virtuellen IP-Adresse, die zuvor zum Konfigurieren des A-Eintrags für Ihre Domäne verwendet wurde. Wenn SNI-basiertes SSL konfiguriert bzw. die Verwendung von SSL nicht konfiguriert ist, ist für diesen Eintrag keine Adresse angegeben.

Ändern Sie mit den von der Domänennamen-Registrierungsstelle bereitgestellten Tools den A-Datensatz für den benutzerdefinierten Domänennamen, sodass dieser auf die im vorherigen Schritt genannte IP-Adresse verweist.

## <a name="rekey-and-sync-the-certificate"></a>Erneute Schlüsselerstellung für das Zertifikat und Synchronisierung

Wenn Sie erneut einen Schlüssel für Ihr Zertifikat erstellen müssen, wählen Sie auf dem Blatt **Zertifikateigenschaften** die Option **Erstellung neuer Schlüssel und Synchronisierung** aus.

Klicken Sie auf die Schaltfläche **Erneute Schlüsselerstellung**, um den Prozess zu initiieren. Dieser Prozess kann 1 bis 10 Minuten in Anspruch nehmen.

![Bild von erneuter Schlüsselerstellung für SSL einfügen](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Bei erneuter Schlüsselerstellung für Ihr Zertifikat wird von der Zertifizierungsstelle ein neues Zertifikat ausgestellt.

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen eines Content Delivery Network zu einer Azure App Service-Instanz](app-service-web-tutorial-content-delivery-network.md)
