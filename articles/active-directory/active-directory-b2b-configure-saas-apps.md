---
title: "Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit in Azure Active Directory | Microsoft-Dokumentation"
description: "Code- und PowerShell-Beispiele für die Azure Active Directory B2B-Zusammenarbeit"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/06/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 0c05cd490ee9125f7e5182cb502db6f4e9390094
ms.openlocfilehash: 3f41fdaa4f0ec31c9f11f2826b5cb9ccbf4db30d


---

# <a name="configure-saas-apps-for-b2b-collaboration"></a>Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit

Die Azure Active Directory B2B-Zusammenarbeit funktioniert mit den meisten Anwendungen, die sich in Azure AD integrieren lassen. In diesem Abschnitt wird erläutert, wie Sie einige beliebte SAS-Apps für die Verwendung mit Azure AD B2B konfigurieren.
Bevor wir die anwendungsspezifischen Anweisungen betrachten, finden Sie im Folgenden einige Faustregeln:

* Denken Sie daran, dass für die meisten Anwendungen die Benutzerbereitstellung manuell erfolgen muss (d.h., Benutzer müssen auch in der Anwendung manuell erstellt werden).

* Bei Apps, die eine automatische Bereitstellung unterstützen (wie z.B. Dropbox), werden von den Anwendungen separate Einladungen erstellt. Benutzer müssen jede Einladung annehmen.

* Legen Sie in den Benutzerattributen die Benutzer-ID immer auf „user.mail“ fest (um mögliche Probleme mit fehlerhaften UPD-Werten bei Gastbenutzern zu minimieren).


##<a name="dropbox-for-business"></a>Dropbox für Unternehmen

Damit Benutzer sich mithilfe ihres Organisationskontos anmelden können, muss Dropbox für Unternehmen manuell für die Verwendung von Azure AD als SAML-Identitätsanbieter konfiguriert werden. Dropbox für Unternehmen kann Benutzer nicht zum Anmelden über Azure AD auffordern oder Benutzern diese Anmeldung ermöglichen, wenn die App nicht vorher dafür konfiguriert wurde.

1. Fügen Sie die App Dropbox für Unternehmen in Azure AD hinzu, wie im Screenshot gezeigt.

  ![Hinzufügen von Dropbox zu Azure AD](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

  ![Hinzufügen von Dropbox zu Azure AD](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

2. Konfigurieren Sie die Anwendung.

  ![Konfigurieren des einmaligen Anmeldens für die App](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

3. Wählen Sie die Konfiguration des einmaligen Anmeldens aus, und ändern Sie die Benutzer-ID zu „user.mail“ (standardmäßig lautet der Wert „UPN“).

4. Laden Sie das Zertifikat herunter, das für die Dropbox-Konfiguration verwendet werden soll.

  ![Herunterladen des Zertifikats](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. Verwenden Sie die Option zum Konfigurieren von Dropbox (der Prozess wird detailliert in den folgenden Screenshots erläutert).

6. Rufen Sie die SAML-URL für das einmalige Anmelden ab, die in der Konfiguration verwendet werden soll.

7. Rufen Sie die Anmelde-URL von der Dropbox-Konfigurationsseite ab.

  ![Anmelden bei Dropbox](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

  ![Dropbox-Menü](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

  ![Dropbox-Authentifizierungsdialogfeld, reduziert](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![Dropbox-Authentifizierungsdialogfeld, erweitert](media/active-directory-b2b-configure-saas-apps/dropbox-auth-02.png)

8. Laden Sie das Zertifikat hoch und fügen Sie hier die SAML-URL für das einmalige Anmelden ein.

  ![Einfügen der SAML-URL für das einmalige Anmelden](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

9. Konfigurieren Sie die automatische Benutzerbereitstellung im Azure-Portal.

  ![Einrichten der automatischen Benutzerbereitstellung](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

10. Nach der Bereitstellung in der Dropbox-App erhält der Gast- bzw. Mitgliedsbenutzer eine separate Einladung von Dropbox. Die Eingeladenen müssen die Einladung durch Klicken auf den Link annehmen, um das einmalige Anmelden für Dropbox verwenden zu können.

## <a name="box"></a>Box
In diesem Abschnitt wird erläutert, wie Sie es Benutzern ermöglichen, eine Verbundanmeldung basierend auf dem SAML-Protokoll zu verwenden, um mit ihrem Konto in Azure AD einen Gastbenutzer bei Box zu authentifizieren. Im Rahmen dieses Verfahrens müssen Sie Metadaten auf Box.com hochladen.

1. Hinzufügen von Box aus den Unternehmensanwendungen

2. Einmaliges Anmelden konfigurieren

  ![Konfigurieren des einmaligen Anmeldens für Box](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

3. Stellen Sie zuerst sicher, dass die Anmelde-URL für Box im Azure-Verwaltungsportal richtig eingerichtet ist. Es handelt sich um die URL Ihres Box.com-Mandanten, die im folgenden Format vorliegen sollte: https://.box.com.

4. Ein Hinweis: Die ID wird für diese Anwendung nicht benötigt, wird jedoch weiterhin als Pflichtfeld angezeigt.

5. Legen Sie die Benutzer-ID auf „user.mail“ fest (um das einmalige Anmelden für Gastkonten sicherzustellen).

6. Erstellen Sie ein neues SAML-Zertifikat.

7. Um die Verwendung von Azure Active Directory als Identitätsanbieter für Ihren Box.com-Mandanten zu konfigurieren, laden Sie zunächst die folgende Metadatendatei herunter und speichern Sie lokal auf Ihrem Computer. Stellen Sie sicher, dass die Datei aktiv ist.

8. Leiten Sie die Metadatendatei an das Supportteam von Box weiter. Das Supportteam muss das einmalige Anmelden für Sie konfigurieren.

9. Richten Sie die automatische Benutzerbereitstellung durch Azure AD ein.

  ![Autorisieren von Azure AD zum Herstellen einer Verbindung mit Box](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Eingeladene müssen ihre Einladung von der Box-Anwendung einlösen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)
* [Dynamische Gruppen und B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit](active-directory-b2b-claims-mapping.md)
* [Externe Office 365-Freigaben](active-directory-b2b-o365-external-user.md)
* [Aktuelle Einschränkungen der B2B-Zusammenarbeit](active-directory-b2b-current-limitations.md)



<!--HONumber=Feb17_HO1-->


