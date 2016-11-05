---
title: Erste Schritte mit der zertifikatbasierten Authentifizierung auf iOS | Microsoft Docs
description: Erfahren Sie, wie Sie die zertifikatbasierte Authentifizierung in Lösungen mit iOS-Geräten konfigurieren.
services: active-directory
author: markusvi
documentationcenter: na
manager: femila

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2016
ms.author: markvi

---
# Erste Schritte mit der zertifikatbasierten Authentifizierung auf iOS – öffentliche Vorschau
> [!div class="op_single_selector"]
> * [iOS](active-directory-certificate-based-authentication-ios.md)
> * [Android](active-directory-certificate-based-authentication-android.md)
> 
> 

In diesem Thema erfahren Sie, wie Sie die zertifikatbasierte Authentifizierung auf einem iOS-Gerät für Benutzer von Mandanten in folgenden Tarifen konfigurieren und nutzen: Office 365 Enterprise, Business und Education.

Bei Verwendung der zertifikatbasierten Authentifizierung können Sie von Azure Active Directory mit einem Clientzertifikat auf einem Android- oder iOS-Gerät authentifiziert werden, wenn Sie Ihr Exchange Online-Konto mit folgenden Anwendungen oder Clients verbinden:

* Mobile Office-Anwendungen wie Microsoft Outlook und Microsoft Word
* Exchange ActiveSync-Clients (EAS)

Wenn Sie dieses Feature konfigurieren, ist es bei bestimmten E-Mail- und Microsoft Office-Anwendungen auf Ihrem mobilen Gerät nicht länger erforderlich, einen Benutzernamen und ein Kennwort einzugeben.

## Unterstützte Szenarien und Voraussetzungen
### Allgemeine Anforderungen
Die folgenden Voraussetzungen müssen für alle Szenarien in diesem Thema erfüllt sein:

* Zugriff auf Zertifizierungsstellen, um Clientzertifikate auszustellen.
* Die Zertifizierungsstelle(n) muss/müssen in Azure Active Directory konfiguriert sein. Ausführliche Schrittanleitungen zum Durchführen der Konfiguration finden Sie im Abschnitt [Erste Schritte](#getting-started).
* Die Stammzertifizierungsstelle und alle Zwischenzertifizierungsstellen müssen in Azure Active Directory konfiguriert sein.
* Jede Zertifizierungsstelle muss über eine Zertifikatsperrliste verfügen, auf die über eine Internet-URL verwiesen werden kann.
* Für die Clientauthentifizierung muss das Clientzertifikat ausgestellt werden.
* Nur bei Exchange ActiveSync-Clients muss das Clientzertifikat über die routingfähige E-Mail-Adresse des Benutzers in Exchange Online verfügen (entweder als Prinzipalname oder als Wert des RFC822-Namens im Feld „Alternativer Antragstellername“). Azure Active Directory ordnet den RFC822-Wert dem Attribut für die Proxyadresse innerhalb des Verzeichnisses zu.

### Unterstützung mobiler Office-Anwendungen
| Apps | Support |
| --- | --- |
| Word/Excel/PowerPoint |![Prüfen][1] |
| OneNote |![Prüfen][1] |
| OneDrive |![Prüfen][1] |
| Outlook |In Kürze verfügbar |
| Yammer |![Prüfen][1] |
| Skype for Business |In Kürze verfügbar |

### Anforderungen
Die Betriebssystemversion des Geräts muss iOS 9 oder eine höhere Version sein.

Ein Verbundserver muss konfiguriert werden.

Für Office-Anwendungen auf iOS ist Azure Authenticator erforderlich.

Damit Azure Active Directory ein Clientzertifikat sperren kann, muss das AD FS-Token die folgenden Ansprüche enthalten:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (Die Seriennummer des Clientzertifikat)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (Die Zeichenfolge für den Aussteller des Clientzertifikats)

Wenn diese Ansprüche im AD FS-Token (oder in einem anderen SAML-Token) enthalten sind, fügt Azure Active Directory die Ansprüche dem Aktualisierungstoken hinzu. Wenn das Aktualisierungstoken überprüft werden muss, werden diese Informationen zum Überprüfen der Sperrung verwendet.

Als bewährte Methode sollten Sie die AD FS-Fehlerseiten mit folgenden Informationen aktualisieren:

* Die Voraussetzungen für die Installation von Azure Authenticator auf iOS
* Anleitungen zum Abrufen eines Benutzerzertifikats

Weitere Einzelheiten finden Sie unter [Anpassen der AD FS-Anmeldeseiten](https://technet.microsoft.com/library/dn280950.aspx).

### Unterstützung von Exchange ActiveSync-Clients
Unter iOS 9 oder höher wird der native iOS-E-Mail-Client unterstützt. Wenden Sie sich bei allen anderen Exchange ActiveSync-Anwendungen an den Anwendungsentwickler, um zu erfragen, ob dieses Feature unterstützt wird.

## Erste Schritte
Zunächst müssen die Zertifizierungsstellen in Azure Active Directory konfiguriert werden. Laden Sie für jede Zertifizierungsstelle die folgenden Informationen hoch:

* Den öffentlichen Teil des Zertifikats im Format *CER*
* Die Internet-URLs der Zertifikatsperrlisten

Nachfolgend ist das Schema für die Zertifizierungsstelle gezeigt:

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


Zum Hochladen der Informationen können Sie das Azure AD-Modul über Windows PowerShell verwenden. Nachfolgend finden Sie Beispiele für das Hinzufügen, Entfernen oder Ändern einer Zertifizierungsstelle.

### Konfigurieren Ihres Azure AD-Mandanten für die zertifikatbasierte Authentifizierung
1. Starten Sie Windows PowerShell mit Administratorrechten.
2. Installieren Sie das Azure AD-Modul. Sie müssen die Version [1\.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) oder eine höhere Version installieren.
   
        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 
3. Verbinden Sie sich mit Ihrem Zielmandanten:
   
        Connect-AzureAD 

### Hinzufügen einer neuen Zertifizierungsstelle
1. Legen Sie verschiedene Eigenschaften der Zertifizierungsstelle fest, und fügen Sie sie zu Azure Active Directory hinzu:
   
        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 
2. Rufen Sie die Zertifizierungsstellen ab:
   
        Get-AzureADTrustedCertificateAuthority 

### Abrufen der Liste mit Zertifizierungsstellen
Rufen Sie die Zertifizierungsstellen ab, die aktuell in Azure Active Directory für Ihren Mandanten gespeichert sind:

        Get-AzureADTrustedCertificateAuthority 


### Entfernen einer Zertifizierungsstelle
1. Rufen Sie die Zertifizierungsstellen ab:
   
     $c=Get-AzureADTrustedCertificateAuthority 
2. Entfernen Sie das Zertifikat für die Zertifizierungsstelle:
   
        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 

### Ändern einer Zertifizierungsstelle
1. Rufen Sie die Zertifizierungsstellen ab:
   
     $c=Get-AzureADTrustedCertificateAuthority 
2. Ändern Sie die Eigenschaften der Zertifizierungsstelle:
   
        $c[0].AuthorityType=1 
3. Legen Sie die **Zertifizierungsstelle** fest:
   
        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 

## Testen mobiler Office-Anwendungen
So testen Sie die Zertifikatauthentifizierung für Ihre mobile Office-Anwendung:

1. Installieren Sie auf Ihrem Testgerät eine mobile Office-Anwendung (z.B. OneDrive) aus dem App Store.
2. Überprüfen Sie, ob das Benutzerzertifikat auf Ihrem Testgerät bereitgestellt wurde.
3. Starten Sie die Anwendung.
4. Geben Sie Ihren Benutzernamen ein, und wählen Sie das Benutzerzertifikat aus, das verwendet werden soll.

Sie sollten erfolgreich angemeldet werden.

## Testen von Exchange ActiveSync-Clientanwendungen
Um über die zertifikatbasierte Authentifizierung auf Exchange ActiveSync zuzugreifen, muss ein EAS-Profil mit dem Clientzertifikat für die Anwendung verfügbar sein. Das EAS-Profil muss folgende Informationen enthalten:

* Das Benutzerzertifikat, das für die Authentifizierung verwendet werden soll.
* Der EAS-Endpunkt muss „outlook.office365.com“ sein, da dieses Feature aktuell nur in der mehrinstanzenfähigen Exchange Online-Umgebung unterstützt wird.

Sie können ein EAS-Profil über eine MDM-Lösung wie Intune konfigurieren und auf dem Gerät platzieren oder das Zertifikat manuell im EAS-Profil auf dem Gerät platzieren.

### Testen von EAS-Clientanwendungen auf iOS
So testen Sie die Zertifikatauthentifizierung mit der nativen E-Mail-Anwendung auf iOS 9 oder einer höheren Version:

1. Konfigurieren Sie ein EAS-Profil, das die oben beschriebenen Anforderungen erfüllt.
2. Installieren Sie das Profil auf dem iOS-Gerät (unter Verwendung einer MDM-Lösung wie Intune oder über die Apple Configurator-Anwendung).
3. Öffnen Sie nach der ordnungsgemäßen Installation des Profils die native E-Mail-Anwendung, und überprüfen Sie, ob E-Mails synchronisiert werden.

## Widerruf
Zum Sperren eines Clientzertifikats ruft Azure Active Directory die Zertifikatsperrliste von den URLs ab, die als Teil der Informationen zur Zertifizierungsstelle hochgeladen wurden, und platziert diese Liste im Zwischenspeicher. Anhand des Zeitstempels der letzten Veröffentlichung (Eigenschaft **Effective Date**) in der Zertifikatsperrliste wird sichergestellt, dass die Liste noch gültig ist. Der Verweis auf die Zertifikatsperrliste erfolgt regelmäßig, um den Zugriff auf Zertifikate zu sperren, die in dieser Liste enthalten sind.

Wenn eine schnellere Sperrung erforderlich ist (beispielsweise beim Verlust eines Geräts), kann der Autorisierungstoken des Benutzers für ungültig erklärt werden. Um das Autorisierungstoken für ungültig zu erklären, legen Sie das Feld **StsRefreshTokenValidFrom** für diesen Benutzer über Windows PowerShell fest. Das Feld **StsRefreshTokenValidFrom** muss für jeden Benutzer aktualisiert werden, für den der Zugriff gesperrt werden soll.

Um sicherzustellen, dass die Sperrung aufrechterhalten wird, muss die Eigenschaft **Effective Date** der Zertifikatsperrliste auf ein Datum festgelegt werden, das nach dem Wert von **StsRefreshTokenValidFrom** liegt. Stellen Sie außerdem sicher, dass das betroffene Zertifikat in der Zertifikatsperrliste enthalten ist.

Die unten aufgeführten Schritte zeigen, wie Sie das Autorisierungstoken aktualisieren und für ungültig erklären, indem Sie das Feld **StsRefreshTokenValidFrom** festlegen.

1. Verbinden Sie sich mit Administratoranmeldeinformationen mit dem MSOL-Dienst:
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 
2. Rufen Sie den aktuellen StsRefreshTokensValidFrom-Wert für einen Benutzer ab:
   
     $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
     $user.StsRefreshTokensValidFrom 
3. Legen Sie einen neuen StsRefreshTokensValidFrom-Wert für den Benutzer fest, der mit dem aktuellen Zeitstempel übereinstimmt:
   
     Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Das festgelegte Datum muss in der Zukunft liegen. Wenn das Datum nicht in der Zukunft liegt, wird die **StsRefreshTokensValidFrom**-Eigenschaft nicht festgelegt. Wenn das Datum in der Zukunft liegt, wird **StsRefreshTokensValidFrom** auf die aktuelle Uhrzeit festgelegt (nicht das Datum, das mit dem Befehl „Set-MsolUser“ angegeben ist).

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png

<!---HONumber=AcomDC_0803_2016-->