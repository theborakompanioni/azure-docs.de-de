<properties
   pageTitle="Resource Manager- und klassische Bereitstellung | Microsoft Azure"
   description="Beschreibt die Unterschiede zwischen dem Ressourcen-Manager-Bereitstellungsmodell und dem klassischen Bereitstellungsmodell (auch als ";Dienstverwaltungsmodell"; bekannt)."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/13/2016"
   ms.author="tomfitz"/>

# Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen

In diesem Thema erhalten Sie Informationen über das Azure Resource Manager-Bereitstellungsmodell und das klassische Bereitstellungsmodell sowie zum Status Ihrer Ressourcen. Zudem erfahren Sie, warum Ihre Ressourcen jeweils mit einem der beiden Modelle bereitgestellt wurden. Das Resource Manager-Bereitstellungsmodell weist einige wichtige Unterschiede zum klassischen Bereitstellungsmodell auf, und die beiden Modelle sind nicht vollständig miteinander kompatibel. Um die Bereitstellung und Verwaltung der Ressourcen zu vereinfachen, empfiehlt Microsoft die Verwendung des Ressourcen-Managers für neue Ressourcen und, sofern möglich, die erneute Bereitstellung vorhandener Ressourcen über den Ressourcen-Manager.

Bei den meisten Ressourcen können Sie problemlos zum Resource Manager-Modell wechseln. Aufgrund der Architekturunterschiede zwischen den Modellen stellen einige Ressourcenanbieter jedoch zwei Versionen ihrer Ressource bereit: eine für das klassische, eine für das Ressourcen-Manager-Modell. Für folgende Ressourcenanbieter bestehen Unterschiede zwischen den beiden Modellen:

- **Computeressourcen** – unterstützen Instanzen von virtuellen Computern und optionale Verfügbarkeitsgruppen.
- **Speicherressourcen** – unterstützen erforderliche Speicherkonten, die VHDs für virtuelle Computer speichern, einschließlich der Datenträger für Betriebssysteme und zusätzlicher Datenträger.
- **Netzwerkressourcen** – unterstützen erforderliche NICs, IP-Adressen virtueller Computer und Subnetze innerhalb von virtuellen Netzwerken sowie optionale Load Balancer, deren IP-Adressen und Netzwerksicherheitsgruppen.

Für diese Ressourcentypen müssen Sie darauf achten, welche Version Sie verwenden, da sich die unterstützten Vorgänge unterscheiden. Wenn Sie bereit sind, Ihre Ressourcen aus der klassischen Bereitstellung in die Resource Manager-Bereitstellung zu migrieren, lesen Sie [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md).

Damit Sie besser nachvollziehen können, welches Modell für die Bereitstellung Ihrer Ressourcen verwendet wurde, werden die beiden Modelle im Folgenden genauer erläutert.

## Merkmale des Ressourcen-Managers

Ressourcen, die über den Ressourcen-Manager erstellt wurden, weisen folgende Merkmale auf:

- Erstellt mit einer der beiden folgenden Methoden:

  - Das [Azure-Portal](https://portal.azure.com/)

        ![Azure portal](./media/resource-manager-deployment-model/preview-portal.png)

        Für Compute-, Speicher- und Netzwerkressourcen haben Sie die Möglichkeit, entweder den Resource Manager oder die klassische Bereitstellung zu nutzen. Wählen Sie **Ressourcen-Manager**.

        ![Resource Manager deployment](./media/resource-manager-deployment-model/select-resource-manager.png)

  - Verwenden Sie in Azure PowerShell die Befehle des Resource Manager-Modells. Diese Befehle haben das Format *Verb-AzureRmNomen*, wie unten dargestellt.

            Get-AzureRmResourceGroupDeployment

  - [Azure-Ressourcen-Manager-REST-API](https://msdn.microsoft.com/library/azure/dn790568.aspx) für REST-Vorgänge.
  - Befehle der Azure-Befehlszeilenschnittstelle, die im Modus **arm** ausgeführt werden.

            azure config mode arm

- Der Name des Ressourcentyps enthält nicht das Wort **(klassisch)**. Die folgende Abbildung zeigt den Typ **Speicherkonto**.

    ![Web-App](./media/resource-manager-deployment-model/resource-manager-type.png)

Die in der folgenden Abbildung dargestellte Anwendung zeigt über den Ressourcen-Manager bereitgestellte Ressourcen in einer einfachen Ressourcengruppe.

  ![](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Außerdem gibt es Beziehungen zwischen den Ressourcen innerhalb der Ressourcenanbieter:

- Ein virtueller Computer hängt von einem bestimmten Speicherkonto ab, das im SRP definiert ist, um die Datenträger im Blobspeicher zu speichern (erforderlich).
- Ein virtueller Computer verweist auf eine bestimmte NIC, die im NRP definiert ist (erforderlich), und eine Verfügbarkeitsgruppe, die im CRP definiert ist (optional).
- Eine NIC verweist auf die dem virtuellen Computer zugewiesene IP-Adresse (erforderlich), das Subnetz des virtuellen Netzwerks für den virtuellen Computer (erforderlich) und eine Netzwerksicherheitsgruppe (optional).
- Ein Subnetz innerhalb eines virtuellen Netzwerks verweist auf eine Netzwerksicherheitsgruppe (optional).
- Eine Lastenausgleichsinstanz verweist auf den Back-End-Pool von IP-Adressen, die die NIC eines virtuellen Computers enthalten (optional) und auf eine öffentliche oder private IP-Adresse für den Lastenausgleich (optional).

## Merkmale der klassischen Bereitstellung

Das klassische Bereitstellungsmodell ist Ihnen möglicherweise unter den Bezeichnungen "Service-Management-Modell" oder "Dienstverwaltungsmodell" bekannt.

In der Azure-Dienstverwaltung werden die Compute-, Speicher- oder Netzwerkressourcen für das Hosten von virtuellen Computern wie folgt bereitgestellt:

- Ein erforderlicher Clouddienst, der als Container für das Hosten virtueller Computer (Compute) fungiert. Virtuelle Computer werden automatisch mit einer Netzwerkschnittstellenkarte (Network Interface Card, NIC) bereitgestellt, und ihnen wird von Azure eine IP-Adresse zugewiesen. Darüber hinaus enthält der Clouddienst eine externe Lastenausgleichsinstanz, eine öffentliche IP-Adresse und Standardendpunkte, um Remotedesktop- und Remote-PowerShell-Datenverkehr für die Windows-basierten virtuellen Computer und Secure Shell-Datenverkehr (SSH) für Linux-basierte virtuelle Computer zu ermöglichen.
- Ein erforderliches Speicherkonto, in dem die VHDs für einen virtuellen Computer gespeichert werden, einschließlich des Betriebssystems sowie temporärer und zusätzlicher Datenträger (Speicher).
- Ein optionales virtuelles Netzwerk, das als zusätzlicher Container fungiert, in dem Sie eine Subnetzstruktur erstellen und das Subnetz benennen können, in dem sich der virtuelle Computer befindet (Netzwerk).

Ressourcen, die im klassischen Bereitstellungsmodell erstellt wurden, weisen folgende Merkmale auf:

- Erstellt mit einer der beiden folgenden Methoden:

  - [Klassisches Portal](https://manage.windowsazure.com)

        ![Classic portal](./media/resource-manager-deployment-model/azure-portal.png)

        Or, the Azure portal and you specify **Classic** deployment (for Compute, Storage, and Networking).

        ![Classic deployment](./media/resource-manager-deployment-model/select-classic.png)

  - Verwenden Sie in Azure PowerShell die Dienstverwaltungsversion der Befehle. Diese Befehlsnamen haben das Format *Verb-AzureNomen*, wie unten dargestellt.

            Get-AzureDeployment

  - [Dienstverwaltungs-REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx) für REST-Vorgänge.
  - Befehle der Azure-Befehlszeilenschnittstelle, die im Modus **asm** oder im Standardmodus ausgeführt werden.
- Der Name des Ressourcentyps enthält das Wort **(klassisch)**. Die folgende Abbildung zeigt den Typ **Speicherkonto (klassisch)**.

    ![Klassischer Typ](./media/resource-manager-deployment-model/classic-type.png)

Sie können weiterhin das Azure-Portal zur Verwaltung von Ressourcen verwenden, die über die klassische Bereitstellung erstellt wurden.

Im Folgenden sind die Komponenten und deren Beziehungen für die Azure-Dienstverwaltung dargestellt.

  ![](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

## Vorteile der Verwendung von Ressourcen-Manager und Ressourcengruppen

Mit dem Ressourcen-Manager wurde das Konzept der Ressourcengruppen eingeführt. Jede über den Ressourcen-Manager erstellte Ressource befindet sich in einer Ressourcengruppe. Das Ressourcen-Manager-Bereitstellungsmodell bietet verschiedene Vorteile:

- Sie müssen die Dienste für Ihre Lösung nicht mehr einzeln bearbeiten, sondern können sie als Gruppe bereitstellen, verwalten und überwachen.
- Sie können die Anwendung während des gesamten App-Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.
- Sie können deklarative Vorlagen verwenden, um Ihre Bereitstellung zu definieren.
- Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.
- Sie können die Zugriffssteuerung auf alle Ressourcen in der Ressourcengruppe anwenden, da die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) standardmäßig in die Verwaltungsplattform integriert ist.
- Sie können Tags auf Ressourcen anwenden, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren.


Vor der Einführung des Ressourcen-Managers befanden sich die über die klassische Bereitstellung erstellten Ressourcen nicht in Ressourcengruppen. Nach Einführung des Ressourcen-Managers wurden alle Ressourcen nachträglich zu Standardressourcengruppen hinzugefügt. Wenn Sie jetzt eine Ressource über die klassische Bereitstellung erstellen, wird diese automatisch in einer standardmäßigen Ressourcengruppe für diesen Dienst erstellt, auch wenn Sie während der Bereitstellung diese Ressourcengruppe nicht angegeben haben. Jedoch bedeutet die bloße Tatsache, dass eine Ressource sich in einer Ressourcengruppe befindet, noch nicht, dass diese Ressource in das Ressourcen-Manager-Modell konvertiert wurde. Für Virtual Machines, Storage und Virtual Networks gilt: Wenn die Ressource über die klassische Bereitstellung erstellt wurde, müssen Sie diese weiterhin mit klassischen Vorgängen ausführen.

Sie können Ressourcen in eine andere Ressourcengruppe verschieben und einer vorhandenen Ressourcengruppe neue Ressourcen hinzufügen. Eine Ressourcengruppe kann daher eine Kombination aus Ressourcen enthalten, die mit dem Ressourcen-Manager und über die klassische Bereitstellung erstellt wurden. Diese Ressourcenkombination kann zu unerwarteten Ergebnissen führen, da die Ressourcen nicht die gleichen Vorgänge unterstützen.

Durch Verwendung von deklarativen Vorlagen können Sie Ihre Bereitstellungsskripts vereinfachen. Anstatt zu versuchen, vorhandene Dienstverwaltungsskripts in Skripts für den Ressourcen-Manager zu konvertieren, empfiehlt es sich, die Bereitstellungsstrategie zu überarbeiten und Ihre Infrastruktur und Konfiguration in einer Vorlage zu definieren.

## Verwenden von Tags

Mit Tags können Sie Ihre Ressourcen logisch organisieren. Nur Ressourcen, die mit dem Ressourcen-Manager erstellt wurden, unterstützen Tags. Sie können keine Tags auf klassische Ressourcen anwenden.

Weitere Informationen zur Verwendung von Tags im Ressourcen-Manager finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).

## Unterstützte Vorgänge für die Bereitstellungsmodelle

Ressourcen, die Sie im klassischen Bereitstellungsmodell erstellen, unterstützen keine Ressourcen-Manager-Vorgänge. In einigen Fällen können Sie mithilfe eines Ressourcen-Manager-Befehls Informationen zu einer mit der klassischen Bereitstellung erstellten Ressource abrufen oder administrative Aufgaben ausführen, z. B. eine klassische Ressource in eine andere Ressourcengruppe verschieben. Dies sollte jedoch nicht zu der Annahme verleiten, dass der entsprechende Ressourcentyp Ressourcen-Manager-Vorgänge unterstützt. Nehmen Sie beispielsweise an, Sie verfügen über eine Ressourcengruppe, die einen mit dem klassischen Bereitstellungsmodell erstellten virtuellen Computer umfasst. Wenn Sie folgenden PowerShell-Befehl ausführen:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

wird der virtuelle Computer zurückgegeben:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Das Cmdlet **Get-AzureRmVM** gibt jedoch nur über Resource Manager bereitgestellte virtuelle Computer zurück. Mit dem folgenden Befehl wird der über das klassische Bereitstellungsmodell erstellte virtuelle Computer nicht zurückgegeben.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Im Allgemeinen sollten Sie davon ausgehen, dass Ressourcen, die über die klassische Bereitstellung erstellt wurden, mit Ressourcen-Manager-Befehlen funktionieren.

Bei der Arbeit mit Ressourcen, die mit dem Ressourcen-Manager erstellt wurden, sollten Sie Ressourcen-Manager-Vorgänge verwenden und nicht zu Dienstverwaltungsvorgängen zurückkehren.

## Überlegungen zu virtuellen Computern

Bei der Arbeit mit virtuellen Computern sind einige wichtige Aspekte zu berücksichtigen.

- Virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht in ein virtuelles Netzwerk eingebunden werden, das mit dem Ressourcen-Manager bereitgestellt wurde.
- Virtuelle Computer, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurden, müssen in ein virtuelles Netzwerk eingebunden werden.
- Virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, müssen nicht in ein virtuelles Netzwerk eingebunden werden.

Wenn Sie bereit sind, Ihre Ressourcen aus der klassischen Bereitstellung in die Resource Manager-Bereitstellung zu migrieren, lesen Sie [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md).

Detaillierte Informationen zum Verschieben von Compute-, Speicher- und Netzwerkressourcen finden Sie unter [Azure-Compute-, Netzwerk- und Speicheranbieter unter dem Azure-Ressourcen-Manager](./virtual-machines/virtual-machines-windows-compare-deployment-models.md).

Informationen zum Verbinden virtueller Netzwerke aus verschiedenen Bereitstellungsmodellen finden Sie unter [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](./virtual-network/virtual-networks-arm-asm-s2s.md).

## Nächste Schritte

- Eine exemplarische Vorgehensweise zum Erstellen einer Vorlage, mit der ein virtueller Computer, ein Speicherkonto und ein virtuelles Netzwerk definiert werden, finden Sie unter [Resource Manager-Vorlage – exemplarische Vorgehensweise](resource-manager-template-walkthrough.md).
- Informationen zur Struktur von Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
- Die Befehle zum Bereitstellen einer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

<!---HONumber=AcomDC_0518_2016-->