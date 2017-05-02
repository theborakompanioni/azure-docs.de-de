---
title: Zuweisen von Benutzern und Gruppen zu einer Anwendung | Microsoft-Dokumentation
description: "Zuweisen von Benutzern zu der Anwendung und Gewähren des Zugriffs darauf"
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
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 972ed736e63089a69726de5d26236b930b390a10
ms.lasthandoff: 04/11/2017


---

# <a name="how-to-assign-users-and-groups-to-an-application"></a>Zuweisen von Benutzern und Gruppen zu einer Anwendung

Damit Benutzer eine der folgenden Aktionen für eine bestimmte Anwendung durchführen können, müssen Sie **sie zunächst der Anwendung zuweisen**, um ihnen Zugriff darauf zu gewähren:

-   Zugriff auf eine Anwendung durch **direktes Navigieren zur URL der Anwendung** (wird auch als vom Dienstanbieter initiierte Anmeldung bezeichnet).

-   Zugriff auf eine Anwendung über die **URL für den Benutzerzugriff** auf der Seite **Eigenschaften** einer Anwendung (wird auch als IdP-initiierte Anmeldung bezeichnet).

-   Eine Anwendung wird in ihrem [Anwendungszugriffsbereich](https://myapps.microsoft.com/) oder in der mobilen Anwendung angezeigt.

-   Eine Anwendung wird in ihrem [Startprogramm für Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) angezeigt.

## <a name="methods-to-assign-applications-with-azure-active-directory"></a>Methoden zum Zuweisen von Anwendungen mit Azure Active Directory 

Zum Zuweisen von Anwendungen mit Azure Active Directory stehen 3 Möglichkeiten zur Verfügung:

-   [Direktes Zuweisen eines Benutzers zu einer Anwendung als Administrator](#assign-a-user-directly-as-an-administrator)

-   [Direktes Zuweisen einer Gruppe zu einer Anwendung als Administrator](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [Aktivieren des Self-Service-Anwendungszugriffs, um Benutzern die Suche ihrer eigenen Anwendungen zu ermöglichen](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a>Direktes Zuweisen eines Benutzers als Administrator

Führen Sie die folgenden Schritte aus, um einer Anwendung einen oder mehrere Benutzer direkt zuzuweisen:

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

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Direktes Zuweisen einer Gruppe zu einer Anwendung als Administrator

Um einer Anwendung eine oder mehrere Gruppen direkt zuzuweisen, führen Sie folgende Schritte aus:

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

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Gruppennamen** der gewünschten Gruppe ein.

11. Zeigen Sie auf die **Gruppe** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Klicken Sie auf das Kontrollkästchen neben dem Profilbild oder Logo der Gruppe, um die Gruppe der Liste **Ausgewählt** hinzuzufügen.

12. **Optional:** Wenn Sie **mehrere Gruppen hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Gruppennamen** ein, und klicken Sie auf das Kontrollkästchen, um diese Gruppe der Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Gruppen ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**, um sie der Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional:** Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Gruppen zugewiesen werden soll.

15. Klicken Sie auf die Schaltfläche **Zuweisen**, um die Anwendung den ausgewählten Gruppen zuzuweisen.

Nach kurzer Zeit können die Benutzer in den ausgewählten Gruppen diese Anwendungen mit den Verfahren starten, die im Abschnitt mit der Lösungsbeschreibung beschrieben wurden. Bei dynamischen Gruppen kann bei der Verarbeitung dieser Zuweisungen eine zusätzliche Verzögerung entstehen, bevor sie für Benutzer in den zugewiesenen Gruppen angezeigt werden.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Aktivieren des Self-Service-Anwendungszugriffs, um Benutzern die Suche ihrer eigenen Anwendungen zu ermöglichen

Der Self-Service-Anwendungszugriff bietet die Möglichkeit, dass Benutzer Anwendungen selbst ermitteln können und dass die entsprechende Geschäftseinheit den Zugriff auf diese Anwendungen optional genehmigen kann. Sie können festlegen, dass die Mitarbeiter der Geschäftseinheit direkt über ihren Zugriffsbereich die den Benutzern zugewiesenen Anmeldeinformationen für Anwendungen mit einmaligem Anmelden per Kennwort verwalten können.

Führen Sie die folgenden Schritte aus, um den Self-Service-Anwendungszugriff auf eine Anwendung zu aktivieren:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie in der Liste die Anwendung aus, für die Sie den Self-Service-Zugriff aktivieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Self-Service**.

8.  Um den Self-Service-Anwendungszugriff auf die Anwendung zu aktivieren, legen Sie **Benutzern das Anfordern des Zugriffs auf diese Anwendung erlauben?** auf **Ja** fest.

9.  Um dann die Gruppe auszuwählen, der Benutzer, die den Zugriff auf diese Anwendung anfordern, hinzugefügt werden sollen, klicken Sie auf die Auswahl neben **Welcher Gruppe sollen zugewiesene Benutzer hinzugefügt werden?**, und wählen Sie eine Gruppe aus.

10. **Optional:** Wenn eine Genehmigung des Unternehmens erforderlich sein soll, damit Benutzer Zugriff erhalten, legen Sie **Genehmigung anfordern, bevor Zugriff auf diese Anwendung gewährt wird?** auf **Ja** fest.

11. **Optional: nur für Anwendungen mit einmaligem Anmelden per Kennwort:** Wenn die genehmigenden Personen des Unternehmens für genehmigte Benutzer die an die Anwendung gesendeten Kennwörter angeben können sollen, legen Sie **Genehmigenden Personen das Festlegen von Benutzerkennwörtern für diese Anwendung gestatten?** auf **Ja** fest.

12. **Optional:** Um die genehmigenden Personen des Unternehmens anzugeben, die den Zugriff auf die Anwendung genehmigen können, klicken Sie auf die Auswahl neben **Wer darf den Zugriff auf diese Anwendung genehmigen?**. Hier können Sie bis zu 10 genehmigende Personen auswählen.

  >[!NOTE]
  >Gruppen werden nicht unterstützt.
  >
  >

13. **Optional:** Wenn Sie für **Anwendungen, die Rollen verfügbar machen**, die für den Self-Service genehmigten Benutzer einer Rolle zuweisen möchten, klicken Sie auf die Auswahl neben **Welcher Rolle sollen Benutzer in dieser Anwendung zugewiesen werden?**, und wählen Sie die Rolle aus, der die Benutzer zugewiesen werden sollen.

14. Klicken Sie abschließend oben auf dem Blatt auf die Schaltfläche **Speichern**.

Nachdem Sie die Self-Service-Anwendungskonfiguration abgeschlossen haben, können Benutzer in ihrem [Anwendungszugriffsbereich](https://myapps.microsoft.com/) auf die Schaltfläche **+Hinzufügen** klicken und die Apps suchen, für die Sie den Self-Service-Zugriff aktiviert haben. Den genehmigenden Personen des Unternehmens wird im [Zugriffsbereich](https://myapps.microsoft.com/) zudem eine Benachrichtigung angezeigt. Sie können festlegen, dass sie in einer E-Mail darüber benachrichtigt werden, dass ein Benutzer den Zugriff auf eine Anwendung angefordert hat, der zu genehmigen ist. 

Diese Genehmigungen unterstützen nur Workflows mit einzelnen Genehmigungen, d.h., wenn Sie mehrere genehmigende Personen angeben, kann jede einzelne genehmigende Person den Zugriff auf die Anwendung genehmigen.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)

