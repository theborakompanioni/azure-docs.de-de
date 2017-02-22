---
title: Technische Artikel zu virtuellen Windows-Computern (klassisch) | Microsoft Azure
description: "Vollständige Liste der Microsoft Azure-Artikel zu virtuellen Windows-Computern im klassischen Bereitstellungsmodell"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-service-management
editor: 
ms.assetid: 7f9a508b-34ec-4bdb-92d1-8844480369d5
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 26c58ae4c509cb768807875ecdf96e9a24d6a472
ms.openlocfilehash: 263b3a1edc94a60e756850b2d917840be87e7a5a


---
# <a name="technical-articles-for-windows-vms-in-the-classic-deployment-model"></a>Technische Artikel zu virtuellen Windows-Computern im klassischen Bereitstellungsmodell
Hier finden Sie die gesamte Dokumentation, die Sie zum Erstellen und Verwalten Windows-basierter virtueller Azure-Computer im klassischen Bereitstellungsmodell benötigen.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

## <a name="overview"></a>Übersicht
[About virtual machines (Informationen zu virtuellen Computern)](virtual-machines-windows-about.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Häufig gestellte Fragen zu virtuellen Azure-Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden](virtual-machines-windows-classic-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Azure App Service, Azure Virtual Machines, Service Fabric und Azure Cloud Services im Vergleich](../app-service-web/choose-web-site-cloud-service-vm.md)

[Virtuelle Computer und Container in Azure](virtual-machines-windows-containers.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="environment-setup"></a>Einrichten der Umgebung
[Kostenloses Konto](https://azure.microsoft.com/free/)

[Installieren von Azure PowerShell](/powershell/azureps-cmdlets-docs)

[Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)

## <a name="get-started"></a>Erste Schritte
[Lernpfad für virtuelle Windows-Computer](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)

[Create a Windows virtual machine in the Azure classic portal (Erstellen eines virtuellen Windows-Computers im klassischen Azure-Portal)](virtual-machines-windows-classic-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird](virtual-machines-windows-classic-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="plan"></a>Plan
[About images for classic virtual machines (Informationen zu Images für virtuelle Computer)](virtual-machines-windows-classic-about-images.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Größen für virtuelle Computer](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Informationen zu virtuellen Computern der H-Serie und der rechenintensiven A-Serie](virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Geplante Wartung für virtuelle Azure-Computer](virtual-machines-windows-planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Implementierungsrichtlinien für Azure-Infrastrukturdienste](virtual-machines-windows-infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Create an availability set for virtual machines (Erstellen einer Verfügbarkeitsgruppe für virtuelle Computer)](virtual-machines-windows-classic-configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="deploy"></a>Bereitstellen
[Erstellen eines benutzerdefinierten virtuellen Computers mit Windows](virtual-machines-windows-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Capture a Windows virtual machine created in the classic deployment model (Erfassen eines virtuellen Windows-Computers, der im klassischen Bereitstellungsmodell erstellt wurde)](virtual-machines-windows-classic-capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Erstellen und Hochladen einer klassischen Windows Server-VHD mithilfe von PowerShell](virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Automatisieren der Bereitstellung virtueller Azure-Computer mit Chef](virtual-machines-windows-chef-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Create and configure a classic Windows Virtual Machine in Azure PowerShell (Erstellen und Konfigurieren eines klassischen virtuellen Windows-Computers in Azure PowerShell)](virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Einfügen benutzerdefinierter Daten in einen virtuellen Azure-Computer](virtual-machines-windows-classic-inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="manage"></a>Verwalten
[Verwalten Ihrer virtuellen Computer mithilfe von Azure PowerShell](virtual-machines-windows-classic-manage-psh.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)

[Informationen zum Agent und zu Erweiterungen für virtuelle Computer](virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Verwalten von Erweiterungen für virtuelle Computer](virtual-machines-windows-classic-manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Custom Script extension for classic Windows virtual machines (Benutzerdefinierte Skripterweiterung für klassische virtuelle Windows-Computer)](virtual-machines-windows-classic-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Plattformgestützte Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)

## <a name="configure"></a>Konfigurieren
[Zurücksetzen eines Kennworts oder des Remotedesktopdiensts für einen virtuelle Windows-Computer](virtual-machines-windows-reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Installieren und Konfigurieren von Symantec Endpoint Protection auf einem virtuellen Windows-Computer](virtual-machines-windows-classic-install-symantec.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem virtuellen Windows-Computer](virtual-machines-windows-classic-install-trend.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Konfigurieren einer Verfügbarkeitsgruppe für virtuelle Computer im klassischen Bereitstellungsmodell](virtual-machines-windows-classic-configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Einrichten von Endpunkten auf einer klassischen virtuellen Azure-Maschine](virtual-machines-windows-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>Speicher
[Informationen zu Datenträgern und VHDs für virtuelle Azure-Computer](../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[How to attach a data disk to a classic Windows virtual machine (Anfügen eines Datenträgers an einen klassischen virtuellen Windows-Computer)](virtual-machines-windows-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[How to dettach a data disk to a classic Windows virtual machine (Trennen eines Datenträgers von einem klassischen virtuellen Windows-Computer)](virtual-machines-windows-classic-detach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Verwenden des Laufwerks D als Datenlaufwerk auf einer Windows-VM](virtual-machines-windows-classic-change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="networking"></a>Netzwerk
[Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)

[Verbinden virtueller Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mit einem virtuellen Netzwerk oder einem Clouddienst](virtual-machines-windows-classic-connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Erstellen von NSGs (klassisch) in PowerShell](../virtual-network/virtual-networks-create-nsg-classic-ps.md)

[Einrichten eines Load Balancers](../load-balancer/load-balancer-get-started-internet-classic-portal.md)

## <a name="develop"></a>Entwickeln
[Erstellen und Verwalten virtueller Azure-Computer in Visual Studio](virtual-machines-windows-classic-manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Erstellen eines virtuellen Computers für eine Webanwendung mit Visual Studio](virtual-machines-windows-classic-web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Gewusst wie: Ausführen einer rechenintensiven Aufgabe in Java auf einem virtuellen Computer](virtual-machines-windows-classic-java-run-compute-intensive-task.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Django-Webanwendung "Hello World" auf einem virtuellen Windows Server-Computer](virtual-machines-windows-classic-python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="workloads"></a>Workloads
[HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[MongoDB](virtual-machines-windows-classic-install-mongodb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[MySQL](virtual-machines-windows-classic-mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Oracle](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html#support)

[SAP](virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[SQL Server](./windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[Tomcat](virtual-machines-windows-classic-java-run-tomcat-app-server.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="reference"></a>Referenz
[Befehle der Azure-Befehlszeilenschnittstelle im Dienstverwaltungsmodus](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)

[Dienstverwaltungs-REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx)

[Service Management .NET API (Dienstverwaltungs-.NET-API)](https://msdn.microsoft.com/library/azure/mt420161.aspx)

[Azure Service Management PowerShell cmdlet reference documentation (Referenz zu PowerShell-Cmdlets für die Azure-Dienstverwaltung)](https://msdn.microsoft.com/library/azure/dn708504.aspx)

## <a name="troubleshooting"></a>Problembehandlung
[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](virtual-machines-windows-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größen von virtuellen Computern in Azure](virtual-machines-windows-allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Behandeln von Problemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)](virtual-machines-windows-classic-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

[Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Windows-Computers in Azure (klassisches Bereitstellungsmodell)](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)



<!--HONumber=Feb17_HO3-->


