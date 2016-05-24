<properties
	pageTitle="Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle"
	description="Dieser Artikel beschreibt die plattformgestützten Migrationsdienstfunktionen der Dienstverwaltung für die Migration zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle."
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

Hier erfahren Sie, wie Sie IaaS-Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle aus dem klassischen Bereitstellungsmodell zu Resource Manager migrieren. Für diesen Artikel muss die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) angemeldet sein (`azure login`).

## Schritt 1: Vorbereiten der Migration

Hier finden Sie einige bewährte Methoden für den Fall, dass Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen.

- Eine Liste mit nicht unterstützten Konfigurationen und Features finden Sie [hier](virtual-machines-windows-migration-classic-resource-manager.md). Sollten Sie über virtuelle Computer mit nicht unterstützten Konfigurationen oder Features verfügen, empfiehlt es sich, auf die Ankündigung der Unterstützung der entsprechenden Features/Konfigurationen zu warten. Alternativ können Sie ggf. die betroffenen Features entfernen oder eine andere Konfiguration verwenden, um die Migration zu ermöglichen.
-	Falls Sie über automatisierte Skripts zur Bereitstellung Ihrer Infrastruktur und Anwendungen verfügen, erstellen Sie eine ähnliche Testumgebung mit diesen Skripts für die Migration. Alternativ können Sie auch Beispielumgebungen über das Azure-Portal einrichten.
- Da es sich bei dem Dienst um eine öffentliche Vorschauversion handelt, muss die Testumgebung für die Migration von der Produktionsumgebung isoliert sein. Speicherkonten, VNETs und andere Ressourcen der Test- und Produktionsumgebung dürfen nicht gemischt werden.

## Schritt 2: Festlegen Ihres Abonnements und Registrieren für die öffentliche Vorschauversion der Migration

Für Migrationsszenarien müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten. [Installieren Sie die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und [wählen Sie Ihr Abonnement aus](../xplat-cli-connect.md).

Führen Sie zum Auswählen des Azure-Abonnements den folgenden Befehl aus:

	azure account set "azure-subscription-name"

Registrieren Sie sich mithilfe des folgenden Befehls für die öffentliche Vorschau. Es kann vorkommen, dass für diesen Befehl eine Zeitüberschreitung auftritt. Die Registrierung ist aber trotzdem erfolgreich. Führen Sie den nächsten Schritt aus, um den Status der Registrierung zu prüfen.

	azure provider register Microsoft.ClassicInfrastructureMigrate

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Warten Sie daher etwas. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden. Vergewissern Sie sich, dass „RegistrationState“ den Wert „Registered“ besitzt, bevor Sie den Vorgang fortsetzen.

	azure provider show Microsoft.ClassicInfrastructureMigrate

Wechseln Sie nun in den asm-Modus der Befehlszeilenschnittstelle:

	azure config mode asm

## Schritt 3: Befehle zum Migrieren von IaaS-Ressourcen

>[AZURE.NOTE] Alle im Anschluss beschriebenen Vorgänge sind idempotent. Sollte ein Fehler auftreten, der nicht auf ein nicht unterstütztes Feature oder auf eine nicht unterstützte Konfiguration zurückzuführen ist, wiederholen Sie den prepare-, abort- oder commit-Vorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

### Migrieren virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)

Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Falls sich die virtuellen Computer in dem Clouddienst in einem VNET befinden oder über Web-/Workerrollen verfügen, wird eine Fehlermeldung zurückgegeben.

	azure service list

Führen Sie den folgenden Befehl aus, um in der ausführlichen Ausgabe den Bereitstellungsnamen für den Clouddienst zu ermitteln. In den meisten Fällen entspricht der Bereitstellungsname dem Namen des Clouddiensts.

	azure service show servicename -vv

Bereiten Sie die virtuellen Computer des Clouddiensts für die Migration vor. Dabei stehen Ihnen zwei Optionen zur Verfügung.

Wenn Sie die virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk migrieren möchten, verwenden Sie den folgenden Befehl:

	azure service deployment prepare-migration servicename deploymentname new "" "" ""

Wenn Sie als Migrationsziel ein vorhandenes virtuelles Netzwerk unter dem Resource Manager-Bereitstellungsmodell verwenden möchten, führen Sie den folgenden Befehl aus:

	azure service deployment prepare-migration serviceName deploymentName existing destinationVNETResourceGroupName subnetName vnetName

Nach der Vorbereitung können Sie sich in der ausführlichen Ausgabe über den Migrationszustand der virtuellen Computer informieren und sich vergewissern, dass der Zustand der virtuellen Computer „Prepared“ lautet.

	azure vm show vmName -vv

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe der Befehlszeilenschnittstelle oder über das Azure-Portal. Falls Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, führen Sie den folgenden Befehl aus:

	azure service deployment abort-migration serviceName deploymentName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	azure service deployment commit-migration serviceName deploymentName

### Migrieren virtueller Computer in einem virtuellen Netzwerk

Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. Falls das virtuelle Netzwerk Web-/Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Validierungsfehler auf.

Rufen Sie mithilfe des folgenden Befehls alle virtuellen Netzwerke im Abonnement ab:

	azure network vnet list

Bereiten Sie das gewünschte virtuelle Netzwerk mithilfe des folgenden Befehls für die Migration vor:

	azure network vnet prepare-migration virtualnetworkname

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe der Befehlszeilenschnittstelle oder über das Azure-Portal. Falls Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, führen Sie den folgenden Befehl aus:

	azure network vnet abort-migration virtualnetworkname

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	azure network vnet commit-migration virtualnetworkname

## Referenzen

- [Platform supported migration of IaaS resources from Classic to Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md) (Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager)
- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)

<!---HONumber=AcomDC_0511_2016-->