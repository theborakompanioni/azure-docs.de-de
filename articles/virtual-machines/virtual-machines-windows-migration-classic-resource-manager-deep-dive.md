<properties
	pageTitle="Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager"
	description="Dieser Artikel enthält ausführliche technische Informationen zu plattformgestützten Migrationsdienstfunktionen der Dienstverwaltung für die Migration zu Azure Resource Manager."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="mahthi"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="mahthi"/>

# Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager

In diesem Artikel beschäftigen wir uns ausführlicher mit den technischen Aspekten der Migration auf Ressourcen- und Featureebene, um zu vermitteln, wie die Plattform Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager migriert. Lesen Sie sich den Artikel mit der Dienstankündigung durch: [Platform supported migration of IaaS resources from Classic to Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md) (Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager).

## Ausführliche Anleitung zur Migration auf Ressourcen- und Featureebene

Im Anschluss finden Sie Informationen zur Darstellung der Ressourcen im klassischen Bereitstellungsmodell und in Resource Manager. In dieser Tabelle nicht enthaltene Features und Ressourcen werden derzeit nicht unterstützt.

| Klassische Darstellung | Resource Manager-Darstellung | Ausführliche Hinweise | | |
|--------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|---|
| Cloudienstname | DNS-Name | Bei der Migration erstellen wir für jeden Clouddienst eine neue Ressourcengruppe mit dem Benennungsmuster `<cloudservicename>-migrated`, die alle Ressourcen enthält. Der Clouddienstname wird zu einem DNS-Namen, der der öffentlichen IP-Adresse zugeordnet ist. | | |
| Virtual Machine | Virtual Machine | VM-spezifische Eigenschaften werden unverändert migriert. Beachten Sie, dass bestimmte osProfile-Informationen wie etwa der Computername im klassischen Bereitstellungsmodell nicht gespeichert wurden und daher nach der Migration leer bleiben. | | |
| An virtuellen Computer angefügte Datenträgerressourcen | An virtuellen Computer angefügte implizite Datenträger | Datenträger werden im Resource Manager-Bereitstellungsmodell nicht als Ressourcen oberster Ebene modelliert. Sie werden als implizite Datenträger unter dem virtuellen Computer migriert. Aktuell werden nur Datenträger unterstützt, die an einen virtuellen Computer angefügt sind. Die Migration nicht zugeordneter Datenträger wird in naher Zukunft unterstützt. Klassische Speicherkonten können jetzt von Resource Manager-VMs verwendet werden, um die Migration zu ermöglichen. Dadurch lassen sich die Datenträger problemlos und ganz ohne Updates zum Resource Manager-Modell migrieren. | | |
| VM-Erweiterungen | VM-Erweiterungen | Mit Ausnahme von XML-Erweiterungen werden alle Ressourcenerweiterungen aus dem klassischen Bereitstellungsmodell migriert. | | |
| Zertifikate für virtuelle Computer | Zertifikate in Azure Key Vault | Wenn ein Clouddienst Dienstzertifikate enthält, erstellt die Plattform jeweils eine neue Azure Key Vault-Instanz pro Clouddienst und verschiebt die Zertifikate in den Schlüsseltresor. Die VMs werden aktualisiert, um auf die Zertifikate aus dem Schlüsseltresor zu verweisen. | | |
| WinRM-Konfiguration | WinRM-Konfiguration unter osProfile | Die WinRM-Konfiguration wird im Rahmen der Migration unverändert übernommen. | | |
| Verfügbarkeitsgruppe als Eigenschaft | Verfügbarkeitsgruppe als Ressource | Im klassischen Bereitstellungsmodell handelte es sich bei der Angabe der Verfügbarkeitsgruppe um eine Eigenschaft auf dem virtuellen Computer. Bei der Migration werden Verfügbarkeitsgruppen zu einer Ressource der obersten Ebene. Folgende Konfigurationen werden nicht unterstützt: mehrere Verfügbarkeitsgruppen pro Clouddienst sowie eine Kombination aus mindestens einer Verfügbarkeitsgruppe und virtuellen Computern, die keiner Verfügbarkeitsgruppe in einem Clouddienst angehören. | | |
| Netzwerkkonfiguration auf einem virtuellen Computer | Primäre Netzwerkschnittstelle | Die Netzwerkkonfiguration auf einem virtuellen Computer wird nach der Migration als primäre Netzwerkschnittstellenressource dargestellt. Bei virtuellen Computern, die sich nicht in einem VNET befinden, ändert sich im Zuge der Migration die interne IP-Adresse. | | |
| Mehrere Netzwerkschnittstellen auf einem virtuellen Computer | Netzwerkschnittstellen | Sind einem virtuellen Computer mehrere Netzwerkschnittstellen zugeordnet, wird jede Netzwerkschnittstelle im Rahmen der Migration zu einer Ressource der obersten Ebene im Resource Manager-Bereitstellungsmodell (einschließlich aller Eigenschaften). | | |
| Lastenausgleichs-Endpunktsatz | Lastenausgleichsmodul | Im klassischen Bereitstellungsmodell wurde von der Plattform für jeden Clouddienst ein impliziter Load Balancer zugewiesen. Im Zuge der Migration wird eine neue Load Balancer-Ressource erstellt, und der Lastenausgleichs-Endpunktsatz wird zu Load Balancer-Regeln. | | |
| Eingehende NAT-Regeln | Eingehende NAT-Regeln | Für den virtuellen Computer definierte Eingabeendpunkte werden bei der Migration in eingehende NAT-Regeln für den Load Balancer umgewandelt. | | |
| Virtuelle IP-Adresse (VIP-Adresse) | Öffentliche IP-Adresse mit DNS-Name | Die virtuelle IP-Adresse (VIP-Adresse) wird zu einer öffentlichen IP-Adresse und dem Load Balancer zugeordnet. | | |
| Virtuelles Netzwerk | Virtuelles Netzwerk | Das virtuelle Netzwerk wird mit allen zugehörigen Eigenschaften in das Resource Manager-Bereitstellungsmodell überführt. Eine neue Ressourcengruppe namens `-migrated` wird erstellt. Informationen zu nicht unterstützten Konfigurationen finden Sie [hier](virtual-machines-windows-migration-classic-resource-manager.md). | | |
| Reservierte IP-Adressen | Öffentliche IP-Adresse mit statischer Zuordnungsmethode (allocationMethod) | Dem Load Balancer zugeordnete reservierte IP-Adressen werden zusammen mit dem Clouddienst oder virtuellen Computer migriert. Die Migration nicht zugeordneter reservierter IP-Adressen wird derzeit nicht unterstützt. | | |
| Öffentliche IP-Adresse pro virtuellem Computer | Öffentliche IP-Adresse mit dynamischer Zuordnungsmethode (allocationMethod) | Die dem virtuellen Computer zugeordnete öffentliche IP-Adresse wird in eine öffentliche IP-Adressressource mit statischer Zuordnungsmethode konvertiert. | | |
| Netzwerksicherheitsgruppen | Netzwerksicherheitsgruppen | Einem Subnetz zugeordnete NSGs werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Die NSG aus dem klassischen Bereitstellungsmodell wird bei der Migration nicht entfernt. Während der Migration werden jedoch Vorgänge auf der Verwaltungsebene für die NSG blockiert. | | |
| DNS-Server | DNS-Server | DNS-Server, die einem VNET oder dem virtuellen Computer zugeordnet sind, werden im Rahmen der entsprechenden Ressourcenmigration zusammen mit allen Eigenschaften migriert. | | |
| Benutzerdefinierte Routen | Benutzerdefinierte Routen | Einem Subnetz zugeordnete benutzerdefinierte Routen (User Defined Routes, UDRs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Die UDR aus dem klassischen Bereitstellungsmodell wird bei der Migration nicht entfernt. Während der Migration werden jedoch Vorgänge auf der Verwaltungsebene für die UDR blockiert. | | |
| IP-Weiterleitungseigenschaft für die Netzwerkkonfiguration eines virtuellen Computers | IP-Weiterleitungseigenschaft für die Netzwerkschnittstellenkarte | Die IP-Weiterleitungseigenschaft für einen virtuellen Computer wird im Zuge der Migration in eine Eigenschaft für die Netzwerkschnittstelle konvertiert. | | |
| Load Balancer mit mehreren IP-Adressen | Load Balancer mit mehreren öffentlichen IP-Ressourcen | Sämtliche öffentliche, dem Load Balancer zugeordnete IP-Adressen werden in eine öffentliche IP-Ressource konvertiert und dem Load Balancer nach der Migration zugeordnet. | | |
| Interne DNS-Namen für den virtuellen Computer | Interne DNS-Namen für die Netzwerkschnittstellenkarte | Bei der Migration werden die internen DNS-Suffixe aktualisiert, um den Suffixen des Resource Manager-Bereitstellungsmodells zu entsprechen. Die Namensauflösung wird durch die Migration nicht beeinträchtigt. Die den virtuellen Computern zugeordneten Suffixe werden jedoch geändert. | | |

## Exemplarische Vorgehensweise für eine einfache Migration

Die folgenden Beispielscreenshots zeigen einen Clouddienst mit einem (nicht in einem VNET enthaltenen) virtuellen Computer nach der Vorbereitungsphase.

![Screenshot der klassischen Darstellung nach der Vorbereitung](./media/virtual-machines-windows-migration-classic-resource-manager/classic-migration-prepare-portal.png) ![Screenshot der Resource Manager-Darstellung nach der Vorbereitung](./media/virtual-machines-windows-migration-classic-resource-manager/resourcemanager-migration-prepare-portal.png)

## Nächste Schritte

Nachdem nun Sie eine Vorstellung von der Migration klassischer IaaS-Ressourcen zu Resource Manager haben, können Sie damit beginnen, die Ressourcen zu migrieren.

- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Platform supported migration of IaaS resources from Classic to Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md) (Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager)
- [Clone a classic Virtual Machine to Azure Resource Manager using Community PowerShell Scripts](virtual-machines-windows-migration-scripts.md) (Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community)

<!---HONumber=AcomDC_0511_2016-->