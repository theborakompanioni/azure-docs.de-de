---
title: Verarbeiten des Status in Resource Manager-Vorlagen | Microsoft Docs
description: Zeigt empfohlene Vorgehensweisen zum Verwenden komplexer Objekte, um Statusdaten für Azure-Ressourcen-Manager-Vorlagen und verknüpfte Vorlagen freizugeben.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: tomfitz

---
# Freigeben des Status in Azure-Ressourcen-Manager-Vorlagen
Dieses Thema beschreibt bewährte Methoden für die Verwaltung und Freigabe des Status in Vorlagen. Die in diesem Thema erläuterten Parameter und Variablen sind Beispiele für die Art von Objekten, die Sie definieren können, um Ihre Bereitstellungsanforderungen bequem zu organisieren. Anhand dieser Beispiele können Sie Ihre eigenen Objekte mit Eigenschaftswerten implementieren, die für Ihre Umgebung sinnvoll sind.

Dieses Thema ist Teil eines umfangreicheren Whitepapers. Um den vollständigen Artikel zu lesen, laden Sie [World Class ARM Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf) herunter.

## Bereitstellen von Standardkonfigurationseinstellungen
Statt eine Vorlage zu bieten, die eine umfassende Flexibilität und unzählige Variationen zulässt, ist es empfehlenswert, bekannte Konfigurationen, die an T-Shirt-Größen wie S, M und L angelehnt sind, sowie eine Experimentierumgebung (auch Sandkasten genannt) zur Auswahl zu stellen. Weitere Beispiele für T-Shirt-Größen sind Produktangebote, wie z. B. Community Edition oder Enterprise Edition. In anderen Fällen gibt es möglicherweise workloadspezifische Konfigurationen einer Technologie wie z. B. MapReduce oder NoSQL.

Mit komplexen Objekten können Sie Variablen erstellen, die Datensammlungen – bisweilen auch als „Eigenschaftenbehälter“ bezeichnet – enthalten und diese Daten zum Ausführen der Ressourcendeklaration in Ihrer Vorlage verwenden. Dieser Ansatz bietet geeignete, bekannte Konfigurationen mit unterschiedlichen Größen, die für Kunden vorkonfiguriert sind. Ohne bekannte Konfigurationen müssen Endkunden Clustergrößen selbst bestimmen, Ressourcenbeschränkungen der Plattform berücksichtigen und Berechnungen ausführen, um die resultierende Partitionierung von Speicherkonten und anderen Ressourcen (aufgrund von Clustergrößen- und Ressourcenbeschränkungen) zu bestimmen. Zusätzlich zum Verbessern der Erfahrung für den Kunden ist eine kleine Anzahl bekannter Konfigurationen einfacher zu unterstützen und ermöglicht Ihnen das Erreichen einer höheren Dichte.

Das folgende Beispiel zeigt, wie Sie Variablen definieren, die komplexe Objekte für die Darstellung von Datensammlungen enthalten. Die Sammlungen definieren Werte, die für die Größe des virtuellen Computers sowie Netzwerkeinstellungen, Betriebssystemeinstellungen und Verfügbarkeitseinstellungen verwendet werden.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Beachten Sie, dass die **tshirtSize**-Variable die T-Shirt-Größe, die Sie über einen Parameter angegeben haben (**Small**, **Medium**, **Large**), mit dem Text **tshirtSize** verkettet. Sie verwenden diese Variable, um die zugeordnete komplexe Objektvariable für diese T-Shirt-Größe abzurufen.

Sie können später in der Vorlage auf diese Variablen verweisen. Die Möglichkeit zum Verweisen auf benannte Variablen und deren Eigenschaften vereinfacht die Vorlagensyntax und erleichtert es den Benutzern, den Kontext zu verstehen. Das folgende Beispiel definiert eine Ressource, die unter Verwendung der oben gezeigten Objekte zum Festlegen von Werten bereitgestellt werden soll. Beachten Sie beispielsweise, dass die Größe des virtuellen Computers durch Abrufen des Werts für `variables('tshirtSize').vmSize` festgelegt wird. Der Wert für die Festplattengröße wird aus `variables('tshirtSize').diskSize` abgerufen. Darüber hinaus wird der URI für eine verknüpfte Vorlage mit dem Wert für `variables('tshirtSize').vmTemplate` festgelegt.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## Übergeben des Status an eine Vorlage
Sie geben den Status in einer Vorlage mithilfe von Parametern frei, die Sie während der Bereitstellung direkt angeben.

Die folgende Tabelle führt häufig verwendete Parameter in Vorlagen auf.

| Name | Wert | Beschreibung |
| --- | --- | --- |
| location |Zeichenfolge aus einer eingeschränkten Liste von Azure-Regionen |Der Speicherort, an dem die Ressourcen bereitgestellt werden. |
| storageAccountNamePrefix |String |Eindeutiger DNS-Name für das Speicherkonto, in dem die VM-Datenträger platziert werden. |
| domainName |String |Domänenname des öffentlich zugänglichen virtuellen Jumpbox-Computers im Format **{domainName}.{location}.cloudapp.com**. Beispiel: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |String |Benutzername für die virtuellen Computer |
| adminPassword |String |Kennwort für die virtuellen Computer |
| tshirtSize |Zeichenfolge aus einer eingeschränkten Liste von angebotenen T-Shirt-Größen |Die Größe der benannten Skalierungseinheit, die bereitgestellt werden soll. Beispielsweise "Small", "Medium", "Large". |
| virtualNetworkName |String |Der Name des virtuellen Netzwerks, das der Nutzer verwenden möchte. |
| enableJumpbox |Zeichenfolge aus einer eingeschränkten Liste (aktiviert/deaktiviert) |Parameter, der angibt, ob eine Jumpbox für die Umgebung aktiviert werden soll. Werte: "aktiviert", "deaktiviert". |

Der **tshirtSize**-Parameter, der im vorherigen Abschnitt verwendet wurde, ist wie folgt definiert:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## Übergeben des Status an verknüpfte Vorlagen
Bei der Verbindung mit verknüpften Vorlagen wird häufig eine Mischung aus statischen und generierten Variablen verwendet.

### Statische Variablen
Mithilfe statischer Variablen werden häufig Basiswerte wie z. B. URLs bereitgestellt, die in einer Vorlage verwendet werden.

Im unten stehenden Vorlagenauszug gibt `templateBaseUrl` das Stammverzeichnis für die Vorlage in GitHub zurück. Die nächste Zeile erstellt eine neue `sharedTemplateUrl`-Variable, die die Basis-URL mit dem bekannten Namen der freigegebenen Ressourcenvorlage verkettet. Darunter wird eine komplexe Objektvariable verwendet, um eine T-Shirt-Größe zu speichern. Hierbei wird die Basis-URL mit dem bekannten Speicherort der Konfigurationsvorlage verkettet und in der Eigenschaft `vmTemplate` gespeichert.

Der Vorteil dieses Ansatzes ist, dass Sie bei einer Änderung des Speicherorts für die Vorlage die statische Variable nur an einer Stelle ändern müssen. Die Änderung wird dann an alle verknüpften Vorlagen übergeben.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### Generierte Variablen
Neben den statischen Variablen werden einige Variablen dynamisch generiert. In diesem Abschnitt werden einige häufig verwendete Arten von generierten Variablen beschrieben.

#### tshirtSize
Sie kennen diese generierte Variable aus den obigen Beispielen.

#### networkSettings
In Vorlagen für Kapazitäten, Funktionen oder End-to-End-Lösungen erstellen verknüpfte Vorlagen in der Regel Ressourcen, die in einem Netzwerk vorhanden sind. Eine einfache Lösung ist es, ein komplexes Objekt zum Speichern von Netzwerkeinstellungen zu verwenden und diese an verknüpfte Vorlagen zu übergeben.

Hier finden Sie ein Beispiel für die Kommunikation der Netzwerkeinstellungen.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### availabilitySettings
In verknüpften Vorlagen erstellte Ressourcen werden häufig in einer Verfügbarkeitsgruppe platziert. Im folgenden Beispiel ist der Name der Verfügbarkeitsgruppe ebenso angegeben wie die Anzahl der zu verwendenden Fehler- und Updatedomänen.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Wenn Sie mehrere Verfügbarkeitsgruppen benötigen (z. B. eine für Masterknoten, eine weitere für Datenknoten), können Sie einen Namen als Präfix verwenden, mehrere Verfügbarkeitsgruppen angeben oder gemäß dem oben gezeigten Modell eine Variable für eine bestimmte T-Shirt-Größe erstellen.

#### storageSettings
Speicherdetails werden häufig mit verknüpften Vorlagen freigegeben. Im folgenden Beispiel stellt ein*storageSettings*-Objekt Details zu den Speicherkonto- und Containernamen bereit.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### osSettings
Bei verknüpften Vorlagen müssen Sie möglicherweise Betriebssystemeinstellungen an verschiedene Arten von Knoten über verschiedene Arten von bekannten Konfigurationen hinweg übergeben. Ein komplexes Objekt ist eine einfache Möglichkeit zum Speichern und Freigeben von Betriebssysteminformationen und erleichtert zudem die Unterstützung mehrerer Betriebssystemoptionen für die Bereitstellung.

Das folgende Beispiel zeigt ein Objekt für *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### machineSettings
Die generierte Variable *machineSettings* ist ein komplexes Objekt, das verschiedene Hauptvariablen zum Erstellen eines neuen virtuellen Computers enthält: Administratorbenutzername und -kennwort, ein Präfix für die Namen der virtuellen Computer und einen Verweis auf ein Betriebssystemimage, wie unten gezeigt:

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Beachten Sie, dass *osImageReference* die Werte aus der in der Hauptvorlage definierten Variable *osSettings* abruft. Dies bedeutet, dass Sie das Betriebssystem eines virtuellen Computers problemlos ändern können – vollständig oder basierend auf den Einstellungen eines Vorlagenutzers.

#### vmScripts
Das *vmScripts*-Objekt enthält Details zu den Skripts zum Herunterladen und Ausführen auf einer virtuellen Computerinstanz, einschließlich der externen und internen Verweise. Externe Verweise schließen die Infrastruktur ein. Interne Verweise umfassen die installierte Software und die Konfiguration.

Mithilfe der *scriptsToDownload*-Eigenschaft listen Sie die Skripts auf, die auf den virtuellen Computer heruntergeladen werden sollen.

Wie das folgende Beispiel zeigt, enthält dieses Objekt auch Verweise auf Befehlszeilenargumente für verschiedene Arten von Aktionen. Zu diesen Aktionen gehört die Ausführung der Standardinstallation für jeden einzelnen Knoten, eine Installation, die nach der Bereitstellung aller Knoten ausgeführt wird, sowie ggf. die Ausführung weiterer vorlagenspezifischer Skripts.

Dieses Beispiel stammt aus einer Vorlage zum Bereitstellen von MongoDB, wofür ein Arbiter erforderlich ist, um Hochverfügbarkeit sicherzustellen. *arbiterNodeInstallCommand* wurde zu *vmScripts* hinzugefügt, um den Arbiter (Vermittlung) zu installieren.

Sie finden die Variablen, die den spezifischen Text zum Ausführen des Skripts mit den richtigen Werten definieren, im Variablenabschnitt.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## Zurückgeben eines Status aus einer Vorlage
Sie können nicht nur Daten in eine Vorlage übergeben, Sie können auch Daten wieder in der aufrufenden Vorlage freigeben. Im **outputs**-Abschnitt einer verknüpften Vorlage können Sie Schlüssel-Wert-Paare bereitstellen, die von der Quellvorlage genutzt werden können.

Das folgende Beispiel zeigt, wie die private IP-Adresse übergeben wird, die in einer verknüpften Vorlage generiert wurde.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Innerhalb der Hauptvorlage können Sie diese Daten mit folgender Syntax verwenden:

    "[reference('master-node').outputs.masterip.value]"

Sie können diesen Ausdruck im Abschnitt „outputs“ oder im Abschnitt „resources“ der Hauptvorlage verwenden. Sie können den Ausdruck nicht im Abschnitt „variables“ verwenden, da er vom Laufzeitstatus abhängt. Verwenden Sie Folgendes, um diesen Wert aus der Hauptvorlage zurückzugeben:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

Ein Beispiel für die Verwendung des Abschnitts „outputs“ einer verknüpften Vorlage, um Datenträger für einen virtuellen Computer zurückzugeben, finden Sie unter [Erstellen mehrerer Datenträger für einen virtuellen Computer](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## Definieren von Authentifizierungseinstellungen für den virtuellen Computer
Sie können das gleiche Muster, das oben für Konfigurationseinstellungen aufgeführt ist, für die Angabe der Authentifizierungseinstellungen für einen virtuellen Computer verwenden. Sie erstellen einen Parameter für die Übergabe des Authentifizierungstyps.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Sie fügen Variablen für den verschiedenen Authentifizierungstypen hinzu. Außerdem fügen Sie eine Variable hinzu, um zu speichern, welcher Typ basierend auf dem Wert des Parameters für diese Bereitstellung verwendet wird.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Wenn Sie den virtuellen Computer definieren, legen Sie **osProfile** auf die erstellte Variable fest.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## Nächste Schritte
* Informationen zu den Abschnitten der Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Unter [Vorlagenfunktionen in Azure Resource Manager](resource-group-template-functions.md) finden Sie alle Funktionen, die in einer Vorlage verfügbar sind.

<!---HONumber=AcomDC_0713_2016-->