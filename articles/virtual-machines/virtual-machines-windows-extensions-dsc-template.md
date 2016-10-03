<properties
   pageTitle="Resource Manager-Vorlage für die Konfiguration des gewünschten Zustands | Microsoft Azure"
   description="Definition der Resource Manager-Vorlage für die Konfiguration des gewünschten Zustands in Azure mit Beispielen und Problembehandlung"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# Windows VMSS und Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen
Dieser Artikel beschreibt die Resource Manager-Vorlage für den [Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands](virtual-machines-windows-extensions-dsc-overview.md).

## Vorlagenbeispiel für einen virtuellen Windows-Computer

Der folgende Codeausschnitt wird in den Ressourcenabschnitt der Vorlage eingefügt.

```json
			"name": "Microsoft.Powershell.DSC",
			"type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## Vorlagenbeispiel für Windows VMSS

Ein VMSS-Knoten weist den Abschnitt „properties“ mit dem Attribut „VirtualMachineProfile“, „extensionProfile“ auf. DSC wird unter „extensions“ hinzugefügt.

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## Ausführliche Einstellungsinformationen

Dies ist das Schema für den Bereich mit den Einstellungen der Azure DSC-Erweiterung in einer Azure Resource Manager-Vorlage.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## Details
| Eigenschaftenname | Typ | Beschreibung |
| --- | --- | --- |
| settings.wmfVersion | string | Gibt die Version von Windows Management Framework an, die auf Ihrem virtuellen Computer installiert sein muss. Wenn diese Eigenschaft auf „neueste“ festgelegt ist, wird die aktuelle Version von WMF installiert. Die einzigen derzeit möglichen Werte für diese Eigenschaft sind **„4.0“, „5.0“, „5.0PP“ und „neueste“**. Diese möglichen Werte werden gelegentlich aktualisiert. Der Standardwert ist „neueste“.|
| settings.configuration.url | string | Gibt den URL-Speicherort an, von dem die ZIP-Datei Ihrer DSC-Konfiguration herunterzuladen ist. Wenn die bereitgestellte URL ein SAS-Token für den Zugriff erfordert, müssen Sie die protectedSettings.configurationUrlSasToken-Eigenschaft auf den Wert Ihres SAS-Tokens festlegen. Diese Eigenschaft ist erforderlich, wenn „settings.configuration.script“ und/oder „settings.configuration.function“ definiert sind. |
| settings.configuration.script | string | Gibt den Dateinamen des Skripts an, das die Definition Ihrer DSC-Konfiguration enthält. Dieses Skript muss sich im Stammverzeichnis der ZIP-Datei befinden, die von der durch die configuration.url-Eigenschaft angegebenen URL heruntergeladen wurde. Diese Eigenschaft ist erforderlich, wenn „settings.configuration.url“ und/oder „settings.configuration.script“ definiert sind. |
| settings.configuration.function | string | Gibt den Namen Ihrer DSC-Konfiguration an. Die Konfiguration mit diesem Namen muss im Skript enthalten sein, das durch „configuration.script“ definiert ist. Diese Eigenschaft ist erforderlich, wenn „settings.configuration.url“ und/oder „settings.configuration.function“ definiert sind. |
| settings.configurationArguments | Sammlung | Definiert beliebige Parameter, die Sie Ihrer DSC-Konfiguration übergeben möchten. Diese Eigenschaft ist nicht verschlüsselt. |
| settings.configurationData.url | string | Gibt die URL an, unter der die Datei mit Ihren Konfigurationsdaten (.pds1) heruntergeladen werden kann, um sie als Eingabe für Ihre DSC-Konfiguration zu nutzen. Wenn die bereitgestellte URL ein SAS-Token für den Zugriff erfordert, müssen Sie die protectedSettings.configurationDataUrlSasToken-Eigenschaft auf den Wert Ihres SAS-Tokens festlegen.|
| settings.privacy.dataEnabled | string | Aktiviert bzw. deaktiviert die Erfassung von Telemetriedaten. Die einzig möglichen Werte für diese Eigenschaft sind **„Aktivieren“, „Disable“ oder „$null“**. Wird die Eigenschaft leer gelassen oder „null“ angegeben, ist die Telemetrie aktiviert. Der Standardwert ist ''. [Weitere Informationen](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Sammlung | Definiert alternative Speicherorte zum Herunterladen von WMF. [Weitere Informationen](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Sammlung | Definiert beliebige Parameter, die Sie Ihrer DSC-Konfiguration übergeben möchten. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationUrlSasToken | string | Gibt das SAS-Token für den Zugriff auf durch „configuration.url“ definierte URL an. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationDataUrlSasToken | string | Gibt das SAS-Token für den Zugriff auf durch „configurationData.url“ definierte URL an. Diese Eigenschaft ist verschlüsselt. |

## „Settings“ im Vergleich zu „ProtectedSettings“
Alle Einstellungen werden auf dem virtuellen Computer in einer Einstellungstextdatei gespeichert. Eigenschaften unter „Settings“ sind öffentliche Eigenschaften, da sie in der Einstellungstextdatei nicht verschlüsselt sind. Eigenschaften unter „ProtectedSettings“ sind mit einem Zertifikat verschlüsselt und werden auf dem virtuellen Computer in dieser Datei nicht als Nur-Text angezeigt.

Wenn die Konfiguration Anmeldeinformationen erfordert, können diese in „ProtectedSettings“ enthalten sein:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
       	    "userName": "UsernameValue1",
       	    "password": "PasswordValue1"
        }
    }
}
```

## Beispiel

Das folgende Beispiel wird aus dem Abschnitt „Erste Schritte“ der Seite [DSC-Erweiterung: Handler-Übersicht](virtual-machines-windows-extensions-dsc-overview.md) abgeleitet. In diesem Beispiel werden Resourcen Manager-Vorlagen anstelle von Cmdlets zum Bereitstellen der Erweiterung verwendet. Speichern Sie die Konfiguration „IisInstall.ps1“, fügen Sie die einer ZIP-Datei hinzu, und laden Sie die Datei unter einer zugänglichen URL hoch. In diesem Beispiel wird Azure-Blobspeicher verwendet, ZIP-Dateien können jedoch von beliebigen Speicherorten heruntergeladen werden.

In der Azure Resource Manager-Vorlage weist der folgende Code den virtuellen Computer an, die richtige Datei herunterzuladen und die entsprechende PowerShell-Funktion auszuführen:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## Aktualisieren des früheren Formats
Alle Einstellungen im vorherigen Format (das die öffentlichen Eigenschaften „ModulesUrl“, „ConfigurationFunction“, „SasToken“ oder „Properties“ enthält) werden automatisch an das aktuelle Format angepasst und wie bisher ausgeführt.

Das frühere Einstellungsschema sah folgendermaßen aus:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

So wird das frühere Format an das aktuelle Format angepasst:

| Eigenschaftenname | Entsprechung im früheren Schema |
| --- | --- |
| settings.wmfVersion | settings.WMFVersion |
| settings.configuration.url | settings.ModulesUrl |
| settings.configuration.script | Erster Teil von „settings.ConfigurationFunction“ (vor „\\\“) |
| settings.configuration.function | Zweiter Teil von „settings.ConfigurationFunction“ (nach „\\\“) |
| settings.configurationArguments | settings.Properties |
| settings.configurationData.url | protectedSettings.DataBlobUri (ohne SAS-Token) |
| settings.privacy.dataEnabled | settings.Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | settings.AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | settings.SasToken |
| protectedSettings.configurationDataUrlSasToken | SAS-Token aus „protectedSettings.DataBlobUri“ |


## Problembehandlung – Fehlercode 1100
Fehlercode 1100 gibt an, dass ein Problem mit der Benutzereingabe in die DSC-Erweiterung vorliegt. Der Text dieser Fehler ist nicht festgelegt und kann sich ändern. Hier finden Sie einige Fehler, die auftreten können, und die entsprechenden Behebungen.

### Ungültige Werte
„Privacy.dataCollection ist „{0}“. Die einzig möglichen Werte sind “, „Aktivieren“ und „Deaktivieren““ „WmfVersion ist „{0}“. Einzig mögliche Werte sind ... und „neueste“.“

Problem: Ein bereitgestellter Wert ist nicht zulässig.

Lösung: Ändern Sie den ungültigen Wert in einen gültigen Wert. Sehen Sie in der Tabelle im Abschnitt „Details“ nach.

### Ungültige URL
„ConfigurationData.url ist „{0}“. Dies ist keine gültige URL“. „DataBlobUri ist „{0}“. Dies ist keine gültige URL“. „Configuration.url ist „{0}“. Dies ist keine gültige URL“

Problem: Ein bereitgestellte URL ist ungültig.

Lösung: Überprüfen Sie alle Ihre angegebenen URLs. Stellen Sie sicher, dass alle URLs in gültige Speicherorte aufgelöst werden, auf die die Erweiterung auf dem Remotecomputer zugreifen kann.

### Ungültiger ConfigurationArgument-Typ
„Ungültiger ConfigurationArgument-Typ {0}“

Problem: Die ConfigurationArguments-Eigenschaft kann nicht in ein Hashtabellenobjekt aufgelöst werden.

Lösung: Stellen Sie sicher, dass Ihre ConfigurationArguments-Eigenschaft eine Hashtabelle ist. Nutzen Sie das im vorherigen Beispiel bereitgestellte Format. Achten Sie auf Anführungszeichen, Kommas und Klammern.

### „ConfigurationArguments“ doppelt vorhanden
„Doppelte Argumente „{0}“ sowohl in öffentlichen und geschützten configurationArguments-Elementen gefunden“

Problem: „ConfigurationArguments“ in öffentlichen Einstellungen und „ConfigurationArguments“ in geschützten Einstellungen enthalten Eigenschaften mit dem gleichen Namen.

Lösung: Entfernen Sie eine der doppelten Eigenschaften.

### Fehlende Eigenschaften
„Configuration.function erfordert die Angabe von configuration.url oder configuration.module“

„Configuration.url erfordert die Angabe von configuration.script“

„Configuration.script erfordert die Angabe von configuration.url“

„Configuration.url erfordert die Angabe von configuration.function“

„ConfigurationUrlSasToken erfordert die Angabe von configuration.url“

„ConfigurationDataUrlSasToken erfordert die Angabe von configurationData.url“

Problem: Eine definierte Eigenschaft benötigt eine andere Eigenschaft, die nicht vorhanden ist.

Lösungen:
- Geben Sie die fehlende Eigenschaft an.
- Entfernen Sie die Eigenschaft, die die fehlende Eigenschaft benötigt.


## Nächste Schritte
[Weitere Informationen zu DSC und VMSS](virtual-machines-scale-sets-dsc.md)

Weitere Informationen finden Sie unter [Sichere Verwaltung von Anmeldeinformationen durch DSC](virtual-machines-windows-extensions-dsc-credentials.md).

Weitere Informationen zum Azure DSC-Erweiterungs-Handler finden Sie unter [Einführung in den Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands](virtual-machines-windows-extensions-dsc-overview.md).

Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview).

<!---HONumber=AcomDC_0921_2016-->