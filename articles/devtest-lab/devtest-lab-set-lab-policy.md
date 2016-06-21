<properties
	pageTitle="Definieren von Labrichtlinien | Microsoft Azure"
	description="Erfahren Sie, wie Sie Labrichtlinien wie VM-Größen, die maximale Anzahl von VMs pro Benutzer und das automatisierte Herunterfahren definieren."
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

# Definieren von Labrichtlinien.

## Übersicht

Mit DevTest Labs können Sie Richtlinien für Schlüssel angeben, die die Verwendung Ihres Labs und seiner virtuellen Computer steuern. Beispielsweise können Sie die Regeln für die VM-Größen festlegen, die zum Erstellen virtueller Computer zulässig sind, sowie den Schwellenwert für die Anzahl von virtuellen Computern, die erstellt werden können. Darüber hinaus können Sie Aufträge so planen, dass sie die Lab-VMs automatisch starten und stoppen.

## Zugreifen auf die Richtlinien eines Labs

Um die Richtlinien für ein Lab anzeigen und ändern zu können, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie **Settings** aus.

	![Einstellungen](./media/devtest-lab-set-lab-policy/lab-blade-settings.png)

1. Auf dem Blatt **Einstellungen** wird eine Gruppierung von Einstellungen mit dem Namen **VM Policies** angezeigt.

	![Einstellungen](./media/devtest-lab-set-lab-policy/policies.png)

	Wählen Sie in der Liste unten die gewünschte Richtlinie, um weitere Informationen zum Festlegen zu erhalten:

	- [Allowed VM Sizes](#set-allowed-vm-sizes): Wählen Sie die Liste der im Lab zulässigen VM-Größen aus. Ein Benutzer kann nur aus dieser Liste virtuelle Computer erstellen.

	- [Maximum VMs per user](#set-maximum-vms-per-user) (maximale Anzahl von VMs pro Benutzer): Geben Sie die maximale Anzahl von VMs an, die ein Benutzer erstellen kann.

	- [Total VMs allowed](#set-total-vms-allowed) (erlaubtes Maximum von VMs): Geben Sie die maximale Anzahl von VMs an, die in einem Lab erstellt werden kann.

	- [Auto shutdown](#set-auto-shutdown) (automatisches Herunterfahren): Geben Sie den Zeitpunkt an, zu dem die VMs des aktuellen Labs automatisch herunterfahren.

	- [Auto start](#set-auto-start) (automatisches Starten): Geben Sie den Zeitpunkt an, zu dem die virtuellen Computer des aktuellen Labs automatisch starten.

## Festlegen von zulässigen VM-Größen

Die Richtlinie für die zulässigen VM-Größen hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die im Lab zulässigen VM-Größen anzugeben. Wenn diese Richtlinie aktiviert ist, können nur VM-Größen aus dieser Liste zum Erstellen von virtuellen Computern verwendet werden.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab unter **VM-Richtlinien** **Zulässige VM-Größen**.

	![Einstellungen](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)
 
1. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

1. Wenn Sie diese Richtlinie aktiviert haben, wählen Sie mindestens eine VM-Größe, die im Lab erstellt werden kann.

1. Wählen Sie **Speichern** aus.

## Festlegen der maximalen Anzahl von VMs pro Benutzer

Mit der Richtlinie für **Maximum VMs per user** können Sie die maximale Anzahl von virtuellen Computern angeben, die von einem einzelnen Benutzer erstellt werden können. Wenn ein Benutzer versucht, einen neuen virtuellen Computer zu erstellen, und die Benutzergrenze erreicht wurde, wird in einer Fehlermeldung darauf hingewiesen, dass der virtuelle Computer nicht erstellt werden kann.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab unter **VM-Richtlinien** **Maximale VM-Anzahl pro Benutzer**.

	![Einstellungen](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

1. Wenn Sie diese Richtlinie aktivieren, geben Sie im Textfeld **Maximum VMs allowed per User** einen numerischen Wert für die maximale Anzahl von virtuellen Computern ein, die von einem Benutzer erstellt werden können. Wenn Sie eine ungültige Zahl eingeben, zeigt die Benutzeroberfläche die maximal zulässige Zahl für dieses Feld an.

1. Wählen Sie **Speichern** aus.

## Festlegen der zulässigen Gesamtanzahl von virtuellen Computern

Mit der Richtlinie für **Total VMs allowed** können Sie die maximale Anzahl von virtuellen Computern angeben, die für das aktuelle Lab erstellt werden können. Wenn ein Benutzer versucht, einen neuen virtuellen Computer zu erstellen, und die Labgrenze erreicht wurde, wird in einer Fehlermeldung darauf hingewiesen, dass der virtuelle Computer nicht erstellt werden kann.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab unter **VM-Richtlinien** **Zulässige VM-Gesamtzahl**.

	![Einstellungen](./media/devtest-lab-set-lab-policy/total-vms-allowed.png)

1. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

1. Wenn Sie diese Richtlinie aktivieren, geben Sie im Textfeld **Total VMs allowed in this lab** einen numerischen Wert für die maximale Anzahl von virtuellen Computern ein, die für das aktuelle Lab erstellt werden können. Wenn Sie eine ungültige Zahl eingeben, zeigt die Benutzeroberfläche die maximal zulässige Zahl für dieses Feld an.

1. Wählen Sie **Speichern** aus.

## Festlegen des Auto-Shutdowns

Die Auto-Shutdown-Richtlinie hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die Uhrzeit anzugeben, zu der die virtuellen Computer für dieses Lab heruntergefahren werden.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab unter **VM-Richtlinien** **Automatisches Herunterfahren**.

	![Einstellungen](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

1. Wenn Sie diese Richtlinie aktivieren, geben Sie die lokale Uhrzeit an, zu der alle virtuellen Computer im derzeitigen Lab heruntergefahren werden sollen.

1. Wählen Sie **Speichern** aus.

1. Standardmäßig gilt diese Richtlinie nach der Aktivierung für alle virtuellen Computer im aktuellen Lab. Um diese Einstellung von einem bestimmten virtuellen Computer zu entfernen, öffnen Sie das Blatt des virtuellen Computers, und ändern Sie die Einstellung **Auto shutdown**.

## Festlegen des automatischen Starts

Mit der Autostart-Richtlinie können Sie angeben, wann die virtuellen Computer im aktuellen Lab gestartet werden sollen.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab unter **VM-Richtlinien** **Automatisches Starten**.

	![Einstellungen](./media/devtest-lab-set-lab-policy/auto-start.png)

1. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

1. Wenn Sie diese Richtlinie aktivieren, geben Sie die geplante lokale Startzeit und die zugeordneten Tage der Woche an.

1. Wählen Sie **Speichern** aus.

1. Nach der Aktivierung wird diese Richtlinie nicht automatisch auf alle virtuellen Computer im aktuellen Lab angewendet. Um diese Einstellung auf einen bestimmten virtuellen Computer anzuwenden, öffnen Sie das Blatt des virtuellen Computers, und ändern Sie die Einstellung **Auto start**.

## Nächste Schritte

Sobald Sie die verschiedenen VM-Richtlinieneinstellungen für Ihr Lab definiert und angewendet haben, können Sie als Nächstes versuchen, die folgenden Aufgaben auszuführen:

- [Konfigurieren des Kostenmanagements](./devtest-lab-configure-cost-management.md): Dieser Artikel veranschaulicht, wie Sie das Diagramm **Monatlicher geschätzter Kostentrend** verwenden, um die geschätzten bisherigen Kosten für den aktuellen Monat sowie die veranschlagten Kosten am Monatsende anzuzeigen.
- [Erstellen eines benutzerdefinierten Images](./devtest-lab-create-template.md): Wenn Sie einen virtuellen Computer erstellen, geben Sie eine Basis an, wobei es sich entweder um ein benutzerdefiniertes Image oder ein Marketplace-Image handeln kann. In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Image aus einer VHD-Datei erstellen.
- [Konfigurieren von Marketplace-Images](./devtest-lab-configure-marketplace-images.md): DevTest Labs unterstützt die Erstellung neuer virtueller Computer auf der Basis von Azure Marketplace-Images. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen neuer virtueller Computer in einem Lab verwendet werden können.
- [Erstellen eines virtuellen Computers in einem Lab](./devtest-lab-add-vm-with-artifacts.md): In diesem Artikel wird veranschaulicht, wie Sie einen neuen virtuellen Computer aus einem Basisimage erstellen (entweder ein benutzerdefiniertes Image oder ein Marketplace-Image) und wie Sie mit Artefakten im virtuellen Computer arbeiten.

<!---HONumber=AcomDC_0608_2016-->