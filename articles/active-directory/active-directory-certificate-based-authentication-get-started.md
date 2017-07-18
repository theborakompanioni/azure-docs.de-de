---
title: "Zertifikatbasierte Authentifizierung mit Azure Active Directory – erste Schritte | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie die zertifikatbasierte Authentifizierung in Ihrer Umgebung konfigurieren.
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: c32f2ca2c799332652d38d882a4d6337bade4f93
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017

---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Erste Schritte mit zertifikatbasierter Authentifizierung in Azure Active Directory

Bei Verwendung der zertifikatbasierten Authentifizierung können Sie von Azure Active Directory mit einem Clientzertifikat auf einem Windows-, Android- oder iOS-Gerät authentifiziert werden, wenn Sie Ihr Exchange Online-Konto mit folgenden Anwendungen oder Clients verbinden: 

- Mobile Office-Anwendungen wie Microsoft Outlook und Microsoft Word   

- Exchange ActiveSync-Clients (EAS) 

Wenn Sie dieses Feature konfigurieren, ist es bei bestimmten E-Mail- und Microsoft Office-Anwendungen auf Ihrem mobilen Gerät nicht länger erforderlich, einen Benutzernamen und ein Kennwort einzugeben. 

Dieses Thema:

- Informiert Sie über die Schritte zum Konfigurieren und Nutzen zertifikatbasierter Authentifizierung für Benutzer von Mandanten in Office 365 Enterprise-, Business-, Education- und US Government-Plänen. Dieses Feature ist als Vorversion in Office 365 China-, US Government Defense- und US Government Federal-Plänen verfügbar. 

- Setzt voraus, dass Sie bereits über eine [Public Key-Infrastruktur (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) verfügen und [AD FS](connect/active-directory-aadconnectfed-whatis.md) konfiguriert haben.    


## <a name="requirements"></a>Anforderungen

Damit Sie eine zertifikatbasierte Authentifizierung konfigurieren können, muss Folgendes zutreffen:  

- Die Stammzertifizierungsstelle und alle Zwischenzertifizierungsstellen müssen in Azure Active Directory konfiguriert sein.  

- Jede Zertifizierungsstelle muss über eine Zertifikatsperrliste verfügen, auf die über eine Internet-URL verwiesen werden kann.  

- Sie müssen mindestens eine Zertifizierungsstelle in Azure Active Directory konfiguriert haben. Die entsprechenden Schritte finden Sie im Abschnitt [Konfigurieren Sie die Zertifizierungsstellen](#step-2-configure-the-certificate-authorities).  

- Bei Exchange ActiveSync-Clients muss das Clientzertifikat über die routingfähige E-Mail-Adresse des Benutzers in Exchange Online verfügen (entweder als Prinzipalname oder als Wert des RFC822-Namens im Feld „Alternativer Antragstellername“). Azure Active Directory ordnet den RFC822-Wert dem Attribut für die Proxyadresse innerhalb des Verzeichnisses zu.  

- Ihr Clientgerät benötigt Zugriff auf mindestens eine Zertifizierungsstelle, die Clientzertifikate ausstellt.  

- Ein Clientzertifikat für die Clientauthentifizierung muss für Ihren Client ausgestellt sein.  




## <a name="step-1-select-your-device-platform"></a>Schritt 1: Wählen Sie Ihre Geräteplattform

Als ersten Schritt müssen Sie für Ihre Geräteplattform Folgendes überprüfen:

- Die Unterstützung mobiler Office-Anwendungen 
- Die spezifischen Implementierungsanforderungen  

Die entsprechenden Informationen sind für die folgenden Geräteplattformen vorhanden:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-the-certificate-authorities"></a>Schritt 2: Konfigurieren Sie die Zertifizierungsstellen 

Laden Sie zum Konfigurieren Ihrer Zertifizierungsstellen in Azure Active Directory für jede Zertifizierungsstelle Folgendes hoch: 

* Den öffentlichen Teil des Zertifikats im Format *CER* 
* Die Internet-URLs der Zertifikatsperrlisten

So sieht das Schema für eine Zertifizierungsstelle aus: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 

Für die Konfiguration können Sie [Azure Active Directory PowerShell, Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) verwenden:  

1. Starten Sie Windows PowerShell mit Administratorrechten. 
2. Installieren Sie das Azure AD-Modul. Sie müssen die Version [2.0.0.33](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) oder eine höhere Version installieren.  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

Im ersten Konfigurationsschritt müssen Sie eine Verbindung mit Ihrem Mandanten herstellen. Sobald eine Verbindung mit Ihrem Mandanten vorhanden ist, können Sie die in Ihrem Verzeichnis definierten vertrauenswürdigen Zertifizierungsstellen überprüfen, hinzufügen, löschen und ändern. 

### <a name="connect"></a>Verbinden

Stellen Sie eine Verbindung mit Ihrem Mandanten mit dem [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0)-Cmdlet her:

    Connect-AzureAD 


### <a name="retrieve"></a>Gerätehandle 

Rufen Sie die in Ihrem Verzeichnis definierten vertrauenswürdigen Zertifizierungsstellen mit dem [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0)-Cmdlet ab. 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a>Hinzufügen

Um eine vertrauenswürdige Zertifizierungsstelle mit dem Cmdlet [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) zu erstellen, legen Sie das Attribut **crlDistributionPoint** auf den richtigen Wert fest: 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a>Remove (Entfernen)

Entfernen Sie eine vertrauenswürdige Zertifizierungsstelle mit dem [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0)-Cmdlet:
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a>Ändern

Ändern Sie eine vertrauenswürdige Zertifizierungsstelle mit dem [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0)-Cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a>Schritt 3: Konfigurieren der Sperrung

Zum Sperren eines Clientzertifikats ruft Azure Active Directory die Zertifikatsperrliste von den URLs ab, die als Teil der Informationen zur Zertifizierungsstelle hochgeladen wurden, und platziert diese Liste im Zwischenspeicher. Anhand des Zeitstempels der letzten Veröffentlichung (Eigenschaft**Effective Date** ) in der Zertifikatsperrliste wird sichergestellt, dass die Liste noch gültig ist. Der Verweis auf die Zertifikatsperrliste erfolgt regelmäßig, um den Zugriff auf Zertifikate zu sperren, die in dieser Liste enthalten sind.

Wenn eine schnellere Sperrung erforderlich ist (beispielsweise beim Verlust eines Geräts), kann der Autorisierungstoken des Benutzers für ungültig erklärt werden. Um das Autorisierungstoken für ungültig zu erklären, legen Sie das Feld **StsRefreshTokenValidFrom** für diesen Benutzer über Windows PowerShell fest. Das Feld **StsRefreshTokenValidFrom** muss für jeden Benutzer aktualisiert werden, für den der Zugriff gesperrt werden soll.

Um sicherzustellen, dass die Sperrung aufrechterhalten wird, muss die Eigenschaft **Effective Date** der Zertifikatsperrliste auf ein Datum festgelegt werden, das nach dem Wert von **StsRefreshTokenValidFrom** liegt. Stellen Sie außerdem sicher, dass das betroffene Zertifikat in der Zertifikatsperrliste enthalten ist.

Die unten aufgeführten Schritte zeigen, wie Sie das Autorisierungstoken aktualisieren und für ungültig erklären, indem Sie das Feld **StsRefreshTokenValidFrom** festlegen. 

**So konfigurieren Sie die Sperrung:** 

1. Verbinden Sie sich mit Administratoranmeldeinformationen mit dem MSOL-Dienst: 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. Rufen Sie den aktuellen StsRefreshTokensValidFrom-Wert für einen Benutzer ab: 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. Legen Sie einen neuen StsRefreshTokensValidFrom-Wert für den Benutzer fest, der mit dem aktuellen Zeitstempel übereinstimmt: 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Das festgelegte Datum muss in der Zukunft liegen. Wenn das Datum nicht in der Zukunft liegt, wird die **StsRefreshTokensValidFrom** -Eigenschaft nicht festgelegt. Wenn das Datum in der Zukunft liegt, wird **StsRefreshTokensValidFrom** auf die aktuelle Uhrzeit festgelegt (nicht das Datum, das mit dem Befehl „Set-MsolUser“ angegeben ist). 


## <a name="step-4-test-your-configuration"></a>Schritt 4: Testen Ihrer Konfiguration

### <a name="testing-your-certificate"></a>Testen Ihres Zertifikats

Als ersten Konfigurationstest sollten Sie versuchen, sich bei [OWA](https://outlook.office365.com) oder [SharePoint Online](https://microsoft.sharepoint.com) mithilfe Ihres **Gerätebrowsers** anzumelden.

Wenn die Anmeldung erfolgreich ist, wissen Sie:

- Das Benutzerzertifikat wurde auf Ihrem Testgerät bereitgestellt.
- AD FS ist richtig konfiguriert.  


### <a name="testing-office-mobile-applications"></a>Testen mobiler Office-Anwendungen

**So testen Sie die zertifikatbasierte Authentifizierung für Ihre mobile Office-Anwendung:** 

1. Installieren Sie auf Ihrem Testgerät eine mobile Office-Anwendung (z.B. OneDrive).
3. Starten Sie die Anwendung. 
4. Geben Sie Ihren Benutzernamen ein, und wählen Sie das Benutzerzertifikat aus, das verwendet werden soll. 

Sie sollten erfolgreich angemeldet werden. 

### <a name="testing-exchange-activesync-client-applications"></a>Testen von Exchange ActiveSync-Clientanwendungen

Um über die zertifikatbasierte Authentifizierung auf Exchange ActiveSync (EAS) zuzugreifen, muss ein EAS-Profil mit dem Clientzertifikat für die Anwendung verfügbar sein. 

Das EAS-Profil muss folgende Informationen enthalten:

- Das Benutzerzertifikat, das für die Authentifizierung verwendet werden soll. 

- Der EAS-Endpunkt (z.B. outlook.office365.com)

Sie können ein EAS-Profil über eine MDM-Lösung (Mobile Device Management, mobile Geräteverwaltung) wie Intune konfigurieren und auf dem Gerät platzieren oder das Zertifikat manuell im EAS-Profil auf dem Gerät platzieren.  

### <a name="testing-eas-client-applications-on-android"></a>Testen von EAS-Clientanwendungen auf Android

**So testen Sie die Zertifikatauthentifizierung:**  

1. Konfigurieren Sie ein EAS-Profil in der Anwendung, das die oben beschriebenen Anforderungen erfüllt.  
2. Öffnen Sie die Anwendung, und stellen Sie sicher, dass die E-Mail synchronisiert wird. 


