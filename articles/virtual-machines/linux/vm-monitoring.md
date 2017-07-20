---
title: "Aktivieren oder Deaktivieren der Azure VM-Überwachung"
description: "Beschreibt, wie die Azure VM-Überwachung aktiviert oder deaktiviert wird"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 9b2fe579113d6ca6bfd27d82eb9d4657657d44ff
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017

---
# <a name="enable-or-disable-azure-vm-monitoring"></a>Aktivieren oder Deaktivieren der Azure VM-Überwachung

Dieser Abschnitt beschreibt, wie die Überwachung auf virtuellen Computern, die auf Azure laufen, aktiviert oder deaktiviert wird. Sie können die Überwachung aktivieren oder deaktivieren, indem Sie das Portal oder die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure-CLI) verwenden.

> [!IMPORTANT]
> Dieses Dokument beschreibt Version 2.3 der Linux-Diagnoseerweiterung, die veraltet ist. Version 2.3 wird bis zum 30. Juni 2018 unterstützt.
>
> Stattdessen kann Version 3.0 der Linux-Diagnoseerweiterung aktiviert werden. Weitere Informationen finden Sie in der [Dokumentation](./diagnostic-extension.md).

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Aktivieren/Deaktivieren der Überwachung über das Azure-Portal

Sie können die Überwachung Ihrer Azure-VM aktivieren, wodurch in Zeitabständen von einer Minute Daten zu Ihrer Instanz bereitgestellt werden. (Speicheränderungen greifen.) Detaillierte Diagnosedaten für die VM sind dann in den Portalgraphen oder über die API verfügbar. Standardmäßig ermöglicht das Azure-Portal die hostbasierte Überwachung eines eingeschränkten Satzes von Metriken. Sie können die Überwachung der Metriken innerhalb einer VM aktivieren, während diese ausgeführt wird oder wenn sie sich im Zustand „Beendet“ befindet.

* Öffnen Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
* Klicken Sie in der linken Navigation auf „Virtuelle Computer“.
* Wählen Sie in der Liste „Virtuelle Computer“ eine laufende oder beendete Instanz aus. Das Blatt „Virtueller Computer“ wird geöffnet.
* Klicken Sie auf „Alle Einstellungen“.
* Klicken Sie auf „Diagnose“.
* Ändern Sie den Status auf „Ein“ oder „Aus“. In diesem Blatt können Sie auch die Ebene der Überwachungsdetails auswählen, die Sie für Ihren virtuellen Computer aktivieren wollen.

![Aktivieren bzw. deaktivieren Sie die Überwachung über das Azure-Portal.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a>Aktivierung bzw. Deaktivierung der Überwachung über die Azure-Befehlszeilenschnittstelle

So aktivieren Sie die Überwachung für eine Azure-VM.

* Erstellen Sie eine Datei (z.B. mit dem Namen „PrivateConfig.json“):

```json
{
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account"
}
```

* Aktivieren Sie die Erweiterung über die Azure-Befehlszeilenschnittstelle.

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

Weitere Informationen finden Sie unter [Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten einer Linux-VM](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png

