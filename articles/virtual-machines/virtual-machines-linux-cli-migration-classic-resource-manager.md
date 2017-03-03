---
title: Migrieren von VMs in den Resource Manager mit der Azure-CLI | Microsoft-Dokumentation
description: "In diesem Artikel wird die plattformgestützte Migration von Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle erläutert."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: e64449991bc28427d8f559ed13c3bdf9160488db
ms.openlocfilehash: 92211cc98b6d8394ff04bc7c2fe33f7bd710713b
ms.lasthandoff: 01/26/2017


---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Azure-Befehlszeilenschnittstelle
Diese Schritte zeigen, wie Sie Befehle der Azure-Befehlszeilenschnittstelle zum Migrieren von IaaS-Ressourcen (Infrastructure as a Service) aus dem klassischen Bereitstellungsmodell in das Azure Resource Manager-Bereitstellungsmodell verwenden. Für diesen Artikel ist die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)erforderlich.

> [!NOTE]
> Alle hier beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.
> 
> 

## <a name="step-1-prepare-for-migration"></a>Schritt 1: Vorbereiten der Migration
Hier finden Sie einige bewährte Methoden, die wir empfehlen, wenn Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen:

* Sehen Sie sich die [Liste mit nicht unterstützten Konfigurationen und Features](virtual-machines-windows-migration-classic-resource-manager.md) an. Verwenden Ihre virtuellen Computer nicht unterstützte Konfigurationen oder Features, empfiehlt es sich, zu warten, bis die Unterstützung für die entsprechenden Features/Konfigurationen angekündigt wird. Alternativ können Sie ggf. die betroffenen Features entfernen oder eine andere Konfiguration verwenden, um die Migration zu ermöglichen.
* Wenn Sie derzeit über automatisierte Skripts zum Bereitstellen Ihrer Infrastruktur und Anwendungen verfügen, versuchen Sie, mithilfe dieser Skripts für die Migration eine ähnliche Testeinrichtung zu erstellen. Alternativ dazu können Sie über das Azure-Portal Beispielumgebungen einrichten.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Schritt 2: Festlegen des Abonnements und Registrieren des Anbieters
Für Migrationsszenarien müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten. [Installieren Sie die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und [wählen Sie Ihr Abonnement aus](../xplat-cli-connect.md).

Melden Sie sich bei Ihrem Konto an.

    azure login

Führen Sie zum Auswählen des Azure-Abonnements den folgenden Befehl aus.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Die Registrierung ist ein einmaliger Schritt, der jedoch einmal ausgeführt werden muss, bevor Sie versuchen, die Migration auszuführen. Ohne Registrierung wird die folgende Fehlermeldung angezeigt: 
> 
> *BadRequest: Subscription is not registered for migration.* (BadRequest: Das Abonnement ist nicht für die Migration registriert.) 
> 
> 

Registrieren Sie sich mithilfe des folgenden Befehls beim Migrationsressourcenanbieter. Beachten Sie, dass in einigen Fällen für diesen Befehl ein Timeout festgelegt ist. Die Registrierung wird jedoch erfolgreich durchgeführt.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Warten Sie daher etwas. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden. Stellen Sie sicher, dass der RegistrationState-Wert `Registered` lautet, bevor Sie fortfahren.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Wechseln Sie nun in den `asm`-Modus der Befehlszeilenschnittstelle.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Schritt 3: Sicherstellen, dass Sie über genügend Kerne in virtuellen Azure Resource Manager-Computern in der Azure-Region Ihrer aktuellen Bereitstellung oder Ihres VNET verfügen
Für diesen Schritt müssen in den `arm` -Modus wechseln. Führen Sie dazu den folgenden Befehl aus.

```
azure config mode arm
```

Mit dem folgenden CLI-Befehl können Sie Ihre aktuelle Anzahl an Kernen in Azure Resource Manager überprüfen. Weitere Informationen zu Kernkontingenten finden Sie unter [Grenzwerte und Azure Resource Manager](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Sobald Sie diesen Schritt überprüft haben, können Sie zurück in den `asm` -Modus wechseln.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Schritt 4: Option 1 – Migrieren von virtuellen Computern in einem Clouddienst
Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Beachten Sie: Falls sich die virtuellen Computer im Clouddienst in einem virtuellen Netzwerk befinden oder über Web-/Workerrollen verfügen, wird eine Fehlermeldung zurückgegeben.

    azure service list

Führen Sie den folgenden Befehl aus, um in der ausführlichen Ausgabe den Bereitstellungsnamen für den Clouddienst zu ermitteln. In den meisten Fällen entspricht der Bereitstellungsname dem Namen des Clouddiensts.

    azure service show <serviceName> -vv

Bereiten Sie die virtuellen Computer im Clouddienst für die Migration vor. Dabei stehen Ihnen zwei Optionen zur Auswahl.

Wenn Sie die virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk migrieren möchten, verwenden Sie den folgenden Befehl.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Wenn Sie als Migrationsziel ein vorhandenes virtuelles Netzwerk im Resource Manager-Bereitstellungsmodell verwenden möchten, führen Sie den folgenden Befehl aus.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Nach der Vorbereitung können Sie sich in der ausführlichen Ausgabe über den Migrationsstatus der virtuellen Computer informieren und sich vergewissern, dass sich die virtuellen Computer im Status `Prepared` befinden.

    azure vm show <vmName> -vv

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe der Befehlszeilenschnittstelle oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

    azure service deployment abort-migration <serviceName> <deploymentName>

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Schritt 4: Option 2 – Migrieren von virtuellen Computern in einem virtuellen Netzwerk
Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. Beachten Sie: Falls das virtuelle Netzwerk Web-/Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Validierungsfehler auf.

Rufen Sie mithilfe des folgenden Befehls alle virtuellen Netzwerke im Abonnement ab.

    azure network vnet list

Die Ausgabe sieht in etwa wie folgt aus:

![Screenshot der Befehlszeile, in dem der gesamte Name des virtuellen Netzwerks hervorgehoben ist.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Im obigen Beispiel ist **virtualNetworkName** der vollständige Name **Group classicubuntu16 classicubuntu16**.

Bereiten Sie das gewünschte virtuelle Netzwerk mithilfe des folgenden Befehls für die Migration vor.

    azure network vnet prepare-migration <virtualNetworkName>

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe der Befehlszeilenschnittstelle oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

    azure network vnet abort-migration <virtualNetworkName>

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Schritt 5: Migrieren eines Speicherkontos
Sobald Sie mit der Migration der virtuellen Computer fertig sind, sollten Sie das Speicherkonto migrieren.

Bereiten Sie das Speicherkonto Netzwerk mithilfe des folgenden Befehls für die Migration vor.

    azure storage account prepare-migration <storageAccountName>

Überprüfen Sie die Konfiguration des vorbereiteten Speicherkontos mithilfe der Befehlszeilenschnittstelle oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

    azure storage account abort-migration <storageAccountName>

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Nächste Schritte
* [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md)
* [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)


