<properties
	pageTitle="Hinzufügen von Besitzern und Benutzern zu einem Lab | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie einen Benutzer, der nicht in Ihrem Abonnement enthalten ist, sicher zu Azure DevTest Labs hinzufügen."
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
	ms.date="06/01/2016"
	ms.author="tarcher"/>

# Hinzufügen von Besitzern und Benutzern zu einem Lab

> [AZURE.NOTE] Zeigen Sie das Video zu diesem Artikel an: [How to set security in DevTest Labs](/documentation/videos/how-to-set-security-in-your-devtest-lab) (Festlegen der Sicherheit in DevTest Labs).

## Übersicht
Der Zugriff auf DevTest Labs wird durch die rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) gesteuert. Suchen Sie im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) nach [rollenbasierte Zugriffssteuerung (RBAC)](https://azure.microsoft.com/search/?q=role%20based%20access%20control), um mehr zu erfahren.


Über zwei Rollen gewähren Sie Zugriff auf Ihr Lab:

- **Besitzer**: Benutzer, denen die Rolle **Besitzer** auf der Labebene zugewiesen ist, haben vollständigen Zugriff auf das Lab, einschließlich der Verwaltungs- und Überwachungsfunktionen. Die auf der Labebene zugewiesene Rolle **Besitzer** gewährt Benutzern keine Berechtigungen für den Zugriff auf Ressourcen im Abonnement außerhalb des Labbereichs. Benutzer mit der Rolle **Besitzer** auf der Azure-Abonnementebene haben automatisch Besitzerrechte für alle in diesem Abonnement erstellten Ressourcen (einschließlich Labs und virtuelle Computer).

-  **DevTest Labs-Benutzer**: Benutzer mit der Rolle **DevTest Labs-Benutzer** können virtuelle Computer im angegebenen Lab erstellen sowie alle Lab-Ressourcen (etwa virtuelle Computer, Richtlinien oder virtuelle Netzwerke) anzeigen. Benutzer können entweder *intern* (ein Mitglied von Azure Active Directory für das Abonnement) oder *extern* (ein Benutzer, der kein Mitglied von Azure AD ist, z. B. ein Mitglied einer Partnerorganisation) sein.
	-  Die Rolle **DevTest Labs-Benutzer** muss über die Kachel **Benutzer hinzufügen** des Labs zugewiesen werden.
	-  Benutzer mit der Rolle **DevTest Labs-Benutzer** können diese Vorgänge nur in dem Lab ausführen, dem sie zugewiesen sind. Beispiel: Ein **DevTest Labs-Benutzer** kann keinen virtuellen Computer mithilfe des Diensts für virtuelle Computer des Abonnements erstellen. Das Erstellen eines virtuellen Computers ist nur über das DevTest Labs-Konto zulässig.
	- *Externe* Benutzer müssen über ein Konto in einer der Microsoft-Kontodomänen verfügen (d. h. @hotmail.com, @live.com, @msn.com, @passport.com, @outlook.com oder eine Variante für ein bestimmtes Land).
 
Nach Erstellung eines virtuellen Computers wird dem Benutzer, der ihn erstellt hat, auf dem erstellten virtuellen Computer automatisch die Rolle **Besitzer** zugewiesen, sodass er sämtliche Aktionen ausführen kann, die im Lab zur Verfügung stehen.

## Hinzufügen eines Besitzers zum Lab

Aufgrund der Weitergabe von Berechtigungen in Azure aus dem übergeordneten Bereich an den untergeordneten Bereich werden Besitzer eines Azure-Abonnements mit enthaltenen Labs automatisch zum Besitzer dieser Labs sowie aller virtuellen Computer und anderen Ressourcen, die von den Benutzern des Labs und vom DevTest Lab-Dienst erstellt werden. Sie können einem Lab über das entsprechende Blatt im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) zwar weitere Besitzer hinzufügen, deren Verwaltungsbereich ist jedoch weniger umfangreich als der der Abonnementbesitzer, da ihr Zugriff auf einige Ressourcen, die im Abonnement durch den DevTest Labs-Dienst erstellt werden, eingeschränkt ist.

Um einen Besitzer zu einem Azure-Abonnement hinzuzufügen, in dem Sie Labs bereits erstellt haben oder neue Labs erstellen werden, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie im linken Navigationsbereich **Abonnements** aus.

	![Link zu Abonnements](./media/devtest-lab-add-devtest-user/subscriptions.png)
	
1. Wählen Sie das Abonnement aus, das die Labs enthalten soll.

1. Wählen Sie das Symbol **Zugriff** aus.

	![Auf Benutzer zugreifen](./media/devtest-lab-add-devtest-user/access-users.png)

1. Wählen Sie auf dem Blatt **Benutzer** die Option **Hinzufügen** aus.

	![Benutzer hinzufügen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1. Wählen Sie auf dem Blatt **Rolle auswählen** die Option **Besitzer** aus.

1. Geben Sie in das Textfeld **Benutzer** die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten. Wenn der Benutzer nicht gefunden werden kann, erhalten Sie eine Fehlermeldung, die das Problem erklärt. Wenn der Benutzer gefunden wird, wird dieser Benutzer unter dem Textfeld **Benutzer** aufgeführt.

1. Wählen Sie den gefundenen Benutzernamen aus.

1. Wählen Sie **Auswählen** aus.

1. Wählen Sie **OK** aus, um das Blatt **Zugriff hinzufügen** zu schließen.

1. Wenn Sie zum Blatt **Benutzer** zurückkehren, sehen Sie, dass der Benutzer als Besitzer hinzugefügt wurde. Diese Person ist nun Besitzer aller Labs, die in diesem Abonnement erstellt werden, und sie kann daher die Aufgaben eines Besitzers ausführen.

## Hinzufügen eines DevTest Labs-Benutzers zum Lab

Um einen DevTest Labs-Benutzer zum Lab hinzuzufügen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** aus.

1. Wählen Sie **DevTest Labs** aus.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie das Symbol **Zugriff** aus.

	![Benutzerzugriff](./media/devtest-lab-add-devtest-user/devtest-lab-home-blade.png)

1. Wählen Sie auf dem Blatt **Benutzer** die Option **Hinzufügen** aus.

	![Benutzer hinzufügen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1. Wählen Sie auf dem Blatt **Rolle auswählen** die Option **DevTest Labs-Benutzer** aus.

1. Auf dem Blatt **Benutzer hinzufügen**:

	1. Auf dem Blatt **Benutzer hinzufügen** wird eine Liste der integrierten Benutzer angezeigt. Wenn der gewünschte Benutzer bereits in der Liste enthalten ist, können Sie einfach die Zeile mit dem Benutzer wählen, um ihn auszuwählen. Links neben dem Benutzer wird ein Häkchen angezeigt, um anzugeben, dass der Benutzer ausgewählt wurde. Halten Sie zum Auswählen mehrerer Benutzer STRG gedrückt, während Sie die einzelnen Benutzer auswählen. Wenn Sie die Auswahl eines Benutzers aufheben möchten, halten Sie STRG gedrückt, und wählen Sie den Benutzer aus. Ein Zähler unten auf dem Blatt gibt die Anzahl der ausgewählten Benutzer an.

	1. Wenn der gewünschte Benutzer nicht in der Liste enthalten ist, geben Sie ein gültiges Microsoft-E-Mail-Konto in das Textfeld **Benutzer** ein. Wenn die E-Mail-Adresse gültig ist, wird der Benutzer unter dem Textfeld **Benutzer** angezeigt.

	1. Wählen Sie **Auswählen** aus, nachdem Sie die Benutzer ausgewählt haben, die Sie dem Lab hinzufügen möchten.

	1. Wählen Sie **OK** aus, um das Blatt **Zugriff hinzufügen** zu schließen.

1. Auf dem Blatt **Benutzer** werden die hinzugefügten Rollen und Benutzer angezeigt.

<!---HONumber=AcomDC_0629_2016-->