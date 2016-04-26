<properties
	pageTitle="Erste Schritte mit Azure Active Directory Premium"
	description="In diesem Thema wird die Registrierung für die Premium Edition von Azure Active Directory beschrieben."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo" 
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/18/2016"
	ms.author="markvi"/>

# Erste Schritte mit Azure Active Directory Premium

Azure Active Directory ist in drei Editions verfügbar: Free, Basic und Premium. Die kostenlose Free Edition ist in einem Azure- oder Office 365-Abonnement enthalten. Die Basic und Premium Editions sind über ein [Microsoft Enterprise Agreement](https://www.microsoft.com/de-DE/licensing/licensing-programs/enterprise.aspx) oder das [Open Volume License](https://www.microsoft.com/de-DE/licensing/licensing-programs/open-license.aspx)-Programm erhältlich. Azure- und Office 365-Abonnenten können Active Directory Premium auch online erwerben. Für den Kauf [hier anmelden](https://portal.office.com/Commerce/Catalog.aspx).

> [AZURE.NOTE]
Die Azure Active Directory Premium und Basic Editions stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Wenn Sie weitere Informationen benötigen, kontaktieren Sie uns im [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

Azure Active Directory Premium ist auch in der **Enterprise Mobility Suite** enthalten. Die Enterprise Mobility Suite stellt für Unternehmen eine kostengünstige Möglichkeit dar, die folgenden Dienste unter einem Lizenzplan gemeinsam zu nutzen:

- Active Directory Premium 
- Azure Rights Management
- Microsoft Intune


Weitere Informationen finden Sie auf der Website [Enterprise Mobility Suite](https://www.microsoft.com/de-DE/server-cloud/enterprise-mobility/overview.aspx).

Führen Sie die Schritte in den nächsten Abschnitten aus, wenn Sie die Azure Active Directory Premium-Features gleich verwenden möchten. Diese Schritte gelten auch für die Azure Active Directory Basic Edition.

## Schritt 1: Registrieren Sie sich für Active Directory Premium

Informationen zur Registrierung finden Sie auf der Website [Volume Licensing](http://www.microsoft.com/de-DE/licensing/how-to-buy/how-to-buy.aspx).

## Schritt 2: Aktivieren Sie Ihren Lizenzplan

Sie erhalten eine E-Mail als Bestätigung, wenn Ihr erster Erwerb eines Lizenzplans über das Enterprise Volume Licensing-Programm von Microsoft abgeschlossen wurde. Sie benötigen diese E-Mail zum Aktivieren Ihres ersten Lizenzplans. Bei jedem nachfolgenden Kauf für dieses Verzeichnis werden die Lizenzen automatisch im gleichen Verzeichnis aktiviert.

Klicken Sie zum Starten der Aktivierung entweder auf **Anmelden** oder **Registrieren**.


![][1]

Wenn Sie bereits über einen Mandanten verfügen, klicken Sie auf **Anmelden**, um sich bei Ihrem vorhandenen Administratorkonto anzumelden. Die Anmeldung muss über die Anmeldeinformationen des globalen Administrators des Verzeichnisses erfolgen, in dem die Lizenzen aktiviert werden sollen.

Wenn Sie einen neuen Azure Active Directory-Mandanten für die Verwendung mit Ihrem Lizenzplan erstellen möchten, klicken Sie auf **Registrieren**, um das Dialogfeld **Kontoprofil erstellen** zu öffnen.

![][2]

Nach Abschluss des Vorgangs wird das folgende Dialogfeld angezeigt, um zu bestätigen, dass die Aktivierung des Lizenzplans für Ihren Mandanten durchgeführt wurde.

![][3]

## Schritt 3: Aktivieren Sie den Zugriff auf Azure Active Directory

Wenn die Lizenzen für Ihr Verzeichnis bereitgestellt wurden, wird eine **Begrüßungs-E-Mail** an Ihre Adresse gesendet. Mit der E-Mail wird bestätigt, dass Sie mit der Verwaltung der Lizenzen und Features von Azure Active Directory Premium oder der Enterprise Mobility Suite beginnen können.

Wenn Sie Microsoft Azure bereits verwendet haben, können Sie mit [http://manage.windowsazure.com](http://manage.windowsazure.com) fortfahren, um neue Lizenzen zuzuweisen (siehe [Schritt 4](#step-4-assign-license-to-user-accounts)).

Falls Microsoft Azure neu für Sie ist, können Sie entweder in der E-Mail auf **Anmelden** klicken oder zur Seite für die [Aktivierung des Zugriffs auf Azure Active Directory](https://account.windowsazure.com/signup?offer=MS-AZR-0110P) wechseln. Bei beiden Methoden werden Sie durch eine Reihe von Schritten geführt, um über das klassische Azure-Portal auf Ihr Verzeichnis zuzugreifen.

![][4]

Nach der erfolgreichen Anmeldung müssen Sie die Schritte für die zweistufige Authentifizierung ausführen. Geben Sie auf dem Bildschirm Ihre Mobiltelefonnummer ein, und bestätigen Sie diese. Wenn Sie die Überprüfung per Mobiltelefon abgeschlossen haben, können Sie Ihren Zugriff auf Azure Active Directory aktivieren, indem Sie auf **Registrieren** klicken.

![][5]

Die Aktivierung kann einige Minuten dauern. Nachdem der Zugriff aktiviert wurde, wird die braune Leiste ausgeblendet, und Sie können auf **Portal** klicken.

![][6]

In diesem Fall ist Ihr Azure-Zugriff auf Azure Active Directory beschränkt.

![][7]

Möglicherweise hatten Sie aufgrund einer früheren Verwendung bereits Zugriff auf Azure. Außerdem können Sie Ihren Azure Active Directory-Zugriff durch Aktivierung weiterer Azure-Abonnements auf einen vollständigen Azure-Zugriff erweitern. In diesen Fällen verfügt das klassische Azure-Portal über mehr Funktionen.

![][8]

Wenn Sie versuchen, den Zugriff auf Azure Active Directory zu aktivieren, bevor Ihnen die Begrüßungs-E-Mail zugestellt wurde, erhalten Sie unter Umständen die unten angegebene Fehlermeldung. Versuchen Sie die Aktivierung in diesem Fall erneut wenige Minuten nach Erhalt der E-Mail.

![][9]

Neue Administratoren innerhalb Ihres Abonnements können ihren Zugriff auf das klassische Azure-Portal ebenfalls über diesen Link aktivieren.

## Schritt 4: Weisen Sie die Lizenzen Benutzerkonten zu

Bevor Sie den erworbenen Plan verwenden können, müssen Sie die Lizenzen manuell Benutzerkonten Ihres Unternehmens zuweisen, damit diese das reichhaltige Featureangebot der Premium Edition nutzen können. Führen Sie zur Zuweisung von Lizenzen zu Benutzern die folgenden Schritte aus, so dass diese die Features der Premium Edition von Azure Active Directory nutzen können.

Zuweisen von Lizenzen zu Benutzern:

1. Melden Sie sich beim klassischen Azure-Portal als globaler Administrator des Verzeichnisses an, das Sie anpassen möchten.
2. Klicken Sie auf **Active Directory**, und wählen Sie dann das Verzeichnis aus, in dem Sie Lizenzen zuweisen möchten.
3. Klicken Sie auf die Registerkarte **Lizenzen**, wählen Sie **Active Directory Premium** oder **Enterprise Mobility Suite** aus, und klicken Sie dann auf **Zuweisen**.

    ![][10]

4. Wählen Sie im Dialogfeld die Benutzer aus, denen Sie Lizenzen zuweisen möchten, und klicken Sie dann auf das Häkchen, um die Änderungen zu speichern.

    ![][11]

## Lizenzeinschränkungen

Es gibt Lizenzpläne, die in anderen Lizenzplänen enthalten sind oder andere Lizenzpläne enthalten. Normalerweise kann einem Benutzer kein Lizenzplan zugewiesen werden, der ihm bereits zugewiesen wurde. Wenn Sie einem Benutzer einen Lizenzplan zuweisen möchten, der andere Lizenzpläne enthält, die dem Benutzer bereits zugewiesen sind, müssen Sie die bereits zugewiesenen Lizenzpläne zunächst entfernen.

## Lizenzanforderungen

Wenn Sie einem Benutzer eine Lizenz zuweisen, können Sie in den Eigenschaften des Benutzerkontos einen primären Verwendungsort angeben. Wenn kein solcher Verwendungsort angegeben ist, wird dem Benutzer automatisch der Standort des Mandanten zugewiesen.

![][12]

Die Verfügbarkeit der Dienste und Features eines Microsoft Clouddiensts variieren je nach Land und Region. Ein Dienst wie Voice over IP (VoIP) kann in einem Land bzw. einer Region verfügbar sein, in einem anderen hingegen nicht. Die Features eines Diensts können aus rechtlichen Gründen in einigen Ländern oder Regionen eingeschränkt sein. Um festzustellen, ob ein Dienst oder ein Feature für Sie uneingeschränkt oder mit Einschränkungen verfügbar ist, suchen Sie Ihr Land bzw. Ihre Region auf der Website mit den Lizenzeinschränkungen des jeweiligen Diensts.

## Nächste Schritte

- [Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite](active-directory-add-company-branding.md)
- [Anzeigen Ihrer Zugriffs- und Nutzungsberichte](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-get-started-premium/MOLSEmail.png
[2]: ./media/active-directory-get-started-premium/MOLSAccountProfile.png
[3]: ./media/active-directory-get-started-premium/MOLSThankYou.png
[4]: ./media/active-directory-get-started-premium/AADEmail.png
[5]: ./media/active-directory-get-started-premium/SignUppage.png
[6]: ./media/active-directory-get-started-premium/Subscriptionspage.png
[7]: ./media/active-directory-get-started-premium/Premiuminportal.png
[8]: ./media/active-directory-get-started-premium/Premiuminportal_large.png
[9]: ./media/active-directory-get-started-premium/Signuppage_oops.png
[10]: ./media/active-directory-get-started-premium/contosolicenseplan.png
[11]: ./media/active-directory-get-started-premium/Assignlicensespicker.png
[12]: ./media/active-directory-get-started-premium/Usagelocation.png

<!---HONumber=AcomDC_0420_2016-->