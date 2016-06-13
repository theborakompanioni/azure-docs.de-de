<properties
	pageTitle="Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft Azure"
	description="In diesem Artikel wird die plattformgestützte Migration von Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle erläutert."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="mahthi"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="mahthi"/>

# Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle

Diese Schritte zeigen, wie Sie Befehle der Azure-Befehlszeilenschnittstelle zum Migrieren von IaaS-Ressourcen (Infrastructure as a Service) aus dem klassischen Bereitstellungsmodell in das Azure Resource Manager-Bereitstellungsmodell verwenden. Für diesen Artikel ist die Anmeldung bei der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) erforderlich (`azure login`).

## Schritt 1: Vorbereiten der Migration

Hier finden Sie einige bewährte Methoden, die wir empfehlen, wenn Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen:

- Sehen Sie sich die [Liste mit nicht unterstützten Konfigurationen und Features](virtual-machines-windows-migration-classic-resource-manager.md) an. Verwenden Ihre virtuellen Computer nicht unterstützte Konfigurationen oder Features, empfiehlt es sich, zu warten, bis die Unterstützung für die entsprechenden Features/Konfigurationen angekündigt wird. Alternativ können Sie ggf. die betroffenen Features entfernen oder eine andere Konfiguration verwenden, um die Migration zu ermöglichen.
-	Wenn Sie derzeit über automatisierte Skripts zum Bereitstellen Ihrer Infrastruktur und Anwendungen verfügen, versuchen Sie, mithilfe dieser Skripts für die Migration eine ähnliche Testeinrichtung zu erstellen. Alternativ dazu können Sie über das Azure-Portal Beispielumgebungen einrichten.
- Da sich der Dienst in der öffentlichen Vorschau befindet, muss die Testumgebung für die Migration von der Produktionsumgebung isoliert sein. Speicherkonten, virtuelle Netzwerke und andere Ressourcen der Test- und Produktionsumgebungen dürfen nicht gemischt werden.

## Schritt 2: Festlegen Ihres Abonnements und Registrieren für die öffentliche Vorschauversion der Migration

Für Migrationsszenarien müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten. [Installieren Sie die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und [wählen Sie Ihr Abonnement aus](../xplat-cli-connect.md).

Führen Sie zum Auswählen des Azure-Abonnements den folgenden Befehl aus.

	azure account set "azure-subscription-name"

Registrieren Sie sich mithilfe des folgenden Befehls für die öffentliche Vorschau. Beachten Sie, dass in einigen Fällen für diesen Befehl ein Timeout festgelegt ist. Die Registrierung wird jedoch erfolgreich durchgeführt.

	azure provider register Microsoft.ClassicInfrastructureMigrate

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Warten Sie daher etwas. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden. Stellen Sie sicher, dass der RegistrationState-Wert `Registered` lautet, bevor Sie fortfahren.

	azure provider show Microsoft.ClassicInfrastructureMigrate

Wechseln Sie nun in den `asm`-Modus der Befehlszeilenschnittstelle:

	azure config mode asm

## Schritt 3: Ausführen von Befehlen zum Migrieren Ihrer IaaS-Ressourcen

>[AZURE.NOTE] Alle hier beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

### Migrieren virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)

Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Beachten Sie: Falls sich die virtuellen Computer im Clouddienst in einem virtuellen Netzwerk befinden oder über Web-/Workerrollen verfügen, wird eine Fehlermeldung zurückgegeben.

	azure service list

Führen Sie den folgenden Befehl aus, um in der ausführlichen Ausgabe den Bereitstellungsnamen für den Clouddienst zu ermitteln. In den meisten Fällen entspricht der Bereitstellungsname dem Namen des Clouddiensts.

	azure service show servicename -vv

Bereiten Sie die virtuellen Computer im Clouddienst für die Migration vor. Dabei stehen Ihnen zwei Optionen zur Auswahl.

Wenn Sie die virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk migrieren möchten, verwenden Sie den folgenden Befehl.

	azure service deployment prepare-migration servicename deploymentname new "" "" ""

Wenn Sie als Migrationsziel ein vorhandenes virtuelles Netzwerk im Resource Manager-Bereitstellungsmodell verwenden möchten, führen Sie den folgenden Befehl aus.

	azure service deployment prepare-migration serviceName deploymentName existing destinationVNETResourceGroupName subnetName vnetName

Nach der Vorbereitung können Sie sich in der ausführlichen Ausgabe über den Migrationsstatus der virtuellen Computer informieren und sich vergewissern, dass sich die virtuellen Computer im Status `Prepared` befinden.

	azure vm show vmName -vv

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe der Befehlszeilenschnittstelle oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum vorherigen Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

	azure service deployment abort-migration serviceName deploymentName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

	azure service deployment commit-migration serviceName deploymentName

### Migrieren virtueller Computer in einem virtuellen Netzwerk

Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. Beachten Sie: Falls das virtuelle Netzwerk Web-/Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Validierungsfehler auf.

Rufen Sie mithilfe des folgenden Befehls alle virtuellen Netzwerke im Abonnement ab.

	azure network vnet list

Bereiten Sie das gewünschte virtuelle Netzwerk mithilfe des folgenden Befehls für die Migration vor.

	azure network vnet prepare-migration virtualnetworkname

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe der Befehlszeilenschnittstelle oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum vorherigen Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

	azure network vnet abort-migration virtualnetworkname

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

	azure network vnet commit-migration virtualnetworkname

## Nächste Schritte

- [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)

<!---HONumber=AcomDC_0601_2016-->