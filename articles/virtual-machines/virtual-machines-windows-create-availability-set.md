---
title: "Erstellen einer VM-Verfügbarkeitsgruppe in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie über Azure PowerShell oder im Portal unter Verwendung des Resource Manager-Bereitstellungsmodells eine verwaltete oder nicht verwaltete Verfügbarkeitsgruppe für Ihre virtuellen Computer erstellen."
keywords: "Verfügbarkeitsgruppe"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 2f4ea765df71ed9aac7e8657e6be3706b635407e
ms.lasthandoff: 03/06/2017

---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a>Erhöhen der VM-Verfügbarkeit durch Erstellen einer Azure-Verfügbarkeitsgruppe 
Verfügbarkeitsgruppen bieten Redundanz für Ihre Anwendung. Wir empfehlen die Gruppierung von mindestens zwei virtuellen Computern in einer Verfügbarkeitsgruppe. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die von der Azure-SLA zugesicherte Verfügbarkeit von 99,95 % eingehalten wird. Weitere Informationen finden Sie unter [SLA für Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Der virtuelle Computer muss in der gleichen Ressourcengruppe wie die Verfügbarkeitsgruppe erstellt werden.
> 

Wenn Ihr virtueller Computer zu einer Verfügbarkeitsgruppe gehören soll, müssen Sie die Verfügbarkeitsgruppe vorab oder während der Erstellung des ersten virtuellen Computers der Gruppe erstellen. Wenn Ihr virtueller Computer verwaltete Datenträger verwenden soll, muss die Verfügbarkeitsgruppe als verwaltete Verfügbarkeitsgruppe erstellt werden.

Weitere Information zum Erstellen und Verwenden von Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Verwenden des Portals, um vor dem Erstellen der virtuellen Computer zunächst eine Verfügbarkeitsgruppe zu erstellen
1. Klicken Sie im Menü des Hubs auf **Durchsuchen**, und wählen Sie **Verfügbarkeitsgruppen** aus.
2. Klicken Sie auf dem Blatt **Verfügbarkeitsgruppen** auf **Hinzufügen**.
   
    ![Screenshot der Schaltfläche „Hinzufügen“ zum Erstellen einer neuen Verfügbarkeitsgruppe.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)
3. Vervollständigen Sie auf dem Blatt **Verfügbarkeitsgruppe erstellen** die Informationen zu Ihrer Gruppe.
   
    ![Screenshot der Informationen, die Sie eingeben müssen, um die Verfügbarkeitsgruppe zu erstellen.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)
   
   * **Name** : Der Name sollte aus 1-80 Zeichen bestehen – Zahlen, Buchstaben, Punkte, Unterstriche und Bindestriche. Das erste Zeichen muss ein Buchstabe oder eine Zahl sein. Das letzte Zeichen muss ein Buchstabe, eine Zahl oder ein Unterstrich sein.
   * **Fehlerdomänen:** Fehlerdomänen definieren eine Gruppe virtueller Computer, die gemeinsam eine Stromquelle und einen Netzwerkswitch nutzen. Standardmäßig werden die virtuellen Computer auf bis zu drei Fehlerdomänen aufgeteilt. Sie können ein bis drei Fehlerdomänen einrichten.
   * **Updatedomänen:** Standardmäßig werden fünf Updatedomänen zugewiesen. Sie können 1 bis 20 Updatedomänen einrichten. Updatedomänen definieren Gruppen virtueller Computer und zugrunde liegender physischer Hardwarekomponenten, die gleichzeitig neu gestartet werden können. Ein Beispiel mit Updatedomänen: Wenn innerhalb einer Verfügbarkeitsgruppe mehr als fünf virtuelle Computer konfiguriert sind, wird der sechste virtuelle Computer in derselben Updatedomäne platziert wie der erste, der siebte virtuelle Computer in derselben Updatedomäne wie der zweite usw. Die Neustarts erfolgen zwar nicht sequenziell, aber es werden nicht mehrere Updatedomänen gleichzeitig neu gestartet.
   * **Abonnement** : Wenn Sie über mehrere Abonnements verfügen, wählen Sie das zu verwendende Abonnement aus.
   * **Ressourcengruppe** : Wählen Sie eine vorhandene Ressourcengruppe aus, indem Sie auf den Pfeil klicken und eine Ressourcengruppe aus der Dropdownliste auswählen. Sie können auch eine neue Ressourcengruppe erstellen, indem Sie einen Namen eingeben. Der Name darf folgende Zeichen enthalten: Buchstaben, Zahlen, Punkte, Bindestriche, Unterstriche und öffnende oder schließende Klammern. Der Name darf nicht mit einem Punkt enden. Alle virtuellen Computer in der Verfügbarkeitsgruppe müssen in derselben Ressourcengruppe wie die Verfügbarkeitsgruppe erstellt werden.
   * **Speicherort** : Wählen Sie einen Speicherort aus der Dropdownliste aus.
   * **Verwaltet**: Wählen Sie *Ja* aus, um eine verwaltete Verfügbarkeitsgruppe für virtuelle Computer zu erstellen, die verwaltete Datenträger zum Speichern verwenden. Wählen Sie **Nein** aus, wenn die virtuellen Computer in der Gruppe nicht verwaltete Datenträger in einem Speicherkonto verwenden.
   
4. Wenn Sie die Eingabe der Informationen abgeschlossen haben, klicken Sie auf **Erstellen**. 

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Verwenden Sie das Portal, um gleichzeitig einen virtuellen Computer und eine Verfügbarkeitsgruppe zu erstellen.
Wenn Sie einen neuen virtuellen Computer über das Portal erstellen, können Sie auch eine neue Verfügbarkeitsgruppe für den virtuellen Computer erstellen, während Sie den ersten virtuellen Computer der Gruppe erstellen. Wenn Sie verwaltete Datenträger für Ihren virtuellen Computer auswählen, wird eine verwaltete Verfügbarkeitsgruppe erstellt.

![Screenshot der Vorgehensweise zum Erstellen einer neuen Verfügbarkeitsgruppe während der Erstellung des virtuellen Computers.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-to-an-existing-availability-set-in-the-portal"></a>Hinzufügen eines neuen virtuellen Computers zu einer vorhandenen Verfügbarkeitsgruppe im Portal
Stellen Sie bei jedem weiteren virtuellen Computer, der Teil der Gruppe sein soll, sicher, dass Sie den Computer in derselben **Ressourcengruppe** erstellen und dann in Schritt 3 die vorhandene Verfügbarkeitsgruppe auswählen. 

![Screenshot der Auswahl einer vorhandenen Verfügbarkeitsgruppe zur Verwendung durch Ihren virtuellen Computer.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-to-create-an-availability-set"></a>Erstellen einer Verfügbarkeitsgruppe mit PowerShell
In diesem Beispiel wird eine Verfügbarkeitsgruppe namens **myAvailabilitySet** in der Ressourcengruppe **myResourceGroup** am Standort **USA, Westen** erstellt. Dies muss erfolgen, bevor Sie den ersten virtuellen Computer der Gruppe erstellen.

Stellen Sie vor Beginn sicher, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning).


Wenn Sie nicht verwaltete Datenträger für Ihre virtuellen Computer verwenden, geben Sie Folgendes ein:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

Wenn Sie Ihre eigenen Speicherkonten für Ihre virtuellen Computer verwenden, geben Sie Folgendes ein:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

Weitere Informationen finden Sie unter [New-AzureRmAvailabilitySet](/powershell/new-azurermavailabilityset).

## <a name="troubleshooting"></a>Problembehandlung
* Wenn Sie beim Erstellen eines virtuellen Computers feststellen, dass sich die gewünschte Verfügbarkeitsgruppe nicht in der Dropdownliste im Portal befindet, haben Sie sie möglicherweise in einer anderen Ressourcengruppe erstellt. Wenn Sie die Ressourcengruppe Ihrer Verfügbarkeitsgruppe nicht kennen, wechseln Sie zum Menü des Hubs und klicken auf „Durchsuchen“ > „Verfügbarkeitsgruppen“, um eine Liste Ihrer Verfügbarkeitsgruppen und der Ressourcengruppen anzuzeigen, zu denen sie gehören.

## <a name="next-steps"></a>Nächste Schritte
Erweitern Sie den Speicher für Ihren virtuellen Computer, indem Sie einen zusätzlichen [Datenträger](virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)hinzufügen.


