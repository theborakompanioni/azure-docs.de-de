<properties 
	pageTitle="Erste Schritte mit Microsoft Azure Multi-Factor Authentication-Anbietern" 
	description="Lernen Sie, wie Sie einen Azure Multi-Factor Authentication-Anbieter erstellen." 
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
	ms.date="08/04/2016" 
	ms.author="billmath"/>



# Erste Schritte mit Azure Multi-Factor Authentication-Anbietern
Multi-Factor Authentication ist standardmäßig für globale Administratoren, die mit Azure Active Directory arbeiten, und für Office 365-Benutzer verfügbar. Falls Sie die [erweiterten Funktionen](multi-factor-authentication-whats-next.md) nutzen möchten, müssen Sie die Vollversion von Azure MFA erwerben.

> [AZURE.NOTE]  Ein Azure Multi-Factor Authentication-Anbieter wird verwendet, um die Features zu nutzen, die mit der Vollversion von Azure MFA bereitgestellt werden. Er wird für Benutzer verwendet, die **keine Lizenzen über Azure MFA, Azure AD Premium oder EMS besitzen**. Azure MFA, Azure AD Premium und EMS enthalten standardmäßig die Vollversion von Azure MFA. Wenn Sie im Besitz von Lizenzen sind, benötigen Sie keinen Azure Multi-Factor Authentication-Anbieter.

Ein Azure Multi-Factor Authentication-Anbieter ist erforderlich, wenn Sie das SDK herunterladen möchten.

> [AZURE.IMPORTANT]  Falls Sie das SDK herunterladen möchten, müssen Sie auch dann einen Azure Multi-Factor Authentication-Anbieter erstellen, wenn Sie über Azure MFA-, AAD Premium- oder EMS-Lizenzen verfügen. Falls Sie zu diesem Zweck einen Azure Multi-Factor Authentication-Anbieter erstellen und bereits im Besitz von Lizenzen sind, müssen Sie den Anbieter mit dem Modell **Pro aktiviertem Benutzer** erstellen und mit dem Verzeichnis verknüpfen, in dem die Azure MFA-, Azure AD Premium- oder EMS-Lizenzen enthalten sind. So wird sichergestellt, dass Ihnen nur Kosten berechnet werden, wenn die Anzahl von eindeutigen Benutzern, die das SDK verwenden, die Anzahl von Lizenzen in Ihrem Besitz übersteigt.
 
Führen Sie die folgenden Schritte aus, um einen Azure Multi-Factor Authentication-Anbieter zu erstellen.

## So erstellen Sie einen Anbieter für mehrstufige Authentifizierung
--------------------------------------------------------------------------------

1. Melden Sie sich beim **klassischen Azure-Portal** als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory** aus.
3. Wählen Sie oben auf der Seite „Active Directory“ die Option **Anbieter für mehrstufige Authentifizierung**. ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Klicken Sie dann auf **Neu**. ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Wählen Sie unter **App Services** den Eintrag **Multi-Factor Authentication-Anbieter** aus. ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Wählen Sie **Schnellerfassung**. ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Füllen Sie die folgenden Felder aus, und wählen Sie **Erstellen**.
	1. **Name**: Der Name des Multi-Factor Authentication-Anbieters
	2. **Nutzungsmodell**: Das Nutzungsmodell des Multi-Factor Authentication-Anbieters
		- Pro Authentifizierung: Ein Kaufmodell, bei dem die Kosten pro Authentifizierung berechnet werden. Wird in der Regel für Szenarien verwendet, in denen Azure Multi-Factor Authentication in einer kundenorientierten Anwendung verwendet wird.
		- Pro aktiviertem Benutzer: Ein Kaufmodell, bei dem die Kosten pro aktiviertem Benutzer berechnet werden. Wird in der Regel für den Mitarbeiterzugriff auf Anwendungen wie Office 365 verwendet.
	2. **Verzeichnis**: Der Azure Active Directory-Mandant, dem der Multi-Factor Authentication-Anbieter zugeordnet ist. Bedenken Sie dabei Folgendes:
		- Zum Erstellen eines Anbieters für mehrstufige Authentifizierung benötigen Sie kein Azure AD-Verzeichnis. Lassen Sie das Feld einfach leer, wenn Sie nur den Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK verwenden möchten.
		- Der Multi-Factor Authentication-Anbieter muss einem Azure AD-Verzeichnis zugeordnet werden, damit die erweiterten Features genutzt werden können.
		- Azure AD Connect, AAD Sync oder DirSync ist nur erforderlich, wenn Sie Ihre lokale Active Directory-Umgebung mit einem Azure AD-Verzeichnis synchronisieren. Wenn Sie nur ein Azure AD-Verzeichnis verwenden, das nicht synchronisiert ist, ist dies nicht erforderlich. ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Nach dem Klicken auf „Erstellen“ wird der Multi-Factor Authentication-Anbieter erstellt und die folgende Meldung angezeigt: **Der Anbieter für Multi-Factor Authentication wurde erstellt**. Klicken Sie auf **OK**. ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)

<!---HONumber=AcomDC_0810_2016-->