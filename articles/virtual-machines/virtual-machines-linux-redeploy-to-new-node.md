<properties 
	pageTitle="Virtuelle Linux-Computer erneut bereitstellen | Microsoft Azure" 
	description="Beschreibt, wie Sie virtuelle Linux-Computer zum Beheben von Problemen mit der SSH-Verbindung bereitstellen." 
	services="virtual-machines-linux" 
	documentationCenter="virtual-machines" 
	authors="iainfoulds" 
	manager="timlt"
	tags="azure-resource-manager,top-support-issue" 
/>
	

<tags 
	ms.service="virtual-machines-linux" 
	ms.devlang="na" 
	ms.topic="support-article" 
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure" 
	ms.date="09/19/2016" 
	ms.author="iainfou" 
/>

# Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen

Wenn Sie Schwierigkeiten mit der Problembehandlung bei SSH oder dem Anwendungszugriff auf einen virtuellen Azure-Computer haben, lassen sich diese u. U. durch das erneute Bereitstellen des virtuellen Computers beseitigen. Wenn Sie einen virtuellen Computer erneut bereitstellen, wird er innerhalb der Azure-Infrastruktur auf einen neuen Knoten verschoben und dann wieder eingeschaltet. Dabei werden alle Ihre Konfigurationsoptionen und zugehörigen Ressourcen beibehalten. In diesem Artikel erfahren Sie, wie ein virtueller Computer mithilfe der Azure-Befehlszeilenschnittstelle oder dem Azure-Portal erneut bereitgestellt wird.

> [AZURE.NOTE] Nachdem Sie einen virtuellen Computer erneut bereitgestellt haben, geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert.


## Verwenden der Azure-Befehlszeilenschnittstelle

Stellen Sie sicher, dass die [aktuelle Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) auf Ihrem Computer installiert ist und Sie sich im Resource Manager-Modus (`azure config mode arm`) befinden.

Verwenden Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle, um Ihren virtuellen Computer erneut bereitzustellen:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Der Status der Änderung des virtuellen Computers wird während des Vorgangs der erneuten Bereitstellung angezeigt. Während der virtuelle Computer den Vorgang der erneuten Bereitstellung auf einem neuen Host durchläuft, ändert sich sein `PowerState` von „Wird ausgeführt“ zu „Wird aktualisiert“, dann zu „Wird gestartet“ und schließlich wieder zu „Wird ausgeführt“. Überprüfen Sie den Status der virtuellen Computer innerhalb einer Ressourcengruppe mit:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## Nächste Schritte
Falls beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, finden Sie spezifische Hilfe unter [Problembehandlung bei SSH-Verbindungen](virtual-machines-linux-troubleshoot-ssh-connection.md) oder [Ausführliche Schritte zur Problembehandlung bei SSH](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Sie können auch die Informationen zur [Problembehandlung bei der Anwendung](virtual-machines-linux-troubleshoot-app-connection.md) lesen, wenn Sie auf eine Anwendung, die auf Ihrem virtuellen Computer ausgeführt wird, nicht zugreifen können.

<!---HONumber=AcomDC_0921_2016-->