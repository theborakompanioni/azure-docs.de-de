# Übersicht
## [About virtual machines (Informationen zu virtuellen Computern)](../../virtual-machines-windows-about.md)
## [Datenträger und VHDs](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
## [Virtuelle Netzwerke](../../../virtual-network/virtual-networks-overview.md)
## [Häufig gestellte Fragen](faq.md)
## [Azure App Service, Azure Virtual Machines, Service Fabric und Azure Cloud Services im Vergleich](../../../app-service/choose-web-site-cloud-service-vm.md)
## [Container](../../virtual-machines-windows-containers.md)

# Erste Schritte
## [Erstellen eines virtuellen Computers über das Portal](tutorial.md)
## [Anmelden an einer VM](connect-logon.md)
## [Installieren von Azure PowerShell](/powershell/azure/overview)
## [Installieren der Azure-Befehlszeilenschnittstelle](../../../cli-install-nodejs.md)

# Anleitung

## Verwenden von Azure Storage
### [Anfügen eines Datenträgers](attach-disk.md)
### [Trennen eines Datenträgers](detach-disk.md)
### [Verwenden von D: als Datenträger](../../virtual-machines-windows-change-drive-letter.md)

## Netzwerk
### [Einrichten von Endpunkten](setup-endpoints.md)
### [Verbinden von VMs mit einem VNET oder Clouddienst](connect-vms.md)
### [Verbinden von klassischen VNETs und Resource Manager-VNETs](../../../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
### [Einrichten eines Load Balancers](../../../load-balancer/load-balancer-get-started-internet-classic-portal.md)
### [Erstellen von NSGs (klassisch) in PowerShell](../../../virtual-network/virtual-networks-create-nsg-classic-ps.md)

## Bereitstellen
### [Erstellen einer benutzerdefinierten VM](createportal.md)
### [Erstellen und Konfigurieren eines virtuellen Computers mit Azure PowerShell](create-powershell.md)
### [Erfassen einer Windows-VM](capture-image.md)
### [Erstellen und Hochladen einer virtuellen Festplatte mithilfe von PowerShell](createupload-vhd.md)
### [Automatisieren der Bereitstellung von Azure-VMs mit Chef](../../virtual-machines-windows-chef-automation.md)
### [Erstellen und Verwalten von VMs in Visual Studio](manage-visual-studio.md)
### [Erstellen eines virtuellen Computers für eine Web-App mit Visual Studio](web-app-visual-studio.md)
### [Ausführen einer rechenintensiven Aufgabe in Java](java-run-compute-intensive-task.md)
### [Django-Webanwendung „Hello World“](python-django-web-app.md)

## Konfigurieren
### [Zurücksetzen eines Kennworts oder des Remotedesktopdiensts](../../virtual-machines-windows-reset-rdp.md)
### [Installieren und Konfigurieren von Symantec Endpoint Protection](install-symantec.md)
### [Installieren und Konfigurieren von Trend Micro Deep Security als Dienst](install-trend.md)
### [Konfigurieren einer Verfügbarkeitsgruppe](configure-availability.md)
### [Ändern der Größe eines virtuellen Windows-Computers, der im klassischen Bereitstellungsmodell erstellt wurde](resize-vm.md)
### [Wartung](planned-maintenance-schedule.md)

## Verwalten
### [Migrieren von klassischen Bereitstellungen zu Resource Manager-Bereitstellungen](../../virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
### [Verwalten Ihrer VMs mit Azure PowerShell](manage-psh.md)
### [Informationen zum VM-Agent und zu Erweiterungen](agents-and-extensions.md)
### [Verwalten von VM-Erweiterungen](manage-extensions.md)
### [Benutzerdefinierte Skripterweiterung für VMs](extensions-customscript.md)
### [Einfügen benutzerdefinierter Daten in einen virtuellen Azure-Computer](inject-custom-data.md)

## Plan
### [Informationen zu Images](about-images.md)
### [Größen für virtuelle Computer](../../virtual-machines-windows-sizes.md)
### [Geplante Wartung für virtuelle Azure-Computer](../../virtual-machines-windows-planned-maintenance.md)
### [Implementierungsrichtlinien für Azure-Infrastrukturdienste](../../virtual-machines-windows-infrastructure-subscription-accounts-guidelines.md)

## Verwalten von Workloads
### [High Performance Computing (HPC)](../../virtual-machines-windows-hpcpack-cluster-options.md)
#### [Automatisches Skalieren von Ressourcen](hpcpack-cluster-node-autogrowshrink.md)
#### [Verwalten von Computeknoten](hpcpack-cluster-node-manage.md)
#### [Erstellen eines Clusters](hpcpack-cluster-powershell-script.md)
#### [Einrichten eines Clusters zum Ausführen von MPI-Anwendungen](hpcpack-rdma-cluster.md)
#### [Ausführen von Excel- und SOA-Workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md)
#### [Erstellen des Hauptknotens mit einem Marketplace-Image](../../virtual-machines-windows-hpcpack-cluster-headnode.md)
#### [Übermitteln von Aufträgen aus der lokalen Bereitstellung an Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md)
### [MongoDB](install-mongodb.md)
### [MySQL](mysql-2008r2.md)
### [Oracle](../../workloads/oracle/oracle-considerations.md)
### [SAP](sap-get-started.md)
### [SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
### [Tomcat](java-run-tomcat-app-server.md)

## Problembehandlung
### [Remotedesktopverbindungen](../../virtual-machines-windows-troubleshoot-rdp-connection.md)
####[Detaillierte Problembehandlung beim Herstellen einer Remotedesktopverbindung](../../virtual-machines-windows-detailed-troubleshoot-rdp.md)
### [Zugreifen auf eine Anwendung](../../virtual-machines-windows-troubleshoot-app-connection.md)
### [Behandeln von typischen Problemen beim Erstellen eines neuen virtuellen Computers](troubleshoot-deployment-new-vm.md)
### [Behandeln von typischen Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Computers](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
### [Zurücksetzen des RDP-Kennworts](reset-rdp.md)
### [Anfügen einer virtuellen Festplatte an den virtuellen Problembehandlungscomputer](troubleshoot-recovery-disks-portal.md)

# Referenz
## [PowerShell](/powershell/azure/overview)
## [Azure-Befehlszeilenschnittstelle](/cli/azure/vm)
## [Java](/java/api)
## [.NET](/dotnet/api/microsoft.azure.management.compute)
## [Erstellen von Resource Manager-Vorlagen](../../../resource-group-authoring-templates.md)
## [Communityvorlagen](https://azure.microsoft.com/documentation/templates)
## [Compute REST](/rest/api/compute)
## [Network REST](/rest/api)
## [Storage REST](/rest/api/storageservices)

# Ressourcen
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=compute)
## [Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Regionale Verfügbarkeit](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-machine)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=virtual-machines)
