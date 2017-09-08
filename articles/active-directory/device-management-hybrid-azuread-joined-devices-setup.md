---
title: "Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte konfigurieren."
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
ms.date: 09/07/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 4580075df9fce74664b22aa24065ba1885692384
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="how-to-configure-hybrid-azure-active-directory-joined-devices"></a>Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten

Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

Wenn Sie in einer lokalen Active Directory-Umgebung Ihre in die Domäne eingebundenen Geräte in Azure AD einbinden möchten, kann dies durch Konfigurieren von in Azure AD eingebundenen Hybridgeräten erfolgen. Die entsprechenden Schritte werden in diesem Thema beschrieben. 


## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie beginnen, in Azure AD eingebundene Hybridgeräte in Ihrer Umgebung zu konfigurieren, sollten Sie sich mit den unterstützten Szenarien und den Einschränkungen vertraut machen.  

In diesem Thema werden die folgenden Bezeichnungen verwendet, um die Lesbarkeit der Beschreibungen zu erleichtern: 

- **Aktuelle Windows-Geräte**: Diese Bezeichnung bezieht sich auf in die Domäne eingebundene Geräte, auf denen Windows 10 oder Windows Server 2016 ausgeführt wird.
- **Kompatible Windows-Geräte**: Diese Bezeichnung bezieht sich auf alle **unterstützten** in die Domäne eingebundenen Windows-Geräte, auf denen weder Windows 10 noch Windows Server 2016 ausgeführt wird.  


### <a name="windows-current-devices"></a>Aktuelle Windows-Geräte

- Für Geräte, auf denen das Windows-Desktopbetriebssystem ausgeführt wird, empfehlen wir die Verwendung von Windows 10 Anniversary Update (Version 1607) oder höher. 
- Die Registrierung von aktuellen Windows-Geräten **wird** in nicht zu einem Verbund gehörenden Umgebungen unterstützt, z.B. Konfigurationen mit Kennworthashsynchronisierung.  


### <a name="windows-down-level-devices"></a>Kompatible Windows-Geräte

- Die folgenden kompatiblen Windows-Geräte werden unterstützt:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- Die Registrierung kompatibler Windows-Geräte **wird** in nicht zu einem Verbund gehörenden Umgebungen durch die [nahtlose einmalige Anmeldung mit Azure Active Directory](https://aka.ms/hybrid/sso) unterstützt.
- Für Geräte, die Roamingprofile verwenden, wird die Registrierung kompatibler Windows-Geräte **nicht** unterstützt. Verwenden Sie Windows 10, wenn Sie das Roaming von Profilen oder Einstellungen nutzen.



## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit dem Aktivieren von in Azure AD eingebundenen Hybridgeräten in Ihrer Organisation beginnen, müssen Sie sicherstellen, dass Sie eine aktuelle Version von Azure AD Connect verwenden.

Azure AD Connect:

- Sorgt dafür, dass die Zuordnung zwischen dem Computerkonto in Ihrer lokalen AD-Instanz (Active Directory) und dem Geräteobjekt in Azure AD beibehalten wird. 
- Ermöglicht die Verwendung von anderen gerätebezogenen Features, z.B. Windows Hello for Business.



## <a name="configuration-steps"></a>Konfigurationsschritte

Sie können in Azure AD eingebundene Hybridgeräte für verschiedene Windows-Geräteplattformen konfigurieren. Dieses Thema enthält die erforderlichen Schritte für alle typischen Konfigurationsszenarien.  

Verwenden Sie die folgende Tabelle, um eine Übersicht über die Schritte zu erhalten, die für Ihr Szenario erforderlich sind:  



| Schritte                                      | Aktuelle Windows-Geräte und Kennworthashsynchronisierung | Aktuelle Windows-Geräte und Verbund | Kompatible Windows-Geräte |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Schritt 1: Konfigurieren des Dienstverbindungspunkts | ![Prüfen][1]                            | ![Prüfen][1]                    | ![Prüfen][1]        |
| Schritt 2: Einrichten der Ausstellung von Ansprüchen           |                                        | ![Prüfen][1]                    | ![Prüfen][1]        |
| Schritt 3: Aktivieren von Geräten, auf denen nicht Windows 10 ausgeführt wird      |                                        |                                | ![Prüfen][1]        |
| Schritt 4: Steuern der Bereitstellung und des Rollouts     | ![Prüfen][1]                            | ![Prüfen][1]                    | ![Prüfen][1]        |
| Schritt 5: Überprüfen der eingebundenen Geräte          | ![Prüfen][1]                            | ![Prüfen][1]                    | ![Prüfen][1]        |



## <a name="step-1-configure-service-connection-point"></a>Schritt 1: Konfigurieren des Dienstverbindungspunkts

Das SCP-Objekt (Service Connection Point, Dienstverbindungspunkt) wird von Ihren Geräten während der Registrierung verwendet, um Informationen zum Azure AD-Mandanten zu ermitteln. In Ihrer lokalen Active Directory-Instanz (AD) muss das SCP-Objekt für die in Azure AD eingebundenen Hybridgeräte in der Partition für den Konfigurationsnamenskontext der Computergesamtstruktur vorhanden sein. Es gibt nur einen Konfigurationsnamenskontext pro Gesamtstruktur. In einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen muss der Dienstverbindungspunkt in allen Gesamtstrukturen vorhanden sein, die in die Domäne eingebundene Computer enthalten.

Sie können das [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx)-Cmdlet verwenden, um den Konfigurationsnamenskontext aus Ihrer Gesamtstruktur abzurufen.  

Für eine Gesamtstruktur mit dem Active Directory-Domänennamen *fabrikam.com* lautet der Konfigurationsnamenskontext:

`CN=Configuration,DC=fabrikam,DC=com`

In Ihrer Gesamtstruktur befindet sich das SCP-Objekt für die automatische Registrierung von in die Domäne eingebundenen Geräten unter:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Je nachdem, wie Sie Azure AD Connect bereitgestellt haben, wurde das SCP-Objekt unter Umständen bereits konfiguriert.
Mit dem folgenden Windows PowerShell-Skript können Sie das Vorhandensein des Objekts überprüfen und die Ermittlungswerte abrufen: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

Die Ausgabe von **$scp.Keywords** enthält die Azure AD-Mandanteninformationen, z.B.:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Wenn der Dienstverbindungsendpunkt nicht vorhanden ist, können Sie ihn erstellen, indem Sie das `Initialize-ADSyncDomainJoinedComputerSync`-Cmdlet auf Ihrem Azure AD Connect-Server ausführen. Für die Ausführung dieses Cmdlets sind die Anmeldeinformationen eines Unternehmensadministrators erforderlich.  
Für das Cmdlet gilt Folgendes:

- Erstellt den Dienstverbindungspunkt in der Active Directory-Gesamtstruktur, mit der Azure AD Connect verbunden ist. 
- Erfordert, dass Sie den Parameter `AdConnectorAccount` angeben. Dies ist das Konto, das in Azure AD Connect als Active Directory-Connectorkonto konfiguriert ist. 


Mit dem folgenden Skript wird ein Beispiel für die Verwendung des Cmdlets veranschaulicht. In diesem Skript ist für `$aadAdminCred = Get-Credential` die Eingabe eines Benutzernamens erforderlich. Sie müssen den Benutzernamen im UPN-Format (Benutzerprinzipalname) eingeben: `user@example.com`. 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

Für das `Initialize-ADSyncDomainJoinedComputerSync`-Cmdlet gilt Folgendes:

- Es verwendet das Active Directory-PowerShell-Modul, das von der Ausführung von Active Directory-Webdiensten auf einem Domänencontroller abhängig ist. Active Directory-Webdienste werden auf Domänencontrollern mit Windows Server 2008 R2 und höher unterstützt.
- Es wird nur von der **MSOnline PowerShell-Modulversion 1.1.166.0** unterstützt. Dieses Modul können Sie [hier](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185) herunterladen.   

Verwenden Sie für Domänencontroller mit Windows Server 2008 oder früher das unten angegebene Skript zum Erstellen des Dienstverbindungsendpunkts.

Bei einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen sollten Sie das folgende Skript verwenden, um den Dienstverbindungsendpunkt in allen Gesamtstrukturen zu erstellen, in denen Computer vorhanden sind:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a>Schritt 2: Einrichten der Ausstellung von Ansprüchen

Bei einer Azure AD-Verbundkonfiguration wird für Geräte AD FS (Active Directory-Verbunddienste) oder ein lokaler Drittanbieter-Verbunddienst genutzt, um die Authentifizierung bei Azure AD durchzuführen. Die Geräte führen die Authentifizierung durch, um ein Zugriffstoken für die Registrierung beim Geräteregistrierungsdienst von Azure Active Directory (Azure DRS) zu erhalten.

Für aktuelle Windows-Geräte wird die Authentifizierung per integrierter Windows-Authentifizierung gegenüber einem aktiven WS-Trust-Endpunkt (entweder Version 1.3 oder 2005) durchgeführt, der vom lokalen Verbunddienst gehostet wird.

> [!NOTE]
> Bei Verwendung von AD FS muss entweder **adfs/services/trust/13/windowstransport** oder **adfs/services/trust/2005/windowstransport** aktiviert sein. Wenn Sie einen Webauthentifizierungsproxy verwenden, stellen Sie außerdem sicher, dass dieser Endpunkt durch den Proxy veröffentlicht wird. Sie können in der AD FS-Verwaltungskonsole unter **Dienst > Endpunkte** sehen, welche Endpunkte aktiviert sind.
>
>Wenn Sie AD FS nicht als lokalen Verbunddienst nutzen, können Sie der Anleitung Ihres jeweiligen Anbieters entnehmen, ob WS-Trust 1.3- oder 2005-Endpunkte unterstützt und per MEX-Datei (Metadata Exchange) veröffentlicht werden.

Die folgenden Ansprüche müssen im Token vorhanden sein, das von Azure DRS empfangen wird, damit die Geräteregistrierung abgeschlossen werden kann. Von Azure DRS wird ein Geräteobjekt in Azure AD mit einigen dieser Informationen erstellt, die dann von Azure AD Connect verwendet werden, um das neu erstellte Geräteobjekt dem lokalen Computerkonto zuzuordnen.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Wenn Sie über mehr als einen verifizierten Domänennamen verfügen, müssen Sie für Computer den folgenden Anspruch angeben:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Falls Sie bereits einen ImmutableID-Anspruch ausstellen (z.B. alternative Anmelde-ID), müssen Sie für Computer einen entsprechenden Anspruch angeben:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Die folgenden Abschnitte enthalten Informationen zu diesen Punkten:
 
- Werte, über die jeder Anspruch verfügen sollte
- Aussehen einer Definition in AD FS

Mithilfe der Definition können Sie überprüfen, ob die Werte vorhanden sind oder ob Sie sie erstellen müssen.

> [!NOTE]
> Wenn Sie als lokalen Verbundserver nicht AD FS nutzen, sollten Sie die Anleitung Ihres Anbieters befolgen, um die entsprechende Konfiguration zum Ausgeben dieser Ansprüche zu erstellen.

### <a name="issue-account-type-claim"></a>Ausstellen des Kontotypanspruchs

**`http://schemas.microsoft.com/ws/2012/01/accounttype`**: Dieser Anspruch muss den Wert **DJ** enthalten, mit dem das Gerät als in die Domäne eingebundener Computer identifiziert wird. In AD FS können Sie eine Ausstellungstransformationsregel hinzufügen, die wie folgt aussieht:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Ausstellen der objectGUID des lokalen Computerkontos

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**: Dieser Anspruch muss den **objectGUID**-Wert des lokalen Computerkontos enthalten. In AD FS können Sie eine Ausstellungstransformationsregel hinzufügen, die wie folgt aussieht:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Ausstellen der objectSID des lokalen Computerkontos

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**: Dieser Anspruch muss den **objectSid**-Wert des lokalen Computerkontos enthalten. In AD FS können Sie eine Ausstellungstransformationsregel hinzufügen, die wie folgt aussieht:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Ausstellen der issuerID für Computer, wenn in Azure AD mehrere verifizierte Domänennamen enthalten sind

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**: Dieser Anspruch muss den URI (Uniform Resource Identifier) von einem der verifizierten Domänennamen enthalten, für die eine Verbindung mit dem lokalen Verbunddienst (AD FS oder Drittanbieter) hergestellt wird, von dem das Token ausgestellt wird. In AD FS können Sie Ausstellungstransformationsregeln hinzufügen, die wie die unten angegebenen Beispiele aussehen (in dieser spezifischen Reihenfolge nach den obigen Regeln). Beachten Sie, dass eine Regel erforderlich ist, um die Regel explizit für Benutzer auszustellen. In den unten angegebenen Regeln wird eine erste Regel hinzugefügt, mit der die Benutzer-/Computerauthentifizierung identifiziert wird.

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


Im Anspruch oben

- ist `$<domain>` die AD FS-Dienst-URL
- und `<verified-domain-name>` ein Platzhalter, den Sie durch einen Ihrer überprüften Domänennamen in Azure AD ersetzen müssen.



Weitere Informationen zu überprüften Domänennamen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-add-domain.md).  
Zum Abrufen einer Liste mit Ihren überprüften Unternehmensdomänen können Sie das [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0)-Cmdlet verwenden. 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Ausstellen der ImmutableID für Computer, wenn ein Wert für Benutzer vorhanden ist (z.B. Angabe der alternativen Anmelde-ID)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**: Dieser Anspruch muss einen gültigen Wert für Computer enthalten. In AD FS können Sie wie folgt eine Ausstellungstransformationsregel erstellen:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Hilfsskript zum Erstellen der AD FS-Ausstellungstransformationsregeln

Das folgende Skript dient Ihnen als Hilfe beim Erstellen der oben beschriebenen Ausstellungstransformationsregeln.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Hinweise 

- Mit diesem Skript werden die Regeln an die bereits vorhandenen Regeln angefügt. Führen Sie das Skript nicht zweimal aus, weil der Regelsatz dann doppelt hinzugefügt wird. Stellen Sie sicher, dass keine entsprechenden Regeln für diese Ansprüche vorhanden sind (unter den jeweiligen Bedingungen), bevor Sie das Skript erneut ausführen.

- Wenn Sie über mehrere verifizierte Domänennamen verfügen (wie im Azure AD-Portal oder per Get-MsolDomains-Cmdlet angegeben), legen Sie den Wert von **$multipleVerifiedDomainNames** im Skript auf **$true** fest. Stellen Sie außerdem sicher, dass Sie alle vorhandenen issuerid-Ansprüche entfernen, die unter Umständen von Azure AD Connect oder auf anderem Wege erstellt wurden. Hier ist ein Beispiel für diese Regel angegeben:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Wenn Sie bereits einen **ImmutableID**-Anspruch für Benutzerkonten ausgestellt haben, legen Sie den Wert von **$immutableIDAlreadyIssuedforUsers** im Skript auf **$true** fest.

## <a name="step-3-enable-windows-down-level-devices"></a>Schritt 3: Aktivieren von kompatiblen Windows-Geräten

Wenn es sich bei einigen Ihrer in die Domäne eingebundenen Geräte um kompatible Windows-Geräte handelt, gehen Sie wie folgt vor:

- Legen Sie eine Richtlinie in Azure AD fest, um Benutzern das Registrieren von Geräten zu ermöglichen.
 
- Konfigurieren Sie Ihren lokalen Verbunddienst für das Ausstellen von Ansprüchen, um die **Integrierte Windows-Authentifizierung (IWA)** für die Geräteregistrierung zu unterstützen.
 
- Fügen Sie den Endpunkt für die Azure AD-Geräteauthentifizierung den lokalen Intranetzonen hinzu, um beim Authentifizieren des Geräts Zertifikataufforderungen zu vermeiden.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Festlegen einer Richtlinie in Azure AD, um Benutzern das Registrieren von Geräten zu ermöglichen

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie sicherstellen, dass die Einstellung festgelegt ist, mit der Benutzer Geräte in Azure AD registrieren können. Im Azure-Portal finden Sie diese Einstellung unter:

`Azure Active Directory > Users and groups > Device settings`
    
Die folgende Richtlinie muss auf **Alle** festgelegt sein: **Benutzer dürfen ihre Geräte für Azure AD registrieren**

![Registrieren von Geräten](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a>Konfigurieren eines lokalen Verbunddiensts 

Ihr lokaler Verbunddienst muss das Ausstellen der Ansprüche **authenticationmehod** und **wiaormultiauthn** unterstützen, wenn eine Authentifizierungsanforderung an die vertrauende Seite von Azure AD empfangen wird, die wie unten gezeigt den Parameter „resource_params“ mit einem codierten Wert enthält:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Wenn eine Anforderung dieser Art eingeht, muss der lokale Verbunddienst den Benutzer per integrierter Windows-Authentifizierung authentifizieren und die folgenden beiden Ansprüche ausstellen, sofern der Vorgang erfolgreich ist:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

In AD FS müssen Sie eine Ausstellungstransformationsregel hinzufügen, die die Authentifizierungsmethode weitergibt.  

**Gehen Sie wie folgt vor, um diese Regel hinzuzufügen:**

1. Navigieren Sie in der AD FS-Verwaltungskonsole zu `AD FS > Trust Relationships > Relying Party Trusts`.
2. Klicken Sie mit der rechten Maustaste auf das Vertrauensstellungsobjekt der vertrauenden Seite, „Microsoft Office 365 Identity Platform“, und wählen Sie dann **Anspruchsregeln bearbeiten** aus.
3. Wählen Sie auf der Registerkarte **Ausstellungstransformationsregeln** die Option **Regel hinzufügen** aus.
4. Wählen Sie in der Vorlagenliste **Anspruchsregel** die Option **Ansprüche mit benutzerdefinierter Regel senden** aus.
5. Wählen Sie **Weiter**.
6. Geben Sie in das Feld **Anspruchsregelname** den Text **Anspruchsregel für Authentifizierungsmethode** ein.
7. Geben Sie im Feld **Anspruchsregel** die folgende Regel ein:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Geben Sie auf Ihrem Verbundserver den hier dargestellten PowerShell-Befehl ein, nachdem Sie **\<RPObjectName\>** durch den Objektnamen der vertrauenden Seite für Ihr Azure AD-Vertrauensstellungsobjekt der vertrauenden Seite ersetzt haben. Dieses Objekt trägt normalerweise den Namen **Microsoft Office 365 Identity Platform**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Hinzufügen des Endpunkts für die Azure AD-Geräteauthentifizierung zu den lokalen Intranetzonen

Um die Anzeige von Zertifikataufforderungen zu vermeiden, wenn Benutzer beim Registrieren von Geräten die Authentifizierung für Azure AD durchführen, können Sie eine Richtlinie auf Ihre in die Domäne eingebundenen Geräte übertragen, um die folgende URL in Internet Explorer den lokalen Intranetzonen hinzuzufügen:

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a>Schritt 4: Steuern der Bereitstellung und des Rollouts

Nachdem Sie die erforderlichen Schritte ausgeführt haben, können in die Domäne eingebundene Geräte automatisch in Azure AD eingebunden werden:

- Für alle in die Domäne eingebundenen Geräte, auf denen Windows 10 Anniversary Update und Windows Server 2016 ausgeführt wird, wird bei einem Neustart des Geräts oder der Anmeldung eines Benutzers eine automatische Registrierung bei Azure AD durchgeführt. 

- Neue Geräte werden bei Azure AD registriert, wenn das Gerät nach Abschluss des Domänenbeitritts neu gestartet wird.

- Geräte, die zuvor bei Azure AD registriert wurden (etwa für Intune), werden zu Geräten mit *Domäneneinbindung und AAD-Registrierung*. Aufgrund des normalen Ablaufs von Domänen- und Benutzeraktivitäten dauert es jedoch etwas, bis dieser Prozess für alle Geräte abgeschlossen ist.

### <a name="remarks"></a>Anmerkungen

- Sie können ein Gruppenrichtlinienobjekt verwenden, um den Rollout der automatischen Registrierung von in die Domäne eingebundenen Computern unter Windows 10/Windows Server 2016 zu steuern.

- Windows 10 November 2015 Update wird **nur** automatisch in Azure AD eingebunden, wenn das Rollout-Gruppenrichtlinienobjekt festgelegt ist.

- Für den Rollout von kompatiblen Windows-Computern ist ein [Windows Installer-Paket](#windows-installer-packages-for-non-windows-10-computers) verfügbar, das auf den von Ihnen ausgewählten Computern bereitgestellt werden kann.

- Wenn Sie das Gruppenrichtlinienobjekt auf in die Domäne eingebundene Computer mit Windows 8.1 übertragen, wird versucht, eine Einbindung durchzuführen. Es wird aber empfohlen, das [Windows Installer-Paket](#windows-installer-packages-for-non-windows-10-computers) zu verwenden, um alle kompatiblen Windows-Geräte einzubinden. 

### <a name="create-a-group-policy-object"></a>Erstellen eines Gruppenrichtlinienobjekts 

Um den Rollout von aktuellen Windows-Computern zu steuern, können Sie die Gruppenrichtlinie **In die Domäne eingebundene Computer als Geräte registrieren** für die zu registrierenden Geräte bereitstellen. Beispielsweise können Sie die Richtlinie für eine Organisationseinheit oder eine Sicherheitsgruppe bereitstellen.

**Legen Sie die Richtlinie fest:**

1. Öffnen Sie den **Server-Manager**, und navigieren Sie zu `Tools > Group Policy Management`.
2. Wechseln Sie zu dem Domänenknoten, der der Domäne entspricht, in der Sie die automatische Registrierung von aktuellen Windows-Computern aktivieren möchten.
3. Klicken Sie mit der rechten Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie dann **Neu** aus.
4. Geben Sie einen Namen für das Gruppenrichtlinienobjekt ein. Beispiel: „*Hybrid Azure AD Join“. 
5. Klicken Sie auf **OK**.
6. Klicken Sie mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann **Bearbeiten** aus.
7. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Geräteregistrierung**. 
8. Klicken Sie mit der rechten Maustaste auf **In die Domäne eingebundene Computer als Geräte registrieren**, und wählen Sie dann die Option **Bearbeiten**.
   
   > [!NOTE]
   > In früheren Versionen der Gruppenrichtlinien-Verwaltungskonsole hatte diese Gruppenrichtlinienvorlage einen anderen Namen. Navigieren Sie zu `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`, falls Sie eine frühere Version der Konsole verwenden. 

7. Wählen Sie **Aktiviert** aus, und klicken Sie dann auf **Übernehmen**.
8. Klicken Sie auf **OK**.
9. Verknüpfen Sie das Gruppenrichtlinienobjekt jetzt mit einem Speicherort Ihrer Wahl. Sie können es beispielsweise mit einer bestimmten Organisationseinheit verknüpfen. Sie können es aber auch mit einer bestimmten Sicherheitsgruppe von Computern verbinden, die automatisch in Azure AD eingebunden werden. Um diese Richtlinie für alle in die Domäne eingebundenen Windows 10- und Windows Server 2016-Computer in Ihrer Organisation festzulegen, verknüpfen Sie das Gruppenrichtlinienobjekt mit der Domäne.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Windows Installer-Pakete für Computer ohne Windows 10

Zum Einbinden von in die Domäne eingebundenen kompatiblen Windows-Computern in einer Verbundumgebung können Sie dieses Windows Installer-Paket (.msi) über die Seite [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) (Microsoft Workplace Join für Computer mit einem anderen Betriebssystem als Windows 10) aus dem Download Center herunterladen und installieren.

Sie können das Paket mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager bereitstellen. Das Paket unterstützt die Standardoptionen für die Installation im Hintergrund unter Verwendung des Parameters *quiet*. System Center Configuration Manager Current Branch bietet zusätzliche Vorteile gegenüber früheren Versionen, z.B. die Möglichkeit zur Nachverfolgung abgeschlossener Registrierungen. Weitere Informationen finden Sie unter [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

Das Installationsprogramm erstellt einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt wird. Der Task wird ausgelöst, wenn sich der Benutzer bei Windows anmeldet. Nach der Authentifizierung per integrierter Windows-Authentifizierung bindet der Task das Gerät unter Verwendung der Anmeldeinformationen des Benutzers automatisch in Azure AD ein. Um den geplanten Task anzuzeigen, wechseln Sie auf dem Gerät zu **Microsoft** > **Arbeitsbereichverknüpfung** und navigieren dann zur Aufgabenplanungsbibliothek.

## <a name="step-5-verify-joined-devices"></a>Schritt 5: Überprüfen der eingebundenen Geräte

Sie können die erfolgreiche Einbindung für die Geräte Ihrer Organisation überprüfen, indem Sie das Cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) im [Azure Active Directory PowerShell-Modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0) verwenden.

In der Ausgabe dieses Cmdlets werden Geräte angezeigt, die in Azure AD registriert und eingebunden sind. Verwenden Sie zum Abrufen aller Geräte den Parameter **-All**, und filtern Sie sie anschließend mit der **deviceTrustType**-Eigenschaft. In die Domäne eingebundene Geräte weisen den Wert **In die Domäne eingebunden** auf.

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png

