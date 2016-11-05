---
title: Erstellen einer Kopie Ihres virtuellen Azure-Computers unter Linux | Microsoft Docs
description: Erfahren Sie, wie Sie eine Kopie eines virtuellen Azure-Computers unter Linux im Resource Manager-Bereitstellungsmodell erstellen.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: timlt
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: cynthn

---
# Erstellen einer Kopie eines virtuellen Linux-Computers, der in Azure ausgeführt wird
In diesem Artikel erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers (VM) unter Linux im Resource Manager-Bereitstellungsmodell erstellen können. Kopieren Sie zuerst die Datenträger für das Betriebssystem und die Daten in einen neuen Container, richten Sie dann die Netzwerkressourcen ein, und erstellen Sie den neuen virtuellen Computer.

Sie können auch [einen virtuellen Computer über ein benutzerdefiniertes Datenträgerimage hochladen und erstellen](virtual-machines-linux-upload-vhd.md).

## Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie mit den Schritten beginnen:

* Sie haben die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) heruntergeladen und auf Ihrem Computer installiert.
* Außerdem benötigen Sie einige Informationen zu Ihrem vorhandenen virtuellen Azure-Computer unter Linux:

| Informationen zum virtuellen Quellcomputer | Ursprung |
| --- | --- |
| Name des virtuellen Computers |`azure vm list` |
| Ressourcengruppenname |`azure vm list` |
| Standort |`azure vm list` |
| Speicherkontoname |`azure storage account list -g <resourceGroup>` |
| Containername |`azure storage container list -a <sourcestorageaccountname>` |
| Name der VHD-Datei des virtuellen Quellcomputers |`azure storage blob list --container <containerName>` |

* Sie müssen Sie einige Entscheidungen für den neuen virtuellen Computer treffen: <br> – Containername <br> – VM-Name <br> – VM-Größe <br> – vNet-Name <br> – Subnetzname <br> – IP-Name <br> – NIC-Name

## Anmelden und Festlegen des Abonnements
1. Melden Sie sich bei der Befehlszeilenschnittstelle an.
   
        azure login
2. Stellen Sie sicher, dass Sie sich im Resource Manager-Modus befinden.
   
        azure config mode arm
3. Legen Sie das richtige Abonnement fest. In der Azure-Kontenliste werden all Ihre Abonnements angezeigt.
   
        azure account set <SubscriptionId>

## Beenden des virtuellen Computers
Beenden Sie den virtuellen Quellcomputer, und geben Sie ihn frei. Über die Liste der virtuellen Azure-Computer erhalten Sie eine Liste aller virtuellen Computer in Ihrem Abonnement und deren Ressourcengruppennamen.

        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## Kopieren der VHD-Datei
Mit `azure storage blob copy start` können Sie die VHD-Datei aus dem Quellspeicher zum Ziel zu kopieren. In diesem Beispiel kopieren wir die VHD-Datei in dasselbe Speicherkonto, aber in einen anderen Container.

Um die VHD-Datei in einen anderen Container im selben Speicherkonto zu kopieren, geben Sie Folgendes ein:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>


## Einrichten des virtuellen Netzwerks für den neuen virtuellen Computer
Richten Sie ein virtuelles Netzwerk und eine NIC für den neuen virtuellen Computer ein.

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## Erstellen des neuen virtuellen Computers
Nun können Sie auf der Grundlage der hochgeladenen virtuellen Festplatte [mithilfe einer Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) oder über die Befehlszeilenschnittstelle einen virtuellen Computer erstellen, indem Sie den URI des kopierten Datenträgers wie folgt angeben:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## Nächste Schritte
Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle, um den neuen virtuellen Computer zu verwalten, finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](azure-cli-arm-commands.md).

<!---HONumber=AcomDC_0803_2016-->