---
title: Festlegen von gerätebasierten Azure Active Directory-Richtlinien für bedingten Zugriff zur Steuerung des Zugriffs auf über Azure Active Directory verbundene Anwendungen
description: Beschreibt, wie IT-Admins gerätebasierte Richtlinien für bedingten Zugriff für über Azure AD verbundene Anwendungen festlegen können.
services: active-directory
documentationcenter: ''
author: markusvi
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2016
ms.author: markvi

---
# Festlegen von gerätebasierten Azure Active Directory-Richtlinien für bedingten Zugriff zur Steuerung des Zugriffs auf über Azure Active Directory verbundene Anwendungen
Mit dem gerätebasierten bedingten Zugriff in Azure Active Directory können Sie Ihre Organisationsressourcen vor Folgendem schützen:

* Zugriff über unbekannte oder nicht verwaltete Geräte
* Zugriff über Geräte, die die von Ihrer Organisation definierten Sicherheitsrichtlinien nicht erfüllen

Eine Übersicht über den bedingten Zugriff finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).

Sie können gerätebasierte Richtlinien für bedingten Zugriff einrichten, um Folgendes zu schützen:

* Office 365 SharePoint Online zum Schützen der Websites und Dokumente der Organisation.
* Office 365 Exchange Online zum Schützen der E-Mails der Organisation.
* SaaS-Anwendungen, die zur Authentifizierung mit Azure AD verbunden sind.
* Lokale Anwendungen, die über den Azure AD-Anwendungsproxy veröffentlicht wurden.

Sie können diese Richtlinie im Azure-Verwaltungsportal festlegen, indem Sie im Verzeichnis zur entsprechenden Anwendung wechseln.

  ![Anwendungen](./media/active-directory-conditional-access-policy-connected-applications/01.png "Anwendungen")

Nach dem Auswählen der Anwendung klicken Sie auf die Registerkarte **Konfigurieren**, um die Richtlinie für bedingten Zugriff festzulegen.

  ![Gerätebasierte Zugriffsregeln](./media/active-directory-conditional-access-policy-connected-applications/02.png "Gerätebasierte Zugriffsregeln")

Um eine gerätebasierte Richtlinie für bedingten Zugriff zu aktivieren, wählen Sie im Abschnitt **Gerätebasierte Zugriffsregeln** bei **Zugriffsregeln aktivieren** die Option **Ein**.

Diese Richtlinie besteht aus drei Komponenten:

1. **Anwenden auf**: Der Bereich von Benutzern, auf die diese Richtlinie angewendet wird, wenn sie auf die Anwendung zugreifen.
2. **Geräteregeln**: Die Bedingungen, die Geräte erfüllen müssen, bevor sie auf die Anwendung zugreifen dürfen.
3. **Anwendungserzwingung**: Die Clientanwendungen, für die die Richtlinie ausgewertet werden soll (nativ/browserbasiert oder nur nativ).
   
   ![Gerätebasierte Zugriffsregeln](./media/active-directory-conditional-access-policy-connected-applications/03.png "Gerätebasierte Zugriffsregeln")

## Auswählen der Benutzer, für die die Richtlinie gilt
Im Abschnitt **Anwenden auf** können Sie den Bereich von Benutzern auswählen, für die diese Richtlinie gelten soll.

Ihnen stehen zwei Optionen zur Verfügung:

* **Alle Benutzer**: Alle Benutzer, die auf die Anwendung zugreifen.
* **Gruppen**: Zum Einschränken des Benutzerbereichs auf Benutzer, die einer oder mehreren Gruppen angehören.

![Anwenden auf](./media/active-directory-conditional-access-policy-connected-applications/11.png "Anwenden auf")

Durch Auswahl der Option **Ausgenommen** können Sie Benutzer während des Zugriffs auf die Anwendung von dieser Richtlinie ausschließen. Dies ist hilfreich, wenn Sie bestimmten Benutzern vorübergehend Zugriff auf die Anwendung gewähren müssen. Wählen Sie diese Option beispielsweise aus, wenn einige Ihrer Benutzer über Geräte verfügen, die nicht für den bedingten Zugriff bereit sind (noch nicht registriert, Kompatibilität nicht mehr erfüllt usw.).

## Auswählen der Bedingungen, die Geräte erfüllen müssen
Mit **Geräteregeln** legen Sie die Bedingungen fest, mit denen Geräte auf die Anwendung zugreifen können.

Folgende Geräte werden für den gerätebasierten bedingten Zugriff unterstützt:

* Windows 10 Anniversary Update, Windows 7 und Windows 8.1
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2
* iOS-Geräte (iPad, iPhone)
* Android-Geräte

Unterstützung für Mac ist in Kürze verfügbar.

  ![Geräte](./media/active-directory-conditional-access-policy-connected-applications/04.png "Anwendungen")

> [!NOTE]
> Weitere Informationen zu den Unterschieden zwischen dem Domänenbeitritt und dem Einbinden über Azure AD finden Sie unter [Verwenden von Windows 10-Geräten an Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).
> 
> 

Ihnen stehen zwei Optionen für Geräteregeln zur Verfügung:

* **Alle Geräte müssen kompatibel sein**: Bei dieser Regel müssen alle Geräteplattformen, die auf die Anwendung zugreifen, kompatibel sein. Plattformen, die den gerätebasierten bedingten Zugriff nicht unterstützen, erhalten keinen Zugriff.
* **Nur ausgewählte Geräte müssen kompatibel sein**: Bei dieser Regel müssen nur ausgewählte Geräteplattformen kompatibel sein. Nicht ausgewählten oder anderen Plattformen wird der Zugriff gewährt.
  
  ![Anwendungen](./media/active-directory-conditional-access-policy-connected-applications/05.png "Anwendungen")

Über Azure AD eingebundene Geräte sind kompatibel, wenn sie von Microsoft Intune oder einem Drittanbietersystem für die Verwaltung mobiler Geräte, das in Azure AD integriert werden kann, im Verzeichnis als **kompatibel** gekennzeichnet wurden.

Bei in die Domäne eingebundenen Geräten wird Kompatibilität auf zwei Arten erreicht:

* Wenn ein Gerät bei Azure AD registriert ist, betrachten viele Organisationen dieses aufgrund der Tatsache, dass es einer Domäne beigetreten ist, als vertrauenswürdiges Gerät.
* Das Gerät wurde von System Center Configuration Manager 2016 in Azure AD als „kompatibel“ gekennzeichnet.
  
  ![Geräteregeln](./media/active-directory-conditional-access-policy-connected-applications/06.png "Geräteregeln")

Persönliche Windows-Geräte sind kompatibel, wenn sie von Microsoft Intune oder einem Drittanbietersystem für die Verwaltung mobiler Geräte, das in Azure AD integriert werden kann, im Verzeichnis als **kompatibel** gekennzeichnet wurden.

Nicht-Windows-Geräte sind kompatibel, wenn sie von Microsoft Intune im Verzeichnis als **kompatibel** gekennzeichnet wurden.

> [!NOTE]
> Weitere Informationen zum Einrichten von Azure AD für die Gerätekompatibilität durch das Verwaltungssystem finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).
> 
> 

Bei der Auswahl bestimmter Geräteplattformen können Sie eine oder mehrere Optionen auswählen: Android, iOS, Windows Mobile (Smartphones und Tablets unter Windows 8.1) und Windows (alle anderen Windows-Geräte einschließlich Windows 10-Geräten). Mit dieser Option findet die Richtlinienauswertung nur auf den ausgewählten Plattformen statt. Wenn der Zugriff über ein Gerät versucht wird, das nicht zur Auswahl gehört, wird keine Geräterichtlinie ausgewertet, und das Gerät wird zugelassen, wenn auch der Benutzer zugelassen ist.

![Geräteregeln](./media/active-directory-conditional-access-policy-connected-applications/07.png "Geräteregeln")

## Auswahl der Art von Clientanwendungen, für die die Richtlinie ausgewertet wird
Legen Sie im Abschnitt **Anwendungserzwingung** die Art von Anwendungen fest, für die die Richtlinie ausgewertet werden muss.

Ihnen stehen zwei Optionen für Anwendungen zur Verfügung:

* Für Browser- und native Anwendungen
* Nur für native Anwendungen

![Anwendungen](./media/active-directory-conditional-access-policy-connected-applications/08.png "Anwendungen")

Die Auswahl von **Für Browser- und native Anwendungen** erzwingt die Richtlinie beim Zugriff auf Anwendungen durch:

* Browser (z.B. Edge in Windows 10, Safari in iOS usw.)
* Anwendungen, die auf einer beliebigen Plattform die Active Directory Authentication Library (ADAL) oder unter Windows 10 die Web Account Manager-API (WAM) verwenden

> [!NOTE]
> Weitere Informationen zur Browserunterstützung sowie weitere Überlegungen für den Endbenutzerzugriff auf Anwendungen, die durch gerätebasierte Richtlinien für bedingten Zugriff geschützt sind, finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).
> 
> 

## Schützen des E-Mail-Zugriffs über Exchange Active Sync-basierte Anwendungen
In Office 365 Exchange Online-Anwendungen steht der zusätzliche Bereich **Exchange ActiveSync** bereit. In diesem Abschnitt können Sie den E-Mail-Zugriff auf E-Mail-Anwendungen blockieren, die auf Exchange Active Sync basieren.

![Anwendungen](./media/active-directory-conditional-access-policy-connected-applications/09.png "Anwendungen")

![Anwendungen](./media/active-directory-conditional-access-policy-connected-applications/10.png "Anwendungen")

## Weitere Themen
* [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md)

<!---HONumber=AcomDC_0914_2016-->