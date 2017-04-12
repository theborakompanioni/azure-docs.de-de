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
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: ffc9a84a3a89b37f772826c612f4fa9abadc1a0e
ms.lasthandoff: 04/03/2017


---
# <a name="enable-or-disable-azure-vm-monitoring"></a>Aktivieren oder Deaktivieren der Azure VM-Überwachung
Dieser Abschnitt beschreibt, wie die Überwachung auf virtuellen Computern, die auf Azure laufen, aktiviert oder deaktiviert wird. Die Überwachung ist auf Azure Virtual Machines standardmäßig aktiviert, wenn sie vom [Azure-Portal](https://portal.azure.com) aus bereitgestellt werden, und Überwachungsgraphen werden standardmäßig mit einem Zeitabstand von einer Minute zur Verfügung gestellt. Sie können die Überwachung aktivieren oder deaktivieren, indem Sie das Portal oder die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure-CLI) verwenden. 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Aktivieren bzw. Deaktivieren der Überwachung über das Azure-Portal
Sie können die Überwachung Ihrer Azure-VM aktivieren, wodurch in Zeitabständen von einer Minute Daten zu Ihrer Instanz bereitgestellt werden. (Speicheränderungen greifen.) Detaillierte Diagnosedaten für die VM sind dann in den Portalgraphen oder über die API verfügbar. Standardmäßig ist im Azure-Portal die Überwachung aktiviert, jedoch können Sie sie, wie unten beschrieben, abstellen. Sie können die Überwachung aktivieren, während die VM läuft, oder während sie sich im Zustand „Beendet“ befindet.

* Öffnen Sie das Azure-Portal unter **[https://portal.azure.com](https://portal.azure.com)**.
* Klicken Sie in der linken Navigation auf „Virtuelle Computer“.
* Wählen Sie in der Liste „Virtuelle Computer“ eine laufende oder beendete Instanz aus. Das Blatt „Virtuelle Computer“ öffnet sich.
* Klicken Sie auf „Alle Einstellungen“.
* Klicken Sie auf „Diagnose“.
* Ändern Sie den Status auf „Ein“ oder „Aus“. In diesem Blatt können Sie auch die Ebene der Überwachungsdetails auswählen, die Sie für Ihren virtuellen Computer aktivieren wollen.

[Azure.Note] Standardmäßig ist beim Erstellen eines neuen virtuellen Computers bei „Diagnose“ der Status „Ein“ eingestellt.

![Aktivieren bzw. Deaktivieren der Überwachung über das Azure-Portal][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a>Aktivierung bzw. Deaktivierung der Überwachung über die Azure-Befehlszeilenschnittstelle
So aktivieren Sie die Überwachung für eine Azure-VM.

* Erstellen Sie eine Datei namens PrivateConfig.json mit dem folgenden Inhalt.
        { "storageAccountName":"the storage account to receive data", "storageAccountKey":"the key of the account" }
* Führen Sie den folgenden Azure-CLI-Befehl aus:
  
        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Sie können von Version 2.0 zu neueren Versionen wechseln, wenn diese verfügbar werden. 

Weitere Informationen zur Konfiguration von Überwachungsmetriken und Beispielen finden Sie im Dokument **[Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten einer Linux-VM](classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png



