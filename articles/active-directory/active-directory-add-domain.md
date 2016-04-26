<properties
	pageTitle="Hinzufügen des benutzerdefinierten Domänennamens zur Vereinfachung der Anmeldung mit Azure Active Directory | Microsoft Azure"
	description="Erfahren Sie, wie Sie Azure Active Directory die Domänennamen Ihres Unternehmens hinzufügen und den Domänennamen überprüfen."
	services="active-directory"
	documentationCenter=""
	authors="jeffsta"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/19/2016"
	ms.author="curtand;jeffsta"/>

# Hinzufügen des benutzerdefinierten Domänennamens zur Vereinfachung der Anmeldung mit Azure Active Directory

Sie können Ihre eigenen benutzerdefinierten Domänennamen verwenden, um die Anmeldung und andere Benutzervorgänge in Azure Active Directory (Azure AD) zu verbessern und zu vereinfachen. Falls Ihr Unternehmen beispielsweise im Besitz des Domänennamens „contoso.com“ ist, können sich Ihre Benutzer mit den vertrauten Benutzernamen anmelden, z.B. „joe@contoso.com“. Sie benötigen Zugriff auf ein Benutzerkonto, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

Wenn Sie das Mandantenverzeichnis in Azure Active Directory neu erhalten haben, melden sich Ihre Benutzer mit Namen wie „alice@contoso.onmicrosoft.com“ an. In diesem Beispiel ist „contoso.onmicrosoft.com“ ein anfänglicher integrierter Domänenname, den Sie verwenden können, bis Sie Ihren Domänennamen verifiziert haben. Einer der nächsten Schritte ist das Hinzufügen eines benutzerdefinierten Domänennamens, der sich im Besitz Ihres Unternehmens befindet, z.B. „contoso.com“. Hiermit können Sie Namen zuweisen, die für Ihre Benutzer vertraut sind, z.B. „alice@contoso.com“.

Hintergrundinformationen zur Verwendung von Domänennamen in Azure AD finden Sie unter [Konzepte der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md). Die meisten Administratoren führen die Aufgaben der Verwaltung von Domänennamen in Azure AD mit dem klassischen Azure-Portal durch. Sie können aber auch [PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) oder die [Graph-API (Vorschau)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations) für die Verwaltungsaufgaben nutzen, falls Sie dies bevorzugen.

## Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Verzeichnis

So fügen Sie Ihrem Verzeichnis einen benutzerdefinierten Domänennamen hinzu

1. Melden Sie sich am [klassischen Azure-Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2. Wählen Sie im linken Navigationsbereich die Option **Active Directory**.

3. Öffnen Sie Ihr Verzeichnis.

4. Wählen Sie die Registerkarte **Domänen**.

5. Wählen Sie in der Befehlsleiste die Option **Hinzufügen**.

6. Geben Sie den Namen der benutzerdefinierten Domäne ein, z.B. „contoso.com“. Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.

7. Aktivieren Sie das Kontrollkästchen, wenn Sie planen, diese Domäne für die [Verbundanmeldung](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) mit Ihrer lokalen Active Directory-Instanz zu konfigurieren.

8. Wählen Sie **Hinzufügen**.

Bevor Sie Benutzernamen zuweisen können, die Ihren benutzerdefinierten Domänennamen enthalten, muss Azure AD überprüfen, ob sich der Domänenname im Besitz Ihres Unternehmens befindet. Zum Durchführen dieser Überprüfung müssen Sie DNS-Einträge bei der Domänennamen-Registrierungsstelle für die Domäne aktualisieren.

## Abrufen der DNS-Einträge, die von Azure AD zum Überprüfen des Domänennamens verwendet werden

Wenn Sie beim Hinzufügen der Domäne die Option zum Konfigurieren der Domäne für den Verbund gewählt haben, wird eine Anleitung zum Herunterladen des Azure AD Connect-Tools angezeigt. Führen Sie das Azure AD Connect-Tool aus, um [die DNS-Einträge abzurufen, die Sie bei der Domänennamen-Registrierungsstelle hinzufügen müssen](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Falls Sie die Option für die Verbundanmeldung mit Windows Server AD nicht ausgewählt haben, werden die DNS-Einträge auf der zweiten Seite des Assistenten **Domäne hinzufügen** angezeigt.

## Hinzufügen der DNS-Einträge zur DNS-Zonendatei für die Domäne

So fügen Sie die für Azure AD erforderlichen DNS-Einträge hinzu

1.  Melden Sie sich an der Domänennamen-Registrierungsstelle für die Domäne an. Wenn Ihre Berechtigungen zum Aktualisieren der DNS-Zonendatei für die Domäne nicht ausreichen, können Sie die DNS-Einträge an eine Person mit der entsprechenden Zugriffsberechtigung in Ihrem Unternehmen weitergeben und sie um die Durchführung der Aktualisierung bitten.

2.  Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie die DNS-Einträge hinzufügen, die von Azure AD bereitgestellt werden. Mit diesen DNS-Einträgen kann Azure AD bestätigen, dass sich die Domäne in Ihrem Besitz befindet. Dies hat keine Änderungen in Bezug auf das Verhalten zur Folge, z.B. E-Mail-Routing oder Webhosting.

[Anleitung für das Hinzufügen von DNS-Einträgen bei gängigen DNS-Registrierungsstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Überprüfen der Domäne mit Azure AD

Nachdem Sie die DNS-Einträge hinzugefügt haben, können Sie die Domäne mit Azure AD überprüfen.

Wenn Sie die Verbundoption für Ihren benutzerdefinierten Domänennamen ausgewählt haben, wird die Überprüfung mit dem Azure AD Connect-Tool automatisch durchgeführt. Führen Sie das Tool aus, nachdem Sie die Voraussetzungen erfüllt haben. Überprüfen Sie die Domäne andernfalls im klassischen Azure-Portal. Wenn der Assistent **Domäne hinzufügen** noch geöffnet ist, können Sie auf der dritten Seite des Assistenten auf die Schaltfläche „Überprüfen“ klicken. Es dauert unter Umständen einige Minuten, bis die DNS-Einträge verteilt wurden.

Wenn der Assistent **Domäne hinzufügen** noch geöffnet ist, können Sie die Domäne im [klassischen Azure-Portal](https://manage.windowsazure.com/) überprüfen:

1.  Melden Sie sich mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2.  Wählen Sie im linken Navigationsbereich die Option **Active Directory**.

3.  Öffnen Sie Ihr Verzeichnis.

4.  Wählen Sie die Registerkarte **Domänen**.

5.  Wählen Sie in der Liste mit den Domänen die Domäne aus, die Sie überprüfen möchten.

6.  Wählen Sie in der Befehlsleiste die Option **Überprüfen**.

7.  Wählen Sie im Dialogfeld die Option **Überprüfen**.

Nun können Sie Benutzernamen zuweisen, die Ihren benutzerdefinierten Domänennamen enthalten.

## Hinzufügen weiterer benutzerdefinierter Domänennamen

Wenn in Ihrem Unternehmen mehr als ein benutzerdefinierter Domänenname verwendet wird, z.B. „contoso.com“ und „contosobank.com“, können Sie diese dem Azure AD-Verzeichnis hinzufügen (maximal 900 Domänen). Verwenden Sie jeweils die oben angegebenen Schritte, um weitere Domänennamen hinzuzufügen.

Nächste Schritte

-   [Zuweisen von Benutzernamen mit einem benutzerdefinierten Domänennamen](active-directory-add-domain-add-users.md)

-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)

-   [Anzeigen des Brandings Ihres Unternehmens bei der Anmeldung der Benutzer](active-directory-add-company-branding.md)

-   [Konzepte der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md)

<!---HONumber=AcomDC_0420_2016-->