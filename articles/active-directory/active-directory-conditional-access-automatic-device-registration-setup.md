---
title: Einrichten der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten bei Azure Active Directory | Microsoft Docs
description: IT-Administratoren können Ihre in Domänen eingebundenen Windows-Geräte automatisch und im Hintergrund bei Azure Active Directory (Azure AD) registrieren.
services: active-directory
documentationcenter: ''
author: markusvi
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2016
ms.author: markvi

---
# <a name="how-to-setup-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Einrichten der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten bei Azure Active Directory
Windows-Computer, die in die Domäne eingebunden sind, müssen für Azure AD registriert werden, um den [gerätebasierten bedingten Zugriff für Azure Active Directory](active-directory-conditional-access.md)zu aktivieren.

Es sind Aktualisierungen vorhanden, wie z.B. die Möglichkeit, ein Geschäfts- oder Schulkonto zu verwenden, um erweiterte SSO-Funktionen in Azure AD-Apps nutzen zu können, geräteübergreifendes Enterprise Roaming von Einstellungen, Verwendung des Windows Store für Unternehmen sowie eine sicherere Authentifizierung und praktische Anmeldung mit Windows Hello. 

> [!NOTE]
> Das Windows 10 November 2015 Update unterstützt einige der Funktionen für eine verbesserte Benutzerumgebung, vollständige Unterstützung des gerätebasierten bedingten Zugriffs bietet jedoch das Anniversary Update.  
> Weitere Informationen zum bedingten Zugriff finden Sie unter [Gerätebasierter bedingter Zugriff für Azure Active Directory](active-directory-conditional-access.md).  
> Weitere Informationen zu Windows 10-Geräten am Arbeitsplatz und zum Benutzererlebnis bei der Registrierung für Azure AD finden Sie unter [Windows 10 für Unternehmen: Möglichkeiten der geschäftlichen Nutzung von Geräten](active-directory-azureadjoin-windows10-devices-overview.md). 
> 
> 

Die Registrierung wird u.a. von folgenden früheren Windows-Versionen unterstützt: 

* Windows 8.1 
* Windows 7 

Wenn Windows Server-Computer als Desktop verwendet werden (z.B. wenn ein Entwickler einen Windows Server-Computer als primären Computer verwendet), können folgende Plattformen registriert werden: 

* Windows Server 2016 
* Windows Server 2012 R2 
* Windows Server 2012 
* Windows Server 2008 R2 

Im Folgenden wird beschrieben, welche Schritte Sie in Ihrer Umgebung ausführen müssen, um die Registrierung von Windows-Geräten, die in die Domäne eingebunden sind, für Azure AD in Ihrer Organisation zu aktivieren. Sie sehen: 

1. Voraussetzungen 
2. Bereitstellung und Rollout 
3. Problembehandlung 
4. Häufig gestellte Fragen 

## <a name="prerequisites"></a>Voraussetzungen
Die wichtigste Voraussetzung für die Aktivierung der automatischen Registrierung von in die Domäne eingebundenen Geräten für Azure AD ist eine aktuelle Version von Azure AD Connect. 

Abhängig davon, wie Sie Azure AD Connect bereitgestellt haben (Express- oder benutzerdefinierte Installation bzw. direktes Upgrade), wurden die folgenden erforderlichen Komponenten möglicherweise automatisch konfiguriert: 

1. Dienstverbindungspunkt in der lokalen Active Directory-Bereitstellung für die Ermittlung von Azure AD-Mandanteninformationen von Computern, die in Azure AD registriert werden. 
2. AD FS-Ausstellungstransformationsregeln für die Computerauthentifizierung bei der Registrierung (gilt für Verbundkonfigurationen). 

Wenn in Ihrer Organisation Computer unter einem anderen Betriebssystem als Windows 10 in die Domäne eingebunden sind, müssen folgende Voraussetzungen erfüllt sein: 

1. Stellen Sie sicher, dass die Richtlinie, die Benutzern die Geräteregistrierung ermöglicht, in Azure AD aktiviert ist. 
2. Stellen Sie sicher, dass die integrierte Windows-Authentifizierung (WIA) als gültige Alternative zur mehrstufigen Authentifizierung (MFA) in AD FS festgelegt ist. 

## <a name="service-connection-point-for-discovery-of-azure-ad-tenant"></a>Dienstverbindungspunkt für die Ermittlung von Azure AD-Mandanten
Ein Dienstverbindungspunktobjekt, das Ermittlungsinformationen über den Azure AD-Mandanten enthält, bei dem Computer registriert werden sollen, muss in der Partition für den Konfigurationsnamenskontext der Gesamtstruktur der Domäne vorhanden sein, in die Computer eingebunden sind. In einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen muss der Dienstverbindungspunkt in allen Gesamtstrukturen vorhanden sein, denen Computer beigetreten sind. 

Der Dienstverbindungspunkt sollte sich in Active Directory an folgender Stelle befinden: 

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context] 

> [!NOTE]
> Für eine Gesamtstruktur mit dem Active Directory-Domänennamen „example.com“ lautet der Konfigurationsnamenskontext wie folgt: CN=Configuration,DC=example,DC=com 
> 
> 

Sie können das Vorhandensein des Objekts und die Werte für die Ermittlung des Azure AD-Mandanten mit dem folgenden PowerShell-Skript überprüfen (ersetzen Sie den Konfigurationsnamenskontext im Beispiel durch den eigenen): 

    $scp = New-Object System.DirectoryServices.DirectoryEntry; 

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com"; 

    $scp.Keywords; 

Die Ausgabe von $scp.Keywords zeigt die Azure AD-Mandanteninformationen wie folgt an: 

    azureADName:microsoft.com 

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47 

Wenn der Dienstverbindungspunkt nicht vorhanden ist, können Sie ihn mithilfe des folgenden PowerShell-Skripts auf dem Azure AD Connect-Server erstellen: 

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1"; 

    $aadAdminCred = Get-Credential; 

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred; 


> [!NOTE]
> Verwenden Sie beim Ausführen von „$aadAdminCred = Get-Credential“ das Format user@example.com für den Benutzernamen der Anmeldeinformationen, die eingegeben werden, wenn das Popupfenster „Get-Credential“ angezeigt wird.  
> Ersetzen Sie beim Ausführen des Cmdlets „Initialize-ADSyncDomainJoinedComputerSync“ den Connectorkontonamen ([connector account name]) durch das Domänenkonto, das als Active Directory-Connectorkonto verwendet wird.  
> Das obige Cmdlet verwendet das Active Directory-PowerShell-Modul, das von Active Directory-Webdiensten (ADWS) in Domänencontrollern (DCs) abhängt. AD-Webdienste werden in Domänencontrollern mit Windows Server 2008 R2 oder einer höheren Version unterstützt. Wenn Sie über Domänencontroller mit Windows Server 2008 oder früheren Versionen verfügen, können Sie die System.DirectoryServices-API über PowerShell verwenden, um den Dienstverbindungspunkt zu erstellen und die entsprechenden Schlüsselwortwerte zuzuweisen. 
> 
> 

## <a name="ad-fs-rules-for-instant-computer-registration-(federated-orgs)"></a>AD FS-Regeln für die sofortige Computerregistrierung (Verbundorganisationen)
In einer Verbundkonfiguration von Azure AD benötigen Computer AD FS (oder den lokalen Verbundserver) für die Authentifizierung gegenüber Azure AD, um die Registrierung beim Azure-Geräteregistrierungsdienst (Azure DRS) durchzuführen. 

> [!NOTE]
> In einer Konfiguration ohne Verbund (d.h. Benutzerkennworthashes, die mit Azure AD synchronisiert sind) werden in die Domäne eingebundene Windows 10- und Windows Server 2016-Computer mithilfe von Anmeldeinformationen gegenüber Azure DRS authentifiziert, die lokal in die Computerkonten geschrieben und über Azure AD Connect an Azure AD übermittelt werden. Informationen zu Computern unter einem anderen Betriebssystem als Windows 10/Windows Server 2016 in einer Konfiguration ohne Verbund finden Sie im Abschnitt zum Windows Installer-Paket für die Registrierung von in Domänen eingebundenen Computern unter einem anderen Betriebssystem als Windows 10/Windows Server 2016 unter „Bereitstellung und Rollout“ weiter unten in diesem Dokument. Dort finden Sie Optionen zur Aktivierung des gerätebasierten bedingten Zugriffs in Ihrer Organisation. 
> 
> 

Für Windows 10- und Windows Server 2016-Computer ordnet Azure AD Connect das Geräteobjekt in Azure AD dem lokalen Computerkonto-Objekt zu. Damit dies funktioniert, müssen die folgenden Ansprüche während der Authentifizierung vorhanden sein, so dass Azure DRS die Registrierung abschließen und das Geräteobjekt erstellen kann: 

* `http://schemas.microsoft.com/ws/2012/01/accounttype` : enthält den Wert „DJ“, der den authentifizierenden Prinzipal als einen in die Domäne eingebundenen Computer identifiziert. 
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid` : enthält den Wert des Attributs „objectGUID“ im lokalen Computerkonto. 
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid` : enthält die primäre SID des Computers, die dem Wert des Attributs „objectSid“ im lokalen Computerkonto entspricht. 
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid` : enthält den entsprechenden Wert, mit dem Azure AD dem von AD FS oder vom lokalen STS ausgegebenen Token vertrauen kann. Dies ist wichtig für eine Active Directory-Konfiguration mit mehreren Gesamtstrukturen, bei der Computer möglicherweise einer anderen Gesamtstruktur hinzugefügt werden als der, die mit Azure AD verbunden ist (und AD FS oder den lokalen STS enthält). Bei AD FS muss der Wert `http://<domain-name>/adfs/services/trust/` lauten, wobei „\<<domain-name>\>“ der validierte Domänenname in Azure AD ist. 

Um diese Regeln manuell zu erstellen, können Sie in AD FS das folgende PowerShell-Skript in einer Sitzung verwenden, die mit Ihrem Server verbunden ist. Beachten Sie, dass die erste Zeile durch den validierten Domänennamen in Azure AD für Ihre Organisation ersetzt werden muss. 

> [!NOTE]
> Wenn Sie nicht AD FS als lokalen Verbundserver nutzen, befolgen Sie die Anweisungen Ihres Anbieters, um die entsprechenden Regeln für diese Ansprüche zu erstellen. 
> 
> 

    $validatedDomain = "example.com"      # Replace example.com with your validated domain name in Azure AD 

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

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account" 

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]) 

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");' 

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID" 

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] && 

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"] 

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));' 

    $rule6 = '@RuleName = "Update issuer for DJ computer auth" 

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"] 

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");' 

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6 

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

> [!NOTE]
> In Domänen eingebundene Windows 10- und Windows Server 2016-Computer authentifizieren sich mit der integrierten Windows-Authentifizierung gegenüber einem aktiven, von AD FS gehosteten WS-Trust-Endpunkt. Stellen Sie sicher, dass dieser Endpunkt aktiviert ist. Wenn Sie einen Webauthentifizierungsproxy verwenden, stellen Sie außerdem sicher, dass dieser Endpunkt durch den Proxy veröffentlicht wird. Der Endpunkt ist „adfs/services/trust/13/windowstransport“. Er sollte in der AD FS-Verwaltungskonsole unter „Dienst > Endpunkte“ als aktiviert angezeigt werden. Wenn Sie nicht AD FS als lokalen Verbundserver nutzen, befolgen Sie die Anweisungen Ihres Anbieters, um sicherzustellen, dass der entsprechende Endpunkt aktiviert ist. 
> 
> 

## <a name="ensure-ad-fs-is-set-up-to-support-authentication-of-device-for-registration"></a>Sicherstellen, dass AD FS für die Unterstützung der Authentifizierung von Geräten für die Registrierung eingerichtet ist
Stellen Sie für die Geräteregistrierung sicher, dass die integrierte Windows-Authentifizierung (WIA) als gültige Alternative zur mehrstufigen Authentifizierung (MFA) in AD FS festgelegt ist.

Dafür benötigen Sie eine Ausstellungstransformationsregel, die die Authentifizierungsmethode weitergibt.

1. Öffnen Sie die AD FS-Verwaltungskonsole, und navigieren Sie zu **AD FS > Vertrauensstellungen > Vertrauensstellungen der vertrauenden Seite**. 
2. Klicken Sie mit der rechten Maustaste auf das Vertrauensstellungsobjekt der vertrauenden Seite, „Microsoft Office 365 Identity Platform“, und wählen Sie dann **Anspruchsregeln bearbeiten**aus.
3. Wählen Sie auf der Registerkarte **Ausstellungstransformationsregeln** die Option **Regel hinzufügen** aus.
4. Wählen Sie in der Vorlagenliste **Anspruchsregel** die Option **Ansprüche mit benutzerdefinierter Regel senden** aus. 
5. Wählen Sie **Weiter**.
6. Geben Sie **Anspruchsregel für Authentifizierungsmethode** in das Textfeld **Anspruchsregelname** ein.
7. Geben Sie im Textfeld **Anspruchsregel** Folgendes ein: `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
   => issue(claim = c);`
8. Öffnen Sie Windows PowerShell auf dem Verbundserver.
9. Geben Sie den folgenden Befehl ein: 
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

**\<RPObjectName>\** ist der Objektname der vertrauenden Seite für Ihr Azure Active Directory-Vertrauensstellungsobjekt der vertrauenden Seite. Dieses Objekt trägt normalerweise den Namen „Microsoft Office 365 Identity Platform“.

## <a name="deployment-and-roll-out"></a>Bereitstellung und Rollout
Nachdem die Voraussetzungen erfüllt worden sind, können in Domänen eingebundene Computer für Azure AD registriert werden. 

In Domänen eingebundene Computer mit Windows 10 Anniversary Update und Windows Server 2016 werden automatisch beim nächsten Neustart oder bei der nächsten Benutzeranmeldung bei Windows für Azure AD registriert. Neue Computer, die der Domäne hinzugefügt werden, werden beim Neustart nach dem Vorgang zum Einbinden des Computers in die Domäne bei Azure AD registriert. 

> [!NOTE]
> In die Domäne eingebundene Windows 10-Computer werden nur dann automatisch bei Azure AD registriert, wenn das Rollout-Gruppenrichtlinienobjekt festgelegt ist. Weitere Informationen finden Sie im folgenden Abschnitt. 
> 
> 

Um das Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern unter Windows 10/Windows Server 2016 zu steuern, kann ein Gruppenrichtlinienobjekt verwendet werden. Für das Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern unter einem anderen Betriebssystem als Windows 10 ist ein Windows Installer-Paket verfügbar, das auf ausgewählten Computern bereitgestellt werden kann. 

> [!NOTE]
> Die Gruppenrichtlinie für die Rolloutsteuerung löst auch die Registrierung von in die Domäne eingebundenen Windows 8.1-Computern aus. Sie können die Richtlinie für die Registrierung von in die Domäne eingebundenen Windows 8.1-Computern verwenden oder, wenn Sie über eine Kombination aus Windows 7- oder Windows Server-Versionen verfügen, die Registrierung aller Computer unter einem anderen Betriebssystem als Windows 10/Windows Server 2016 über das Windows Installer-Paket aktivieren. 
> 
> 

### <a name="group-policy-object-to-control-roll-out-of-automatic-registration"></a>Gruppenrichtlinienobjekt, um das Rollout der automatischen Registrierung zu steuern
Um das Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern bei Azure AD zu steuern, können Sie die in die Domäne „Gruppenrichtlinienregistrierung“ eingebundenen Computer als Geräte für die zu registrierenden Computer bereitstellen. Beispielsweise können Sie die Richtlinie basierend auf einer Sicherheitsgruppe oder in einer Organisationseinheit (OE) bereitstellen. 

Um die Richtlinie festzulegen, führen Sie die folgenden Schritte aus: 

1. Öffnen Sie den Server-Manager, und navigieren Sie zu **Tools > Gruppenrichtlinienverwaltung**. 
2. Navigieren Sie von der Gruppenrichtlinienverwaltung aus zu dem Domänenknoten, der der Domäne entspricht, in der Sie die automatische Registrierung von Windows 10- oder Windows Server 2016-Computern aktivieren möchten. 
3. Klicken Sie mit der rechten Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie dann **Neu** aus. 
4. Geben Sie für das Gruppenrichtlinienobjekt einen Namen ein, beispielsweise *Automatische Registrierung bei Azure AD*. Klicken Sie auf OK. 
5. Klicken Sie mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann **Bearbeiten**aus. 
6. Navigieren Sie zu **Computerkonfiguration > Richtlinien > Administrative Vorlagen > Windows-Komponenten > Geräteregistrierung**, klicken Sie mit der rechten Maustaste auf **In die Domäne eingebundene Computer als Geräte registrieren**, und wählen Sie dann **Bearbeiten** aus. 
   
   > [!NOTE]
   > In früheren Versionen der Gruppenrichtlinienverwaltungskonsole hatte diese Gruppenrichtlinienvorlage einen anderen Namen. Wenn Sie eine frühere Version der Konsole ausführen, finden Sie die Richtlinie unter „Computerkonfiguration“ > „Richtlinien“ > „Administrative Vorlagen“ > „Windows-Komponenten“ > Arbeitsplatzeinbindung mit dem Namen „Automatische Arbeitsplatzeinbindung von Clientcomputern“. 
   > 
   > 
7. Aktivieren Sie das Optionsfeld **Aktiviert**, und klicken Sie dann auf **Übernehmen**. 
8. Klicken Sie auf **OK**. 
9. Verknüpfen Sie das Gruppenrichtlinienobjekt mit einem Speicherort Ihrer Wahl. Wählen Sie z.B. eine bestimmte Organisationseinheit (OU), in der sich Computer befinden, oder eine bestimmte Sicherheitsgruppe mit Computern, die automatisch bei Azure AD registriert werden. Um diese Richtlinie für alle in die Domäne eingebundenen Windows 10- und Windows Server 2016-Computer in Ihrer Organisation zu aktivieren, verknüpfen Sie das Gruppenrichtlinienobjekt mit der Domäne. 

## <a name="msi-package-for-non-windows-10-computers"></a>MSI-Paket für Nicht-Windows 10-Computer
Für die Registrierung von in die Domäne eingebundenen Computern unter Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012 oder Windows Server 2012 R2 kann ein Windows Installer-Paket (MSI) heruntergeladen werden:

* [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
* [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Sie sollten dieses Paket mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager bereitstellen. Das Paket unterstützt die standardmäßigen Optionen für die automatische Installation unter Verwendung des Parameters „/quiet“. Die Verwendung von System Center Configuration Manager 2016 bietet zusätzliche Vorteile, z.B. die Möglichkeit zur Nachverfolgung abgeschlossener Registrierungen. Weitere Informationen finden Sie unter [System Center 2016](https://www.microsoft.com/cloud-platform/system-center-2016). 

Das Installationsprogramm erstellt einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt und ausgelöst wird, wenn sich der Benutzer bei Windows anmeldet. Nach der Authentifizierung über die integrierte Windows-Authentifizierung registriert der Task das Gerät unter Verwendung der Anmeldeinformationen des Benutzers automatisch bei Azure AD. Den geplanten Task finden Sie in der Aufgabenplanungsbibliothek unter **Microsoft > Arbeitsplatzeinbindung**. 

## <a name="additional-topics"></a>Weitere Themen
* [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md)

<!--HONumber=Oct16_HO2-->


