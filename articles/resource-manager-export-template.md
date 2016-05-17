<properties 
	pageTitle="Exportieren einer Azure Resource Manager-Vorlage | Microsoft Azure" 
	description="Verwenden Sie Azure Resource Manager zum Exportieren einer Vorlage aus einer vorhandenen Ressourcengruppe." 
	services="azure-resource-manager" 
	documentationCenter="" 
	authors="tfitzmac" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="azure-resource-manager" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="05/10/2016" 
	ms.author="tomfitz"/>

# Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen

Es kann sein, dass Ihnen die Erstellung von Azure Resource Manager-Vorlagen als schwierige Aufgabe erscheint. Resource Manager unterstützt Sie bei dieser Aufgabe aber durch die Möglichkeit, eine Vorlage aus vorhandenen Ressourcen in Ihrem Abonnement zu exportieren. Sie können diese generierte Vorlage verwenden, um sich über die Vorlagensyntax zu informieren oder um die erneute Bereitstellung Ihrer Lösung je nach Bedarf zu automatisieren.

In diesem Tutorial erstellen Sie ein Speicherkonto über das Portal und exportieren die Vorlage für das Speicherkonto. Anschließend ändern Sie die Ressourcengruppe, indem Sie ihr ein virtuelles Netzwerk hinzufügen und eine neue Vorlage exportieren, die den aktuellen Zustand darstellt. In diesem Thema geht es zwar hauptsächlich um eine vereinfachte Infrastruktur, aber Sie können diese Schritte auch nutzen, um eine Vorlage für eine kompliziertere Lösung zu exportieren.

## Speicherkonto erstellen

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Neu**, **Daten und Speicher** und **Speicherkonto**.

      ![Speicher erstellen](./media/resource-manager-export-template/create-storage.png)

2. Erstellen Sie ein Speicherkonto mit dem Namen **storage**, Ihren Initialen und dem Datum. Der Name des Speicherkontos muss in Azure eindeutig sein. Ändern Sie ihn also ab, falls der Name bereits verwendet wird. Verwenden Sie als Ressourcengruppe **ExportGroup**. Sie können für die anderen Eigenschaften die Standardwerte verwenden. Klicken Sie auf **Erstellen**.

      ![Werte für Speicher angeben](./media/resource-manager-export-template/provide-storage-values.png)

Nach Abschluss der Bereitstellung enthält das Abonnement das Speicherkonto.

## Exportieren der Vorlage für die Bereitstellung
   
1. Navigieren Sie zum Blatt „Ressourcengruppe“ für die neue Ressourcengruppe. Sie sehen, dass das Ergebnis der letzten Bereitstellung angezeigt wird. Wählen Sie diesen Link aus.

      ![Blatt „Ressourcengruppe“](./media/resource-manager-export-template/resource-group-blade.png)
   
2. Der Verlauf der Bereitstellungen für die Gruppe wird angezeigt. In Ihrem Fall ist vermutlich nur eine Bereitstellung aufgeführt. Wählen Sie diese Bereitstellung aus.

     ![Letzte Bereitstellung](./media/resource-manager-export-template/last-deployment.png)

3. Es wird eine Zusammenfassung der Bereitstellung angezeigt. Die Zusammenfassung enthält den Status der Bereitstellung und die dazugehörigen Vorgänge sowie die Werte, die Sie für die Parameter angegeben haben. Wählen Sie **Vorlage anzeigen**, um die Vorlage anzuzeigen, die für die Bereitstellung verwendet wurde.

     ![Zusammenfassung der Bereitstellungen anzeigen](./media/resource-manager-export-template/deployment-summary.png)

4. Resource Manager ruft fünf Dateien für Sie ab. Sie lauten wie folgt:

   1. Die Vorlage, mit der die Infrastruktur für Ihre Lösung definiert wird. Wenn Sie das Speicherkonto über das Portal erstellt haben, hat Resource Manager eine Vorlage für die Bereitstellung verwendet und die Vorlage zur späteren Verwendung gespeichert.

   2. Eine Parameterdatei, die Sie zum Übergeben von Werten während der Bereitstellung verwenden können. Sie enthält die Werte, die Sie bei der ersten Bereitstellung angegeben haben, aber Sie können diese Werte ändern, wenn Sie die Vorlage erneut bereitstellen.

   3. Eine Azure PowerShell-Skriptdatei, die Sie zum Bereitstellen der Vorlage verwenden können.

   4. Eine Skriptdatei der Azure-Befehlszeilenschnittstelle, die Sie zum Bereitstellen der Vorlage verwenden können.
   
   5. Ein .NET-Klasse, die Sie zum Bereitstellen der Vorlage verwenden können.

     Die Dateien sind über die Links im Blatt verfügbar. Standardmäßig ist die Vorlage ausgewählt.
     
       ![Vorlage anzeigen](./media/resource-manager-export-template/view-template.png)
     
     Wir sehen uns die Vorlage nun genauer an. Die Vorlage sollte in etwa wie folgt aussehen:
   
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "name": {
              "type": "String"
            },
            "accountType": {
              "type": "String"
            },
            "location": {
              "type": "String"
            },
            "encryptionEnabled": {
              "defaultValue": false,
              "type": "Bool"
            }
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "sku": {
                "name": "[parameters('accountType')]"
              },
              "kind": "Storage",
              "name": "[parameters('name')]",
              "apiVersion": "2016-01-01",
              "location": "[parameters('location')]",
              "properties": {
                "encryption": {
                  "services": {
                    "blob": {
                      "enabled": "[parameters('encryptionEnabled')]"
                    }
                  },
                  "keySource": "Microsoft.Storage"
                }
              }
            } 
          ]
        }
   
     Beachten Sie, dass Parameter für Name, Typ und Standort des Speicherkontos und der Aktivierungsstatus der Verschlüsselung angegeben ist (Standardwert **false**). Im Abschnitt **Ressourcen** wird die Definition für das bereitzustellende Speicherkonto angezeigt.
     
In rechteckigen Klammern ist ein Ausdruck enthalten, der während der Bereitstellung ausgewertet wird. Die oben angezeigten Ausdrücke in Klammern werden verwendet, um während der Bereitstellung Parameterwerte abzurufen. Sie können noch viele andere Ausdrücke verwenden, und weiter unten in diesem Thema sind Beispiele für andere Ausdrücke angegeben. Eine vollständige Liste finden Sie unter [Azure Resource Manager-Vorlagenfunktionen](resource-group-template-functions.md).
   
Weitere Informationen zur Struktur einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).

## Hinzufügen eines virtuellen Netzwerks

Mit der Vorlage, die Sie im vorherigen Abschnitt heruntergeladen haben, wurde die Infrastruktur für die ursprüngliche Bereitstellung dargestellt. Hierfür werden aber keine Änderungen berücksichtigt, die Sie nach der Bereitstellung vornehmen. Um dieses Problem zu veranschaulichen, ändern wir die Ressourcengruppe, indem wir über das Portal ein virtuelles Netzwerk hinzufügen.

1. Wählen Sie im Blatt „Ressourcengruppe“ die Option **Hinzufügen**, und wählen Sie in den verfügbaren Ressourcen dann **Virtuelles Netzwerk**.
   
2. Geben Sie dem virtuellen Netzwerk den Namen **VNET**, und übernehmen Sie für die anderen Eigenschaften die Standardwerte. Klicken Sie auf **Erstellen**.

      ![Warnung festlegen](./media/resource-manager-export-template/create-vnet.png)
   
3. Sehen Sie sich den Bereitstellungsverlauf erneut an, nachdem das virtuelle Netzwerk erfolgreich für die Ressourcengruppe bereitgestellt wurde. Jetzt werden zwei Bereitstellungen angezeigt. Wählen Sie die aktuellere Bereitstellung aus.

      ![Bereitstellungsverlauf](./media/resource-manager-export-template/deployment-history.png)
   
4. Sehen Sie sich die Vorlage für diese Bereitstellung an. Beachten Sie, dass damit nur die Änderungen definiert werden, die Sie für das Hinzufügen des virtuellen Netzwerks vorgenommen haben.

Die bewährte Methode besteht im Allgemeinen darin, eine Vorlage zu verwenden, bei der die gesamte Infrastruktur für die Lösung mit nur einem Vorgang bereitgestellt wird. Sie müssen dann also nicht viele unterschiedliche Vorlagen für die Bereitstellung im Kopf behalten.


## Exportieren einer Vorlage für die Ressourcengruppe

Auch wenn für jede Bereitstellung nur die Änderungen angezeigt werden, die Sie an der Ressourcengruppe vorgenommen haben, können Sie jederzeit eine Vorlage exportieren, um die Attribute der gesamten Ressourcengruppe anzuzeigen.

1. Wählen Sie zum Anzeigen der Vorlage für eine Ressourcengruppe die Option **Vorlage exportieren**.

      ![Ressourcengruppe exportieren](./media/resource-manager-export-template/export-resource-group.png)

2. Es werden wieder die fünf Dateien angezeigt, die Sie zum erneuten Bereitstellen der Lösung verwenden können, aber dieses Mal sieht die Vorlage etwas anders aus. Diese Vorlage verfügt nur über zwei Parameter (einen für den Speicherkontonamen und einen für den Namen des virtuellen Netzwerks).

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },
        
     Mit Resource Manager wurden die während der Bereitstellung verwendeten Vorlagen nicht abgerufen. Stattdessen wurde basierend auf der aktuellen Konfiguration der Ressourcen eine neue Vorlage generiert. Resource Manager besitzt keine Informationen dazu, welche Werte Sie als Parameter übergeben möchten. Daher werden die meisten Werte anhand des Werts in der Ressourcengruppe hartcodiert. Der Standort des Speicherkontos und der Replikationswert werden beispielsweise wie folgt festgelegt:
     
        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Laden Sie die Vorlage herunter, damit Sie sie lokal verwenden können.

      ![Vorlage herunterladen](./media/resource-manager-export-template/download-template.png)

4. Suchen Sie nach der ZIP-Datei, die Sie heruntergeladen haben, und extrahieren Sie den Inhalt. Sie können diese heruntergeladene Vorlage verwenden, um die Infrastruktur erneut bereitzustellen.

## Nächste Schritte

Glückwunsch! Sie haben gelernt, wie Sie eine Vorlage aus Ressourcen exportieren, die Sie im Portal erstellt haben.

- Im zweiten Teil dieses Tutorials passen Sie die gerade heruntergeladene Vorlage an, indem Sie weitere Parameter hinzufügen und die erneute Bereitstellung per Skript durchführen. Weitere Informationen finden Sie unter [Anpassen und erneutes Bereitstellen der exportierten Vorlage](resource-manager-customize-template.md).
- Informationen zum Exportieren einer Vorlage mithilfe von PowerShell finden Sie unter [Verwenden von Azure PowerShell mit dem Azure Resource Manager](powershell-azure-resource-manager.md).
- Informationen zum Exportieren einer Vorlage mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure Resource Manager](xplat-cli-azure-resource-manager.md). 

<!---HONumber=AcomDC_0511_2016-->