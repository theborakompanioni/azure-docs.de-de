---
title: "Festlegen von Richtlinien für den gerätebasierten bedingten Zugriff für über Azure Active Directory verbundene Anwendungen | Microsoft-Dokumentation"
description: "Legen Sie gerätebasierte Richtlinien für über Azure AD verbundene Anwendungen fest."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 1e7e764bdb1e883c28c137292de6a3ef8873e473
ms.openlocfilehash: e4b8622fc6d06be480a2de057070155b12746133


---
# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Festlegen von gerätebasierten Richtlinien für über Azure AD verbundene Anwendungen
Mit dem gerätebasierten bedingten Zugriff in Azure Active Directory (Azure AD) können Sie Ihre Organisationsressourcen vor Folgendem schützen:

* Zugriffsversuche von unbekannten oder nicht verwalteten Geräten
* Geräte, die die Sicherheitsrichtlinien Ihrer Organisation nicht erfüllen

Eine Übersicht über den bedingten Zugriff finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).

Sie können gerätebasierte Richtlinien für bedingten Zugriff einrichten, um folgende Anwendungen zu schützen:

* Office 365 SharePoint Online zum Schützen der Websites und Dokumente Ihrer Organisation
* Office 365 Exchange Online zum Schützen der E-Mails Ihrer Organisation
* SaaS-Anwendungen (Software-as-a-Service) mit Verbindungen mit Azure AD zur Authentifizierung
* Lokale Anwendungen, die mit Azure AD-Anwendungsproxydiensten veröffentlicht wurden

Wechseln Sie zum Festlegen einer Richtlinie für den gerätebasierten bedingten Zugriff im Azure-Portal zu der jeweiligen Anwendung im Verzeichnis.

  ![Liste der Anwendungen im Azure-Portalverzeichnis](./media/active-directory-conditional-access-policy-connected-applications/01.png "Anwendungen")

Wählen Sie die Anwendung aus, und klicken Sie auf die Registerkarte **Konfigurieren**, um die Richtlinie für bedingten Zugriff festzulegen.  

  ![Konfigurieren der Anwendung](./media/active-directory-conditional-access-policy-connected-applications/02.png "Gerätebasierte Zugriffsregeln")

Um eine gerätebasierte Richtlinie für bedingten Zugriff festzulegen, wählen Sie im Abschnitt **Gerätebasierte Zugriffsregeln** unter **Zugriffsregeln aktivieren** die Option **Ein** aus.

Eine Richtlinie für den gerätebasierten bedingten Zugriff besteht aus drei Komponenten:

* **Anwenden auf**. Der Gültigkeitsbereich unter den Benutzern, für die die Richtlinie gilt.
* **Geräteregeln**. Die Bedingungen, die ein Gerät erfüllen muss, bevor es auf die Anwendung zugreifen darf.
* **Anwendungserzwingung**. Die Clientanwendungen (nativ oder Browser), für die die Richtlinie gilt.
  
  ![Die drei Komponenten einer Richtlinie für den gerätebasierten Zugriff](./media/active-directory-conditional-access-policy-connected-applications/03.png "Gerätebasierte Zugriffsregeln")

## <a name="select-the-users-the-policy-applies-to"></a>Wählen Sie die Benutzer aus, für die die Richtlinie gilt.
Im Abschnitt **Anwenden auf** können Sie den Bereich von Benutzern auswählen, für die diese Richtlinie gelten soll.

Sie haben beim Erstellen eines Gültigkeitsbereichs für die Benutzer einer Zugriffsrichtlinie zwei Optionen:

* **Alle Benutzer**. Die Richtlinie wird auf alle Benutzer mit Zugriff auf die Anwendung angewendet.
* **Gruppen**. Die Richtlinie wird auf Benutzer beschränkt, die Mitglied einer bestimmten Gruppe sind.

![Anwenden der Richtlinie auf alle Benutzer oder eine Gruppe](./media/active-directory-conditional-access-policy-connected-applications/11.png "Anwenden auf")

 Wenn Sie einen Benutzer aus einer Richtlinie ausschließen möchten, aktivieren Sie das Kontrollkästchen **Ausgenommen**. Dies ist hilfreich, wenn Sie einem bestimmten Benutzer vorübergehend Zugriff auf die Anwendung gewähren müssen. Aktivieren Sie diese Option beispielsweise, wenn einige Ihrer Benutzer über Geräte verfügen, die nicht für den bedingten Zugriff bereit sind. Die Geräte sind möglicherweise noch nicht registriert oder bald nicht mehr kompatibel.

## <a name="select-the-conditions-that-devices-must-meet"></a>Auswählen der Bedingungen, die Geräte erfüllen müssen
Legen Sie unter **Geräteregeln** die Bedingungen fest, die ein Gerät erfüllen muss, um auf die Anwendung Zugriff zu erhalten.

Sie können den gerätebasierten bedingten Zugriff für diese Gerätetypen festlegen:

* Windows 10 Anniversary Update, Windows 8.1 und Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2
* iOS-Geräte (iPad, iPhone)
* Android-Geräte

Unterstützung für Mac ist in Kürze verfügbar.

  ![Anwenden von Richtlinien auf Geräte](./media/active-directory-conditional-access-policy-connected-applications/04.png "Anwendungen")

> [!NOTE]
> Weitere Informationen zu den Unterschieden zwischen dem Domänenbeitritt und dem Einbinden von Geräten über Azure AD finden Sie unter [Verwenden von Windows 10-Geräten an Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).
> 
> 

Ihnen stehen zwei Optionen für Geräteregeln zur Verfügung:

* **Alle Geräte müssen kompatibel sein**. Alle Geräteplattformen, die auf die Anwendung zugreifen, müssen kompatibel sein. Geräte auf Plattformen, die den gerätebasierten bedingten Zugriff nicht unterstützen, erhalten keinen Zugriff.
* **Nur ausgewählte Geräte müssen kompatibel sein**. Nur bestimmte Geräteplattformen müssen kompatibel sein. Andere Plattformen oder andere Plattformen mit Zugriff auf die Anwendung erhalten Zugriff.
  
  ![Festlegen des Gültigkeitsbereichs für Geräteregeln](./media/active-directory-conditional-access-policy-connected-applications/05.png "Anwendungen")

Über Azure AD eingebundene Geräte sind kompatibel, wenn sie von Intune oder einem Drittanbietersystem für die Verwaltung mobiler Geräte, das in Azure AD integriert werden kann, im Verzeichnis als **kompatibel** gekennzeichnet wurden.

In eine Domäne eingebundene Geräte sind unter folgenden Bedingungen kompatibel:

* Sie wurden in Azure AD registriert. Viele Organisationen behandeln in die Domäne eingebundene Geräte als vertrauenswürdige Geräte.
* Die Geräte wurden von System Center Configuration Manager in Azure AD als **kompatibel** gekennzeichnet.
  
  ![Kompatible in die Domäne eingebundene Geräte](./media/active-directory-conditional-access-policy-connected-applications/06.png "Geräteregeln")

Persönliche Windows-Geräte sind kompatibel, wenn sie von Intune oder einem Drittanbietersystem für die Verwaltung mobiler Geräte, das in Azure AD integriert werden kann, im Verzeichnis als **kompatibel** gekennzeichnet wurden.

Nicht-Windows-Geräte sind kompatibel, wenn sie von Intune im Verzeichnis als **kompatibel** gekennzeichnet wurden.

> [!NOTE]
> Weitere Informationen zum Einrichten von Azure AD für die Gerätekompatibilität in unterschiedlichen Verwaltungssystemen finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).
> 
> 

Sie können eine oder mehrere Geräteplattformen für eine Richtlinie für den gerätebasierten Zugriff auswählen. Dazu gehören Android, iOS, Windows Mobile (Windows 8.1-Telefone und -Tablets) und Windows (alle anderen Windows-Geräte, einschließlich aller Windows 10-Geräte).
Die Richtlinienauswertung findet nur auf den ausgewählten Plattformen statt. Wird der Zugriff über ein Gerät versucht, auf dem keine der ausgewählten Plattformen ausgeführt wird, erhält das Gerät Zugriff auf die Anwendung, wenn der Benutzer Zugriff hat. Es wird keine Geräterichtlinie ausgewertet.

![Auswählen der Plattformen für Geräteregeln](./media/active-directory-conditional-access-policy-connected-applications/07.png "Geräteregeln")

## <a name="set-policy-evaluation-for-a-type-of-application"></a>Festlegen der Richtlinienauswertung für einen Anwendungstyp
Legen Sie im Abschnitt **Anwendungserzwingung** den Anwendungstyp fest, für den die Richtlinie den Benutzer- und Gerätezugriff auswerten muss.

Sie haben zwei Auswahlmöglichkeiten für den Anwendungstyp:

* Browser- und native Anwendungen
* Nur native Anwendungen

![Auswählen von Browser- und nativen Anwendungen](./media/active-directory-conditional-access-policy-connected-applications/08.png "Anwendungen")

Um die Zugriffsrichtlinie für Anwendungen zu erzwingen, wählen Sie **Für Browser- und systemeigene Anwendungen** aus. Sie können dann Folgendes einschließen:

* Browser (z.B. Microsoft Edge unter Windows 10 oder Safari unter iOS Safari)
* Anwendungen, die auf einer beliebigen Plattform die Active Directory Authentication Library (ADAL) oder unter Windows 10 die Web Account Manager-API (WAM) verwenden

> [!NOTE]
> Weitere Informationen zur Browserunterstützung sowie weitere Überlegungen für den Benutzerzugriff auf Anwendungen, die durch gerätebasierte Richtlinien für bedingten Zugriff geschützt sind, finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md).
> 
> 

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Schützen des E-Mail-Zugriffs über Exchange Active Sync-basierte Anwendungen
In Office 365 Exchange Online-Anwendungen können Sie Exchange ActiveSync verwenden, um den E-Mail-Zugriff auf Exchange ActiveSync-basierte E-Mail-Anwendungen zu blockieren.

![Exchange ActiveSync-Kompatibilitätsoptionen](./media/active-directory-conditional-access-policy-connected-applications/09.png "Anwendungen")

![Kompatibles Gerät für den E-Mail-Zugriff erforderlich](./media/active-directory-conditional-access-policy-connected-applications/10.png "Anwendungen")

## <a name="next-steps"></a>Nächste Schritte
* [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md)




<!--HONumber=Dec16_HO4-->


