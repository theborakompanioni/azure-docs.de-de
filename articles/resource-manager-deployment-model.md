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
   ms.date="07/29/2016"
   ms.author="tomfitz"/>

# Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen

In diesem Thema erhalten Sie Informationen über das Azure Resource Manager-Bereitstellungsmodell und das klassische Bereitstellungsmodell sowie zum Status Ihrer Ressourcen. Zudem erfahren Sie, warum Ihre Ressourcen jeweils mit einem der beiden Modelle bereitgestellt wurden. Das Resource Manager-Bereitstellungsmodell weist einige wichtige Unterschiede zum klassischen Bereitstellungsmodell auf, und die beiden Modelle sind nicht vollständig miteinander kompatibel. Um die Bereitstellung und Verwaltung der Ressourcen zu vereinfachen, empfiehlt Microsoft die Verwendung des Ressourcen-Managers für neue Ressourcen und, sofern möglich, die erneute Bereitstellung vorhandener Ressourcen über den Ressourcen-Manager.

Falls der Resource Manager völlig neu für Sie ist, ist es ratsam, zuerst die Terminologiedefinitionen unter [Übersicht über Azure Resource Manager](resource-group-overview.md) durchzulesen.

## Verlauf der Bereitstellungsmodelle

In Azure war ursprünglich nur das klassische Bereitstellungsmodell verfügbar. In diesem Modell war jede Ressource unabhängig von den anderen vorhanden. Zusammengehörige Ressourcen konnten nicht gruppiert werden. Stattdessen mussten Sie manuell nachverfolgen, aus welchen Ressourcen die Lösung oder Anwendung bestand, und daran denken, sie mit einem koordinierten Ansatz zu verwalten. Zum Bereitstellen einer Lösung mussten Sie entweder jede Ressource einzeln über das klassische Portal erstellen oder ein Skript erstellen, mit dem alle Ressourcen in der richtigen Reihenfolge bereitgestellt wurden. Beim Löschen einer Lösung mussten Sie jede Ressource einzeln löschen. Es war nicht einfach möglich, Richtlinien für die Zugriffssteuerung für zusammengehörige Ressourcen anzuwenden und zu aktualisieren. Außerdem gab es keinen Weg, Tags auf Ressourcen anzuwenden, um diese zur Unterstützung der Überwachung Ihrer Ressourcen und der Abrechnungsverwaltung mit Begriffen zu versehen.

Im Jahr 2014 wurden Resource Manager und das Konzept der Ressourcengruppen von Azure eingeführt. Eine Ressourcengruppe ist ein Container für Ressourcen, die über einen gemeinsamen Lebenszyklus verfügen. Das Resource Manager-Bereitstellungsmodell bietet verschiedene Vorteile:

- Sie müssen die Dienste für Ihre Lösung nicht mehr einzeln bearbeiten, sondern können sie als Gruppe bereitstellen, verwalten und überwachen.
- Sie können die Lösung während ihres Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.
- Sie können die Zugriffssteuerung auf alle Ressourcen der Ressourcengruppe anwenden. Diese Richtlinien werden automatisch angewendet, wenn der Ressourcengruppe neue Ressourcen hinzugefügt werden.
- Sie können Tags auf Ressourcen anwenden, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren.
- Sie können JavaScript Object Notation (JSON) verwenden, um die Infrastruktur für Ihre Lösung zu definieren. Die JSON-Datei wird als Resource Manager-Vorlage bezeichnet.
- Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.

Nach Einführung des Ressourcen-Managers wurden alle Ressourcen nachträglich zu Standardressourcengruppen hinzugefügt. Wenn Sie jetzt eine Ressource über die klassische Bereitstellung erstellen, wird diese automatisch in einer standardmäßigen Ressourcengruppe für diesen Dienst erstellt, auch wenn Sie während der Bereitstellung diese Ressourcengruppe nicht angegeben haben. Jedoch bedeutet die bloße Tatsache, dass eine Ressource sich in einer Ressourcengruppe befindet, noch nicht, dass diese Ressource in das Ressourcen-Manager-Modell konvertiert wurde. Im nächsten Abschnitt wird beschrieben, wie die einzelnen Dienste die beiden Bereitstellungsmodelle behandeln.

## Grundlagen der Unterstützung von Modellen 

Bei der Entscheidung, welches Bereitstellungsmodell Sie für Ihre Ressourcen verwenden, sollten drei Szenarien berücksichtigt werden:

1. Der Dienst unterstützt Resource Manager und stellt nur einen einzelnen Typ bereit.
2. Der Dienst unterstützt Resource Manager, stellt aber zwei Typen bereit: einen für Resource Manager und einen für das klassische Modell. Dies gilt nur für virtuelle Computer, Speicherkonten und virtuelle Netzwerke.
3. Der Dienst weist keine Unterstützung von Resource Manager auf.

Informationen zur Ermittlung, ob ein Dienst Resource Manager unterstützt, finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für den Ressourcen-Manager](resource-manager-supported-services.md).

Wenn der gewünschte Dienst Resource Manager nicht unterstützt, müssen Sie weiterhin die klassische Bereitstellung verwenden.

Falls der Dienst Resource Manager unterstützt und **kein** virtueller Computer, Speicherkonto oder virtuelles Netzwerk ist, können Sie Resource Manager ohne Komplikationen nutzen.

Für virtuelle Computer, Speicherkonten und virtuelle Netzwerke gilt: Wenn die Ressource über die klassische Bereitstellung erstellt wurde, müssen Sie diese weiterhin mit klassischen Vorgängen ausführen. Falls der virtuelle Computer, das Speicherkonto oder das virtuelle Netzwerk per Resource Manager-Bereitstellung erstellt wurde, müssen Sie weiterhin Resource Manager-Vorgänge verwenden. Diese Unterscheidung kann besonders verwirrend sein, wenn Ihr Abonnement eine Mischung aus Ressourcen enthält, die mit Resource Manager und mit der klassischen Bereitstellung erstellt wurden. Diese Ressourcenkombination kann zu unerwarteten Ergebnissen führen, da die Ressourcen nicht die gleichen Vorgänge unterstützen.

In einigen Fällen können Sie mithilfe eines Resource Manager-Befehls Informationen zu einer mit der klassischen Bereitstellung erstellten Ressource abrufen oder eine administrative Aufgabe ausführen, z.B. eine klassische Ressource in eine andere Ressourcengruppe verschieben. Dies sollte jedoch nicht zu der Annahme verleiten, dass der entsprechende Ressourcentyp Resource Manager-Vorgänge unterstützt. Nehmen Sie beispielsweise an, Sie verfügen über eine Ressourcengruppe, die einen mit dem klassischen Bereitstellungsmodell erstellten virtuellen Computer umfasst. Bei Ausführung des folgenden Resource Manager-PowerShell-Befehls:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

wird der virtuelle Computer zurückgegeben:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Das Resource Manager-Cmdlet **Get-AzureRmVM** gibt aber nur per Resource Manager bereitgestellte virtuelle Computer zurück. Mit dem folgenden Befehl wird der über das klassische Bereitstellungsmodell erstellte virtuelle Computer nicht zurückgegeben.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Bei der Arbeit mit virtuellen Computern sind auch noch andere wichtige Aspekte zu berücksichtigen.

- Virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht in ein virtuelles Netzwerk eingebunden werden, das mit dem Ressourcen-Manager bereitgestellt wurde.
- Virtuelle Computer, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurden, müssen in ein virtuelles Netzwerk eingebunden werden.
- Virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, müssen nicht in ein virtuelles Netzwerk eingebunden werden.

Informationen zum Verbinden virtueller Netzwerke aus verschiedenen Bereitstellungsmodellen finden Sie unter [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](./virtual-network/virtual-networks-arm-asm-s2s.md).

Nur Ressourcen, die mit dem Ressourcen-Manager erstellt wurden, unterstützen Tags. Sie können keine Tags auf klassische Ressourcen anwenden. Weitere Informationen zur Verwendung von Tags im Ressourcen-Manager finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).

## Merkmale des Ressourcen-Managers

Zum besseren Verständnis der beiden Modelle sehen wir uns nun die Merkmale der Resource Manager-Typen an:

- Erstellt mit dem [Azure-Portal](https://portal.azure.com/).

     ![Azure-Portal](./media/resource-manager-deployment-model/portal.png)

     Für Compute-, Speicher- und Netzwerkressourcen haben Sie die Möglichkeit, entweder den Resource Manager oder die klassische Bereitstellung zu nutzen. Wählen Sie **Ressourcen-Manager**.

     ![Resource Manager-Bereitstellung](./media/resource-manager-deployment-model/select-resource-manager.png)

- Erstellt mit der Resource Manager-Version der Azure PowerShell-Cmdlets. Diese Befehle haben das Format *Verb-AzureRmNomen*, wie unten dargestellt.

        New-AzureRmResourceGroupDeployment

- Erstellt mit der [Azure Resource Manager-REST-API](https://msdn.microsoft.com/library/azure/dn790568.aspx) für REST-Vorgänge.

- Erstellt mit Befehlen der Azure-Befehlszeilenschnittstelle, die im Modus **arm** ausgeführt werden.

        azure config mode arm
        azure group deployment create 

- Der Name des Ressourcentyps enthält nicht das Wort **(klassisch)**. Die folgende Abbildung zeigt den Typ **Speicherkonto**.

    ![Web-App](./media/resource-manager-deployment-model/resource-manager-type.png)

Die in der folgenden Abbildung dargestellte Anwendung zeigt über den Ressourcen-Manager bereitgestellte Ressourcen in einer einfachen Ressourcengruppe.

  ![Resource Manager – Architektur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Außerdem gibt es Beziehungen zwischen den Ressourcen innerhalb der Ressourcenanbieter:

- Ein virtueller Computer hängt von einem bestimmten Speicherkonto ab, das im SRP definiert ist, um die Datenträger im Blobspeicher zu speichern (erforderlich).
- Ein virtueller Computer verweist auf eine bestimmte NIC, die im NRP definiert ist (erforderlich), und eine Verfügbarkeitsgruppe, die im CRP definiert ist (optional).
- Eine NIC verweist auf die dem virtuellen Computer zugewiesene IP-Adresse (erforderlich), das Subnetz des virtuellen Netzwerks für den virtuellen Computer (erforderlich) und eine Netzwerksicherheitsgruppe (optional).
- Ein Subnetz innerhalb eines virtuellen Netzwerks verweist auf eine Netzwerksicherheitsgruppe (optional).
- Eine Lastenausgleichsinstanz verweist auf den Back-End-Pool von IP-Adressen, die die NIC eines virtuellen Computers enthalten (optional) und auf eine öffentliche oder private IP-Adresse für den Lastenausgleich (optional).

## Merkmale der klassischen Bereitstellung

Das klassische Bereitstellungsmodell ist Ihnen möglicherweise unter den Bezeichnungen "Service-Management-Modell" oder "Dienstverwaltungsmodell" bekannt.

Ressourcen, die im klassischen Bereitstellungsmodell erstellt wurden, weisen folgende Merkmale auf:

- Erstellt mit dem [klassischen Portal](https://manage.windowsazure.com).

     ![Klassisches Portal](./media/resource-manager-deployment-model/classic-portal.png)

     Alternativ können Sie das Azure-Portal nutzen und eine **klassische** Bereitstellung (für Compute-, Speicher- und Netzwerkressourcen) festlegen.

     ![Klassische Bereitstellung](./media/resource-manager-deployment-model/select-classic.png)

- Erstellt mit der Dienstverwaltungsversion der Azure PowerShell-Cmdlets. Diese Befehlsnamen haben das Format *Verb-AzureNomen*, wie unten dargestellt.

        New-AzureVM 

- Erstellt mit der [Dienstverwaltungs-REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx) für REST-Vorgänge.
- Erstellt mit Befehlen der Azure-Befehlszeilenschnittstelle, die im Modus **asm** ausgeführt werden.

        azure config mode asm
        azure vm create 

- Der Name des Ressourcentyps enthält das Wort **(klassisch)**. Die folgende Abbildung zeigt den Typ **Speicherkonto (klassisch)**.

    ![Klassischer Typ](./media/resource-manager-deployment-model/classic-type.png)

Sie können weiterhin das Azure-Portal zur Verwaltung von Ressourcen verwenden, die über die klassische Bereitstellung erstellt wurden.

In der Azure-Dienstverwaltung werden die Compute-, Speicher- oder Netzwerkressourcen für das Hosten von virtuellen Computern wie folgt bereitgestellt:

- Ein erforderlicher Clouddienst, der als Container für das Hosten virtueller Computer (Compute) fungiert. Virtuelle Computer werden automatisch mit einer Netzwerkschnittstellenkarte (Network Interface Card, NIC) bereitgestellt, und ihnen wird von Azure eine IP-Adresse zugewiesen. Darüber hinaus enthält der Clouddienst eine externe Lastenausgleichsinstanz, eine öffentliche IP-Adresse und Standardendpunkte, um Remotedesktop- und Remote-PowerShell-Datenverkehr für die Windows-basierten virtuellen Computer und Secure Shell-Datenverkehr (SSH) für Linux-basierte virtuelle Computer zu ermöglichen.
- Ein erforderliches Speicherkonto, in dem die VHDs für einen virtuellen Computer gespeichert werden, einschließlich des Betriebssystems sowie temporärer und zusätzlicher Datenträger (Speicher).
- Ein optionales virtuelles Netzwerk, das als zusätzlicher Container fungiert, in dem Sie eine Subnetzstruktur erstellen und das Subnetz benennen können, in dem sich der virtuelle Computer befindet (Netzwerk).

Im Folgenden sind die Komponenten und deren Beziehungen für die Azure-Dienstverwaltung dargestellt.

  ![Klassische Architektur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

## Migrieren von der klassischen Bereitstellung zu Resource Manager

Wenn Sie bereit zum Migrieren Ihrer Ressourcen von der klassischen Bereitstellung zur Resource Manager-Bereitstellung sind, helfen Ihnen die folgenden Informationen weiter:

1. [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Platform supported migration of IaaS resources from Classic to Azure Resource Manager (Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager)](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## Nächste Schritte

- Eine exemplarische Vorgehensweise zum Erstellen einer Vorlage, mit der ein virtueller Computer, ein Speicherkonto und ein virtuelles Netzwerk definiert werden, finden Sie unter [Resource Manager-Vorlage – exemplarische Vorgehensweise](resource-manager-template-walkthrough.md).
- Informationen zur Struktur von Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
- Die Befehle zum Bereitstellen einer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

<!---HONumber=AcomDC_0803_2016-->