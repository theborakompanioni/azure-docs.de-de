---
title: Bedingter Zugriff mit Azure Active Directory | Microsoft Docs
description: "Verwenden Sie die Steuerung des bedingten Zugriffs in Azure Active Directory, um bei der Authentifizierung für den Zugriff auf Anwendungen eine Überprüfung auf bestimmte Bedingungen durchzuführen."
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: d4b38f1f90301c971cb6000f46d831047acb220e
ms.openlocfilehash: 5a1ce66e02943caedd52976c5dcb3cf75c23bd49
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# Bedingter Zugriff in Azure Active Directory
<a id="conditional-access-in-azure-active-directory" class="xliff"></a>

> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-conditional-access-azure-portal.md)
> * [klassischen Azure-Portal](active-directory-conditional-access.md)

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory das einmalige Anmelden an Geräten, Apps und Diensten an jedem Ort. Aufgrund der steigenden Zahl von Geräten (z.B. „BYOD“), das Arbeiten außerhalb von Unternehmensnetzwerken und der Nutzung von SaaS-Apps von Drittanbietern, gelten für IT-Spezialisten zwei gegensätzliche Zielsetzungen:

- Schaffen von Bedingungen für Endbenutzer, unter denen sie an jedem Ort und zu jeder Zeit produktiv sein können
- Lückenloses Schützen der Assets eines Unternehmens

Zur Verbesserung der Produktivität verfügt Azure Active Directory über viele Optionen für Ihre Benutzer, mit denen sie auf die Assets des Unternehmens zugreifen können. Durch die Verwaltung des Anwendungszugriffs können Sie per Azure Active Directory sicherstellen, dass nur *die richtigen Personen* auf Ihre Anwendungen zugreifen können. Was kann getan werden, wenn Sie mehr Kontrolle darüber haben möchten, wie die richtigen Personen unter bestimmten Bedingungen auf Ihre Ressourcen zugreifen können? Welche Möglichkeiten haben Sie, wenn es Fälle gibt, in denen Sie den Zugriff auf bestimmte Apps auch für die *richtigen Personen* blockieren möchten? Es kann beispielsweise gewollt sein, dass die richtigen Personen über ein vertrauenswürdiges Netzwerk auf bestimmte Apps zugreifen. Aber es kann unerwünscht sein, dass der Zugriff auf diese Apps über ein Netzwerk erfolgt, das für Sie nicht vertrauenswürdig ist. Sie können diese Fragen mithilfe des bedingten Zugriffs beantworten.

Der bedingte Zugriff ist eine Funktion von Azure Active Directory, mit der Sie die Kontrolle des Zugriffs für Apps in Ihrer Umgebung basierend auf bestimmten Bedingungen erzwingen können. Im Rahmen der Kontrolle können Sie entweder weitere Anforderungen mit dem Zugriff verknüpfen oder den Zugriff blockieren. Die Implementierung des bedingten Zugriffs basiert auf Richtlinien. Ein auf Richtlinien basierender Ansatz führt zu einer Vereinfachung der Konfigurationsbenutzeroberfläche, da die Vorgehensweise dann an Ihren Zugriffsanforderungen ausgerichtet ist.  

Normalerweise definieren Sie die Zugriffsanforderungen, indem Sie Anweisungen definieren, die auf dem folgenden Muster basieren:

![Kontrolle](./media/active-directory-conditional-access-azure-portal/10.png)

Wenn Sie in der obigen Abbildung „*this*“ jeweils durch echte Informationen ersetzen, entsteht ein Beispiel für eine Richtlinienanweisung, die Ihnen vielleicht bekannt vorkommt:

*Wenn Auftragnehmer versuchen, über nicht vertrauenswürdige Netzwerke auf unsere Cloud-Apps zuzugreifen, wird der Zugriff blockiert.*

Die obige Richtlinienanweisung verdeutlicht die Nützlichkeit des bedingten Zugriffs. Sie können Auftragnehmern grundsätzlich Zugriff auf Ihre Cloud-Apps gewähren (**wer**), und mit dem bedingten Zugriff können Sie dann zusätzlich Bedingungen definieren, unter denen der Zugriff möglich ist (**wie**).

Im Zusammenhang mit dem bedingten Zugriff per Azure Active Directory gilt Folgendes:

- „**When this happens**“ wird als **Bedingungsanweisung**  bezeichnet.
- „**Then do this**“ wird als **Kontrolle** bezeichnet.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/11.png)

Die Kombination aus Bedingungsanweisung und Ihren Kontrollen ergibt eine Richtlinie für den bedingten Zugriff.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/12.png)


## Kontrollen
<a id="controls" class="xliff"></a>

Bei einer Richtlinie für den bedingten Zugriff wird mit Kontrollen definiert, was passieren soll, wenn eine Bedingungsanweisung erfüllt wurde.  
Mit Kontrollen können Sie den Zugriff entweder blockieren oder unter Erfüllung zusätzlicher Anforderungen zulassen.
Wenn Sie eine Richtlinie konfigurieren, mit der der Zugriff zugelassen wird, müssen Sie mindestens eine Anforderung auswählen.   

### Gewährungssteuerelemente
<a id="grant-controls" class="xliff"></a>
Bei der aktuellen Implementierung von Azure Active Directory können Sie die folgenden Anforderungen für Gewährungssteuerelemente konfigurieren:

![Kontrolle](./media/active-directory-conditional-access-azure-portal/05.png)

- **Multi-Factor Authentication**: Sie können über die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) eine sichere Authentifizierung erzwingen. Als Anbieter können Sie Azure Multi-Factor oder einen lokalen Multi-Factor Authentication-Anbieter in Kombination mit Active Directory-Verbunddienste (AD FS) verwenden. Die mehrstufige Authentifizierung unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der möglicherweise Zugriff auf die Anmeldeinformationen eines gültigen Benutzers erlangt hat.

- **Konformes Gerät**: Sie können die Richtlinien für den bedingten Zugriff auf Geräteebene festlegen. Sie können eine Richtlinie einrichten, bei der nur konforme Computer oder mobile Geräte, die in einer Anwendung zur Verwaltung mobiler Geräte registriert sind, auf Ihre Organisationsressourcen zugreifen können. Beispielsweise können Sie Intune verwenden, um die Gerätekompatibilität zu überprüfen und das Ergebnis zur Richtlinienerzwingung an Azure AD übermitteln, wenn der Benutzer versucht, auf eine Anwendung zuzugreifen. Ausführliche Anleitungen dazu, wie Intune zum Schutz von Apps und Daten eingesetzt werden kann, finden Sie unter „Schützen von Apps und Daten mit Microsoft Intune“. Sie können mithilfe von Intune auch Datenschutz für verlorene oder gestohlene Geräte erzwingen. Weitere Informationen finden Sie unter „Schützen Ihrer Daten mit vollständigem oder selektivem Löschen über Microsoft Intune“.

- **In die Domäne eingebundenes Gerät**: Sie können festlegen, dass das Gerät, das Sie zum Herstellen der Verbindung mit Azure Active Directory verwendet haben, ein in die Domäne eingebundenes Gerät sein muss. Diese Richtlinie gilt für Windows-Desktops, -Laptops und -Unternehmenstablets. Weitere Informationen zum Einrichten der automatischen Registrierung von in die Domäne eingebundenen Geräten bei Azure AD finden Sie unter [Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte](active-directory-conditional-access-automatic-device-registration.md).

Wenn Sie in einer Richtlinie für den bedingten Zugriff mehr als eine Anforderung ausgewählt haben, können Sie auch die Anforderungen für deren Anwendung konfigurieren. Sie können auswählen, ob nur eine oder alle ausgewählten Kontrollen erforderlich sind.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/06.png)

### Sitzungssteuerelemente
<a id="session-controls" class="xliff"></a>
Sitzungssteuerelemente ermöglichen das Einschränken der Benutzeroberfläche innerhalb einer Cloud-App. Die Sitzungssteuerelemente werden von Cloud-Apps erzwungen und verlassen sich auf zusätzliche Informationen über die Sitzung, die der App von Azure AD bereitgestellt werden.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/session-control-pic.png)

#### Verwenden von App-erzwungenen Einschränkungen
<a id="use-app-enforced-restrictions" class="xliff"></a>
Sie können dieses Steuerelement verwenden, um von Azure AD anzufordern, die Geräteinformationen an die Cloud-App zu übergeben. Dadurch erfährt die Cloud-App, ob der Benutzer von einem konformen Gerät oder einem in eine Domäne eingebundenen Gerät stammt. Dieses Steuerelement wird derzeit nur mit SharePoint als Cloud-App unterstützt. SharePoint verwendet die Geräteinformationen dazu, Benutzern eine eingeschränkte oder vollständige Benutzeroberfläche zur Verfügung zu stellen, je nach Gerätezustand.
Weitere Informationen zum Anfordern von eingeschränktem Zugriff mit SharePoint finden Sie [hier](https://aka.ms/spolimitedaccessdocs).

## Bedingungsanweisung
<a id="condition-statement" class="xliff"></a>

Im vorherigen Abschnitt wurden die unterstützten Optionen zum Blockieren oder Einschränken des Zugriffs auf Ihre Ressourcen in Form von Kontrollen beschrieben. In einer Richtlinie für den bedingten Zugriff definieren Sie die Kriterien, die erfüllt sein müssen, damit die Kontrollen angewendet werden, in Form einer Bedingungsanweisung.  

Sie können die folgenden Zuweisungen in die Bedingungsanweisung einbinden:

![Kontrolle](./media/active-directory-conditional-access-azure-portal/07.png)


- **Wer**: In vielen Fällen sollen die Kontrollen auf eine bestimmte Gruppe von Benutzern angewendet werden. In einer Bedingungsanweisung können Sie diese Gruppe definieren, indem Sie die Benutzer und Gruppen auswählen, für die Ihre Richtlinie gilt. Bei Bedarf können Sie eine Gruppe von Benutzern auch explizit aus der Richtlinie ausschließen.  
Durch die Auswahl der Option „Benutzer und Gruppen“ definieren Sie die Benutzergruppe, für die die Richtlinie gilt.    

    ![Kontrolle](./media/active-directory-conditional-access-azure-portal/08.png)



- **Was**: Normalerweise erfordern einige Apps, die in Ihrer Umgebung ausgeführt werden, aus Sicht des Schutzes mehr Aufmerksamkeit als andere Apps. Dies können beispielsweise Apps sein, über die Zugriff auf sensible Daten besteht.
Durch die Auswahl von „Cloud-Apps“ definieren Sie den Umfang der Cloud-Apps, für die die Richtlinie gilt. Bei Bedarf können Sie auch eine bestimmte Gruppe von Apps aus der Richtlinie ausschließen.

    ![Kontrolle](./media/active-directory-conditional-access-azure-portal/09.png)


- **Wie**: Solange der Zugriff auf Ihre Apps unter Bedingungen erfolgt, die Sie kontrollieren können, ist es ggf. nicht erforderlich, weitere Kontrollen für den Zugriff auf Cloud-Apps durch Ihre Benutzer einzubauen. Es sieht aber schon anders aus, wenn der Zugriff auf Ihre Cloud-Apps beispielsweise über Netzwerke, die nicht vertrauenswürdig sind, oder über nicht konforme Geräte erfolgt. In einer Bedingungsanweisung können Sie bestimmte Zugriffsbedingungen definieren, für die zusätzliche Anforderungen in Bezug auf die Durchführung des Zugriffs auf Ihre Apps gelten.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/21.png)


## Bedingungen
<a id="conditions" class="xliff"></a>

In der aktuellen Implementierung von Azure Active Directory können Sie Bedingungen für die folgenden Bereiche definieren:

- **Anmelderisiko:** Das Anmelderisiko ist ein Objekt, mit dem Azure Active Directory die Wahrscheinlichkeit dafür ermittelt, dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. In diesem Objekt wird die Wahrscheinlichkeit (hoch, mittel oder niedrig) in Form eines Attributs mit dem Namen [Risikostufe für die Anmeldung](active-directory-reporting-risk-events.md#risk-level) gespeichert. Dieses Objekt wird bei einer Anmeldung eines Benutzers generiert, wenn von Azure Active Directory Anmelderisiken erkannt wurden. Weitere Informationen finden Sie unter [Riskante Anmeldungen](active-directory-identityprotection.md#risky-sign-ins).  
Sie können die berechnete Anmelderisikostufe als Bedingung in einer Richtlinie für den bedingten Zugriff verwenden. 

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/22.png)

- **Geräteplattformen**: Die Geräteplattform ist durch das Betriebssystem gekennzeichnet, das auf dem Gerät ausgeführt wird (Android, iOS, Windows Phone, Windows). Sie können die Geräteplattformen angeben, die in eine Richtlinie einbezogen bzw. davon ausgeschlossen werden.  
Ändern Sie zum Verwenden von Geräteplattformen in der Richtlinie zuerst die Option „Konfigurieren“ in **Ja**, und wählen Sie dann einige oder alle Geräteplattformen aus, für die die Richtlinie gelten soll. Wenn Sie nur eine Geräteplattform auswählen, gilt die Richtlinie nur für die jeweilige Plattform. In diesem Fall sind Anmeldungen an anderen unterstützten Plattformen von der Richtlinie nicht betroffen.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/02.png)

- **Standorte**: Der Standort wird durch die IP-Adresse des Clients identifiziert, den Sie verwendet haben, um eine Verbindung mit Azure Active Directory herzustellen. Für diese Bedingung ist es erforderlich, dass Sie sich mit vertrauenswürdigen IPs auskennen. „Vertrauenswürdige IPs“ ist ein Feature der Multi-Factor Authentication, mit dem Sie Bereiche mit vertrauenswürdigen IP-Adressen definieren können, die für das lokale Intranet Ihrer Organisation stehen. Beim Konfigurieren der Bedingungen eines Standorts können Sie mit der Option „Vertrauenswürdige IPs“ zwischen Verbindungen, die über das Netzwerk Ihrer Organisation hergestellt werden, und allen anderen Standorten unterscheiden. Weitere Informationen finden Sie unter [Vertrauenswürdige IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  
Sie können entweder alle Standorte oder alle vertrauenswürdigen IPs einbinden, und Sie können alle vertrauenswürdigen IPs ausschließen.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/03.png)


- **Client-App**: Die Client-App kann entweder eine allgemeine App (Webbrowser, mobile App, Desktopclient) sein, die Sie zum Herstellen der Verbindung mit Azure Active Directory verwendet haben, oder Sie können die spezifische Option „Exchange Active Sync“ auswählen.  
Die Legacyauthentifizierung bezieht sich auf Clients, für die eine einfache Authentifizierung verwendet wird, z.B. ältere Office-Clients, für die keine moderne Authentifizierung genutzt wird. Für die Legacyauthentifizierung wird der bedingte Zugriff derzeit nicht unterstützt.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/04.png)


## Gängige Szenarien
<a id="common-scenarios" class="xliff"></a>

### Erzwingen von Multi-Factor Authentication für Apps
<a id="requiring-multi-factor-authentication-for-apps" class="xliff"></a>

Viele Umgebungen verfügen über Apps, für die eine höhere Schutzebene als für andere Apps erforderlich ist.
Dies gilt beispielsweise für Apps, für die Zugriff auf sensible Daten besteht.
Wenn Sie diesen Apps eine weitere Schutzebene hinzufügen möchten, können Sie eine Richtlinie für den bedingten Zugriff konfigurieren, für die eine Multi-Factor Authentication erforderlich ist, wenn Benutzer auf diese Apps zugreifen.


### Erzwingen der Multi-Factor Authentication für den Zugriff aus nicht vertrauenswürdigen Netzwerken
<a id="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted" class="xliff"></a>

Dieses Szenario ähnelt dem vorherigen Szenario, da hiermit eine Anforderung für die Multi-Factor Authentication hinzugefügt wird.
Der Hauptunterschied ist aber die Bedingung für diese Anforderung.  
Während der Schwerpunkt des vorherigen Szenarios auf Apps mit Zugriff auf sensible Daten lag, geht es in diesem Szenario nun um vertrauenswürdige Standorte.  
Anders ausgedrückt: Sie können beispielsweise die Multi-Factor Authentication erzwingen, wenn auf eine App von einem Benutzer über ein Netzwerk zugegriffen wird, das für Sie nicht vertrauenswürdig ist.


### Nur mit vertrauenswürdigen Geräten kann auf Office 365-Dienste zugegriffen werden
<a id="only-trusted-devices-can-access-office-365-services" class="xliff"></a>

Wenn Sie Intune in Ihrer Umgebung verwenden, können Sie sofort damit beginnen, die Oberfläche für Richtlinien für den bedingten Zugriff in der Azure-Konsole zu verwenden.

Viele Intune-Kunden nutzen den bedingten Zugriff, um sicherzustellen, dass nur vertrauenswürdige Geräte auf Office 365-Dienste zugreifen können. Dies bedeutet, dass mobile Geräte bei Intune registriert werden und die Anforderungen der Konformitätsrichtlinien erfüllen und dass Windows-PCs in eine lokale Domäne eingebunden werden. Eine wichtige Verbesserung besteht darin, dass Sie eine Richtlinie nicht für jeden Office 365-Dienst neu festlegen müssen.  Konfigurieren Sie die Cloud-Apps beim Erstellen einer neuen Richtlinie so, dass alle O365-Apps eingebunden sind, die Sie per bedingtem Zugriff schützen möchten.

## Nächste Schritte
<a id="next-steps" class="xliff"></a>

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

Ausführliche Informationen darüber, was Sie wissen sollten und was Sie beim Konfigurieren von Richtlinien für den bedingten Zugriff vermeiden sollten, finden Sie unter 
