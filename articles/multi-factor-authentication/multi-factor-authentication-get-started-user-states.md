---
title: Benutzerstatus in Microsoft Azure Multi-Factor Authentication
description: "Erfahren Sie etwas über Benutzerstatus in Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 999361daa2faebe3e88cab0b6085a938d6f40e9d
ms.openlocfilehash: 3079969783f589cb90f17eac116c5d57884db49f
ms.lasthandoff: 02/17/2017


---
# <a name="user-states-in-azure-multi-factor-authentication"></a>Benutzerstatus in Azure Multi-Factor Authentication
Benutzerkonten in Azure Multi-Factor Authentication können die folgenden drei Zustände aufweisen:

| Zustand | Beschreibung | Nicht-Browser-Apps betroffen | 
|:---:|:---:|:---:|
| Deaktiviert |Der Standardstatus für einen neuen Benutzer, der nicht für Azure Multi-Factor Authentication (MFA) registriert ist. |Nein |
| Aktiviert |Der Prozess der Registrierung für Azure MFA für den Benutzer wurde begonnen, aber noch nicht abgeschlossen. Der Benutzer wird aufgefordert, sich bei der nächsten Anmeldung zu registrieren. |Nein.  Sie werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist. |
| Erzwungen |Der Benutzer wurde registriert und hat den Registrierungsprozess für Azure MFA abgeschlossen. |Ja.  Für Apps sind App-Kennwörter erforderlich. |

## <a name="changing-a-user-state"></a>Ändern eines Benutzerstatus
Der Status eines Benutzers gibt an, ob ein Administrator den Benutzer für Azure MFA registriert hat und ob der Registrierungsprozess abgeschlossen ist.

Der anfängliche Status lautet für alle Benutzer *Deaktiviert*. Wenn Sie Benutzer für Azure MFA registrieren, ändert sich der Status zu *Aktiviert*. Wenn aktivierte Benutzer sich anmelden und den Registrierungsprozess abschließen, ändert sich ihr Status zu *Erzwungen*.  

### <a name="view-user-states"></a>Anzeigen von Benutzerstatus

Führen Sie die folgenden Schritte aus, um auf die Seite zuzugreifen, auf der Sie Benutzerstatus anzeigen und verwalten können:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis für den Benutzer aus, den Sie anzeigen möchten.
   ![Verzeichnis auswählen – Screenshot](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Wählen Sie **Benutzer** aus.
5. Wählen Sie unten auf der Seite **Multi-Factor Authentication verwalten** aus. 
   ![„Multi-Factor Authentication verwalten“ auswählen – Screenshot](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Es wird eine neue Registerkarte geöffnet, auf der die Benutzerstatus angezeigt werden.
   ![Azure Multi-Factor Authentication-Benutzerstatus – Screenshot](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-state-from-disabled-to-enabled"></a>Ändern des Status von „Deaktiviert“ zu „Aktiviert“

1. Verwenden Sie die obigen Schritte, um die Seite mit den Multi-Factor Authentication-Benutzern zu öffnen. 
2. Suchen Sie den Benutzer, den Sie für Azure MFA aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. Vergewissern Sie sich, dass der Status **Deaktiviert** lautet.
   ![Benutzer suchen – Screenshot](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Aktivieren Sie das Kontrollkästchen neben dem gewünschten Namen.
4. Klicken Sie auf der rechten Seite unter QuickSteps auf **Aktivieren**.
   ![Ausgewählten Benutzer aktivieren – Screenshot](./media/multi-factor-authentication-get-started-cloud/user1.png)
5. Wählen Sie **Multi-Factor Authentication aktivieren** aus.
   ![Multi-Factor Authentication aktivieren – Screenshot](./media/multi-factor-authentication-get-started-cloud/enable2.png)
6. Beachten Sie, dass sich der Status des Benutzers von **Deaktiviert** zu **Aktiviert** geändert hat.
   ![Benutzer ist jetzt aktiviert – Screenshot](./media/multi-factor-authentication-get-started-cloud/user.png)

Nachdem Sie Benutzer aktiviert haben, sollten Sie diese per E-Mail benachrichtigen. Informieren Sie die Benutzer darüber, dass sie bei der nächsten Anmeldung aufgefordert werden, sich zu registrieren, und dass die zweistufige Überprüfung bei einigen nicht browserbasierten Apps möglicherweise nicht funktioniert. Sie können auch einen Link zu unserem [Azure MFA-Leitfaden für Endbenutzer](./end-user/multi-factor-authentication-end-user.md) in die E-Mail einfügen. 

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>So ändern Sie den Status von „Aktiviert/Erzwungen“ in „Deaktiviert“

1. Verwenden Sie die Schritte unter [Anzeigen von Benutzerstatus](#view-user-states), um die Seite mit den Multi-Factor Authentication-Benutzern zu öffnen.
6. Suchen Sie den Benutzer, den Sie deaktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. Stellen Sie sicher, dass der Status entweder **Aktiviert** oder **Erzwungen** lautet.
7. Aktivieren Sie das Kontrollkästchen neben dem gewünschten Namen.
8. Klicken Sie auf der rechten Seite unter QuickSteps auf **Deaktivieren**.
   ![Benutzer deaktivieren – Screenshot](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
9. Sie werden aufgefordert, die Aktion zu bestätigen. Klicken Sie auf **Ja**.
10. Wenn der Benutzer erfolgreich deaktiviert wurde, erhalten Sie eine entsprechende Meldung. Klicken Sie auf **Schließen**.


