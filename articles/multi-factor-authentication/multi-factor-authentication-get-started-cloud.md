<properties 
	pageTitle="Erste Schritte mit Microsoft Azure Multi-Factor Authentication in der Cloud" 
	description="Auf dieser Seite zu Microsoft Azure Multi-Factor Authentication werden die ersten Schritte mit Azure MFA in der Cloud beschrieben." 
	services="multi-factor-authentication" 
	documentationCenter="" 
	authors="billmath" 
	manager="stevenpo" 
	editor="curtand"/>

<tags 
	ms.service="multi-factor-authentication" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="05/12/2016" 
	ms.author="billmath"/>

# Erste Schritte mit Azure Multi-Factor Authentication in der Cloud
In diesem Artikel werden die ersten Schritte mit Azure Multi-Factor Authentication in der Cloud beschrieben.

> [AZURE.NOTE]  Die folgende Dokumentation enthält Informationen zum Aktivieren von Benutzern mithilfe des klassischen Azure-Portals. Weitere Informationen zum Einrichten von Azure Multi-Factor Authentication für Office 365-Benutzer finden Sie unter [Einrichten der mehrstufigen Authentifizierung für Office 365-Benutzer](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=de-DE&rs=de-DE&ad=US).

![MFA in der Cloud](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## Voraussetzungen
Um Azure Multi-Factor Authentication für Ihre Benutzer aktivieren zu können, müssen zunächst folgende Voraussetzungen erfüllt werden:




- [Registrieren Sie sich für ein Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/): Falls Sie noch nicht über ein Azure-Abonnement verfügen, müssen Sie sich für ein Abonnement registrieren. Wenn Sie die Verwendung von Azure MFA testen möchten, können Sie ein Testabonnement verwenden.
2. [Erstellen Sie einen Multi-Factor Authentication-Anbieter](multi-factor-authentication-get-started-auth-provider.md), und weisen Sie ihn Ihrem Verzeichnis zu, oder [weisen Sie Benutzern Lizenzen zu](multi-factor-authentication-get-started-assign-licenses.md). 

> [AZURE.NOTE]  Lizenzen sind für Benutzer verfügbar, die über Azure MFA, Azure AD Premium oder Enterprise Mobility Suite (EMS) verfügen. MFA ist in Azure AD Premium und in EMS enthalten. Wenn Sie über genügend Lizenzen verfügen, müssen Sie keinen Authentifizierungsanbieter erstellen.
		

## Aktivieren von Multi-Factor Authentication für Benutzer
Zur Aktivierung von Multi-Factor Authentication für einen Benutzer muss lediglich der Zustand des Benutzers von „Deaktiviert“ in „Aktiviert“ geändert werden. Weitere Informationen zu Benutzerzuständen finden Sie unter [User States in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md) (Benutzerzustände in Azure Multi-Factor Authentication).

Gehen Sie wie unten angegeben vor, um MFA für die Benutzer zu aktivieren.

### So aktivieren Sie Multi-Factor Authentication
--------------------------------------------------------------------------------
1.  Melden Sie sich beim klassischen Azure-Portal als Administrator an.
2.  Klicken Sie im linken Bereich auf **Active Directory**.
3.  Klicken Sie unter **Verzeichnis** auf das Verzeichnis für den Benutzer, den Sie aktivieren möchten. ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Klicken Sie oben auf **Benutzer**.
5.  Klicken Sie unten auf der Seite auf **Multi-Factor Auth verwalten**. ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Dadurch wird eine neue Browserregisterkarte geöffnet. Suchen Sie den Benutzer, für den Sie Multi-Factor Authentication aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. Vergewissern Sie sich, dass der Status **Deaktiviert** lautet. ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Aktivieren Sie das Kontrollkästchen neben dem gewünschten Namen.
7.  Klicken Sie auf der rechten Seite auf **Aktivieren**. ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klicken Sie auf **Multi-Factor Auth aktivieren**. ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Der Zustand des Benutzers wird von **Deaktiviert** in **Aktiviert** geändert. ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Es wird empfohlen, dass Sie den Benutzer per E-Mail benachrichtigen, nachdem Sie ihn aktiviert haben. Diese sollte auch eine Information dazu enthalten, wie Benutzer ihre Nicht-Browser-Apps verwenden können, um eine Sperrung zu vermeiden.


## Automatisieren der Aktivierung von Multi-Factor Authentication mithilfe von PowerShell

Sie können den Zustand[](multi-factor-authentication-whats-next.md) mithilfe von [Azure AD PowerShell](powershell-install-configuremd) auf folgende Weise ändern. `$st.State` kann auf einen der folgenden Zustände festgelegt werden:


- Aktiviert
- Erzwungen
- Deaktiviert  

> [AZURE.IMPORTANT]  Hinweis: Bei einem direkten Wechsel von „Deaktiviert“ zu „Erzwungen“ können ältere Authentifizierungsclients nicht mehr verwendet werden, da der Benutzer die MFA-Registrierung nicht durchlaufen und kein [App-Kennwort](multi-factor-authentication-whats-next.md#app-passwords) bezogen hat. Sollten Sie über ältere Authentifizierungsclients verfügen und App-Kennwörter benötigen, empfiehlt es sich, von „Deaktiviert“ zu „Aktiviert“ zu wechseln. So können sich Benutzer registrieren und ihr App-Kennwort beziehen.
		
		$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
		$st.RelyingParty = "*"
		$st.State = “Enabled”
		$sta = @($st)
		Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Die Verwendung von PowerShell ist eine Option für die Massenaktivierung von Benutzern. Derzeit steht im Azure-Portal keine Option für eine Massenaktivierung zur Verfügung, sodass jeder Benutzer einzeln ausgewählt werden muss. Dies kann bei einer großen Anzahl von Benutzern äußerst aufwendig sein. Mithilfe eines PowerShell-Skripts mit den obigen Angaben können Sie eine Liste mit Benutzern durchlaufen und die Benutzer aktivieren. Beispiel:
    
    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
    	$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
    	$st.RelyingParty = "*"
    	$st.State = “Enabled”
    	$sta = @($st)
    	Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Weitere Informationen zu Benutzerzuständen finden Sie unter [User States in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md) (Benutzerzustände in Azure Multi-Factor Authentication).

## Nächste Schritte
Nachdem Sie die Multi-Factor Authentication in der Cloud eingerichtet haben, können Sie die Bereitstellung konfigurieren und einrichten. Weitere Informationen finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication].

<!---HONumber=AcomDC_0518_2016-->