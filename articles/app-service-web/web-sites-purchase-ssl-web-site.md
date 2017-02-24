---
title: "Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service"
description: "Erfahren Sie, wie Sie ein SSL-Zertifikat für Ihren Azure App Service kaufen und konfigurieren."
services: app-service
documentationcenter: .net
author: apurvajo
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
ms.author: apurvajo
translationtype: Human Translation
ms.sourcegitcommit: a1b492b7884deb2d0d4f255af0737e1633606384
ms.openlocfilehash: 0a016d88b8d7a800bf726e4f582deeaaf3bc6ad6


---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service
> [!div class="op_single_selector"]
> * [Kaufen eines SSL-Zertifikats in Azure](web-sites-purchase-ssl-web-site.md)
> * [Verwenden eines SSL-Zertifikats von einer anderen Stelle](web-sites-configure-ssl-certificate.md)
> 
> 

Standardmäßig aktiviert **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)** über ein Platzhalterzertifikat für die Domäne *.azurewebsites.net bereits HTTPS für Ihre Web-App. Wenn Sie keine benutzerdefinierte Domäne konfigurieren möchten, können Sie das Standard-HTTPS-Zertifikat nutzen. *[Platzhalterdomänen](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates) sind jedoch generell nicht so sicher wie die Verwendung einer benutzerdefinierten Domäne mit Ihrem eigenen Zertifikat. Azure App Service bietet Ihnen jetzt eine wirklich einfache Möglichkeit, ein SSL-Zertifikat direkt vom Azure-Portal aus zu erwerben und zu verwalten, ohne das Portal verlassen zu müssen.  
In diesem Artikel erfahren Sie, wie Sie ein SSL-Zertifikat für Ihre **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**-Instanz in 3 einfachen Schritten kaufen und konfigurieren können. 

> [!NOTE]
> SSL-Zertifikate für benutzerdefinierte Domänennamen können nicht mit kostenlosen und freigegebenen Web-Apps verwendet werden. Sie müssen Ihre Web-App für den Basic-, Standard- oder Premium-Modus konfigurieren. Von dieser Einstellung hängt möglicherweise ab, wie Ihr Abonnement abgerechnet wird. Weitere Informationen finden Sie unter **[Web-Apps – Preisübersicht](https://azure.microsoft.com/pricing/details/web-sites/)**.
> 
> 

## <a name="a-namebkmkoverviewaoverview"></a><a name="bkmk_Overview"></a>Übersicht
> [!NOTE]
> Versuchen Sie nicht, den Kauf eines SSL-Zertifikats mithilfe eines Abonnements durchzuführen, dem keine aktive Kreditkarte zugeordnet ist. Dadurch kann das Abonnement deaktiviert werden. 
> 
> 

## <a name="a-purchase-store-and-assign-an-ssl-certificate-for-your-custom-domain-a"></a><a> Erwerben, Speichern und Zuweisen eines SSL-Zertifikats für Ihre benutzerdefinierte Domäne </a>
Wenn Sie HTTPS für eine benutzerdefinierte Domäne wie contoso.com aktivieren möchten, müssen Sie zunächst einen **[benutzerdefinierten Domänennamen in Azure App Service konfigurieren](web-sites-custom-domain-name.md)**.

Bevor Sie ein SSL-Zertifikat anfordern, müssen Sie zuerst festlegen, welche Domänennamen durch das Zertifikat geschützt werden. Davon hängt ab, welchen Typ von Zertifikat Sie benötigen. Wenn Sie lediglich einen einzelnen Domänennamen wie „contoso.com“ oder „www.contoso.com“ schützen müssen, reicht ein Standardzertifikat (Basic) aus. Wenn Sie mehrere Domänennamen sichern müssen – z.B. contoso.com, www.contoso.com und mail.contoso.com –, können Sie ein **[Platzhalterzertifikat](http://en.wikipedia.org/wiki/Wildcard_certificate)** anfordern.

## <a name="a-namebkmkpurchasecertastep-0-place-an-ssl-certificate-order"></a><a name="bkmk_purchasecert"></a>Schritt 0: Bestellen eines SSL-Zertifikats
In diesem Schritt erfahren Sie, wie Sie ein SSL-Zertifikat Ihrer Wahl bestellen.

1. Klicken Sie im **[Azure-Portal](https://portal.azure.com/)** auf „Durchsuchen“, geben Sie „App Service-Zertifikate“ in die Suchleiste ein, wählen Sie „App Service-Zertifikate“ aus dem Suchergebnis aus, und klicken Sie auf „Hinzufügen“. 
   
   ![Bild vom Erstellen mit Durchsuchen einfügen](./media/app-service-web-purchase-ssl-web-site/browse.jpg)
   
   ![Bild vom Erstellen mit Durchsuchen einfügen](./media/app-service-web-purchase-ssl-web-site/add.jpg)
2. Geben Sie einen **Anzeigenamen** für das SSL-Zertifikat ein.
3. Geben Sie einen **Hostnamen**
   
   > [!NOTE]
   > Dies ist einer der wichtigsten Teile des Einkaufsvorgangs. Achten Sie darauf, dass Sie den richtigen Hostnamen (benutzerdefinierte Domäne) eingeben, den Sie mit diesem Zertifikat schützen möchten. Fügen Sie dem Hostnamen **NICHT** WWW an. Wenn der Name Ihrer benutzerdefinierten Domäne z. B. „www.contoso.com“ lautet, dann geben Sie einfach „contoso.com“ in das Feld „Hostname“ ein, und das betreffende Zertifikat schützt sowohl WWW- als auch Stammdomänen. 
   > 
   > 
4. Wählen Sie Ihr **Abonnement**aus. 
   
   Wenn Sie mehrere Abonnements haben, stellen Sie sicher, dass Sie ein SSL-Zertifikat in dem gleichen Abonnement erstellen, das Sie für Ihre entsprechende benutzerdefinierte Domäne oder Web-App verwendet haben.
5. Wählen Sie eine **Ressourcengruppe**aus, oder erstellen Sie eine.
   
   Ressourcengruppen ermöglichen Ihnen, verwandte Azure-Ressourcen als Einheit zu verwalten, und sind hilfreich beim Einrichten von RBAC-Regeln (Role-Based Access Control, rollenbasierte Zugriffssteuerung) für Ihre Apps. Weitere Informationen finden Sie unter „Verwalten Ihrer Azure-Ressourcen“.
6. Wählen Sie die **Zertifikat-SKU** 
   
   Wählen Sie abschließend die Zertifikat-SKU, die Ihren Anforderungen entspricht, und klicken Sie auf „Erstellen“. Zurzeit erlaubt Azure App Service den Erwerb von zwei unterschiedlichen SKUs.
   
        •    S1 – Standard Certificate with 1-year validity and auto renewal  
        •    W1 – Wild card Certificate with 1-year validity and auto renewal      
   Weitere Informationen finden Sie unter **[Web-Apps – Preisübersicht](https://azure.microsoft.com/pricing/details/web-sites/)**.

![Bild von Zertifikat-SKU einfügen](./media/app-service-web-purchase-ssl-web-site/SKU.jpg)

> [!NOTE]
> Das Erstellen von SSL-Zertifikaten von beliebigem Standort aus dauert 1 bis 10 Minuten. Dieser Prozess führt mehrere Schritte im Hintergrund aus, die ansonsten sehr mühsam manuell ausführbar wären.  
> 
> 

## <a name="a-namebkmkstorekeyvaultastep-1-store-the-certificate-in-azure-key-vault"></a><a name="bkmk_StoreKeyVault"></a>Schritt 1: Speichern des Zertifikats in Azure Key Vault
In diesem Schritt erfahren Sie, wie Sie ein SSL-Zertifikat, das Sie erworben haben, im Azure Key Vault Ihrer Wahl speichern.

1. Nachdem Sie das SSL-Zertifikat erworben haben, müssen Sie manuell das Ressourcenblatt **App Service-Zertifikate** öffnen, indem Sie es erneut aufrufen (siehe Schritt 1 oben).   
   
   ![Bild von Bereitschaft zum Speichern in KV einfügen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.jpg)
   
   Sie werden bemerken, dass der Zertifikatstatus **„Ausstehende Ausstellung“** ist, da Sie einige zusätzliche Schritte abschließen müssen, bevor Sie diese Zertifikate verwenden können.
2. Klicken Sie auf dem Blatt „Zertifikateigenschaften“ auf **Zertifikatkonfiguration** und dann auf **Schritt 1: Speichern**, um dieses Zertifikat in Azure Key Vault zu speichern.
3. Klicken Sie auf dem Blatt **Key Vault-Status** auf **Key Vault-Repository**, um einen bestehenden Schlüsseltresor zum Speichern dieses Zertifikats auszuwählen, ODER auf **Neuen Schlüsseltresor erstellen**, um einen neuen Schlüsseltresor im gleichen Abonnement und in der gleichen Ressourcengruppe zu erstellen.
   
   ![Bild vom Erstellen eines neuen KV einfügen](./media/app-service-web-purchase-ssl-web-site/NewKV.jpg)
   
   > [!NOTE]
   > Azure Key Vault berechnet minimale Gebühren für das Speichern dieses Zertifikats. Weitere Informationen finden Sie in der **[Preisübersicht zu Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
   > 
   > 
4. Wenn Sie das Key Vault-Repository zum Speichern dieses Zertifikats ausgewählt haben, speichern Sie es durch Klicken auf die Schaltfläche **Speichern** am oberen Rand des Blatts **Key Vault-Status**.  
   
    Damit ist der Schritt zum Speichern des Zertifikats, das Sie erworben haben, im Azure Key Vault Ihrer Wahl abgeschlossen. Nach dem Aktualisieren des Blatts sollte auch diesen Schritt ein grünes Häkchen markieren.

## <a name="a-namebkmkverifyownershipastep-2-verify-the-domain-ownership"></a><a name="bkmk_VerifyOwnership"></a>Schritt 2: Überprüfen des Domänenbesitzes
In diesem Schritt erfahren Sie, wie Sie die Überprüfung des Domänenbesitzes für ein SSL-Zertifikat ausführen, das Sie gerade bestellt haben. 

1. Klicken Sie auf **Schritt 2: Überprüfen** auf dem Blatt **Zertifikatkonfiguration**. Drei Typen von Domänenüberprüfungen werden von App Service-Zertifikaten unterstützt.
   
   * **Domänenüberprüfung** 
     
     * Dies ist **NUR DANN** der zweckmäßigste Prozess, wenn Sie **[Ihre benutzerdefinierte Domäne von Azure App Service](custom-dns-web-site-buydomains-web-app.md)** erworben haben.
     * Klicken Sie auf die Schaltfläche **„Überprüfen“** , um diesen Schritt abzuschließen.
     * Klicken Sie auf **„Aktualisieren“** , um den Zertifikatsstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.
   * **Überprüfungs-E-Mail**
     
     * Die Überprüfungs-E-Mail wurde bereits an die E-Mail-Adresse(n) gesendet, die dieser benutzerdefinierten Domäne zugeordnet ist/sind.
     * Öffnen Sie die E-Mail, und klicken Sie auf den Link für die Überprüfung, um den Schritt der E-Mail-Bestätigung abzuschließen. 
     * Wenn Sie die Überprüfungs-E-Mail erneut senden müssen, klicken Sie auf die Schaltfläche **„E-Mail erneut senden“** .
   * **Manuelle Überprüfung**    
     
      **Überprüfung des DNS-TXT-Eintrags**
        
        * Erstellen Sie mithilfe des DNS-Managers einen TXT-Eintrag in der Unterdomäne **DZC** mit einem Wert, der dem **Domänenüberprüfungstoken** entspricht.
        * Klicken Sie auf **„Aktualisieren“** , um den Zertifikatsstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.
          
          Um beispielsweise die Validierung für ein Platzhalterzertifikat mit dem Hostnamen **\*.contosocertdemo.com** oder **\*.subdomain.contosocertdemo.com** und dem Domänenüberprüfungstoken **cAGgQrKc** auszuführen, müssen Sie einen TXT-Eintrag auf dzc.contosocertdemo.com mit dem Wert **cAGgQrKc** erstellen.     

## <a name="a-namebkmkassigncertificateastep-3-assign-certificate-to-app-service-app"></a><a name="bkmk_AssignCertificate"></a>Schritt 3: Zuweisen des Zertifikats zur App Service-App
In diesem Schritt erfahren Sie, wie Sie dieses neu erworbene Zertifikat Ihren App Service-Apps zuweisen. 

> [!NOTE]
> Bevor Sie die Schritte in diesem Abschnitt ausführen, müssen Sie einen benutzerdefinierten Domänennamen mit Ihrer App verknüpft haben. Weitere Informationen finden Sie unter **[Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App](web-sites-custom-domain-name.md)**.
> 
> 

1. Öffnen Sie das **[Azure-Portal](https://portal.azure.com/)** in Ihrem Browser.
2. Klicken Sie auf der Seite links auf die Option **App Service** .
3. Klicken Sie auf den Namen Ihrer App, der Sie dieses Zertifikat zuweisen möchten. 
4. Klicken Sie unter **Einstellungen** auf **SSL-Zertifikate**.
5. Klicken Sie auf **Import App Service Certificate** (App Service-Zertifikat importieren), und wählen Sie das soeben erworbene Zertifikat aus.
   
   ![Bild von Zertifikatimport einfügen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)
6. Klicken Sie im Abschnitt **SSL-Bindungen** auf **Bindung hinzufügen**.
7. Wählen Sie auf dem Blatt **Add SSL Binding** (SSL-Bindung hinzufügen) mithilfe der Dropdownlisten den Domänennamen, der mit SSL geschützt werden soll, sowie das zu verwendende Zertifikat aus. Sie können auch auswählen, ob SSL auf der **[Servernamensanzeige](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Server Name Indication, SNI) oder IP basieren soll.
   
    ![Bild von SSL-Bindungen einfügen](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)
   
       •    IP based SSL associates a certificate with a domain name by mapping the dedicated public IP address of the server to the domain name. This requires each domain name (contoso.com, fabricam.com, etc.) associated with your service to have a dedicated IP address. This is the traditional          method of associating SSL certificates with a web server.
       •    SNI based SSL is an extension to SSL and **[Transport Layer Security](http://en.wikipedia.org/wiki/Transport_Layer_Security)** (TLS) that allows multiple domains to share the same IP address, with separate security certificates for each domain. Most modern browsers (including Internet Explorer, Chrome, Firefox and Opera) support SNI, however older browsers may not support SNI. For more information on SNI, see the **[Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)** article on Wikipedia.
8. Klicken Sie auf **Bindung hinzufügen** , um die Änderungen zu speichern und SSL zu aktivieren.

Wenn Sie **IP-basiertes SSL** ausgewählt haben und Ihre benutzerdefinierte Domäne einen A-Datensatz verwendet, müssen Sie die folgenden zusätzlichen Schritte ausführen:

* Nach der Konfiguration einer IP-basierten SSL-Bindung wird Ihrer App eine dedizierte IP-Adresse zugewiesen. Diese IP-Adresse befindet sich auf der Seite **Benutzerdefinierte Domäne** unter den Einstellungen Ihrer App (direkt über dem Abschnitt **Hostnamen**). Sie ist als **Externe IP-Adresse**
  
    ![Bild von IP-SSL einfügen](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)
  
    Diese IP-Adresse unterscheidet sich von der virtuellen IP-Adresse, die zuvor zum Konfigurieren des A-Datensatzes für Ihre Domäne verwendet wurde. Wenn SNI-basiertes SSL konfiguriert bzw. wenn die Verwendung von SSL nicht konfiguriert ist, wird für diesen Eintrag keine Adresse aufgeführt.

1. Ändern Sie mit den von der Domänennamen-Registrierungsstelle bereitgestellten Tools den A-Datensatz für den benutzerdefinierten Domänennamen, sodass dieser auf die im vorherigen Schritt genannte IP-Adresse verweist.
   Sie sollten jetzt in der Lage sein, Ihre App über HTTPS:// anstelle von HTTP:// zu erreichen. Sie können damit überprüfen, ob das Zertifikat richtig konfiguriert wurde.

## <a name="a-namebkmkrekeyarekey-and-sync-the-certificate"></a><a name="bkmk_Rekey"></a>Erneute Schlüsselerstellung für das Zertifikat und Synchronisierung
1. Wenn Sie jemals erneut einen Schlüssel für Ihr Zertifikat erstellen müssen, wählen Sie aus Sicherheitsgründen einfach die Option **Erstellung neuer Schlüssel und Synchronisierung** auf dem Blatt **Zertifikateigenschaften** aus. 
2. Klicken Sie auf die Schaltfläche **„Erneute Schlüsselerstellung“** , um den Prozess zu initiieren. Dieser Prozess kann 1 bis 10 Minuten in Anspruch nehmen. 
   
    ![Bild von erneuter Schlüsselerstellung für SSL einfügen](./media/app-service-web-purchase-ssl-web-site/Rekey.jpg)
3. Bei erneuter Schlüsselerstellung für Ihr Zertifikat wird ein neues Zertifikat von der Zertifizierungsstelle ausgestellt.
4. Für die erneute Schlüsselerstellung werden Ihnen für die Gültigkeitsdauer des Zertifikats keine Gebühren in Rechnung gestellt. 
5. Die erneute Schlüsselerstellung für Ihr Zertifikat durchläuft den Status „Ausstehende Ausstellung“. 
6. Sobald das Zertifikat bereit ist, stellen Sie sicher, dass Sie Ihre Ressourcen mithilfe dieses Zertifikats synchronisieren, um eine Unterbrechung des Diensts zu verhindern.
7. Die Synchronisierungsoption ist nicht für Zertifikate verfügbar, die noch nicht der Web-App zugewiesen sind. 

## <a name="more-resources"></a>Weitere Ressourcen
* [Aktivieren von HTTPS für eine App in Azure App Service](web-sites-configure-ssl-certificate.md)
* [Kaufen und Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
* [Microsoft Azure Trust Center](/support/trust-center/security/)
* [Konfigurationsoptionen in Azure-Websites](http://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)
* [Azure-Verwaltungsportal](https://manage.windowsazure.com)

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 




<!--HONumber=Feb17_HO2-->


