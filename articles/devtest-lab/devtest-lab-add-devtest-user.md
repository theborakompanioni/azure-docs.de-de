<properties
	pageTitle="Hinzufügen von Besitzern und Benutzern in Azure DevTest Labs | Microsoft Azure"
	description="Hinzufügen von Besitzern und Benutzern in Azure DevTest Labs über das Azure-Portal oder PowerShell"
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="tarcher"/>

# Hinzufügen von Besitzern und Benutzern in Azure DevTest Labs

> [AZURE.VIDEO how-to-set-security-in-your-devtest-lab]

Der Zugriff in Azure DevTest Labs wird durch die [rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](../active-directory/role-based-access-control-what-is.md) gesteuert. Mithilfe von RBAC können Sie Aufgaben in Ihrem Team in *Rollen* verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Drei dieser RBAC-Rollen sind *Besitzer*, *DevTest Labs-Benutzer*, und *Mitwirkender*. In diesem Artikel erfahren Sie, welche Aktionen in jeder der drei wichtigsten RBAC-Rollen ausgeführt werden können. Sie erfahren, wie Sie Benutzer über das Portal oder über ein PowerShell-Skript zu einem Lab hinzufügen und Benutzer auf Abonnementebene hinzufügen.

## Aktionen, die in jeder Rolle ausgeführt werden können

Es gibt drei wichtige Rollen, die Sie einem Benutzer zuweisen können:

- Besitzer
- DevTest Labs-Benutzer
- Mitwirkender

Die folgende Tabelle zeigt die Aktionen, die von Benutzern in jeder dieser Rollen ausgeführt werden können:

| **Aktionen, die Benutzer in dieser Rolle ausführen können** | **DevTest Labs-Benutzer** | **Besitzer** | **Mitwirkender** |
|---|---|---|---|
| **Lab-Aufgaben** | | | |
| Benutzer zu einem Lab hinzufügen | Nein | Ja | Nein |
| Kosteneinstellungen aktualisieren | Nein | Ja | Ja |
| **Grundlegende Aufgaben für virtuelle Computer** | | | |
| Benutzerdefinierte Images hinzufügen und entfernen | Nein | Ja | Ja |
| Formeln hinzufügen, aktualisieren und löschen | Ja | Ja | Ja |
| Azure Marketplace-Images in eine Positivliste aufnehmen | Nein | Ja | Ja |
| **Aufgaben für virtuelle Computer** | | | |
| Virtuelle Computer erstellen | Ja | Ja | Ja |
| Virtuelle Computer starten, beenden und löschen | Nur vom Benutzer erstellte virtuelle Computer | Ja | Ja |
| VM-Richtlinien aktualisieren | Nein | Ja | Ja |
| Datenträgern zu virtuellen Computern hinzufügen bzw. davon entfernen | Nur vom Benutzer erstellte virtuelle Computer | Ja | Ja |
| **Artefakt-Aufgaben** | | | |
| Artefaktrepositorys hinzufügen und entfernen | Nein | Ja | Ja |
| Artefakte anwenden | Ja | Ja | Ja |

> [AZURE.NOTE] Wenn ein Benutzer einen virtuellen Computer erstellt, wird diesem Benutzer automatisch die **Besitzer**-Rolle des virtuellen Computers zugewiesen.

## Hinzufügen eines Besitzers oder Benutzers auf der Lab-Ebene

Besitzer und Benutzer können über das Azure-Portal auf der Lab-Ebene hinzugefügt werden. Das schließt externe Benutzer mit einem [Microsoft-Konto (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account) ein. Die folgenden Schritte führen Sie durch den Prozess des Hinzufügens eines Besitzers oder Benutzers zu einem Lab in Azure DevTest Labs:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt des Labs **Configuration** (Konfiguration) aus.

1. Wählen Sie auf dem Blatt **Konfiguration** die Option **Benutzer** aus.

1. Wählen Sie auf dem Blatt **Benutzer** **+Hinzufügen**.

	![Benutzer hinzufügen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1. Wählen Sie auf dem Blatt **Rolle auswählen** die gewünschte Rolle aus. Im Abschnitt [Aktionen, die in jeder Rolle ausgeführt werden können](#actions-that-can-be-performed-in-each-role) finden Sie die verschiedenen Aktionen, die Benutzer in den Rollen „Besitzer“, „DevTest-Benutzer“ und „Beitragender“ ausführen können.

1. Geben Sie auf dem Blatt **Hinzufügen von Benutzern** die E-Mail-Adresse oder den Namen des Benutzers ein, den Sie der Rolle hinzufügen möchten. Wenn der Benutzer nicht gefunden werden kann, erhalten Sie eine Fehlermeldung, die das Problem erklärt. Wird der Benutzer gefunden, so wird dieser Benutzer aufgeführt und ausgewählt.

1. Wählen Sie **Auswählen**.

1. Wählen Sie **OK**, um das Blatt **Zugriff hinzufügen** zu schließen.

1. Wenn Sie zum Blatt **Benutzer** zurückkehren, wurde der Benutzer hinzugefügt.

## Hinzufügen eines externen Benutzers zu einem Lab mit PowerShell

Sie können nicht nur Benutzer im Azure-Portal hinzufügen, sondern auch externe Benutzer über ein PowerShell-Skript zu Ihrem Lab hinzufügen. Ändern Sie im folgenden Beispiel einfach die Werte der Parameter unter dem Kommentar **Values to change** (Zu ändernde Werte). Sie können die Werte `subscriptionId`, `labResourceGroup` und `labName` aus dem Lab-Blatt im Azure-Portal abrufen.

> [AZURE.NOTE]
Im Beispielskript wird davon ausgegangen, dass der angegebene Benutzer dem Active Directory als Gast hinzugefügt wurde. Wenn dies nicht der Fall ist, tritt ein Fehler auf. Um einem Lab einen Benutzer hinzuzufügen, der nicht in Active Directory ist, weisen Sie dem Benutzer im Azure-Portal wie im Abschnitt [Hinzufügen eines Besitzers oder Benutzers auf der Lab-Ebene](#add-an-owner-or-user-at-the-lab-level) beschrieben eine Rolle zu.

	# Add an external user in DevTest Labs user role to a lab
	# Ensure that guest users can be added to the Azure Active directory:
	# https://azure.microsoft.com/de-DE/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

	# Values to change
	$subscriptionId = "<Enter Azure subscription ID here>"
	$labResourceGroup = "<Enter lab's resource name here>"
	$labName = "<Enter lab name here>"
	$userDisplayName = "<Enter user's display name here>"

	# Log into your Azure account
	Login-AzureRmAccount
	
	# Select the Azure subscription that contains the lab. 
	# This step is optional if you have only one subscription.
	Select-AzureRmSubscription -SubscriptionId $subscriptionId
	
	# Retrieve the user object
	$adObject = Get-AzureRmADUser -SearchString $userDisplayName
	
	# Create the role assignment. 
	$labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
	New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## Hinzufügen eines Besitzers oder Benutzers auf der Abonnementebene

Azure-Berechtigungen werden vom übergeordneten Bereich an den untergeordneten Bereich in Azure weitergegeben. Daher sind Besitzer eines Azure-Abonnements, das Labs enthält, automatisch Besitzer dieser Labs. Sie besitzen auch die virtuellen Computer und andere Ressourcen, die von den Lab-Benutzern und dem Azure DevTest Labs-Dienst erstellt werden.

Sie können einem Lab über das entsprechende Blatt im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) zusätzliche Besitzer hinzufügen. Der Verwaltungsbereich des hinzugefügten Besitzers ist jedoch weniger umfangreich als der Bereich des Abonnementbesitzers. Beispielsweise erhalten die hinzugefügten Besitzer keinen vollständigen Zugriff auf einige der Ressourcen, die vom DevTest Labs-Dienst im Abonnement erstellt werden.

Um einen Besitzer zu einem Azure-Abonnement hinzuzufügen, gehen Sie folgendermaßen vor:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Weitere Dienste** und dann **Abonnements** aus der Liste aus.

1. Wählen Sie das gewünschte Abonnement aus.

1. Wählen Sie das Symbol **Zugriff** aus.

	![Auf Benutzer zugreifen](./media/devtest-lab-add-devtest-user/access-users.png)

1. Wählen Sie auf dem Blatt **Benutzer** **Hinzufügen**.

	![Benutzer hinzufügen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1. Wählen Sie auf dem Blatt **Rolle auswählen** **Besitzer**.

1. Geben Sie auf dem Blatt **Hinzufügen von Benutzern** die E-Mail-Adresse oder den Namen des Benutzers ein, den Sie als Besitzer hinzufügen möchten. Wenn der Benutzer nicht gefunden werden kann, erhalten Sie eine Fehlermeldung, die das Problem erklärt. Wenn der Benutzer gefunden wird, wird dieser Benutzer unter dem Textfeld **Benutzer** aufgeführt.

1. Wählen Sie den gefundenen Benutzernamen aus.

1. Wählen Sie **Auswählen**.

1. Wählen Sie **OK**, um das Blatt **Zugriff hinzufügen** zu schließen.

1. Wenn Sie zum Blatt **Benutzer** zurückkehren, wurde der Benutzer als Besitzer hinzugefügt. Dieser Benutzer ist nun Besitzer aller Labs, die in diesem Abonnement erstellt werden, und sie kann daher die Aufgaben eines Besitzers ausführen.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

<!---HONumber=AcomDC_0914_2016-->