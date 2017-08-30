---
title: "Was ist die Self-Service-Registrierung für Azure? | Microsoft Docs"
description: "Eine Übersicht über die Self-Service-Registrierung für Azure und die Verwaltung des Registrierungsprozesses sowie darüber, wie man einen DNS-Domänennamen übernimmt."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 63818878e01dd52a988e10a6e4f624638ea1cfae
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---
# <a name="what-is-self-service-signup-for-azure"></a>Was ist die Self-Service-Registrierung für Azure?
Dieses Thema erläutert den Self-Service-Registrierungsvorgang und das Übernehmen eines DNS-Domänennamens.  

## <a name="why-use-self-service-signup"></a>Gründe für das Verwenden der Self-Service-Registrierung
* Kunden erhalten schneller die gewünschten Dienste.
* E-Mail-basierte Angebote für einen Dienst lassen sich rasch erstellen.
* E-Mail-basierte Registrierungsabläufe lassen sich schnell erstellen, um Benutzern das Erstellen von Identitäten mithilfe ihrer einfach zu merkenden geschäftlichen E-Mail-Aliase zu ermöglichen.
* Nicht verwaltete Azure-Verzeichnisse können später in verwaltete Verzeichnisse umgewandelt und für andere Dienste wiederverwendet werden.

## <a name="terms-and-definitions"></a>Begriffe und Definitionen
* **Self-Service-Registrierung**: Dies ist die Methode, mit der sich ein Benutzer für einen Clouddienst registriert, wobei für ihn basierend auf seiner E-Mail-Domäne automatisch eine Identität in Azure Active Directory (Azure AD) erstellt wird.
* **Nicht verwaltetes Azure-Verzeichnis**: Dies ist das Verzeichnis, in dem diese Identität erstellt wird. Ein nicht verwaltetes Verzeichnis ist ein Verzeichnis ohne globalen Administrator.
* **Über E-Mail verifizierter Benutzer**: Dies ist ein Typ von Benutzerkonto in Azure AD. Ein Benutzer, für den nach der Registrierung für ein Self-Service-Angebot automatisch eine Identität erstellt wird, wird als über E-Mail verifizierter Benutzer bezeichnet. Ein über E-Mail verifizierter Benutzer ist ein normales Mitglied eines Verzeichnisses mit der Kennzeichnung "creationmethod=EmailVerified".

## <a name="user-experience"></a>Benutzererfahrung
Angenommen, ein Benutzer, dessen E-Mail-Adresse Dan@BellowsCollege.com ist, empfängt vertrauliche Dateien per E-Mail. Die Dateien wurden durch Azure Rights Management (Azure RMS) geschützt. Doch die Organisation von Dan, das Bellows College, hat sich nicht für Azure RMS registriert und auch nicht Active Directory RMS bereitgestellt. In diesem Fall kann Dan sich für ein kostenloses Abonnement für RMS für Einzelpersonen registrieren, um die geschützten Dateien lesen zu können.

Wenn Dan der erste Benutzer mit einer E-Mail-Adresse von „BellowsCollege.com“ ist, der sich für dieses Self-Service-Angebot registriert, wird in Azure AD ein nicht verwaltetes Verzeichnis für „BellowsCollege.com“ erstellt. Wenn sich andere Benutzer aus der Domäne „BellowsCollege.com“ für dieses Angebot oder ein ähnliches Self-Service-Angebot registrieren, besitzen sie über E-Mail verifizierte Benutzerkonten, die im gleichen nicht verwalteten Verzeichnis in Azure erstellt wurden.

## <a name="admin-experience"></a>Administratorerfahrung
Ein Administrator, der Besitzer des DNS-Domänennamens eines nicht verwalteten Azure-Verzeichnisses ist, kann das Verzeichnis nach dem Nachweis des Besitzes übernehmen oder zusammenführen. In den nächsten Abschnitten wird die Administratorerfahrung im Detail erläutert. Hier zunächst eine Zusammenfassung:

* Wenn Sie ein nicht verwaltetes Azure-Verzeichnis übernehmen, werden Sie einfach zum globalen Administrator des nicht verwalteten Verzeichnisses. Dies wird mitunter als interne Übernahme bezeichnet.
* Wenn Sie ein nicht verwaltetes Azure-Verzeichnis zusammenführen, fügen Sie den DNS-Domänennamen des nicht verwalteten Verzeichnisses Ihrem verwalteten Azure-Verzeichnis hinzu. Eine Zuordnung von Benutzern zu Ressourcen wird erstellt, damit Benutzer weiterhin ohne Unterbrechung auf Dienste zugreifen können. Dies wird mitunter als externe Übernahme bezeichnet.

## <a name="what-gets-created-in-azure-active-directory"></a>Was wird in Azure Active Directory erstellt?
#### <a name="directory"></a>Verzeichnis
* Für die Domäne wird ein Azure Active Directory-Verzeichnis erstellt (ein Verzeichnis pro Domäne).
* Das Azure AD-Verzeichnis hat keinen globalen Administrator.

#### <a name="users"></a>Benutzer
* Für jeden Benutzer, der sich registriert, wird ein Benutzerobjekt im Azure AD-Verzeichnis erstellt.
* Jedes Benutzerobjekt wird als „extern“ markiert.
* Jeder Benutzer erhält Zugriff auf den Dienst, für den er sich registriert hat.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Wie beanspruche ich ein Self-Service-Azure AD-Verzeichnis für eine Domäne, die ich besitze?
Sie können ein Self-Service-Azure AD-Verzeichnis beanspruchen, indem Sie eine Domänenüberprüfung ausführen. Die Domänenüberprüfung weist nach, dass Sie die Domäne besitzen, indem DNS-Einträge erstellt werden.

Für eine DNS-Übernahme eines Azure AD-Verzeichnisses gibt es zwei Möglichkeiten:

* interne Übernahme (der Administrator ermittelt ein nicht verwaltetes Azure-Verzeichnis, das er in ein verwaltetes Verzeichnis umwandeln möchte)
* externe Übernahme (der Administrator versucht, seinem verwalteten Azure-Verzeichnis eine neue Domäne hinzuzufügen)

Möglicherweise möchten Sie überprüfen, ob Sie eine Domäne besitzen, da Sie ein nicht verwaltetes Verzeichnis übernehmen, nachdem ein Benutzer eine Self-Service-Registrierung ausgeführt hat. Oder Sie möchten einem vorhandenen verwalteten Verzeichnis eine neue Domäne hinzufügen. Angenommen, Sie haben die Domäne "contoso.com" und möchten eine neue Domäne mit dem Namen "contoso.co.uk" oder "contoso.uk" hinzufügen.

## <a name="what-is-domain-takeover"></a>Was bedeutet Domänenübernahme?
In diesem Abschnitt wird beschrieben, wie Sie bestätigen, dass Sie eine Domäne besitzen.

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Was ist die Domänenüberprüfung, und warum wird sie verwendet?
Damit Sie Vorgänge für ein Verzeichnis ausführen können, ist in Azure AD die Überprüfung des Besitzes der DNS-Domäne erforderlich.  Die Überprüfung der Domäne ermöglicht Ihnen, das Verzeichnis zu beanspruchen und entweder das Self-Service-Verzeichnis zu einem verwalteten Verzeichnis hochzustufen oder das Self-Service-Verzeichnis mit einem vorhandenen verwalteten Verzeichnis zusammenzuführen.

## <a name="examples-of-domain-validation"></a>Beispiele für die Domänenüberprüfung
Für eine DNS-Übernahme eines Verzeichnisses gibt es zwei Möglichkeiten:

* interne Übernahme (der Administrator ermittelt beispielsweise ein nicht verwaltetes Self-Service-Verzeichnis, das er in ein verwaltetes Verzeichnis umwandeln möchte)
* externe Übernahme (der Administrator versucht beispielsweise, einem verwalteten Verzeichnis eine neue Domäne hinzuzufügen)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interne Übernahme – Hochstufen eines nicht verwalteten Self-Service-Verzeichnisses zu einem verwalteten Verzeichnis
Bei einer internen Übernahme wird das Verzeichnis von einem nicht verwalteten Verzeichnis in ein verwaltetes Verzeichnis umgewandelt. Sie müssen eine Überprüfung des DNS-Domänennamens ausführen, bei der Sie einen MX-Eintrag oder TXT-Eintrag in der DNS-Zone erstellen. Diese Aktion bewirkt Folgendes:

* Überprüft, ob Sie die Domäne besitzen
* Ändert das Verzeichnis in ein verwaltetes Verzeichnis
* Macht Sie zum globalen Administrator des Verzeichnisses

Angenommen, ein IT-Administrator am Bellows College ermittelt, dass Benutzer in der Hochschule sich für Self-Service-Angebote registriert haben. Als registrierter Besitzer des DNS-Namens „BellowsCollege.com“ kann der IT-Administrator den Besitz des DNS-Namens in Azure überprüfen und dann das nicht verwaltete Verzeichnis übernehmen. Das Verzeichnis wird dann zu einem verwalteten Verzeichnis, und dem IT-Administrator wird für das Verzeichnis „BellowsCollege.com“ die globale Administratorrolle zugewiesen.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externe Übernahme – Zusammenführen eines Self-Service-Verzeichnisses mit einem vorhandenen verwalteten Verzeichnis
Bei einer externen Übernahme verfügen Sie bereits über ein verwaltetes Verzeichnis und möchten, dass alle Benutzer und Gruppen in einem nicht verwalteten Verzeichnis diesem verwalteten Verzeichnis beitreten, anstatt zwei separate Verzeichnisse einzurichten.

Als Administrator eines verwalteten Verzeichnisses fügen Sie eine Domäne hinzu, und dieser Domäne ist zufällig ein nicht verwaltetes Verzeichnis zugeordnet.

Angenommen, Sie sind IT-Administrator und verfügen bereits über ein verwaltetes Verzeichnis für „Contoso.com“, einen Domänennamen, der für Ihre Organisation registriert ist. Sie erkennen, dass Benutzer in Ihrer Organisation eine Self-Service-Registrierung für ein Angebot mithilfe des E-Mail-Domänennamens user@contoso.co.uk durchgeführt haben, der ein weiterer Domänenname im Besitz Ihrer Organisation ist. Diese Benutzer haben derzeit Konten in einem nicht verwalteten Verzeichnis für „contoso.co.uk“.

Sie möchten nicht zwei separate Verzeichnisse verwalten, weshalb Sie das nicht verwaltete Verzeichnis für „contoso.co.uk“ mit Ihrem vorhandenen von der IT-Abteilung verwalteten Verzeichnis für „contoso.com“ zusammenführen.

Bei der externen Übernahme erfolgt der gleiche DNS-Überprüfungsprozess wie bei der internen Übernahme.  Der Unterschied ist, dass Benutzer und Dienste dem von der IT verwalteten Verzeichnis neu zugeordnet werden.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Was sind die Auswirkungen einer externen Übernahme?
Mit einer externen Übernahme erfolgt eine Zuordnung von Benutzern zu Ressourcen, damit Benutzer weiter ohne Unterbrechung auf Dienste zugreifen können. Viele Anwendungen, einschließlich RMS für Einzelpersonen, führen die Zuordnung von Benutzern zu Ressourcen ordnungsgemäß durch, sodass Benutzer ohne Änderungen weiterhin Zugriff auf diese Dienste haben. Wenn eine Anwendung die Zuordnung von Benutzern zu Ressourcen nicht effizient durchführt, kann die externe Übernahme explizit blockiert werden, um eine schlechte Benutzererfahrung zu verhindern.

#### <a name="directory-takeover-support-by-service"></a>Dienste mit Unterstützung der Verzeichnisübernahme
Derzeit unterstützen die folgenden Dienste die Übernahme:

* RMS

Die folgenden Dienste werden die Übernahme in Kürze unterstützen:

* PowerBI

Die folgenden Dienste unterstützen die Übernahme nicht und erfordern zusätzliche Administratoraktionen für die Migration von Benutzerdaten nach einer externen Übernahme.

* SharePoint/OneDrive

## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Ausführen der Übernahme eines DNS-Domänennamens
Sie haben verschiedene Möglichkeiten zum Ausführen einer Domänenüberprüfung (und einer Übernahme, falls gewünscht):

1. Azure-Verwaltungsportal

   Eine Übernahme wird ausgelöst, indem Sie eine Domäne hinzufügen.  Wenn für die Domäne bereits ein Verzeichnis vorhanden ist, können Sie eine externe Übernahme durchführen.

   Melden Sie sich mit Ihren Anmeldedaten am Azure-Portal an.  Navigieren Sie zu Ihrem vorhandenen Verzeichnis, und klicken Sie dann auf **Domäne hinzufügen**.
2. Office 365

   Verwenden Sie die Optionen auf der Seite [Domänen verwalten](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) in Office 365, um mit Ihren Domänen und DNS-Einträgen zu arbeiten. Siehe [Überprüfen Ihre Domäne in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   Die folgenden Schritte sind für die Validierung mithilfe von Windows PowerShell erforderlich.

   | Schritt | Zu verwendendes Cmdlet |
   | --- | --- |
   | Erstellen eines Objekts mit Anmeldeinformationen |Get-Credential |
   | Mit Azure AD verbinden |Connect-MsolService |
   | Abrufen einer Liste von Domänen |Get-MsolDomain |
   | Erstellen einer Abfrage |Get-MsolDomainVerificationDns |
   | Erstellen eines DNS-Eintrags |Erfolgt auf Ihrem DNS-Server |
   | Überprüfen der Abfrage |Confirm-MsolEmailVerifiedDomain |

Beispiel:

1. Verwenden Sie für die Verbindung mit Azure AD diesen Anmeldeinformationen wir für die Verbindung mit der Self-Service-Lösung:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Rufen Sie eine Liste von Domänen ab:

    Get-MsolDomain
3. Führen Sie dann das Cmdlet "Get-MsolDomainVerificationDns" aus, um eine Abfrage zu erstellen:

    Get-MsolDomainVerificationDns –DomainName *Name_Ihrer_Domäne* –Mode DnsTxtRecord

    Beispiel:

    Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
4. Kopieren Sie den Wert (die Abfrage), der von diesem Befehl zurückgegeben wird.

    Beispiel:

    MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
5. Erstellen Sie in Ihrem öffentlichen DNS-Namespace einen "DnsTxt"-Eintrag, der den Wert enthält, den Sie im vorherigen Schritt kopiert haben.

    Der Name für diesen Eintrag ist der Name der übergeordneten Domäne. Wenn Sie also diesen Ressourceneintrag mithilfe der DNS-Rolle von Windows Server erstellen, sollten Sie den Eintragsnamen leer lassen und bloß den Wert in das Textfeld kopieren.
6. Führen Sie das Cmdlet "Confirm-MsolDomain" aus, um die Abfrage zu überprüfen:

    Confirm-MsolEmailVerifiedDomain -DomainName *Name_Ihrer_Domäne*

    Beispiel:

    Confirm-MsolEmailVerifiedDomain -DomainName contoso.com

Bei einer erfolgreichen Abfrage kehren Sie ohne Fehler zur Eingabeaufforderung zurück.

## <a name="how-do-i-control-self-service-settings"></a>Wie steuere ich Self-Service-Einstellungen?
Administratoren stehen derzeit zwei Self-Service-Steuerungsmöglichkeiten zur Verfügung. Sie können steuern:

* Ob Benutzer dem Verzeichnis per E-Mail beitreten können.
* Ob Benutzer sich selbst für Anwendungen und Dienste lizenzieren können.

### <a name="how-can-i-control-these-capabilities"></a>Wie können diese Funktionen gesteuert werden?
Ein Administrator kann diese Funktionen mit den Parametern des Azure AD-Cmdlets "Set-MsolCompanySettings" konfigurieren:

* **AllowEmailVerifiedUsers** steuert, ob ein Benutzer ein nicht verwaltetes Verzeichnis erstellen oder diesem beitreten kann. Wenn Sie diesen Parameter auf „$false“ festlegen, können dem Verzeichnis keine über E-Mail verifizierten Benutzer beitreten.
* **AllowAdHocSubscriptions** steuert, ob Benutzern das Ausführen der Self-Service-Registrierung erlaubt ist. Wenn Sie diesen Parameter auf "$false" festlegen, können Benutzer keine Self-Service-Registrierung ausführen.

### <a name="how-do-the-controls-work-together"></a>Wie funktionieren diese Steuerungsmöglichkeiten zusammen?
Diese beiden Parameter können zusammen verwendet werden, um eine genauere Steuerung der Self-Service-Registrierung zu erreichen. Der folgende Befehl erlaubt Benutzern beispielsweise die Self-Service-Registrierung, jedoch nur, wenn die Benutzer bereits über ein Konto in Azure AD verfügen (d. h. dass Benutzer, für die ein über E-Mail verifiziertes Konto erstellt werden müsste, die Self-Service-Registrierung nicht erlaubt ist):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Im folgenden Flussdiagramm werden die verschiedenen Kombinationen für diese Parameter und die resultierenden Bedingungen für das Verzeichnis und die Self-Service-Registrierung erläutert.

![][1]

Weitere Informationen und Beispiele zum Verwenden dieser Parameter finden Sie unter [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Weitere Informationen
* [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-Cmdlet-Referenz](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png

