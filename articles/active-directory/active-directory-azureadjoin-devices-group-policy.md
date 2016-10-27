<properties
    pageTitle="Verbinden von in die Domäne eingebundenen Geräten mit Azure AD für Windows 10-Funktionen | Microsoft Azure"
    description="Erklärt, wie Gruppenrichtlinien konfiguriert werden müssen, damit Geräte durch Domänenbeitritt ins Unternehmensnetzwerk eingebunden werden können."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Verbinden von in die Domäne eingebundenen Geräten mit Azure AD für Windows 10-Funktionen

Der Domänenbeitritt ist das herkömmliche Verfahren, mit dem Unternehmen in den letzten 15 oder mehr Jahren eine Verbindung für geschäftliche Geräte hergestellt haben. Benutzer konnten sich mit ihren Geschäfts- oder Schulkonten in Windows Server Active Directory (Active Directory) bei ihren Geräten anmelden, und die IT-Mitarbeiter konnten diese Geräte vollständig verwalten. Organisationen nutzen normalerweise Verfahren für die Imageerstellung, um Geräte für Benutzer bereitzustellen, und sie verwalten die Geräte im Allgemeinen mit System Center Configuration Manager (SCCM) oder mit Gruppenrichtlinien.

Ein Domänenbeitritt unter Windows 10 bietet nach der Verbindung von Geräten mit Azure Active Directory (Azure AD) folgende Vorteile:

- Einmaliges Anmelden (SSO, Single Sign-On) bei Azure AD-Ressourcen von jedem Ort aus
- Zugriff auf Windows Store des Unternehmens per Geschäfts- oder Schulkonto (kein Microsoft-Konto erforderlich)
- Für Unternehmen geeignetes geräteübergreifendes Roaming von Benutzereinstellungen per Geschäfts- oder Schulkonto (kein Microsoft-Konto erforderlich)
- Strenge Authentifizierung und bequeme Anmeldung beim Geschäfts- oder Schulkonto mit Microsoft Passport und Windows Hello
- Möglichkeit, den Zugriff auf Geräte zu beschränken, die den Gruppenrichtlinien der Organisation für Geräte entsprechen

## <a name="prerequisites"></a>Voraussetzungen

Beitritte zu einer Domäne sind weiterhin nützlich. Damit Sie jedoch die Vorteile von Azure AD nutzen können, z. B. für einmaliges Anmelden, Roaming von Einstellungen bei Geschäfts- oder Schulkonten, Zugriff auf den Windows Store mit Geschäfts- oder Schulkonten, benötigen Sie Folgendes:

- Azure AD-Abonnement
- Azure AD Connect, um das lokale Verzeichnis auf Azure AD zu erweitern
- Eine Richtlinie für Geräte, die einer Domäne beigetreten sind, zum Herstellen einer Verbindung mit Azure AD
- Windows 10 (Build 10551 oder neuer) für die Geräte

Zum Aktivieren von Microsoft Passport for Work und Windows Hello benötigen Sie zusätzlich Folgendes:

- Eine Public Key-Infrastruktur (PKI) zum Ausstellen von Benutzerzertifikaten.
- System Center Configuration Manager, Version 1509 für Technical Preview Weitere Informationen erhalten Sie unter [Microsoft System Center Configuration Manager Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) (Microsoft System Center Configuration Manager: Technische Vorschau) und [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx) (System Center Configuration Manager: Team-Blog). Dies ist erforderlich, um Benutzerzertifikate auf der Basis von Microsoft Passport-Schlüsseln bereitzustellen.

Alternativ zur PKI-Bereitstellung haben Sie folgende Möglichkeit:

- Verwenden einiger Domänencontroller mit Windows Server 2016 Active Directory Domain Services

Um einen bedingten Zugriff zu ermöglichen, können Sie Gruppenrichtlinieneinstellungen erstellen, die den Zugriff auf in die Domäne eingebundene Geräte ohne weitere Bereitstellungen zulassen. Um die Zugriffssteuerung anhand der Gerätecompliance zu verwalten, benötigen Sie Folgendes:

- System Center Configuration Manager, Version 1509 für Technical Preview, für Passport-Szenarien

## <a name="deployment-instructions"></a>Anweisungen zur Bereitstellung



### <a name="step-1:-deploy-azure-active-directory-connect"></a>Schritt 1: Bereitstellen von Azure Active Directory Connect

Azure AD Connect ermöglicht Ihnen die Bereitstellung lokaler Computer als Geräteobjekte in der Cloud. Informationen zum Bereitstellen von Azure AD Connect finden Sie unter „Installieren von Azure AD Connect“ im Artikel [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect).

 - Wenn Sie eine [benutzerdefinierte Installation für Azure AD Connect](./aad-connect/active-directory-aadconnect-get-started-custom.md) ausgeführt haben (nicht die Express-Installation), führen Sie das später in diesem Schritt beschriebene Verfahren **Erstellen eines Dienstverbindungspunkts im lokalen Active Directory** aus.
 - Wenn Sie vor dem Installieren von Azure AD Connect über eine Verbundkonfiguration für Azure AD verfügen (z.B. weil Sie zuvor Active Directory-Verbunddienste [AD FS] bereitgestellt haben), führen Sie das später in diesem Schritt beschriebene Verfahren **Konfigurieren von AD FS-Anspruchsregeln** aus.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Erstellen eines Dienstverbindungspunkts im lokalen Active Directory

In die Domäne eingebundene Geräte verwenden den Dienstverbindungspunkt, um während der automatischen Registrierung beim Azure-Geräteregistrierungsdienst die Azure AD-Mandanteninformationen zu ermitteln.

Führen Sie auf dem Azure AD Connect-Server die folgenden PowerShell-Befehle aus:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Verwenden Sie beim Ausführen des Cmdlets „$aadAdminCred = Get-Credential“ das Format *user@example.com* für den Benutzernamen der Anmeldeinformationen, die eingegeben werden, wenn das Popupfenster „Get-Credential“ angezeigt wird.

Ersetzen Sie beim Ausführen des Cmdlets „Initialize-ADSyncDomainJoinedComputerSync...“ den Konnektorkontonamen ([*connector account name*]) durch das Domänenkonto, das als Active Directory-Konnektorkonto verwendet wird.

#### <a name="configure-ad-fs-claim-rules"></a>Konfigurieren von AD FS-Anspruchsregeln
Konfigurieren von AD FS-Anspruchsregeln ermöglicht die sofortige Registrierung eines Computers beim Azure-Geräteregistrierungsdienst, indem den Computern die Authentifizierung mit Kerberos/NTLM über AD FS erlaubt wird. Ohne diesen Schritt werden Computer nur mit Verzögerung in Azure AD eingebunden (abhängig von den Synchronisationszeiten von Azure AD Connect).

>[AZURE.NOTE]
Wenn Sie lokal nicht mit AD FS als Verbundserver arbeiten, befolgen Sie die Anweisungen Ihres Anbieters, um die Anspruchsregeln zu erstellen.

Führen Sie auf Ihrem AD FS-Server (oder in einer mit dem AD FS-Server verbundenen Sitzung) die folgenden PowerShell-Befehle aus:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10-Computer authentifizieren sich mit der integrierten Windows-Authentifizierung bei einem aktiven von AD FS gehosteten WS-Trust-Endpunkt. Stellen Sie sicher, dass dieser Endpunkt aktiviert ist. Wenn Sie einen Webauthentifizierungsproxy verwenden, stellen Sie außerdem sicher, dass dieser Endpunkt durch den Proxy veröffentlicht wird. Überprüfen Sie hierzu adfs/services/trust/13/windowstransport. Er sollte in der AD FS-Verwaltungskonsole unter „**Dienst** > **Endpunkte**“ als aktiviert angezeigt werden.


### <a name="step-2:-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Schritt 2: Konfigurieren der automatischen Geräteregistrierung mithilfe einer Gruppenrichtlinie in Active Directory

Sie können eine Active Directory-Gruppenrichtlinie verwenden, um Ihre in die Domäne eingebundenen Windows 10-Geräte für die automatische Registrierung bei Azure AD zu konfigurieren.

> [AZURE.NOTE]
> Aktuelle Anweisungen zum Einrichten der automatischen Geräteregistrierung finden Sie unter [Einrichten der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten bei Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Die Gruppenrichtlinienvorlage wurde in Windows 10 umbenannt. Wenn Sie das Gruppenrichtlinientool auf einem Windows 10-Computer ausführen, wird die Richtlinie angezeigt als:  <br>
> **In die Domäne eingebundene Computer als Geräte registrieren**<br>
> Die Richtlinie befindet sich an folgendem Speicherort:<br>
> ***Computerkonfiguration/Richtlinien/Administrative Vorlagen/Windows-Komponenten/Geräteregistrierung***


## <a name="additional-information"></a>Zusätzliche Informationen
* [Windows 10 für Unternehmen: Möglichkeiten der geschäftlichen Nutzung von Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloudfunktionen auf Windows 10-Geräte über Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Weitere Informationen zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Benutzererfahrungen beim Verknüpfen von in die Domäne eingebundenen Windows 10-Geräten mit Azure AD](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)



<!--HONumber=Oct16_HO2-->


