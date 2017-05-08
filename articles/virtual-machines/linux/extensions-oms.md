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
ms.date: 04/26/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: 05f823955eb5c47ce024c2b7d246e361e1302d78
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="oms-virtual-machine-extension-for-linux"></a>OMS-Azure-VM-Erweiterung für Linux

## <a name="overview"></a>Übersicht

Die Operations Management Suite (OMS) bietet Überwachungs- und Warnungsfunktionen sowie Funktionen zum Beheben von Warnungen für cloudbasierte und lokale Ressourcen. Die OMS-Agent-VM-Erweiterung für Linux wird von Microsoft veröffentlicht und unterstützt. Die Erweiterung installiert den OMS-Agent auf virtuellen Azure-Computern und registriert virtuelle Computer in einem vorhandenen OMS-Arbeitsbereich. Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die OMS-VM-Erweiterung für Linux.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die OMS-Agent-Erweiterung kann für folgende Linux-Distributionen ausgeführt werden:

| Distribution | Version |
|---|---|
| CentOS Linux | 5, 6 und 7 |
| Oracle Linux | 5, 6 und 7 |
| Red Hat Enterprise Linux-Server | 5, 6 und 7 |
| Debian GNU/Linux | 6, 7 und 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 und 12 |

### <a name="internet-connectivity"></a>Internetkonnektivität

Um die OMS-Agent-Erweiterung für Linux verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-schema"></a>Erweiterungsschema

Der folgende JSON-Code zeigt das Schema für die OMS Agent-Erweiterung. Für die Erweiterung werden die Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel aus dem OMS-Zielarbeitsbereich benötigt. Diese Werte sind im OMS-Portal zu finden. Da der Arbeitsbereichsschlüssel als vertrauliche Information behandelt werden muss, sollte er in einer geschützten Einstellungskonfiguration gespeichert werden. Die geschützten Einstellungsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt. Bitte beachten Sie, dass bei **workspaceId** und **workspaceKey** die Groß-/Kleinschreibung beachtet wird.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.3",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| Name | Wert/Beispiel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Herausgeber | Microsoft.EnterpriseCloud.Monitoring |
| Typ | OmsAgentForLinux |
| typeHandlerVersion | 1.3 |
| workspaceId (z.B.) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (z.B.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Vorlagen sind ideal, wenn Sie virtuelle Computer bereitstellen, die nach der Bereitstellung konfiguriert werden müssen (beispielsweise, um sie in OMS zu integrieren). Eine Resource Manager-Beispielvorlage mit der OMS-Agent-VM-Erweiterung finden Sie im [Azure-Schnellstartkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Der JSON-Code für eine Erweiterung des virtuellen Computers kann innerhalb der VM-Ressource geschachtelt oder im Stamm bzw. auf der obersten Ebene einer Resource Manager-JSON-Vorlage platziert werden. Die Platzierung des JSON-Codes wirkt sich auf den Wert von Name und Typ der Ressource aus. Weitere Informationen finden Sie unter [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md) (Festlegen von Name und Typ für untergeordnete Ressourcen). 

Im folgenden Beispiel wird davon ausgegangen, dass die OMS-Erweiterung in der VM-Ressource geschachtelt ist. Beim Schachteln der Ressource für die Erweiterung wird der JSON-Code im `"resources": []`-Objekt des virtuellen Computers platziert.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.3",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Beim Platzieren des JSON-Codes für die Erweiterung im Stamm der Vorlage enthält der Name der Ressource einen Verweis auf die übergeordnete VM, und der Typ spiegelt die geschachtelte Konfiguration wider.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.3",
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

Sie können die OMS-Agent-VM-Erweiterung mithilfe der Azure-Befehlszeilenschnittstelle auf einem vorhandenen virtuellen Computer bereitstellen. Ersetzen Sie den OMS-Schlüssel und die OMS-ID durch die entsprechenden Angaben aus Ihrem OMS-Arbeitsbereich. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.0 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Fehlercodes und ihre Bedeutung

| Fehlercode | Bedeutung | Mögliche Aktion |
| :---: | --- | --- |
| 2 | Für Shellbündel ungültige Option bereitgestellt | |
| 3 | Für Shellbündel keine Option bereitgestellt | |
| 4 | Ungültiger Pakettyp | |
| 5 | Das Shellbündel muss als Root ausgeführt werden | |
| 6 | Ungültige Paketarchitektur | |
| 10 | VM ist bereits mit einem OMS-Arbeitsbereich verbunden | Zum Verbinden der VM mit dem im Erweiterungsschema angegebenen Arbeitsbereich legen Sie „stopOnMultipleConnections“ in den öffentlichen Einstellungen auf FALSE fest, oder entfernen Sie diese Eigenschaft. Diese VM wird für jeden Arbeitsbereich, mit dem Sie verbunden ist, einmal in Rechnung gestellt. |
| 11 | Ungültige Konfiguration der Erweiterung bereitgestellt | Folgen Sie den vorherigen Beispielen, um alle für die Bereitstellung erforderlichen Eigenschaftswerte festzulegen. |
| 20 | Fehler bei der SCX-/OMI-Installation | |
| 21 | Fehler bei der Installation von SCX/Anbieterkits | |
| 22 | Fehler bei der Installation des gebündelten Pakets | |
| 23 | SCX- oder OMI-Paket bereits installiert | |
| 30 | Interner Bündelfehler | |
| 51 | Diese Erweiterung wird vom Betriebssystem der VM nicht unterstützt. | |
| 60 | Nicht unterstützte Version von OpenSSL | Installieren Sie eine Version von OpenSSL, die unsere [Paketanforderungen](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#package-requirements) erfüllt. |
| 61 | Für Python fehlende ctypes-Bibliothek | Installieren Sie die Python-Bibliothek bzw. das Python-Paket ctypes (python-ctypes). |
| 62 | Fehlendes Programm tar | Installieren Sie tar. |
| 63 | Fehlendes Programm sed | Installieren Sie sed. |

Weitere Informationen zur Problembehandlung finden Sie im [Handbuch zur Problembehandlung für den OMS-Agent für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/en-us/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/en-us/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/en-us/support/faq/).

