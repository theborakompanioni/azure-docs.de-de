---
title: Auf eine VM-ID zugreifen
description: Beschreibt die Verwendung und den Zugriff auf die eindeutige Azure VM-ID
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 8bbde19aeccec285f9c5727540f49b7c9207b3b3


---
# <a name="accessing-and-using-azure-vm-unique-id"></a>Verwenden einer eindeutigen Azure VM-ID und darauf zugreifen
Eine eindeutige Azure VM-ID ist ein 128-Bit-Bezeichner, der in dem SMBIOS aller Azure IaaS-VMs codiert und gespeichert ist. Derzeit kann er mithilfe von BIOS-Plattformbefehlen gelesen werden.

Die eindeutige Azure VM-ID ist eine schreibgeschützte Eigenschaft. Die eindeutige Azure VM-ID ändert sich weder beim Neustart/Herunterfahren (entweder geplant oder ungeplant), noch beim Starten/Beenden der Speicherplatzfreigabe, der Dienstreparatur oder der Wiederherstellung vor Ort. Wenn die VM jedoch eine Momentaufnahme ist und zum Erstellen einer neuen Instanz kopiert wird, wird eine neue Azure VM-ID konfiguriert.

> [!NOTE]
> Wenn Sie ältere VMs haben, die vor dem Rollout dieser neuen Funktion (18. September 2014 ) erstellt wurden und seitdem in Betrieb sind, starten Sie bitte Ihre VM neu, um automatisch eine eindeutige Azure-ID zu erhalten.
> 
> 

So greifen Sie aus der VM heraus auf die eindeutige Azure VM-ID zu:

## <a name="create-a-vm"></a>Erstellen einer VM
Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers](virtual-machines-linux-creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM
Weitere Informationen finden Sie unter [SSH von Linux](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="query-vm-unique-id"></a>Abfragen einer eindeutigen VM-ID
Befehl (das Beispiel verwendet **Ubuntu**):

```bash
sudo dmidecode | grep UUID
```

Die erwarteten Ergebnisse aus dem Beispiel:

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

Aufgrund der Big-Endian-Byte-Reihenfolge wird die tatsächliche eindeutige VM-ID in diesem Fall die folgende sein:

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

Die eindeutige Azure VM-ID kann in verschiedenen Szenarios verwendet werden (je nachdem, ob die VM in Azure oder lokal ausgeführt wird). Darüber hinaus unterstützt es Ihre Lizenzierung, Ihre Berichterstattung oder Ihre allgemeinen Nachverfolgungsanforderungen, die Sie möglicherweise auf Ihren Azure-IaaS-Bereitstellungen haben. Viele unabhängige Softwareanbieter, die Anwendungen in Azure erstellen und zertifizieren, müssen eine Azure VM möglicherweise während ihres gesamten Lebenszyklus identifizieren und feststellen können, ob die VM in Azure, lokal oder auf einem anderen Cloudanbieter ausgeführt wird. Dieser Plattformbezeichner kann z.B. helfen, herauszufinden, ob die Software ordnungsgemäß lizenziert ist. Außerdem kann er VM-Daten ihrer Quelle zuordnen, um so z.B. beim Festlegen der richtigen Metriken für die richtige Plattform zu unterstützen, oder diese Metriken u.a. nachzuverfolgen und zuzuordnen.




<!--HONumber=Nov16_HO3-->


