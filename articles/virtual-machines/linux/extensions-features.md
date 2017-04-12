---
title: "Erweiterungen und Features für virtuelle Computer für Linux | Microsoft-Dokumente"
description: "Sie erhalten einen Überblick über die Erweiterungen für virtuelle Azure-Computer, gruppiert nach den bereitgestellten oder verbesserten Funktionen."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 0182d0d600af691daf8c2ac7a5cb93d7755f61da
ms.lasthandoff: 04/03/2017


---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Informationen zu Erweiterungen und Features für virtuelle Computer für Linux

Erweiterungen für virtuelle Azure-Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z.B. Software auf einem virtuellen Computer installiert werden muss, Virenschutz oder eine Docker-Konfiguration erforderlich ist, kann eine VM-Erweiterung zum Ausführen dieser Aufgaben verwendet werden. Azure VM-Erweiterungen können mithilfe der Azure-Befehlszeilenoberfläche, PowerShell, Azure Resource Manager-Vorlagen und dem Azure-Portal ausgeführt werden. Erweiterungen können mit einer neuen Bereitstellung für virtuelle Computer gebündelt oder in Bezug auf ein bestehendes System ausgeführt werden.

Dieses Dokument enthält eine Übersicht der VM-Erweiterungen, erläutert Voraussetzungen für die Verwendung von Azure VM-Erweiterungen und bietet Hilfestellung beim Erkennen, Verwalten und Entfernen von VM-Erweiterungen. Dieses Dokument enthält verallgemeinerte Informationen, da eine Vielzahl von VM-Erweiterungen verfügbar ist, die alle eine potenziell eigene Konfiguration aufweisen. Erweiterungsspezifische Details finden Sie in der für die jeweilige Erweiterung spezifischen Dokumentation.

## <a name="use-cases-and-samples"></a>Anwendungsfälle und Beispiele

Es sind verschiedene Azure VM-Erweiterungen für jeweils spezifische Anwendungsfälle verfügbar. Hier einige Beispiele:

- Anwenden von gewünschten Statuskonfigurationen mit PowerShell auf einen virtuellen Computer mithilfe der DSC-Erweiterung für Linux. Weitere Informationen finden Sie unter [Azure Desired State configuration extension (Azure-Erweiterung für die gewünschte Statuskonfiguration)](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Konfigurieren der Überwachung eines virtuellen Computers mit der Microsoft Monitoring Agent-VM-Erweiterung. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivierung der Überwachung von virtuellen Computern](vm-monitoring.md).
- Konfigurieren der Überwachung Ihrer Azure-Infrastruktur mit der Datadog-Erweiterung. Weitere Informationen finden Sie im [Datadog-Blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Konfigurieren eines Docker-Hosts auf einem virtuellen Azure-Computer mithilfe der Docker-VM-Erweiterung. Weitere Informationen finden Sie unter [Docker-VM-Erweiterung](dockerextension.md).

Über prozessspezifische Erweiterungen hinaus ist sowohl für virtuelle Windows- als auch für virtuelle Linux-Computer eine benutzerdefinierte Skripterweiterung verfügbar. Die benutzerdefinierte Skripterweiterung für Linux ermöglicht die Ausführung beliebiger Bash-Skripts auf virtuellen Computern. Benutzerdefinierte Skripts sind beim Entwerfen von Azure-Bereitstellungen nützlich, die Konfiguration über das Maß hinaus erfordern, das mithilfe von Azure-Tools erreicht werden kann. Weitere Informationen finden Sie unter [Benutzerdefinierte Skripterweiterung für Linux-VMs](extensions-customscript.md).

Wenn Sie ein Beispiel für den Einsatz einer VM-Erweiterung in einer End-to-End-Anwendungsbereitstellung durcharbeiten möchten, lesen Sie [Automatisieren von Anwendungsbereitstellungen auf virtuellen Azure-Computern](../windows/dotnet-core-1-landing.md).

## <a name="prerequisites"></a>Voraussetzungen

Für jede VM-Erweiterung gilt möglicherweise ein eigener Satz Voraussetzungen. Beispielsweise setzt die Docker-VM-Erweiterung eine unterstützte Linux-Distribution voraus. Die Anforderungen der einzelnen Erweiterungen werden ausführlich in der erweiterungsspezifischen Dokumentation behandelt.

### <a name="azure-vm-agent"></a>Azure-VM-Agent

Der Azure VM-Agent verwaltet Interaktionen zwischen einem virtuellen Azure-Computer und dem Azure Fabric Controller. Der VM-Agent ist für viele funktionale Aspekte in Bezug auf die Bereitstellung und Verwaltung virtueller Azure-Computer verantwortlich. Dies umfasst auch das Ausführen von VM-Erweiterungen. Der Azure-VM-Agent ist in Images aus dem Azure Marketplace vorinstalliert und kann manuell auf unterstützten Betriebssystemen installiert werden.

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie unter [Informationen zum Agent und zu Erweiterungen für virtuelle Computer](../windows/classic/agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Ermitteln von VM-Erweiterungen

Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. Wenn Sie eine vollständige Liste erhalten möchten, führen Sie mit der Azure-Befehlszeilenoberfläche den folgenden Befehl aus. Ersetzen Sie den Beispielspeicherort dabei durch den gewünschten Speicherort.

```azurecli
azure vm extension-image list westus
```

## <a name="run-vm-extensions"></a>Ausführen von VM-Erweiterungen

Azure VM-Erweiterungen können auf vorhandenen virtuellen Computern ausgeführt werden, was nützlich ist, um Konfigurationsänderungen vorzunehmen oder die Konnektivität für eine bereits bereitgestellte VM wiederherzustellen. VM-Erweiterungen können darüber hinaus mit der Bereitstellung von Azure Resource Manager-Vorlagen gekoppelt werden. Durch die Verwendung von Erweiterungen mit Resource Manager-Vorlagen können virtuelle Azure--Computer bereitgestellt und konfiguriert werden, ohne dass Eingriffe nach der Bereitstellung erforderlich werden.

Die folgenden Methoden können verwendet werden, um eine Erweiterung für einen vorhandenen virtuellen Computer auszuführen.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Azure VM-Erweiterungen können mithilfe des `azure vm extension set`-Befehls für einen vorhandenen virtuellen Computer ausgeführt werden. In diesem Beispiel wird die benutzerdefinierte Skripterweiterung für einen virtuellen Computer ausgeführt.

```azurecli
azure vm extension set myResourceGroup myVM CustomScript Microsoft.Azure.Extensions 2.0 \
  --auto-upgrade-minor-version \
  --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Das Skript erzeugt eine Ausgabe ähnlich wie bei folgendem Text:

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure-Portal

VM-Erweiterungen können mithilfe des Azure-Portals auf einen vorhandenen virtuellen Computer angewendet werden. Wählen Sie zu diesem Zweck den virtuellen Computer aus, wählen Sie **Erweiterungen** aus, und klicken Sie auf **Hinzufügen**. Wählen Sie die Erweiterung aus, die Sie aus der Liste verfügbarer Erweiterungen erhalten, und befolgen Sie die Anweisungen im Assistenten.

Das folgende Bild zeigt die Installation der benutzerdefinierten Linux-Skripterweiterung aus dem Azure-Portal.

![Installieren benutzerdefinierter Skripterweiterung](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

VM-Erweiterungen können einer Azure Resource Manager-Vorlage hinzugefügt und mit der Bereitstellung der Vorlage ausgeführt werden. Wenn Sie eine Erweiterung mithilfe einer Vorlage bereitstellen, können Sie vollständig konfigurierte Azure-Bereitstellungen erstellen. Beispielsweise stammt der folgende JSON-Code aus einer Resource Manager-Vorlage. Die Vorlage stellt einen Satz von virtuellen Computern mit Lastenausgleich und einer Azure SQL-Datenbank bereit und installiert dann auf jedem virtuellen Computer eine .NET Core-Anwendung. Die VM-Erweiterung erledigt die Softwareinstallation.

Weitere Informationen finden Sie in der vollständigen [Resource Manager-Vorlage](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Weitere Informationen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen mit Linux-VM-Erweiterungen](../windows/extensions-authoring-templates.md).

## <a name="secure-vm-extension-data"></a>Schützen der Daten von VM-Erweiterungen

Beim Ausführen einer VM-Erweiterung ist es möglicherweise erforderlich, vertrauliche Informationen, wie Anmeldeinformationen, Namen von Speicherkonten und Zugriffsschlüssel von Speicherkonten mit aufzunehmen. Viele VM-Erweiterungen beinhalten eine geschützte Konfiguration, die Daten verschlüsselt und sie ausschließlich innerhalb des virtuellen Zielcomputers entschlüsselt. Jede Erweiterung weist ein spezifisches Schema für die geschützte Konfiguration auf, und jede wird in der erweiterungsspezifischen Dokumentation ausführlich erläutert.

Das folgende Beispiel zeigt eine Instanz der benutzerdefinierten Skripterweiterung für Linux. Beachten Sie, dass der auszuführende Befehl einen Satz Anmeldeinformationen enthält. In diesem Beispiel wird der auszuführende Befehl nicht verschlüsselt.


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Durch Verschieben der **CommandToExecute**-Eigenschaft in die **protected**-Konfiguration wird die Ausführungszeichenfolge geschützt.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a>Problembehandlung bei VM-Erweiterungen

Jede VM-Erweiterung kann für die Erweiterung spezifische Schritte zur Problembehandlung aufweisen. Wenn Sie beispielsweise die benutzerdefinierte Skripterweiterung verwenden, finden Sie Details zur Skriptausführung lokal auf dem virtuellen Computer, auf dem die Erweiterung ausgeführt wurde. Alle erweiterungsspezifischen Schritte zur Problembehandlung sind ausführlich in der Dokumentation zur Erweiterung erläutert.

Die folgenden Schritte zur Problembehandlung gelten für alle VM-Erweiterungen.

### <a name="view-extension-status"></a>Anzeigen des Erweiterungsstatus

Verwenden Sie nach dem Anwenden einer VM-Erweiterung auf einen virtuellen Computer den folgenden Azure-Befehlszeilenbefehl zum Zurückgeben des Erweiterungsstatus. Ersetzen Sie die Namen der Beispielparameter durch Ihre eigenen Werte.

```azurecli
azure vm extension get myResourceGroup myVM
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "myVM"
data:    Publisher                   Name             Version  State
data:    --------------------------  ---------------  -------  ---------
data:    Microsoft.Azure.Extensions  DockerExtension  1.0      Succeeded
info:    vm extension get command OK         :
```

Der Ausführungsstatus von Erweiterungen findet sich ebenfalls im Azure-Portal. Um den Status einer Erweiterung anzuzeigen, wählen Sie den virtuellen Computer aus, wählen Sie **Erweiterungen** und dann die gewünschte Erweiterung aus.

### <a name="rerun-a-vm-extension"></a>Erneutes Ausführen einer VM-Erweiterung

In manchen Fällen kann die erneute Ausführung einer VM-Erweiterung erforderlich sein. Sie können eine Erweiterung erneut ausführen, indem Sie sie entfernen und die Erweiterung dann mit einer Ausführungsmethode Ihrer Wahl erneut ausführen. Um eine Erweiterung zu entfernen, führen Sie den folgenden Befehl in der Azure-Befehlszeile aus. Ersetzen Sie die Namen der Beispielparameter durch Ihre eigenen Werte.

```azurecli
azure vm extension set myResourceGroup myVM --uninstall CustomScript Microsoft.Azure.Extensions 2.0
```

Im Azure-Portal können Sie eine Erweiterung mithilfe der folgenden Schritte entfernen:

1. Wählen Sie einen virtuellen Computer aus.
2. Wählen Sie **Erweiterungen** aus.
3. Wählen Sie die gewünschte Erweiterung aus.
4. Wählen Sie **Deinstallieren** aus.

## <a name="common-vm-extension-reference"></a>Allgemeine VM-Erweiterungsreferenz
| Name der Erweiterung | Beschreibung | Weitere Informationen |
| --- | --- | --- |
| Benutzerdefinierte Skripterweiterung für Linux |Führt Skripts auf einem virtuellen Azure-Computer aus |[Benutzerdefinierte Skripterweiterung für Linux](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Docker-Erweiterung |Installiert den Docker-Daemon zur Unterstützung von Docker-Remotebefehlen. |[Docker-VM-Erweiterung](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Erweiterungen für den Zugriff auf virtuelle Computer |Wiedererlangen des Zugriffs auf einen virtuellen Azure-Computer |[Erweiterungen für den Zugriff auf virtuelle Computer](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure-Diagnoseerweiterung |Verwalten der Azure-Diagnose |[Azure-Diagnoseerweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Erweiterung für den Zugriff auf virtuelle Azure-Computer |Verwalten von Benutzern und Anmeldeinformationen |[Erweiterungen für den Zugriff auf virtuelle Computer für Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

