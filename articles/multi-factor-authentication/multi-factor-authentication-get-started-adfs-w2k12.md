<properties
	pageTitle="MFA-Server mit AD FS unter Windows Server 2012 R2 | Microsoft Azure"
	description="In diesem Artikel werden die ersten Schritte mit Azure Multi-Factor Authentication und AD FS in Windows Server 2012 R2 beschrieben."
	services="multi-factor-authentication"
	documentationCenter=""
	authors="kgremban"
	manager="femila"
	editor="curtland"/>

<tags
	ms.service="multi-factor-authentication"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="09/22/2016"
	ms.author="kgremban"/>


# Schützen von Cloudressourcen und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit AD FS unter Windows Server 2012 R2

Wenn in Ihrer Organisation Active Directory-Verbunddienste (AD FS) verwendet wird und Sie Cloudressourcen oder lokale Ressourcen schützen möchten, können Sie Azure Multi-Factor Authentication-Server bereitstellen und für AD FS konfigurieren. Diese Konfiguration löst die Multi-Factor Authentication für Endpunkte mit hohem Wert aus.

In diesem Artikel wird die Verwendung von Azure Multi-Factor Authentication-Server mit AD FS unter Windows Server 2012 R2 beschrieben. Weitere Informationen finden Sie unter [Sichern von Cloud- und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## Schützen von Windows Server 2012 R2 AD FS mit Azure Multi-Factor Authentication-Server

Bei der Installation von Azure Multi-Factor Authentication-Server haben Sie die folgenden Möglichkeiten:

- Lokale Installation von Azure Multi-Factor Authentication-Server auf demselben Server wie AD FS
- Lokale Installation von Azure Multi-Factor Authentication-Adapter auf dem AD FS-Server und anschließende Installation von Multi-Factor Authentication-Server auf einem anderen Computer

Bevor Sie beginnen, sollten Sie Folgendes beachten:

- Es ist nicht zwingend erforderlich, Azure Multi-Factor Authentication-Server auf Ihrem AD FS-Server zu installieren. Aber Sie müssen den Multi-Factor Authentication-Adapter für AD FS auf einem Windows Server 2012 R2-Computer installieren, auf dem AD FS ausgeführt wird. Sie können den Server auf einem anderen Computer installieren, sofern es sich um eine unterstützte Version handelt und Sie den AD FS-Adapter separat auf dem AD FS-Verbundserver installieren. In den folgenden Verfahren wird die separate Installation des Adapters beschrieben.
- Bei der Entwicklung des AD FS-Adapters von Multi-Factor Authentication-Server wurde erwartet, dass AD FS den Namen der vertrauenden Seite an den Adapter übergibt und als Anwendungsname verwendet. Dies war jedoch nicht der Fall. Wenn Ihre Organisation als Authentifizierungsmethode SMS oder mobile Apps verwenden, enthalten die in den Unternehmenseinstellungen festgelegten Zeichenfolgen einen Platzhalter: <$*application\_name*$>. Dieser Platzhalter wird nicht automatisch ersetzt, wenn Sie den AD FS-Adapter verwenden. Es wird empfohlen, beim Schützen von AD FS den Platzhalter aus den entsprechenden Zeichenfolgen zu entfernen.

- Das Konto, das Sie zum Anmelden verwenden, muss über Benutzerrechte zum Erstellen von Sicherheitsgruppen im Active Directory-Dienst verfügen.

- Der Installations-Assistent für den Multi-Factor Authentication AD FS-Adapter erstellt die Sicherheitsgruppe „PhoneFactor Admins“ in Ihrer Instanz von Active Directory und fügt dieser Gruppe dann das AD FS-Dienstkonto Ihres Verbunddiensts hinzu. Es wird empfohlen, auf Ihrem Domänencontroller sicherzustellen, dass die Gruppe „PhoneFactor Admins“ auch wirklich erstellt wird und dass das AD FS-Dienstkonto ein Mitglied dieser Gruppe ist. Bei Bedarf fügen Sie das AD FS-Dienstkonto der Gruppe „PhoneFactor Admins“ auf dem Domänencontroller manuell hinzu.
- Informationen zum Installieren des Webdienst-SDK mit dem Benutzerportal finden Sie unter [Bereitstellen des Benutzerportals für den Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-portal.md).


### Lokale Installation von Azure Multi-Factor Authentication-Server auf dem AD FS-Server

1. Laden Sie Azure Multi-Factor Authentication-Server herunter, und installieren Sie diese Komponente auf dem AD FS-Verbundserver. Informationen zur Installation finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-server.md).
2. Klicken Sie in der Verwaltungskonsole von Azure Multi-Factor Authentication-Server auf das Symbol **AD FS**, und wählen Sie dann die Optionen **Benutzerregistrierung zulassen** und **Methodenauswahl durch Benutzer zulassen**.
3. Wählen Sie alle weiteren Optionen aus, die Sie für Ihre Organisation angeben möchten.
4. Klicken Sie auf **AD FS-Adapter installieren**.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Wenn der Computer zu einer Domäne gehört und die Active Directory-Konfiguration zum Sichern der Kommunikation zwischen dem AD FS-Adapter und dem Multi-Factor Authentication-Dienst unvollständig ist, wird das Fenster **Active Directory** angezeigt. Klicken Sie auf **Weiter**, um diese Konfiguration automatisch abzuschließen, oder aktivieren Sie das Kontrollkästchen **Automatische Active Directory-Konfiguration überspringen und Einstellungen manuell konfigurieren**, und klicken Sie dann auf **Weiter**.
6. Wenn der Computer zu einer Domäne gehört und die Active Directory-Konfiguration zum Sichern der Kommunikation zwischen dem AD FS-Adapter und dem Multi-Factor Authentication-Dienst unvollständig ist, wird das Fenster **Lokale Gruppe** angezeigt. Klicken Sie auf die Schaltfläche **Weiter**, um diese Konfiguration automatisch abzuschließen, oder aktivieren Sie das Kontrollkästchen **Automatische Konfiguration der lokalen Gruppe überspringen und Einstellungen manuell konfigurieren**, und klicken Sie dann auf **Weiter**.
7. Klicken Sie im Installations-Assistenten auf **Weiter**. Azure Multi-Factor Authentication-Server erstellt die Gruppe „PhoneFactor Admins“ und fügt das AD FS-Dienstkonto der Gruppe „PhoneFactor Admins“ hinzu.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Klicken Sie auf der Seite **Installer starten** auf **Weiter**.
9. Klicken Sie im Installer für den Multi-Factor Authentication AD FS-Adapter auf **Weiter**.
10. Klicken Sie auf **Schließen**, wenn die Installation abgeschlossen ist.
11. Nach der Installation des Adapters müssen Sie ihn bei AD FS registrieren. Öffnen Sie Windows PowerShell, und führen Sie den folgenden Befehl aus:<br> `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Bearbeiten Sie die globale Authentifizierungsrichtlinie in AD FS, damit der neu registrierte Adapter verwendet werden kann. Wechseln Sie in der AD FS-Verwaltungskonsole zum Knoten **Authentifizierungsrichtlinien**. Klicken Sie im Abschnitt **Multi-Factor Authentication** auf den Link **Bearbeiten** neben dem Abschnitt **Globale Einstellungen**. Wählen Sie im Fenster **Globale Authentifizierungsrichtlinie bearbeiten** als zusätzliche Authentifizierungsmethode **Multi-Factor Authentication** aus, und klicken Sie dann auf **OK**. Der Adapter wird als "WindowsAzureMultiFactorAuthentication" registriert. Sie müssen den AD FS-Dienst neu starten, damit die Registrierung wirksam wird.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

An diesem Punkt ist Multi-Factor Authentication-Server als zusätzlicher Authentifizierungsanbieter für die Verwendung mit AD FS eingerichtet.

## Installieren einer eigenständigen Instanz des AD FS-Adapters mit dem Webdienst-SDK
1. Installieren Sie das Webdienst-SDK auf dem Server, auf dem Multi-Factor Authentication-Server ausgeführt wird.
2. Kopieren Sie die folgenden Dateien aus dem Verzeichnis „\\Programme\\Multi-Factor Authentication Server“ auf den Server, auf dem Sie den AD FS-Adapter installieren möchten:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Führen Sie die Installationsdatei „MultiFactorAuthenticationAdfsAdapterSetup64.msi“ aus.
4. Klicken Sie im Multi-Factor Authentication AD FS-Adapter-Installer auf **Weiter**, um die Installation durchzuführen.
5. Klicken Sie auf **Schließen**, wenn die Installation abgeschlossen ist.

## Bearbeiten der Datei „MultiFactorAuthenticationAdfsAdapter.config“

Führen Sie die folgenden Schritte aus, um die Datei „MultiFactorAuthenticationAdfsAdapter.config“ zu bearbeiten:

1. Legen Sie den Knoten **UseWebServiceSdk** auf **true** fest.
2. Legen Sie den Wert für **WebServiceSdkUrl** auf die URL des Multi-Factor Authentication-Webdienst-SDKs fest. Beispiel: **https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx** („certificatename“ steht hierbei für den Namen Ihres Zertifikats.)
3. Bearbeiten Sie das Skript „Register-MultiFactorAuthenticationAdfsAdapter.ps1“, indem Sie *-ConfigurationFilePath &lt;Pfad&gt;* am Ende des Befehls `Register-AdfsAuthenticationProvider` hinzufügen, wobei *&lt;Pfad&gt;* der vollständige Pfad zur Datei „MultiFactorAuthenticationAdfsAdapter.config“ ist.

### Konfigurieren des Webdienst-SDKs mit einem Benutzernamen und einem Kennwort

Das Webdienst-SDK kann auf zwei Arten konfiguriert werden: mit einem Benutzernamen und einem Kennwort oder mit einem Clientzertifikat. Im Anschluss finden Sie die Schritte für die erste Option. Informationen zur zweiten Option finden Sie weiter unten.

1. Legen Sie den Wert für **WebServiceSdkUsername** auf ein Konto fest, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Verwenden Sie das Format „&lt;Domäne&gt;&#92;&lt;Benutzername&gt;“.
2. Legen Sie den Wert für **WebServiceSdkPassword** auf das entsprechende Kontokennwort fest.

### Konfigurieren des Webdienst-SDKs mit einem Clientzertifikat

Falls Sie keine Kombination aus Benutzername und Kennwort verwenden möchten, gehen Sie wie folgt vor, um das Webdienst-SDK mit einem Clientzertifikat zu konfigurieren.

1. Beziehen Sie für den Server mit dem Webdienst-SDK ein Clientzertifikat von einer Zertifizierungsstelle. Informationen dazu finden Sie [hier](https://technet.microsoft.com/library/cc770328.aspx).
2. Importieren Sie das Clientzertifikat auf dem Server mit dem Webdienst-SDK in den persönlichen Zertifikatspeicher des lokalen Computers. Hinweis: Vergewissern Sie sich, dass das öffentliche Zertifikat der Zertifizierungsstelle im Zertifikatspeicher für die vertrauenswürdigen Stammzertifikate enthalten ist.
3. Exportieren Sie den öffentlichen und privaten Schlüssel des Clientzertifikats in eine PFX-Datei.
4. Exportieren Sie den öffentlichen Schlüssel im Base64-Format in eine CER-Datei.
5. Vergewissern Sie sich im Server-Manager, dass das Feature „Webserver (IIS)\\Webserver\\Sicherheit\\Authentifizierung über IIS-Clientzertifikatzuordnung“ installiert ist. Falls nicht, wählen Sie **Rollen und Features hinzufügen** aus, um das Feature hinzuzufügen.
6. Doppelklicken Sie in IIS-Manager für die Website, die das virtuelle Verzeichnis des Webdienst-SDKs enthält, auf **Konfigurations-Editor**. Hinweis: Dieser Schritt muss unbedingt auf der Websiteebene (nicht auf Ebene des virtuellen Verzeichnisses) ausgeführt werden.
7. Navigieren Sie zum Abschnitt **system.webServer/security/authentication/iisClientCertificateMappingAuthentication**.
8. Legen Sie **enabled** auf **true** fest.
9. Legen Sie **oneToOneCertificateMappingsEnabled** auf **true** fest.
10. Klicken Sie neben **oneToOneMappings** auf die Schaltfläche **...** und anschließend auf den Link **Hinzufügen**.
11. Öffnen Sie die zuvor exportierte Datei „Base64.cer“. Entfernen Sie *-----BEGIN CERTIFICATE-----*, *-----END CERTIFICATE-----* sowie alle Zeilenumbrüche. Kopieren Sie die resultierende Zeichenfolge.
12. Legen Sie **certificate** auf die im vorherigen Schritt kopierte Zeichenfolge fest.
13. Legen Sie **enabled** auf **true** fest.
14. Legen Sie **userName** auf ein Konto fest, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Verwenden Sie das Format „&lt;Domäne&gt;&#92;&lt;Benutzername&gt;“.
15. Legen Sie das Kennwort auf das entsprechende Kontokennwort fest, und schließen Sie den Konfigurations-Editor.
16. Klicken Sie auf den Link **Übernehmen**.
17. Doppelklicken Sie im virtuellen Verzeichnis des Webdienst-SDKs auf **Authentifizierung**.
18. Vergewissern Sie sich, dass **ASP.NET-Identitätswechsel** und **Standardauthentifizierung** aktiviert und alle anderen Elemente deaktiviert sind.
19. Doppelklicken Sie im virtuellen Verzeichnis des Webdienst-SDKs auf **SSL-Einstellungen**.
20. Legen Sie **Clientzertifikate** auf **Akzeptieren** fest, und klicken Sie auf **Übernehmen**.
21. Kopieren Sie die zuvor exportierte PFX-Datei auf den Server, auf dem der AD FS-Adapter ausgeführt wird.
22. Importieren Sie die PFX-Datei in den persönlichen Zertifikatspeicher des lokalen Computers.
23. Klicken Sie mit der rechten Maustaste, wählen Sie **Private Schlüssel verwalten** aus, und gewähren Sie dem Konto, das Sie für die Anmeldung beim Active Directory-Verbunddienst verwendet haben, Lesezugriff.
24. Öffnen Sie das Clientzertifikat, und kopieren Sie den Fingerabdruck aus der Registerkarte **Details**.
25. Legen Sie in der Datei „MultiFactorAuthenticationAdfsAdapter.config“ das Element **WebServiceSdkCertificateThumbprint** auf die im vorherigen Schritt kopierte Zeichenfolge fest.


Führen Sie schließlich das Skript „\\Programme\\Multi-Factor Authentication Server\\Register-MultiFactorAuthenticationAdfsAdapter.ps1“ in PowerShell aus, um den Adapter zu registrieren. Der Adapter wird als "WindowsAzureMultiFactorAuthentication" registriert. Sie müssen den AD FS-Dienst neu starten, damit die Registrierung wirksam wird.

## Verwandte Themen

Hilfe bei der Problembehandlung finden Sie unter [Azure Multi-Factor Authentication – Häufig gestellte Fragen](multi-factor-authentication-faq.md).

<!---HONumber=AcomDC_0928_2016-->