<properties
	pageTitle="Anleitungen für die Zertifikaterneuerung für Office 365- und Azure Active Directory-Benutzer | Microsoft Azure"
	description="In diesem Artikel wird für Office 365-Benutzer erläutert, wie Probleme mit E-Mails behoben werden, die sie zum Erneuern eines Zertifikats auffordern."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="femila"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/08/2016"
	ms.author="billmath"/>


# Erneuern von Verbundzertifikaten für Office 365 und Azure Active Directory

##Übersicht

Damit ein erfolgreicher Verbund zwischen Azure Active Directory (Azure AD) und Active Directory-Verbunddienste (AD FS) eingerichtet werden kann, müssen die Zertifikate, die AD FS zum Signieren der Sicherheitstoken für Azure AD verwendet, mit den in Azure AD konfigurierten Werten übereinstimmen. Jede fehlende Übereinstimmung kann zu einem Bruch der Vertrauensstellung führen. Azure AD stellt sicher, dass die Informationen bei der Bereitstellung von AD FS und einem Webanwendungsproxy (für Extranetzugriff) synchron gehalten werden.

Dieser Artikel enthält weitere Informationen zum Verwalten der Tokensignaturzertifikate und Synchronisieren der Zertifikate mit Azure AD in den folgenden Fällen:

* Sie stellen den Webanwendungsproxy nicht bereit, und daher sind die Verbundmetadaten nicht im Extranet verfügbar.
* Sie verwenden nicht die Standardkonfiguration von AD FS für Tokensignaturzertifikate.
* Sie verwenden einen externen Identitätsanbieter.

## Standardkonfiguration von AD FS für Tokensignaturzertifikate

Bei Tokensignatur- und Tokenentschlüsselungszertifikaten handelt es sich in der Regel um selbstsignierte Zertifikate, die ein Jahr lang gültig sind. AD FS enthält standardmäßig einen automatischen Erneuerungsprozess mit der Bezeichnung **AutoCertificateRollover**. Wenn Sie AD FS 2.0 oder höher verwenden, aktualisieren Office 365 und Azure AD das Zertifikat automatisch, bevor es abläuft.

### Erneuerungsbenachrichtigung über das Office 365-Portal oder per E-Mail

>[AZURE.NOTE] Wenn Sie eine E-Mail oder Portalbenachrichtigung mit der Aufforderung erhalten haben, Ihr Zertifikat für Office zu erneuern, können Sie unter [Verwalten von Änderungen an Tokensignaturzertifikaten](#managecerts) überprüfen, ob Sie Maßnahmen ergreifen müssen. Microsoft ist sich des möglichen Problems bewusst, dass Benachrichtigungen zur Zertifikaterneuerung auch dann gesendet werden, wenn keine Maßnahmen erforderlich sind.

Azure AD versucht, die Verbundmetadaten zu überwachen und die Tokensignaturzertifikate wie von den Metadaten angegeben zu aktualisieren. 30 Tage vor Ablauf der Tokensignaturzertifikate überprüft Azure AD durch Abfragen der Verbundmetadaten, ob neue Zertifikate verfügbar sind.

* Wenn die Verbundmetadaten und die neuen Zertifikate abgerufen werden können, erhält der Benutzer keine E-Mail-Benachrichtigung oder Warnung im Office 365-Portal.
* Falls die neuen Tokensignaturzertifikate nicht abgerufen werden können, weil die Verbundmetadaten nicht erreichbar sind oder das automatische Zertifikatrollover nicht aktiviert ist, gibt Azure AD eine E-Mail-Benachrichtigung und eine Warnung im Office 365-Portal aus.

![Office 365-Portalbenachrichtigung](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Wenn Sie AD FS verwenden, überprüfen Sie zur Gewährleistung der Geschäftskontinuität, ob auf Ihren Servern die folgenden Updates installiert sind, damit keine Authentifizierungsfehler aufgrund von bereits bekannten Problemen auftreten. Dadurch werden bekannte AD FS-Proxyserverprobleme bei dieser Erneuerung und künftigen Erneuerungsperioden vermieden:
>
>Server 2012 R2 – [Windows Server May 2014 rollup](http://support.microsoft.com/kb/2955164) (Windows Server-Rollup von Mai 2014)
>
>Server 2008 R2 und 2012 – [Authentication through proxy fails in Windows Server 2012 or Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446) (Fehler bei der Authentifizierung über Proxy unter Windows Server 2008 oder Windows 2012 R2 R2)

## Überprüfen, ob die Zertifikate aktualisiert werden müssen <a name="managecerts"></a>

### Schritt 1: Überprüfen des AutoCertificateRollover-Status

Öffnen Sie auf dem AD FS-Server die PowerShell. Stellen Sie sicher, dass der AutoCertificateRollover-Wert auf „True“ festgelegt ist.

	Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Führen Sie bei Verwendung von AD FS 2.0 zuerst „Add-Pssnapin Microsoft.Adfs.Powershell“ aus.

### Schritt 2: Überprüfen, ob AD FS und Azure AD synchronisiert sind

Öffnen Sie auf dem AD FS-Server die Azure AD PowerShell-Eingabeaufforderung, und stellen Sie eine Verbindung mit Azure AD her.

>[AZURE.NOTE] Sie können Azure AD PowerShell [hier](https://technet.microsoft.com/library/jj151815.aspx) herunterladen.

	Connect-MsolService

Überprüfen Sie, ob die in AD FS und Azure AD konfigurierten Zertifikate den Eigenschaften für die angegebene Domäne vertrauen.

	Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Wenn die Fingerabdrücke in beiden Ausgaben übereinstimmen, sind Ihre Zertifikate mit Azure AD synchronisiert.

### Schritt 3: Überprüfen, ob das Zertifikat bald abläuft

Überprüfen Sie in der Ausgabe von „Get-MsolFederationProperty“ oder „Get-AdfsCertificate“ das Datum unter „Not after“ (Nicht nach). Falls das Datum weniger als 30 Tage entfernt ist, sollten Sie Maßnahmen ergreifen.

| AutoCertificateRollover | Mit Azure AD synchronisierte Zertifikate | Verbundmetadaten sind öffentlich zugänglich | Gültigkeitsdauer | Aktion |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Ja | Ja | Ja | - | Keine Aktion erforderlich. Siehe [Automatisches Erneuern von Tokensignaturzertifikaten](#autorenew). | 
| Ja | Nein | - | Weniger als 15 Tage | Sofort erneuern. Siehe [Manuelles Erneuern von Tokensignaturzertifikaten](#manualrenew). | 
| Nein | - | - | Weniger als 30 Tage | Sofort erneuern. Siehe [Manuelles Erneuern von Tokensignaturzertifikaten](#manualrenew). |

[-] Unwichtig

## Automatisches Erneuern von Tokensignaturzertifikaten (empfohlen) <a name="autorenew"></a>

Sie müssen keine manuellen Schritte ausführen, wenn die beiden folgenden Bedingungen erfüllt sind:
- Sie haben einen Webanwendungsproxy bereitgestellt, mit dem der Zugriff auf die Verbundmetadaten aus dem Extranet ermöglicht werden kann.
- Sie verwenden die AD FS-Standardkonfiguration (AutoCertificateRollover ist aktiviert).

Überprüfen Sie Folgendes, um sicherzustellen, dass das Zertifikat automatisch aktualisiert werden kann.

**1. Die AD FS-Eigenschaft „AutoCertificateRollover“ muss auf „True“ festgelegt sein.** Damit wird angegeben, dass AD FS automatisch neue Tokensignatur- und Tokenentschlüsselungszertifikate generiert, bevor die alten ablaufen.

**2. Die AD FS-Verbundmetadaten sind öffentlich zugänglich.** Überprüfen Sie, ob die Verbundmetadaten öffentlich zugänglich sind, indem Sie von einem Computer im öffentlichen Internet (außerhalb des Unternehmensnetzwerks) zur folgenden URL navigieren:


https://(your_FS_name)/federationmetadata/2007-06/federationmetadata.xml

Dabei wird `(your_FS_name) ` durch den Verbunddiensthostnamen ersetzt, den Ihre Organisation verwendet, z. B. "fs.contoso.com". Wenn Sie diese beiden Einstellungen erfolgreich überprüfen können, müssen Sie nichts weiter tun.

Beispiel: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Manuelles Erneuern des Tokensignaturzertifikats <a name="manualrenew"></a>

Sie können die Tokensignaturzertifikate auch manuell signieren. Beispielsweise funktionieren die folgenden Szenarien unter Umständen besser für die manuelle Erneuerung:
* Bei den Tokensignaturzertifikaten handelt es sich nicht um selbstsignierte Zertifikate. Die häufigste Ursache hierfür ist, dass Ihre Organisation AD FS-Zertifikate verwaltet, die von einer Organisationszertifizierungsstelle registriert werden.
* Die Netzwerksicherheit lässt nicht zu, dass die Verbundmetadaten öffentlich zugänglich sind.

In diesen Szenarien müssen Sie bei jeder Aktualisierung der Tokensignaturzertifikate auch die Office 365-Domäne mithilfe des PowerShell-Befehls „Update-MsolFederatedDomain“ aktualisieren.

### Schritt 1: Sicherstellen, dass AD FS über neue Tokensignaturzertifikate verfügt

**Nicht standardmäßige Konfiguration**

Wenn Sie eine nicht dem Standard entsprechende Konfiguration von AD FS nutzen, bei der **AutoCertificateRollover** auf **False** festgelegt ist, verwenden Sie wahrscheinlich benutzerdefinierte Zertifikate (nicht selbstsigniert). Weitere Informationen zur Erneuerung der AD FS-Tokensignaturzertifikate finden Sie unter [Anleitung für Kunden, die keine selbstsignierten AD FS-Zertifikate verwenden](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Die Verbundmetadaten sind nicht öffentlich verfügbar.**

Wenn **AutoCertificateRollover** auf **True** festgelegt ist, aber Ihre Verbundmetadaten nicht öffentlich zugänglich sind, sollten Sie sich zunächst vergewissern, ob von AD FS neue Tokensignaturzertifikate generiert wurden. Überprüfen Sie, ob Sie über neue Tokensignaturzertifikate verfügen, indem Sie die folgenden Schritte ausführen:

1. Stellen Sie sicher, dass Sie am primären AD FS-Server angemeldet sind.
2. Überprüfen Sie die aktuellen Signaturzertifikate in AD FS, indem Sie ein PowerShell-Befehlsfenster öffnen und den folgenden Befehl ausführen:

	PS C:\>Get-ADFSCertificate –CertificateType token-signing

	>[AZURE.NOTE] Bei Verwendung von AD FS 2.0 müssen Sie zuerst „Add-Pssnapin Microsoft.Adfs.Powershell“ ausführen.

3. Sehen Sie sich in der Ausgabe des Befehls die aufgeführten Zertifikate an. Wenn AD FS ein neues Zertifikat generiert hat, sollten Sie zwei Zertifikate in der Ausgabe sehen: Bei einem ist der **IsPrimary**-Wert **True** und das **NotAfter**-Datum liegt innerhalb von 5 Tagen, und beim anderen ist der **IsPrimary**-Wert **False** und das **NotAfter**-Datum liegt etwa ein Jahr in der Zukunft.

4. Wenn nur ein Zertifikat angezeigt wird und das **NotAfter**-Datum innerhalb von 5 Tagen liegt, müssen Sie ein neues Zertifikat generieren.

5. Um ein neues Zertifikat zu generieren, führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Überprüfen Sie das Update, indem Sie den folgenden Befehl erneut ausführen: PS C:\>Get-ADFSCertificate –CertificateType token-signing

Nun sollten zwei Zertifikate aufgeführt werden, bei denen das **NotAfter**-Datum in etwa ein Jahr in der Zukunft liegt und bei dem der **IsPrimary**-Wert **False** lautet.

### Schritt 2: Aktualisieren der neuen Tokensignaturzertifikate für die Office 365-Vertrauensstellung

Aktualisieren Sie Office 365 wie folgt mit den neuen Tokensignaturzertifikaten für die Vertrauensstellung:

1.	Öffnen Sie das Microsoft Azure Active Directory-Modul für Windows PowerShell.
2.	Führen Sie $cred=Get-Credential aus. Wenn Sie dieses Cmdlet zur Eingabe von Anmeldeinformationen auffordert, geben Sie die Anmeldeinformationen Ihres Clouddienstadministrators ein.
3.	Führen Sie Connect-MsolService –Credential $cred aus. Dieses Cmdlet verbindet Sie mit dem Clouddienst. Sie müssen einen Kontext erstellen, der Sie mit dem Clouddienst verbindet, bevor Sie andere Cmdlets ausführen, die vom Tool installiert werden.
4.	Wenn Sie diese Befehle auf einem Computer ausführen, der nicht der primäre AD FS-Verbundserver ist, führen Sie „Set-MSOLAdfscontext -Computer <primärer AD FS-Server>“ aus, wobei <primärer AD FS-Server> der interne FQDN des primären AD FS-Servers ist. Dieses Cmdlet erstellt einen Kontext, der Sie mit AD FS verbindet.
5.	Führen Sie „Update-MSOLFederatedDomain –DomainName <Domäne>“ aus. Dieses Cmdlet aktualisiert die Einstellungen von AD FS im Clouddienst und konfiguriert die Vertrauensstellung zwischen den beiden Komponenten.


>[AZURE.NOTE] Wenn Sie mehrere Domänen der obersten Ebene unterstützen müssen, z.B. „contoso.com“ und „fabrikam.com“, müssen Sie den **SupportMultipleDomain**-Switch mit den Cmdlets verwenden. Weitere Informationen finden Sie unter [Unterstützung mehrerer Domänen der obersten Ebene](active-directory-aadconnect-multiple-domains.md).

## Reparieren einer Azure AD-Vertrauensstellung mit Azure AD Connect <a name="connectrenew"></a>

Wenn Sie Ihre AD FS-Farm und die Azure AD-Vertrauensstellung unter Verwendung von Azure AD Connect konfiguriert haben, können Sie mit Azure AD Connect erkennen, ob Sie für Ihre Tokensignaturzertifikate Maßnahmen ergreifen müssen. Falls Sie die Zertifikate erneuern müssen, können Sie dafür Azure AD Connect nutzen.

Weitere Informationen finden Sie unter [Reparieren der Vertrauensstellung](./active-directory-aadconnect-federation-management.md#repairing-the-trust).

<!---HONumber=AcomDC_0928_2016-->