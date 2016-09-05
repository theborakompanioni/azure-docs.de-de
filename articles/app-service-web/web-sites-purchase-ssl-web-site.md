<properties
	pageTitle="Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service"
	description="Erfahren Sie, wie Sie ein SSL-Zertifikat für Ihren Azure App Service kaufen und konfigurieren."
	services="app-service"
	documentationCenter=".net"
	authors="apurvajo"
	manager="stefsch"
	editor="cephalin"
	tags="buy-ssl-certificates"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/01/2016"
	ms.author="apurvajo"/>

#Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service

> [AZURE.SELECTOR]
- [Kaufen eines SSL-Zertifikats in Azure](web-sites-purchase-ssl-web-site.md)
- [Verwenden eines SSL-Zertifikats von einer anderen Stelle](web-sites-configure-ssl-certificate.md)

Standardmäßig aktiviert **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)** über ein Platzhalterzertifikat für die Domäne „*.azurewebsites.net“ bereits HTTPS für Ihre Web-App. Wenn Sie keine benutzerdefinierte Domäne konfigurieren möchten, können Sie das Standard-HTTPS-Zertifikat nutzen. *[Platzhalterdomänen](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates) sind jedoch generell nicht so sicher wie die Verwendung einer benutzerdefinierten Domäne mit Ihrem eigenen Zertifikat. Azure App Service bietet Ihnen jetzt eine wirklich einfache Möglichkeit, ein SSL-Zertifikat direkt vom Azure-Portal aus zu erwerben und zu verwalten, ohne das Portal verlassen zu müssen. In diesem Artikel erfahren Sie, wie Sie ein SSL-Zertifikat für Ihren **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)** in 3 einfachen Schritten kaufen und konfigurieren können.

> [AZURE.NOTE]
SSL-Zertifikate für benutzerdefinierte Domänennamen können nicht mit kostenlosen und freigegebenen Web-Apps verwendet werden. Sie müssen Ihre Web-App für den Basic-, Standard- oder Premium-Modus konfigurieren. Von dieser Einstellung hängt möglicherweise ab, wie Ihr Abonnement abgerechnet wird. Weitere Informationen finden Sie unter **[App Service – Preise](https://azure.microsoft.com/pricing/details/web-sites/)**.



##<a name="bkmk_Overview"></a>Übersicht
> [AZURE.NOTE]
Versuchen Sie nicht, den Kauf eines SSL-Zertifikats mithilfe eines Abonnements durchzuführen, dem keine aktive Kreditkarte zugeordnet ist. Dadurch kann das Abonnement deaktiviert werden.

##<a> Erwerben, Speichern und Zuweisen eines SSL-Zertifikats für Ihre benutzerdefinierte Domäne </a>
Wenn Sie HTTPS für eine benutzerdefinierte Domäne wie „contoso.com“ aktivieren möchten, müssen Sie zunächst einen **[benutzerdefinierten Domänennamen in Azure App Service konfigurieren](web-sites-custom-domain-name.md)**.

Bevor Sie ein SSL-Zertifikat anfordern, müssen Sie zuerst festlegen, welche Domänennamen durch das Zertifikat geschützt werden. Davon hängt ab, welchen Typ von Zertifikat Sie benötigen. Wenn Sie lediglich einen einzelnen Domänennamen wie „contoso.com“ oder „www.contoso.com“ schützen müssen, reicht ein Standardzertifikat (Basic) aus. Wenn Sie mehrere Domänennamen sichern müssen – z.B. „contoso.com“, „www.contoso.com“ und „mail.contoso.com“ – erhalten Sie ein **[Platzhalterzertifikat](http://en.wikipedia.org/wiki/Wildcard_certificate)**.

##<a name="bkmk_purchasecert"></a>Schritt 0: Bestellen eines SSL-Zertifikats

In diesem Schritt erfahren Sie, wie Sie ein SSL-Zertifikat Ihrer Wahl bestellen.

1.	Klicken Sie im **[Azure-Portal](https://portal.azure.com/)** auf „Durchsuchen“, geben Sie „App Service-Zertifikate“ in die Suchleiste ein, wählen Sie „App Service-Zertifikate“ aus dem Suchergebnis, und klicken Sie auf „Hinzufügen“.

    ![Bild vom Erstellen mit Durchsuchen einfügen](./media/app-service-web-purchase-ssl-web-site/browse.jpg)

    ![Bild vom Erstellen mit Durchsuchen einfügen](./media/app-service-web-purchase-ssl-web-site/add.jpg)

2.	Geben Sie einen **Anzeigenamen** für das SSL-Zertifikat ein.

3.	Geben Sie den **Hostnamen** ein.
> [AZURE.NOTE]
    Dies ist einer der wichtigsten Teile des Einkaufsvorgangs. Achten Sie darauf, dass Sie den richtigen Hostnamen (benutzerdefinierte Domäne) eingeben, den Sie mit diesem Zertifikat schützen möchten. Fügen Sie dem Hostnamen **NICHT** WWW an. Wenn der Name Ihrer benutzerdefinierten Domäne z. B. „www.contoso.com“ lautet, dann geben Sie einfach „contoso.com“ in das Feld „Hostname“ ein, und das betreffende Zertifikat schützt sowohl WWW- als auch Stammdomänen.
    
4.	Wählen Sie Ihr **Abonnement** aus.

    Wenn Sie mehrere Abonnements haben, stellen Sie sicher, dass Sie ein SSL-Zertifikat in dem gleichen Abonnement erstellen, das Sie für Ihre entsprechende benutzerdefinierte Domäne oder Web-App verwendet haben.
       
5.	Wählen Sie eine **Ressourcengruppe** aus, oder erstellen Sie eine.

    Ressourcengruppen ermöglichen Ihnen, verwandte Azure-Ressourcen als Einheit zu verwalten, und sind hilfreich beim Einrichten von RBAC-Regeln (Role-Based Access Control, rollenbasierte Zugriffssteuerung) für Ihre Apps. Weitere Informationen finden Sie unter „Verwalten Ihrer Azure-Ressourcen“.
     
6.	Wählen Sie die **Zertifikat-SKU** aus.

    Wählen Sie abschließend die Zertifikat-SKU, die Ihren Anforderungen entspricht, und klicken Sie auf „Erstellen“. Heute können Sie mit Azure App Service zwei verschiedene SKUs erwerben • S1 – Standardzertifikat mit 1 Jahr Gültigkeit und automatischer Verlängerung • W1 – Platzhalterzertifikat mit 1 Jahr Gültigkeit und automatischer Verlängerung Weitere Informationen finden Sie unter **[App Service – Preise](https://azure.microsoft.com/pricing/details/web-sites/)**.

![Bild von Zertifikat-SKU einfügen](./media/app-service-web-purchase-ssl-web-site/SKU.jpg)

> [AZURE.NOTE]
Das Erstellen von SSL-Zertifikaten von beliebigem Standort aus dauert 1 bis 10 Minuten. Dieser Prozess führt mehrere Schritte im Hintergrund aus, die ansonsten sehr mühsam manuell ausführbar wären.

##<a name="bkmk_StoreKeyVault"></a>Schritt 1: Speichern des Zertifikats in Azure Key Vault

In diesem Schritt erfahren Sie, wie Sie ein SSL-Zertifikat, das Sie erworben haben, im Azure Key Vault Ihrer Wahl speichern.

1.	Nachdem Sie das SSL-Zertifikat erworben haben, müssen Sie manuell das Ressourcenblatt **App Service-Zertifikate** öffnen, indem Sie es erneut aufrufen (siehe Schritt 1 oben).

    ![Bild von Bereitschaft zum Speichern in KV einfügen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.jpg)

    Sie werden bemerken, dass der Zertifikatstatus **„Ausstehende Ausstellung“** ist, da Sie einige zusätzliche Schritte abschließen müssen, bevor Sie diese Zertifikate verwenden können.
 
2. Klicken Sie im Blatt „Zertifikateigenschaften“ auf **„Zertifikatkonfiguration“**, und klicken Sie auf **„Schritt 1: Speichern“**, um dieses Zertifikat in Azure Key Vault zu speichern.

3.	Klicken Sie im Blatt **„Schlüsseltresorstatus“** auf **„Schlüsseltresorrepository“**, um einen bestehenden Schlüsseltresor zum Speichern dieses Zertifikats auszuwählen, ODER auf **„Neuen Schlüsseltresor erstellen“**, um einen neuen Schlüsseltresor im gleichen Abonnement und in der gleichen Ressourcengruppe zu erstellen.
 
    ![Bild vom Erstellen eines neuen KV einfügen](./media/app-service-web-purchase-ssl-web-site/NewKV.jpg)
 
    > [AZURE.NOTE]
    Azure Key Vault berechnet minimale Gebühren für das Speichern dieses Zertifikats. Weitere Informationen finden Sie in der **[Preisübersicht zu Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.

4. Wenn Sie das Schlüsseltresorrepository zum Speichern dieses Zertifikats ausgewählt haben, speichern Sie es durch Klicken auf die Schaltfläche **„Speichern“** am oberen Rand des Blatts **„Schlüsseltresorstatus“**.

    Damit ist der Schritt zum Speichern des Zertifikats, das Sie erworben haben, im Azure Key Vault Ihrer Wahl abgeschlossen. Nach dem Aktualisieren des Blatts sollte auch diesen Schritt ein grünes Häkchen markieren.
    
##<a name="bkmk_VerifyOwnership"></a>Schritt 2: Überprüfen des Domänenbesitzes

In diesem Schritt erfahren Sie, wie Sie die Überprüfung des Domänenbesitzes für ein SSL-Zertifikat ausführen, das Sie gerade bestellt haben.

1.	Klicken Sie auf **„Schritt 2: Überprüfen“** im Blatt **„Zertifikatkonfiguration“**. Vier Typen von Domänenüberprüfungen werden von App Service-Zertifikaten unterstützt.

    * **App Service-Überprüfung**
    
        * Dies ist der zweckmäßigste Prozess, falls Sie bereits **Ihre benutzerdefinierte Domäne den App Service-Apps zugewiesen haben**. Diese Methode listet alle App Service-Apps auf, die diese Kriterien erfüllen. In diesem Fall ist z.B. **contosocertdemo.com** eine benutzerdefinierte Domäne, die der App Service-App **„ContosoCertDemo“** zugewiesen ist und daher als einzige App Service-App hier aufgeführt wird. Bei einer Bereitstellung in mehreren Regionen würde sie in allen Regionen aufgelistet.
        
           Die Überprüfungsmethode ist NUR für Käufe von Standardzertifikaten (Basic) verfügbar. Für Platzhalterzertifikate überspringen Sie diesen Schritt bitte und fahren Sie mit Option B, C oder D unten fort.
        * Klicken Sie auf die Schaltfläche **„Überprüfen“**, um diesen Schritt abzuschließen.
        * Klicken Sie auf **„Aktualisieren“**, um den Zertifikatsstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.
        
        ![Bild von App Service-Überprüfung einfügen](./media/app-service-web-purchase-ssl-web-site/AppVerify.jpg)

    * **Domänenüberprüfung**

        * Dies ist **NUR DANN** der zweckmäßigste Prozess, wenn Sie **[Ihre benutzerdefinierte Domäne von Azure App Service](custom-dns-web-site-buydomains-web-app.md)** erworben haben.
        
        * Klicken Sie auf die Schaltfläche **„Überprüfen“**, um diesen Schritt abzuschließen.
        
        * Klicken Sie auf **„Aktualisieren“**, um den Zertifikatsstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.

    * **Überprüfungs-E-Mail**
        
        * Die Überprüfungs-E-Mail wurde bereits an die E-Mail-Adresse(n) gesendet, die dieser benutzerdefinierten Domäne zugeordnet ist/sind.
         
        * Öffnen Sie die E-Mail, und klicken Sie auf den Link für die Überprüfung, um den Schritt der E-Mail-Bestätigung abzuschließen.
        
        * Wenn Sie die Überprüfungs-E-Mail erneut senden müssen, klicken Sie auf die Schaltfläche **„E-Mail erneut senden“**.
         
    * **Manuelle Überprüfung**
                 
        1. **Überprüfung der HTML-Webseite**
        
            * Erstellen Sie eine HTML-Datei mit dem Namen **{Domänenüberprüfungstoken}**.html (Sie können das Token aus dem Blatt „Domänenüberprüfungsstatus“ kopieren).
            
            * Der Inhalt dieser Datei sollte genau dem Namen des **Domänenüberprüfungstokens** entsprechen.
            
            * Aktualisieren Sie diese Datei im Stammverzeichnis des Webservers, der Ihre Domäne hostet.
            
            * Klicken Sie auf **„Aktualisieren“**, um den Zertifikatsstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.
            
            Wenn Sie beispielsweise ein Standardzertifikat für „contosocertdemo.com“ mit dem Domänenüberprüfungstoken **'cAGgQrKc'** und einer Webanforderung an **'http://contosocertdemo.com/cAGgQrKc.html'** kaufen, sollte **cAGgQrKc** zurückgeben werden.
        2. **Überprüfung des DNS-TXT-Eintrags**

            * Erstellen Sie mithilfe des DNS-Managers einen TXT-Eintrag in der Unterdomäne **‘DZC’** mit einem Wert, der dem **Domänenüberprüfungstoken** entspricht.
            
            * Klicken Sie auf **„Aktualisieren“**, um den Zertifikatsstatus nach Abschluss der Überprüfung zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.
                              
            Um beispielsweise die Validierung für ein Platzhalterzertifikat mit dem Hostnamen ***. contosocertdemo.com** oder ***. subdomain.contosocertdemo.com** und dem Domänenüberprüfungstoken **cAGgQrKc** auszuführen, müssen Sie einen TXT-Eintrag auf „dzc.contosocertdemo.com“ mit dem Wert **cAGgQrKc** erstellen.


##<a name="bkmk_AssignCertificate"></a>Schritt 3: Zuweisen des Zertifikats zur App Service-App

In diesem Schritt erfahren Sie, wie Sie dieses neu erworbene Zertifikat Ihren App Service-Apps zuweisen.

> [AZURE.NOTE]
Bevor Sie die Schritte in diesem Abschnitt ausführen, müssen Sie einen benutzerdefinierten Domänennamen mit Ihrer App verknüpft haben. Weitere Informationen finden Sie unter **[Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](web-sites-custom-domain-name.md)**.

1.	Öffnen Sie in Ihrem Browser das **[Azure-Portal](https://portal.azure.com/)**.
2.	Klicken Sie auf der Seite links auf die Option **App Service**.
3.	Klicken Sie auf den Namen Ihrer App, der Sie dieses Zertifikat zuweisen möchten.
4.	Klicken Sie unter **Einstellungen** auf **SSL-Zertifikate**.
5.	Klicken Sie auf **Import App Service Certificate** (App Service-Zertifikat importieren), und wählen Sie das soeben erworbene Zertifikat aus.

    ![Bild von Zertifikatimport einfügen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

6. Klicken Sie im Abschnitt **SSL-Bindungen** auf **Bindung hinzufügen**.
7. Wählen Sie auf dem Blatt **Add SSL Binding** (SSL-Bindung hinzufügen) mithilfe der Dropdownlisten den Domänennamen, der mit SSL geschützt werden soll, sowie das zu verwendende Zertifikat aus. Sie können auch auswählen, ob das SSL auf Basis der **[Servernamensanzeige](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Server Name Indication, SNI) oder ein IP-basiertes SSL verwendet werden soll.

    ![Bild von SSL-Bindungen einfügen](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

       • Bei IP-basiertem SSL wird ein Zertifikat mit einem Domänennamen verknüpft, indem die dedizierte öffentliche IP-Adresse des Servers dem Domänennamen zugeordnet wird. Voraussetzung dafür ist, dass jeder mit Ihrem Dienst verknüpfte Domänenname (contoso.com, fabricam.com usw.) eine dedizierte IP-Adresse hat. Dies ist die herkömmliche Methode, SSL-Zertifikate einem Webserver zuzuordnen. • SNI-basiertes SSL ist eine Erweiterung für SSL und **[Transport Layer Security](http://en.wikipedia.org/wiki/Transport_Layer_Security)** (TLS). Dabei können mehrere Domänen die gleiche IP-Adresse gemeinsam nutzen, während jede Domäne über eigene Sicherheitszertifikate verfügt. Die meisten modernen Browser (einschließlich Internet Explorer, Chrome, Firefox und Opera) unterstützen SNI, ältere Browser hingegen möglicherweise nicht. Weitere Informationen über SNI finden Sie im Wikipedia-Artikel **[Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Servernamensanzeige).
       
7. Klicken Sie auf **Bindung hinzufügen**, um die Änderungen zu speichern und SSL zu aktivieren.



Wenn Sie **IP-basiertes SSL** ausgewählt haben und Ihre benutzerdefinierte Domäne einen A-Datensatz verwendet, müssen Sie die folgenden zusätzlichen Schritte ausführen:

* Nach der Konfiguration einer IP-basierten SSL-Bindung wird Ihrer App eine dedizierte IP-Adresse zugewiesen. Diese IP-Adresse befindet sich auf der Seite **Benutzerdefinierte Domäne** unter den Einstellungen Ihrer App (direkt über dem Abschnitt **Hostnamen**). Sie ist als **Externe IP-Adresse** angegeben.
    
    ![Bild von IP-SSL einfügen](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

    Diese IP-Adresse unterscheidet sich von der virtuellen IP-Adresse, die zuvor zum Konfigurieren des A-Datensatzes für Ihre Domäne verwendet wurde. Wenn SNI-basiertes SSL konfiguriert bzw. wenn die Verwendung von SSL nicht konfiguriert ist, wird für diesen Eintrag keine Adresse aufgeführt.
    
2. Ändern Sie mit den von der Domänennamen-Registrierungsstelle bereitgestellten Tools den A-Datensatz für den benutzerdefinierten Domänennamen, sodass dieser auf die im vorherigen Schritt genannte IP-Adresse verweist. Sie sollten jetzt in der Lage sein, Ihre App über HTTPS:// anstelle von HTTP:// zu erreichen. Sie können damit überprüfen, ob das Zertifikat richtig konfiguriert wurde.


##<a name="bkmk_Rekey"></a>Erneute Schlüsselerstellung für das Zertifikat und Synchronisierung

1. Wenn Sie jemals erneut einen Schlüssel für Ihr Zertifikat erstellen müssen, wählen Sie aus Sicherheitsgründen einfach die Option **„Erneute Schlüsselerstellung und Synchronisierung“** im Blatt **„Zertifikateigenschaften“**.

2. Klicken Sie auf die Schaltfläche **„Erneute Schlüsselerstellung“**, um den Prozess zu initiieren. Dieser Prozess kann 1 bis 10 Minuten in Anspruch nehmen.

    ![Bild von erneuter Schlüsselerstellung für SSL einfügen](./media/app-service-web-purchase-ssl-web-site/Rekey.jpg)

3. Bei erneuter Schlüsselerstellung für Ihr Zertifikat wird ein neues Zertifikat von der Zertifizierungsstelle ausgestellt.
4. Für die erneute Schlüsselerstellung werden Ihnen für die Gültigkeitsdauer des Zertifikats keine Gebühren in Rechnung gestellt.
5. Die erneute Schlüsselerstellung für Ihr Zertifikat durchläuft den Status „Ausstehende Ausstellung“.
6. Sobald das Zertifikat bereit ist, stellen Sie sicher, dass Sie Ihre Ressourcen mithilfe dieses Zertifikats synchronisieren, um eine Unterbrechung des Diensts zu verhindern.
7. Die Synchronisierungsoption ist nicht für Zertifikate verfügbar, die noch nicht der Web-App zugewiesen sind.

## Weitere Ressourcen ##
- [Aktivieren von HTTPS für eine App in Azure App Service](web-sites-configure-ssl-certificate.md)
- [Kaufen und Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
- [Microsoft Azure Trust Center](/support/trust-center/security/)
- [Konfigurationsoptionen in Azure-Websites](http://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)
- [Azure-Verwaltungsportal](https://manage.windowsazure.com)

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

<!---HONumber=AcomDC_0824_2016-->