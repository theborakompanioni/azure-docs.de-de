<properties
	pageTitle="Erste Schritte mit Azure Active Directory Premium"
	description="In diesem Thema wird beschrieben, wie Sie sich über die Volumenlizenzierung-Website für die Azure Active Directory Premium Edition registrieren."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="femila" 
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/25/2016"
	ms.author="markvi"/>

# Erste Schritte mit Azure Active Directory Premium


Es gibt mehrere Möglichkeiten, sich für Active Directory Premium zu registrieren:

**Azure oder Office 365**: Als Abonnent von Azure oder Office 365 können Sie Active Directory Premium online erwerben. Ausführliche Schritte finden Sie unter [How to Purchase Azure Active Directory Premium - Existing Customers](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-Existing-Customer) (Erwerben von Azure Active Directory Premium – Bestandskunden) oder [How to Purchase Azure Active Directory Premium - New Customers](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-New-Customers) (Erwerben von Azure Active Directory Premium – Neukunden).

**Enterprise Mobility + Security**: Enterprise Mobility + Security (früher „Enterprise Mobility Suite“) ist eine kostengünstige Lösung für Unternehmen, die die folgenden Dienste zusammen unter einem Lizenzplan verwenden möchten: Active Directory Premium, Azure Rights Management und Microsoft Intune. Weitere Informationen finden Sie auf der Website [Enterprise Mobility + Security](https://www.microsoft.com/de-DE/server-cloud/enterprise-mobility/overview.aspx). Klicken Sie [hier](https://portal.office.com/Signup/Signup.aspx?OfferId=2E63A04D-BE0B-4A0F-A8CF-407C1C299221&dl=EMS&ali=1#0), um eine kostenlose Testversion für 30 Tage zu erhalten.


**Microsoft-Volumenlizenzierung**: Azure Active Directory Premium ist per [Microsoft Enterprise Agreement](https://www.microsoft.com/de-DE/licensing/licensing-programs/enterprise.aspx) (250 Lizenzen oder mehr) oder das [Open Volume License](https://www.microsoft.com/de-DE/licensing/licensing-programs/open-license.aspx)-Programm (5 bis 250 Lizenzen) erhältlich.


In diesem Thema werden die ersten Schritte mit der Anwendung Azure Active Directory Premium beschrieben, die Sie im Rahmen des Volumenlizenzprogramms erworben haben. Falls Sie mit den verschiedenen Editionen von Azure Active Directory noch nicht vertraut sind, helfen Ihnen die Informationen unter [Azure Active Directory-Editionen](active-directory-editions.md) weiter.

> [AZURE.NOTE]
Die Azure Active Directory-Editionen Premium und Basic stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Wenn Sie weitere Informationen benötigen, kontaktieren Sie uns im [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).




## Schritt 1: Registrieren Sie sich für Active Directory Premium

Informationen zur Registrierung finden Sie unter [How to purchase through Volume Licensing](http://www.microsoft.com/de-DE/licensing/how-to-buy/how-to-buy.aspx) (Kauf per Volumenlizenzierung).



## Schritt 2: Aktivieren Sie Ihren Lizenzplan

Ist dies Ihr erster Kauf eines Lizenzplans über das Enterprise Volume Licensing-Programm von Microsoft? In diesem Fall erhalten Sie eine Bestätigungs-E-Mail, wenn Ihr Kauf abgeschlossen wurde. Sie benötigen diese E-Mail zum Aktivieren Ihres ersten Lizenzplans.

Bei jedem nachfolgenden Kauf für dieses Verzeichnis werden die Lizenzen automatisch im gleichen Verzeichnis aktiviert.



**Führen Sie einen der folgenden Schritte aus, um den Lizenzplan zu aktivieren:**


1. Klicken Sie zum Starten der Aktivierung entweder auf **Anmelden** oder **Registrieren**.

    ![Anmelden][1]



    - Wenn Sie bereits über einen Mandanten verfügen, klicken Sie auf **Anmelden**, um sich bei Ihrem vorhandenen Administratorkonto anzumelden. Sie müssen sich mit den Anmeldeinformationen des globalen Administrators des Verzeichnisses anmelden, in dem die Lizenzen aktiviert werden sollen.

    - Wenn Sie einen neuen Azure Active Directory-Mandanten für die Verwendung mit Ihrem Lizenzplan erstellen möchten, klicken Sie auf **Registrieren**, um das Dialogfeld **Kontoprofil erstellen** zu öffnen.

        ![Kontoprofil erstellen][2]

Nach Abschluss des Vorgangs wird das folgende Dialogfeld angezeigt, um zu bestätigen, dass die Aktivierung des Lizenzplans für Ihren Mandanten durchgeführt wurde.

![Bestätigung][3]

## Schritt 3: Aktivieren Sie den Zugriff auf Azure Active Directory

Wenn Sie Microsoft Azure bereits verwendet haben, können Sie mit [Schritt 4](#step-4-assign-license-to-user-accounts) fortfahren.

Wenn die Lizenzen für Ihr Verzeichnis bereitgestellt wurden, wird eine **Begrüßungs-E-Mail** an Ihre Adresse gesendet. Mit der E-Mail wird bestätigt, dass Sie mit der Verwaltung der Lizenzen und Features von Azure Active Directory Premium oder der Enterprise Mobility Suite beginnen können.

Wenn Sie versuchen, den Zugriff auf Azure Active Directory zu aktivieren, bevor Ihnen die Begrüßungs-E-Mail zugestellt wurde, erhalten Sie die unten angegebene Fehlermeldung.

![Zugriff nicht verfügbar][9]

Wiederholen Sie den Aktivierungsversuch in diesem Fall in wenigen Minuten, wenn Sie die E-Mail erhalten haben.

Neue Administratoren innerhalb Ihres Abonnements können ihren Zugriff auf das klassische Azure-Portal ebenfalls über diesen Link aktivieren.






**Führen Sie die folgenden Schritte aus, um den Azure Active Directory-Zugriff zu aktivieren:**

1. Klicken Sie in der **Willkommens-E-Mail** auf **Anmelden**.
    
    ![Willkommens-E-Mail][4]

2. Nach der Anmeldung müssen Sie eine zweistufige Authentifizierung in Form einer mobilen Verifizierung durchführen:

    ![Mobile Verifizierung][5]

Die Aktivierung kann einige Minuten dauern. Nachdem der Zugriff aktiviert wurde, wird die braune Leiste ausgeblendet, und Sie können auf **Portal** klicken.

![Bitte warten Sie, bis die Einrichtung abgeschlossen ist][6]

In diesem Fall ist Ihr Azure-Zugriff auf Azure Active Directory beschränkt.

![Azure-Funktionen][7]

Möglicherweise hatten Sie aufgrund einer früheren Verwendung bereits Zugriff auf Azure. Außerdem können Sie Ihren Azure Active Directory-Zugriff durch Aktivierung weiterer Azure-Abonnements auf einen vollständigen Azure-Zugriff erweitern. In diesen Fällen verfügt das klassische Azure-Portal über mehr Funktionen.

![Azure-Funktionen][8]



## Schritt 4: Weisen Sie die Lizenzen Benutzerkonten zu

Bevor Sie den erworbenen Plan verwenden können, müssen Sie die Lizenzen manuell Benutzerkonten Ihres Unternehmens zuweisen, damit diese das reichhaltige Featureangebot der Premium Edition nutzen können. Führen Sie zur Zuweisung von Lizenzen zu Benutzern die folgenden Schritte aus, so dass diese die Features der Premium Edition von Azure Active Directory nutzen können.

**Führen Sie die folgenden Schritte aus, um Benutzern Lizenzen zuzuweisen:**

1. Melden Sie sich beim klassischen Azure-Portal als globaler Administrator des Verzeichnisses an, das Sie anpassen möchten.
2. Klicken Sie auf **Active Directory**, und wählen Sie dann das Verzeichnis aus, in dem Sie Lizenzen zuweisen möchten.
3. Klicken Sie auf die Registerkarte **Lizenzen**, wählen Sie **Active Directory Premium** oder **Enterprise Mobility Suite** aus, und klicken Sie dann auf **Zuweisen**.

    ![Lizenzpläne][10]

4. Wählen Sie im Dialogfeld die Benutzer aus, denen Sie Lizenzen zuweisen möchten, und klicken Sie dann auf das Häkchen, um die Änderungen zu speichern.

    ![Zuweisen von Lizenzen][11]

### Lizenzeinschränkungen

Es gibt Lizenzpläne, die in anderen Lizenzplänen enthalten sind oder andere Lizenzpläne enthalten. Normalerweise kann einem Benutzer kein Lizenzplan zugewiesen werden, der ihm bereits zugewiesen wurde. Wenn Sie einem Benutzer einen Lizenzplan zuweisen möchten, der andere Lizenzpläne enthält, die dem Benutzer bereits zugewiesen sind, müssen Sie die bereits zugewiesenen Lizenzpläne zunächst entfernen.

### Lizenzanforderungen

Wenn Sie einem Benutzer eine Lizenz zuweisen, können Sie in den Eigenschaften des Benutzerkontos einen primären Verwendungsort angeben. Wenn kein solcher Verwendungsort angegeben ist, wird dem Benutzer automatisch der Standort des Mandanten zugewiesen.

![Benutzerstandort][12]

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

<!---HONumber=AcomDC_0810_2016-->