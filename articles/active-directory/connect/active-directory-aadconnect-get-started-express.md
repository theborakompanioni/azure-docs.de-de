---
title: 'Azure AD Connect: Erste Schritte mit Expresseinstellungen | Microsoft Docs'
description: "Informationen zum Herunterladen, Installieren und Ausführen des Setup-Assistenten für Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: curtand
ms.assetid: b6ce45fd-554d-4f4d-95d1-47996d561c9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 19be73fd0aec3a8f03a7cd83c12cfcc060f6e5e7
ms.openlocfilehash: 8a08f6e441a856a06bf7870747ca20af45a0364e
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Erste Schritte mit Azure AD Connect mit Expresseinstellungen
Die **Expresseinstellungen** von Azure AD Connect werden verwendet, wenn Sie eine Topologie mit einer einzelnen Gesamtstruktur und die [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) für die Authentifizierung verwenden. **Expresseinstellungen** sind die Standardoption und werden für die häufigsten Bereitstellungsszenarien verwendet. Mit nur wenigen Klicks können Sie Ihr lokales Verzeichnis auf die Cloud erweitern.

Vor dem Installieren von Azure AD Connect müssen Sie [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) herunterladen und die unter [Azure AD Connect: Hardware und Voraussetzungen](active-directory-aadconnect-prerequisites.md) beschriebenen vorbereitenden Schritte ausführen.

Falls Expresseinstellungen nicht zu Ihrer Topologie passen, finden Sie weitere Szenarien in der [verwandten Dokumentation](#related-documentation) .

## <a name="express-installation-of-azure-ad-connect"></a>Expressinstallation von Azure AD Connect
Im Abschnitt [Videos](#videos) können Sie diese Schritte in Aktion sehen.

1. Melden Sie sich als lokaler Administrator auf dem Server an, auf dem Sie Azure AD Connect installieren möchten. Dies sollte der Server sein, der als Synchronisierungsserver verwendet werden soll.
2. Navigieren Sie zu **AzureADConnect.msi**, und doppelklicken Sie darauf.
3. Aktivieren Sie auf der Willkommensseite das Kontrollkästchen zum Zustimmen zu den Lizenzbedingungen, und klicken Sie auf **Weiter**.  
4. Klicken Sie im Bildschirm "Expresseinstellungen" auf **Expresseinstellungen verwenden**.  
   ![Willkommen bei Azure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. Geben Sie auf dem Bildschirm „Mit Azure AD verbinden“ den Benutzernamen und das Kennwort eines globalen Azure-Administrators für Azure AD ein. Klicken Sie auf **Weiter**.  
   ![Herstellen einer Verbindung mit Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Falls ein Fehler auftritt und Sie Probleme mit der Konnektivität haben, können Sie unter [Problembehebung bei Konnektivitätsproblemen](active-directory-aadconnect-troubleshoot-connectivity.md) nach einer Lösung suchen.
6. Geben Sie auf dem Bildschirm „Mit AD DS verbinden“ den Benutzernamen und das Kennwort für ein Enterprise-Administratorkonto ein. Sie können den Domänenteil entweder im NetBIOS- oder FQDN-Format eingeben, also „FABRIKAM\administrator“ oder „fabrikam.com\administrator“. Klicken Sie auf **Weiter**.  
   ![Herstellen einer Verbindung mit AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Die Seite [**Konfiguration der Azure AD-Anmeldung**](active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) wird nur angezeigt, wenn Sie den Vorgang zum [Überprüfen Ihrer Domänen](../active-directory-add-domain.md) unter [Voraussetzungen](active-directory-aadconnect-prerequisites.md) nicht abgeschlossen haben.
   ![Nicht überprüfte Domänen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
   Wenn diese Seite angezeigt wird, sollten Sie jede Domäne überprüfen, die als **Nicht hinzugefügt** und **Nicht überprüft** gekennzeichnet ist. Stellen Sie sicher, dass die verwendeten Domänen in Azure AD überprüft wurden. Klicken Sie auf das Symbol zum Aktualisieren, wenn Sie Ihre Domänen überprüft haben.
8. Klicken Sie im Bildschirm "Bereit zur Konfiguration" auf **Installieren**.
   * Optional können Sie auf der Seite „Bereit zur Konfiguration“ das Kontrollkästchen **Starten Sie den Synchronisierungsvorgang, sobald die Konfiguration abgeschlossen wurde** deaktivieren. Deaktivieren Sie dieses Kontrollkästchen, wenn Sie weitere Konfigurationseinstellungen vornehmen möchten, z.B. [Filtern](active-directory-aadconnectsync-configure-filtering.md). Wenn Sie diese Option deaktivieren, konfiguriert der Assistent die Synchronisierung, der Scheduler bleibt jedoch deaktiviert. Er wird erst ausgeführt, wenn Sie ihn manuell aktivieren, indem Sie den [Installations-Assistenten erneut ausführen](active-directory-aadconnectsync-installation-wizard.md).
   * Wenn Sie in Ihrer lokalen Active Directory-Instanz Exchange nutzen, können Sie auch [**Hybridbereitstellungen in Exchange**](https://technet.microsoft.com/library/jj200581.aspx) aktivieren. Aktivieren Sie diese Option, wenn Sie Exchange-Postfächer gleichzeitig in der Cloud und lokal bereitstellen möchten.
     ![Bereit zur Konfiguration von Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Klicken Sie nach Abschluss der Installation auf **Beenden**.
10. Melden Sie sich nach Abschluss der Installation ab und erneut wieder an, ehe Sie den Synchronisierungsdienst-Manager oder Synchronisierungsregel-Editor verwenden.

## <a name="videos"></a>Videos
Ein Video zur Expressinstallation finden Sie hier:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Express-Settings/player]
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Azure AD Connect installiert haben, können Sie [die Installation überprüfen und Lizenzen zuweisen](active-directory-aadconnect-whats-next.md).

Weitere Informationen zu den Features, die mit der Installation aktiviert wurden: [Automatisches Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md), [Verhindern von versehentlichen Löschungen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) und [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Weitere Informationen zu folgenden allgemeinen Themen: [Scheduler und Auslösen der Synchronisierung](active-directory-aadconnectsync-feature-scheduler.md).

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

## <a name="related-documentation"></a>verwandten Dokumentation
| Thema |
| --- | --- |
| Übersicht über Azure AD Connect |
| Installation mit benutzerdefinierten Einstellungen |
| Upgrade von DirSync |
| Für die Installation verwendete Konten |


