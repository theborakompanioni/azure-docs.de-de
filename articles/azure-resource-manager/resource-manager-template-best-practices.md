---
title: "Bewährte Methoden für Resource Manager-Vorlagen | Microsoft Docs"
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
ms.date: 10/25/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: 049436b35346c355509398f7bb105dc1511eb8f7


---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen
Anhand der folgenden Leitfäden können Sie zuverlässige und benutzerfreundliche Resource Manager-Vorlagen erstellen. Diese Leitlinien dienen nur als Vorschläge und nicht als absolute Anforderungen. Für Ihr Szenario sind ggf. Abweichungen von diesen Leitlinien erforderlich.

## <a name="resource-names"></a>Ressourcennamen
Im Allgemeinen arbeiten Sie mit drei Arten von Ressourcennamen:

1. Ressourcennamen, die eindeutig sein müssen.
2. Ressourcennamen, die nicht eindeutig sein müssen, wobei Sie jedoch einen Namen angeben möchten, der beim Bestimmen des Kontexts hilft.
3. Ressourcennamen, die allgemein sein können.

Hilfe zum Festlegen einer Benennungskonvention finden Sie unter [Benennungsrichtlinien für die Infrastruktur](../virtual-machines/virtual-machines-windows-infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informationen zu Einschränkungen für Ressourcennamen finden Sie unter [Empfohlene Benennungskonventionen für Azure-Ressourcen](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Eindeutige Ressourcennamen
Sie müssen einen eindeutigen Ressourcennamen für alle Ressourcentypen angeben, die einen Datenzugriffsendpunkt haben. Es folgen allgemeine Typen, für die ein eindeutiger Name erforderlich ist:

* Speicherkonto
* Website
* SQL Server
* Schlüsseltresor
* Redis-Cache
* Batch-Konto
* Traffic Manager
* Suchdienst
* HDInsight-Cluster

Darüber hinaus gilt für Namen von Speicherkonten Folgendes: Kleinbuchstaben, max. 24 Zeichen, keine Bindestriche.

Wenn Sie einen Parameter für diese Ressourcennamen angeben, müssen Sie während der Bereitstellung einen eindeutigen Namen raten. Stattdessen können Sie eine Variable erstellen, die mit der [uniqueString()](resource-group-template-functions.md#uniquestring)-Funktion einen Namen generiert. Häufig empfiehlt sich auch, dem Ergebnis von **uniqueString** ein Präfix oder Postfix hinzuzufügen, damit Sie den Ressourcentyp beim Untersuchen des Namens einfacher bestimmen können. Beispielsweise generieren Sie einen eindeutigen Namen für ein Speicherkonto mit der folgenden Variablen:

    "variables": {
        "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
    }

Speicherkonten mit einem „uniqueString“-Präfix werden nicht in denselben Kategorien gruppiert.

### <a name="resource-names-for-identification"></a>Ressourcennamen zur Identifizierung
Für Ressourcentypen, die Sie benennen möchten, ohne Eindeutigkeit gewährleisten zu müssen, geben Sie einen Namen an, der sowohl den Kontext als auch Ressourcentyp identifiziert. Sie sollten einen beschreibenden Namen bereitstellen, anhand dessen Sie ihn in einer Liste mit Ressourcennamen erkennen können. Wenn Sie den Namen der Ressource im Verlauf von Bereitstellungen variieren möchten, verwenden Sie einen Parameter für den Namen:

    "parameters": {
        "vmName": { 
            "type": "string",
            "defaultValue": "demoLinuxVM",
            "metadata": {
                "description": "The name of the VM to create."
            }
        }
    }

Wenn Sie während der Bereitstellung keinen Namen übergeben müssen, verwenden Sie eine Variable: 

    "variables": {
        "vmName": "demoLinuxVM"
    }

Oder einen hartcodierten Wert:

    {
      "type": "Microsoft.Compute/virtualMachines",
      "name": "demoLinuxVM",
      ...
    }

### <a name="generic-resource-names"></a>Allgemeine Ressourcennamen
Für Ressourcentypen, auf die hauptsächlich über eine andere Ressource zugegriffen wird, können Sie einen allgemeinen Namen wählen, der in der Vorlage hartcodiert ist. Sie möchten beispielsweise wahrscheinlich keinen anpassbaren Namen für die Firewallregeln eines SQL-Servers angeben.

    {
        "type": "firewallrules",
        "name": "AllowAllWindowsAzureIps",
        ...
    }

## <a name="parameters"></a>Parameter
1. Minimieren Sie Parameter nach Möglichkeit. Falls möglich, sollten Sie eine Variable oder ein Literal verwenden. Geben Sie Parameter nur für Folgendes an:
   
   * Die Einstellungen, die Sie je nach Umgebung variieren möchten (z.B. SKU, Größe oder Kapazität).
   * Ressourcennamen, die Sie zur leichteren Identifizierung angeben möchten.
   * Werte, die Sie häufig nutzen, um andere Aufgaben zu erledigen (z.B. Benutzername des Administrators).
   * Geheime Schlüssel (z.B. Kennwörter)
   * Anzahl oder Array von Werten, die beim Erstellen mehrerer Instanzen eines Ressourcentyps verwendet werden.
2. Parameternamen müssen die **lowerCamel**-Schreibweise aufweisen.
3. Geben Sie für jeden Parameter in den Metadaten eine Beschreibung an.
   
        "parameters": {
            "storageAccountType": {
                "type": "string",
                "metadata": {
                    "description": "The type of the new storage account created to store the VM disks"
                }
            }
        }
4. Definieren Sie Standardwerte für Parameter (außer für Kennwörter und SSH-Schlüssel).
   
        "parameters": {
            "storageAccountType": {
                "type": "string",
                "defaultValue": "Standard_GRS",
                "metadata": {
                    "description": "The type of the new storage account created to store the VM disks"
                }
            }
        }
5. Verwenden Sie **securestring** für alle Kennwörter und geheimen Schlüssel. 
   
        "parameters": {
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        }
6. Vermeiden Sie nach Möglichkeit einen Parameter zum Angeben des **Speicherorts**. Verwenden Sie stattdessen die „location“-Eigenschaft der Ressourcengruppe. Mithilfe des Ausdrucks **resourceGroup().location** für alle Ihre Ressourcen werden die Ressourcen in der Vorlage am selben Speicherort wie die Ressourcengruppe bereitgestellt.
   
        "resources": [
          {
              "name": "[variables('storageAccountName')]",
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              ...
          }
        ]
   
     Wenn ein Ressourcentyp nur von einer begrenzten Anzahl von Standorten unterstützt wird, erwägen Sie, einen gültigen Speicherort direkt in der Vorlage anzugeben. Wenn Sie einen „location“-Parameter verwenden müssen, nutzen Sie diesen Parameterwert nach Möglichkeit so umfassend wie möglich mit Ressourcen gemeinsam, die sich wahrscheinlich am selben Speicherort befinden. Bei diesem Ansatz wird der Aufwand seitens der Benutzer zum Angeben von Speicherorten für jeden Ressourcentyp minimiert.
7. Vermeiden Sie das Verwenden eines Parameters oder einer Variablen für die API-Version eines Ressourcentyps. Ressourceneigenschaften und -werte können je nach Versionsnummer variieren. Mithilfe von IntelliSense kann in Code-Editoren nicht das richtige Schema ermittelt werden, wenn die API-Version auf einen Parameter oder eine Variable festgelegt ist. Stattdessen sollten Sie die API-Version in der Vorlage hartcodieren.

## <a name="variables"></a>Variablen
1. Verwenden Sie Variablen für Werte, die Sie mehr als einmal in einer Vorlage verwenden müssen. Wenn ein Wert nur einmal verwendet wird, erleichtert ein hartcodierter Wert das Lesen Ihrer Vorlage.
2. Im Abschnitt mit den Variablen können Sie die [reference](resource-group-template-functions.md#reference)-Funktion nicht nutzen. Die „reference“-Funktion leitet ihren Wert vom Laufzeitstatus der Ressource ab. Variablen werden dagegen während der erstmaligen Analyse der Vorlage aufgelöst. Erstellen Sie stattdessen Werte, die die **reference**-Funktion direkt in den Abschnitten **resources** oder **outputs** der Vorlage benötigen.
3. Beziehen Sie Variablen für Ressourcennamen ein, die eindeutig sein müssen (siehe [Ressourcennamen](#resource-names)).
4. Sie können Variablen in komplexen Objekten gruppieren. Sie können auf einen Wert in einem komplexen Objekt im Format **variable.subentry**verweisen. Das Gruppieren von Variablen hilft beim Nachverfolgen verwandter Variablen und verbessert die Lesbarkeit der Vorlage.
   
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
   
   > [!NOTE]
   > Ein komplexes Objekt darf keinen Ausdruck enthalten, der auf einen Wert in einem komplexen Objekt verweist. Definieren Sie zu diesem Zweck eine gesonderte Variable.
   > 
   > 
   
     Weitere Beispiele für das Verwenden komplexer Objekte als Variablen finden Sie unter [Freigeben des Status in Azure Resource Manager-Vorlagen](best-practices-resource-manager-state.md).

## <a name="resources"></a>Ressourcen
1. Geben Sie in der Vorlage **Kommentare** für jede Ressource ein, damit andere Mitwirkende den Zweck der Ressource verstehen.
   
        "resources": [
          {
              "name": "[variables('storageAccountName')]",
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              "comments": "This storage account is used to store the VM disks",
              ...
          }
        ]
2. Verwenden Sie Tags, um Metadaten zu Ressourcen hinzuzufügen, mit denen Sie Ihre Ressourcen durch weitere Informationen ergänzen können. Beispielsweise können Sie einer Ressource Metadaten zu Abrechnungsdetails hinzufügen. Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
3. Wenn Sie einen **öffentlichen Endpunkt** in Ihrer Vorlage verwenden (z. B. in Form eines Blobspeichers), dürfen Sie den Namespace **nicht hartcodieren**. Verwenden Sie die **reference**-Funktion, um den Namespace dynamisch abzurufen. Mit diesem Ansatz können Sie die Vorlage in anderen öffentlichen Namespace-Umgebungen bereitstellen, ohne den Endpunkt in der Vorlage manuell zu ändern. Legen Sie die "apiVersion" auf die Version fest, die Sie für das Speicherkonto in der Vorlage verwenden.
   
        "osDisk": {
            "name": "osdisk",
            "vhd": {
                "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
            }
        }
   
     Wenn das Speicherkonto in derselben Vorlage bereitgestellt wird, müssen Sie beim Verweisen auf die Ressource nicht den Namespace des Anbieters angeben. Die vereinfachte Syntax lautet wie folgt:
   
        "osDisk": {
            "name": "osdisk",
            "vhd": {
                "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
            }
        }
   
     Wenn Sie andere Werte in der Vorlage mit einem öffentlichen Namespace konfiguriert haben, ändern Sie diese Werte entsprechend der „reference“-Funktion. Beispiel: Die „storageUri“ Eigenschaft des virtuellen Computers „diagnosticsProfile“.
   
        "diagnosticsProfile": {
            "bootDiagnostics": {
                "enabled": "true",
                "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
            }
        }
   
     Sie können auch auf ein in einer anderen Ressourcengruppe vorhandenes Speicherkonto **verweisen** .

        "osDisk": {
            "name": "osdisk", 
            "vhd": {
                "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
            }
        }

1. Weisen Sie einem virtuellen Computer nur dann öffentliche IP-Adressen (publicIPAddresses) zu, wenn dies für eine Anwendung erforderlich ist. Verwenden Sie für das Debuggen, die Verwaltung oder administrative Zwecken entweder eingehende NAT-Regeln (inboundNatRules), virtuelle Netzwerkgateways (virtualNetworkGateways) oder eine Jumpbox.
   
     Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Computern finden Sie unter:
   
   * [Running VMs for an N-tier architecture on Azure](../guidance/guidance-compute-n-tier-vm.md) (Ausführen virtueller Computer in einer Azure-Architektur mit n Ebenen)
   * [Einrichten des Zugriffs auf WinRM für virtuelle Computer in Azure Resource Manager](../virtual-machines/virtual-machines-windows-winrm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Ermöglichen des externen Zugriffs auf einen virtuellen Computer über das Azure-Portal](../virtual-machines/virtual-machines-windows-nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Zulassen des externen Zugriffs auf eine VM mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Öffnen von Ports und Endpunkten](../virtual-machines/virtual-machines-linux-nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. Die Eigenschaft **domainNameLabel** für „publicIPAdresses“ muss eindeutig sein. „domainNameLabel“ muss 3 bis 63 Zeichen lang sein und den Regeln des regulären Ausdrucks `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`entsprechen. Da die „uniqueString“-Funktion eine Zeichenfolge mit 13 Zeichen erstellt, ist der dnsPrefixString-Parameter auf 50 Zeichen beschränkt.
   
        "parameters": {
            "dnsPrefixString": {
                "type": "string",
                "maxLength": 50,
                "metadata": {
                    "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
                }
            }
        },
        "variables": {
            "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
        }
3. Verwenden Sie beim Hinzufügen eines Kennworts zu **customScriptExtension** die **commandToExecute**-Eigenschaft in „protectedSettings“.
   
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
   
   > [!NOTE]
   > Zum Sicherzustellen der Verschlüsselung von Kennwörtern, die als Parameter an „virtualMachines/extensions“ übergeben werden, verwenden Sie die „protectedSettings“-Eigenschaft der entsprechenden Erweiterungen.
   > 
   > 

## <a name="outputs"></a>Ausgaben
Wenn **publicIPAddresses** mit einer Vorlage erstellt wird, sollte diese den Abschnitt **output** enthalten, der Details zur IP-Adresse und den vollständig qualifizierten Domäne zurückgibt. Anhand dieser Ausgabewerte können Sie diese Informationen nach der Bereitstellung problemlos abrufen. Beim Verweisen auf die Ressource verwenden Sie die API-Version, die zu ihrer Erstellung verwendet wurde. 

```
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

## <a name="single-template-or-nested-templates"></a>Einzelvorlage oder geschachtelte Vorlagen
Um Ihre Lösung bereitzustellen, können Sie entweder eine Einzelvorlage oder eine Hauptvorlage mit mehreren geschachtelten Vorlagen verwenden. Geschachtelte Vorlagen sind für komplexere Szenarien üblich. Geschachtelte Vorlagen bieten die folgenden Vorteile:

1. Die Lösung kann in Zielkomponenten zerlegt werden.
2. Geschachtelte Vorlagen können mit anderen Hauptvorlagen erneut verwendet werden.

Wenn Sie Ihren Vorlagenentwurf in mehrere geschachtelte Vorlagen zerlegen möchten, helfen Ihnen die folgenden Leitfäden beim Standardisieren des Entwurfs. Diese Leitlinien basieren auf dem Artikel [Bewährte Methoden für das Entwerfen von Azure-Ressourcen-Manager-Vorlagen](best-practices-resource-manager-design-templates.md) . Der empfohlene Entwurf umfasst die folgenden Vorlagen:

* **Hauptvorlage** (azuredeploy.json). Wird für die Eingabeparameter verwendet.
* **Vorlage für freigegebene Ressourcen** Stellt die freigegebenen Ressourcen bereit, die von allen anderen Ressourcen (z. B. virtuelles Netzwerk, Verfügbarkeitsgruppen) verwendet werden. Der Ausdruck „dependsOn“ erzwingt, dass diese Vorlage vor den anderen Vorlagen bereitgestellt wird.
* **Vorlage für optionale Ressourcen** Dient zum bedingten Bereitstellen von Ressourcen basierend auf einem Parameter (z. B. einer Jumpbox).
* **Vorlage für Memberknotenressourcen** Jeder Instanztyp auf einer Anwendungsebene verfügt über eine eigene Konfiguration. Innerhalb einer Ebene können andere Instanztypen definiert werden (wie z. B. erste Instanz erstellt einen neuen Cluster, zusätzliche Instanzen werden dem vorhandenen Cluster hinzugefügt). Jeder Instanztyp verfügt über eine eigene Bereitstellungsvorlage.
* **Skripts** Umfassend wiederverwendbare Skripts gibt es für jeden Instanztyp (z. B. zum Initialisieren und Formatieren zusätzlicher Datenträger). Benutzerdefinierte Skripts werden für spezifische Anpassungszwecke erstellt und unterscheiden sich je nach Instanztyp.

![Geschachtelte Vorlage](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-template"></a>Bedingtes Verknüpfen mit geschachtelter Vorlage
Sie können mit geschachtelten Vorlagen eine bedingte Verknüpfung erstellen, indem Sie einen Parameter verwenden, der Teil des URI der Vorlage wird.

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

## <a name="template-format"></a>Vorlagenformat
1. Es empfiehlt sich, Ihre Vorlage einer JSON-Gültigkeitsprüfung zu unterziehen, um überflüssige Kommas und runde sowie eckige Klammern zu entfernen, die während der Bereitstellung einen Fehler verursachen können. Nutzen Sie beispielsweise [JSONlint](http://jsonlint.com/) oder ein Linter-Paket für Ihre bevorzugte Bearbeitungsumgebung (Visual Studio Code, Atom, Sublime Text, Visual Studio usw.).
2. Es ist auch eine gute Idee, Ihren JSON-Code für eine bessere Lesbarkeit zu formatieren. Sie können das JSON-Formatierungsprogramm für Ihren lokalen Editor verwenden. In Visual Studio formatieren Sie das Dokument durch Drücken von **STRG+K, STRG+D**. In Visual Studio Code müssen Sie **ALT+UMSCHALT+F**drücken. Wenn der lokale Editor das Dokument nicht formatiert, können Sie ein [online verfügbares Formatierungsprogramm](https://www.bing.com/search?q=json+formatter)verwenden.

## <a name="next-steps"></a>Nächste Schritte
* Eine Anleitung zum Gestalten Ihrer Lösung für virtuelle Computer finden Sie unter [Ausführen einer Windows-VM in Azure](../guidance/guidance-compute-single-vm.md) und [Ausführen einer Linux-VM in Azure](../guidance/guidance-compute-single-vm-linux.md).
* Eine Anleitung zum Einrichten eines Speicherkontos finden Sie unter [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage/storage-performance-checklist.md).
* Hilfe zu virtuellen Netzwerken finden Sie unter [Richtlinien für die Netzwerkinfrastruktur](../virtual-machines/virtual-machines-windows-infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Nov16_HO3-->


