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
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 00e252e249dbd1a38a4649e435071685860722e4
ms.lasthandoff: 04/07/2017


---

# <a name="add-an-ssl-certificate-to-your-app-service-app"></a>Hinzufügen eines SSL-Zertifikats zu Ihrer App Service-App
> [!div class="op_single_selector"]
> * [Kaufen eines SSL-Zertifikats in Azure](web-sites-purchase-ssl-web-site.md)
> * [Verwenden eines SSL-Zertifikats von einer anderen Stelle](web-sites-configure-ssl-certificate.md)
> 
> 

Standardmäßig aktiviert [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) HTTPS für Ihre Web-App über ein Platzhalterzertifikat für die Domäne „\*.azurewebsites.net“. Wenn Sie keine benutzerdefinierte Domäne einrichten möchten, können Sie das HTTPS-Standardzertifikat nutzen. Wie alle [Platzhalterdomänen](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates) ist das Azure-Platzhalterzertifikat jedoch nicht so sicher wie die Verwendung einer benutzerdefinierten Domäne mit Ihrem eigenen Zertifikat.

Dank App Service können Sie ein SSL-Zertifikat ganz einfach im Azure-Portal erwerben und verwalten. 

In diesem Artikel erfahren Sie, wie Sie ein SSL-Zertifikat für Ihre [App Service](http://go.microsoft.com/fwlink/?LinkId=529714)-App kaufen und einrichten. 

> [!NOTE]
> Sie können SSL-Zertifikate nicht für benutzerdefinierte Domänennamen für Apps verwenden, die in einem App Service-Plan vom Typ „Free“ oder „Shared“ gehostet werden. Damit Sie SSL-Zertifikate verwenden können, muss Ihre Web-App in einem App Service-Plan vom Typ „Basic“, „Standard“ oder „Premium“ gehostet werden. Durch die Änderung des Abonnementtyps können sich auch die Kosten für Ihr Abonnement ändern. Weitere Informationen finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/web-sites/).
> 
> 

> [!WARNING]
> Versuchen Sie nicht, ein SSL-Zertifikat mithilfe eines Abonnements zu erwerben, für das keine gültige Kreditkarte hinterlegt wurde. Dies kann dazu führen, dass Ihr Abonnement deaktiviert wird. 
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Um HTTPS für eine benutzerdefinierte Domäne zu aktivieren, beginnen Sie zunächst mit dem [Zuordnen eines benutzerdefinierten Domänennamens zu Ihrer Azure-App](web-sites-custom-domain-name.md).

Bevor Sie ein SSL-Zertifikat anfordern, legen Sie zuerst fest, welche Domänennamen durch das Zertifikat geschützt werden. Dies bestimmt den Typ des erforderlichen Zertifikats. Wenn Sie einen einzelnen Domänennamen wie „contoso.com“ *oder* „www.contoso.com“ schützen möchten, können Sie ein Standardzertifikat (Basic) verwenden. Wenn Sie mehrere Domänennamen sichern müssen, z.B. „contoso.com“, „www.contoso.com“ *und* „mail.contoso.com“, können Sie ein [Platzhalterzertifikat](http://en.wikipedia.org/wiki/Wildcard_certificate) anfordern.

## <a name="bkmk_purchasecert"></a>Kaufen eines SSL-Zertifikats

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Durchsuchen**. Geben Sie im Feld **Suchen** den Begriff **App Service-Zertifikat** ein. Wählen Sie in den Suchergebnissen **App Service-Zertifikat** aus. 

   ![Erstellen mithilfe von „Durchsuchen“](./media/app-service-web-purchase-ssl-web-site/browse.jpg)
   
2. Wählen Sie auf der Seite **App Service-Zertifikate** die Option **Hinzufügen**. 

   ![Hinzufügen eines Zertifikats](./media/app-service-web-purchase-ssl-web-site/add.jpg)

3. Geben Sie einen Namen ****für das SSL-Zertifikat ein.
4. Geben Sie den Hostnamen ein****.
   
   > [!WARNING]
   > Dies ist einer der wichtigsten Teile des Einkaufsvorgangs. Geben Sie unbedingt den richtigen Hostnamen (benutzerdefinierter Domänenname) ein, der von diesem Zertifikat geschützt werden soll. Fügen Sie am Anfang des Hostennamens *nicht*  „www“ hinzu. Wenn der benutzerdefinierte Domänenname beispielsweise „www.contoso.com“ lautet, geben Sie im Feld **Hostname** den Namen **contoso.com** ein. Das Zertifikat schützt „www“ und Stammdomänen. 
   > 

5. Wählen Sie Ihr **Abonnement**aus. 
   
   Wenn Sie mehrere Abonnements haben, erstellen Sie das SSL-Zertifikat in dem gleichen Abonnement, das Sie für Ihre benutzerdefinierte Domäne oder Web-App verwenden.

6. Wählen Sie eine **Ressourcengruppe**aus, oder erstellen Sie eine.
   
   Sie können mithilfe von Ressourcengruppen verwandte Azure-Ressourcen als Einheit verwalten. Ressourcengruppen sind nützlich, wenn Sie Regeln für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Ihre Apps festlegen möchten. Weitere Informationen finden Sie unter „Verwalten Ihrer Azure-Ressourcen“.

7. Wählen Sie die **Zertifikat-SKU**. 
   
   Wählen Sie die Zertifikat-SKU, die Ihren Anforderungen entspricht, und klicken Sie auf **Erstellen**. 
   
   Sie können in App Service zwischen zwei SKUS wählen:
   * **S1**: Standard-Zertifikat mit einem Jahr Gültigkeit und automatischer Verlängerung  
   * **W1**: Platzhalterzertifikat mit einem Jahr Gültigkeit und automatischer Verlängerung       
  
    ![Zertifikat-SKU](./media/app-service-web-purchase-ssl-web-site/SKU.jpg)

    Weitere Informationen finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/web-sites/).

> [!NOTE]
> Die Erstellung eines SSL-Zertifikats kann bis zu zehn Minuten dauern. Der Prozess umfasst mehrere Schritte, die im Hintergrund ausgeführt werden.  
> 
> 

## <a name="bkmk_StoreKeyVault"></a>Speichern des Zertifikats in Azure Key Vault

1. Wenn Sie den Kauf des SSL-Zertifikats abgeschlossen haben, navigieren Sie im Azure-Portal zum Blatt **App Service-Zertifikate**.

   ![Zertifikat bereit für die Speicherung in Key Vault](./media/app-service-web-purchase-ssl-web-site/ReadyKV.jpg)
   
   Beachten Sie, dass der Zertifikatstatus **Ausstellung steht aus** lautet. Bevor dieses Zertifikat verwendet werden kann, sind einige Schritte erforderlich.

2. Wählen Sie auf dem Blatt **Zertifikateigenschaften** die Option **Zertifikatkonfiguration**. Wählen Sie zum Speichern dieses Zertifikats in Key Vault **Schritt 1: Speichern**.
3. Wählen Sie auf dem Blatt **Key Vault-Status** die Option **Key Vault-Repository**, um einen vorhandenen Schlüsseltresor zum Speichern dieses Zertifikats festzulegen.  Wählen Sie zum Erstellen eines neuen Schlüsseltresors im gleichen Abonnement und in der gleichen Ressourcengruppe **Neuen Schlüsseltresor erstellen**.

   ![Erstellen eines neuen Schlüsseltresors](./media/app-service-web-purchase-ssl-web-site/NewKV.jpg)
   
   > [!NOTE]
   > Azure Key Vault berechnet minimale Gebühren für das Speichern des Zertifikats. Weitere Informationen finden Sie auf der Seite [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/).
   > 
   > 

4. Wenn Sie das Key Vault-Repository zum Speichern des Zertifikats ausgewählt haben, wählen Sie oben auf dem Blatt **Key Vault-Status** die Schaltfläche **Speichern**.  
   
Zum Überprüfen Ihrer Auswahl können Sie in Ihrem Browser auf die Schaltfläche „Aktualisieren“ klicken. Ein grünes Häkchen gibt an, dass dieser Schritt abgeschlossen ist.

## <a name="bkmk_VerifyOwnership"></a>Überprüfen des Domänenbesitzes

1. Wählen Sie auf dem Blatt **Zertifikatkonfiguration** die Option **Schritt 2: Überprüfen**.
2. Wählen Sie anhand der folgenden Informationen Überprüfungsoptionen aus. 

App Service-Zertifikate unterstützen drei Typen von Domänenüberprüfungen:

   * Domänenüberprüfung
   * Überprüfung per E-Mail
   * Manuelle Überprüfung

### <a name="domain-verification"></a>Domänenüberprüfung 
     
Die Domänenüberprüfung ist *nur dann* der zweckmäßigste Prozess, wenn Sie [Ihre benutzerdefinierte Domäne von Azure App Service](custom-dns-web-site-buydomains-web-app.md) erworben haben.

1. Wählen Sie zum Abschließen dieses Schritts **Überprüfen**.
2. Wählen Sie **Aktualisieren**, um den Zertifikatstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.

### <a name="mail-verification"></a>Überprüfung per E-Mail
     
Bei einer benutzerdefinierten Domäne wird eine Überprüfungs-E-Mail an die E-Mail-Adresse gesendet, die der Domäne zugeordnet ist. 

1. Öffnen Sie die E-Mail, und klicken Sie auf den Link für die Überprüfung, um den Schritt der E-Mail-Bestätigung abzuschließen. 
2. Wenn Sie die Überprüfungs-E-Mail erneut senden müssen, klicken Sie auf die Schaltfläche **E-Mail erneut senden**.

### <a name="manual-verification"></a>Manuelle Überprüfung    
     
**HTML-Webseitenüberprüfung (funktioniert nur mit Standardzertifikat-SKU)**

1. Erstellen Sie eine HTML-Datei mit dem Namen „starfield.html“. Der Inhalt dieser Datei sollte genau dem Namen des Domänenüberprüfungstokens entsprechen. (Sie können das Token auf dem Blatt mit dem Domänenüberprüfungsstatus**** kopieren.)
2. Laden Sie diese Datei in den Stamm des Webservers hoch, auf dem Ihre Domäne gehostet wird. Beispiel: /.well-known/pki-validation/starfield.html.
3.  Wählen Sie nach Abschluss der Überprüfung **Aktualisieren**, um den Zertifikatstatus zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.

    Wenn Sie beispielsweise ein Standardzertifikat für **contosocertdemo.com** mit dem Domänenüberprüfungstoken **tgjgthq8d11ttaeah97s3fr2sh** kaufen und anschließend eine Webanforderung an **http://contosocertdemo.com/.well-known/pki-validation/starfield.html** durchgeführt wird, sollte **tgjgthq8d11ttaeah97s3fr2sh** zurückgeben werden.

**Überprüfung des DNS-TXT-Eintrags**
        
1. Erstellen Sie mithilfe des DNS-Managers einen TXT-Eintrag in der Unterdomäne **@** mit einem Wert, der dem **Domänenüberprüfungstoken** entspricht.
2. Wählen Sie **Aktualisieren**, um den Zertifikatstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.
 
   Um beispielsweise die Validierung für ein Platzhalterzertifikat mit dem Hostnamen **\*.contosocertdemo.com** oder **\*.subdomain.contosocertdemo.com** und dem Domänenüberprüfungstoken **tgjgthq8d11ttaeah97s3fr2sh** durchzuführen, erstellen Sie einen TXT-Eintrag auf **contosocertdemo.com** mit dem Wert **tgjgthq8d11ttaeah97s3fr2sh**.     

## <a name="bkmk_AssignCertificate"></a>Zuweisen des Zertifikats zu einer App Service-App

> [!NOTE]
> Bevor Sie die Schritte in diesem Abschnitt ausführen, müssen Sie einen benutzerdefinierten Domänennamen mit Ihrer App verknüpfen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App](web-sites-custom-domain-name.md).
> 
> 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) im Menü **App Service**.
2. Wählen Sie den Namen Ihrer App, der Sie dieses Zertifikat zuweisen möchten. 
3. Navigieren Sie zu **Einstellungen** > **SSL-Zertifikate** > **App Service-Zertifikat importieren**, und wählen Sie dann das Zertifikat aus.

   ![Importieren des Zertifikats](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

4. Wählen Sie im Abschnitt **SSL-Bindungen** die Option **Add bindings** (Bindungen hinzufügen).
5. Wählen Sie auf dem Blatt **SSL-Bindung hinzufügen** den Domänennamen aus, den Sie mit dem SSL-Zertifikat sichern möchten. Wählen Sie das Zertifikat aus, das Sie verwenden möchten. Sie können auch auswählen, ob SSL auf der [Servernamensanzeige](http://en.wikipedia.org/wiki/Server_Name_Indication) (Server Name Indication, SNI) oder IP basieren soll.

   ![SSL-Bindungen](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)
   
    * Zum Zuweisen eines Zertifikats zu einem Domänennamen ordnet IP-basiertes SSL die dedizierte öffentliche IP-Adresse des Servers dem Domänennamen zu. Bei der Verwendung von IP-basiertem SSL muss jeder Ihrem Dienst zugeordnete Domänenname (etwa „contoso.com“ oder „fabricam.com“) eine dedizierte IP-Adresse besitzen. Dies ist die herkömmliche Methode zum Verknüpfen von SSL-Zertifikaten mit einem Webserver.
    * SNI-basiertes SSL ist eine Erweiterung von SSL und [Transport Layer Security](http://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS). Wenn Sie SNI-basiertes SSL verwenden, können mehrere Domänen die gleiche IP-Adresse nutzen. Jede Domäne besitzt ein separates Sicherheitszertifikat. Die meisten modernen Browser (einschließlich Internet Explorer, Chrome, Firefox und Opera) unterstützen SNI. Von älteren Browsern wird SNI nicht unterstützt. Weitere Informationen zu SNI finden Sie im Wikipedia-Artikel [Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication).

6. Wählen Sie **Bindung hinzufügen**, um die Änderungen zu speichern und SSL zu aktivieren.

Wenn Sie **IP-basiertes SSL** auswählen und Ihre benutzerdefinierte Domäne einen A-Datensatz verwendet, müssen Sie die folgenden zusätzlichen Schritte ausführen:

1.  Nach der Konfiguration einer IP-basierten SSL-Bindung wird Ihrer App eine dedizierte IP-Adresse zugewiesen. Die IP-Adresse finden Sie unter **Einstellungen** > **Benutzerdefinierte Domäne**. Direkt über dem Abschnitt **Hostnamen** ist Ihre IP-Adresse als **Externe IP-Adresse** aufgeführt.

   ![IP-basiertes SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)
    
  Diese IP-Adresse unterscheidet sich von der virtuellen IP-Adresse, die zuvor zum Konfigurieren des A-Datensatzes für Ihre Domäne verwendet wurde. Wenn Ihre App für die Verwendung von SNI-basiertem SSL oder nicht für die Verwendung von SSL konfiguriert wird, ist hier keine IP-Adresse angegeben.

2.  Ändern Sie mit den von der Domänennamen-Registrierungsstelle bereitgestellten Tools den A-Datensatz für den benutzerdefinierten Domänennamen, sodass dieser auf die im vorherigen Schritt verwendete IP-Adresse verweist.

3.  Um die richtige Konfiguration des Zertifikats zu überprüfen, rufen Sie Ihre App mithilfe von „HTTPS:// instead of HTTP://“ auf.

## <a name="bkmk_Export"></a>Exportieren eines App Service-Zertifikats
Sie können eine lokale PFX-Kopie eines App Service-Zertifikats erstellen. Wenn Sie eine lokale Kopie besitzen, können Sie das Zertifikat mit anderen Azure-Diensten verwenden. Weitere Informationen finden Sie in unserem Blogbeitrag [Create a local PFX copy of your App Service certificate](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/) (Erstellen einer lokalen PFX-Kopie des App Service-Zertifikats).

## <a name="bkmk_Renew"></a>Automatisches Verlängern des App Service-Zertifikats
Um die Einstellungen für die automatische Verlängerung Ihres Zertifikats festzulegen oder Ihr Zertifikat manuell zu verlängern, wählen Sie auf dem Blatt **Zertifikateigenschaften** die Option **Einstellungen für die automatische Verlängerung**. 

![Einstellungen für die automatische Verlängerung](./media/app-service-web-purchase-ssl-web-site/autorenew.png)

Legen Sie **Automatisch verlängern** auf **Ein** fest, um das Zertifikat vor dem Ablauf automatisch zu verlängern. Dies ist die Standardoption. Wenn die automatische Verlängerung aktiviert ist, versuchen wir, Ihr Zertifikat ab dem 90. Tag vor Ablauf zu verlängern. Wenn Sie SSL-Bindungen für Ihre App Service-Apps im Azure-Portal erstellt haben, werden die Bindungen auch aktualisiert, wenn das neue Zertifikat bereitsteht (wie in Szenarien mit erneuter Schlüsselerstellung und Synchronisierung). 

Wenn Sie Verlängerungen manuell festlegen möchten, wählen Sie für **Automatisch verlängern** die Option **Aus**. Sie können ein App Service-Zertifikat erst manuell verlängern, wenn das Ablaufdatum innerhalb der nächsten 90 Tage liegt.

## <a name="bkmk_Rekey"></a>Erneute Schlüsselerstellung für das Zertifikat und Synchronisierung

1. Wenn Sie jemals (aus Sicherheitsgründen) erneut einen Schlüssel für Ihr Zertifikat erstellen müssen, wählen Sie auf dem Blatt **Zertifikateigenschaften** die Option **Erstellung neuer Schlüssel und Synchronisierung** aus. 
2. Wählen Sie **Neue Schlüssel erstellen**. Die Ausführung des Vorgangs kann bis zu zehn Minuten dauern. 

   ![Erneute Schlüsselerstellung für SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.jpg)

Hier finden Sie einige zusätzliche Informationen zum Erstellen neuer Schlüssel:

* Beim Erstellen neuer Schlüssel für Ihr Zertifikat wird das Zertifikat durch ein neues Zertifikat ersetzt. Das neue Zertifikat wird von der Zertifizierungsstelle ausgestellt.
* Für die erneute Schlüsselerstellung werden Ihnen für die Gültigkeitsdauer des Zertifikats keine Gebühren in Rechnung gestellt. 
* Durch die Erstellung neuer Schlüssel erhält Ihr Zertifikat den Status **Ausstellung steht aus**. 
* Wenn das Zertifikat bereitsteht, stellen Sie sicher, dass Sie Ihre Ressourcen mit diesem Zertifikat synchronisieren, um eine Unterbrechung des Diensts zu vermeiden.
* Die Synchronisierungsoption ist nicht für Zertifikate verfügbar, die noch keiner Web-App zugewiesen sind. 

## <a name="next-steps"></a>Nächste Schritte

* [Sichern Ihrer benutzerdefinierten App-Domäne mit HTTPS](web-sites-configure-ssl-certificate.md)
* [Kaufen und Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
* [Microsoft Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/)

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto registrieren, besuchen Sie [Azure App Service testen](https://azure.microsoft.com/try/app-service/). Sie können eine kurzlebige Starter-Web-App in App Service erstellen. Dafür ist keine Kreditkarte erforderlich, und es entstehen Ihnen keine Verpflichtungen.
> 
> 

