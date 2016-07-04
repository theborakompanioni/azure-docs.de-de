<properties
	pageTitle="Erstellen einer VM-Verfügbarkeitsgruppe | Microsoft Azure"
	description="Erfahren Sie, wie Sie im Azure-Portal oder über PowerShell unter Verwendung des Resource Manager-Bereitstellungsmodells eine Verfügbarkeitsgruppe für Ihre virtuellen Computer erstellen."
	keywords="Verfügbarkeitsgruppe"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>
<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/17/2016"
	ms.author="cynthn"/>


# Verfügbarkeitsgruppe erstellen 

Wenn Ihr virtueller Computer Mitglied einer Verfügbarkeitsgruppe sein soll, müssen Sie bei Verwendung des Portals zunächst die Verfügbarkeitsgruppe erstellen.

Weitere Information zum Erstellen und Verwenden von Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](virtual-machines-windows-manage-availability.md).


## Verwenden des Portals, um vor dem Erstellen der virtuellen Computer zunächst eine Verfügbarkeitsgruppe zu erstellen

1. Klicken Sie im Menü des Hubs auf **Durchsuchen**, und wählen Sie **Verfügbarkeitsgruppen** aus.

2. Klicken Sie auf dem **Blatt „Verfügbarkeitsgruppen“** auf **Hinzufügen**.

	![Screenshot der Schaltfläche „Hinzufügen“ zum Erstellen einer neuen Verfügbarkeitsgruppe.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Vervollständigen Sie auf dem Blatt **Verfügbarkeitsgruppe erstellen** die Informationen zu Ihrer Gruppe.

	![Screenshot der Informationen, die Sie eingeben müssen, um die Verfügbarkeitsgruppe zu erstellen.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

	- **Name**: Der Name sollte aus 1-80 Zeichen bestehen – Zahlen, Buchstaben, Punkte, Unterstriche und Bindestriche. Das erste Zeichen muss ein Buchstabe oder eine Zahl sein. Das letzte Zeichen muss ein Buchstabe, eine Zahl oder ein Unterstrich sein.
	- **Fehlerdomänen**: Fehlerdomänen definieren eine Gruppe virtueller Computer, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. Standardmäßig werden die virtuellen Computer auf bis zu drei Fehlerdomänen aufgeteilt. Sie können 1-3 Fehlerdomänen einrichten.
	- **Updatedomänen**: Standardmäßig werden fünf Updatedomänen zugewiesen. Sie können 1-20 Updatedomänen einrichten. Updatedomänen definieren Gruppen virtueller Computer und zugrunde liegender physischer Hardwarekomponenten, die gleichzeitig neu gestartet werden können. Ein Beispiel mit Updatedomänen: Wenn innerhalb einer Verfügbarkeitsgruppe mehr als fünf virtuelle Computer konfiguriert sind, wird der sechste virtuelle Computer in derselben Updatedomäne platziert wie der erste, der siebte virtuelle Computer in derselben Updatedomäne wie der zweite usw. Die Neustarts erfolgen zwar nicht sequenziell, aber es werden nicht mehrere Updatedomänen gleichzeitig neu gestartet.
	- **Abonnement**: Wählen Sie das zu verwendende Abonnement, wenn Sie über mehrere verfügen.
	- **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus, indem Sie auf den Pfeil klicken und eine Ressourcengruppe aus der Dropdownliste auswählen. Sie können auch eine neue Ressourcengruppe erstellen, indem Sie einen Namen eingeben. Der Name darf folgende Zeichen enthalten: Buchstaben, Zahlen, Punkte, Bindestriche, Unterstriche und öffnende oder schließende Klammern. Der Name darf nicht mit einem Punkt enden. Alle virtuellen Computer in der Verfügbarkeitsgruppe müssen in derselben Ressourcengruppe wie die Verfügbarkeitsgruppe erstellt werden.
	- **Speicherort**: Wählen Sie einen Speicherort aus der Dropdownliste aus.

4. Wenn Sie die Eingabe der Informationen abgeschlossen haben, klicken Sie auf **Erstellen**. Nachdem die Verfügbarkeitsgruppe erstellt wurde, wird sie nach Aktualisierung der Portalseite in der Liste angezeigt.

## Verwenden Sie das Portal, um gleichzeitig einen virtuellen Computer und eine Verfügbarkeitsgruppe zu erstellen.

Wenn Sie einen neuen virtuellen Computer über das Portal erstellen, können Sie auch eine neue Verfügbarkeitsgruppe für den virtuellen Computer erstellen, während Sie den ersten virtuellen Computer der Gruppe erstellen.

![Screenshot der Vorgehensweise zum Erstellen einer neuen Verfügbarkeitsgruppe während der Erstellung des virtuellen Computers.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## Hinzufügen eines neuen virtuellen Computers zu einer vorhandenen Verfügbarkeitsgruppe

Stellen Sie bei jedem weiteren virtuellen Computer, der Teil der Gruppe sein soll, sicher, dass Sie den Computer in derselben **Ressourcengruppe** erstellen und dann in Schritt 3 die vorhandene Verfügbarkeitsgruppe auswählen.

![Screenshot der Auswahl einer vorhandenen Verfügbarkeitsgruppe zur Verwendung durch Ihren virtuellen Computer.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## Erstellen einer Verfügbarkeitsgruppe mit PowerShell

In diesem Beispiel wird eine Verfügbarkeitsgruppe in der **RMResGroup**-Ressourcengruppe am Standort **USA, Westen** erstellt. Dies muss erfolgen, bevor Sie den ersten virtuellen Computer der Gruppe erstellen.

	New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
	
Weitere Informationen finden Sie unter [New-AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx).


## Problembehandlung

- Wenn Sie beim Erstellen eines virtuellen Computers feststellen, dass sich die gewünschte Verfügbarkeitsgruppe nicht in der Dropdownliste im Portal befindet, haben Sie sie möglicherweise in einer anderen Ressourcengruppe erstellt. Wenn Sie die Ressourcengruppe Ihrer Verfügbarkeitsgruppe nicht kennen, wechseln Sie zum Menü des Hubs und klicken auf „Durchsuchen“ > „Verfügbarkeitsgruppen“, um eine Liste Ihrer Verfügbarkeitsgruppen und der Ressourcengruppen anzuzeigen, zu denen sie gehören.


## Nächste Schritte

Erweitern Sie den Speicher für Ihren virtuellen Computer, indem Sie einen zusätzlichen [Datenträger](virtual-machines-windows-attach-disk-portal.md) hinzufügen.

<!---HONumber=AcomDC_0622_2016-->