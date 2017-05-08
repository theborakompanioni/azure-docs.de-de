---
title: Erstellen von HDInsight-Clustern (Hadoop) mit Vorlagen | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Cluster für HDInsight mit Azure Resource Manager-Vorlagen erstellen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/14/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 1c095ece98c09290992ffeaca1f7b91a87a78818
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Erstellen von Hadoop-Clustern in HDInsight mit Azure Resource Manager-Vorlagen
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In diesem Artikel lernen Sie verschiedene Möglichkeiten zum Erstellen von Azure HDInsight-Clustern mit Azure Resource Manager-Vorlagen. Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md). Informationen über weitere Tools und Features zur Clustererstellung finden Sie, indem Sie oben auf dieser Seite auf die Registerkartenauswahl klicken, oder unter [Methoden zur Clustererstellung](hdinsight-hadoop-provision-linux-clusters.md#cluster-creation-methods).

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Um die Anweisungen in diesem Artikel befolgen zu können, benötigen Sie:

* Ein [Azure-Abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell und/oder Azure CLI.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Resource Manager-Vorlagen
Eine Resource Manager-Vorlage erleichtert das Erstellen des Folgenden für Ihre Anwendung in einem einzelnen, koordinierten Vorgang:
* HDInsight-Cluster und ihre abhängigen Ressourcen (z.B. das Standardspeicherkonto)
* Andere Ressourcen (z.B. Azure SQL-Datenbank zur Verwendung von Apache Sqoop)

In der Vorlage definieren Sie die Ressourcen, die für die Anwendung erforderlich sind. Sie geben außerdem die Bereitstellungsparameter zur Eingabe von Werten für unterschiedliche Umgebungen an. Die Vorlage besteht aus JSON-Code und Ausdrücken, mit denen Sie Werte für Ihre Bereitstellung erstellen.

Sie finden Beispiele für HDInsight-Vorlagen unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?term=hdinsight). Verwenden Sie den plattformübergreifenden [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) mit der [Resource Manager-Erweiterung](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) oder einem Text-Editor, um die Vorlage in einer Datei auf Ihrer Arbeitsstation zu speichern. Sie erfahren, wie die Vorlage mithilfe verschiedener Methoden aufgerufen werden kann.

Weitere Informationen über Resource Manager-Vorlagen finden Sie in den folgenden Artikeln:

* [Erstellen von Azure Resource-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Bereitstellen einer Anwendung mit Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Generieren von Vorlagen

Mithilfe des Azure-Portals können Sie alle Eigenschaften eines Clusters konfigurieren und dann die Vorlage speichern, bevor Sie sie bereitstellen. Sie können die Vorlage dann wiederverwenden.

**So generieren Sie eine Vorlage mithilfe des Azure-Portals**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **Neu**, klicken Sie auf **Intelligence + Analyse**, und klicken Sie dann auf **HDInsight**.
3. Befolgen Sie die Anweisungen beim Eingeben der Eigenschaften. Verwenden Sie entweder die **Schnellerfassung** oder die Option **Benutzerdefiniert**.
4. Klicken Sie auf der Registerkarte **Zusammenfassung** auf **Vorlage und Parameter herunterladen**:

    ![Herunterladen der Resource Manager-Vorlage zum Erstellen von HDInsight-Hadoop-Clustern](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    Sie sehen eine Liste mit Vorlagendatei, Parameterdatei und Codebeispielen, die zum Bereitstellen der Vorlage verwendet werden:

    ![Optionen zum Herunterladen der Resource Manager-Vorlage zum Erstellen von HDInsight-Hadoop-Clustern](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Von hier aus können Sie die Vorlage herunterladen, sie in Ihrer Vorlagenbibliothek speichern oder die Vorlage bereitstellen.

    Um auf eine Vorlage in Ihrer Bibliothek zuzugreifen, klicken Sie im linken Menü auf **Weitere Dienste**, und klicken Sie dann auf **Vorlagen** (unter der Kategorie **Sonstiges**).

    > [!Note]
    > Vorlage- und Parameterdatei müssen zusammen verwendet werden. Andernfalls erhalten Sie möglicherweise unerwartete Ergebnisse. Beispielsweise ist der Standardwert der Eigenschaft **clusterKind** immer **hadoop**, unabhängig von Ihren Angaben vor dem Herunterladen der Vorlage.



## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

Mit diesem Verfahren wird ein Hadoop-Cluster in HDInsight erstellt.

1. Speichern Sie die JSON-Datei im [Anhang](#appx-a-arm-template) auf Ihrer Arbeitsstation. Im PowerShell-Skript ist der Dateiname `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. Legen Sie ggf. die Parameter und Variablen fest.
3. Führen Sie die Vorlage mithilfe des folgenden PowerShell-Skripts aus:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    Mit dem PowerShell-Skript wird nur der Name des Clusters konfiguriert. Der Name des Speicherkontos ist in der Vorlage hartcodiert. Sie werden aufgefordert, das Clusterbenutzerkennwort einzugeben. (Der Standardbenutzername lautet **admin**.) Sie werden auch aufgefordert, das SSH-Benutzerkennwort einzugeben. (Der Standard-SSH-Benutzername ist **sshuser**.)  

Weitere Informationen finden Sie unter [Bereitstellen mit PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>Bereitstellen mit dem CLI
Im folgenden Beispiel wird die Azure-Befehlszeilenschnittstelle (CLI) verwendet. Sie erstellt einen Cluster und dessen abhängiges Speicherkonto sowie den zugehörigen Container durch Aufrufen einer Resource Manager-Vorlage:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

Sie werden aufgefordert, Folgendes einzugeben:
* Den Clusternamen.
* Das Clusterbenutzerkennwort. (Der Standardbenutzername lautet **admin**.)
* Das SSH-Benutzerkennwort. (Der Standard-SSH-Benutzername ist **sshuser**.)

Der folgende Code gibt Inlineparameter an:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-the-rest-api"></a>Bereitstellen mit der REST-API
Informationen hierzu finden Sie unter [Bereitstellen mit der REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Bereitstellen mit Visual Studio 2013
 Verwenden Sie Visual Studio, um ein Ressourcengruppenprojekt zu erstellen und über die Benutzeroberfläche in Azure bereitzustellen. Sie wählen den Typ der Ressourcen aus, die in Ihr Projekt einbezogen werden sollen. Diese Ressourcen werden automatisch der Resource Manager-Vorlage hinzugefügt. Das Projekt enthält auch ein PowerShell-Skript zum Bereitstellen der Vorlage.

Eine Einführung in die Verwendung von Visual Studio mit Ressourcengruppen finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-administer-use-portal-linux.md#create-clusters) an.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Möglichkeiten zum Erstellen von HDInsight-Clustern kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Ein ausführliches Beispiel für die Bereitstellung einer Anwendung finden Sie unter [Vorhersagbares Bereitstellen von Microservices in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
* Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](../solution-dev-test-environments.md).
* Informationen zu den Abschnitten der Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).
* Unter [Vorlagenfunktionen](../azure-resource-manager/resource-group-template-functions.md)finden Sie eine Liste der Funktionen, die Sie in einer Azure Resource Manager-Vorlage verwenden können.

## <a name="appendix-resource-manager-template"></a>Anhang: Resource Manager-Vorlage
Die folgende Azure Resource Manager-Vorlage erstellt einen Linux-basierten Hadoop-Cluster mit abhängigem Azure-Speicherkonto.

> [!NOTE]
> Das Beispiel enthält Konfigurationsinformationen für den Hive-Metastore und den Oozie-Metastore. Entfernen Sie den entsprechenden Abschnitt, oder konfigurieren Sie ihn, bevor Sie die Vorlage verwenden.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

