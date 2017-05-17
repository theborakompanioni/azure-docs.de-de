---
title: "Bewährte Methoden für das Erstellen von Resource Manager-Vorlagen | Microsoft-Dokumentation"
description: "Leitlinien für die Vereinfachung Ihrer Azure Resource Manager-Vorlagen."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 24c2b27948e9373bc3957f706ed802cc36c04148
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen
Anhand dieser Leitfäden können Sie zuverlässige und benutzerfreundliche Azure Resource Manager-Vorlagen erstellen. Bei diesen Richtlinien handelt es sich lediglich um Vorschläge. Sie müssen nicht zwingend eingehalten werden (es sei denn, es ist etwas anderes angegeben). Für Ihr Szenario muss einer der folgenden Ansätze oder eines der folgenden Beispiel unter Umständen angepasst werden.

## <a name="resource-names"></a>Ressourcennamen
Im Allgemeinen arbeiten Sie in Resource Manager mit drei Arten von Ressourcennamen:

* Ressourcennamen, die eindeutig sein müssen.
* Ressourcennamen, die nicht eindeutig sein müssen. Sie möchten jedoch einen Namen angeben, der Sie beim Ermitteln eines ressourcenbasierten Kontexts unterstützt.
* Ressourcennamen, die allgemein sein können.

Hilfe zum Festlegen einer Benennungskonvention finden Sie unter [Benennungsrichtlinien für die Azure-Infrastruktur für Windows-VMs](../virtual-machines/windows/infrastructure-naming-guidelines.md). Informationen zu Einschränkungen für Ressourcennamen finden Sie unter [Empfohlene Benennungskonventionen für Azure-Ressourcen](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Eindeutige Ressourcennamen
Sie müssen einen eindeutigen Ressourcennamen für alle Ressourcentypen angeben, die einen Datenzugriffsendpunkt haben. Es folgen allgemeine Ressourcentypen, für die ein eindeutiger Name erforderlich ist:

* Azure Storage<sup>1</sup> 
* Web-Apps-Funktion von Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup>Für Namen von Speicherkonten gilt darüber hinaus Folgendes: Kleinbuchstaben, max. 24 Zeichen, keine Bindestriche.

Wenn Sie einen Parameter für einen Ressourcennamen angeben, müssen Sie beim Bereitstellen der Ressource einen eindeutigen Namen angeben. Optional können Sie eine Variable erstellen, die mit der [uniqueString()](resource-group-template-functions-string.md#uniquestring)-Funktion einen Namen generiert. 

Sie können auch ein Präfix oder Suffix zum **uniqueString**-Ergebnis hinzufügen. Wenn Sie den eindeutigen Namen ändern, können Sie den Ressourcentyp einfacher aus dem Namen ableiten. Beispielsweise können Sie einen eindeutigen Namen für ein Speicherkonto mit der folgenden Variablen generieren:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Ressourcennamen zur Identifizierung
Einige Ressourcentypen möchten Sie vielleicht benennen, ihre Namen müssen jedoch nicht eindeutig sein. Für diese Ressourcentypen können Sie einen Namen festlegen, der sowohl den Ressourcenkontext als auch den Ressourcentyp angibt. Geben Sie einen beschreibenden Namen an, der Ihnen die Identifizierung der Ressource in einer Liste von Ressourcen erleichtert. Wenn Sie einen anderen Ressourcennamen für verschiedene Bereitstellungen verwenden müssen, können Sie einen Parameter für den Namen einsetzen:

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

Wenn Sie während der Bereitstellung keinen Namen übergeben müssen, können Sie eine Variable verwenden: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

Sie können auch einen hartcodierten Wert verwenden:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a>Allgemeine Ressourcennamen
Für Ressourcentypen, auf die hauptsächlich über eine andere Ressource zugegriffen wird, können Sie einen allgemeinen Namen wählen, der in der Vorlage hartcodiert ist. Beispielsweise können Sie einen allgemeinen Standardnamen für Firewallregeln auf einer SQL Server-Instanz festlegen:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a>Parameter
Die folgenden Informationen können bei der Verwendung von Parametern hilfreich sein:

* Verwenden Sie möglichst wenig Parameter. Verwenden Sie nach Möglichkeit eine Variable oder einen Literalwert. Verwenden Sie Parameter nur in den folgenden Szenarien:
   
   * Einstellungen, die sie an die Umgebung (SKU, Größe, Kapazität) anpassen möchten.
   * Ressourcennamen, die Sie zur leichteren Identifizierung angeben möchten
   * Werte, die Sie häufig nutzen, um andere Aufgaben zu erledigen (z.B. Benutzername des Administrators)
   * Geheimnisse (z.B. Kennwörter)
   * Anzahl oder Array von Werten, die beim Erstellen mehrerer Instanzen eines Ressourcentyps verwendet werden
* Verwenden Sie die camel-Schreibweise für Parameternamen.
* Geben Sie für jeden Parameter in den Metadaten eine Beschreibung an.

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Definieren Sie Standardwerte für Parameter (außer für Kennwörter und SSH-Schlüssel):
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Verwenden Sie **securestring** für alle Kennwörter und Geheimnisse: 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Verwenden Sie möglichst keinen Parameter zum Angeben eines Speicherorts. Verwenden Sie stattdessen die **location**-Eigenschaft der Ressourcengruppe. Mithilfe des Ausdrucks **resourceGroup().location** für alle Ihre Ressourcen werden Ressourcen in der Vorlage am selben Speicherort wie die Ressourcengruppe bereitgestellt:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Wenn ein Ressourcentyp nur von einer begrenzten Anzahl von Standorten unterstützt wird, können Sie einen gültigen Speicherort direkt in der Vorlage angeben. Wenn Sie einen **location**-Parameter verwenden müssen, nutzen Sie diesen Parameterwert nach Möglichkeit so umfassend wie möglich mit Ressourcen gemeinsam, die sich wahrscheinlich am selben Speicherort befinden. Dadurch werden Benutzer weniger häufig zur Angabe von Speicherortinformationen aufgefordert.
* Vermeiden Sie das Verwenden eines Parameters oder einer Variablen für die API-Version eines Ressourcentyps. Ressourceneigenschaften und -werte können je nach Versionsnummer variieren. Mithilfe von IntelliSense kann in Code-Editoren nicht das richtige Schema ermittelt werden, wenn die API-Version auf einen Parameter oder eine Variable festgelegt ist. Stattdessen sollten Sie die API-Version in der Vorlage hartcodieren.

## <a name="variables"></a>Variables
Die folgenden Informationen können bei der Verwendung von Variablen hilfreich sein:

* Verwenden Sie Variablen für Werte, die Sie mehr als einmal in einer Vorlage verwenden müssen. Wenn ein Wert nur einmal verwendet wird, erleichtert ein hartcodierter Wert das Lesen Ihrer Vorlage.
* Im Abschnitt **variables** können Sie die [reference](resource-group-template-functions-resource.md#reference)-Funktion nicht nutzen. Die **reference**-Funktion leitet ihren Wert aus dem Laufzeitstatus der Ressource ab. Variablen werden jedoch während der ersten Analyse der Vorlage aufgelöst. Erstellen Sie Werte, die die **reference**-Funktion direkt in den Abschnitten **resources** oder **outputs** der Vorlage benötigen.
* Beziehen Sie Variablen für Ressourcennamen ein, die eindeutig sein müssen (siehe [Ressourcennamen](#resource-names)).
* Sie können Variablen in komplexen Objekten gruppieren. Sie können auf einen Wert in einem komplexen Objekt im Format **variable.subentry** verweisen. Durch die Gruppierung von Variablen können Sie verwandte Variablen nachverfolgen. Dadurch wird auch die Lesbarkeit der Vorlage verbessert. Hier sehen Sie ein Beispiel:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Ein komplexes Objekt darf keinen Ausdruck enthalten, der auf einen Wert in einem komplexen Objekt verweist. Definieren Sie zu diesem Zweck eine gesonderte Variable.
   > 
   > 
   
     Fortgeschrittenere Beispiele für das Verwenden komplexer Objekte als Variablen finden Sie unter [Freigeben des Status für und aus Azure Resource Manager-Vorlagen](best-practices-resource-manager-state.md).

## <a name="resources"></a>Ressourcen
Die folgenden Informationen können bei der Verwendung von Ressourcen hilfreich sein:

* Geben Sie in der Vorlage **Kommentare** für jede Ressource ein, damit andere Mitwirkende den Zweck der Ressource verstehen.
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Sie können Tags verwenden, um Ressourcen Metadaten hinzuzufügen. Fügen Sie Informationen über Ihre Ressourcen mithilfe von Metadaten hinzu. Beispielsweise können Sie für eine Ressource Metadaten zu Abrechnungsdetails hinzufügen. Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
* Wenn Sie einen *öffentlichen Endpunkt* in Ihrer Vorlage verwenden (z.B. in Form eines Azure-Blobspeichers), dürfen Sie den Namespace *nicht hartcodieren*. Verwenden Sie die **reference**-Funktion, um den Namespace dynamisch abzurufen. Mit diesem Ansatz können Sie die Vorlage in anderen öffentlichen Namespace-Umgebungen bereitstellen, ohne den Endpunkt in der Vorlage manuell zu ändern. Legen Sie die API-Version auf die Version fest, die Sie für das Speicherkonto in der Vorlage verwenden:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Wenn das Speicherkonto in der Vorlage bereitgestellt wird, die Sie gerade erstellen, müssen Sie beim Verweisen auf die Ressource nicht den Namespace des Anbieters angeben. Hier sehen Sie die vereinfachte Syntax:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Wenn die Vorlage andere Werte enthält, die für die Verwendung eines öffentlichen Namespace konfiguriert sind, ändern Sie diese Werte entsprechend der **reference**-Funktion. Beispiel: Sie können die **storageUri**-Eigenschaft des Diagnoseprofils des virtuellen Computers festlegen:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Sie können auch auf ein in einer anderen Ressourcengruppe vorhandenes Speicherkonto verweisen:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Weisen Sie einem virtuellen Computer nur dann öffentliche IP-Adressen zu, wenn dies für eine Anwendung erforderlich ist. Verwenden Sie zum Herstellen einer Verbindung mit einem virtuellen Computer für das Debuggen, die Verwaltung oder administrative Zwecke entweder NAT-Eingangsregeln, ein Gateway für virtuelle Netzwerke oder eine Jumpbox.
   
     Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Computern finden Sie unter:
   
   * [Run Windows VMs for an N-tier application](../guidance/guidance-compute-n-tier-vm.md) (Ausführen virtueller Windows-Computer in einer Architektur mit n Ebenen in Azure)
   * [Einrichten des Zugriffs auf WinRM für virtuelle Computer in Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe des Azure-Portals](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Öffnen von Ports und Endpunkten für einen virtuellen Computer in Azure mithilfe von PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Öffnen von Ports und Endpunkten für einen virtuellen Linux-Computer mithilfe der Azure CLI](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* Die Eigenschaft **domainNameLabel** für öffentliche IP-Adressen muss eindeutig sein. Der Wert **domainNameLabel** muss 3 bis 63 Zeichen lang sein und den Regeln des regulären Ausdrucks `^[a-z][a-z0-9-]{1,61}[a-z0-9]$` entsprechen. Da die **uniqueString**-Funktion eine Zeichenfolge mit 13 Zeichen erstellt, ist der **dnsPrefixString**-Parameter auf 50 Zeichen beschränkt:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Verwenden Sie beim Hinzufügen eines Kennworts zu einer benutzerdefinierten Skripterweiterung die **commandToExecute**-Eigenschaft in **protectedSettings**:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Zum Sicherzustellen der Verschlüsselung von Geheimnissen, die als Parameter an virtuelle Computer und Erweiterungen übergeben werden, verwenden Sie die **protectedSettings**-Eigenschaft der entsprechenden Erweiterungen.
   > 
   > 

## <a name="outputs"></a>Ausgaben
Wenn Sie öffentliche IP-Adressen mithilfe einer Vorlage erstellen, sollte diese den Abschnitt **output** enthalten, der Details zur IP-Adresse und den vollständig qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) zurückgibt. Mit Ausgabewerten können Sie nach der Bereitstellung ganz einfach Details zu öffentlichen IP-Adressen und FQDNs abrufen. Beim Verweisen auf die Ressource verwenden Sie die API-Version, die Sie zu ihrer Erstellung verwendet haben: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Einzelvorlage im Vergleich zu geschachtelten Vorlagen
Um Ihre Lösung bereitzustellen, können Sie entweder eine Einzelvorlage oder eine Hauptvorlage mit mehreren geschachtelten Vorlagen verwenden. Geschachtelte Vorlagen sind für komplexere Szenarien üblich. Die Verwendung einer geschachtelten Vorlage bietet folgende Vorteile:

* Eine Lösung kann in Zielkomponenten unterteilt werden.
* Geschachtelte Vorlagen können mit anderen Hauptvorlagen erneut verwendet werden.

Wenn Sie sich für die Verwendung geschachtelter Vorlagen entscheiden, unterstützen die folgenden Richtlinien Sie beim Standardisieren des Vorlagenentwurfs. Diese Leitlinien basieren auf dem Artikel [Entwurfsmuster für Azure Resource Manager-Vorlagen bei der Bereitstellung komplexer Lösungen](best-practices-resource-manager-design-templates.md). Wir empfehlen einen Entwurf mit folgenden Vorlagen:

* **Hauptvorlage** (azuredeploy.json). Wird für die Eingabeparameter verwendet.
* **Vorlage für freigegebene Ressourcen** Wird für die Bereitstellung von freigegebenen Ressourcen verwendet, die von allen anderen Ressourcen (z.B. dem virtuellen Netzwerk und Verfügbarkeitsgruppen) verwendet werden. Stellen Sie mithilfe des Ausdrucks **dependsOn** sicher, dass diese Vorlage vor den anderen Vorlagen bereitgestellt wird.
* **Vorlage für optionale Ressourcen** Dient zum bedingten Bereitstellen von Ressourcen basierend auf einem Parameter (z.B. einer Jumpbox).
* **Vorlage für Memberknotenressourcen** Jeder Instanztyp auf einer Anwendungsebene verfügt über eine eigene Konfiguration. Innerhalb einer Ebene können Sie verschiedene Instanztypen definieren. (Beispiel: Die erste Instanz erstellt einen Cluster, und dem vorhandenen Cluster werden zusätzliche Instanzen hinzugefügt.) Jeder Instanztyp verfügt über eine eigene Bereitstellungsvorlage.
* **Skripts** Umfassend wiederverwendbare Skripts gibt es für jeden Instanztyp (z. B. zum Initialisieren und Formatieren zusätzlicher Datenträger). Für spezifische Anpassungszwecke erstellte benutzerdefinierte Skripts unterscheiden sich je nach Instanztyp.

![Geschachtelte Vorlage](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-templates"></a>Bedingtes Verknüpfen mit geschachtelten Vorlagen
Für das bedingte Verknüpfen mit geschachtelten Vorlagen kann ein Parameter verwendet werden. Der Parameter wird Teil des URI für die Vorlage:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Vorlagenformat
Es empfiehlt sich, Ihre Vorlage einer JSON-Gültigkeitsprüfung zu unterziehen. Bei dieser Überprüfung können überflüssige Kommas und runde sowie eckige Klammern entfernt werden, die während der Bereitstellung einen Fehler verursachen könnten. Nutzen Sie beispielsweise [JSONlint](http://jsonlint.com/) oder ein Linter-Paket für Ihre bevorzugte Bearbeitungsumgebung (Visual Studio Code, Atom, Sublime Text, Visual Studio).

Es ist auch eine gute Idee, Ihren JSON-Code für eine bessere Lesbarkeit zu formatieren. Sie können das JSON-Formatierungsprogramm für Ihren lokalen Editor verwenden. In Visual Studio formatieren Sie das Dokument durch Drücken von STRG+K, STRG+D****. Drücken Sie in Visual Studio Code ALT+UMSCHALT+F****. Wenn der lokale Editor das Dokument nicht formatiert, können Sie ein [online verfügbares Formatierungsprogramm](https://www.bing.com/search?q=json+formatter)verwenden.

## <a name="next-steps"></a>Nächste Schritte
* Eine Anleitung zum Gestalten Ihrer Lösung für virtuelle Computer finden Sie unter [Run a Windows VM on Azure](../guidance/guidance-compute-single-vm.md) (Ausführen eines virtuellen Windows-Computers in Azure) und [Run a Linux VM on Azure](../guidance/guidance-compute-single-vm-linux.md) (Ausführen eines virtuellen Linux-Computers in Azure).
* Eine Anleitung zum Einrichten eines Speicherkontos finden Sie unter [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage/storage-performance-checklist.md).
* Hilfe zu virtuellen Netzwerken finden Sie unter [Richtlinien für die Azure-Netzwerkinfrastruktur für Windows-VMs](../virtual-machines/windows/infrastructure-networking-guidelines.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst – präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


