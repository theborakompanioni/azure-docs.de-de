<properties
	pageTitle="Verwalten benutzerdefinierter Azure DevTest Labs-Images zum Erstellen virtueller Computer | Microsoft Azure"
	description="Erfahren Sie, wie ein benutzerdefiniertes Image aus einer VHD-Datei oder einem vorhandenen virtuellen Computer in Azure DevTest Labs erstellen."
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
	ms.date="09/07/2016"
	ms.author="tarcher"/>

# Verwalten benutzerdefinierter Azure DevTest Labs-Images zum Erstellen virtueller Computer

In Azure DevTest Labs können Sie benutzerdefinierte Images verwenden, um virtuelle Computer (VMs, Virtual Machines) besonders schnell zu erstellen. Bei diesem Verfahren brauchen Sie nicht auf die Installation der gesamten auf dem Zielcomputer erforderlichen Software zu warten. Mithilfe benutzerdefinierter Images können Sie die gesamte benötigte Software in einer VHD-Datei vorinstallieren und dann mithilfe dieser VHD-Datei einen virtuellen Computer erstellen. Weil die Software bereits installiert ist, geht das Erstellen des virtuellen Computers viel schneller. Außerdem werden benutzerdefinierte Images zum Klonen von virtuellen Computern verwendet. Dabei wird von einem virtuellen Computer ein benutzerdefiniertes Image erstellt, und auf Basis dieses Images werden weitere virtuelle Computer erstellt.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

- [Erstellen eines benutzerdefinierten Images aus einer VHD-Datei](#create-a-custom-image-from-a-vhd-file), damit Sie anschließend aus diesem benutzerdefinierten Image einen virtuellen Computer erstellen können.
- [Erstellen eines benutzerdefinierten Images von einem virtuellen Computer](#create-a-custom-image-from-a-vm), um ein schnelles Klonen von virtuellen Computern zu ermöglichen.

## Erstellen eines benutzerdefinierten Images aus einer VHD-Datei

In diesem Abschnitt erfahren Sie, wie Sie ein benutzerdefiniertes Image aus einer VHD-Datei erstellen. Sie benötigen Zugriff auf eine gültige VHD-Datei, um alle in diesem Abschnitt beschriebenen Schritte auszuführen.


1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt des Labs **Konfiguration** aus.

1. Wählen Sie auf dem Blatt **Konfiguration** für das Lab die Option **Benutzerdefinierte Images** aus.

1. Wählen Sie auf dem Blatt **Benutzerdefinierte Images** **+ Benutzerdefiniertes Image**.

    ![Hinzufügen des benutzerdefinierten Images](./media/devtest-lab-create-template/add-custom-image.png)

1. Geben Sie den Namen des benutzerdefinierten Images ein. Dieser Name wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Geben Sie die Beschreibung des benutzerdefinierten Images ein. Diese Beschreibung wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Wählen Sie **VHD-Datei**.

1. Wenn Sie Zugriff auf eine VHD-Datei haben, die nicht aufgelistet ist, fügen Sie sie gemäß der Anweisungen im Abschnitt [Hochladen einer VHD-Datei](#upload-a-vhd-file) hinzu, und kehren Sie nach Abschluss zu diesem Punkt zurück.

1. Wählen Sie die gewünschte VHD-Datei aus.

1. Wählen Sie **OK**, um das Blatt **VHD-Datei** zu schließen.

1. Wählen Sie **Betriebssystemkonfiguration**.

1. Wählen Sie auf der Registerkarte **Betriebssystemkonfiguration** eine der Optionen **Windows** oder **Linux** aus.

1. Wenn Sie **Windows** ausgewählt haben, geben Sie über das Kontrollkästchen an, ob *Sysprep* auf dem Computer ausgeführt wurde.

1. Wählen Sie **OK**, um das Blatt **Betriebssystemkonfiguration** zu schließen.

1. Wählen Sie **OK**, um das benutzerdefinierte Image zu erstellen.

1. Gehen Sie zum Abschnitt [Nächste Schritte](#next-steps).

###Hochladen einer VHD-Datei

Um ein benutzerdefiniertes Image hinzufügen zu können, benötigen Sie Zugriff auf eine VHD-Datei.

1. Wählen Sie auf dem Blatt **VHD-Datei** die Option **Hochladen einer VHD-Datei mit PowerShell**.

    ![Image hochladen](./media/devtest-lab-create-template/upload-image-using-psh.png)

1. Auf dem nächsten Blatt wird eine Anleitung zum Ändern und Ausführen eines PowerShell-Skripts angezeigt, das eine VHD-Datei in Ihr Azure-Abonnement hochlädt. **Hinweis**: Dieser Vorgang kann abhängig von der Größe der VHD-Datei und der Geschwindigkeit Ihrer Verbindung lange dauern.

## Erstellen eines benutzerdefinierten Images von einem virtuellen Computer
Wenn Sie über einen bereits konfigurierten virtuellen Computer verfügen, können Sie von diesem ein benutzerdefiniertes Image erstellen und das Image anschließend zum Erstellen weiterer, identischer virtueller Computer verwenden. Die folgenden Schritte beschreiben, wie Sie ein benutzerdefiniertes Image von einem virtuellen Computer erstellen:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt des Labs die Option **My virtual machines** (Meine virtuellen Computer) aus.
 
1. Wählen Sie auf dem Blatt **My virtual machines** (Meine virtuellen Computer) den virtuellen Computer aus, von dem Sie das benutzerdefinierte Image erstellen möchten.

1. Wählen Sie auf dem Blatt des virtuellen Computers **Create custom image (VHD)** (Benutzerdefiniertes Image erstellen (VHD)).

	![Menüelement: Benutzerdefiniertes Bild erstellen](./media/devtest-lab-create-template/create-custom-image.png)

1. Geben Sie auf dem Blatt **Create image** (Image erstellen) einen Namen und eine Beschreibung für Ihr benutzerdefiniertes Image ein. Diese Informationen werden in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

	![Blatt: Benutzerdefiniertes Bild erstellen](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Wählen Sie aus, ob „sysprep“ auf dem virtuellen Computer ausgeführt wurde. Wenn „sysprep“ auf dem virtuellen Computer nicht ausgeführt wurde, geben Sie an, ob „sysprep“ ausgeführt werden soll, wenn Sie einen virtuellen Computer anhand dieses benutzerdefinierten Images erstellen.

1. Wählen Sie **OK**, wenn Sie mit der Erstellung des benutzerdefinierten Images fertig sind.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##Nächste Schritte

Nachdem Sie ein benutzerdefiniertes Image zum Erstellen eines virtuellen Computers hinzugefügt haben, besteht der nächste Schritt darin, [Ihrem Lab einen virtuellen Computer hinzuzufügen](./devtest-lab-add-vm-with-artifacts.md).

<!---HONumber=AcomDC_0907_2016-->