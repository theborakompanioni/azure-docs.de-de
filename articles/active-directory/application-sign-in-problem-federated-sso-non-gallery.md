---
title: "Probleme beim Anmelden bei einer nicht im Katalog enthaltenen Anwendung, die für einmaliges Anmelden im Verbund konfiguriert ist | Microsoft-Dokumentation"
description: "Leitfaden zu den möglichen Problemen beim Anmelden bei einer Anwendung, die für SAML-basiertes einmaliges Anmelden im Verbund mit Azure AD konfiguriert wurde"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 30c3a1f6a90a430abc67a20134bc5474cd33468b
ms.lasthandoff: 04/17/2017


---


# <a name="problems-signing-in-to-a-non-gallery-application-configured-for-federated-single-sign-on"></a>Probleme beim Anmelden bei einer nicht im Katalog enthaltenen Anwendung, die für einmaliges Anmelden im Verbund konfiguriert ist

Um das Problem zu beheben, müssen Sie die Anwendungskonfiguration in Azure AD wie folgt überprüfen:

-   Sie haben die Konfigurationsschritte für die Azure AD-Kataloganwendung ausgeführt.

-   Die in AAD konfigurierte ID und Antwort-URL entsprechen den erwarteten Werten in der Anwendung.

-   Sie haben der Anwendung Benutzer zugewiesen.

## <a name="application-not-found-in-directory"></a>Die Anwendung wurde im Verzeichnis nicht gefunden.

*Fehler: Die Anwendung mit dem Bezeichner „https://contoso.com“ wurde im Verzeichnis nicht gefunden.*

**Mögliche Ursache**

Das in der SAML-Anforderung aus der Anwendung an Azure AD gesendete Aussteller-Attribut entspricht nicht dem in der Azure AD-Anwendung konfigurierten ID-Wert.

**Lösung**

Stellen Sie sicher, dass das Aussteller-Attribut in der SAML-Anforderung mit dem in Azure AD konfigurierten Bezeichner übereinstimmt:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  <span id="_Hlk477190042" class="anchor"></span>Wechseln Sie zum Abschnitt **Domäne und URLs**. Stellen Sie sicher, dass der Wert im Textfeld „Bezeichner“ mit dem in der Fehlermeldung angezeigten ID-Wert übereinstimmt.

Nachdem Sie den ID-Wert in Azure AD aktualisiert haben und er mit dem von der Anwendung in der SAML-Anforderung gesendeten Wert übereinstimmt, sollten Sie in der Lage sein, sich bei der Anwendung anzumelden.

## <a name="user-not-assigned-a-role"></a>Benutzer wurde keine Rolle zugewiesen

*Fehler: Der angemeldete Benutzer „brian@contoso.com“ ist keiner Rolle für die Anwendung zugewiesen.*

**Mögliche Ursache**

Dem Benutzer wurde kein Zugriff auf die Anwendung in Azure AD erteilt.

**Lösung**

Um einer Anwendung Benutzer direkt zuzuweisen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie in der Liste die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Benutzer und Gruppen**.

8.  Klicken Sie oberhalb der Liste **Benutzer und Gruppen** auf die Schaltfläche **Hinzufügen**, um das Blatt **Zuweisung hinzufügen** zu öffnen.

9.  Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Benutzer und Gruppen**.

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Namen** oder die **E-Mail-Adresse** des gewünschten Benutzers ein.

11. Zeigen Sie auf den **Benutzer** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Klicken Sie auf das Kontrollkästchen neben dem Profilbild oder Logo des Benutzers, um den Benutzer der Liste **Ausgewählt** hinzuzufügen.

12. **Optional:** Wenn Sie **mehrere Benutzer hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Namen** oder eine weitere **E-Mail-Adresse** ein, und klicken Sie auf das Kontrollkästchen, um diesen Benutzer der Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Benutzer ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**, um sie der Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional:** Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Benutzern zugewiesen werden soll.

15. Klicken Sie auf die Schaltfläche **Zuweisen**, um die Anwendung den ausgewählten Benutzern zuzuweisen.

Nach kurzer Zeit können die ausgewählten Benutzer diese Anwendungen mit den Verfahren starten, die im Abschnitt mit der Lösungsbeschreibung beschrieben wurden.

## <a name="not-a-valid-saml-request"></a>Keine gültige SAML-Anforderung

*Fehler: Die Anforderung ist keine gültige Nachricht im SAML2-Protokoll.*

**Mögliche Ursache**

Azure AD unterstützt die von der Anwendung für einmaliges Anmelden gesendete SAML-Anforderung nicht. Häufige Probleme sind beispielsweise die folgenden:

-   Fehlende erforderliche Felder in der SAML-Anforderung

-   Codierte Methode in der SAML-Anforderung

**Lösung**

1.  Erfassen Sie die SAML-Anforderung. Im Tutorial [Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) erhalten Sie Informationen zum Erfassen der SAML-Anforderung.

2.  Wenden Sie sich an den Anwendungshersteller, und machen Sie folgende Angaben:

    -   SAML-Anforderung

    -   [Anforderungen des SAML-Protokolls für einmaliges Anmelden bei Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Der Hersteller sollte bestätigen, dass er die SAML-Implementierung in Azure AD für einmaliges Anmelden unterstützt.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Keine Ressource in der Liste requiredResourceAccess

*Fehler: Die Clientanwendung hat Zugriff auf die Ressourcen „00000002-0000-0000-c000-000000000000“ angefordert. Bei dieser Anforderung ist ein Fehler aufgetreten, da der Client diese Ressource nicht in der Liste requiredResourceAccess angegeben hat.*

**Mögliche Ursache**

Das Anwendungsobjekt ist beschädigt.

**Lösung**

Um das Problem zu beheben, entfernen Sie die Anwendung aus dem Verzeichnis. Fügen Sie dann die Anwendung erneut hinzu, konfigurieren Sie sie, und führen Sie die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Klicken Sie in der linken oberen Ecke des Blatts **Übersicht** der Anwendung auf **Löschen**.

8.  Aktualisieren Sie Azure AD, und fügen Sie die Anwendung aus dem Azure AD-Katalog hinzu. Konfigurieren Sie anschließend die Anwendung erneut.

Nach dem erneuten Konfigurieren der Anwendung sollten Sie in der Lage sein, sich bei der Anwendung anzumelden.

## <a name="certificate-or-key-not-configured"></a>Zertifikat oder Schlüssel nicht konfiguriert

Fehler: kein Signaturschlüssel konfiguriert.

**Mögliche Ursache**

Das Anwendungsobjekt ist beschädigt, und Azure AD erkennt das für die Anwendung konfigurierte Zertifikat nicht.

**Lösung**

Um das Zertifikat zu löschen und ein neues zu erstellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

9.  Wählen Sie „Ablaufdatum“ aus. Klicken Sie dann auf **Speichern**.

10. Aktivieren Sie **Neues Zertifikat aktivieren**, um das aktive Zertifikat außer Kraft zu setzen. Klicken Sie oben auf dem Blatt auf **Speichern**, und akzeptieren Sie die Aktivierung des Rolloverzertifikats.

11. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Entfernen**, um das **nicht verwendete** Zertifikat zu entfernen.

## <a name="next-steps"></a>Nächste Schritte
[Anforderungen des SAML-Protokolls für einmaliges Anmelden bei Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

