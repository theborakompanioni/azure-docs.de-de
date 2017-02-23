---
title: "Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory | Microsoft-Dokumentation"
description: "Einrichten Ihrer in Domänen eingebundenen Windows-Geräte für die automatische und im Hintergrund durchgeführte Registrierung bei Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 617599c7df6284e6319a1b3331d1e894e5d4b2d1
ms.openlocfilehash: 2ced7d0d4e2e653013c605932066c412c4202082


---
# <a name="how-to-configure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Es wird beschrieben, wie Sie die automatische Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory konfigurieren.

Wenn Sie den [gerätebasierten bedingten Zugriff über Azure Active Directory](active-directory-conditional-access.md) verwenden möchten, müssen Ihre Computer in Azure Active Directory (Azure AD) registriert werden. Dieser Artikel enthält die Schritte zum Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure AD in Ihrer Organisation.

> [!NOTE]
> Der November-Update für Windows 10 bietet bereits einige der erweiterten Benutzererfahrungen in Azure AD, aber das Windows 10 Anniversary Update unterstützt den gerätebasierten bedingten Zugriff vollständig. Weitere Informationen zum bedingten Zugriff finden Sie unter [Gerätebasierter bedingter Zugriff für Azure Active Directory](active-directory-conditional-access.md). Weitere Informationen zu Windows 10-Geräten am Arbeitsplatz und dazu, wie Benutzer Windows 10-Geräte bei Azure AD registrieren, finden Sie unter [Windows 10 für Unternehmen: Möglichkeiten der geschäftlichen Nutzung von Geräten](active-directory-azureadjoin-windows10-devices-overview.md).
> 
> 

Für Geräte, auf denen Windows ausgeführt wird, können Sie einige frühere Versionen von Windows registrieren, z.B.:

- Windows 8.1
- Windows 7

Für Geräte, auf denen Windows Server ausgeführt wird, können Sie die folgenden Plattformen registrieren:

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2



## <a name="prerequisites"></a>Voraussetzungen

Die wichtigste Voraussetzung für die automatische Registrierung von in die Domäne eingebundenen Geräten mit Azure AD ist eine aktuelle Version von Azure Active Directory Connect (Azure AD Connect).

Abhängig davon, wie Sie Azure AD Connect bereitgestellt haben (Express- oder benutzerdefinierte Installation bzw. direktes Upgrade), wurden die folgenden Voraussetzungen möglicherweise automatisch konfiguriert:

- **Dienstverbindungspunkt in der lokalen Active Directory-Instanz:** Für die Ermittlung von Azure AD-Mandanteninformationen mit Computern, die für Azure AD registriert werden.
 
- **AD FS-Ausstellungstransformationsregeln (Active Directory Federation Services):** Für die Computerauthentifizierung bei der Registrierung (gilt für Verbundkonfigurationen).

Führen Sie die folgenden Schritte aus, wenn einige Geräte in Ihrer Organisation keine in die Domäne eingebundenen Windows 10-Geräte sind:

* Festlegen einer Richtlinie in Azure AD, um Benutzern das Registrieren von Geräten zu ermöglichen
* Einrichten der integrierten Windows-Authentifizierung (IWA) als gültige Alternative zur Multi-Factor Authentication (MFA) in AD FS

## <a name="step-1-configure-service-connection-point"></a>Schritt 1: Konfigurieren des Dienstverbindungspunkts 

Ein Dienstverbindungspunkt-Objekt (Service Connection Point, SCP) muss in der Partition für den Konfigurationsnamenskontext der Computerdomäne vorhanden sein. Der Dienstverbindungspunkt enthält die ermittelten Informationen zum Azure AD-Mandanten, beim dem die Computer registriert werden. In einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen muss der Dienstverbindungspunkt in allen Gesamtstrukturen vorhanden sein, die in die Domäne eingebundene Computer enthalten.

Der Dienstverbindungspunkt befindet sich unter:  

**CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Ihr Konfigurationsnamenskontext]**

Für eine Gesamtstruktur mit dem Active Directory-Domänennamen *example.com* lautet der Konfigurationsnamenskontext:  

**CN=Configuration,DC=example,DC=com**

Mit dem folgenden Windows PowerShell-Skript können Sie das Vorhandensein des Objekts überprüfen und die Ermittlungswerte abrufen: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

Die Ausgabe von **$scp.Keywords** enthält die Azure AD-Mandanteninformationen, z.B.:

azureADName:microsoft.com  
azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Wenn der Dienstverbindungspunkt nicht vorhanden ist, erstellen Sie ihn mithilfe des folgenden PowerShell-Skripts auf Ihrem Azure AD Connect-Server:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;



**Hinweise:**

- Beim Ausführen von **$aadAdminCred = Get-Credential** müssen Sie einen Benutzernamen eingeben. Verwenden Sie für den Benutzernamen das folgende Format: **user@example.com** 


- Ersetzen Sie beim Ausführen des Cmdlets **Initialize-ADSyncDomainJoinedComputerSync** den [*Connectorkontonamen*] durch das Domänenkonto, das als Active Directory-Connectorkonto verwendet wird.
  
- Das Cmdlet verwendet das Active Directory-PowerShell-Modul, das von Active Directory-Webdiensten auf einem Domänencontroller abhängt. Active Directory-Webdienste werden auf Domänencontrollern unter Windows Server 2008 R2 und höher unterstützt. Verwenden Sie für Domänencontroller in Windows Server 2008 oder früher die API System.DirectoryServices mithilfe von PowerShell, um den Dienstverbindungspunkt zu erstellen, und weisen Sie dann die Werte der Schlüsselwörter zu.
 
 



##<a name="step-2-register-your-devices"></a>Schritt 2: Registrieren der Geräte

Die richtigen Schritte zum Registrieren des Geräts hängen davon ab, ob für Ihre Organisation ein Verbund verwendet wird. 


### <a name="device-registration-in-non-federated-organizations"></a>Registrierung von Geräten in Organisationen ohne Verbund

Die Registrierung von Geräten in Organisationen ohne Verbund wird nur unterstützt, wenn folgende Voraussetzungen erfüllt sind:

- Auf dem Gerät wird entweder Windows 10 oder Windows Server 2016 ausgeführt
- Die Geräte sind in eine Domäne eingebunden
- Die Kennwortsynchronisierung mit Azure AD Connect ist aktiviert

Wenn diese Anforderungen vollständig erfüllt sind, müssen Sie nichts weiter unternehmen, um die Geräte zu registrieren.  


### <a name="device-registration-in-federated-organizations"></a>Registrierung von Geräten in Organisationen mit Verbund

In einer Azure AD-Verbundkonfiguration benötigen die Geräte AD FS (oder den lokalen Verbundserver) zum Authentifizieren in Azure AD. Sie werden über den Azure Active Directory-Geräteregistrierungsdienst registriert.

Für Windows 10- und Windows Server 2016-Computer ordnet Azure AD Connect das Geräteobjekt in Azure AD dem lokalen Computerkonto-Objekt zu. Die folgenden Ansprüche müssen während der Authentifizierung vorhanden sein, damit der Azure AD-Geräteregistrierungsdienst die Registrierung abschließen und das Geräteobjekt erstellen kann:

- **http://schemas.microsoft.com/ws/2012/01/accounttype**: Enthält den DJ-Wert, der den Prinzipalauthentifikator als in die Domäne eingebundenen Computer identifiziert.

- **http://schemas.microsoft.com/identity/claims/onpremobjectguid**: Enthält den Wert des **objectGUID**-Attributs für das lokale Computerkonto.
 
- **http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid**: Enthält die primäre Sicherheits-ID (SID) des Computers, die dem Wert des **objectSid**-Attributs des lokalen Computerkontos entspricht.

- **http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid**: Enthält den Wert, mit dem Azure AD die Vertrauensstellung für den Token bewertet, das von AD FS oder vom lokalen Sicherheitstokendienst (Security Token Service, STS) ausgestellt wurde. Dies ist wichtig, wenn Sie in Azure AD über mehrere überprüfte Domänen verfügen. Bei AD FS verwenden Sie den Wert **http://\<*Domänenname*\>/adfs/services/trust/**, wobei **\<Domänenname\>** der überprüfte Domänenname in Azure AD ist.

Weitere Informationen zu überprüften Domänennamen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-add-domain.md).  
Zum Abrufen einer Liste mit Ihren überprüften Unternehmensdomänen können Sie das [Get-MsolDomain](https://docs.microsoft.com/powershell/msonline/v1/get-msoldomain)-Cmdlet verwenden. 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)


In Domänen eingebundene Windows 10- und Windows Server 2016-Computer authentifizieren sich mit der integrierten Windows-Authentifizierung gegenüber einem aktiven, von AD FS gehosteten WS-Trust-Endpunkt. Stellen Sie sicher, dass dieser Endpunkt aktiviert ist. Wenn Sie einen Webauthentifizierungsproxy verwenden, stellen Sie außerdem sicher, dass dieser Endpunkt durch den Proxy veröffentlicht wird. Der Endpunkt ist **adfs/services/trust/13/windowstransport**. 

Er sollte in der AD FS-Verwaltungskonsole unter **Dienst > Endpunkte** aktiviert werden. Wenn Sie nicht AD FS als lokalen Verbundserver nutzen, befolgen Sie die Anweisungen Ihres Anbieters, um sicherzustellen, dass der entsprechende Endpunkt aktiviert ist. 



> [!NOTE]
> Wenn Sie als lokalen Verbundserver nicht AD FS nutzen, befolgen Sie die Anweisungen Ihres Anbieters, um die Regeln für diese Ansprüche zu erstellen.
> 
> 




**Gehen Sie in AD FS wie folgt vor, um die Regeln manuell zu erstellen:**

- Wählen Sie eines der folgenden Windows PowerShell-Skripts aus. 
- Führen Sie das Windows PowerShell-Skript in einer Sitzung aus, die mit Ihrem Server verbunden ist. 
- Ersetzen Sie die erste Zeile durch den überprüften Domänennamen Ihrer Organisation in Azure AD.




#### <a name="setting-ad-fs-rules-in-a-single-domain-environment"></a>Festlegen von AD FS-Regeln in einer Umgebung mit einer Einzeldomäne

Verwenden Sie das folgende Skript, um die AD FS-Regeln hinzuzufügen, falls Sie nur über **eine überprüfte Domäne** verfügen:


    <#----------------------------------------------------------------------
    |   Modify the Azure AD Relying Party to include the claims needed
    |   for DomainJoin++. The rules include:
    |   -ObjectGuid
    |   -AccountType
    |   -ObjectSid
    +---------------------------------------------------------------------#>

    $rule1 = '@RuleName = "Issue object GUID" 

    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "-515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] && 

    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] 

    => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);' 

    $rule2 = '@RuleName = "Issue account type for domain joined computers" 

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "-515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] 

    => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");' 

    $rule3 = '@RuleName = "Pass through primary SID" 

    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "-515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] && 

    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] 

    => issue(claim = c2);' 

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 


#### <a name="setting-ad-fs-rules-in-a-multi-domain-environment"></a>Festlegen von AD FS-Regeln in einer Umgebung mit mehreren Domänen

Führen Sie die folgenden Schritte aus, wenn Sie mehr als eine überprüfte Domäne verwenden:

1. Entfernen Sie die vorhandene **IssuerID**-Regel, die mit Azure AD Connect erstellt wurde.  
Hier ist ein Beispiel für diese Regel angegeben: c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/")); 


2. Führen Sie dieses Skript aus: 

        <#----------------------------------------------------------------------  
        |   Modify the Azure AD Relying Party to include the claims needed  
        |   for DomainJoin++. The rules include:
        |   -ObjectGuid
        |   -AccountType
        |   -ObjectSid
        +---------------------------------------------------------------------#>

        $VerifiedDomain = 'example.com'      # Replace example.com with one of your verified domains

        $rule1 = '@RuleName = "Issue object GUID" 

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "-515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] && 

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] 

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);' 

        $rule2 = '@RuleName = "Issue account type for domain joined computers" 

        c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "-515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] 

        => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");' 

        $rule3 = '@RuleName = "Pass through primary SID" 

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "-515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] && 

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] 

        => issue(claim = c2);' 

        $rule4 = '@RuleName = "Issue AccountType with the value User when its not a computer account" 

        NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]) 

        => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");' 

        $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID" 

        c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] && 

        c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"] 

        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));' 

        $rule6 = '@RuleName = "Update issuer for DJ computer auth" 

        c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"] 

        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$VerifiedDomain+'/adfs/services/trust/");' 

        $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

        $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4+ $rule5+  $rule6 

        $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

        Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 



## <a name="step-3-setup-ad-fs-for-authentication-of-device-registration"></a>Schritt 3: Einrichten von AD FS für die Authentifizierung von Geräteregistrierungen

Vergewissern Sie sich, dass WIA als gültige Alternative zur Multi-Factor Authentication für die Geräteregistrierung in AD FS festgelegt ist. Dafür benötigen Sie eine Ausstellungstransformationsregel, die die Authentifizierungsmethode weitergibt.

1. Navigieren Sie in der AD FS-Verwaltungskonsole zu **AD FS** > **Vertrauensstellungen** > **Vertrauensstellungen der vertrauenden Seite**.
2. Klicken Sie mit der rechten Maustaste auf das Vertrauensstellungsobjekt der vertrauenden Seite, „Microsoft Office 365 Identity Platform“, und wählen Sie dann **Anspruchsregeln bearbeiten** aus.
3. Wählen Sie auf der Registerkarte **Ausstellungstransformationsregeln** die Option **Regel hinzufügen** aus.
4. Wählen Sie in der Vorlagenliste **Anspruchsregel** die Option **Ansprüche mit benutzerdefinierter Regel senden** aus.
5. Wählen Sie **Weiter**.
6. Geben Sie in das Feld **Anspruchsregelname** den Text **Anspruchsregel für Authentifizierungsmethode** ein.
7. Geben Sie im Feld **Anspruchsregel** diese Regel ein:  
**c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);**
8. Geben Sie auf dem Verbundserver diesen PowerShell-Befehl ein:
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

**\<RPObjectName\>** ist der Objektname der vertrauenden Seite für Ihr Azure AD-Vertrauensstellungsobjekt der vertrauenden Seite. Dieses Objekt trägt normalerweise den Namen **Microsoft Office 365 Identity Platform**.



##<a name="step-4-deployment-and-rollout"></a>Schritt 4: Bereitstellung und Rollout

Nachdem die Voraussetzungen erfüllt wurden, können in Domänen eingebundene Computer bei Azure AD registriert werden.

In Domänen eingebundene Computer mit Windows 10 Anniversary Update und Windows Server 2016 werden automatisch beim nächsten Geräteneustart oder bei der nächsten Benutzeranmeldung bei Windows für Azure AD registriert. Neue Computer, die der Domäne hinzugefügt werden, werden beim Geräteneustart nach dem Vorgang zum Einbinden des Computers in die Domäne bei Azure AD registriert.

> [!NOTE]
> In Domänen eingebundene Windows 10 Computer mit dem November-Update für Windows 10 werden nur dann automatisch bei Azure AD registriert, wenn das Rollout-Gruppenrichtlinienobjekt festgelegt ist.
> 
> 

Sie können ein Gruppenrichtlinienobjekt verwenden, um den Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern unter Windows 10/Windows Server 2016 zu steuern. 

Für den Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern unter einem anderen Betriebssystem als Windows 10 ist ein Windows Installer-Paket verfügbar, das auf den von Ihnen ausgewählten Computern bereitgestellt werden kann.

> [!NOTE]
> Für alle Nicht-Windows 10/Windows Server 2016-Computer wird empfohlen, das Windows Installer-Paket wie nachfolgend in diesem Dokument beschrieben zu verwenden.
> 
> 

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Erstellen eines Gruppenrichtlinienobjekts zum Steuern des Rollouts der automatischen Registrierung

Um den Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern bei Azure AD zu steuern, können Sie die Gruppenrichtlinie **In die Domäne eingebundene Computer als Geräte registrieren** für die zu registrierenden Computer bereitstellen. Beispielsweise können Sie die Richtlinie für eine Organisationseinheit oder eine Sicherheitsgruppe bereitstellen.

**Legen Sie die Richtlinie fest:**

1. Öffnen Sie den Server-Manager, und navigieren Sie zu **Tools** > **Gruppenrichtlinienverwaltung**.
2. Wechseln Sie zu dem Domänenknoten, der der Domäne entspricht, in der Sie die automatische Registrierung von Computern unter Windows 10 oder Windows Server 2016 aktivieren möchten.
3. Klicken Sie mit der rechten Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie dann **Neu** aus.
4. Geben Sie einen Namen für das Gruppenrichtlinienobjekt ein. Beispiel: *Automatische Registrierung bei Azure AD*. Klicken Sie auf **OK**.
5. Klicken Sie mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann **Bearbeiten** aus.
6. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Geräteregistrierung**. Klicken Sie mit der rechten Maustaste auf **In die Domäne eingebundene Computer als Geräte registrieren**, und wählen Sie dann **Bearbeiten** aus.
   
   > [!NOTE]
   > In früheren Versionen der Gruppenrichtlinien-Verwaltungskonsole hatte diese Gruppenrichtlinienvorlage einen anderen Namen. Wenn Sie eine frühere Version der Konsole verwenden, navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Arbeitsbereichverknüpfung** > **Automatically workplace join client computers** (Clientcomputer automatisch in Arbeitsbereich einbinden).
   > 
   > 
7. Wählen Sie **Aktiviert** und dann **Übernehmen** aus.
8. Klicken Sie auf **OK**.
9. Verknüpfen Sie das Gruppenrichtlinienobjekt jetzt mit einem Speicherort Ihrer Wahl. Sie können es beispielsweise mit einer bestimmten Organisationseinheit verknüpfen. Sie können es aber auch mit einer bestimmten Sicherheitsgruppe von Computern verbinden, die automatisch bei Azure AD registriert werden. Um diese Richtlinie für alle in die Domäne eingebundenen Windows 10- und Windows Server 2016-Computer in Ihrer Organisation festzulegen, verknüpfen Sie das Gruppenrichtlinienobjekt mit der Domäne.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Windows Installer-Pakete für Computer ohne Windows 10
Für die Registrierung von in die Domäne eingebundenen Computern unter Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 in einer Verbundumgebung können Sie die folgenden Windows Installer-Paketdateien (.msi) herunterladen und installieren:

* [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
* [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Stellen Sie das Paket mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager bereit. Das Paket unterstützt die Standardoptionen für die Installation im Hintergrund unter Verwendung des Parameters *quiet*. System Center Configuration Manager 2016 bietet zusätzliche Vorteile gegenüber früheren Versionen, z.B. die Möglichkeit zur Nachverfolgung abgeschlossener Registrierungen. Weitere Informationen finden Sie unter [System Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Das Installationsprogramm erstellt einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt wird. Der Task wird ausgelöst, wenn sich der Benutzer bei Windows anmeldet. Nach der Authentifizierung durch IWA registriert der Task das Gerät unter Verwendung der Anmeldeinformationen des Benutzers im Hintergrund bei Azure AD. Um den geplanten Task anzuzeigen, wechseln Sie zu **Microsoft** > **Arbeitsbereichverknüpfung**, und navigieren Sie dann zur Aufgabenplanungsbibliothek.

## <a name="next-steps"></a>Nächste Schritte

- Wenn während der automatischen Geräteregistrierung ein Fehler auftritt, finden Sie hilfreiche Informationen in den folgenden Themen zur Problembehandlung:
    - [Für Windows 10 und Windows Server 2016](active-directory-conditional-access-automatic-device-registration-troubleshoot-windows.md)
    - [Für kompatible Windows-Clients](active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy.md)
- Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zur automatischen Geräteregistrierung](active-directory-conditional-access-automatic-device-registration-faq.md).




<!--HONumber=Feb17_HO2-->


