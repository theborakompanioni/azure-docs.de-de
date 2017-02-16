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
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 89d32955506afdfb9c45c5b0ce3d38d40f012e72
ms.openlocfilehash: 570a0f0a0c0932a00bbe92b1e137951ceffcd660


---
# <a name="conditional-access-in-azure-active-directory---preview"></a>Bedingter Zugriff in Azure Active Directory – Vorschau

> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-conditional-access-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-conditional-access.md)


Das in diesem Thema beschriebene Verhalten befindet sich derzeit in der [Vorschauphase](active-directory-preview-explainer.md).

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

Bei der aktuellen Implementierung von Azure Active Directory können Sie die folgenden Anforderungen konfigurieren: 

![Kontrolle](./media/active-directory-conditional-access-azure-portal/05.png)

- **Multi-Factor Authentication**: Sie können über die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) eine sichere Authentifizierung erzwingen. Als Anbieter können Sie Azure Multi-Factor oder einen lokalen Multi-Factor Authentication-Anbieter in Kombination mit Active Directory-Verbunddienste (AD FS) verwenden. Die mehrstufige Authentifizierung unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der möglicherweise Zugriff auf die Anmeldeinformationen eines gültigen Benutzers erlangt hat.

- **Konformes Gerät**: Sie können die Richtlinien für den bedingten Zugriff auf Geräteebene festlegen. Sie können eine Richtlinie einrichten, bei der nur konforme Computer oder mobile Geräte, die in einer Anwendung zur Verwaltung mobiler Geräte registriert sind, auf Ihre Organisationsressourcen zugreifen können. Beispielsweise können Sie Intune verwenden, um die Gerätekompatibilität zu überprüfen und das Ergebnis zur Richtlinienerzwingung an Azure AD übermitteln, wenn der Benutzer versucht, auf eine Anwendung zuzugreifen. Ausführliche Anleitungen dazu, wie Intune zum Schutz von Apps und Daten eingesetzt werden kann, finden Sie unter „Schützen von Apps und Daten mit Microsoft Intune“. Sie können mithilfe von Intune auch Datenschutz für verlorene oder gestohlene Geräte erzwingen. Weitere Informationen finden Sie unter „Schützen Ihrer Daten mit vollständigem oder selektivem Löschen über Microsoft Intune“.

- **In die Domäne eingebundenes Gerät**: Sie können festlegen, dass das Gerät, das Sie zum Herstellen der Verbindung mit Azure Active Directory verwendet haben, ein in die Domäne eingebundenes Gerät sein muss. Diese Richtlinie gilt für Windows-Desktops, -Laptops und -Unternehmenstablets. Weitere Informationen zum Einrichten der automatischen Registrierung von in die Domäne eingebundenen Geräten bei Azure AD finden Sie unter [Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte](active-directory-conditional-access-automatic-device-registration.md).

Wenn Sie in einer Richtlinie für den bedingten Zugriff mehr als eine Anforderung ausgewählt haben, können Sie auch die Anforderungen für deren Anwendung konfigurieren. Sie können auswählen, ob nur eine oder alle ausgewählten Kontrollen erforderlich sind.

![Kontrolle](./media/active-directory-conditional-access-azure-portal/06.png)


## <a name="condition-statement"></a>Bedingungsanweisung

Im vorherigen Abschnitt wurden die unterstützten Optionen zum Blockieren oder Einschränken des Zugriffs auf Ihre Ressourcen in Form von Kontrollen beschrieben. In einer Richtlinie für den bedingten Zugriff definieren Sie die Kriterien, die erfüllt sein müssen, damit die Kontrollen angewendet werden, in Form einer Bedingungsanweisung.  

Sie können die folgenden Zuweisungen in die Bedingungsanweisung einbinden:
    
![Kontrolle](./media/active-directory-conditional-access-azure-portal/07.png)


- **Wer**: In vielen Fällen sollen die Kontrollen auf eine bestimmte Gruppe von Benutzern angewendet werden. In einer Bedingungsanweisung können Sie diese Gruppe definieren, indem Sie die Benutzer und Gruppen auswählen, für die Ihre Richtlinie gilt. Bei Bedarf können Sie eine Gruppe von Benutzern auch explizit aus der Richtlinie ausschließen.  
Durch die Auswahl der Option „Benutzer und Gruppen“ definieren Sie die Benutzergruppe, für die die Richtlinie gilt.    

    ![Kontrolle](./media/active-directory-conditional-access-azure-portal/08.png)



- **Was**: Normalerweise erfordern einige Apps, die in Ihrer Umgebung ausgeführt werden, aus Sicht des Schutzes mehr Aufmerksamkeit als andere Apps. Dies können beispielsweise Apps sein, über die Zugriff auf sensible Daten besteht. Durch die Auswahl von „Cloud-Apps“ definieren Sie den Umfang der Cloud-Apps, für die die Richtlinie gilt. Bei Bedarf können Sie auch eine bestimmte Gruppe von Apps aus der Richtlinie ausschließen. 

    ![Kontrolle](./media/active-directory-conditional-access-azure-portal/09.png)


- **Wie**: Solange der Zugriff auf Ihre Apps unter Bedingungen erfolgt, die Sie kontrollieren können, ist es ggf. nicht erforderlich, weitere Kontrollen für den Zugriff auf Cloud-Apps durch Ihre Benutzer einzubauen. Es sieht aber schon anders aus, wenn der Zugriff auf Ihre Cloud-Apps beispielsweise über Netzwerke, die nicht vertrauenswürdig sind, oder über nicht konforme Geräte erfolgt. In einer Bedingungsanweisung können Sie bestimmte Zugriffsbedingungen definieren, für die zusätzliche Anforderungen in Bezug auf die Durchführung des Zugriffs auf Ihre Apps gelten.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/01.png)


## <a name="conditions"></a>Bedingungen

In der aktuellen Implementierung von Azure Active Directory können Sie Bedingungen für die folgenden Bereiche definieren:


- **Geräteplattformen**: Die Geräteplattform ist durch das Betriebssystem gekennzeichnet, das auf dem Gerät ausgeführt wird (Android, iOS, Windows Phone, Windows). Sie können die Geräteplattformen angeben, die in eine Richtlinie einbezogen bzw. davon ausgeschlossen werden.  
Ändern Sie zum Verwenden von Geräteplattformen in der Richtlinie zuerst die Option „Konfigurieren“ in **Ja**, und wählen Sie dann einige oder alle Geräteplattformen aus, für die die Richtlinie gelten soll. Wenn Sie nur eine Geräteplattform auswählen, gilt die Richtlinie nur für die jeweilige Plattform. In diesem Fall sind Anmeldungen an anderen unterstützten Plattformen von der Richtlinie nicht betroffen. 

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/02.png)

- **Standorte**: Der Standort wird durch die IP-Adresse des Clients identifiziert, den Sie verwendet haben, um eine Verbindung mit Azure Active Directory herzustellen. Für diese Bedingung ist es erforderlich, dass Sie sich mit vertrauenswürdigen IPs auskennen. „Vertrauenswürdige IPs“ ist ein Feature der Multi-Factor Authentication, mit dem Sie Bereiche mit vertrauenswürdigen IP-Adressen definieren können, die für das lokale Intranet Ihrer Organisation stehen. Beim Konfigurieren der Bedingungen eines Standorts können Sie mit der Option „Vertrauenswürdige IPs“ zwischen Verbindungen, die über das Netzwerk Ihrer Organisation hergestellt werden, und allen anderen Standorten unterscheiden. Weitere Informationen finden Sie unter [Vertrauenswürdige IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  
Sie können entweder alle Standorte oder alle vertrauenswürdigen IPs einbinden, und Sie können alle vertrauenswürdigen IPs ausschließen.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/03.png)


- **Client-App**: Die Client-App kann entweder eine allgemeine App (Webbrowser, mobile App, Desktopclient) sein, die Sie zum Herstellen der Verbindung mit Azure Active Directory verwendet haben, oder Sie können die spezifische Option „Exchange Active Sync“ auswählen.  
Die Legacyauthentifizierung bezieht sich auf Clients, für die eine einfache Authentifizierung verwendet wird, z.B. ältere Office-Clients, für die keine moderne Authentifizierung genutzt wird. Für die Legacyauthentifizierung wird der bedingte Zugriff derzeit nicht unterstützt.

    ![Bedingungen](./media/active-directory-conditional-access-azure-portal/04.png)


## <a name="what-you-should-know"></a>Wichtige Informationen

### <a name="do-i-need-to-assign-a-user-to-my-policy"></a>Muss ich meiner Richtlinie einen Benutzer zuweisen?

Beim Konfigurieren einer Richtlinie für den bedingten Zugriff sollten Sie ihr mindestens eine Gruppe zuweisen. Eine Richtlinie für den bedingten Zugriff, der keine Benutzer und Gruppen zugewiesen sind, wird niemals angewendet.

Wenn Sie einer Richtlinie mehrere Benutzer und Gruppen zuweisen möchten, sollten Sie klein anfangen, indem Sie nur einen Benutzer oder eine Gruppe zuweisen und anschließend die Konfiguration testen. Wenn die Richtlinie wie erwartet funktioniert, können Sie ihr weitere Zuweisungen hinzufügen.  


### <a name="how-are-assignments-evaluated"></a>Wie werden Zuweisungen ausgewertet?

Alle Zuweisungen sind logisch per **UND**-Operator verbunden. Wenn Sie mehr als eine Zuweisung konfiguriert haben, müssen die Bedingungen aller Zuweisungen erfüllt sein, damit die Richtlinie ausgelöst wird.  

Falls Sie eine Standortbedingung konfigurieren müssen, die für alle Verbindungen von außerhalb des Organisationsnetzwerks gelten, können Sie dies wie folgt erreichen:

- Einschließen: **All locations** (Alle Standorte)
- Ausschließen: **All trusted IPs** (Alle vertrauenswürdigen IPs) 

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Was passiert, wenn Sie im klassischen Azure-Portal und im Azure-Portal Richtlinien konfiguriert haben?  
Beide Richtlinien werden von Azure Active Directory erzwungen, und ein Benutzer erhält nur dann Zugriff, wenn alle Anforderungen erfüllt sind.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Was passiert, wenn Sie im Intune Silverlight-Portal und im Azure-Portal über Richtlinien verfügen?
Beide Richtlinien werden von Azure Active Directory erzwungen, und ein Benutzer erhält nur dann Zugriff, wenn alle Anforderungen erfüllt sind. 

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Was passiert, wenn ich mehrere Richtlinien für denselben Benutzer konfiguriert habe?  
Bei jeder Anmeldung werden von Azure Active Directory alle Richtlinien ausgewertet, und es wird sichergestellt, dass alle Anforderungen erfüllt sind, bevor dem Benutzer der Zugriff gewährt wird.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Funktioniert der bedingte Zugriff mit Exchange ActiveSync?
 
Nein, Sie können Exchange ActiveSync derzeit nicht in einer Richtlinie für den bedingten Zugriff verwenden.


### <a name="what-happens-if-i-require-multi-factor-authentication-or-a-compliant-device"></a>Was geschieht, wenn ich die mehrstufige Authentifizierung oder ein kompatibles Gerät benötige?

Derzeit wird der Benutzer unabhängig vom Gerät zur mehrstufigen Authentifizierung aufgefordert.



## <a name="common-scenarios"></a>Gängige Szenarien

### <a name="requiring-multi-factor-authentication-for-apps"></a>Erzwingen von Multi-Factor Authentication für Apps

Viele Umgebungen verfügen über Apps, für die eine höhere Schutzebene als für andere Apps erforderlich ist.
Dies gilt beispielsweise für Apps, für die Zugriff auf sensible Daten besteht. Wenn Sie diesen Apps eine weitere Schutzebene hinzufügen möchten, können Sie eine Richtlinie für den bedingten Zugriff konfigurieren, für die eine Multi-Factor Authentication erforderlich ist, wenn Benutzer auf diese Apps zugreifen.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Erzwingen der Multi-Factor Authentication für den Zugriff aus nicht vertrauenswürdigen Netzwerken

Dieses Szenario ähnelt dem vorherigen Szenario, da hiermit eine Anforderung für die Multi-Factor Authentication hinzugefügt wird.
Der Hauptunterschied ist aber die Bedingung für diese Anforderung.  
Während der Schwerpunkt des vorherigen Szenarios auf Apps mit Zugriff auf sensible Daten lag, geht es in diesem Szenario nun um vertrauenswürdige Standorte.  
Anders ausgedrückt: Sie können beispielsweise die Multi-Factor Authentication erzwingen, wenn auf eine App von einem Benutzer über ein Netzwerk zugegriffen wird, das für Sie nicht vertrauenswürdig ist. 


### <a name="only-trusted-devices-can-access-office-365-services"></a>Nur mit vertrauenswürdigen Geräten kann auf Office 365-Dienste zugegriffen werden

Wenn Sie Intune in Ihrer Umgebung verwenden, können Sie sofort damit beginnen, die Oberfläche für Richtlinien für den bedingten Zugriff in der Azure-Konsole zu verwenden.

Viele Intune-Kunden nutzen den bedingten Zugriff, um sicherzustellen, dass nur vertrauenswürdige Geräte auf Office 365-Dienste zugreifen können. Dies bedeutet, dass mobile Geräte bei Intune registriert werden und die Anforderungen der Konformitätsrichtlinien erfüllen und dass Windows-PCs in eine lokale Domäne eingebunden werden. Eine wichtige Verbesserung besteht darin, dass Sie eine Richtlinie nicht für jeden Office 365-Dienst neu festlegen müssen.  Konfigurieren Sie die Cloud-Apps beim Erstellen einer neuen Richtlinie so, dass alle O365-Apps eingebunden sind, die Sie per bedingtem Zugriff schützen möchten. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.



<!--HONumber=Jan17_HO3-->


