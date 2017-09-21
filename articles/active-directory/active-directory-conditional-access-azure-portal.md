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
ms.date: 09/10/2017
ms.author: markvi
ms.reviewer: calebb
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 19bc7abbbf7e133018b234399d91604dfdbfe73f
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="conditional-access-in-azure-active-directory"></a>Bedingter Zugriff in Azure Active Directory

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

- „**When this happens**“ wird als **Bedingungsanweisung ** bezeichnet.
- „**Then do this**“ wird als **Kontrolle** bezeichnet.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/11.png)

Die Kombination aus Bedingungsanweisung und Ihren Kontrollen ergibt eine Richtlinie für den bedingten Zugriff.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Kontrollen

Bei einer Richtlinie für den bedingten Zugriff wird mit Kontrollen definiert, was passieren soll, wenn eine Bedingungsanweisung erfüllt wurde.  
Mit Kontrollen können Sie den Zugriff entweder blockieren oder unter Erfüllung zusätzlicher Anforderungen zulassen.
Wenn Sie eine Richtlinie konfigurieren, mit der der Zugriff zugelassen wird, müssen Sie mindestens eine Anforderung auswählen.  

Es gibt zwei Arten von Kontrollen: 

- **Gewährungssteuerelemente**: Mit Gewährungssteuerelementen steuern Sie, ob ein Benutzer die Authentifizierung durchführen und auf die Ressource zugreifen kann, bei der er sich anmelden möchte. Wenn Sie mehrere Steuerelemente ausgewählt haben, können Sie konfigurieren, ob bei der Verarbeitung der Richtlinie alle erforderlich sind.
Bei der aktuellen Implementierung von Azure Active Directory können Sie die folgenden Anforderungen für Gewährungssteuerelemente konfigurieren:

    ![Kontrolle](./media/active-directory-conditional-access-azure-portal/05.png)

- **Sitzungssteuerelemente**: Sitzungssteuerelemente ermöglichen das Einschränken der Benutzeroberfläche innerhalb einer Cloud-App. Die Sitzungssteuerelemente werden von Cloud-Apps erzwungen und verlassen sich auf zusätzliche Informationen über die Sitzung, die der App von Azure AD bereitgestellt werden.

    ![Kontrolle](./media/active-directory-conditional-access-azure-portal/31.png)


Weitere Informationen finden Sie unter [Steuerelemente beim bedingten Zugriff mit Azure Active Directory](active-directory-conditional-access-controls.md).


## <a name="condition-statement"></a>Bedingungsanweisung

Im vorherigen Abschnitt wurden die unterstützten Optionen zum Blockieren oder Einschränken des Zugriffs auf Ihre Ressourcen in Form von Kontrollen beschrieben. In einer Richtlinie für den bedingten Zugriff definieren Sie die Kriterien, die erfüllt sein müssen, damit die Kontrollen angewendet werden, in Form einer Bedingungsanweisung.  

Sie können die folgenden Zuweisungen in die Bedingungsanweisung einbinden:

![Kontrolle](./media/active-directory-conditional-access-azure-portal/07.png)


### <a name="who"></a>Wer?

Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, müssen Sie die Benutzer oder Gruppen auswählen, für die Ihre Richtlinie gilt. In vielen Fällen sollen die Kontrollen auf eine bestimmte Gruppe von Benutzern angewendet werden. In einer Bedingungsanweisung können Sie diese Gruppe definieren, indem Sie die Benutzer und Gruppen auswählen, für die Ihre Richtlinie gilt. Bei Bedarf können Sie eine Gruppe von Benutzern auch explizit aus der Richtlinie ausschließen.  

![Kontrolle](./media/active-directory-conditional-access-azure-portal/08.png)



### <a name="what"></a>Was?

Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, müssen Sie die Cloud-Apps auswählen, für die Ihre Richtlinie gilt.
Normalerweise erfordern einige Apps in Ihrer Umgebung aus Sicht des Schutzes mehr Aufmerksamkeit als andere Apps. Dies können beispielsweise Apps sein, über die Zugriff auf sensible Daten besteht.
Durch die Auswahl von „Cloud-Apps“ definieren Sie den Umfang der Cloud-Apps, für die die Richtlinie gilt. Bei Bedarf können Sie auch eine bestimmte Gruppe von Apps aus der Richtlinie ausschließen.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/09.png)

Eine vollständige Liste der Cloud-Apps, die Sie in der Richtlinie für bedingten Zugriff verwenden können, bietet Ihnen die [Technische Referenz zum bedingten Azure Active Directory-Zugriff](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).

### <a name="how"></a>Vorgehensweise

Solange der Zugriff auf Ihre Apps unter Bedingungen erfolgt, die Sie kontrollieren können, ist es ggf. nicht erforderlich, weitere Kontrollen für den Zugriff auf Cloud-Apps durch Ihre Benutzer einzubauen. Es sieht aber schon anders aus, wenn der Zugriff auf Ihre Cloud-Apps beispielsweise über Netzwerke, die nicht vertrauenswürdig sind, oder über nicht konforme Geräte erfolgt. In einer Bedingungsanweisung können Sie bestimmte Zugriffsbedingungen definieren, für die zusätzliche Anforderungen in Bezug auf die Durchführung des Zugriffs auf Ihre Apps gelten.

![Bedingungen](./media/active-directory-conditional-access-azure-portal/21.png)


## <a name="conditions"></a>Bedingungen

In der aktuellen Implementierung von Azure Active Directory können Sie Bedingungen für die folgenden Bereiche definieren:

- Anmelderisiko
- Geräteplattformen
- Standorte
- Client-Apps

![Bedingungen](./media/active-directory-conditional-access-azure-portal/21.png)

### <a name="sign-in-risk"></a>Anmelderisiko

Ein Anmelderisiko ist ein Objekt, mit dem Azure Active Directory die Wahrscheinlichkeit dafür ermittelt, dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. In diesem Objekt wird die Wahrscheinlichkeit (hoch, mittel oder niedrig) in Form eines Attributs mit dem Namen [Risikostufe für die Anmeldung](active-directory-reporting-risk-events.md#risk-level) gespeichert. Dieses Objekt wird bei einer Anmeldung eines Benutzers generiert, wenn von Azure Active Directory Anmelderisiken erkannt wurden. Weitere Informationen finden Sie unter [Riskante Anmeldungen](active-directory-identityprotection.md#risky-sign-ins).  
Sie können die berechnete Anmelderisikostufe als Bedingung in einer Richtlinie für den bedingten Zugriff verwenden. 

![Bedingungen](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Geräteplattformen

Die Geräteplattform ist durch das Betriebssystem gekennzeichnet, das auf dem Gerät ausgeführt wird:

- Android
- iOS
- Windows Phone
- Windows
- macOS (Vorschau) 

![Bedingungen](./media/active-directory-conditional-access-azure-portal/02.png)

Sie können die Geräteplattformen angeben, die in eine Richtlinie einbezogen bzw. davon ausgeschlossen werden.  
Ändern Sie zum Verwenden von Geräteplattformen in der Richtlinie zuerst die Option „Konfigurieren“ in **Ja**, und wählen Sie dann einige oder alle Geräteplattformen aus, für die die Richtlinie gelten soll. Wenn Sie nur eine Geräteplattform auswählen, gilt die Richtlinie nur für die jeweilige Plattform. In diesem Fall sind Anmeldungen an anderen unterstützten Plattformen von der Richtlinie nicht betroffen.


### <a name="locations"></a>Standorte

Der Standort wird durch die IP-Adresse des Clients identifiziert, den Sie verwendet haben, um eine Verbindung mit Azure Active Directory herzustellen. Diese Bedingung erfordert, dass Sie sich mit **benannten Orten** und **durch MFA bestätigten IPs** auskennen.  

**Benannte Orte** ist ein Feature von Azure Active Directory, das Ihnen ermöglicht, in Ihrer Organisation vertrauenswürdige IP-Adressbereiche zu bezeichnen. Sie können in Ihrer Umgebung benannte Orte sowohl im Kontext der Erkennung von [Risikoereignissen](active-directory-reporting-risk-events.md) als auch für bedingten Zugriff verwenden. Weitere Informationen zum Konfigurieren von benannten Orten in Azure Active Directory finden Sie unter [Benannte Orte in Azure Active Directory](active-directory-named-locations.md).

Die Anzahl von Orten, die Sie konfigurieren können, wird durch die Größe des verbundenen Objekts in Azure AD eingeschränkt. Sie können Folgendes konfigurieren:
 
 - Einen benannten Ort mit bis zu 500 IP-Adressbereichen
 - Maximal 60 benannte Orte (Vorschau), wobei jedem ein IP-Adressbereich zugewiesen wird. 


**Durch MFA bestätigte IPs** ist ein Feature der Multi-Factor Authentication, mit dem Sie Bereiche mit vertrauenswürdigen IP-Adressen definieren können, die für das lokale Intranet Ihrer Organisation stehen. Beim Konfigurieren der Bedingungen eines Standorts können Sie mit der Option „Vertrauenswürdige IPs“ zwischen Verbindungen, die über das Netzwerk Ihrer Organisation hergestellt werden, und allen anderen Standorten unterscheiden. Weitere Informationen finden Sie unter [Vertrauenswürdige IP-Adressen](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  



Sie können entweder alle Standorte oder alle vertrauenswürdigen IPs einbinden, und Sie können alle vertrauenswürdigen IPs ausschließen.

![Bedingungen](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-apps"></a>Client-Apps

Die Client-App kann entweder eine allgemeine App (Webbrowser, mobile App, Desktopclient) sein, die Sie zum Herstellen der Verbindung mit Azure Active Directory verwendet haben, oder Sie können die spezifische Option „Exchange Active Sync“ auswählen.  
Die Legacyauthentifizierung bezieht sich auf Clients, für die eine einfache Authentifizierung verwendet wird, z.B. ältere Office-Clients, für die keine moderne Authentifizierung genutzt wird. Für die Legacyauthentifizierung wird der bedingte Zugriff derzeit nicht unterstützt.

![Bedingungen](./media/active-directory-conditional-access-azure-portal/04.png)


Eine vollständige Liste der Client-Apps, die Sie in der Richtlinie für bedingten Zugriff verwenden können, bietet Ihnen die [Technische Referenz zum bedingten Azure Active Directory-Zugriff](active-directory-conditional-access-technical-reference.md#client-apps-condition).



## <a name="common-scenarios"></a>Gängige Szenarien

### <a name="requiring-multi-factor-authentication-for-apps"></a>Erzwingen von Multi-Factor Authentication für Apps

Viele Umgebungen verfügen über Apps, für die eine höhere Schutzebene als für andere Apps erforderlich ist.
Dies gilt beispielsweise für Apps, für die Zugriff auf sensible Daten besteht.
Wenn Sie diesen Apps eine weitere Schutzebene hinzufügen möchten, können Sie eine Richtlinie für den bedingten Zugriff konfigurieren, für die eine Multi-Factor Authentication erforderlich ist, wenn Benutzer auf diese Apps zugreifen.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Erzwingen der Multi-Factor Authentication für den Zugriff aus nicht vertrauenswürdigen Netzwerken

Dieses Szenario ähnelt dem vorherigen Szenario, da hiermit eine Anforderung für die Multi-Factor Authentication hinzugefügt wird.
Der Hauptunterschied ist aber die Bedingung für diese Anforderung.  
Während der Schwerpunkt des vorherigen Szenarios auf Apps mit Zugriff auf sensible Daten lag, geht es in diesem Szenario nun um vertrauenswürdige Standorte.  
Anders ausgedrückt: Sie können beispielsweise die Multi-Factor Authentication erzwingen, wenn auf eine App von einem Benutzer über ein Netzwerk zugegriffen wird, das für Sie nicht vertrauenswürdig ist.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Nur mit vertrauenswürdigen Geräten kann auf Office 365-Dienste zugegriffen werden

Wenn Sie Intune in Ihrer Umgebung verwenden, können Sie sofort damit beginnen, die Oberfläche für Richtlinien für den bedingten Zugriff in der Azure-Konsole zu verwenden.

Viele Intune-Kunden nutzen den bedingten Zugriff, um sicherzustellen, dass nur vertrauenswürdige Geräte auf Office 365-Dienste zugreifen können. Dies bedeutet, dass mobile Geräte bei Intune registriert werden und die Anforderungen der Konformitätsrichtlinien erfüllen und dass Windows-PCs in eine lokale Domäne eingebunden werden. Eine wichtige Verbesserung besteht darin, dass Sie eine Richtlinie nicht für jeden Office 365-Dienst neu festlegen müssen.  Konfigurieren Sie die Cloud-Apps beim Erstellen einer neuen Richtlinie so, dass alle O365-Apps einbezogen werden, die Sie mit bedingtem Zugriff schützen möchten.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 

