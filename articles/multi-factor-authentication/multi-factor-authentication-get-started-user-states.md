---
title: Benutzerstatus in Microsoft Azure Multi-Factor Authentication
description: "Erfahren Sie etwas über Benutzerstatus in Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: 1869b7a4ef42536a3cd909ba2983ae0fe97185a9
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Vorgehensweise zum Erzwingen einer Überprüfung in zwei Schritten für einen Benutzer oder eine Gruppe

Es gibt zwei Ansätze, um eine Überprüfung in zwei Schritten zu erzwingen. Die erste Option besteht darin, jeden einzelnen Benutzer für Azure Multi-Factor Authentication (MFA) zu aktivieren. Wenn Benutzer einzeln aktiviert werden, führen sie immer eine Überprüfung in zwei Schritten durch (bis auf einige Ausnahmen wie etwa, wenn sie sich von vertrauenswürdigen IP-Adressen aus anmelden oder wenn die gespeicherte Gerätefunktion aktiviert ist). Die zweite Möglichkeit besteht darin, eine Richtlinie für bedingten Zugriff einzurichten, die unter bestimmten Umständen eine Überprüfung in zwei Schritten erfordert.

>[!TIP] 
>Wählen Sie eine der folgenden Methoden, um die Überprüfung in zwei Schritten zu erzwingen. Wählen Sie jedoch nicht beide Methoden aus. Durch die Aktivierung eines Benutzers für Azure MFA werden alle Richtlinien für bedingten Zugriff überschrieben.

## <a name="which-option-is-right-for-you"></a>Auswählen der für Sie geeigneten Option

Die **Aktivierung von Azure MFA durch Ändern der Benutzerstatus** ist der herkömmliche Ansatz, um die Überprüfung in zwei Schritten zu erzwingen. Dies funktioniert sowohl für Azure MFA in der Cloud als auch für Azure MFA Server. Bei allen Benutzern, die Sie aktivieren, ist der Ablauf identisch: Jedes Mal, wenn sie sich anmelden, müssen sie die Überprüfung in zwei Schritten ausführen. Durch die Aktivierung eines Benutzers werden alle Richtlinien für bedingten Zugriff überschrieben, die diesen Benutzer betreffen könnten. 

Die **Aktivierung von Azure MFA mit einer Richtlinie für bedingten Zugriff** ist ein flexiblerer Ansatz, um die Überprüfung in zwei Schritten zu erzwingen. Er funktioniert jedoch nur bei Azure MFA in der Cloud. Zudem ist der bedingte Zugriff eine [kostenpflichtige Funktion von Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Sie können Richtlinien für bedingten Zugriff erstellen, die sowohl für Gruppen als auch für einzelne Benutzer gelten. Gruppen, bei denen ein hohes Risiko besteht, können umfassendere Einschränkungen zugewiesen werden. Alternativ kann die Überprüfung in zwei Schritten nur für Cloud-Apps mit hohem Risiko erzwungen und für Apps mit niedrigem Risiko übersprungen werden. 

Bei beiden Optionen werden Benutzer aufgefordert, sich für Azure Multi-Factor Authentication zu registrieren, wenn Sie sich zum ersten Mal nach der Aktivierung der Anforderungen anmelden. Beide Optionen funktionieren auch mit den konfigurierbaren [Azure Multi-Factor Authentication-Einstellungen](multi-factor-authentication-whats-next.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Aktivieren von Azure MFA durch Ändern des Benutzerstatus

Benutzerkonten in Azure Multi-Factor Authentication können die folgenden drei Zustände aufweisen:

| Status | Beschreibung | Nicht-Browser-Apps betroffen |
|:---:|:---:|:---:|
| Deaktiviert |Der Standardstatus für einen neuen Benutzer, der nicht für Azure Multi-Factor Authentication (MFA) registriert ist. |Nein |
| Aktiviert |Der Prozess der Registrierung für Azure MFA für den Benutzer wurde begonnen, aber noch nicht abgeschlossen. Der Benutzer wird aufgefordert, sich bei der nächsten Anmeldung zu registrieren. |Nein.  Sie werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist. |
| Erzwungen |Der Benutzer wurde registriert und hat den Registrierungsprozess für Azure MFA abgeschlossen. |Ja.  Für Apps sind App-Kennwörter erforderlich. |

Der Status eines Benutzers gibt an, ob ein Administrator den Benutzer für Azure MFA registriert hat und ob der Registrierungsprozess abgeschlossen ist.

Der anfängliche Status lautet für alle Benutzer *Deaktiviert*. Wenn Sie Benutzer für Azure MFA registrieren, ändert sich der Status zu *Aktiviert*. Wenn aktivierte Benutzer sich anmelden und den Registrierungsprozess abschließen, ändert sich ihr Status zu *Erzwungen*.  

### <a name="view-the-status-for-a-user"></a>Anzeigen des Status eines Benutzers

Führen Sie die folgenden Schritte aus, um auf die Seite zuzugreifen, auf der Sie Benutzerstatus anzeigen und verwalten können:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer**.
3. Wählen Sie **Multi-Factor Authentication** aus.
   ![Auswählen von Multi-Factor Authentication](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Es wird eine neue Seite geöffnet, auf der die Benutzerstatus angezeigt werden.
   ![Azure Multi-Factor Authentication-Benutzerstatus – Screenshot](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Ändern des Status eines Benutzers

1. Verwenden Sie die obigen Schritte, um die Seite mit den Multi-Factor Authentication-Benutzern zu öffnen.
2. Suchen Sie den Benutzer, den Sie für Azure MFA aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. 
   ![Benutzer suchen – Screenshot](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Aktivieren Sie das Kontrollkästchen neben dem gewünschten Namen.
4. Wählen Sie auf der rechten Seite unter QuickSteps **Aktivieren** oder **Deaktivieren**.
   ![Ausgewählten Benutzer aktivieren – Screenshot](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >Wenn sich Benutzer für Azure MFA registrieren, wechselt der Benutzerstatus *Aktiviert* automatisch zu *Erzwungen*. Sie sollten den Benutzerstatus nicht manuell in „Erzwungen“ ändern. 

5. Bestätigen Sie Ihre Auswahl im Popupfenster, das geöffnet wird. 

Nachdem Sie Benutzer aktiviert haben, sollten Sie diese per E-Mail benachrichtigen. Teilen Sie ihnen mit, dass sie zur Registrierung aufgefordert werden, wenn sie sich beim nächsten Mal anmelden. Wenn Ihre Organisation zudem andere Apps verwendet, die keine Browser-Apps sind und die moderne Authentifizierung nicht unterstützen, müssen sie App-Kennwörter erstellen. Sie können auch einen Link zu unserem [Azure MFA-Leitfaden für Endbenutzer](./end-user/multi-factor-authentication-end-user.md) in die E-Mail einfügen.

### <a name="use-powershell"></a>Verwenden von PowerShell
Um den Benutzerstatus mit [Azure AD PowerShell](/powershell/azure/overview) zu ändern, ändern Sie `$st.State`. Es gibt drei mögliche Status:

* Aktiviert
* Erzwungen
* Deaktiviert  

Ändern Sie nicht direkt den Status *Erzwungen* für Benutzer. Andere Apps als Browser-Apps funktionieren nicht mehr, da der Benutzer die MFA-Registrierung durchlaufen und ein [App-Kennwort](multi-factor-authentication-whats-next.md#app-passwords) erhalten hat. 

PowerShell ist eine gute Wahl, wenn Sie Benutzer massenweise aktivieren müssen. Erstellen Sie ein PowerShell-Skript, das eine Schleife durch eine Liste von Benutzern durchführt und sie aktiviert:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Beispiel:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Aktivieren von Azure MFA mit einer Richtlinie für bedingten Zugriff

Der bedingte Zugriff ist eine kostenpflichtige Funktion von Azure Active Directory mit zahlreichen möglichen Konfigurationsoptionen. In den folgenden Schritten werden Sie durch eine Methode zum Erstellen einer Richtlinie geführt. Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Wählen Sie **Azure Active Directory** > **Bedingter Zugriff**.
3. Wählen Sie **Neue Richtlinie**.
4. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**. Geben Sie über die Registerkarten **Einschließen** und **Ausschließen** an, welche Benutzer und Gruppen durch die Richtlinie verwaltet werden.
5. Klicken Sie unter **Zuweisungen** auf **Cloud-Apps**. Wählen Sie **Alle Cloud-Apps**, um alle einzuschließen.
6. Klicken Sie unter **Zugriffskontrollen** auf **Gewähren**. Wählen Sie **Mehrstufige Authentifizierung anfordern**.
7. Legen Sie **Richtlinie aktivieren** auf **Ein** fest, und wählen Sie dann **Speichern**.

Mit den anderen Optionen in der Richtlinie für bedingten Zugriff können Sie genau angeben, wann die Überprüfung in zwei Schritten erforderlich sein soll. Beispielsweise könnten Sie eine Richtlinie für den folgenden Fall erstellen: Wenn Auftragnehmer von nicht vertrauenswürdigen Netzwerken aus über Geräte, die nicht in Domänen eingebunden sind, auf unsere Beschaffungs-App zugreifen, müssen sie die Überprüfung in zwei Schritten durchführen. 

## <a name="next-steps"></a>Nächste Schritte

- Erhalten Sie Tipps zu [bewährten Methoden für den bedingten Zugriff](../active-directory/active-directory-conditional-access-best-practices.md).

- Verwalten Sie Multi-Factor Authentication-Einstellungen für [Ihre Benutzer und deren Geräte](multi-factor-authentication-manage-users-and-devices.md).
