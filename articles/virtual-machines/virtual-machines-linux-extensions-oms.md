---
title: "OMS-Azure-VM-Erweiterung für Linux | Microsoft-Dokumentation"
description: Stellen Sie den OMS-Agent mithilfe einer VM-Erweiterung auf einem virtuellen Linux-Computer bereit.
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 9f27890e52cb7a9a0d46f2bb84bfe92f7c6fff37
ms.openlocfilehash: 9864314956d79317785c1c2a4bc87621bc6a3e2d


---
# <a name="oms-virtual-machine-extension-for-linux"></a>OMS-Azure-VM-Erweiterung für Linux

## <a name="overview"></a>Übersicht

Die Operations Management Suite (OMS) bietet Überwachungs- und Warnungsfunktionen sowie Funktionen zum Beheben von Warnungen für cloudbasierte und lokale Ressourcen. Die OMS-Agent-VM-Erweiterung für Linux wird von Microsoft veröffentlicht und unterstützt. Die Erweiterung installiert den OMS-Agent auf virtuellen Azure-Computern und registriert virtuelle Computer in einem vorhandenen OMS-Arbeitsbereich. Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die OMS-VM-Erweiterung für Linux.

Allgemeine Informationen zu Azure-VM-Erweiterungen finden Sie in der [Übersicht über VM-Erweiterungen](./virtual-machines-linux-extensions-features.md).

Weitere Informationen zur Operations Management Suite finden Sie in der [Übersicht über die Operations Management Suite](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die OMS-Agent-Erweiterung kann für folgende Linux-Distributionen ausgeführt werden:

| Distribution | Version |
|---|---|
| CentOS Linux | 5, 6 und 7 |
| Oracle Linux | 5, 6 und 7 |
| Red Hat Enterprise Linux-Server | 5, 6 und 7 |
| Debian GNU/Linux | 6, 7 und 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04 |
| SUSE Linux Enterprise Server | 11 und 12 |

### <a name="connectivity"></a>Konnektivität

Um die OMS-Agent-Erweiterung für Linux verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-configuration"></a>Konfiguration der Erweiterung

Für die OMS-Agent-VM-Erweiterung für Linux werden die Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel aus dem OMS-Zielarbeitsbereich benötigt. Da der Arbeitsbereichsschlüssel als vertrauliche Information behandelt werden muss, wird er in einer geschützten Konfiguration gespeichert. Die geschützten Konfigurationsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt. Die öffentliche und die private Konfiguration werden zum Zeitpunkt der Bereitstellung angegeben, wie in den folgenden Abschnitten dieses Dokuments erläutert.

### <a name="public-configuration"></a>Öffentliche Konfiguration

Schema für die öffentliche Konfiguration:

- workspaceId – erforderliche Zeichenfolge: Die ID des OMS-Arbeitsbereichs, in den der virtuelle Computer integriert werden soll.

```json
{
  "workspaceId": "myWorkspaceId"
}
```

### <a name="private-configuration"></a>Private Konfiguration

Schema für die öffentliche Konfiguration:

- workspaceKey – erforderliche Zeichenfolge: Der primäre/sekundäre gemeinsam verwendete Schlüssel des Arbeitsbereichs.

```json
{
  "workspaceKey": "myWorkSpaceKey"
}
```

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Vorlagen sind ideal, wenn Sie virtuelle Computer bereitstellen, die nach der Bereitstellung konfiguriert werden müssen (beispielsweise, um sie in OMS zu integrieren). Eine Resource Manager-Beispielvorlage mit der OMS-Agent-VM-Erweiterung finden Sie im [Azure-Schnellstartkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Das Beispiel kann über dieses Dokument mithilfe der folgenden Schaltfläche bereitgestellt werden:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-oms-extension-ubuntu-vm%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Der JSON-Code zum Bereitstellen der OMS-Agent-VM-Erweiterung sieht wie im folgenden JSON-Beispiel aus:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<extension-deployment-name>",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.0",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Sie können die OMS-Agent-VM-Erweiterung mithilfe der Azure-Befehlszeilenschnittstelle auf einem vorhandenen virtuellen Computer bereitstellen. Erstellen Sie vor dem Bereitstellen der OMS-Agent-Erweiterung eine Datei vom Typ „public.json“ und eine Datei vom Typ „protected.json“. Das Schema für diese Dateien wird weiter oben in diesem Dokument erläutert.

```azurecli
azure vm extension set <resource-group> <vm-name> \
  OmsAgentForLinux Microsoft.EnterpriseCloud.Monitoring 1.0 \
  --public-config-path public.json  \
  --private-config-path protected.json
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```azurecli
azure vm extension get myResourceGroup myVM
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert:

`
/opt/microsoft/omsagent/bin/stdout
`

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/en-us/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/en-us/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/en-us/support/faq/).



<!--HONumber=Dec16_HO1-->


