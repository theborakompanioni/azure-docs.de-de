---
title: Erstellen von Vorlagen mit Linux-VM-Erweiterungen | Microsoft Docs
description: "Hier finden Sie Informationen zum Erstellen von Azure Resource Manager-Vorlagen mit Erweiterungen für virtuelle Linux-Computer."
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 322f8f0b-6697-4acb-b5f3-b3f58d28358b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 42c2a12551db35f4cb3994179ba2e277145f2d9b


---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Erstellen von Azure Resource Manager-Vorlagen mit Linux-VM-Erweiterungen
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus:

      Azure VM extension list

Dieser Befehl gibt den Herausgebernamen, den Erweiterungsnamen und die Version zurück:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Diese drei Eigenschaften sind im oben dargestellten Vorlagenausschnitt „publisher“, „type“ und „typeHandlerVersion“ zugeordnet.

> [!NOTE]
> Es empfiehlt sich, die aktuelle Erweiterungsversion zu verwenden, um die neuesten Funktionen zu erhalten.
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifizieren des Schemas für die Erweiterungskonfigurationsparameter
Im nächsten Schritt zur Erstellung einer Erweiterungsvorlage wird das Format für die Bereitstellung von Konfigurationsparametern festgelegt. Jede Erweiterung unterstützt einen eigenen Satz von Parametern.

Beispielkonfigurationen für Linux-Erweiterungen finden Sie in der Dokumentation unter [Beispiele für Linux-Erweiterungen](virtual-machines-linux-extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Eine vollständige Vorlage mit VM-Erweiterungen finden Sie hier:

[Benutzerdefinierte Skripterweiterung auf einem virtuellen Linux-Computer](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Die erstellte Vorlage können Sie anschließend über die Azure-Befehlszeilenschnittstelle bereitstellen.




<!--HONumber=Nov16_HO3-->


