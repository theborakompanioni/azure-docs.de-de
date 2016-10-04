<properties
	pageTitle="Sichern der benutzerdefinierten Domäne Ihrer App mit HTTPS | Microsoft Azure"
	description="Erfahren Sie, wie Sie den benutzerdefinierten Domänennamen für Ihre App in Azure App Service sichern, indem Sie eine SSL-Zertifikatsbindung konfigurieren. Außerdem erfahren Sie, wie Sie ein SSL-Zertifikat aus mehreren Tools abrufen."
	services="app-service"
	documentationCenter=".net"
	authors="cephalin"
	manager="wpickett"
	editor="jimbe"
	tags="top-support-issue"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/08/2016"
	ms.author="cephalin"/>

# Sichern Ihrer benutzerdefinierten App-Domäne mit HTTPS


> [AZURE.SELECTOR]
- [Kaufen eines SSL-Zertifikats in Azure](web-sites-purchase-ssl-web-site.md)
- [Verwenden eines SSL-Zertifikats von einer anderen Stelle](web-sites-configure-ssl-certificate.md)


In diesem Artikel erfahren Sie, wie Sie HTTPS für Web-Apps, Back-Ends für mobile Apps oder API-Apps, die einen benutzerdefinierten Domänennamen verwenden, in [Azure App Service](../app-service/app-service-value-prop-what-is.md) aktivieren. Es wird nur die Serverauthentifizierung behandelt. Wenn Sie gegenseitige Authentifizierung (einschließlich Clientauthentifizierung) benötigen, finden Sie entsprechende Informationen unter [Konfigurieren der gegenseitigen TLS-Authentifizierung für eine Web-App](app-service-web-configure-tls-mutual-auth.md).

Um mit HTTPS eine App zu sichern, die einen benutzerdefinierten Domänennamen hat, fügen Sie ein Zertifikat für diesen Domänennamen hinzu. Standardmäßig schützt Azure die **\*.azurewebsites.net**-Platzhalterdomäne mit einem einzigen SSL-Zertifikat, damit Ihre Clients bereits unter **https://*&lt;appname>*. azurewebsites.net** auf Ihre App zugreifen können. Aber wenn Sie eine benutzerdefinierte Domäne verwenden möchten, wie z.B. **contoso.com**, **www.contoso.com** und **\*.contoso.com**, kann das Standardzertifikat diese nicht sichern. Darüber hinaus ist das Standardzertifikat wie alle [Platzhalterzertifikate](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/) nicht so sicher wie die Verwendung einer benutzerdefinierten Domäne und eines Zertifikats für diese benutzerdefinierte Domäne.

>[AZURE.NOTE] Hilfe erhalten Sie jederzeit in den [Azure-Foren](https://azure.microsoft.com/support/forums/) von den Azure-Experten. Wenn Sie persönlicheren Support wünschen, rufen Sie den [Azure-Support](https://azure.microsoft.com/support/options/) auf, und klicken Sie auf **Support erhalten**.

<a name="bkmk_domainname"></a>
## Erforderliches Element
Um Ihren benutzerdefinierten Domänennamen mit HTTPS zu sichern, binden Sie ein benutzerdefiniertes SSL-Zertifikat an diese benutzerdefinierte Domäne in Azure. Vor dem Binden eines benutzerdefinierten Zertifikats müssen Sie die folgenden Schritte ausführen:

- **Konfigurieren Sie die benutzerdefinierte Domäne** – App Service ermöglicht das Hinzufügen eines Zertifikats nur für einen Domänennamen, der bereits in Ihrer App konfiguriert ist. Anweisungen finden Sie unter [Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App](web-sites-custom-domain-name.md).
- **Skalieren Sie zentral hoch bis zur Stufe „Basic“ oder höher** – App Service-Pläne in unteren Preisstufen unterstützen keine benutzerdefinierten SSL-Zertifikate. Anweisungen finden Sie unter [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md).
- **Rufen Sie ein SSL-Zertifikat ab** – wenn Sie nicht bereits über eines verfügen, müssen Sie eines von einer vertrauenswürdigen [Zertifizierungsstelle](http://en.wikipedia.org/wiki/Certificate_authority) (Certificate Authority, CA) abrufen. Das Zertifikat muss die folgenden Anforderungen erfüllen:

	- Es ist von einer vertrauenswürdigen Zertifizierungsstelle (keine privaten CA-Server) signiert.
	- Es enthält einen privaten Schlüssel.
	- Es ist für den Schlüsselaustausch erstellt und in eine PFX-Datei exportiert.
	- Es verwendet mindestens eine 2.048-Bit-Verschlüsselung.
	- Sein Antragstellername stimmt mit der benutzerdefinierten Domäne überein, die es sichern soll. Um mehrere Domänen mit einem Zertifikat zu sichern, müssen Sie einen Platzhalternamen verwenden (z.B. ***.contoso.com**), oder geben Sie subjectAltName-Werte an.
	- Es wird mit allen **[Zwischenzertifikaten](http://en.wikipedia.org/wiki/Intermediate_certificate_authorities)** zusammengeführt, die von Ihrer Zertifizierungsstelle verwendet werden. Andernfalls können bei einigen Clients nicht nachvollziehbare Interoperabilitätsprobleme auftreten.

		>[AZURE.NOTE] Die einfachste Möglichkeit, ein SSL-Zertifikat abzurufen, das alle Anforderungen erfüllt, ist der [direkte Kauf im Azure-Portal](web-sites-purchase-ssl-web-site.md). In diesem Artikel erfahren Sie, wie Sie dies manuell durchführen und das Zertifikat dann an Ihre benutzerdefinierte Domäne in App Service binden.
		>	
		> **ECC-Zertifikate (Elliptic Curve Cryptography, Kryptografie für elliptische Kurven)** können mit App Service funktionieren, werden aber nicht in diesem Artikel behandelt. Erarbeiten Sie mit Ihrer Zertifizierungsstelle die einzelnen Schritte zum Erstellen von ECC-Zertifikaten.

<a name="bkmk_getcert"></a>
## Schritt 1: Beziehen eines SSL-Zertifikats

Da Zertifizierungsstellen die verschiedenen SSL-Zertifikattypen zu unterschiedlichen Preisen anbieten, sollten Sie zunächst entscheiden, welchen SSL-Zertifikattyp Sie erwerben möchten. Um einen einzelnen Domänennamen zu sichern (**www.contoso.com**), benötigen Sie nur ein einfaches Zertifikat. Um mehrere Domänennamen zu sichern (**contoso.com** *und* **www.contoso.com** *und* **mail.contoso.com**), benötigen Sie entweder ein [Platzhalterzertifikat](http://en.wikipedia.org/wiki/Wildcard_certificate) oder ein Zertifikat mit einem [alternativen Antragstellernamen](http://en.wikipedia.org/wiki/SubjectAltName) (`subjectAltName`).

Sobald Sie wissen, welches SSL-Zertifikat Sie erwerben möchten, senden Sie eine Zertifikatsignieranforderung (Certificate Signing Request, CSR) an eine Zertifizierungsstelle. Wenn Sie das angeforderte Zertifikat von der Zertifizierungsstelle erhalten haben, generieren Sie eine PFX-Datei aus dem Zertifikat. Sie können diese Schritte mit dem Tool Ihrer Wahl ausführen. Hier finden Sie Anweisungen für die gängigen Tools:

- [Schritte für „Certreq.exe“](#bkmk_certreq): das Windows-Hilfsprogramm zum Erstellen von Zertifikatanforderungen. Es ist seit Windows XP/Windows Server 2000 ein Teil von Windows.
- [Schritte für IIS-Manager](#bkmk_iismgr): Das Tool Ihrer Wahl, wenn Sie bereits damit vertraut sind.
- [Schritte für OpenSSL](#bkmk_openssl): ein [plattformübergreifendes Open Source-Tool](https://www.openssl.org). Verwenden Sie es, um ein SSL-Zertifikat von einer beliebigen Plattform anzufordern.
- [Schritte für SubjectAltName mit OpenSSL](#bkmk_subjectaltname): Schritte zum Anfordern von `subjectAltName`-Zertifikaten.

Wenn Sie das Setup im App Service testen möchten, bevor Sie ein Zertifikat erwerben, können Sie ein [selbstsigniertes Zertifikat](https://en.wikipedia.org/wiki/Self-signed_certificate) generieren. Dieses Tutorial zeigt Ihnen zwei Methoden zum Generieren des Zertifikats:

- [Selbstsigniertes Zertifikat, Schritte für „Certreq.exe“](#bkmk_sscertreq)
- [Selbstsigniertes Zertifikat, Schritte für OpenSSL](#bkmk_ssopenssl)

<a name="bkmk_certreq"></a>
### Erhalten eines Zertifikats mit Certreq.exe

1. Erstellen Sie eine Datei (z.B. **myrequest.txt**), kopieren Sie den folgenden Text hinein, und speichern Sie sie in einem Arbeitsverzeichnis. Ersetzen Sie den Platzhalter `<your-domain>` durch den benutzerdefinierten Domänennamen Ihrer App.

		[NewRequest]
		Subject = "CN=<your-domain>"  ; E.g. "CN=www.contoso.com", or "CN=*.contoso.com" for a wildcard certificate
		Exportable = TRUE
		KeyLength = 2048              ; Required minimum is 2048
		KeySpec = 1
		KeyUsage = 0xA0
		MachineKeySet = True
		ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
		ProviderType = 12
		HashAlgorithm = SHA256

		[EnhancedKeyUsageExtension]
		OID=1.3.6.1.5.5.7.3.1         ; Server Authentication

	Weitere Informationen zu den Optionen der CSR sowie über weitere verfügbare Optionen finden Sie in der [Certreq-Referenzdokumentation](https://technet.microsoft.com/library/dn296456.aspx).

4. Wechseln Sie in einer Befehlszeile mit `CD` zu Ihrem Arbeitsverzeichnis, und führen Sie den folgenden Befehl aus, um die CSR zu erstellen:

		certreq -new myrequest.txt myrequest.csr

	**myrequest.csr** wird jetzt in Ihrem aktuellen Arbeitsverzeichnis erstellt.

5. Übermitteln Sie **myrequest.csr** an eine Zertifizierungsstelle, um ein SSL-Zertifikat abzurufen. Laden Sie die Datei entweder hoch, oder kopieren Sie den Inhalt aus einem Text-Editor in ein Webformular.

	Eine Liste der Zertifizierungsstellen, die von Microsoft als vertrauenswürdig eingestuft werden, finden Sie unter [Microsoft Trusted Root Certificate Program: Participants (v.2016-April)][cas] \(Microsoft-Programm für vertrauenswürdige Stammzertifikate: Teilnehmer [v.2016-April]).

6. Sobald die Zertifizierungsstelle Ihnen durch Senden eines Zertifikats (CER-Datei) geantwortet hat, speichern Sie es in Ihrem Arbeitsverzeichnis. Führen Sie dann den folgenden Befehl aus, um die ausstehende CSR abzuschließen.

		certreq -accept -user <certificate-name>.cer

	Dieser Befehl speichert das fertig gestellte Zertifikat im Windows-Zertifikatspeicher.

6. Wenn Ihre Zertifizierungsstelle Zwischenzertifikate verwendet, installieren Sie diese, bevor Sie fortfahren. Diese werden in der Regel als separater Download und in verschiedenen Formaten für unterschiedliche Webservertypen von der Zertifizierungsstelle zur Verfügung gestellt. Wählen Sie die Version für Microsoft IIS.

	Klicken Sie nach dem Herunterladen der Zertifikate im Windows Explorer auf jedes mit der rechten Maustaste, und wählen Sie **Zertifikat installieren**. Verwenden Sie die Standardwerte im **Zertifikat-Importassistenten**, und wählen Sie **Weiter** aus, bis der Import abgeschlossen ist.

7. Um das SSL-Zertifikat aus dem Zertifikatspeicher zu exportieren, drücken Sie `Win`+`R`, und führen Sie **certmgr.msc** aus, um den Zertifikat-Manager zu starten. Wählen Sie **Persönlich** > **Zertifikate**. In der Spalte **Ausgestellt für** sollte ein Eintrag mit Ihrem benutzerdefinierten Domänennamen angezeigt werden, und in der Spalte **Ausgestellt von** die Zertifizierungsstelle, die Sie zum Generieren des Zertifikats verwendet haben.

	![hier Bild von Zertifikat-Manager einfügen][certmgr]

9. Klicken Sie mit der rechten Maustaste auf das Zertifikat, und wählen Sie **Alle Aufgaben** > **Exportieren**. Klicken Sie im **Zertifikat-Exportassistenten** auf **Weiter**, wählen Sie dann **Ja, privaten Schlüssel exportieren**, und klicken Sie wieder auf **Weiter**.

	![Privaten Schlüssel exportieren][certwiz1]

10. Wählen Sie **Persönlicher Informationsaustausch - PKCS #12**, **Nach Möglichkeit alle Zertifikate in der Zertifikatkette berücksichtigen** und **Alle erweiterten Eigenschaften exportieren**. Klicken Sie auf **Weiter**.

	![Alle Zertifikate und erweiterten Eigenschaften berücksichtigen][certwiz2]

11. Wählen Sie **Kennwort** aus. Geben Sie das Kennwort ein, und bestätigen Sie es. Klicken Sie auf **Weiter**.

	![Kennwort angeben][certwiz3]

12. Geben Sie einen Pfad und Dateinamen für das exportierte Zertifikat mit der Dateierweiterung **.pfx** an. Klicken Sie zum Fertigstellen auf **Weiter** .

	![Dateipfad angeben][certwiz4]

Sie können nun die exportierte PFX-Datei in App Service hochladen. Siehe [Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats](#bkmk_configuressl).

<a name="bkmk_iismgr"></a>
### Erhalten eines Zertifikats mit dem IIS-Manager

1. Generieren Sie eine CSR zum Senden an die Zertifizierungsstelle mit IIS-Manager. Weitere Informationen zum Generieren einer CSR finden Sie unter [Anfordern eines Internetserverzertifikats (IIS 7)][iiscsr].

3. Senden Sie Ihre CSR an eine Zertifizierungsstelle, um ein SSL-Zertifikat zu beziehen. Eine Liste der Zertifizierungsstellen, die von Microsoft als vertrauenswürdig eingestuft werden, finden Sie unter [Microsoft Trusted Root Certificate Program: Participants (v.2016-April)][cas] \(Microsoft-Programm für vertrauenswürdige Stammzertifikate: Teilnehmer [v.2016-April]).


3. Schließen Sie die CSR mit dem Zertifikat ab, das die Zertifizierungsstelle an Sie zurücksendet. Weitere Informationen zum Ausfüllen der CSR finden Sie unter [Installieren eines Internetserverzertifikats (IIS 7)][installcertiis].

4. Wenn Ihre Zertifizierungsstelle Zwischenzertifikate verwendet, installieren Sie diese, bevor Sie fortfahren. Diese werden in der Regel als separater Download und in verschiedenen Formaten für unterschiedliche Webservertypen von der Zertifizierungsstelle zur Verfügung gestellt. Wählen Sie die Version für Microsoft IIS.

	Klicken Sie nach dem Herunterladen der Zertifikate im Windows Explorer auf jedes mit der rechten Maustaste, und wählen Sie **Zertifikat installieren**. Verwenden Sie die Standardwerte im **Zertifikat-Importassistenten**, und wählen Sie **Weiter** aus, bis der Import abgeschlossen ist.

4. Exportieren Sie das SSL-Zertifikat aus dem IIS-Manager. Weitere Informationen zum Exportieren des Zertifikats finden Sie unter [Exportieren eines Serverzertifikats (IIS 7)][exportcertiis].

	>[AZURE.IMPORTANT] Wählen Sie im **Zertifikat-Exportassistenten** die Option **Ja, privaten Schlüssel exportieren**,
	>
	>![Privaten Schlüssel exportieren][certwiz1]
	>
	> und wählen Sie auch **Persönlicher Informationsaustausch - PKCS #12**, **Nach Möglichkeit alle Zertifikate in der Zertifikatkette berücksichtigen** und **Alle erweiterten Eigenschaften exportieren**.
	>
	>![Alle Zertifikate und erweiterten Eigenschaften berücksichtigen][certwiz2]

Sie können nun die exportierte PFX-Datei in App Service hochladen. Siehe [Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats](#bkmk_configuressl).

<a name="bkmk_openssl"></a>
### Erhalten eines Zertifikats mit OpenSSL

1. Wechseln Sie in einer Befehlszeile mit `CD` zu einem Arbeitsverzeichnis, generieren Sie einen privaten Schlüssel und führen Sie die CSR mit folgendem Befehl aus:

		openssl req -sha256 -new -nodes -keyout myserver.key -out server.csr -newkey rsa:2048

2. Geben Sie bei Aufforderung die entsprechenden Informationen ein. Beispiel:

 		Country Name (2 letter code)
        State or Province Name (full name) []: Washington
        Locality Name (eg, city) []: Redmond
        Organization Name (eg, company) []: Microsoft
        Organizational Unit Name (eg, section) []: Azure
        Common Name (eg, YOUR name) []: www.microsoft.com
        Email Address []:

		Please enter the following 'extra' attributes to be sent with your certificate request

       	A challenge password []:

	Wenn Sie fertig sind, sollte Ihr Arbeitsverzeichnis zwei Dateien enthalten: **myserver.key** und **server.csr**. Die Datei **server.csr** enthält die CSR, und **myserver.key** benötigen Sie später.

3. Senden Sie Ihre CSR an eine Zertifizierungsstelle, um ein SSL-Zertifikat zu beziehen. Eine Liste der Zertifizierungsstellen, die von Microsoft als vertrauenswürdig eingestuft werden, finden Sie unter [Microsoft Trusted Root Certificate Program: Participants (v.2016-April)][cas] \(Microsoft-Programm für vertrauenswürdige Stammzertifikate: Teilnehmer [v.2016-April]).


4. Sobald die Zertifizierungsstelle das angeforderte Zertifikat sendet, speichern Sie es in einer Datei namens **myserver.crt** in Ihrem Arbeitsverzeichnis. Wenn die Zertifizierungsstelle das Zertifikat in einem Textformat bereitstellt, kopieren Sie einfach den Inhalt in einen Text-Editor, und speichern Sie ihn in einer Datei mit dem Namen **myserver.crt**. Ihre Datei sollte folgendermaßen aussehen:

		-----BEGIN CERTIFICATE-----
		MIIDJDCCAgwCCQCpCY4o1LBQuzANBgkqhkiG9w0BAQUFADBUMQswCQYDVQQGEwJV
		UzELMAkGA1UECBMCV0ExEDAOBgNVBAcTB1JlZG1vbmQxEDAOBgNVBAsTB0NvbnRv
		c28xFDASBgNVBAMTC2NvbnRvc28uY29tMB4XDTE0MDExNjE1MzIyM1oXDTE1MDEx
		NjE1MzIyM1owVDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAwDgYDVQQHEwdS
		ZWRtb25kMRAwDgYDVQQLEwdDb250b3NvMRQwEgYDVQQDEwtjb250b3NvLmNvbTCC
		ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN96hBX5EDgULtWkCRK7DMM3
		enae1LT9fXqGlbA7ScFvFivGvOLEqEPD//eLGsf15OYHFOQHK1hwgyfXa9sEDPMT
		3AsF3iWyF7FiEoR/qV6LdKjeQicJ2cXjGwf3G5vPoIaYifI5r0lhgOUqBxzaBDZ4
		xMgCh2yv7NavI17BHlWyQo90gS2X5glYGRhzY/fGp10BeUEgIs3Se0kQfBQOFUYb
		ktA6802lod5K0OxlQy4Oc8kfxTDf8AF2SPQ6BL7xxWrNl/Q2DuEEemjuMnLNxmeA
		Ik2+6Z6+WdvJoRxqHhleoL8ftOpWR20ToiZXCPo+fcmLod4ejsG5qjBlztVY4qsC
		AwEAATANBgkqhkiG9w0BAQUFAAOCAQEAVcM9AeeNFv2li69qBZLGDuK0NDHD3zhK
		Y0nDkqucgjE2QKUuvVSPodz8qwHnKoPwnSrTn8CRjW1gFq5qWEO50dGWgyLR8Wy1
		F69DYsEzodG+shv/G+vHJZg9QzutsJTB/Q8OoUCSnQS1PSPZP7RbvDV9b7Gx+gtg
		7kQ55j3A5vOrpI8N9CwdPuimtu6X8Ylw9ejWZsnyy0FMeOPpK3WTkDMxwwGxkU3Y
		lCRTzkv6vnHrlYQxyBLOSafCB1RWinN/slcWSLHADB6R+HeMiVKkFpooT+ghtii1
		A9PdUQIhK9bdaFicXPBYZ6AgNVuGtfwyuS5V6ucm7RE6+qf+QjXNFg==
		-----END CERTIFICATE-----

5. Führen Sie in einer Befehlszeile den folgenden Befehl aus, um **myserver.pfx** aus **myserver.key** und **myserver.crt** zu exportieren:

		openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt

	Definieren Sie bei entsprechender Aufforderung ein Kennwort, um die PFX-Datei zu sichern.

	> [AZURE.NOTE] Wenn die Zertifizierungsstelle Zwischenzertifikate verwendet, müssen Sie diese mit dem `-certfile`-Parameter einbeziehen. Diese werden in der Regel als separater Download und in verschiedenen Formaten für unterschiedliche Webservertypen von der Zertifizierungsstelle zur Verfügung gestellt. Wählen Sie die Version mit der `.pem`-Erweiterung.
	>
	> Ihr `openssl -export`-Befehl sollte wie im folgenden Beispiel aussehen, in dem eine PFX-Datei erstellt wird, die die Zwischenzertifikate aus der Datei **intermediate-cets.pem** einbezieht:
	>  
	> `openssl pkcs12 -chain -export -out myserver.pfx -inkey myserver.key -in myserver.crt -certfile intermediate-cets.pem`

Sie können nun die exportierte PFX-Datei in App Service hochladen. Siehe [Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats](#bkmk_configuressl).

<a name="bkmk_subjectaltname"></a>
### Erhalten eines SubjectAltName-Zertifikats mit OpenSSL

1. Erstellen Sie eine Datei namens **sancert.cnf**, kopieren Sie den folgenden Text hinein, und speichern Sie sie in einem Arbeitsverzeichnis:

		# -------------- BEGIN custom sancert.cnf -----
		HOME = .
		oid_section = new_oids
		[ new_oids ]
		[ req ]
		default_days = 730
		distinguished_name = req_distinguished_name
		encrypt_key = no
		string_mask = nombstr
		req_extensions = v3_req # Extensions to add to certificate request
		[ req_distinguished_name ]
		countryName = Country Name (2 letter code)
		countryName_default =
		stateOrProvinceName = State or Province Name (full name)
		stateOrProvinceName_default =
		localityName = Locality Name (eg, city)
		localityName_default =
		organizationalUnitName  = Organizational Unit Name (eg, section)
		organizationalUnitName_default  =
		commonName              = Your common name (eg, domain name)
		commonName_default      = www.mydomain.com
		commonName_max = 64
		[ v3_req ]
		subjectAltName=DNS:ftp.mydomain.com,DNS:blog.mydomain.com,DNS:*.mydomain.com
		# -------------- END custom sancert.cnf -----

	Ersetzen Sie in der Zeile, die mit `subjectAltName` beginnt, den Wert mit allen Domänennamen, die Sie (zusätzlich zu `commonName`) sichern möchten. Beispiel:

		subjectAltName=DNS:sales.contoso.com,DNS:support.contoso.com,DNS:fabrikam.com

	Sie müssen, einschließlich `commonName`, keine weiteren Felder ändern. Sie werden aufgefordert, sie in den nächsten Schritten anzugeben.

1. Wechseln Sie in einer Befehlszeile mit `CD` zu Ihrem Arbeitsverzeichnis, und führen Sie den folgenden Befehl aus:

		openssl req -sha256 -new -nodes -keyout myserver.key -out server.csr -newkey rsa:2048 -config sancert.cnf

2. Geben Sie bei Aufforderung die entsprechenden Informationen ein. Beispiel:

 		Country Name (2 letter code) []: US
        State or Province Name (full name) []: Washington
        Locality Name (eg, city) []: Redmond
        Organizational Unit Name (eg, section) []: Azure
        Your common name (eg, domain name) []: www.microsoft.com

	Wenn Sie fertig sind, sollte Ihr Arbeitsverzeichnis zwei Dateien enthalten: **myserver.key** und **server.csr**. Die Datei **server.csr** enthält die CSR, und **myserver.key** benötigen Sie später.

3. Senden Sie Ihre CSR an eine Zertifizierungsstelle, um ein SSL-Zertifikat zu beziehen. Eine Liste der Zertifizierungsstellen, die von Microsoft als vertrauenswürdig eingestuft werden, finden Sie unter [Microsoft Trusted Root Certificate Program: Participants (v.2016-April)][cas] \(Microsoft-Programm für vertrauenswürdige Stammzertifikate: Teilnehmer [v.2016-April]).


4. Sobald die Zertifizierungsstelle das angeforderte Zertifikat sendet, speichern Sie es in einer Datei namens **myserver.crt**. Wenn die Zertifizierungsstelle das Zertifikat in einem Textformat bereitstellt, kopieren Sie einfach den Inhalt in einen Text-Editor, und speichern Sie ihn in einer Datei mit dem Namen **myserver.crt**. Die Datei sollte folgendermaßen aussehen:

		-----BEGIN CERTIFICATE-----
		MIIDJDCCAgwCCQCpCY4o1LBQuzANBgkqhkiG9w0BAQUFADBUMQswCQYDVQQGEwJV
		UzELMAkGA1UECBMCV0ExEDAOBgNVBAcTB1JlZG1vbmQxEDAOBgNVBAsTB0NvbnRv
		c28xFDASBgNVBAMTC2NvbnRvc28uY29tMB4XDTE0MDExNjE1MzIyM1oXDTE1MDEx
		NjE1MzIyM1owVDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAwDgYDVQQHEwdS
		ZWRtb25kMRAwDgYDVQQLEwdDb250b3NvMRQwEgYDVQQDEwtjb250b3NvLmNvbTCC
		ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN96hBX5EDgULtWkCRK7DMM3
		enae1LT9fXqGlbA7ScFvFivGvOLEqEPD//eLGsf15OYHFOQHK1hwgyfXa9sEDPMT
		3AsF3iWyF7FiEoR/qV6LdKjeQicJ2cXjGwf3G5vPoIaYifI5r0lhgOUqBxzaBDZ4
		xMgCh2yv7NavI17BHlWyQo90gS2X5glYGRhzY/fGp10BeUEgIs3Se0kQfBQOFUYb
		ktA6802lod5K0OxlQy4Oc8kfxTDf8AF2SPQ6BL7xxWrNl/Q2DuEEemjuMnLNxmeA
		Ik2+6Z6+WdvJoRxqHhleoL8ftOpWR20ToiZXCPo+fcmLod4ejsG5qjBlztVY4qsC
		AwEAATANBgkqhkiG9w0BAQUFAAOCAQEAVcM9AeeNFv2li69qBZLGDuK0NDHD3zhK
		Y0nDkqucgjE2QKUuvVSPodz8qwHnKoPwnSrTn8CRjW1gFq5qWEO50dGWgyLR8Wy1
		F69DYsEzodG+shv/G+vHJZg9QzutsJTB/Q8OoUCSnQS1PSPZP7RbvDV9b7Gx+gtg
		7kQ55j3A5vOrpI8N9CwdPuimtu6X8Ylw9ejWZsnyy0FMeOPpK3WTkDMxwwGxkU3Y
		lCRTzkv6vnHrlYQxyBLOSafCB1RWinN/slcWSLHADB6R+HeMiVKkFpooT+ghtii1
		A9PdUQIhK9bdaFicXPBYZ6AgNVuGtfwyuS5V6ucm7RE6+qf+QjXNFg==
		-----END CERTIFICATE-----

5. Führen Sie in einer Befehlszeile den folgenden Befehl aus, um **myserver.pfx** aus **myserver.key** und **myserver.crt** zu exportieren:

		openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt

	Definieren Sie bei entsprechender Aufforderung ein Kennwort, um die PFX-Datei zu sichern.

	> [AZURE.NOTE] Wenn die Zertifizierungsstelle Zwischenzertifikate verwendet, müssen Sie diese mit dem `-certfile`-Parameter einbeziehen. Diese werden in der Regel als separater Download und in verschiedenen Formaten für unterschiedliche Webservertypen von der Zertifizierungsstelle zur Verfügung gestellt. Wählen Sie die Version mit der `.pem`-Erweiterung.
	>
	> Ihr `openssl -export`-Befehl sollte wie im folgenden Beispiel aussehen, in dem eine PFX-Datei erstellt wird, die die Zwischenzertifikate aus der Datei **intermediate-cets.pem** einbezieht:
	>  
	> `openssl pkcs12 -chain -export -out myserver.pfx -inkey myserver.key -in myserver.crt -certfile intermediate-cets.pem`

Sie können nun die exportierte PFX-Datei in App Service hochladen. Siehe [Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats](#bkmk_configuressl).

<a name="bkmk_sscertreq"></a>
### Generieren eines selbstsignierten Zertifikats mit „Certreq.exe“ ###

>[AZURE.IMPORTANT] Selbstsignierte Zertifikate sind nur für Testzwecke verwendbar. Die meisten Browser geben beim Besuch einer Website, die durch ein selbstsigniertes Zertifikat gesichert wird, Fehlermeldungen zurück. Einige Browser verweigern möglicherweise sogar den Aufruf der Website.

1. Erstellen Sie eine Textdatei (z.B. **mycert.txt**), kopieren Sie den folgenden Text hinein, und speichern Sie sie in einem Arbeitsverzeichnis. Ersetzen Sie den Platzhalter `<your-domain>` durch den benutzerdefinierten Domänennamen Ihrer App.

		[NewRequest]
		Subject = "CN=<your-domain>"  ; E.g. "CN=www.contoso.com", or "CN=*.contoso.com" for a wildcard certificate
		Exportable = TRUE
		KeyLength = 2048              ; KeyLength can be 2048, 4096, 8192, or 16384 (required minimum is 2048)
		KeySpec = 1
		KeyUsage = 0xA0
		MachineKeySet = True
		ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
		ProviderType = 12
		HashAlgorithm = SHA256
		RequestType = Cert            ; Self-signed certificate
		ValidityPeriod = Years
		ValidityPeriodUnits = 1

		[EnhancedKeyUsageExtension]
		OID=1.3.6.1.5.5.7.3.1         ; Server Authentication

	Der wichtige Parameter ist `RequestType = Cert`, denn er gibt an, dass es sich um ein selbstsigniertes Zertifikat handelt. Weitere Informationen zu den Optionen der CSR sowie über weitere verfügbare Optionen finden Sie in der [Certreq-Referenzdokumentation](https://technet.microsoft.com/library/dn296456.aspx).

4. Wechseln Sie in einer Befehlszeile mit `CD` zu Ihrem Arbeitsverzeichnis, und führen Sie den folgenden Befehl aus:

		certreq -new mycert.txt mycert.crt
	
	Ihr neues selbstsigniertes Zertifikat ist jetzt im Zertifikatspeicher installiert.

7. Um das Zertifikat aus dem Zertifikatspeicher zu exportieren, drücken Sie `Win`+`R`, und führen Sie **certmgr.msc** aus, um den Zertifikat-Manager zu starten. Wählen Sie **Persönlich** > **Zertifikate**. In der Spalte **Ausgestellt für** sollte ein Eintrag mit Ihrem benutzerdefinierten Domänennamen angezeigt werden, und in der Spalte **Ausgestellt von** die Zertifizierungsstelle, die Sie zum Generieren des Zertifikats verwendet haben.

	![hier Bild von Zertifikat-Manager einfügen][certmgr]

9. Klicken Sie mit der rechten Maustaste auf das Zertifikat, und wählen Sie **Alle Aufgaben** > **Exportieren**. Klicken Sie im **Zertifikat-Exportassistenten** auf **Weiter**, wählen Sie dann **Ja, privaten Schlüssel exportieren**, und klicken Sie wieder auf **Weiter**.

	![Privaten Schlüssel exportieren][certwiz1]

10. Wählen Sie **Persönlicher Informationsaustausch - PKCS #12**, **Nach Möglichkeit alle Zertifikate in der Zertifikatkette berücksichtigen** und **Alle erweiterten Eigenschaften exportieren**. Klicken Sie auf **Weiter**.

	![Alle Zertifikate und erweiterten Eigenschaften berücksichtigen][certwiz2]

11. Wählen Sie **Kennwort** aus. Geben Sie das Kennwort ein, und bestätigen Sie es. Klicken Sie auf **Weiter**.

	![Kennwort angeben][certwiz3]

12. Geben Sie einen Pfad und Dateinamen für das exportierte Zertifikat mit der Dateierweiterung **.pfx** an. Klicken Sie zum Fertigstellen auf **Weiter** .

	![Dateipfad angeben][certwiz4]

Sie können nun die exportierte PFX-Datei in App Service hochladen. Siehe [Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats](#bkmk_configuressl).

<a name="bkmk_ssopenssl"></a>
###Generieren eines selbstsignierten Zertifikats mit OpenSSL ###

>[AZURE.IMPORTANT] Selbstsignierte Zertifikate sind nur für Testzwecke verwendbar. Die meisten Browser geben beim Besuch einer Website, die durch ein selbstsigniertes Zertifikat gesichert wird, Fehlermeldungen zurück. Einige Browser verweigern möglicherweise sogar den Aufruf der Website.

1. Erstellen Sie eine Textdatei namens **serverauth.cnf**, kopieren Sie den folgenden Inhalt hinein, und speichern Sie sie in einem Arbeitsverzeichnis:

        [ req ]
        default_bits           = 2048
        default_keyfile        = privkey.pem
        distinguished_name     = req_distinguished_name
        attributes             = req_attributes
        x509_extensions        = v3_ca

        [ req_distinguished_name ]
        countryName			= Country Name (2 letter code)
        countryName_min			= 2
        countryName_max			= 2
        stateOrProvinceName		= State or Province Name (full name)
        localityName			= Locality Name (eg, city)
        0.organizationName		= Organization Name (eg, company)
        organizationalUnitName		= Organizational Unit Name (eg, section)
        commonName			= Common Name (eg, your app's domain name)
        commonName_max			= 64
        emailAddress			= Email Address
        emailAddress_max		= 40

        [ req_attributes ]
        challengePassword		= A challenge password
        challengePassword_min		= 4
        challengePassword_max		= 20

        [ v3_ca ]
         subjectKeyIdentifier=hash
         authorityKeyIdentifier=keyid:always,issuer:always
         basicConstraints = CA:false
         keyUsage=nonRepudiation, digitalSignature, keyEncipherment
         extendedKeyUsage = serverAuth

2. Wechseln Sie in einer Befehlszeile mit `CD` zu Ihrem Arbeitsverzeichnis, und führen Sie den folgenden Befehl aus:

		openssl req -sha256 -x509 -nodes -days 365 -newkey rsa:2048 -keyout myserver.key -out myserver.crt -config serverauth.cnf

	Dieser Befehl erstellt zwei Dateien: **myserver.crt** (das selbstsignierte Zertifikat) und **myserver.key** (der private Schlüssel), basierend auf den Einstellungen in **serverauth.cnf**.

3. Exportieren Sie das Zertifikat mit folgendem Befehl in eine PFX-Datei:

		openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt

	Definieren Sie bei entsprechender Aufforderung ein Kennwort, um die PFX-Datei zu sichern.

Sie können nun die exportierte PFX-Datei in App Service hochladen. Siehe [Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats](#bkmk_configuressl).

<a name="bkmk_configuressl"></a>
## Schritt 2: Hochladen und Binden des benutzerdefinierten SSL-Zertifikats

Bevor Sie fortfahren, lesen Sie den Abschnitt [Was Sie alles benötigen](#bkmk_domainname), und stellen Sie Folgendes sicher:

- Sie haben eine benutzerdefinierte Domäne, die Ihrer Azure-App zugeordnet ist;
- Ihre App wird im Tarif **Basic** oder höher ausgeführt, und
- Sie haben ein SSL-Zertifikat für die benutzerdefinierte Domäne, das von einer Zertifizierungsstelle ausgestellt wurde.


1. Öffnen Sie in Ihrem Browser das **[Azure-Portal](https://portal.azure.com/)**.
2.	Klicken Sie auf der Seite links auf die Option **App Service**.
3.	Klicken Sie auf den Namen Ihrer App, der Sie dieses Zertifikat zuweisen möchten.
4.	Klicken Sie unter **Einstellungen** auf **SSL-Zertifikate**.
5.	Klicken Sie auf **Zertifikat hochladen**.
6.	Wählen Sie die PFX-Datei, die Sie in [Schritt 1](#bkmk_getcert) exportiert haben, und geben Sie das Kennwort an, das Sie zuvor erstellt haben. Klicken Sie anschließend auf **Hochladen**, um das Zertifikat hochzuladen. Ihr hochgeladenes Zertifikat sollte nun auf dem Blatt **SSL-Zertifikat** angezeigt werden.
7. Klicken Sie im Abschnitt **SSL-Bindungen** auf **Bindung hinzufügen**.
8. Wählen Sie auf dem Blatt **Add SSL Binding** (SSL-Bindung hinzufügen) mithilfe der Dropdownlisten den Domänennamen, der mit SSL geschützt werden soll, sowie das zu verwendende Zertifikat aus. Sie können auch auswählen, ob das SSL auf Basis der **[Servernamensanzeige](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Server Name Indication, SNI) oder ein IP-basiertes SSL verwendet werden soll.

    ![Bild von SSL-Bindungen einfügen](./media/web-sites-configure-ssl-certificate/sslbindings.png)

       • Bei IP-basiertem SSL wird ein Zertifikat mit einem Domänennamen verknüpft, indem die dedizierte öffentliche IP-Adresse des Servers dem Domänennamen zugeordnet wird. Voraussetzung dafür ist, dass jeder mit Ihrem Dienst verknüpfte Domänenname (contoso.com, fabricam.com usw.) eine dedizierte IP-Adresse hat. Dies ist die herkömmliche Methode, SSL-Zertifikate einem Webserver zuzuordnen. • SNI-basiertes SSL ist eine Erweiterung für SSL und **[Transport Layer Security](http://en.wikipedia.org/wiki/Transport_Layer_Security)** (TLS). Dabei können mehrere Domänen die gleiche IP-Adresse gemeinsam nutzen, während jede Domäne über eigene Sicherheitszertifikate verfügt. Die meisten modernen Browser (einschließlich Internet Explorer, Chrome, Firefox und Opera) unterstützen SNI, ältere Browser hingegen möglicherweise nicht. Weitere Informationen über SNI finden Sie im Wikipedia-Artikel **[Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Servernamensanzeige).
     
9. Klicken Sie auf **Bindung hinzufügen**, um die Änderungen zu speichern und SSL zu aktivieren.

## Schritt 3: Ändern Sie Ihre Domänennamenzuordnung (nur IP-basiertes SSL).

Wenn Sie nur **SNI SSL**-Bindungen verwenden, überspringen Sie diesen Abschnitt. Mehrere **SNI SSL**-Bindungen können zusammen bei den vorhandenen freigegebenen IP-Adressen funktionieren, die Ihrer App zugewiesen sind. Wenn Sie allerdings eine Bindung gemäß **IP-basiertem SSL** erstellen, erstellt App Service eine dedizierte IP-Adresse für die Bindung, weil **IP-basiertes SSL** dies erfordert. Nur eine dedizierte IP-Adresse kann erstellt werden. Daher kann nur eine **IP-basierte SSL**-Bindung hinzugefügt werden.

Aufgrund dieser dedizierten IP-Adresse müssen Sie Ihre App unter folgenden Umständen weiter konfigurieren:

- Sie [haben einen A-Eintrag verwendet, um Ihre benutzerdefinierte Domäne Ihrer Azure-App zuzuordnen](web-sites-custom-domain-name.md#a), und Sie haben soeben eine Bindung gemäß **IP-basiertem SSL** hinzugefügt. In diesem Szenario müssen Sie den vorhandenen A-Eintrag wie folgt neu zuordnen, sodass er auf die dedizierte IP-Adresse zeigt:

	1. Nach der Konfiguration einer IP-basierten SSL-Bindung wird Ihrer App eine dedizierte IP-Adresse zugewiesen. Diese IP-Adresse befindet sich auf der Seite **Benutzerdefinierte Domäne** unter den Einstellungen Ihrer App (direkt über dem Abschnitt **Hostnamen**). Sie ist als **Externe IP-Adresse** angegeben.
    
	    ![Virtuelle IP-Adresse](./media/web-sites-configure-ssl-certificate/virtual-ip-address.png)

	2. [Ordnen Sie den A-Eintrag für Ihren benutzerdefinierten Domänennamen dieser neuen IP-Adresse neu zu](web-sites-custom-domain-name.md#a).

- Es sind bereits eine oder mehrere **SNI SSL**-Bindungen in Ihrer App vorhanden, und Sie haben gerade eine Bindung gemäß **IP-basiertem SSL** hinzugefügt. Sobald die Bindung abgeschlossen ist, zeigt Ihr *&lt;App-Name>*.azurewebsites.net-Domänenname auf die neue IP-Adresse. Darum empfängt jede vorhandene [CNAME-Zuordnung von der benutzerdefinierten Domäne](web-sites-custom-domain-name.md#cname) zu *&lt;App-Name>*.azurewebsites.net, einschließlich der **SNI SSL** gesicherten, auch Datenverkehr über die neue Adresse, die nur für **IP-basiertes SSL** erstellt ist. In diesem Szenario müssen Sie den **SNI SSL**-Datenverkehr wie folgt an die ursprünglich freigegebene IP-Adresse zurücksenden:

	1. Identifizieren Sie alle [CNAME-Zuordnungen von benutzerdefinierten Domänen](web-sites-custom-domain-name.md#cname) zu Ihrer App, die eine **SNI SSL**-Bindung hat.

	2. Ordnen Sie jeden CNAME-Eintrag **sni.**&lt;App-Name>.azurewebsites.net anstatt &lt;App-Name>.azurewebsites.net neu zu.

## Schritt 4: Testen von HTTPS für Ihre benutzerdefinierte Domäne

Sie müssen jetzt nur noch sicherstellen, dass HTTPS für Ihre benutzerdefinierte Domäne funktioniert. Rufen Sie in verschiedenen Browsern `https://<your.custom.domain>` auf, um zu sehen, ob Ihre App angeboten wird.

- Wenn Ihre App Zertifikatüberprüfungsfehler meldet, verwenden Sie wahrscheinlich ein selbstsigniertes Zertifikat.

- Wenn dies nicht der Fall ist, haben Sie beim Exportieren des PFX-Zertifikats vielleicht Zwischenzertifikate ausgelassen. Gehen Sie zurück zu [Was Sie alles benötigen](#bkmk_domainname), um zu überprüfen, ob Ihre CSR alle Voraussetzungen von App Service erfüllt.

<a name="bkmk_enforce"></a>
## Erzwingen von HTTPS in Ihrer App

Wenn Sie weiterhin HTTP-Zugriff auf Ihre App zulassen möchten, überspringen Sie diesen Schritt. App Service erzwingt HTTPS *nicht*, sodass Besucher weiterhin mit HTTP auf Ihre App zugreifen können. Wenn Sie HTTPS für Ihre App erzwingen möchten, können Sie in der `web.config`-Datei eine Neuschreibungsregel für Ihre App definieren. Jede App Service-App hat diese Datei, unabhängig vom Sprachenrahmen Ihrer App.

> [AZURE.NOTE] Dort erfolgt die sprachspezifische Weiterleitung von Anfragen. ASP.NET MVC kann den [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)-Filter anstelle der Neuschreibungsregel in `web.config` verwenden (siehe [Erstellen einer ASP.NET MVC 5-App mit Authentifizierung, SQL-Datenbank und Bereitstellung in Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)).

Folgen Sie diesen Schritten:

1. Navigieren Sie zu der Kudu-Debugkonsole für Ihre App. Die Adresse ist `https://<appname>.scm.azurewebsites.net/DebugConsole`.

2. Wechseln Sie in der Debugkonsole zu `D:\home\site\wwwroot`.

3. Öffnen Sie `web.config` durch Klicken auf die Schaltfläche „Stift“.

	![](./media/web-sites-configure-ssl-certificate/openwebconfig.png)

	Wenn Sie Ihre App mit Visual Studio oder Git bereitstellen, generiert App Service automatisch die entsprechende `web.config` für Ihre .NET-, PHP-, Node.js- oder Python-App im Stammverzeichnis der Anwendung. Wenn `web.config` nicht vorhanden ist, führen Sie `touch web.config` in der webbasierten Befehlszeile aus, um sie zu erstellen. Alternativ können Sie sie in Ihrem lokalen Projekt erstellen und Ihren Code erneut bereitstellen.

4. Wenn Sie eine `web.config` erstellen mussten, kopieren Sie den folgenden Code hinein, und speichern Sie sie. Wenn Sie eine vorhandene „web.config“ geöffnet haben, müssen Sie nur das gesamte `<rule>`-Tag in das `configuration/system.webServer/rewrite/rules`-Element Ihrer `web.config` kopieren.

		<?xml version="1.0" encoding="UTF-8"?>
		<configuration>
		  <system.webServer>
		    <rewrite>
		      <rules>
			    <!-- BEGIN rule TAG FOR HTTPS REDIRECT -->
		        <rule name="Force HTTPS" enabled="true">
		          <match url="(.*)" ignoreCase="false" />
		          <conditions>
		            <add input="{HTTPS}" pattern="off" />
		          </conditions>
		          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
		        </rule>
				<!-- END rule TAG FOR HTTPS REDIRECT -->
		      </rules>
		    </rewrite>
          </system.webServer>
		</configuration>

	Diese Regel gibt HTTP 301 (permanente Weiterleitung) an das HTTPS-Protokoll zurück, wenn der Benutzer eine Seite mit HTTP anfordert. Die Weiterleitung erfolgt von http://contoso.com zu https://contoso.com.

	>[AZURE.IMPORTANT] Wenn sich bereits andere `<rule>`-Tags in Ihrer `web.config` befinden, platzieren Sie das kopierte `<rule>`-Tag vor den anderen `<rule>`-Tags.

4. Speichern Sie die Datei in der Kudu-Debugkonsole. Mit sofortiger Wirkung sollten alle Anfragen an HTTPS weitergeleitet werden.

Weitere Informationen zum IIS-URL-Rewrite-Modul finden Sie unter der Dokumentation [URL-Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite).

## Weitere Ressourcen ##
- [Microsoft Azure Trust Center](/support/trust-center/security/)
- [Konfigurationsoptionen in Azure-Websites](/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)
- [Aktivieren der Diagnoseprotokollierung](web-sites-enable-diagnostic-log.md)
- [Konfigurieren von Web-Apps in Azure App Service](web-sites-configure.md)
- [Azure-Verwaltungsportal](https://manage.windowsazure.com)

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, bevor Sie sich für ein Azure-Konto registrieren, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine temporäre Starter-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

[customdomain]: web-sites-custom-domain-name.md
[iiscsr]: http://technet.microsoft.com/library/cc732906(WS.10).aspx
[cas]: http://social.technet.microsoft.com/wiki/contents/articles/31634.microsoft-trusted-root-certificate-program-participants-v-2016-april.aspx
[installcertiis]: http://technet.microsoft.com/library/cc771816(WS.10).aspx
[exportcertiis]: http://technet.microsoft.com/library/cc731386(WS.10).aspx
[openssl]: http://www.openssl.org/
[portal]: https://manage.windowsazure.com/
[tls]: http://en.wikipedia.org/wiki/Transport_Layer_Security
[staticip]: ./media/web-sites-configure-ssl-certificate/staticip.png
[website]: ./media/web-sites-configure-ssl-certificate/sslwebsite.png
[scale]: ./media/web-sites-configure-ssl-certificate/sslscale.png
[standard]: ./media/web-sites-configure-ssl-certificate/sslreserved.png
[pricing]: /pricing/details/
[configure]: ./media/web-sites-configure-ssl-certificate/sslconfig.png
[uploadcert]: ./media/web-sites-configure-ssl-certificate/ssluploadcert.png
[uploadcertdlg]: ./media/web-sites-configure-ssl-certificate/ssluploaddlg.png
[sslbindings]: ./media/web-sites-configure-ssl-certificate/sslbindings.png
[sni]: http://en.wikipedia.org/wiki/Server_Name_Indication
[certmgr]: ./media/web-sites-configure-ssl-certificate/waws-certmgr.png
[certwiz1]: ./media/web-sites-configure-ssl-certificate/waws-certwiz1.png
[certwiz2]: ./media/web-sites-configure-ssl-certificate/waws-certwiz2.png
[certwiz3]: ./media/web-sites-configure-ssl-certificate/waws-certwiz3.png
[certwiz4]: ./media/web-sites-configure-ssl-certificate/waws-certwiz4.png

<!---HONumber=AcomDC_0914_2016-->