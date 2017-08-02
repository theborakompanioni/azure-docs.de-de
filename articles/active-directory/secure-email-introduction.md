---
title: Empfohlene Sicherheitsrichtlinien und Konfigurationen | Microsoft-Dokumentation
description: "Beschreibt Empfehlungen und Kernkonzepte von Microsoft für die Bereitstellung von Richtlinien und Konfigurationen für sichere E-Mails, Dokumente und Apps."
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
ms.openlocfilehash: 3c8e636921ab269a5314f74789c8144602685412
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="recommended-security-policies-and-configurations"></a>Empfohlene Sicherheitsrichtlinien und Konfigurationen
 
## <a name="introduction"></a>Einführung

Die Empfehlungen in diesem Artikel beschreiben allgemeine Microsoft-Richtlinien zum Anwenden von Sicherheitsrichtlinien und Konfigurationen im gesamten Unternehmen. Diese Empfehlungen stellen sicher, dass Ihre Mitarbeiter geschützt und produktiv sind. 

### <a name="intended-audience"></a>Zielpublikum

Die Zielgruppe für diese Empfehlungen sind Enterprise Infrastructure Architects und IT-Spezialisten, die mit den Produkten [Exchange Online](https://technet.microsoft.com/library/jj200580.aspx) (Office 365) und [Microsoft Enterprise Mobility + Security](http://microsoft.com/ems) vertraut sind. Diese Produkte enthalten u. a. Azure Active Directory (Identität), Microsoft Intune (Geräteverwaltung) und Azure Information Protection (Datenschutz). 

### <a name="customer-environment"></a>Kundenumgebung

Die empfohlene Richtlinien gelten für Unternehmensorganisationen, die vollständig in der Microsoft Cloud arbeiten, und für Kunden, deren Infrastruktur lokal und in der Microsoft Cloud bereitgestellt ist. 

### <a name="assumptions"></a>Annahmen

Viele der hier beschriebenen Empfehlungen basieren auf Diensten, die nur mit einem Enterprise Mobility + Security (EMS) E5-Abonnement verfügbar sind. In den Empfehlungen werden alle Funktionen des EMS E5-Abonnements vorausgesetzt. 

### <a name="caveats"></a>Einschränkungen

Für Ihre Organisation gelten möglicherweise gesetzliche oder andere Complianceanforderungen, einschließlich spezieller Empfehlungen, die das Umsetzen von Richtlinien erfordern, die von den hier empfohlenen Konfigurationen abweichen.  Für diese Konfigurationen werden Nutzungskontrollen empfohlen, die bisher nicht verfügbar waren.  Wir empfehlen diese Kontrollen, da wir glauben, dass sie ein ausgewogenes Verhältnis zwischen Sicherheit und Produktivität ermöglichen.  

Wir waren bestrebt, eine große Vielfalt von Schutzanforderungen der Organisationen zu berücksichtigen. Es ist uns jedoch nicht möglich, alle potenziellen Anforderungen oder alle besonderen Aspekte Ihrer Organisation zu berücksichtigen. Verwenden Sie dieses Dokument als Leitfaden, um die Konzepte von Microsoft und des Secure Productive Enterprise-Teams für das richtige Anwenden von Richtlinien in Ihrer Organisation zu verstehen. 

>[!NOTE]
>Einen Überblick über die wichtigen Konzepte, die für das Verständnis der in diesen Empfehlungen beschriebenen Schutzfunktionen erforderlich sind, finden Sie unter [Übersicht über die Dienste von EMS und Office 365](secure-email-ems-office365-service-descriptions.md).
>

## <a name="core-concepts"></a>Wichtige Konzepte

Alle Sicherheitsmaßnahmen in der ganzen Welt sind nutzlos, wenn Benutzer, die sich bei ihrer Arbeit durch unnötige Erschwernisse behindert fühlen, die Sicherheitsrichtlinien Ihrer Organisation umgehen. Mit einmaligem Anmelden (Single Sign-On, SSO), einem Feature von Azure AD, wird versucht, die Belastung für Benutzer zu minimieren. Benutzer können produktiv bleiben und dennoch die Zugriffssteuerungsrichtlinien der Organisation erfüllen. 

### <a name="single-sign-on-authentication"></a>Authentifizierung durch einmaliges Anmelden

Das folgende Diagramm veranschaulicht einen typischen Ablauf der SSO-Authentifizierung:

![Einmaliges Anmelden für Endbenutzer](./media/secure-email/typical-authentication-flow.png)

Zum Starten der Authentifizierung sendet der Client Anmeldeinformationen (z.B. Benutzername und Kennwort) und/oder in der Vergangenheit erhaltene SSO-Elemente an Azure AD. Ein SSO-Element kann ein Sitzungstoken für einen Browser oder ein Aktualisierungstoken für funktionsreiche Anwendungen sein. 

Azure AD überprüft die Anmeldeinformationen und/oder SSO-Elemente und wertet alle anwendbaren Richtlinien aus. Anschließend wird ein Zugriffstoken für den Ressourcenanbieter (in der Abbildung Exchange Online) ausgegeben. Azure AD gibt außerdem in der Antwort ein SSO-Element aus, damit der Client in zukünftigen Anforderungen SSO nutzen kann. 

Der Client speichert das SSO-Element und sendet das Zugriffstoken als Identitätsnachweis an den Ressourcenanbieter. Nachdem Exchange Online das Zugriffstoken überprüft und die erforderlichen Überprüfungen ausgeführt hat, wird dem Client Zugriff auf die E-Mail-Nachrichten gewährt.

#### <a name="single-sign-on-sso-refresh-tokens"></a>SSO-Aktualisierungstoken

SSO kann auf verschiedene Weise erreicht werden. Beispielsweise können Cookies von einem Identitätsanbieter als SSO-Element verwendet werden, um den Anmeldungsstatus eines Benutzer in einem Browser zu speichern. Anschließend ist es möglich, sich mithilfe desselben Identitätsanbieters automatisch (ohne Aufforderung zur Eingabe von Anmeldeinformationen) bei Anwendungen anzumelden. 

Wenn sich ein Benutzer bei Azure AD authentifiziert, wird eine SSO-Sitzung beim Browser des Benutzers und bei Azure AD hergestellt. Das SSO-Token stellt diese Sitzung dar (in Form eines Cookies). In Azure AD werden zwei Arten von SSO-Sitzungstoken verwendet: beständig und nicht beständig. Beständige Sitzungstoken werden in Browsern als beständige Cookies gespeichert, wenn während der Anmeldung das Kontrollkästchen „Angemeldet bleiben“ aktiviert ist. Nicht beständige Sitzungstoken werden als Sitzungscookies gespeichert und zerstört, wenn der Browser geschlossen wird. 

Für stabile Anwendungen, die moderne Authentifizierungsprotokolle, z.B. [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html) und [OAuth 2.0](https://tools.ietf.org/html/rfc6749), nutzen können, wird SSO ermöglicht, indem Aktualisierungstoken als SSO-Elemente verwendet werden (zusätzlich zu den zuvor beschriebenen SSO-Cookies). Die Aktualisierungstoken werden einem Autorisierungsserver präsentiert, wenn eine Anwendung ein neues Zugriffstoken anfordert. 

Das Aktualisierungstoken enthält Ansprüche und Attribute bezüglich der Art der beim Authentifizieren von Benutzern verwendeten Authentifizierungsmethoden. Wenn sich zum Beispiel ein Benutzer mit mehreren Methoden (Benutzername und Kennwort sowie telefonbasierte Authentifizierung) erfolgreich authentifiziert hat, wird im Aktualisierungstoken ein MFA-Anspruch (Multi-Factor Authentication) präsentiert. Es können zudem zusätzliche Ansprüche vorhanden sein, die z.B. Daten zur MFA-Gültigkeitsdauer enthalten. 

Mithilfe von Aktualisierungstoken kann der Client ein neues Zugriffstoken erhalten, ohne eine neue interaktive Authentifizierung durchzuführen. Aktualisierungstoken haben eine wesentlich längere Gültigkeitsdauer als Zugriffstoken und können eingelöst werden, um ein neues Paar von Zugriffs-und Aktualisierungstoken zu erhalten. Die erhaltenen neuen Aktualisierungstoken können dann kontinuierlich verwendet werden, um einen weiteren Satz von Zugriffs- und Aktualisierungstoken abzurufen. 

Der Client setzt den SSO-Vorgang fort, bis die festgelegte maximale Inaktivitätszeit des Aktualisierungstokens abgelaufen ist, das maximale Alter des Aktualisierungstokens abgelaufen ist oder sich die Anforderungen der Authentifizierungs- und Autorisierungsrichtlinie ändern. Diese Änderung erfolgt während der Ausstellung des ursprünglichen Aktualisierungstokens. Wesentliche Änderungen von Benutzerattributen, z.B. eine Kennwortzurücksetzung, erfordern ebenfalls das Generieren eines neuen Authentifizierungstokens. Der Client muss eine neue interaktive Authentifizierung ausführen, um fortfahren zu können. Im Wesentlichen bedeutet dies eine Unterbrechung des SSO-Prozesses, die bisher nicht auf dem Client aufgetreten war. 

#### <a name="conditional-access"></a>Bedingter Zugriff

Azure AD bestimmt als Autorisierungsserver für Ihre Anwendungen durch die Auswertung ggf. vorhandener Richtlinien für bedingten Zugriff, die für die Ressource gelten, auf die Sie zuzugreifen versuchen, ob Zugriffstoken ausgestellt werden. Wenn die Richtlinienanforderungen erfüllt sind, werden ein Zugriffstoken und ein aktualisiertes Aktualisierungstoken ausgestellt. Wenn die Richtlinie nicht erfüllt wird, erhält der Benutzer Anweisungen zum Erfüllen der Richtlinie, und/oder er muss zusätzliche Schritte, einschließlich Multi-Factor Authentication (MFA) ausführen.  Nach Abschluss der MFA wird dem resultierenden Aktualisierungstoken der MFA-Anspruch hinzugefügt.  

Im Aktualisierungstoken sammeln sich im Zeitverlauf Ansprüche an. Für einige der Ansprüche gelten Ablauffristen, nach denen sie bei Autorisierungsüberprüfungen nicht mehr berücksichtigt werden. Dies kann manchmal zu unerwarteten Ergebnissen führen. Nehmen wir zum Beispiel an, dass eine Richtlinie für bedingten Zugriff so konfiguriert ist, dass für Authentifizierungsversuche von Extranetadressen MFA erforderlich ist. In diesem Fall erhalten Benutzer beim Zugriff auf eine Ressource aus dem Extranet manchmal nicht die erwartete MFA-Aufforderung. Ein möglicher Grund dafür ist, dass der Benutzer kurz vor Verlassen des Intranets MFA durchgeführt hat. Deshalb enthält sein Zugriffstoken einen gültigen MFA-Anspruch. Dieser MFA-Anspruch erfüllt die Richtlinienanforderung, und daher erhält der Benutzer keine zusätzliche MFA-Anforderung von Azure AD.

#### <a name="token-lifetime-policy"></a>Tokengültigkeitsdauer-Richtlinie

Nicht nur für die einzelnen Ansprüche in einem Token gelten Ablauffristen, sondern auch für die Token selbst. Wie bereits erwähnt, sind abgelaufene Token ein Grund für das Unterbrechen der SSO-Erfahrung. Mit [Tokengültigkeitsdauer-Richtlinien](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) können Sie die Gültigkeitsdauer eines von Azure AD ausgestellten Tokens festlegen. Wie sich aus den obigen Ausführungen schließen lässt, sind die Konturen einer SSO-Sitzung schwieriger zu bestimmen. Beispielsweise können sich funktionsreiche Apps als verschiedenartige und scheinbar getrennte Faktoren auf die Gültigkeitsdauer einer SSO-Sitzung auswirken.

>[!NOTE]
>Globale Azure AD-Administratoren können die Gültigkeits- und Inaktivitätszeiträume von Aktualisierungstoken steuern. Im Artikel [Konfigurierbare Tokengültigkeitsdauern in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) finden Sie weitere Informationen zu Einstellungen für Zugriffstoken und Ansprüchen.
>

#### <a name="primary-refresh-tokens"></a>Primäre Aktualisierungstoken

In diesem Artikel wurde bisher die Funktionsweise von SSO im Kontext eines einzelnen Clients beschrieben. Für echtes SSO reicht eine einzelne App jedoch nicht aus. Heutzutage sind Benutzer in interaktive Workflows eingebunden, die mehrere Anwendungen innerhalb der gleichen Anwendungssuite, z.B. Microsoft Office, umfassen. Benutzer benötigen Zugriff auf nicht verwandte Anwendungen, einschließlich intern entwickelter Branchenanwendungen. 

Bislang lässt sich mit in Domänen eingebundenen Windows-Geräten unter Verwendung von integrierter Windows-Authentifizierung (Kerberos) ein hoher Grad von unterbrechungsfreiem SSO über mehrere Anwendungen und Ressourcen hinweg erzielen. Diese Apps umfassen unterstützte Browser, z.B. Internet Explorer und Edge. Für Azure AD bietet das primäre Aktualisierungstoken (Primary Refresh Token, PRT) einen analogen Mechanismus. 

Dieses privilegierte PRT-Token ist nur für einen ausgewählten Satz von Cliententitäten (z.B. Plattformsystemkomponenten) verfügbar. Die Entitäten können dann nach vermittelter Authentifizierung Zugriff auf andere Clientanwendungen zulassen, um eine nahtlose SSO-Erfahrung bieten zu können. 

Für Office 365-Benutzer mit [iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)- und [Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)-Geräten wurden zusätzliche Anstrengungen unternommen, um durch das Anwenden von Authentifizierungsbrokerfunktionalität die Anzahl der erforderlichen Authentifizierungen zu verringern. Diese Funktionalität ist in die Apps Microsoft Authenticator und Intune-Unternehmensportal integriert.

>[!NOTE]
>In den Empfehlungen in diesem Dokument wird davon ausgegangen, dass eine der Apps auf den iOS- oder Android-Geräten der Benutzer bereitgestellt wurde. Beispielsweise eine App, die ein Authentifizierungsbroker ist (Microsoft Authenticator oder Intune-Unternehmensportal).
>

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Multi-Factor Authentication (MFA) erhöht den Grad der Vertrauenswürdigkeit des Antragstellers der Authentifizierung, da dieser mehrere Nachweise seiner Identität liefert. Bei den Nachweisen kann es sich um im Voraus festgelegte Geheimnisse handeln, die nur dem Antragsteller und der Authentifizierungsstelle bekannt sind, oder um eine physische Entität, die nur im Besitz des Antragstellers sein kann. MFA wird normalerweise in mehreren Stufen ausgeführt. Zuerst wird mit einem Kennwort die Identität nachgewiesen, und anschließend ist als zweiter Faktor eine andere Authentifizierungsmethode (die mehr Sicherheit vor bösartigen Angriffen bietet) erforderlich, oder umgekehrt.

Das Konzept von MFA oder strenger Authentifizierung kann je nach Authentifizierungsstelle geringfügige Unterschiede aufweisen. Beispielsweise begreifen manche Authentifizierungsstellen (bzw. die Administratoren, von denen die Richtlinie für die Authentifizierungsstelle konfiguriert wird) die auf der physischen Smartcard basierende Authentifizierung als MFA. Dies kann vorkommen, obwohl die Authentifizierung mit Smartcard streng genommen eine einstufige Authentifizierung ist. 

Die Kombination aus physischer Smartcard und Eingabe einer PIN (Geheimnis) zum Verwenden der Smartcard kann als MFA interpretiert werden. Manche Authentifizierungsstellen stellen möglicherweise geringere Anforderungen bezüglich der Häufigkeit, mit der lästigere Authentifizierungsmethoden ausgeführt werden müssen. Deshalb werden in solchen Fällen normale Authentifizierungen, die zwischen strengeren Authentifizierungen erfolgen, für Ressourcen akzeptiert, die in der Regel strenge Authentifizierung erfordern. Beispielsweise ist es in manchen Organisationen eventuell zulässig, von einem Benutzer MFA in Abständen von einigen Stunden oder Tagen zu verlangen. Die Zeitspanne hängt von der Vertraulichkeit der zu schützenden Ressourcen und davon ab, ob der Benutzer vom gleichen physischen Standort auf eine Ressource zuzugreifen versucht.

Bei Azure AD und AD FS gibt der MFA-Anspruch an, ob die Authentifizierung mit MFA erfolgt. Azure AD stellt standardmäßig Token mit MFA-Anspruch aus, wenn die Authentifizierung mit [Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud) oder [Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification) erfolgt. In Verbundszenarien erkennt Azure AD den MFA-Anspruch von Verbundidentitätsanbietern, z.B. von AD FS, an und übernimmt den MFA-Anspruch in die Token. 



## <a name="recommended-client-configuration-for-sso-and-conditional-access"></a>Empfohlene Clientkonfiguration für SSO und bedingten Zugriff
In diesem Abschnitt werden die empfohlenen Standardplattform-Clientkonfigurationen beschrieben, die Ihren Benutzern eine optimale SSO-Erfahrung bieten, sowie die technischen Voraussetzungen für bedingten Zugriff erläutert.

### <a name="windows-devices"></a>Windows-Geräte
Es wird Windows 10 (Version 1703 oder höher) empfohlen, da Azure für lokale Umgebungen und Azure AD eine besonders nahtlose SSO-Erfahrung bietet. Vom Arbeitgeber oder der Bildungseinrichtung ausgegebene Geräte sollten für den direkten Beitritt zu Azure AD konfiguriert werden. Wenn die Organisation den Beitritt zu einer lokalen AD-Domäne verwendet, sollten diese Geräte [für die automatische und unbeaufsichtigte Registrierung bei Azure AD konfiguriert](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup) werden. 

Für BYOD-Windows-Geräte können Benutzer die Option „Geschäfts-, Schul- oder Unikonto hinzufügen“ verwenden. Für BYOD-Windows-Geräte können Benutzer die Option „Geschäfts-, Schul- oder Unikonto hinzufügen“ verwenden. Beachten Sie, dass Benutzer des Browsers Chrome unter Windows 10 [eine Erweiterung installieren](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji?utm_source=chrome-app-launcher-info-dialog) müssen, damit sie sich so unkompliziert wie bei Verwendung von Edge/IE anmelden können. Wenn Ihre Organisation über Windows 7-Geräte verfügt, die Mitglied einer Domäne sind, können Sie für die Computer das Paket „[Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/download/details.aspx?id=53554)“ installieren, um die Geräte bei Azure AD zu registrieren.

### <a name="ios-devices"></a>iOS-Geräte

Wir empfehlen, die [Microsoft Authenticator-App](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to) auf den Geräten der Benutzer zu installieren, bevor Sie Richtlinien für bedingten Zugriff oder MFA-Richtlinien bereitstellen. Die App sollte unbedingt installiert werden, wenn Benutzer [aufgefordert werden, ihr Gerät bei Azure AD zu registrieren](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user-first-time), indem sie ein Geschäfts-, Schul- oder Unikonto hinzufügen, oder wenn sie die Intune-Unternehmensportal-App installieren, um ihr Gerät bei der Verwaltung zu registrieren. Dies hängt von der Konfiguration der Richtlinie für bedingten Zugriff auf.

### <a name="android-devices"></a>Android-Geräte

Es wird empfohlen, dass Benutzer die [Intune-Unternehmensportal-App](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal&hl=en
) und die [Microsoft Authenticator-App](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to) installieren, bevor Richtlinien für bedingten Zugriff bereitgestellt werden oder wenn diese für bestimmte Authentifizierungsversuche erforderlich sind. Nach der Installation der App werden die Benutzer möglicherweise aufgefordert, sich bei Azure AD zu registrieren oder ihr Gerät bei Intune zu registrieren. Dies hängt von der Konfiguration der Richtlinie für bedingten Zugriff auf. 

Es wird außerdem empfohlen, unternehmenseigene Geräte (Corporate-Owned Devices, CODs) bezüglich OEMs und Versionen zu standardisieren, die Android for Work oder Samsung Knox unterstützen, damit E-Mail-Konten mit der Intune-MDM-Richtlinie verwaltet und geschützt werden können.


### <a name="recommended-email-clients"></a>Empfohlene E-Mail-Clients
Die folgenden E-Mail-Clients unterstützen moderne Authentifizierung und bedingten Zugriff. Azure Information Protection ist noch nicht für alle Clients verfügbar.

|Plattform|Client-|Version/Hinweise|Azure Information Protection|
|:-------|:-----|:------------|:--------------------|
|**Windows**|Outlook|2016, 2013 ([ermöglichen moderne Authentifizierung]((https://support.office.com/article/Enable-Modern-Authentication-for-Office-2013-on-Windows-devices-7dc1c01a-090f-4971-9677-f1b192d6c910)))|Ja|
|**iOS**|Outlook|[Neueste](https://itunes.apple.com/us/app/microsoft-outlook-email-and-calendar/id951937596?mt=8)|Nein|
|**Android**|Outlook|[Neueste](https://play.google.com/store/apps/details?id=com.microsoft.office.outlook&hl=en)|Nein|
|**macOS**|Unterstützung in Kürze verfügbar||Nein|
|**Linux**|Nicht unterstützt||Nein|

#### <a name="additional-client-software"></a>Zusätzliche Clientsoftware
Für den Zugriff auf mit Azure Information Protection geschützte Dokumente ist möglicherweise zusätzliche Software erforderlich. Achten Sie darauf, dass Sie zum Erstellen und Anzeigen geschützter Dokumente mit Azure Information Protection [unterstützte Software und Dokumentformate](https://docs.microsoft.com/information-protection/get-started/requirements-applications) verwenden.


### <a name="security-guidelines"></a>Sicherheitsrichtlinien

Dieser Abschnitt enthält allgemeine Sicherheitsrichtlinien, die beim Implementieren jeglicher in den nachfolgenden Abschnitten beschriebenen Empfehlungen befolgt werden sollten.

#### <a name="security-and-productivity-trade-offs"></a>Abwägung zwischen Sicherheit und Produktivität

Zwischen den Anforderungen von Sicherheit und Produktivität muss eine Abwägung vorgenommen werden. Für das bessere Verständnis dieser Abwägung wird häufig eine Trias verwendet, die aus den drei Elementen „Sicherheit“, „Funktionalität“ und „Benutzerfreundlichkeit“ (Security-Functionality-Usability/Ease of Use, SFU) besteht:

![Abwägung zwischen Sicherheit und Produktivität](./media/secure-email/security-triad.png)

Die Empfehlungen in diesem Dokument basieren entsprechend dem Modell der SFU-Trias auf den folgenden Prinzipien: 

* Berücksichtigen Sie die Zielgruppe – gehen Sie flexibel nach beruflicher Funktion/Sicherheitsstufe vor.
* Wenden Sie die Sicherheitsrichtlinien bedarfsabhängig an, und stellen Sie sicher, dass sie sinnvoll sind.

#### <a name="administrators-versus-users"></a>Administratoren und Benutzer

Es wird empfohlen, Sicherheitsgruppen zu erstellen, die alle Benutzer enthalten, die über Administratorkonten verfügen oder berechtigt sind, temporäre Administratorkontoberechtigungen zu erhalten. Diese Sicherheitsgruppen werden zum Definieren von speziellen Richtlinien für bedingten Zugriff für Azure AD- und Office 365-Administratoren verwendet.  

In den Richtlinienempfehlungen werden die einem Konto zugeordneten Berechtigungen berücksichtigt. [Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) verfügen über deutlich mehr Berechtigungen bezüglich Exchange Online und Office 365. Deshalb sind unsere Richtlinienempfehlungen für diese Konten strenger als für reguläre Benutzerkonten. Alle Richtlinienempfehlungen, die sich auf Administratoren beziehen, gelten für Office 365-Administratorkonten.

#### <a name="reduce-the-number-of-accounts-with-persistent-admin-access"></a>Reduzieren Sie die Anzahl der Konten mit permanentem Administratorzugriff

Verwenden Sie Azure AD Privileged Identity Management, um die Anzahl der permanenten Administratorkonten zu reduzieren. Darüber hinaus wird empfohlen, dass Office 365-Administratoren über ein eigenes Benutzerkonto für reguläre nicht administrative Aufgaben verfügen und ihr Administratorkonto nur verwenden, wenn dies für eine Aufgabe im Rahmen ihrer Tätigkeit erforderlich ist.


## <a name="tiers-of-security-and-protection"></a>Sicherheits- und Schutzebenen
In den meisten Organisationen bestehen bestimmte Anforderungen an Sicherheit und Datenschutz. Diese Anforderungen variieren nach Branchensegment und Tätigkeiten in den Organisationen. Beispielsweise erfordern die Rechtsabteilung und die Office 365-Administratoren eventuell mehr Kontrollen von Sicherheit und Datenschutz bei ihrer E-Mail-Korrespondenz als Benutzer in anderen Geschäftsbereichen. 

Außerdem gibt es in jeder Branche einen eigenen Satz spezieller Vorschriften. Statt einer Liste aller möglichen Sicherheitsoptionen oder einer Empfehlung pro Branchensegment oder Tätigkeit wurden Empfehlungen für drei Ebenen von Sicherheit und Schutz bereitgestellt, die entsprechend Ihren Anforderungen angewendet werden können: [„Baseline“, „Sensibel“ und „Stark reglementiert“](https://go.microsoft.com/fwlink/p/?linkid=841656).  

**Baseline**. Es wird empfohlen, einen Mindeststandard für den Schutz der Daten sowie Identitäten und Geräte festzulegen, die auf die Daten zugreifen. Die Befolgung von Baseline-Empfehlungen bietet einen starken Standardschutz, der die Anforderungen vieler Organisationen erfüllt. 

**Sensibel**. Manche Kunden benötigen für eine Teilmenge ihrer Daten oder für ihre gesamten Daten einen höheren Schutz. Sie können stärkeren Schutz auf alle oder bestimmte Datasets in Ihrer Office 365-Umgebung anwenden. Es wird empfohlen, Identitäten und Geräte, die auf sensible Daten zugreifen, mit vergleichbaren Sicherheitsstufen zu schützen. 

**Stark reglementiert**. Manche Organisationen verfügen eventuell über eine sehr geringe Menge von Daten, die streng geheim sind, unter das Geschäftsgeheimnis fallen oder strengen Regeln unterliegen. Microsoft bietet Funktionen, die Organisationen diese Anforderungen zu erfüllen helfen, darunter zusätzlichen Schutz für Identitäten und Geräte. 

### <a name="default-protection-mechanism-recommendations"></a>Standardempfehlungen für Schutzmechanismen
Die folgende Tabelle enthält für jede der zuvor definierten Ebenen von Sicherheit und Schutz Standardempfehlungen für die Schutzmechanismen:

|Schutzmechanismus|Grundwert|Sensibel|Stark reglementiert|
|:-------------------|:-------|:--------|:---------------|
|**MFA erzwingen**|Bei mittlerem oder höheren Anmelderisiko|Bei niedrigem oder höheren Anmelderisiko|Bei allen neuen Sitzungen|
|**Kennwortänderung erzwingen**|Für stark gefährdete Benutzer|Für stark gefährdete Benutzer|Für stark gefährdete Benutzer|
|**Intune-Anwendungsschutz erzwingen**|Ja|Ja|Ja|
|**Intune-Registrierung erzwingen (unternehmenseigene Geräte)**|Kompatibles oder in eine Domäne eingebundenes Gerät erforderlich|Kompatibles oder in eine Domäne eingebundenes Gerät erforderlich|Kompatibles oder in eine Domäne eingebundenes Gerät erforderlich|

### <a name="device-ownership"></a>Gerätebesitz
Die oben dargestellte Tabelle entspricht dem Trend in vielen Organisationen, eine Mischung aus unternehmenseigenen Geräten (Corporate-Owned Devices, CODs) und persönlichen oder BYOD-Geräten (Bring Your Own Device) zu unterstützen, um die mobile Produktivität ihrer Arbeitskräfte zu ermöglichen. Intune-App-Schutzrichtlinien stellen sicher, dass E-Mails sowohl auf CODs als auch auf BYOD-Geräten nicht aus der mobilen Outlook-App und anderen mobilen Office-Apps extrahiert werden.  

Unternehmenseigene Geräte müssen von Intune verwaltet werden oder in eine Domäne eingebunden sein, um zusätzlichen Schutz und zusätzliche Kontrolle anzuwenden.  Abhängig von der Vertraulichkeit der Daten kann die Organisation BYOD-Geräte für bestimmte Benutzergruppen oder bestimmte Apps untersagen.


## <a name="next-steps"></a>Nächste Schritte
[E-Mail-Richtlinien und Konfigurationen](secure-email-recommended-policies.md)

