<properties
	pageTitle="Schützen von Cloud- und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit Windows Server 2012 R2 AD FS | Microsoft Azure"
	description="In diesem Artikel werden die ersten Schritte mit Azure Multi-Factor Authentication und AD FS in Windows Server 2012 R2 beschrieben."
	services="multi-factor-authentication"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtland"/>

<tags
	ms.service="multi-factor-authentication"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/04/2016"
	ms.author="billmath"/>


# Schützen von Cloud- und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit AD FS in Windows Server 2012 R2

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
6. Bearbeiten Sie die Datei "MultiFactorAuthenticationAdfsAdapter.config" wie folgt:

|Schritt "MultiFactorAuthenticationAdfsAdapter.config"| Unterschritt|
|:------------- | :------------- |
|Legen Sie den Knoten **UseWebServiceSdk** auf **true** fest.||
|Legen Sie den Wert für **WebServiceSdkUrl** auf die URL des Multi-Factor Authentication-Webdienst-SDK fest.||
|Konfigurieren Sie das Webdienst-SDK.<br><br>*Option 1*: Verwenden eines Benutzernamens und Kennworts|<ol type="a"><li>Legen Sie den Wert für **WebServiceSdkUsername** auf ein Konto fest, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Verwenden Sie das Format &lt;Domäne&gt;&#92;&lt;Benutzername&gt;.<li>Legen Sie den Wert für **WebServiceSdkPassword** auf das entsprechende Kontokennwort fest.</li></ol>
|Konfigurieren des Webdienst-SDK, *Fortsetzung*<br><br>*Option 2*: Verwenden eines Clientzertifikats|<ol type="a"><li>Rufen Sie für den Server mit dem Webdienst-SDK ein Clientzertifikat von einer Zertifizierungsstelle ab. Informieren Sie sich über das [Abrufen von Clientzertifikaten](https://technet.microsoft.com/library/cc770328.aspx).</li><li>Importieren Sie das Clientzertifikat auf dem Server mit dem Webdienst-SDK in den persönlichen Zertifikatspeicher des lokalen Computers. Hinweis: Stellen Sie sicher, dass das öffentliche Zertifikat der Zertifizierungsstelle im Zertifikatspeicher für die vertrauenswürdigen Stammzertifikate enthalten ist.</li><li>Exportieren Sie die öffentlichen und privaten Schlüssel des Clientzertifikats in eine PFX-Datei.</li><li>Exportieren Sie den öffentlichen Schlüssel im Base64-Format in eine CER-Datei.</li><li>Überprüfen Sie im Server-Manager, ob das Feature „Webserver (IIS)\\Webserver\\Sicherheit\\Authentifizierung über IIS-Clientzertifikatzuordnung“ installiert ist. Falls nicht, wählen Sie **Rollen und Features hinzufügen**, um das Feature hinzuzufügen.</li><li>Doppelklicken Sie in IIS-Manager auf den **Konfigurations-Editor** der Website, die das virtuelle Verzeichnis des Webdienst-SDK enthält. Hinweis: Es ist sehr wichtig, dies auf Websiteebene und nicht auf Ebene des virtuellen Verzeichnisses vorzunehmen.</li><li>Navigieren Sie zum Abschnitt **system.webServer/security/authentication/iisClientCertificateMappingAuthentication**.</li><li>Legen Sie **enabled** auf **true** fest.</li><li>Legen Sie **oneToOneCertificateMappingsEnabled** auf **true** fest.</li><li>Klicken Sie auf die Schaltfläche **...** neben **oneToOneMappings** und dann auf den Link **Hinzufügen**.</li><li>Öffnen Sie die zuvor exportierte Base64-CER-Datei. Entfernen Sie *-----BEGIN CERTIFICATE-----*, *-----END CERTIFICATE-----* sowie alle Zeilenumbrüche. Kopieren Sie die resultierende Zeichenfolge.</li><li>Legen Sie das **Zertifikat** auf die im vorherigen Schritt kopierte Zeichenfolge fest.</li><li>Legen Sie **enabled** auf **true** fest.</li><li>Legen Sie **userName** auf ein Konto fest, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Verwenden Sie das Format &lt;Domäne&gt;&#92;&lt;Benutzername&gt;.</li><li>Legen Sie das Kennwort auf das entsprechende Kontokennwort fest, und schließen Sie den Konfigurations-Editor.</li><li>Klicken Sie auf den Link **Übernehmen**.</li><li>Doppelklicken Sie im virtuellen Verzeichnis des Webdienst-SDK auf **Authentifizierung**.</li><li>Stellen Sie sicher, dass **ASP.NET-Identitätswechsel** und **Standardauthentifizierung** auf **Enabled** und alle anderen Elemente auf **Disabled** festgelegt sind.</li><li>Doppelklicken Sie im virtuellen Verzeichnis des Webdienst-SDK auf **SSL-Einstellungen**.</li><li>Legen Sie **Clientzertifikate** auf **Akzeptieren** fest, und klicken Sie auf **Übernehmen**.</li><li>Kopieren Sie die zuvor exportierte PFX-Datei auf den Server, auf dem der AD FS-Adapter ausgeführt wird.</li><li>Importieren Sie die PFX-Datei in den persönlichen Zertifikatspeicher des lokalen Computers.</li><li>Klicken Sie mit der rechten Maustaste, und wählen Sie **Private Schlüssel verwalten**. Gewähren Sie den Lesezugriff für das Konto, das Sie zum Anmelden am AD FS-Dienst verwendet haben.</li><li>Öffnen Sie das Clientzertifikat, und kopieren Sie den Fingerabdruck von der Registerkarte **Details**.</li><li>Legen Sie in der Datei „MultiFactorAuthenticationAdfsAdapter.config“ **WebServiceSdkCertificateThumbprint** auf die im vorherigen Schritt kopierte Zeichenfolge fest.</li></ol>
| Bearbeiten Sie das Skript „Register-MultiFactorAuthenticationAdfsAdapter.ps1“, indem Sie *-ConfigurationFilePath &lt;Pfad&gt;* am Ende des Befehls `Register-AdfsAuthenticationProvider` hinzufügen, wobei *&lt;Pfad&gt;* der vollständige Pfad zur Datei „MultiFactorAuthenticationAdfsAdapter.config“ ist.||

Führen Sie das Skript „\\Programme\\Multi-Factor Authentication Server\\Register-MultiFactorAuthenticationAdfsAdapter.ps1“ in PowerShell aus, um den Adapter zu registrieren. Der Adapter wird als "WindowsAzureMultiFactorAuthentication" registriert. Sie müssen den AD FS-Dienst neu starten, damit die Registrierung wirksam wird.

<!---HONumber=AcomDC_0810_2016-->