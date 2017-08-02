---
title: "Empfohlene Richtlinien für sichere E-Mails | Microsoft-Dokumentation"
description: "Beschreibt die Richtlinien für Microsoft-Empfehlungen zum Anwenden von E-Mail-Richtlinien und -Konfigurationen."
author: jeffgilb
manager: femila
editor: jsnow
ms.service: guidance
ms.topic: article
ms.date: 07/12/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
pnp.series.title: Best Practices
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 1d9577f44adcdefa0b0b8f0f2bafce8d0bda4697
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="recommended-policies"></a>Empfohlene Richtlinien
 
In diesem Artikel werden empfohlene Richtlinien beschrieben, die unseren Kunden helfen, mit sicheren E-Mails und E-Mail-Clients der Organisation moderne Authentifizierung und bedingten Zugriff zu unterstützen. Außerdem werden die empfohlenen Standardplattform-Clientkonfigurationen, die Ihren Benutzern eine optimale SSO-Erfahrung bieten, sowie die technischen Voraussetzungen für bedingten Zugriff erläutert.

## <a name="prerequisites"></a>Voraussetzungen

Vor der Implementierung der im Rest dieses Dokuments beschriebenen Richtlinien muss Ihre Organisation mehrere Voraussetzungen erfüllen:
* [Konfigurieren Sie benannte Orte](https://docs.microsoft.com/azure/active-directory/active-directory-known-networks-azure-portal). Azure AD Identity Protection sammelt und analysiert alle verfügbaren Sitzungsdaten, um eine Risikobewertung zu generieren. Es wird empfohlen, in der Azure AD-Konfiguration benannter Orte die öffentlichen IP-Adressbereiche Ihrer Organisation für das Netzwerk anzugeben. Datenverkehr aus diesen Bereichen erhält eine niedrigere Risikobewertung, und Datenverkehr von außerhalb der Unternehmensumgebung erhält eine höhere Risikobewertung.
* [Registrieren Sie alle Benutzer für Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-manage-users-and-devices). Azure AD Identity Protection nutzt für eine zusätzliche Sicherheitsüberprüfung Azure MFA. Es wird empfohlen, im Voraus von allen Benutzern zu verlangen, sich für Azure MFA zu registrieren.
* [Aktivieren Sie die automatische Geräteregistrierung von in Domänen eingebundenen Windows-Computern](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup). Mit bedingtem Zugriff lässt sich sicherstellen, dass das Gerät, das eine Verbindung mit dem Dienst herstellt, in eine Domäne eingebunden oder kompatibel ist. Um dies für Windows-Computer zu unterstützen, muss das Gerät bei Azure AD registriert sein.  In diesem Artikel wird das Konfigurieren der automatischen Geräteregistrierung erläutert.  Beachten Sie, dass AD FS erforderlich ist.
* **Vorbereiten Ihres Supportteams**. Treffen Sie Vorkehrungen für Benutzer, die keine MFA ausführen können. Sie können sie zu diesem Zweck einer Richtlinienausschlussgruppe hinzufügen oder neue MFA-Informationen für sie registrieren. Bevor Sie eine dieser sicherheitsrelevanten Änderungen vornehmen, müssen Sie sicherstellen, dass sie vom tatsächlichen Benutzer angefordert wird. Dabei ist es hilfreich, von den Vorgesetzten der Benutzer die Unterstützung bei der Genehmigung anzufordern.
* [Konfigurieren Sie das Rückschreiben von Kennwörtern in das lokale AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). Mithilfe des Rückschreibens von Kennwörtern kann Azure AD von Benutzern das Ändern ihrer lokalen Kennwörter anfordern, wenn ein hohes Risiko von Kontogefährdung erkannt wurde. Sie können dieses Feature mithilfe von Azure AD Connect auf zwei Arten aktivieren. Sie können das Zurückschreiben von Kennwörtern entweder im Bildschirm für die optionalen Features im Setup-Assistenten für Azure AD Connect aktivieren, oder Sie verwenden zur Aktivierung Windows PowerShell.  
* [Aktivieren Sie moderne Authentifizierung](https://support.office.com/article/Enable-Exchange-Online-for-modern-authentication-58018196-f918-49cd-8238-56f57f38d662), und [schützen Sie Legacyendpunkte](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-supported-apps).  Der bedingte Zugriff kann sowohl für mobile Anwendungen als auch für Desktopanwendungen mit modernen Authentifizierungsverfahren verwendet werden. Wenn die Anwendung Legacyauthentifizierungsprotokolle verwendet, kann sie möglicherweise trotz angewendeter Bedingungen Zugriff erlangen. Es ist wichtig zu wissen, welche Anwendungen Regeln für den bedingten Zugriff verwenden können, und die Schritte zu kennen, die Sie zum Schutz anderer Anwendungseinstiegspunkte durchführen müssen.
* [Aktivieren Sie Azure Information Protection](https://docs.microsoft.com/information-protection/get-started/infoprotect-tutorial-step1) durch Aktivieren von Rights Management. Verwenden Sie Azure Information Protection mit E-Mail, um mit der Klassifizierung von E-Mails zu beginnen. Absolvieren Sie das Schnellstarttutorial zum Anpassen und Veröffentlichen von Richtlinien.  

> [!NOTE]
> Die folgenden Empfehlungen basieren auf drei verschiedenen Ebenen der Sicherheit und des Schutzes für Ihre E-Mails, die entsprechend Ihren Anforderungen angewendet werden können: [„Baseline“, „Sensibel“ und „Stark reglementiert“](https://docs.microsoft.com/azure/active-directory/secure-email-introduction). Um weitere Informationen hierzu zu erhalten, können Sie die [Infografik „Identitäts- und Geräteschutz für Office 365“](https://go.microsoft.com/fwlink/p/?linkid=841656) herunterladen.  


## <a name="baseline"></a>Grundwert 
In diesem Abschnitt werden die Empfehlungen für sichere E-Mails für die Baseline-Ebene des Daten-, Identitäts- und Geräteschutzes beschrieben. Diese Empfehlungen erfüllen i. d. R. die Standardschutzanforderungen vieler Organisationen.
>[!NOTE]
>Die folgenden Richtlinien bauen aufeinander auf. In jedem Abschnitt werden nur die auf die jeweilige Ebene angewendeten Richtlinien beschrieben.
>

### <a name="conditional-access-policy-settings"></a>Einstellungen der Richtlinie für bedingten Zugriff

#### <a name="identity-protection"></a>Schutz der Identität (Identity Protection) 
Sie können Benutzern einmaliges Anmelden (Single Sign-On, SSO) ermöglichen, wie in vorherigen Abschnitten beschrieben. Sie müssen nur bei Bedarf basierend auf [Risikoereignissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) eingreifen.  

* Machen Sie bei mittlerem oder höherem Anmelderisiko MFA-basierte Anmeldung erforderlich.
* Machen Sie für stark gefährdete Benutzer die sichere Kennwortänderung erforderlich.

>[!IMPORTANT]
>Für diese Richtlinienempfehlung sind [Kennwortsynchronisierung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) und [Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) erforderlich.
>

#### <a name="data-loss-prevention"></a>Verhindern von Datenverlusten 
Mit den Richtlinien für die Geräte- und App-Verwaltung sollen Datenverluste im Fall eines verlorenen oder gestohlenen Geräts verhindert werden. Zu diesem Zweck können Sie sicherstellen, dass der Zugriff auf Daten durch eine PIN geschützt wird, dass die Daten auf dem Gerät verschlüsselt sind und dass das Gerät nicht gefährdet ist.

|Richtlinienempfehlung|Beschreibung|
|:--------------------|:----------|
|**Verwaltung der Benutzer-PCs erforderlich machen**|Legen Sie fest, dass Benutzer ihre PCs einer Active Directory-Domäne hinzufügen oder für die Verwaltung mit Intune oder Configuration Manager registrieren müssen.|
|**Sicherheitseinstellungen über Gruppenrichtlinienobjekte (Group Policy Objects, GPOs) oder Configuration Manager-Richtlinien für in Domänen eingebundene PCs anwenden**|Stellen Sie Richtlinien bereit, die verwaltete PCs für die Aktivierung von BitLocker, Virenschutz und Firewall konfigurieren.|
|**Die Verwaltung der mobilen Geräte der Benutzer erforderlich machen**|Legen Sie fest, dass Benutzergeräte, die für den Zugriff auf E-Mails verwendet werden, durch Intune verwaltet werden oder dass der Zugriff auf Unternehmens-E-Mails nur über mobile E-Mail-Apps erfolgt, die durch Intune-App-Schutz geschützt werden (z.B. Outlook Mobile).|
|**Eine Intune-Gerätekompatibilitätsrichtlinie auf verwaltete Geräte anwenden**|Wenden Sie auf verwaltete mobile Geräte des Unternehmens und auf von Intune verwaltete PCs eine Intune-Gerätekompatibilitätsrichtlinie an, die Folgendes erfordert: eine PIN mit einer Mindestlänge von 6 Zeichen, Geräteverschlüsselung, ein fehlerfreies Gerät (nicht mit Jailbreak oder Rooting manipuliert, verfügt über Integritätsnachweis). Stellen Sie außerdem ggf. die Anforderung, dass die Geräte gemäß der Bewertung durch einen MTP-Drittanbieter, z.B. Lookout oder SkyCure, Geräte mit geringem Risiko sind.|
|**Eine Intune-App-Schutzrichtlinie für verwaltete Apps, die auf nicht verwalteten Geräten ausgeführt werden, anwenden**|Wenden Sie für verwaltete Apps, die auf nicht verwalteten persönlichen Mobilgeräten ausgeführt werden, eine Intune-App-Schutzrichtlinie an, die Folgendes erfordert: eine PIN mit einer Mindestlänge von 6 Zeichen, Geräteverschlüsselung und ein fehlerfreies Gerät (nicht mit Jailbreak oder Rooting manipuliert, verfügt über Integritätsnachweis).|

### <a name="user-impact"></a>Auswirkungen auf die Benutzer

Für die meisten Organisationen ist es wichtig, den Benutzern mitteilen zu können, wann und unter welchen Bedingungen sie sich bei Office 365 anmelden müssen, um auf ihre E-Mails zuzugreifen.  

Benutzer profitieren in der Regel von einmaligem Anmelden (Single Sign-On, SSO), außer in den folgenden Situationen: 
* Beim Anfordern von Authentifizierungstoken für Exchange Online:
  * Benutzer werden möglicherweise immer zum Ausführen von MFA aufgefordert, wenn ein mittleres oder höheres Anmelderisiko festgestellt wird und sie in der aktuellen Sitzung noch keine MFA ausgeführt haben.  
  * Die Benutzer müssen E-Mail-Apps verwenden, die das Intune-App-Schutz-SDK unterstützen, oder für den Zugriff auf E-Mails Geräte verwenden, die mit Intune kompatibel oder in eine AD-Domäne eingebunden sind. 
* Wenn gefährdete Benutzer sich anmelden und die MFA erfolgreich abschließen, werden sie zum Ändern ihres Kennworts aufgefordert.

## <a name="sensitive"></a>Sensibel

In diesem Abschnitt werden die Empfehlungen für sichere E-Mails für die Ebene „Sensibel“ des Daten-, Identitäts- und Geräteschutzes beschrieben. Diese Empfehlungen gelten für Kunden, die eine Teilmenge ihrer Daten oder ihre gesamten Daten auf höheren Ebenen schützen müssen. 

Sie können stärkeren Schutz auf alle oder bestimmte Datasets in Ihrer Office 365-Umgebung anwenden. Sie können beispielsweise Richtlinien anwenden, die sicherstellen, dass sensible Daten nur zwischen geschützten Apps freigegeben werden, um Datenverlust zu verhindern. Es wird empfohlen, Identitäten und Geräte, die auf sensible Daten zugreifen, mit vergleichbaren Sicherheitsstufen zu schützen. 

### <a name="conditional-access-policy-settings"></a>Einstellungen der Richtlinie für bedingten Zugriff
#### <a name="identity-protection"></a>Schutz der Identität (Identity Protection) 

Sie können Benutzern einmaliges Anmelden (Single Sign-On, SSO) ermöglichen, wie in vorherigen Abschnitten beschrieben. Sie müssen nur bei Bedarf basierend auf [Risikoereignissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) eingreifen.   

* Machen Sie für Sitzungen mit geringem oder höherem Risiko MFA erforderlich.
 * Machen Sie für stark gefährdete Benutzer die sichere Kennwortänderung erforderlich.

>[!IMPORTANT]
>Für diese Richtlinienempfehlung sind [Kennwortsynchronisierung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) und [Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) erforderlich.
>

#### <a name="data-loss-prevention"></a>Verhindern von Datenverlusten 

Mit diesen Richtlinien für die Geräte- und App-Verwaltung sollen Datenverluste im Fall eines verlorenen oder gestohlenen Geräts verhindert werden. Zu diesem Zweck können Sie sicherstellen, dass der Zugriff auf Daten durch eine PIN geschützt wird, dass die Daten auf dem Gerät verschlüsselt sind und dass das Gerät nicht gefährdet ist.

|Richtlinienempfehlung|Beschreibung|
|:--------------------|:----------|
|**Verwaltung der Benutzer-PCs erforderlich machen**|Legen Sie fest, dass Benutzer ihre PCs einer Active Directory-Domäne hinzufügen oder für die Verwaltung mit Intune oder Configuration Manager registrieren müssen. Stellen Sie sicher, dass diese Geräte die Richtlinien erfüllen, bevor Sie E-Mail-Zugriff zulassen.|
|**Sicherheitseinstellungen über Gruppenrichtlinienobjekte (Group Policy Objects, GPOs) oder Configuration Manager-Richtlinien für in Domänen eingebundene PCs anwenden**|Stellen Sie Richtlinien bereit, die verwaltete PCs für die Aktivierung von BitLocker, Virenschutz und Firewall konfigurieren.|
|**Die Verwaltung der mobilen Geräte der Benutzer erforderlich machen**|Legen Sie fest, dass Benutzergeräte, die für den Zugriff auf E-Mails verwendet werden, durch Intune verwaltet werden oder dass der Zugriff auf Unternehmens-E-Mails nur über mobile E-Mail-Apps erfolgt, die durch Intune-App-Schutz geschützt werden (z.B. Outlook Mobile).|
|**Eine Intune-Gerätekompatibilitätsrichtlinie auf verwaltete Geräte anwenden**|Wenden Sie auf verwaltete mobile Geräte des Unternehmens und auf von Intune verwaltete PCs eine Intune-Gerätekompatibilitätsrichtlinie an, die Folgendes erfordert: eine PIN mit einer Mindestlänge von 6 Zeichen, Geräteverschlüsselung, ein fehlerfreies Gerät (nicht mit Jailbreak oder Rooting manipuliert, verfügt über Integritätsnachweis). Stellen Sie außerdem ggf. die Anforderung, dass die Geräte gemäß der Bewertung durch einen MTP-Drittanbieter, z.B. Lookout oder SkyCure, Geräte mit geringem Risiko sind.|
|**Eine Intune-App-Schutzrichtlinie für verwaltete Apps, die auf nicht verwalteten Geräten ausgeführt werden, anwenden**|Wenden Sie für verwaltete Apps, die auf nicht verwalteten persönlichen Mobilgeräten ausgeführt werden, eine Intune-App-Schutzrichtlinie an, die Folgendes erfordert: eine PIN mit einer Mindestlänge von 6 Zeichen, Geräteverschlüsselung und ein fehlerfreies Gerät (nicht mit Jailbreak oder Rooting manipuliert, verfügt über Integritätsnachweis).|

### <a name="user-impact"></a>Auswirkungen auf die Benutzer

Für die meisten Organisationen ist es wichtig, den Benutzer mitteilen zu können, wann und unter welchen Bedingungen sie sich bei Office 365-E-Mail anmelden müssen. 

Benutzer profitieren in der Regel von einmaligem Anmelden (Single Sign-On, SSO), außer in den folgenden Situationen: 
* Beim Anfordern von Authentifizierungstoken für Exchange Online:
  * Benutzer werden immer zum Ausführen von MFA aufgefordert, wenn ein niedriges oder höheres Anmelderisiko festgestellt wird und sie in der aktuellen Sitzung noch keine MFA ausgeführt haben.  
  * Die Benutzer müssen E-Mail-Apps verwenden, die das Intune-App-Schutz-SDK unterstützen, oder für den Zugriff auf E-Mails Geräte verwenden, die mit Intune kompatibel oder in eine AD-Domäne eingebunden sind. 
* Wenn gefährdete Benutzer sich anmelden und die MFA erfolgreich abschließen, werden sie zum Ändern ihres Kennworts aufgefordert.

## <a name="highly-regulated"></a>Stark reglementiert
In diesem Abschnitt werden die Empfehlungen für sichere E-Mails für die Ebene „Stark reglementiert“ des Daten-, Identitäts- und Geräteschutzes beschrieben. Diese Empfehlungen gelten für Kunden, die eventuell über eine sehr geringe Menge von Daten verfügen, die streng geheim sind, unter das Geschäftsgeheimnis fallen oder strengen Regeln unterliegen. Microsoft bietet Funktionen, die Organisationen diese Anforderungen zu erfüllen helfen, darunter zusätzlichen Schutz für Identitäten und Geräte. 

### <a name="conditional-access-policy-settings"></a>Einstellungen der Richtlinie für bedingten Zugriff
#### <a name="identity-protection"></a>Schutz der Identität (Identity Protection) 

Für die Ebene „Stark reglementiert“ empfiehlt Microsoft das Erzwingen von MFA für alle neuen Sitzungen.
* Machen Sie für alle neuen Sitzungen MFA erforderlich.
* Machen Sie für stark gefährdete Benutzer die sichere Kennwortänderung erforderlich.

>[!IMPORTANT]
>Für diese Richtlinienempfehlung sind [Kennwortsynchronisierung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) und [Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) erforderlich.
>

#### <a name="data-loss-prevention"></a>Verhindern von Datenverlusten
Mit diesen Richtlinien für die Geräte- und App-Verwaltung sollen Datenverluste im Fall eines verlorenen oder gestohlenen Geräts verhindert werden. Dies wird erreicht, indem Sie sicherstellen, dass der Zugriff auf Daten durch eine PIN geschützt wird, dass die Daten auf dem Gerät verschlüsselt sind und dass das Gerät nicht gefährdet ist.

Für die Ebene „Stark reglementiert“ wird empfohlen, die Ausführung von Apps, die die Intune-App-Schutzrichtlinie unterstützen, nur auf Geräten zuzulassen, die mit Intune kompatibel oder in eine Domäne eingebunden sind.

|Richtlinienempfehlung|Beschreibung|
|:--------------------|:----------|
|**Verwaltung der Benutzer-PCs erforderlich machen**|Legen Sie fest, dass Benutzer ihre PCs einer Active Directory-Domäne hinzufügen oder für die Verwaltung mit Intune oder Configuration Manager registrieren müssen. Stellen Sie sicher, dass diese Geräte die Richtlinien erfüllen, bevor Sie E-Mail-Zugriff zulassen.|
|**Sicherheitseinstellungen über Gruppenrichtlinienobjekte (Group Policy Objects, GPOs) oder Configuration Manager-Richtlinien für in Domänen eingebundene PCs anwenden**|Stellen Sie Richtlinien bereit, die verwaltete PCs für die Aktivierung von BitLocker, Virenschutz und Firewall konfigurieren.|
|**Die Verwaltung der mobilen Geräte der Benutzer erforderlich machen**|Legen Sie fest, dass Benutzergeräte, die für den Zugriff auf E-Mails verwendet werden, durch Intune verwaltet werden oder dass der Zugriff auf Unternehmens-E-Mails nur über mobile E-Mail-Apps erfolgt, die durch Intune-App-Schutz geschützt werden (z.B. Outlook Mobile).|
|**Eine Intune-Gerätekompatibilitätsrichtlinie auf verwaltete Geräte anwenden**|Wenden Sie auf verwaltete mobile Geräte des Unternehmens und auf von Intune verwaltete PCs eine Intune-Gerätekompatibilitätsrichtlinie an, die Folgendes erfordert: eine PIN mit einer Mindestlänge von 6 Zeichen, Geräteverschlüsselung, ein fehlerfreies Gerät (nicht mit Jailbreak oder Rooting manipuliert, verfügt über Integritätsnachweis). Stellen Sie außerdem ggf. die Anforderung, dass die Geräte gemäß der Bewertung durch einen MTP-Drittanbieter, z.B. Lookout oder SkyCure, Geräte mit geringem Risiko sind.|

### <a name="user-impact"></a>Auswirkungen auf die Benutzer
Für die meisten Organisationen ist es wichtig, den Benutzer mitteilen zu können, wann und unter welchen Bedingungen sie sich bei Office 365-E-Mail anmelden müssen. 

* Die maximale Gültigkeitsdauer einer Sitzung für einmaliges Anmelden beträgt 1 Tag. Nach Ablauf der Sitzung müssen sich die Benutzer erneut mit MFA authentifizieren.
* Wenn gefährdete Benutzer sich anmelden, werden sie nach Abschluss der MFA zum Ändern ihres Kennworts aufgefordert.
* Beim Anfordern von Authentifizierungstoken für Exchange Online:
  * Immer wenn Benutzer eine neue Sitzung starten, werden sie zum Ausführen von MFA aufgefordert.  
  * Die Benutzer müssen E-Mail-Apps verwenden, die das Intune-App-Schutz-SDK unterstützen
  * Der E-Mail-Zugriff der Benutzer muss über Geräte erfolgen, die mit Intune kompatibel oder in eine AD-Domäne eingebunden sind. 
 
 ## <a name="next-steps"></a>Nächste Schritte
 [Bereitstellen von empfohlenen Richtlinien für sichere E-Mails](secure-email-deploy-recommended-policies.md)

