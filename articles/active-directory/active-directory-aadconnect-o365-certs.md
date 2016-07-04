<properties
	pageTitle="Anleitungen für die Zertifikaterneuerung für Office 365- und Azure AD-Benutzer | Microsoft Azure"
	description="In diesem Artikel wird für Office 365-Benutzer erläutert, wie Probleme mit E-Mails behoben werden, die sie zum Erneuern eines Zertifikats auffordern."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="billmath"/>


# Erneuern von Verbundzertifikaten für Office 365 und Azure AD

##Übersicht

Damit Azure AD und AD FS im Verbund genutzt werden können, müssen die Zertifikate, die AD FS zum Signieren der Sicherheitstoken für Azure AD verwendet, mit den in Azure AD konfigurierten Werten übereinstimmen. Abweichungen können zu einem Fehler bei der Vertrauensstellung zwischen AD FS und AAD führen. Azure AD stellt sicher, dass die Informationen bei der Bereitstellung von AD FS und einem Webanwendungsproxy (für Extranetzugriff) synchron gehalten werden.

Dieser Artikel enthält weitere Informationen zum Verwalten der Tokensignaturzertifikate und Synchronisieren der Zertifikate mit Azure AD in den folgenden Fällen:

* Sie stellen den Webanwendungsproxy nicht bereit, und daher sind die Verbundmetadaten nicht im Extranet verfügbar.
* Sie verwenden nicht die Standardkonfiguration von AD FS für Tokensignaturzertifikate.
* Sie verwenden einen externen Identitätsanbieter.

## Standardkonfiguration von AD FS für Tokensignaturzertifikate

Bei Tokensignatur- und Tokenentschlüsselungszertifikaten handelt es sich in der Regel um selbstsignierte Zertifikate, die ein Jahr lang gültig sind. Die Standardkonfiguration von AD FS in Bezug auf die Tokensignatur- und Tokenentschlüsselungszertifikate beinhaltet den Prozess **AutoCertificateRollover** zur automatischen Erneuerung. Wenn Sie AD FS 2.0 oder höher verwenden, aktualisieren Office 365 und Azure AD das Zertifikat automatisch, bevor es abläuft.

### Erneuerungsbenachrichtigung – O365-Portal- und E-Mail-Benachrichtigung

>[AZURE.NOTE] Wenn Sie eine E-Mail oder Portalbenachrichtigung mit der Aufforderung erhalten haben, Ihr Zertifikat für Office zu erneuern, können Sie mit den unter [Verwalten von Änderungen an Tokensignaturzertifikaten](#managecerts) aufgeführten Schritten überprüfen, ob Sie Maßnahmen ergreifen müssen. Microsoft ist sich eines möglichen Problems bewusst, das dazu führt, dass Benachrichtigungen zur Zertifikaterneuerung an Benutzer gesendet werden, auch wenn keine Maßnahmen erforderlich sind.

Azure AD versucht, die Verbundmetadaten zu überwachen und die Tokensignaturzertifikate wie von den Verbundmetadaten angegeben zu aktualisieren. 30 Tage vor dem Ablauf der Tokensignaturzertifikate überprüft Azure AD durch Abfragen der Verbundmetadaten, ob neue Zertifikate verfügbar sind.

* Wenn die Verbundmetadaten und die neuen Zertifikate abgerufen werden können, erhält der Benutzer keine E-Mail-Benachrichtigung oder O365-Portalwarnung.
* Können die neuen Tokensignaturzertifikate nicht abgerufen werden, weil die Verbundmetadaten nicht erreichbar sind oder das automatische Zertifikatrollover nicht aktiviert ist, wird eine E-Mail-Benachrichtigung ausgegeben und eine Warnung im O365-Portal angezeigt.

![O365-Portalbenachrichtigung](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Wenn Sie AD FS verwenden, überprüfen Sie zur Gewährleistung der Geschäftskontinuität, ob auf Ihren Servern die folgenden Patches installiert sind, um Authentifizierungsfehler durch bereits bekannte Probleme zu vermeiden. Dadurch werden bekannte AD FS-Proxyserverprobleme bei dieser Erneuerung und künftigen Erneuerungsperioden vermieden:
>
>Server 2012 R2 – [Windows Server May 2014 rollup](http://support.microsoft.com/kb/2955164) (Windows Server-Rollup von Mai 2014)
>
>Server 2008 R2 und 2012 – [Authentication through proxy fails in Windows Server 2012 or Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446) (Fehler bei der Authentifizierung über Proxy unter Windows Server 2008 oder Windows 2012 R2 R2)

## Überprüfen, ob die Zertifikate aktualisiert werden müssen <a name="managecerts"></a>

### Schritt 1: Überprüfen des AutoCertificateRollover-Status

Öffnen Sie auf dem AD FS-Server Microsoft PowerShell. Überprüfen Sie, ob der Wert für „AutoCertRollover“ auf TRUE festgelegt ist.

	Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Bei Verwendung von AD FS 2.0 müssen Sie zuerst „Add-Pssnapin Microsoft.Adfs.Powershell“ ausführen.

### Schritt 2: Überprüfen, ob AD FS und Azure AD synchronisiert sind

Öffnen Sie auf dem AD FS-Server die Azure AD PowerShell-Eingabeaufforderung, und stellen Sie eine Verbindung mit Azure AD her.

>[AZURE.NOTE] Falls Azure AD PowerShell noch nicht installiert ist, können Sie die Module [hier](https://technet.microsoft.com/library/jj151815.aspx) herunterladen.

	Connect-MsolService

Überprüfen Sie, ob die in AD FS und Azure AD konfigurierten Zertifikate den Eigenschaften für die angegebene Domäne vertrauen.

	Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Wenn die Fingerabdrücke in beiden Ausgaben übereinstimmen, wird dadurch bestätigt, dass Ihre Zertifikate mit Azure AD synchronisiert sind.

### Schritt 3: Überprüfen, ob das Zertifikat bald abläuft

Überprüfen Sie in der Ausgabe von „Get-MsolFederationProperty“ oder „Get-AdfsCertificate“ das Datum für „Not after“ (Nicht nach). Ist das Datum in weniger als 30 Tagen, müssen Sie Maßnahmen ergreifen.

### Nächster Schritt

| AutoCertificateRollover | Mit Azure AD synchronisierte Zertifikate | Verbundmetadaten sind öffentlich zugänglich | Gültigkeitsdauer | Aktion |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Ja | Ja | Ja | - | Keine Aktion erforderlich. Siehe [Automatisches Erneuern von Tokensignaturzertifikaten](#autorenew) | | Ja | Nein | - | Weniger als 15 Tage | Sofort erneuern. Siehe [Manuelles Erneuern von Tokensignaturzertifikaten](#manualrenew) | | Nein | - | - | Weniger als 30 Tage | Sofort erneuern. Siehe [Manuelles Erneuern von Tokensignaturzertifikaten](#manualrenew) |

[-] Unwichtig

## Automatisches Erneuern von Tokensignaturzertifikaten (empfohlen) <a name="autorenew"></a>

Wenn Sie den Webanwendungsproxy bereitgestellt haben, der den Zugriff auf die Verbundmetadaten aus dem Extranet ermöglichen kann, und die AD FS-Standardkonfiguration verwenden (d.h. „AutoCertificateRollover“ ist aktiviert), **müssen Sie keine Schritte manuell ausführen**. Überprüfen Sie Folgendes, um sicherzustellen, dass die Aktualisierung des Zertifikats automatisch erfolgen kann:

**1. Die AD FS-Eigenschaft „AutoCertificateRollover“ muss auf TRUE festgelegt sein.**

Damit wird angegeben, dass AD FS automatisch neue Tokensignatur- und Tokenentschlüsselungszertifikate generiert, bevor die alten ablaufen.

**2. Die AD FS-Verbundmetadaten sind öffentlich zugänglich.**

Überprüfen Sie, ob die Verbundmetadaten öffentlich zugänglich sind, indem Sie von einem Computer im öffentlichen Internet (außerhalb des Unternehmensnetzwerks) zur folgenden URL navigieren:


https://(your_FS_name)/federationmetadata/2007-06/federationmetadata.xml

Dabei wird `(your_FS_name) ` durch den Verbunddiensthostnamen ersetzt, den Ihre Organisation verwendet, z. B. "fs.contoso.com". Wenn Sie diese beiden Einstellungen erfolgreich überprüfen können, müssen Sie nichts weiter tun.

Beispiel: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Manuelles Erneuern von Tokensignaturzertifikaten <a name="manualrenew"></a>

Sie können die Tokensignaturzertifikate auch manuell signieren. Einige häufige Szenarien, in denen Sie die Tokensignaturzertifikate vielleicht manuell aktualisieren möchten:
* Bei den Tokensignaturzertifikaten handelt es sich nicht um selbstsignierte Zertifikate. Die häufigste Ursache hierfür ist, dass Ihre Organisation AD FS-Zertifikate verwaltet, die von einer Organisationszertifizierungsstelle registriert werden. 
* Die Netzwerksicherheit lässt nicht zu, dass die Verbundmetadaten öffentlich zugänglich sind.

In diesen Szenarien müssen Sie bei jeder Aktualisierung der Tokensignaturzertifikate auch die Office 365-Domäne mithilfe des PowerShell-Befehls „Update-MsolFederatedDomain“ aktualisieren.

### Schritte zum Erneuern des Tokensignaturzertifikats und Aktualisieren der O365-Verbundvertrauensstellung

**Schritt 1: Sicherstellen, dass AD FS über neue Tokensignaturzertifikate verfügt**

### Nicht standardmäßige Konfiguration
Wenn Sie eine nicht dem Standard entsprechende Konfiguration von AD FS nutzen, bei der **AutoCertificateRollover** auf **False** festgelegt ist, verwenden Sie wahrscheinlich benutzerdefinierte Zertifikate (nicht selbstsigniert). Lesen Sie die [Anleitung für Kunden, die keine selbstsignierten AD FS-Zertifikate verwenden](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert). Diese Anleitung enthält umfassende Anweisungen zum Erneuern der AD FS-Tokensignaturzertifikate.

### Die Verbundmetadaten sind nicht öffentlich verfügbar.
Wenn **AutoCertificateRollover** auf **True** festgelegt ist, aber Ihre Verbundmetadaten nicht öffentlich zugänglich sind, vergewissern Sie sich zunächst, ob von AD FS neue Tokensignaturzertifikate generiert wurden. Führen Sie die folgenden Schritte aus, um zu überprüfen, ob neue Tokensignaturzertifikate vorliegen.

1. Stellen Sie sicher, dass Sie am primären AD FS-Server angemeldet sind.
2. Überprüfen Sie die aktuellen Signaturzertifikate in AD FS, indem Sie ein PowerShell-Befehlsfenster öffnen und den folgenden Befehl ausführen:

	PS C:\>Get-ADFSCertificate –CertificateType token-signing

	>[AZURE.NOTE] Bei Verwendung von AD FS 2.0 müssen Sie zuerst „Add-Pssnapin Microsoft.Adfs.Powershell“ ausführen.

3. Sehen Sie sich in der Ausgabe des Befehls die aufgeführten Zertifikate an. Wenn AD FS ein neues Zertifikat generiert hat, sollten Sie zwei Zertifikate in der Ausgabe sehen: Bei einem ist der IsPrimary-Wert "True" und das NotAfter-Datum ist innerhalb von 5 Tagen, und bei einem ist der IsPrimary-Wert "False" und das NotAfter-Datum ist etwa ein Jahr in der Zukunft.

4. Wenn nur ein Zertifikat angezeigt wird und das NotAfter-Datum innerhalb von 5 Tagen ist, müssen Sie ein neues Zertifikat generieren, indem Sie die folgenden Schritte ausführen.

5. Um ein neues Zertifikat zu generieren, führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Überprüfen Sie das Update, indem Sie den folgenden Befehl erneut ausführen: PS C:\>Get-ADFSCertificate –CertificateType token-signing

Nun sollten zwei Zertifikate aufgeführt werden, bei denen eines ein NotAfter-Datum in etwa einem Jahr in der Zukunft hat und bei dem der IsPrimary-Wert "False" ist.

**Schritt 2: Aktualisieren der neuen Tokensignaturzertifikate für die Office 365-Vertrauensstellung**

Führen Sie die folgenden Schritte zum Aktualisieren von Office 365 mit den neuen Tokensignaturzertifikaten aus, die für die Vertrauensstellung verwendet werden sollen.

1.	Öffnen Sie das Microsoft Azure Active Directory-Modul für Windows PowerShell.
2.	Führen Sie $cred=Get-Credential aus. Wenn Sie dieses Cmdlet zur Eingabe von Anmeldeinformationen auffordert, geben Sie die Anmeldeinformationen Ihres Clouddienstadministrators ein.
3.	Führen Sie Connect-MsolService –Credential $cred aus. Dieses Cmdlet verbindet Sie mit dem Clouddienst. Sie müssen einen Kontext erstellen, der Sie mit dem Clouddienst verbindet, bevor Sie andere Cmdlets ausführen, die vom Tool installiert werden.
4.	Wenn Sie diese Befehle auf einem Computer ausführen, der nicht der primäre AD FS-Verbundserver ist, führen Sie "Set-MSOLAdfscontext -Computer <AD FS primary server>" aus, wobei <AD FS primary server> der interne FQDN des primären AD FS-Servers ist. Dieses Cmdlet erstellt einen Kontext, der Sie mit AD FS verbindet.
5.	Führen Sie "Update-MSOLFederatedDomain -DomainName <domain>" aus. Dieses Cmdlet aktualisiert die Einstellungen von AD FS im Clouddienst und konfiguriert die Vertrauensstellung zwischen beiden.


>[AZURE.NOTE] Wenn Sie mehrere Domänen der obersten Ebene unterstützen müssen, z. B. "contoso.com" und "fabrikam.com", müssen Sie den SupportMultipleDomain-Switch mit den Cmdlets verwenden. Weitere Informationen finden Sie unter [Unterstützung mehrerer Domänen der obersten Ebene](active-directory-aadconnect-multiple-domains.md).

## Reparieren der Azure AD-Vertrauensstellung mithilfe von AAD Connect <a name="connectrenew"></a>

Wenn Sie Ihre AD FS-Farm/Azure AD-Vertrauensstellung mit Azure AD Connect konfiguriert haben, können Sie mithilfe von Azure AD Connect ermitteln, ob Maßnahmen für Ihre Tokensignaturzertifikate ergriffen werden müssen. Wenn Sie die Zertifikate erneuern müssen, können Sie mithilfe Azure AD Connect mit nur wenigen Klicks die erforderlichen Schritte ausführen.

Weitere Informationen finden Sie unter [Reparieren der Vertrauensstellung](./active-directory-aadconnect-federation-management.md#repairing-the-trust).

<!---HONumber=AcomDC_0622_2016-->