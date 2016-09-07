<properties
	pageTitle="Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager | Microsoft Azure"
	description="Dieser Artikel bietet ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="mahthi"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/18/2016"
	ms.author="kasing"/>

# Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager

In diesem Artikel beschäftigen wir uns ausführlicher mit den technischen Aspekten der Migration auf Ressourcen- und Featureebene, um zu vermitteln, wie die Plattform Ressourcen vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell migriert. Weitere Informationen finden Sie im Artikel zur Dienstankündigung: [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md).

## Ausführliche Anleitung zur Migration auf Ressourcen- und Featureebene

Die folgende Tabelle zeigt die Darstellung der Ressourcen im klassischen Bereitstellungsmodell und in Resource Manager. In dieser Tabelle nicht enthaltene Features und Ressourcen werden derzeit nicht unterstützt.

| Klassische Darstellung | Resource Manager-Darstellung | Ausführliche Hinweise | | |
|--------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|---|
| Name des Clouddiensts | DNS-Name | Während der Migration wird eine neue Ressourcengruppe für jeden Clouddienst mit dem Benennungsmuster `<cloudservicename>-migrated` erstellt. Diese Ressourcengruppe enthält alle Ressourcen. Der Name des Clouddiensts wird als DNS-Name verwendet, der der öffentlichen IP-Adresse zugeordnet ist. | | |
| Virtueller Computer | Virtueller Computer | VM-spezifische Eigenschaften werden unverändert migriert. Beachten Sie, dass bestimmte osProfile-Informationen wie etwa der Computername im klassischen Bereitstellungsmodell nicht gespeichert werden und daher nach der Migration leer bleiben. | | |
| An virtuellen Computer angefügte Datenträgerressourcen | An virtuellen Computer angefügte implizite Datenträger | Datenträger werden im Resource Manager-Bereitstellungsmodell nicht als Ressourcen oberster Ebene modelliert. Sie werden als implizite Datenträger unter dem virtuellen Computer migriert. Zu diesem Zeitpunkt werden nur Datenträger unterstützt, die mit einem virtuellen Computer verbunden sind. Resource Manager-VMs können nun klassische Speicherkonten verwenden, um die Migration zu ermöglichen. Dadurch lassen sich die Datenträger problemlos und ganz ohne Updates zum Resource Manager-Modell migrieren. | | |
| VM-Erweiterungen | VM-Erweiterungen | Mit Ausnahme von XML-Erweiterungen werden alle Ressourcenerweiterungen aus dem klassischen Bereitstellungsmodell migriert. | | |
| Zertifikate für virtuelle Computer | Zertifikate in Azure Key Vault | Wenn ein Clouddienst Dienstzertifikate enthält, erstellt die Plattform jeweils eine neue Azure Key Vault-Instanz pro Clouddienst und verschiebt die Zertifikate in den Schlüsseltresor. Die VMs werden aktualisiert, um auf die Zertifikate aus dem Schlüsseltresor zu verweisen. | | |
| WinRM-Konfiguration | WinRM-Konfiguration unter osProfile | Die Konfiguration der Windows-Remoteverwaltung wird im Rahmen der Migration unverändert verschoben. | | |
| „Availability-set“-Eigenschaft | „Availability-set“-Ressource | Im klassischen Bereitstellungsmodell handelte es sich bei der Angabe der Verfügbarkeitsgruppe um eine Eigenschaft auf dem virtuellen Computer. Bei der Migration werden Verfügbarkeitsgruppen zu einer Ressource der obersten Ebene. Folgende Konfigurationen werden nicht unterstützt: mehrere Verfügbarkeitsgruppen pro Clouddienst sowie eine Kombination aus mindestens einer Verfügbarkeitsgruppe und virtuellen Computern, die keiner Verfügbarkeitsgruppe in einem Clouddienst angehören. | | |
| Netzwerkkonfiguration auf einem virtuellen Computer | Primäre Netzwerkschnittstelle | Die Netzwerkkonfiguration auf einem virtuellen Computer wird nach der Migration als primäre Netzwerkschnittstellenressource dargestellt. Bei virtuellen Computern, die sich nicht in einem virtuellen Netzwerk befinden, ändert sich im Zuge der Migration die interne IP-Adresse. | | |
| Mehrere Netzwerkschnittstellen auf einem virtuellen Computer | Netzwerkschnittstellen | Sind einem virtuellen Computer mehrere Netzwerkschnittstellen zugeordnet, wird jede Netzwerkschnittstelle im Rahmen der Migration zu einer Ressource der obersten Ebene im Resource Manager-Bereitstellungsmodell (einschließlich aller Eigenschaften). | | |
| Endpunktsatz mit Lastenausgleich | Load Balancer | Im klassischen Bereitstellungsmodell wurde von der Plattform für jeden Clouddienst ein impliziter Load Balancer zugewiesen. Im Zuge der Migration wird eine neue Load Balancer-Ressource erstellt, und der Lastenausgleichs-Endpunktsatz wird zu Load Balancer-Regeln. | | |
| Eingehende NAT-Regeln | Eingehende NAT-Regeln | Für den virtuellen Computer definierte Eingabeendpunkte werden bei der Migration in eingehende NAT-Regeln (Netzwerkadressenübersetzung) für den Load Balancer umgewandelt. | | |
| VIP-Adresse | Öffentliche IP-Adresse mit DNS-Name | Die virtuelle IP-Adresse wird zur öffentlichen IP-Adresse, die dem Load Balancer zugeordnet ist. | | |
| Virtuelles Netzwerk | Virtuelles Netzwerk | Das virtuelle Netzwerk wird mit allen zugehörigen Eigenschaften in das Resource Manager-Bereitstellungsmodell migriert. Eine neue Ressourcengruppe namens `-migrated` wird erstellt. Beachten Sie die [nicht unterstützten Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). | | |
| Reservierte IP-Adressen | Öffentliche IP-Adresse mit statischer Zuordnungsmethode | Dem Load Balancer zugeordnete reservierte IP-Adressen werden zusammen mit dem Clouddienst oder virtuellen Computer migriert. Die Migration nicht zugeordneter reservierter IP-Adressen wird derzeit nicht unterstützt. | | |
| Öffentliche IP-Adresse pro virtuellem Computer | Öffentliche IP-Adresse mit dynamischer Zuordnungsmethode | Die dem virtuellen Computer zugeordnete öffentliche IP-Adresse wird in eine öffentliche IP-Adressressource mit statischer Zuordnungsmethode konvertiert. | | |
| NSGs | NSGs | Einem Subnetz zugeordnete Netzwerksicherheitsgruppen (Network Security Groups, NSGs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Beachten Sie, dass die NSG aus dem klassischen Bereitstellungsmodell bei der Migration nicht entfernt wird. Während der Migration werden jedoch Vorgänge auf Verwaltungsebene für die NSG blockiert. | | |
| DNS-Server | DNS-Server | DNS-Server, die einem virtuellen Netzwerk oder dem virtuellen Computer zugeordnet sind, werden im Rahmen der entsprechenden Ressourcenmigration zusammen mit allen Eigenschaften migriert. | | |
| UDRs | UDRs | Einem Subnetz zugeordnete benutzerdefinierte Routen (User-Defined Routes, UDRs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Beachten Sie, dass die UDR aus dem klassischen Bereitstellungsmodell bei der Migration nicht entfernt wird. Während der Migration werden jedoch Vorgänge auf Verwaltungsebene für die UDR blockiert. | | |
| IP-Weiterleitungseigenschaft für die Netzwerkkonfiguration eines virtuellen Computers | IP-Weiterleitungseigenschaft für die Netzwerkschnittstellenkarte | Die IP-Weiterleitungseigenschaft für einen virtuellen Computer wird im Zuge der Migration in eine Eigenschaft der Netzwerkschnittstelle konvertiert. | | |
| Load Balancer mit mehreren IP-Adressen | Load Balancer mit mehreren öffentlichen IP-Ressourcen | Sämtliche öffentliche, dem Load Balancer zugeordnete IP-Adressen werden in eine öffentliche IP-Ressource konvertiert und dem Load Balancer nach der Migration zugeordnet. | | |
| Interne DNS-Namen für den virtuellen Computer | Interne DNS-Namen für die Netzwerkschnittstellenkarte | Während der Migration werden die internen DNS-Suffixe für die virtuellen Computer in eine schreibgeschützte Eigenschaft mit dem Namen „InternalDomainNameSuffix“ auf der NIC migriert. Das Suffix bleibt nach der Migration unverändert, und die virtuelle Computerlösung sollte weiterhin wie zuvor funktionieren. | | |

## Vorgehensweise für eine einfache Migration

Die folgenden Beispielscreenshots zeigen einen Clouddienst mit einem (nicht in einem virtuellen Netzwerk enthaltenen) virtuellen Computer nach der Vorbereitungsphase.

![Screenshot der klassischen Darstellung nach der Vorbereitung](./media/virtual-machines-windows-migration-classic-resource-manager/classic-migration-prepare-portal.png) ![Screenshot der Resource Manager-Darstellung nach der Vorbereitung](./media/virtual-machines-windows-migration-classic-resource-manager/resourcemanager-migration-prepare-portal.png)

## Nächste Schritte

Nachdem Sie nun eine Vorstellung von der Migration klassischer IaaS-Ressourcen zu Resource Manager haben, können Sie damit beginnen, die Ressourcen zu migrieren.

- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community](virtual-machines-windows-migration-scripts.md)

<!---HONumber=AcomDC_0824_2016-->