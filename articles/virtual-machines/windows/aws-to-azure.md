---
title: Verschieben von Windows-AWS-VMs nach Azure | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie eine Windows-Instanz vom Typ „Amazon Web Services (AWS) EC2“ mit Azure PowerShell auf Azure Virtual Machines verschieben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 22f5dda489f94ebb5a22e017b72c2524a8f22d6a
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---

<a id="move-a-windows-vm-from-amazon-web-services-aws-to-azure-using-powershell" class="xliff"></a>

# Verschieben einer Windows-VM aus Amazon Web Services (AWS) nach Azure mit PowerShell

Wenn Sie virtuelle Azure-Computer für das Hosten von Workloads auswerten, können Sie eine vorhandene Windows-VM-Instanz vom Typ „Amazon Web Services (AWS) EC2“ exportieren und die virtuelle Festplatte (VHD) dann in Azure hochladen. Nach dem Hochladen der VHD können Sie in Azure aus der VHD eine neue VM erstellen. 

In diesem Thema wird beschrieben, wie Sie eine einzelne VM aus AWS nach Azure verschieben. Wenn Sie VMs bedarfsorientiert aus AWS nach Azure verschieben möchten, helfen Ihnen die Informationen unter [Migrieren virtueller Computer von Amazon Web Services (AWS) zu Azure mit Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md) weiter.

<a id="prepare-the-vm" class="xliff"></a>

## Vorbereiten des virtuellen Computers 
 
Sie können generalisierte sowie spezialisierte VHDs in Azure hochladen. Für jeden Typ müssen Sie den virtuellen Computer vor dem Export aus AWS vorbereiten. 

- **Generalisierte VHD:** Auf einer generalisierten VHD wurden alle Ihre persönlichen Kontoinformationen mit Sysprep entfernt. Wenn Sie die VHD als Image zum Erstellen neuer virtueller Computer verwenden möchten, sollten Sie folgende Schritte ausführen: 
 
    * [Vorbereiten einer Windows-VM](prepare-for-upload-vhd-image.md)  
    * Generalisieren eines virtuellen Computers mithilfe von Sysprep  

 
- **Spezialisierte VHD:** Auf einer spezialisierten VHD werden die Benutzerkonten, Anwendungen und andere Statusdaten Ihres ursprünglichen virtuellen Computers beibehalten. Wenn Sie die VHD unverändert zum Erstellen eines neuen virtuellen Computers verwenden möchten, führen Sie auf jeden Fall die folgenden Schritte aus.  
    * [Vorbereiten einer Windows-VHD für das Hochladen in Azure.](prepare-for-upload-vhd-image.md) Generalisieren Sie den virtuellen Computer **nicht** mithilfe von Sysprep. 
    * Entfernen Sie alle auf dem virtuellen Computer installierten Gast-Virtualisierungstools und Agents (d.h. VMware-Tools). 
    * Stellen Sie sicher, dass der virtuelle Computer so konfiguriert ist, dass die zugehörigen IP-Adressen und DNS-Einstellungen per Pullvorgang über DHCP übertragen werden. Dadurch wird sichergestellt, dass der Server beim Starten eine IP-Adresse innerhalb des VNET bezieht.  


<a id="export-and-download-the-vhd" class="xliff"></a>

## Exportieren und Herunterladen der VHD 

Exportieren Sie die EC2-Instanz auf eine VHD in einem Amazon S3-Bucket. Führen Sie die Schritte aus, die im Amazon-Dokumentationsthema [Exporting an Instance as a VM Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) (Exportieren einer Instanz als VM per VM-Import/Export) beschrieben sind, und führen Sie den Befehl [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) aus, um die EC2-Instanz in eine VHD-Datei zu exportieren. 

Die exportierte VHD-Datei wird in dem von Ihnen angegebenen Amazon S3-Bucket gespeichert. Die grundlegende Syntax zum Exportieren der VHD ist unten angegeben. Ersetzen Sie den Platzhaltertext in <brackets> einfach durch Ihre Informationen.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

Befolgen Sie nach dem Exportieren der VHD die Anleitung unter [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) (Wie lade ich ein Objekt aus einem S3-Bucket herunter?), um die VHD-Datei aus dem S3-Bucket herunterzuladen. 

> [!IMPORTANT]
> Bei AWS fallen bei der Datenübertragung Gebühren für das Herunterladen der VHD an. Weitere Informationen finden Sie unter [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/) (Amazon S3 – Preise).


<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Jetzt können Sie die VHD in Azure hochladen und eine neue VM erstellen. 

- Wenn Sie auf Ihrer Quelle Sysprep ausgeführt haben, um sie vor dem Exportieren zu **generalisieren**, helfen Ihnen die Informationen unter [Erstellen einer generalisierten VHD in Azure, die mithilfe verwalteter Datenträger in Azure hochgeladen wurde](upload-generalized-managed.md) weiter.
- Wenn Sie Sysprep vor dem Export nicht ausgeführt haben, wird die VHD als **speziell** bezeichnet. In diesem Fall helfen Ihnen die Informationen unter [Erstellen eines virtuellen Computers von einem speziellen Datenträger](create-vm-specialized.md) weiter.

 

