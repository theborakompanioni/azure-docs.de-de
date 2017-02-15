---
title: "Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager | Microsoft Docs"
description: "Dieser Artikel bietet ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ee40d32-a5e8-42a2-97d0-3232fd3cbb98
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 63d8270811797db5283870e4b4d3482271b97793


---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager
Wir betrachten nun genau die Migration vom klassischen Azure-Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell. Ressourcen betrachten wir auf Ressourcen- und Featureebene, damit Sie verstehen, wie die Azure-Plattform Ressourcen zwischen den beiden Bereitstellungsmodellen migriert. Weitere Informationen finden Sie im Artikel zur Dienstankündigung: [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md).

## <a name="detailed-guidance-on-migration"></a>Ausführliche Anleitung zur Migration
Die folgende Tabelle zeigt die Darstellung der Ressourcen im klassischen Bereitstellungsmodell und in Resource Manager. Andere Features und Ressourcen werden derzeit nicht unterstützt.

| Klassische Darstellung | Resource Manager-Darstellung | Ausführliche Hinweise |
| --- | --- | --- |
| Name des Clouddiensts |DNS-Name |Während der Migration wird für jeden Clouddienst eine neue Ressourcengruppe mit dem Benennungsmuster `<cloudservicename>-migrated`erstellt. Diese Ressourcengruppe enthält alle Ihre Ressourcen. Der Name des Clouddiensts wird als DNS-Name verwendet, der der öffentlichen IP-Adresse zugeordnet ist. |
| Virtueller Computer |Virtueller Computer |VM-spezifische Eigenschaften werden unverändert migriert. Bestimmte osProfile-Informationen, wie etwa der Computername, werden im klassischen Bereitstellungsmodell nicht gespeichert und bleiben daher nach der Migration leer. |
| An virtuellen Computer angefügte Datenträgerressourcen |An virtuellen Computer angefügte implizite Datenträger |Datenträger werden im Resource Manager-Bereitstellungsmodell nicht als Ressourcen oberster Ebene modelliert. Sie werden als implizite Datenträger unter dem virtuellen Computer migriert. Derzeit werden nur Datenträger unterstützt, die mit einem virtuellen Computer verbunden sind. Resource Manager-VMs können nun klassische Speicherkonten verwenden, sodass die Datenträger auf einfache Weise ohne jegliche Updates migriert werden können. |
| VM-Erweiterungen |VM-Erweiterungen |Mit Ausnahme von XML-Erweiterungen werden alle Ressourcenerweiterungen aus dem klassischen Bereitstellungsmodell migriert. |
| Zertifikate für virtuelle Computer |Zertifikate in Azure Key Vault |Wenn ein Clouddienst Dienstzertifikate enthält, wird eine neue Azure Key Vault-Instanz pro Clouddienst erstellt, und die Zertifikate werden in den Schlüsseltresor verschoben. Die VMs werden aktualisiert, um auf die Zertifikate aus dem Schlüsseltresor zu verweisen. <br><br> **HINWEIS**: Löschen Sie den Schlüsseltresor nicht, da dies dazu führen kann, dass die VM in einen fehlerhaften Zustand wechselt. Wir arbeiten an Verbesserungen im Back-End, damit Schlüsseltresore sicher gelöscht oder gemeinsam mit der VM in ein neues Abonnement verschoben werden können. |
| WinRM-Konfiguration |WinRM-Konfiguration unter osProfile |Die Konfiguration der Windows-Remoteverwaltung wird im Rahmen der Migration unverändert verschoben. |
| „Availability-set“-Eigenschaft |„Availability-set“-Ressource |Im klassischen Bereitstellungsmodell handelte es sich bei der Angabe der Verfügbarkeitsgruppe um eine Eigenschaft auf dem virtuellen Computer. Bei der Migration werden Verfügbarkeitsgruppen zu einer Ressource der obersten Ebene. Folgende Konfigurationen werden nicht unterstützt: mehrere Verfügbarkeitsgruppen pro Clouddienst sowie eine Kombination aus mindestens einer Verfügbarkeitsgruppe und virtuellen Computern, die keiner Verfügbarkeitsgruppe in einem Clouddienst angehören. |
| Netzwerkkonfiguration auf einem virtuellen Computer |Primäre Netzwerkschnittstelle |Die Netzwerkkonfiguration auf einem virtuellen Computer wird nach der Migration als primäre Netzwerkschnittstellenressource dargestellt. Bei virtuellen Computern, die sich nicht in einem virtuellen Netzwerk befinden, ändert sich im Zuge der Migration die interne IP-Adresse. |
| Mehrere Netzwerkschnittstellen auf einem virtuellen Computer |Netzwerkschnittstellen |Sind einem virtuellen Computer mehrere Netzwerkschnittstellen zugeordnet, wird jede Netzwerkschnittstelle im Rahmen der Migration zu einer Ressource der obersten Ebene im Resource Manager-Bereitstellungsmodell (einschließlich aller Eigenschaften). |
| Endpunktsatz mit Lastenausgleich |Load Balancer |Im klassischen Bereitstellungsmodell wurde von der Plattform für jeden Clouddienst ein impliziter Load Balancer zugewiesen. Im Zuge der Migration wird eine neue Load Balancer-Ressource erstellt, und der Lastenausgleichs-Endpunktsatz wird zu Load Balancer-Regeln. |
| Eingehende NAT-Regeln |Eingehende NAT-Regeln |Auf dem virtuellen Computer definierte Eingabeendpunkte werden bei der Migration in Netzwerkadressenübersetzungs-Eingangsregeln für den Load Balancer umgewandelt. |
| VIP-Adresse |Öffentliche IP-Adresse mit DNS-Name |Die virtuelle IP-Adresse wird zur öffentlichen IP-Adresse, die dem Load Balancer zugeordnet ist. |
| Virtuelles Netzwerk |Virtuelles Netzwerk |Das virtuelle Netzwerk wird mit allen zugehörigen Eigenschaften in das Resource Manager-Bereitstellungsmodell migriert. Eine neue Ressourcengruppe namens `-migrated`wird erstellt. Es gibt [nicht unterstützte Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). |
| Reservierte IP-Adressen |Öffentliche IP-Adresse mit statischer Zuordnungsmethode |Dem Load Balancer zugeordnete reservierte IP-Adressen werden zusammen mit dem Clouddienst oder virtuellen Computer migriert. Die Migration nicht zugeordneter reservierter IP-Adressen wird derzeit nicht unterstützt. |
| Öffentliche IP-Adresse pro virtuellem Computer |Öffentliche IP-Adresse mit dynamischer Zuordnungsmethode |Die dem virtuellen Computer zugeordnete öffentliche IP-Adresse wird in eine öffentliche IP-Adressressource mit statischer Zuordnungsmethode konvertiert. |
| NSGs |NSGs |Einem Subnetz zugeordnete Netzwerksicherheitsgruppen (Network Security Groups, NSGs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Die NSG aus dem klassischen Bereitstellungsmodell wird bei der Migration nicht entfernt. Während der Migration werden jedoch Vorgänge auf Verwaltungsebene für die NSG blockiert. |
| DNS-Server |DNS-Server |DNS-Server, die einem virtuellen Netzwerk oder dem virtuellen Computer zugeordnet sind, werden im Rahmen der entsprechenden Ressourcenmigration zusammen mit allen Eigenschaften migriert. |
| UDRs |UDRs |Einem Subnetz zugeordnete benutzerdefinierte Routen (User-Defined Routes, UDRs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Die UDR aus dem klassischen Bereitstellungsmodell wird bei der Migration nicht entfernt. Während der Migration werden Vorgänge auf Verwaltungsebene für die UDR blockiert. |
| IP-Weiterleitungseigenschaft für die Netzwerkkonfiguration eines virtuellen Computers |IP-Weiterleitungseigenschaft für die Netzwerkschnittstellenkarte |Die IP-Weiterleitungseigenschaft für einen virtuellen Computer wird im Zuge der Migration in eine Eigenschaft der Netzwerkschnittstelle konvertiert. |
| Load Balancer mit mehreren IP-Adressen |Load Balancer mit mehreren öffentlichen IP-Ressourcen |Sämtliche öffentliche, dem Load Balancer zugeordnete IP-Adressen werden in eine öffentliche IP-Ressource konvertiert und dem Load Balancer nach der Migration zugeordnet. |
| Interne DNS-Namen für den virtuellen Computer |Interne DNS-Namen für die Netzwerkschnittstellenkarte |Während der Migration werden die internen DNS-Suffixe für die virtuellen Computer in eine schreibgeschützte Eigenschaft mit dem Namen InternalDomainNameSuffix auf der NIC migriert. Das Suffix bleibt nach der Migration unverändert, und die virtuelle Computerlösung sollte weiterhin wie zuvor funktionieren. |
| Gateway des virtuellen Netzwerks |Gateway des virtuellen Netzwerks |Eigenschaften des Gateways des virtuellen Netzwerks werden unverändert migriert. Die dem Gateway zugeordnete VIP bleibt ebenfalls unverändert. |
| Standort des lokalen Netzwerks |Gateway des lokalen Netzwerks |Die Eigenschaften des lokalen Netzwerkstandorts werden unverändert in eine neue Ressource namens „Lokales Netzwerkgateway“ migriert. Sie repräsentiert lokale Adresspräfixe und die Remotegateway-IP. |
| Verbindungsverweise |Verbindung |Konnektivitätsverweise zwischen dem Gateway- und dem lokalen Netzwerkstandort in der Netzwerkkonfiguration werden nach der Migration im Resource Manager durch eine neu erstellte Ressource namens „Verbindung“ repräsentiert. Alle Eigenschaften von Konnektivitätsverweisen in Netzwerkkonfigurationsdateien werden unverändert in die neu erstellte Verbindungsressource kopiert. VNET-zu-VNET-Konnektivität im klassischen Modell wird durch das Erstellen von zwei IPSec-Tunneln zu lokalen Netzwerkstandorten erreicht, die VNETs repräsentieren. Im Resource Manager-Modell erfolgt eine Transformation in den Verbindungstyp „Vnet2Vnet“, ohne dass lokale Netzwerkgateways erforderlich sind. |

## <a name="illustration-of-a-simple-migration-walkthrough"></a>Vorgehensweise für eine einfache Migration
Der folgende Beispielscreenshot zeigt einen vorhandenen Clouddienst mit einem (nicht in einem virtuellen Netzwerk enthaltenen) virtuellen Computer nach der Vorbereitungsphase:

![Darstellung des klassischen Bereitstellungsmodells nach der Vorbereitung](./media/virtual-machines-windows-migration-classic-resource-manager/classic-migration-prepare-portal.png)

Der folgende, nach Abschluss des Migrationsvorgangs erstellte Screenshot zeigt, dass die neuen Ressourcen in einer neuen Ressourcengruppe erstellt wurden: ![Resource Manager-Darstellung nach der Vorbereitung](./media/virtual-machines-windows-migration-classic-resource-manager/resourcemanager-migration-prepare-portal.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun eine Vorstellung von der Migration klassischer IaaS-Ressourcen zu Resource Manager haben, können Sie damit beginnen, die Ressourcen zu migrieren.

* [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
* [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)
* [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md)
* [Klonen eines klassischen virtuellen Computers nach Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community](virtual-machines-windows-migration-scripts.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Überprüfen der häufigsten Fehler bei der Migration](virtual-machines-migration-errors.md)




<!--HONumber=Nov16_HO3-->


