---

title: Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory | Microsoft-Dokumentation
description: Informationen zum Zuweisen von Lizenzen mithilfe der gruppenbasierten Lizenzierung in Azure Active Directory
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: f8b63e5831897d3a45298b0415bb2d6d44ab0de1
ms.openlocfilehash: 73727d8d1e5de01794589538bd67d3d698d325a0
ms.lasthandoff: 03/01/2017


---

# <a name="assign-licenses-to-a-group-of-users-in-azure-active-directory"></a>Zuweisen von Lizenzen zu einer Gruppe von Benutzern in Azure Active Directory

In diesem Artikel durchlaufen wir das grundlegende Verfahren der Zuweisung von Produktlizenzen zu einer Gruppe und Überprüfung, ob alle Mitglieder der Gruppe ordnungsgemäß lizenziert sind.

In seinem Beispiel enthält der Mandant eine Sicherheitsgruppe namens **HR Department**, zu der alle Mitglieder der Personalabteilung zählen (in diesem Fall ca. 1.000 Benutzer). Der Administrator möchte der gesamten Abteilung Office 365 Enterprise E3-Lizenzen zuweisen. Der in dem Produkt enthaltene Yammer Enterprise-Dienst soll vorübergehend bis zu dem Zeitpunkt deaktiviert werden, zu dem die Abteilung für seine Nutzung bereit ist. Der Administrator möchte außerdem Enterprise Mobility + Security-Lizenzen für die gleiche Benutzergruppe bereitstellen.

## <a name="step-1-assign-the-required-licenses"></a>Schritt 1: Zuweisen der erforderlichen Lizenzen

1. Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com) mit einem Administratorkonto an. Zum Verwalten von Lizenzen benötigt das Konto die Rolle „Globaler Administrator“ oder „Benutzerkontoadministrator“.

2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie dann **Azure Active Directory** aus. Sie können dieses Blatt als Favoriten festlegen, indem Sie auf das Sternsymbol klicken oder es an das Dashboard im Portal anheften.

3. Wählen Sie auf dem Blatt **Azure Active Directory** den Eintrag **Lizenzen** aus. Daraufhin wird ein Blatt geöffnet, auf dem Sie alle lizenzierbaren Produkte im Mandanten anzeigen und verwalten können.

4. Wählen Sie unter **Alle Produkte** sowohl Office 365 Enterprise E3 als auch Enterprise Mobility + Security aus, indem Sie auf die Produktnamen klicken. Wählen Sie oben auf dem Blatt **Zuweisen** aus, um die Zuweisung zu starten.

  ![Alle Produkte, Lizenz zuweisen](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. Klicken Sie auf dem Blatt **Lizenz zuweisen** auf **Benutzer und Gruppen**, um das Blatt „Benutzer und Gruppen“ zu öffnen. Suchen Sie den Gruppennamen *HR Department*, wählen Sie die Gruppe aus, und klicken Sie zur Bestätigung unten auf dem Blatt auf **Auswählen**.

  ![Auswählen einer Gruppe](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. Klicken Sie auf dem Blatt **Lizenz zuweisen** auf **Zuweisungsoptionen (optional)**. Daraufhin werden alle Servicepläne angezeigt, die aus den beiden zuvor ausgewählten Produkten bestehen. Wechseln Sie zu Yammer Enterprise, und legen Sie das Produkt auf **Aus** fest, um diesen Dienst in der Produktlizenz zu deaktivieren. Bestätigen Sie, indem Sie unten in **Zuweisungsoptionen** auf **OK** klicken.

  ![Zuweisungsoptionen](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. Klicken Sie schließlich unten auf dem Blatt **Lizenz zuweisen** auf **Zuweisen**, um die Zuweisung abzuschließen.

8. Rechts oben wird eine Benachrichtigung mit dem Status und Ergebnis des Vorgangs angezeigt. Wenn die Zuweisung an die Gruppe nicht abgeschlossen werden konnte (z.B. aufgrund bereits vorhandener Lizenzen für die Gruppe), klicken Sie auf die Benachrichtigung, um Details zum Fehler anzuzeigen.

Wir haben nun eine Lizenzvorlage für die Gruppe „HR Department“ angegeben. Ein Hintergrundprozess in Azure AD wurde gestartet, um alle vorhandenen Mitglieder dieser Gruppe zu verarbeiten. Dieser anfängliche Vorgang dauert je nach aktueller Größe der Gruppe einige Zeit. Im nächsten Schritt erläutern wir das Überprüfen, ob der Vorgang abgeschlossen wurde und ob weitere Maßnahmen zur Problembehebung erforderlich sind.

> [!NOTE]
> Die gleiche Zuweisung kann von anderer Stelle aus gestartet werden, nämlich in **Benutzer und Gruppen** in Azure AD. Wechseln Sie zu **Azure Active Directory &gt; Benutzer und Gruppen &gt; Alle Gruppen**. Wählen Sie die gewünschte Gruppe aus, und wechseln Sie zur Registerkarte **Lizenzen**. Über die Schaltfläche **Zuweisen** oben auf dem Blatt wird das Blatt zur Lizenzzuweisung geöffnet.

## <a name="step-2-verify-that-the-initial-assignment-has-completed"></a>Schritt 2: Überprüfen, ob die anfängliche Zuweisung erfolgt ist

1. Wechseln Sie zu **Azure Active Directory &gt; Benutzer und Gruppen &gt; Alle Gruppen**. Wählen Sie die Gruppe *HR Department* aus, der die Lizenzen zugewiesen wurden.

2. Wählen Sie auf dem Blatt der Gruppe *HR Department* den Eintrag **Lizenzen** aus, um schnell zu prüfen, ob Lizenzen Benutzern vollständig zugewiesen wurden und ob es Fehler gibt, die ggf. untersucht werden müssen, wie z.B.:

  - Produktlizenzen, die der Gruppe zugewiesen wurden. Wählen Sie einen Eintrag aus, um die bestimmten Dienste anzuzeigen, die aktiviert wurden, und um Änderungen vorzunehmen.

  - Status der letzten Änderungen an der Lizenzzuweisung:. Prüfen Sie, ob die Änderungen verarbeitet wurden oder ob die Verarbeitung für alle Gruppenmitglieder erfolgt ist.

  - Wenn Fehler aufgetreten sind, Informationen zu Benutzern mit dem Status „Fehler“ oder denen keine Lizenzen zugewiesen werden konnten.

  ![Zuweisungsoptionen](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Detaillierte Informationen zur Lizenzverarbeitung finden Sie unter **Azure Active Directory &gt; Benutzer und Gruppen &gt;
    *Gruppenname* &gt; Überwachungsprotokolle**:

  - Aktivität: **Starten der gruppenbasierten Lizenzzuweisung zu Benutzern**. Es wird protokolliert, wann unser System die Änderung der Lizenzzuweisung für die Gruppe verarbeitet und beginnt, diese für alle Gruppenmitglieder zu übernehmen. Das Protokoll enthält Informationen über die erfolgte Änderung.

  - Aktivität: **Beenden der gruppenbasierten Lizenzzuweisung zu Benutzern**. Es wird protokolliert, wann unser System mit der Verarbeitung aller Benutzer in der Gruppe fertig ist. Das Protokoll enthält eine Übersicht über die Anzahl der erfolgreich verarbeiteten Benutzer und die Anzahl der Benutzer, denen die Gruppenlizenzen nicht zugewiesen werden konnten.

## <a name="step-3-checking-for-license-problems-and-resolving-them"></a>Schritt 3: Suchen nach Lizenzproblemen und deren Behebung

1. Wechseln Sie zu **Azure Active Directory &gt; Benutzer und Gruppen &gt; Alle Gruppen**. Wählen Sie die Gruppe *HR Department* aus, der die Lizenzen zugewiesen wurden.

2. Wählen Sie auf dem Blatt der Gruppe **HR Department** den Eintrag **Lizenzen** aus. Die Benachrichtigung oben auf dem Blatt gibt an, dass 10 Benutzern keine Lizenzen zugewiesen werden konnten. Es wird eine Liste aller Benutzer mit dem Lizenzierungsstatus „Fehler“ in dieser Gruppe geöffnet.

3. Die Spalte **Fehlerhafte Zuweisungen** informiert uns, dass den Benutzern beide Produktlizenzen nicht zugewiesen werden konnten. **Häufigste Fehlerursache** enthält die Ursache des Fehlers, in diesem Fall **Im Konflikt stehende Diensteeinstellungen**.

  ![Fehlerhafte Zuweisungen](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Wählen Sie einen Benutzer aus, um das Blatt **Lizenzen** zu öffnen, das alle Lizenzen zeigt, die dem Benutzer derzeit zugewiesen sind. Bei diesem Beispiel sehen wir, dass der Benutzer die Office 365 Enterprise E1-Lizenz von der Gruppe **Kiosk users** übernommen hat. Dies steht in Konflikt mit der E3-Lizenz, die das System versucht, der Gruppe **HR Department** zuzuweisen. Dies hat zur Folge, dass keine der Lizenzen dieser Gruppe dem Benutzer zugewiesen wurden.

  ![Anzeigen von Lizenzen für einen Benutzer](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. Um dieses Problem zu beheben, entfernen Sie den Benutzer aus der Gruppe **Kiosk users**. Nachdem Azure AD die Änderung verarbeitet hat, sind die Lizenzen für **HR Department** richtig zugewiesen.

  ![Lizenz ordnungsgemäß zugewiesen](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen für die Lizenzverwaltung mithilfe von Gruppen finden Sie unter

* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

