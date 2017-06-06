---
title: Genehmigungsworkflows in Azure Privileged Identity Management | Microsoft-Dokumentation
description: Lernen Sie Genehmigungsworkflows in Privileged Identity Management (PIM) kennen.
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 31b797361fca0c92db228f4150a6bff7e38e2b22
ms.contentlocale: de-de
ms.lasthandoff: 04/29/2017

---

# <a name="approvals-preview"></a>Genehmigungen (Vorschau)

## <a name="overview"></a>Übersicht

Mit Genehmigungen für Privileged Identity Management können Sie Rollen konfigurieren, um die Genehmigung für die Aktivierung anzufordern, und einen oder mehrere Benutzer oder Gruppen als delegierte genehmigende Personen auswählen. Im Folgenden erfahren Sie, wie Sie Rollen konfigurieren und genehmigende Personen auswählen.

>[!NOTE]
Bitte denken Sie daran, dass sich dieses Feature noch in der Entwicklungsphase befindet, und Fehler auftreten können. Die Funktionalität, einschließlich Text und Benennungskonventionen, kann noch geändert werden und sollte nicht als final betrachtet werden.


## <a name="key-terminology"></a>Wesentliche Terminologie

*Benutzer mit berechtigter Rolle*: Ein Benutzer mit berechtigter Rolle ist ein Benutzer in Ihrer Organisation, der einer Rolle in Azure AD als berechtigt zugewiesen ist (Rolle erfordert Aktivierung).

*Delegierte genehmigende Person*: Eine oder mehrere Personen oder Gruppen in Ihrem Azure AD, die für die Genehmigung von Anforderungen der Rollenaktivierung verantwortlich sind, sind genehmigende Personen.

## <a name="scenarios"></a>Szenarien

Die private Vorschau unterstützt die folgenden Szenarien:

**Als Administrator für privilegierte Rollen (Privileged Role Administrator, PRA) können Sie:**

-   [die Genehmigung für bestimmte Rollen aktivieren](#enable-approval-for-specific-roles)

-   [Benutzer und/oder Gruppen für die Genehmigung von Anforderungen als genehmigende Personen festlegen](#specify-approver-users-and/or-groups-to-approve-requests)

-   [den Anforderungs- und Genehmigungsverlauf für alle privilegierten Rollen anzeigen](#view-request-and-approval-history-for-all-privileged-roles)

**Als festgelegte genehmigende Person können Sie:**

-   [ausstehende Genehmigungen (Anforderungen) anzeigen](#view-pending-approvals-requests)

-   [Anforderungen von Rechteerweiterungen für Rollen (einzeln und/oder mehrere) genehmigen oder ablehnen](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [eine Begründung für die Genehmigung/Ablehnung abgeben](#provide-justification-for-my-approval/rejection) 

**Als Benutzer mit berechtigter Rolle können Sie:**

-   [die Aktivierung einer Rolle anfordern, die genehmigt werden muss](#request-activation-of-a-role-that-requires-approval)

-   [den Status Ihrer Aktivierungsanforderung anzeigen](#view-the-status-of-your-request-to-activate)

-   [Ihre Aufgabe in Azure AD fertig stellen, wenn die Aktivierung genehmigt wurde](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navigation

Wir haben die Navigation zur Unterstützung von Genehmigungen aktualisiert.

![](media/azure-ad-pim-approval-workflow/image001.png)

Die standardmäßige Angebotsseite bietet komfortablen Zugriff auf Informationen zu PIM und die neue Genehmigungendokumentation.

![](media/azure-ad-pim-approval-workflow/image002.png)

Wir haben auch einen neuen Abschnitt für alle PIM-Benutzer hinzugefügt: „Mein Überwachungsverlauf“. Hier finden Sie alle Informationen, die für Ihre Identität relevant sind. Dies beinhaltet auch, dass Sie alle Ihre ausstehenden und abgeschlossenen Anforderungen, alle Entscheidungen, die Sie über die Anforderungen getroffen haben, die Ihnen vorgelegt wurden, und alle Ihre früheren Rollenaktivierungen bequem an einem Ort anzeigen können.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Aktivieren der Genehmigung für bestimmte Rollen

Um die Genehmigung für eine bestimmte Rolle zu aktivieren, müssen Sie zuerst im linken Navigationsbereich „Verzeichnisrollen“ auswählen.

![](media/azure-ad-pim-approval-workflow/image004.png)

Suchen und Auswählen von Einstellungen im linken Navigationsbereich „Verzeichnisrollen“

![](media/azure-ad-pim-approval-workflow/image006.png)

Wählen Sie privilegierte Rollen aus:

![](media/azure-ad-pim-approval-workflow/image009.png)

Wählen Sie im Abschnitt „Genehmigung anfordern“ die Option „Aktivieren“ aus:

![](media/azure-ad-pim-approval-workflow/image011.png)

Nach der Aktivierung wird das Blatt erweitert, um die folgenden Details anzuzeigen:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Wenn Sie KEINE genehmigenden Personen angeben, werden die PRAs die standardmäßigen genehmigenden Personen. PRAs wären erforderlich, um ALLE Aktivierungsanforderungen für diese Rolle zu genehmigen.

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a>Festlegen von Benutzern und/oder Gruppen als genehmigende Personen für die Genehmigung von Anforderungen

Um die Genehmigung zu delegieren, klicken Sie auf die Option „Genehmigende Personen auswählen“:

![](media/azure-ad-pim-approval-workflow/image015.png)

Wenn das Blatt „Genehmigende Personen auswählen“ geladen wird, können Sie über die Suchleiste am oberen Rand nach einem bestimmten Benutzer oder einer bestimmten Gruppe suchen oder Ihre Auswahl in der bereits gefüllten Liste treffen und dann auf „Auswählen“ klicken:

![](media/azure-ad-pim-approval-workflow/image017.png)

Hinweis: Sie können mehrere Benutzer oder Gruppen gleichzeitig auswählen.

Ihre Auswahl wird in der Liste der ausgewählten genehmigenden Personen angezeigt, wie unten dargestellt:

![](media/azure-ad-pim-approval-workflow/image019.png)

Um eine genehmigende Person zu entfernen, klicken Sie einfach auf die Schaltfläche „Entfernen“ neben ihrem Namen.

Um zusätzliche genehmigende Personen hinzuzufügen, wiederholen Sie den Vorgang.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Anzeigen des Anforderungs- und Genehmigungsverlaufs für alle privilegierten Rollen

Um den Anforderungs- und Genehmigungsverlaufs für alle privilegierten Rollen anzuzeigen, wählen Sie im Dashboard „Überwachungsverlauf“:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Sie können die Daten nach der „Aktion“ sortieren und nach „Aktivierung genehmigt“ suchen.

### <a name="view-pending-approvals-requests"></a>Anzeigen ausstehender Genehmigungen (Anforderungen)

Als delegierte genehmigende Person erhalten Sie E-Mail-Benachrichtigungen, wenn Ihre Genehmigung einer Anforderung aussteht. Wählen Sie zum Anzeigen dieser Anforderungen im PIM-Portal im Dashboard (in der neuen Navigation) in der linken Navigationsleiste die Registerkarte „Ausstehende Genehmigungsanforderungen“.

![](media/azure-ad-pim-approval-workflow/image023.png)

Dort sehen Sie eine Liste der Anforderungen mit ausstehender Genehmigung:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Genehmigen oder Ablehnen von Anforderungen von Rechteerweiterungen für Rollen (einzeln und/oder mehrere)

Wählen Sie die Anforderungen aus, die Sie genehmigen oder ablehnen möchten, und klicken Sie auf die entsprechende Schaltfläche in der Aktionsleiste:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>Abgeben einer Begründung für die Genehmigung/Ablehnung

Dadurch wird ein neues Blatt zum gleichzeitigen Genehmigen oder Ablehnen mehrerer Anforderungen geöffnet. Geben Sie eine Begründung für Ihre Entscheidung ein, und klicken Sie am unteren Rand des Blatts auf „Genehmigen“ (oder „Ablehnen“):

![](media/azure-ad-pim-approval-workflow/image029.png)

Nach Abschluss des Anforderungsprozesses spiegelt das Statussymbol die Entscheidung wider, die Sie getroffen haben (in diesem Beispiel die Genehmigung):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Anfordern der Aktivierung einer Rolle, die genehmigt werden muss

Die Anforderung der Aktivierung einer Rolle, die genehmigt werden muss, kann entweder von der alten PIM-Navigation oder der neuen Navigation aus initiiert werden, da der Prozess der Rollenaktivierung gleich bleibt. Wählen Sie einfach eine Rolle aus der Liste der zu aktivierenden Rollen:

![](media/azure-ad-pim-approval-workflow/image033.png)

Wenn eine privilegierte Rolle Multi-Factor Authentication erfordert, werden Sie aufgefordert, diese Aufgabe zuerst auszuführen:

![](media/azure-ad-pim-approval-workflow/image035.png)

Klicken Sie nach Abschluss der Aufgabe auf „Aktivieren“, und geben Sie eine Begründung ab (falls erforderlich):

![](media/azure-ad-pim-approval-workflow/image037.png)

Der Anforderer erhält eine Benachrichtigung, dass die Genehmigung der Anforderung aussteht:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a>Anzeigen des Status Ihrer Aktivierungsanforderung

Der Zugriff auf die Anzeige des Status einer ausstehenden Aktivierungsanforderung muss über die neue Navigation erfolgen. Wählen Sie in der linken Navigationsleiste die Registerkarte „My Requests“ (Meine Anforderungen) aus:

![](media/azure-ad-pim-approval-workflow/image041.png)

Der standardmäßige Anforderungsstatus ist „Ausstehend“, aber Sie können umschalten, um alle oder die abgelehnten Anforderungen anzuzeigen.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Fertigstellen Ihrer Aufgabe in Azure AD, wenn die Aktivierung genehmigt wurde

Sobald die Anforderung genehmigt ist, ist die Rolle aktiv, und Sie können mit jeder Arbeit fortfahren, die diese Rolle erfordert.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Nächste Schritte

Ihr Feedback ist für uns wichtig. Bitte teilen Sie uns Ihr Feedback und Ihre Kommentare mit!

