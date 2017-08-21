---

title: Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory | Microsoft-Dokumentation
description: Zuweisen von Lizenzen zu Benutzern mit der Gruppenlizenzierung von Azure Active Directory
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
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 42b18eab9cb419e6ada72ba72dc8be8d7f7b2eed
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---

# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Zuweisen von Lizenzen zu Benutzer nach Gruppenmitgliedschaft in Azure Active Directory

Dieser Artikel führt Sie durch die Zuweisung von Produktlizenzen zu einer Benutzergruppe in Azure Active Directory (Azure AD) und die anschließende Überprüfung, ob die Lizenzierung ordnungsgemäß durchgeführt wurde.

In diesem Beispiel enthält der Mandant eine Sicherheitsgruppe namens **HR Department** (Personalabteilung). Diese Gruppe enthält alle Mitglieder der Personalabteilung (etwa 1.000 Benutzer). Sie möchten der gesamten Abteilung Office 365 Enterprise E3-Lizenzen zuweisen. Der in dem Produkt enthaltene Yammer Enterprise-Dienst muss vorübergehend deaktiviert werden, bis die Abteilung für dessen Verwendung bereit ist. Sie möchten außerdem Enterprise Mobility + Security-Lizenzen für die gleiche Benutzergruppe bereitstellen.

> [!NOTE]
> Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft „Verwendungsstandort“ für den Benutzer angeben.

> Bei der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses. Wenn Sie Benutzer an mehreren Standorten haben, wird empfohlen, die Angabe des Verwendungsstandorts als Teil des Benutzererstellungsablaufs in Azure AD (z.B. über die AAD Connect-Konfiguration) einzustellen. Dadurch wird sichergestellt, dass das Ergebnis der Lizenzzuweisung immer korrekt ist und die Benutzer keine Dienste für Standorte empfangen, die nicht zugelassen sind.

## <a name="step-1-assign-the-required-licenses"></a>Schritt 1: Zuweisen der erforderlichen Lizenzen

1. Melden Sie sich beim [**Azure portal (Azure-Portal)**](https://portal.azure.com) mit einem Administratorkonto an. Zum Verwalten von Lizenzen muss das Konto eine globale Administratorrolle besitzen oder ein Benutzerkontoadministrator sein.

2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie anschließend **Azure Active Directory** aus. Sie können dieses Blatt zu den Favoriten hinzufügen oder auf dem Dashboard im Portal anheften.

3. Wählen Sie auf dem Blatt **Azure Active Directory** den Eintrag **Lizenzen** aus. Daraufhin wird ein Blatt geöffnet, auf dem Sie alle lizenzierbaren Produkte im Mandanten anzeigen und verwalten können.

4. Wählen Sie unter **Alle Produkte** sowohl Office 365 Enterprise E3 als auch Enterprise Mobility + Security aus, indem Sie auf die Produktnamen klicken. Wählen Sie oben auf dem Blatt **Assign** (Zuweisen) aus, um die Zuweisung zu starten.

   ![Alle Produkte, Lizenz zuweisen](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. Klicken Sie auf dem Blatt **Lizenz zuweisen** auf **Benutzer und Gruppen**, um das Blatt **Benutzer und Gruppen** zu öffnen. Suchen Sie den Gruppennamen *HR Department*, wählen Sie die Gruppe aus, und klicken Sie zur Bestätigung unten auf dem Blatt auf **Auswählen**.

   ![Auswählen einer Gruppe](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. Klicken Sie auf dem Blatt **Lizenz zuweisen** auf **Zuweisungsoptionen (optional)**. Daraufhin werden alle Servicepläne angezeigt, die in den beiden zuvor ausgewählten Produkten enthalten sind. Wechseln Sie zu **Yammer Enterprise**, und legen Sie das Produkt auf **Aus** fest, um den Dienst in der Produktlizenz zu deaktivieren. Bestätigen Sie, indem Sie unten in **Zuweisungsoptionen** auf **OK** klicken.

   ![Zuweisungsoptionen](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. Klicken Sie schließlich unten auf dem Blatt **Assign license** (Lizenz zuweisen) auf **Assign** (Zuweisen), um die Zuweisung abzuschließen.

8. Rechts oben wird eine Benachrichtigung mit dem Status und Ergebnis des Vorgangs angezeigt. Falls die Gruppenzuweisung nicht abgeschlossen werden konnte (beispielsweise aufgrund bereits vorhandener Lizenzen für die Gruppe), klicken Sie auf die Benachrichtigung, um Details zum Fehler anzuzeigen.

Wir haben nun eine Lizenzvorlage für die Gruppe „HR Department“ angegeben. Ein Hintergrundprozess in Azure AD wurde gestartet, um alle vorhandenen Mitglieder dieser Gruppe zu verarbeiten. Dieser anfängliche Vorgang dauert je nach aktueller Größe der Gruppe einige Zeit. Im nächsten Schritt erfahren Sie, wie Sie überprüfen, ob der Vorgang abgeschlossen wurde und ob weitere Maßnahmen zur Problembehebung erforderlich sind.

> [!NOTE]
> Die gleiche Zuweisung kann auch über **Benutzer und Gruppen** in Azure AD gestartet werden. Navigieren Sie zu **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Gruppen**. Suchen Sie dann nach der Gruppe, wählen Sie sie aus, und navigieren Sie zur Registerkarte **Lizenzen**. Über die Schaltfläche **Assign** (Zuweisen) oben auf dem Blatt wird das Blatt zur Lizenzzuweisung geöffnet.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Schritt 2: Überprüfen, ob die anfängliche Zuweisung erfolgt ist

1. Navigieren Sie zu **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Gruppen**. Suchen Sie dann nach der Gruppe **HR Department**, der Lizenzen zugewiesen wurden.

2. Wählen Sie auf dem Blatt der Gruppe **HR Department** den Eintrag **Lizenzen** aus. Dadurch können Sie schnell überprüfen, ob die Lizenzen den Benutzern vollständig zugewiesen wurden und ob Fehler vorliegen, die untersucht werden müssen. Folgende Informationen stehen zur Verfügung:

   - Liste mit den Produktlizenzen, die der Gruppe aktuell zugewiesen sind. Wählen Sie einen Eintrag aus, um die bestimmten Dienste anzuzeigen, die aktiviert wurden, und um Änderungen vorzunehmen.

   - Status der letzten Lizenzänderungen für die Gruppe (also ob die Änderungen verarbeitet werden oder ob die Verarbeitung für alle Benutzermitglieder abgeschlossen ist).

   - Informationen zu Benutzern, bei denen ein Fehler vorliegt, da ihnen keine Lizenzen zugewiesen werden konnten.

   ![Zuweisungsoptionen](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Ausführlichere Informationen zur Lizenzverarbeitung finden Sie unter **Azure Active Directory** > **Benutzer und Gruppen** > *Gruppenname* > **Überwachungsprotokolle**. Beachten Sie folgende Aktivitäten:

   - Aktivität: **Starten der gruppenbasierten Lizenzzuweisung zu Benutzern**. Dies wird protokolliert, wenn das System die Änderung der Lizenzzuweisung für die Gruppe verarbeitet und beginnt, diese für alle Benutzer zu übernehmen. Das Protokoll enthält Informationen über die erfolgte Änderung.

   - Aktivität: **Beenden der gruppenbasierten Lizenzzuweisung zu Benutzern**. Wird protokolliert, wenn das System alle Benutzer in der Gruppe verarbeitet hat. Das Protokoll enthält eine Übersicht über die Anzahl erfolgreich verarbeiteter Benutzer und über die Anzahl von Benutzern, denen keine Gruppenlizenzen zugewiesen werden konnten.

   [Lesen Sie diesen Abschnitt](./active-directory-licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity), um mehr darüber zu erfahren, wie Überwachungsprotokolle dazu verwendet werden können, um durch die gruppenbasierte Lizenzierung vorgenommene Änderungen zu analysieren.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Schritt 3: Suchen nach Lizenzproblemen und Beheben der Probleme

1. Wechseln Sie zu **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Gruppen**. Wählen Sie die Gruppe **HR Department** aus, der Lizenzen zugewiesen wurden.
2. Wählen Sie auf dem Blatt der Gruppe **HR Department** den Eintrag **Lizenzen** aus. Die Benachrichtigung oben auf dem Blatt gibt an, dass zehn Benutzern keine Lizenzen zugewiesen werden konnten. Wenn Sie auf die Benachrichtigung klicken, wird für diese Gruppe eine Liste aller Benutzer mit dem Lizenzierungsstatus „Fehler“ geöffnet.
3. In der Spalte **Fehlerhafte Zuweisungen** ist zu sehen, dass den Benutzern beide Produktlizenzen nicht zugewiesen werden konnten. Die Spalte **Top reason for failure**(Häufigste Fehlerursache) enthält die Ursache des Fehlers. In diesem Fall: **Widersprüchliche Servicepläne**.

   ![Fehlerhafte Zuweisungen](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Wählen Sie einen Benutzer aus, um das Blatt **Lizenzen** zu öffnen. Auf diesem Blatt werden alle Lizenzen angezeigt, die dem Benutzer derzeit zugewiesen sind. In diesem Beispiel hat der Benutzer die Office 365 Enterprise E1-Lizenz von der Gruppe **Kiosk users** (Kioskbenutzer) geerbt. Dies steht in Konflikt mit der E3-Lizenz, die über die Gruppe **HR Department** angewendet werden sollte. Daher wurde dem Benutzer keine der Lizenzen aus dieser Gruppe zugewiesen.

   ![Lizenzen für einen Benutzer anzeigen](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. Um dieses Problem zu beheben, entfernen Sie den Benutzer aus der Gruppe **Kiosk users** (Kioskbenutzer). Nachdem Azure AD die Änderung verarbeitet hat, sind die Lizenzen für **HR Department** richtig zugewiesen.

   ![Lizenz ordnungsgemäß zugewiesen](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Features für die Lizenzverwaltung mithilfe von Gruppen finden Sie in den folgenden Artikeln:

* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

