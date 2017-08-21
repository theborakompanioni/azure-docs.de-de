---
title: "Ändern von virtuellen Linux-Computernamen in Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, weshalb Gerätenamen geändert werden. Weiterhin wird eine Lösung für dieses Problem beschrieben."
services: virtual-machines-linux
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: 
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 07/12/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 789f4580901a22dc3aaae9599c7205c76f268403
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---

# <a name="troubleshooting-linux-vm-device-names-are-changed"></a>Problembehandlung: Die Namen von virtuellen Linux-Computern werden geändert

In diesem Artikel wird erläutert, weshalb nach dem Neustart eines virtuellen Linux-Computers (Virtual Machine, VM) und nach dem erneuten Anfügen der Datenträger die Gerätenamen geändert werden. Darüber hinaus wird eine Lösung für dieses Problem beschrieben.

## <a name="symptom"></a>Symptom

Wenn Sie in Microsoft Azure virtuelle Linux-Computer verwenden, können die folgenden Probleme auftreten.

- Der virtuelle Computer startet nach einem Neustart nicht.

- Wenn Datenträger getrennt und wieder angefügt werden, werden deren Namen geändert.

- Anwendungen oder Skripts, die über den Gerätenamen auf einen Datenträger verweisen, lösen einen Fehler aus. Der Gerätename des Datenträgers hat sich geändert.

## <a name="cause"></a>Ursache

In Linux ist nicht garantiert, dass die Gerätepfade nach einem Neustart konsistent bleiben. Die Gerätenamen setzen sich aus Major- (Buchstaben) und Minor-Nummern zusammen.  Wenn der Linux-Speichergerätetreiber ein neues Gerät erkennt, weist er ihm aus dem verfügbaren Bereich Major- und Minor-Nummern zu. Wenn ein Gerät entfernt wird, werden die Gerätenummern für eine spätere Verwendung freigegeben.

Das Problem tritt auf, weil die vom SCSI-Subsystem geplante Geräteüberprüfung in Linux asynchron erfolgt. Die endgültige Benennung des Gerätepfads kann bei verschiedenen Neustarts unterschiedlich ausfallen. 

## <a name="solution"></a>Lösung

Um dieses Problem zu beheben, können Sie die persistente Benennung verwenden. Für die persistente Benennung stehen vier Methoden zur Verfügung: nach der Dateisystembezeichnung, nach der UUID, nach der ID und nach dem Pfad. Für virtuelle Linux-Computer in Azure wird die Dateisystembezeichnung und UUID als Methoden empfohlen. 

Die meisten Distributionen stellen die Optionen **nofail**, **nobootwait** oder „fstab“ zur Verfügung. Mit diesen Optionen kann ein System auch dann neu gestartet werden, wenn der Datenträger beim Start nicht bereitgestellt werden kann. Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation zu Ihrer Distribution. Weitere Informationen über das Konfigurieren eines virtuellen Linux-Computers für die Verwendung einer UUID beim Hinzufügen eines Datenträgers finden Sie in [Herstellen einer Verbindung mit dem virtuellen Linux-Computer zum Bereitstellen des neuen Datenträgers](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

Beim Installieren des Azure-Linux-Agenten auf einem virtuellen Computer werden für den Aufbau einer Reihe symbolischer Links in **/dev/disk/azure** udev-Regeln verwendet. Diese udev-Regeln können von Anwendungen und Skripts zum Identifizieren der an den virtuellen Computer angefügten Datenträger sowie deren Typ und der logischen Gerätenummer verwendet werden.

## <a name="more-information"></a>Weitere Informationen

### <a name="identify-disk-luns"></a>Identifizieren der logischen Datenträgernummern

Eine Anwendung kann logische Gerätenummern dazu verwenden, alle angefügten Datenträger zu ermitteln und symbolische Links aufzubauen. Der Azure-Linux-Agent enthält jetzt udev-Regeln, mit denen symbolische Links zwischen einer logischen Gerätenummer und den Geräten erstellt werden. Dies geschieht wie folgt:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 

Informationen über die logischen Gerätenummern können auch mit Isscsi oder einem ähnlichen Tool vom Linux-Gast abgerufen werden. Dies ist auf folgende Weise möglich.

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Die Informationen über die logischen Gerätenummern des Linux-Gasts können mit den Metadaten des Azure-Abonnements verwendet werden, um den Speicherort im Azure-Speicher auf der VHD zu ermitteln, an dem die Partitionsdaten gespeichert weden. Verwenden Sie z.B. „az cli“:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                             
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Erkunden des Dateisystem-UUIDs mit „blkid“

Die Ausgabe von „blkid“ oder ähnlichen Informationsquellen können mit einem Skript oder einer Anwendung gelesen werden. Mit einem solchen Skript oder einer solchen Anwendung ist es auch möglich, in **/dev** symbolische Links aufzubauen. Die Ausgabe enthält die UUIDs aller an den virtuellen Computer angefügten Datenträger sowie die Gerätedatei, der sie zugeordnet sind:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Die waagent udev-Regeln erstellen in **/dev/disk/azure** eine Reihe symbolischer Links:


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


Die Anwendung kann diese Informationen dazu nutzen, den Startdatenträger und den (kurzlebigen) Ressourcendatenträger zu ermitteln. In Azure sollten die Anwendungen auf **/dev/disk/azure/root-part1** oder **/dev/disk/azure-resource-part1** verweisen, um diese Partitionen zu erkennen.

Wenn zusätzliche Partitionen aus der „blkid“-Liste vorhanden sind, befinden Sie sich auf einem Datenträger. Um den Gerätenamen zur Laufzeit zu erkennen, können die Anwendungen die UUID für diese Partitionen erhalten und einen Pfad analog zu dem nachstehend angegebenen verwenden:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a>Abrufen der neuesten Azure-Speicherregeln

Um die neuesten Azure-Speicherregeln abzurufen, führen Sie die folgenden Befehle aus:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


Weitere Informationen finden Sie in den folgenden Artikeln:

- [Ubuntu: Using UUID (Ubuntu: Verwenden der UUID)](https://help.ubuntu.com/community/UsingUUID)

- [Red Hat: Persistent Naming (Red Hat: Persistente Benennung)](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [Linux: What UUIDs can do for you (Linux: Wie UUIDs Ihnen helfen)](https://www.linux.com/news/what-uuids-can-do-you)

- [Udev: Introduction to Device Management In Modern Linux System (uDev: Einführung in die Geräteverwaltung in modernen Linux-Systemen)](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)


