<properties
	pageTitle="Verwalten von DevTest Labs-Formeln zum Erstellen virtueller Computer | Microsoft Azure"
	description="Erfahren Sie, wie Sie DevTest Labs-Formeln erstellen, aktualisieren, entfernen und zum Erstellen neuer virtueller Computer verwenden."
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

# Verwalten von DevTest Labs-Formeln zum Erstellen virtueller Computer

## Übersicht

Formeln – ähnlich wie [benutzerdefinierte Images](./devtest-lab-create-template.md) und [Marketplace-Images](./devtest-lab-configure-marketplace-images.md) – bieten einen Mechanismus zur schnellen Bereitstellung virtueller Computer. Eine Formel in DevTest Labs ist eine Liste standardmäßiger Eigenschaftswerte, die zum Erstellen eines virtuellen Labcomputers verwendet werden. Beim Erstellen eines virtuellen Computers aus einer Formel können die Standardwerte wie vorhanden verwendet oder geändert werden.

In diesem Artikel erfahren Sie, wie Sie folgende Aufgaben ausführen:

- [Erstellen einer neuen Formel](#create-a-new-formula)
- [Verwenden einer Formel zum Erstellen eines neuen virtuellen Computers](#use-a-formula-to-create-a-new-vm)
- [Ändern einer Formel](#modify-a-formula)
- [Löschen einer Formel](#delete-a-formula)

> [AZURE.NOTE] Formeln ähneln [benutzerdefinierten Images](./devtest-lab-create-template.md), da beide Methoden zum Erstellen eines Basisimages aus einer virtuellen Festplatte dienen, die zum Bereitstellen von virtuellen Computern verwendet wird. Um leichter entscheiden zu können, welche Methode sich für Ihre Umgebung besser eignet, lesen Sie die Informationen unter [Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs](./devtest-lab-comparing-vm-base-image-types.md).

## Erstellen einer neuen Formel
Jeder Benutzer mit der Berechtigung *Benutzer* in DevTest Labs kann in einem Lab virtuelle Computer auf Basis einer Formel erstellen. Es gibt zwei Möglichkeiten, um Formeln zu erstellen:

- Von Grund auf: Entscheiden Sie sich für diese Option, wenn Sie alle Merkmale der Formel von Grund auf neu definieren möchten.
- Aus einem vorhandenen virtuellen Labcomputer: Entscheiden Sie sich für diese Option, wenn Sie eine Formel basierend auf den Einstellungen eines vorhandenen virtuellen Computers erstellen möchten.

### Erstellen einer neuen Formel von Grund auf
Die folgenden Schritte führen Sie durch den Prozess der Erstellung einer neuen Formel von Grund auf.

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Das Blatt **Einstellungen** des ausgewählten Labs wird angezeigt.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab **Formeln**.

    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)

1. Wählen Sie auf dem Blatt **Labformeln** **+ Hinzufügen**.

    ![Neue Formel hinzufügen](./media/devtest-lab-manage-formulas/add-formula.png)

1. Wählen Sie auf dem Blatt **Basis auswählen** das benutzerdefinierte Image oder das Marketplace-Image, aus dem Sie die Formel erstellen möchten.

    ![Basisliste](./media/devtest-lab-manage-formulas/base-list.png)

1. Geben Sie auf dem Blatt **Formel erstellen** folgende Werte an:

	- **Formelname**: Geben Sie einen Namen für die Formel ein. Dieser Wert wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen. Der Name wird während der Eingabe überprüft. Falls er nicht gültig ist, werden Sie in einer Meldung über die Anforderungen für einen gültigen Namen informiert.
	- **Beschreibung**: Geben Sie eine aussagekräftige Beschreibung für Ihre Formel ein. Dieser Wert steht über das Kontextmenü der Formel zur Verfügung, wenn Sie einen virtuellen Computer erstellen.
	- **Image**: Dieses Feld zeigt den Namen des Basisimage an, das Sie auf dem vorherigen Blatt ausgewählt haben.
	- **VM-Größe**: Wählen Sie eines der vordefinierten Elemente aus, mit denen die Prozessorkerne, die RAM-Größe und die Größe der Festplatte des zu erstellenden virtuellen Computers angegeben werden.
	- **Virtuelles Netzwerk**: Wählen Sie diese Option und das gewünschte virtuelle Netzwerk aus.
	- **Subnetz**: Wählen Sie diese Option und das gewünschte Subnetz aus.
	- **Öffentliche IP-Adresse**: Wenn die Labrichtlinie öffentliche IP-Adressen für das ausgewählte Subnetz zulässt, geben Sie durch Auswahl von **Ja** oder **Nein** an, ob die IP-Adresse öffentlich sein soll. Andernfalls ist diese Option deaktiviert und als **Nein** festgelegt.
	- **Artefakte**: Wählen Sie diese Option und – aus der Liste der Artefakte – die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie. Beachten Sie, dass Artefaktparameter, bei denen es sich um sichere Zeichenfolgen handelt, nicht angezeigt werden, da die Formel keine sicheren Zeichenfolgenwerte speichert.

    	![Formel erstellen](./media/devtest-lab-manage-formulas/create-formula.png)

1. Wählen Sie **Erstellen**, um die Formel zu erstellen. Wenn die Formel erfolgreich erstellt wurde, wird sie auf dem Blatt **Labformeln** aufgeführt.

	![Neu erstellte Formel](./media/devtest-lab-manage-formulas/newly-created-formula.png)

### Erstellen einer neuen Formel aus einem virtuellen Labcomputer
Die folgenden Schritte führen Sie durch den Prozess der Erstellung einer neuen Formel aus einem vorhandenen virtuellen Labcomputer.

> [AZURE.NOTE] Nur virtuelle Computer, die nach dem 30. März 2016 erstellt wurden, unterstützen die Erstellung einer neuen Formel aus einem virtuellen Labcomputer.

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Suchen Sie auf dem Blatt des Labs nach dem Abschnitt mit dem Titel **Meine VMs**, und wählen Sie den virtuellen Computer, aus dem Sie die neue Formel erstellen möchten.

	![Virtuelle Labs-Computer](./media/devtest-lab-manage-formulas/my-vms.png)

1. Wählen Sie auf dem Blatt **Einstellungen** des virtuellen Computers **Formel erstellen**.

	![Formel erstellen](./media/devtest-lab-manage-formulas/create-formula-menu.png)

1. Geben Sie auf dem Blatt **Formel erstellen** einen **Namen** und eine **Beschreibung** für Ihre neue Formel ein, und wählen Sie **OK**. Wenn die Formel erfolgreich erstellt wurde, wird sie auf dem Blatt **Labformeln** aufgeführt.

	![Blatt „Formel erstellen“](./media/devtest-lab-manage-formulas/create-formula-blade.png)

## Ändern einer Formel
Um eine Formel zu ändern, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab **Formeln**.

    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)

1. Wählen Sie auf dem Blatt **Labformeln** die Formel, die Sie ändern möchten.

1. Nehmen Sie auf dem Blatt **Formel aktualisieren** die gewünschten Änderungen vor, und wählen Sie **Aktualisieren**.

## Löschen einer Formel 
Um eine Formel zu löschen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab **Formeln**.

    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)

1. Wählen Sie auf dem Blatt **Labformeln** die Auslassungspunkte rechts neben der Formel, die Sie löschen möchten.

    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)

1. Wählen Sie im Kontextmenü der Formel die Option **Löschen** aus.

    ![Kontextmenü „Formel“](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)

1. Wählen Sie im Bestätigungsdialogfeld **Ja**.

    ![Kontextmenü „Formel“](./media/devtest-lab-manage-formulas/formula-delete-confirmation.png)

## Verwandte Blogbeiträge

- [Custom images or formulas?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/) (Benutzerdefinierte Images oder Formeln?)

## Nächste Schritte
Nachdem Sie eine Formel erstellt haben, die zum Erstellen virtueller Computer verwendet werden soll, besteht der nächste Schritt darin, [Ihrem Lab einen virtuellen Computer hinzuzufügen](./devtest-lab-add-vm-with-artifacts.md).

<!---HONumber=AcomDC_0803_2016-->