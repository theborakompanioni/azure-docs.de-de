<properties
	pageTitle="Hinzufügen einer VM mit Artefakten zu einem Lab | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine VM mit Artefakten in DevTest Labs hinzufügen."
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
	ms.date="08/01/2016"
	ms.author="tarcher"/>

# Hinzufügen einer VM mit Artefakten zu einem Lab

> [AZURE.VIDEO how-to-create-vms-with-artifacts-in-a-devtest-lab]

## Übersicht

Sie erstellen einen virtuellen Computer in einem Lab aus einer *Basis*, bei der es sich entweder um ein [benutzerdefiniertes Image](./devtest-lab-create-template.md), eine [Formel](./devtest-lab-manage-formulas.md) oder ein [Marketplace-Image](./devtest-lab-configure-marketplace-images.md) handelt.

Mit DevTest Labs-*Artefakten* können Sie *Aktionen* angeben, die ausgeführt werden, wenn der virtuelle Computer erstellt wird.

Artefaktaktionen können Prozeduren ausführen, z.B. die Ausführung von Windows PowerShell-Skripts und Bash-Befehlen und eine Softwareinstallation.

Mit *Parametern* für Artefakte können Sie das Artefakt für ein bestimmtes Szenario anpassen.

In diesem Artikel wird die Erstellung eines virtuellen Computers in Ihrem Lab mithilfe von Artefakten veranschaulicht.

## Hinzufügen einer VM mit Artefakten

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer erstellen möchten.

1. Wählen Sie auf dem Blatt des Labs **+ Lab-VM**, wie in der folgenden Abbildung dargestellt. ![Schaltfläche „Lab-VM hinzufügen“](./media/devtest-lab-add-vm-with-artifacts/devtestlab-home-blade-add-vm.png)

1. Wählen Sie auf dem Blatt **Basisdesign auswählen** eine Basis für den virtuellen Computer aus.

1. Geben Sie auf dem Blatt **Lab-VM** einen Namen für den neuen virtuellen Computer in das Textfeld **Name der Lab-VM** ein.

	![Blatt „Lab-VM“](./media/devtest-lab-add-vm-with-artifacts/devtestlab-lab-vm-blade.png)

1. Geben Sie einen **Benutzernamen** ein, dem Administratorrechte auf dem virtuellen Computer erteilt werden.

1. Wenn das Betriebssystem für die ausgewählte Basis Linux ist, geben Sie entweder *Kennwort* oder *Öffentlicher SSH-Schlüssel* als Authentifizierungstyp an.

1. Geben Sie je nach angegebenem Authentifizierungstyp ein Kennwort oder einen öffentlichen SSH-Schlüssel ein.

1. Wählen Sie **VM-Größe**, und wählen Sie eines der vordefinierten Elemente aus, mit denen die Prozessorkerne, die RAM-Größe und die Größe der Festplatte des zu erstellenden virtuellen Computers angegeben werden.

1. Wählen Sie **Virtuelles Netzwerk**, und wählen Sie das gewünschte virtuelle Netzwerk aus.

1. Wählen Sie **Subnetz**, und wählen Sie das Subnetz aus.

1. Wenn die Labrichtlinie öffentliche IP-Adressen für das ausgewählte Subnetz zulässt, geben Sie durch Auswahl von **Ja** oder **Nein** an, ob die IP-Adresse öffentlich sein soll. Andernfalls ist diese Option deaktiviert und als **Nein** festgelegt.

1. Wählen Sie **Artefakte**, und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie. **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, fahren Sie mit dem Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](#add-an-existing-artifact-to-a-vm) fort, und kehren Sie dann hierher zurück.

1. Wenn Sie die ARM-Vorlage anzeigen oder kopieren möchten, fahren Sie mit dem Abschnitt [Speichern der ARM-Vorlage](#save-arm-template) fort, und kehren Sie anschließend hierher zurück.

1. Wählen Sie **Erstellen**, um den angegebene virtuellen Computer dem Lab hinzuzufügen.

1. Auf dem Blatt für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Erstellung**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.

1. Gehen Sie zum Abschnitt [Nächste Schritte](#next-steps).

## Hinzufügen eines vorhandenen Artefakts zu einer VM

Beim Erstellen eines virtuellen Computers können Sie vorhandene Artefakte hinzufügen. Jedes Lab enthält Artefakte aus dem öffentlichen DevTest Labs-Artefaktrepository, sowie Artefakte, die Sie erstellt und Ihrem eigenen Artefaktrepository hinzugefügt haben. Informationen zum Erstellen von Artefakten finden Sie im Artikel [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Labs-VM](devtest-lab-artifact-author.md).

1. Wählen Sie auf dem Blatt **Lab-VM** die Option **Artefakte**.

1. Wählen Sie auf dem Blatt **Artefakte hinzufügen** das gewünschte Artefakt aus.

![Blatt „Artefakte hinzufügen“](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifact-blade.png)

1. Geben Sie die erforderlichen Parameterwerte und alle optionalen Parameter ein, die Sie benötigen.

1. Wählen Sie **Hinzufügen**, um das Artefakt hinzuzufügen, und kehren Sie zum Blatt **Artefakte hinzufügen** zurück.

1. Fügen Sie weiterhin nach Bedarf für Ihren virtuellen Computer Artefakte hinzu.

1. Sobald Sie Ihre Artefakte hinzugefügt haben, können Sie [die Reihenfolge ändern, in der die Artefakte ausgeführt werden](#change-the-order-in-which-artifacts-are-run). Sie können auch zum [Anzeigen oder Ändern eines Artefakts](#view-or-modify-an-artifact) zurückgehen.

## Ändern der Reihenfolge, in der Artefakte ausgeführt werden

Standardmäßig werden die Aktionen der Artefakte in der Reihenfolge ausgeführt, in der sie der VM hinzugefügt wurden. Die folgenden Schritte veranschaulichen, wie Sie die Reihenfolge ändern, in der die Artefakte ausgeführt werden.

1. Wählen Sie oben auf dem Blatt **Artefakte hinzufügen** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.

    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)

1. Positionieren Sie die Artefakte in der Liste mit Drag & Drop so, dass sie die gewünschte Reihenfolge widerspiegeln. **Hinweis:** Wenn beim Ziehen eines Artefakts Probleme auftreten, stellen Sie sicher, dass Sie von der linken Seite des Artefakts aus ziehen.

1. Wählen Sie nach Abschluss des Vorgangs **OK**.

## Anzeigen oder Ändern eines Artefakts

Die folgenden Schritte veranschaulichen das Anzeigen oder Ändern der Parameter eines Artefakts:

1. Wählen Sie oben auf dem Blatt **Artefakte hinzufügen** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.

    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)

1. Wählen Sie auf dem Blatt **Ausgewählte Artefakte** das Artefakt aus, das Sie anzeigen oder bearbeiten möchten.

1. Nehmen Sie auf dem Blatt **Artefakt hinzufügen** die erforderlichen Änderungen vor, und wählen Sie **OK**, um das Blatt **Artefakt hinzufügen** zu schließen.

1. Wählen Sie **OK**, um das Blatt **Ausgewählte Artefakte** zu schließen.

## Speichern der ARM-Vorlage

Eine ARM-Vorlage bietet eine deklarative Möglichkeit zum Definieren einer wiederholbaren Bereitstellung. Mit den folgenden Schritte wird erläutert, wie die ARM-Vorlage für die zu erstellende VM gespeichert wird. Nach dem Speichern können Sie die ARM-Vorlage zum [Bereitstellen neuer VMs mit Azure PowerShell](../resource-group-overview.md#template-deployment) nutzen.

1. Wählen Sie auf dem Blatt **Lab-VM** die Option **ARM-Vorlage anzeigen**.

1. Markieren Sie auf dem Blatt **Azure Resource Manager-Vorlage anzeigen** den gesamten Text der Vorlage.

1. Kopieren Sie den markierten Text in die Zwischenablage.

1. Wählen Sie **OK**, um das Blatt **Azure Resource Manager-Vorlage anzeigen** zu schließen.

1. Öffnen Sie einen Text-Editor.

1. Fügen Sie den Text der Vorlage aus der Zwischenablage ein.

1. Speichern Sie die Datei für eine spätere Verwendung.

## Nächste Schritte

- Nach der Erstellung des virtuellen Computers können Sie **Verbinden** auf dem Blatt des virtuellen Computers wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
- Informieren Sie sich über das [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Lab-VM](devtest-lab-artifact-author.md).
- Untersuchen Sie den [DevTest Labs-ARM-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).

<!---HONumber=AcomDC_0803_2016-->