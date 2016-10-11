<properties
	pageTitle="Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory | Microsoft Azure"
	description="Erfahren Sie, wie Sie Azure Active Directory die Domänennamen Ihres Unternehmens hinzufügen und den Domänennamen überprüfen."
	services="active-directory"
	documentationCenter=""
	authors="jeffsta"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="09/30/2016"
	ms.author="curtand;jeffsta"/>

# Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-Portal](active-directory-domains-add-qzure-portal.md)
- [Klassisches Azure-Portal](active-directory-add-domain.md)

Sie verfügen über mindestens einen Domänennamen, den Ihre Organisation zu geschäftlichen Zwecken verwendet, und Ihre Benutzer melden sich unter Verwendung Ihres geschäftlichen Domänennamens bei Ihrem Unternehmensnetzwerk an. Mit Azure Active Directory (Azure AD) können Sie den Namen Ihrer Unternehmensdomäne nun auch zu Azure AD hinzufügen. Dadurch können Sie in dem Verzeichnis Benutzernamen zuweisen, die Ihren Benutzern vertraut sind (etwa „alice@contoso.com“). Der Prozess ist ganz einfach:

1. Fügen Sie Ihrem Verzeichnis den benutzerdefinierten Domänennamen hinzu.
2. Fügen Sie bei der Domänennamen-Registrierungsstelle einen DNS-Eintrag für den Domänennamen hinzu.
3. Überprüfen Sie den benutzerdefinierten Domänennamen in Azure AD.

> [AZURE.NOTE] Wenn Sie Ihren benutzerdefinierten Domänennamen für die Verwendung mit Active Directory-Verbunddiensten (AD FS) oder einem anderen Sicherheitstokendienst (STS) im Unternehmensnetzwerk konfigurieren möchten, lesen Sie die Anleitung unter [Hinzufügen und Konfigurieren einer Domäne für den Verbund mit Azure Active Directory](active-directory-add-domain-federated.md). Dies ist hilfreich, wenn Sie Benutzer aus Ihrem Unternehmensverzeichnis mit Azure AD synchronisieren möchten und die [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) Ihren Anforderungen nicht gerecht wird.

## Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Verzeichnis

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto an, das als globaler Administrator Ihres Azure AD-Verzeichnisses fungiert.

2. Öffnen Sie in **Active Directory** Ihr Verzeichnis, und wählen Sie die Registerkarte **Domänen** aus.

3. Wählen Sie in der Befehlsleiste **Hinzufügen** aus. Geben Sie den Namen der benutzerdefinierten Domäne ein, wie z.B. „contoso.com“. Achten Sie darauf, dass die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene vorhanden ist, und lassen Sie das Kontrollkästchen für „Einmaliges Anmelden“ (Verbund) deaktiviert.

4. Wählen Sie **Hinzufügen**.

5. Rufen Sie auf der zweiten Seite des Assistenten zum Hinzufügen von Domänen den DNS-Eintrag ab, mit dem Azure AD überprüft, ob Ihre Organisation der Besitzer des benutzerdefinierten Domänennamens ist.

Nach dem Hinzufügen des Domänennamens muss Azure AD überprüfen, ob der Domänenname Ihrer Organisation gehört. Dazu muss allerdings zuerst der DNS-Zonendatei ein DNS-Eintrag für den Domänennamen hinzugefügt werden. Dieser Schritt wird auf der Website für die Registrierung von Domänennamen ausgeführt.

## Hinzufügen des DNS-Eintrags bei der Domänennamen-Registrierungsstelle für die Domäne

Der nächste Schritt der Verwendung des benutzerdefinierten Domänennamens mit Azure AD ist die Aktualisierung der DNS-Zonendatei für die Domäne. So kann Azure AD überprüfen, ob sich der benutzerdefinierte Domänenname im Besitz Ihrer Organisation befindet.

1.  Melden Sie sich an der Domänennamen-Registrierungsstelle für die Domäne an. Falls Sie nicht über die Zugriffsberechtigung zur Aktualisierung des DNS-Eintrags verfügen, können Sie eine Person oder ein Team mit entsprechendem Zugriff bitten, Schritt 2 auszuführen und Sie zu benachrichtigen, wenn der Vorgang abgeschlossen wurde.

2.  Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie den DNS-Eintrag hinzufügen, der von Azure AD bereitgestellt wird. Mit diesem DNS-Eintrag kann Azure AD prüfen, ob sich die Domäne in Ihrem Besitz befindet. Der DNS-Eintrag nimmt keine Änderungen in Bezug auf das Verhalten, wie etwa E-Mail-Routing oder Webhosting, vor.

Hilfe zum Hinzufügen des DNS-Eintrags finden Sie unter [Anleitung für das Hinzufügen eines DNS-Eintrags bei gängigen DNS-Registrierungsstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

## Überprüfen des Domänennamens mit Azure AD

Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure AD überprüfen.

Falls der Assistent **Domäne hinzufügen** noch geöffnet ist, klicken Sie auf der dritten Seite des Assistenten auf **Überprüfen**. Wenn Sie **Überprüfen** auswählen, sucht Azure AD in der DNS-Zonendatei für die Domäne nach dem DNS-Eintrag. Azure AD kann den Domänennamen erst überprüfen, nachdem die DNS-Einträge weitergegeben wurden. Häufig dauert diese Weitergabe nur wenige Sekunden, aber der Vorgang kann unter Umständen auch eine Stunde oder länger dauern. Versuchen Sie, den Vorgang später erneut durchzuführen, wenn die Überprüfung beim ersten Mal nicht funktioniert.

Ist der Assistent **Domäne hinzufügen** nicht mehr geöffnet, können Sie die Domäne im [klassischen Azure-Portal](https://manage.windowsazure.com/) überprüfen:

1.  Melden Sie sich mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2.  Öffnen Sie Ihr Verzeichnis, und wählen Sie die Registerkarte **Domänen** aus.

3.  Wählen Sie den Domänennamen aus, den Sie überprüfen möchten, und wählen Sie auf der Befehlsleiste die Option **Überprüfen** aus.

4. Wählen Sie im Dialogfeld die Option **Überprüfen** aus, um die Überprüfung abzuschließen.

Nun können Sie [Benutzernamen zuweisen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-add-domain-add-users.md).

## Problembehandlung

Versuchen Sie Folgendes, wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können. Wir beginnen mit den häufigsten Ursachen und fahren mit den weniger häufigen fort.

1.	**Warten Sie eine Stunde**. DNS-Einträge müssen weitergegeben (propagiert) werden, bevor Azure AD die Domäne überprüfen kann. Dies kann eine Stunde oder auch länger dauern.

2.	**Stellen Sie sicher, dass der DNS-Eintrag eingegeben wurde und richtig ist**. Führen Sie diesen Schritt auf der Website der Domänennamen-Registrierungsstelle für die Domäne aus. Azure AD kann den Domänennamen nicht überprüfen, wenn der DNS-Eintrag nicht in der DNS-Zonendatei enthalten ist oder wenn sich keine genaue Übereinstimmung mit dem DNS-Eintrag ergibt, der von Azure AD bereitgestellt wird. Wenn Sie keinen Zugriff für die Aktualisierung von DNS-Einträgen für die Domäne bei der Domänennamen-Registrierungsstelle haben, können Sie den DNS-Eintrag für eine Person bzw. ein Team in Ihrer Organisation mit dem entsprechenden Zugriff bereitstellen und um das Hinzufügen des DNS-Eintrags bitten.

3.	**Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann nur in einem einzelnen Verzeichnis überprüft werden. Falls ein Domänenname bereits in einem anderen Verzeichnis überprüft wurde, muss er darin zuerst gelöscht werden, bevor er im neuen Verzeichnis überprüft werden kann. Informationen zum Löschen von Domänennamen finden Sie unter [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md).


## Hinzufügen weiterer benutzerdefinierter Domänennamen

Wenn Sie in Ihrer Organisation mehrere benutzerdefinierte Domänennamen verwenden, z.B. „contoso.com“ und „contosobank.com“, können Sie maximal 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel, um die einzelnen Domänennamen hinzuzufügen.

## Nächste Schritte

-   [Zuweisen von Benutzernamen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-add-domain-add-users.md)
-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
-   [Informationen zu den Konzepten der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md)
-   [Anzeigen des Brandings Ihres Unternehmens bei der Anmeldung der Benutzer](active-directory-add-company-branding.md)
-   [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

<!---HONumber=AcomDC_1005_2016-->