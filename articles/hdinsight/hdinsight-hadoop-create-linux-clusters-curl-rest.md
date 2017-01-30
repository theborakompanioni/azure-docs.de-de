---
title: Bereitstellen von Hadoop-, HBase- oder Storm-Clustern unter Linux in HDInsight mit cURL und der Azure-REST-API | Microsoft Docs
description: "Erfahren Sie, wie Sie Linux-basierte HDInsight-Cluster mit cURL, Azure-Ressourcen-Manager-Vorlagen und der Azure-REST-API erstellen. Sie können den Cluster-Typ (Hadoop, HBase oder Storm) angeben oder Skripts verwenden, um benutzerdefinierte Komponenten zu installieren."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 98be5893-2c6f-4dfa-95ec-d4d8b5b7dcb5
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 8056e7ece1942c9090a7c36447a96829febaf1a4
ms.openlocfilehash: 491f8540e8e53f366327ed80caff2e1e360132fc


---
# <a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Erstellen von Linux-basierten Clustern in HDInsight mithilfe von cURL und der Azure-REST-API

[!INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Mit der Azure-REST-API können Sie Verwaltungsvorgänge für Dienste durchführen, die auf der Azure-Plattform gehostet werden. Beispielsweise können Sie neue Ressourcen wie etwa Linux-basierte HDInsight-Cluster erstellen. In diesem Dokument erfahren Sie, wie Sie Azure-Ressourcen-Manager-Vorlagen erstellen, um einen HDInsight-Cluster sowie den zugehörigen Speicher zu konfigurieren. Sie erfahren auch, wie Sie anschließend die Vorlage mithilfe von cURL in der Azure-REST-API bereitstellen, um einen neuen HDInsight-Cluster zu erstellen.

> [!IMPORTANT]
> Bei den Schritten in diesem Dokument wird die Standardanzahl von Workerknoten (4) für einen HDInsight-Cluster verwendet. Wenn Sie mehr als 32 Workerknoten planen, entweder bei Erstellung des Clusters oder durch eine Skalierung des Clusters nach der Erstellung, müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen.
>
> Weitere Informationen zu Knotengrößen und damit verbundenen Kosten finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).


## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI 2.0** (Vorschau) Die Azure-Befehlszeilenschnittstelle wird verwendet, um einen Dienstprinzipal zu erstellen, der zum Generieren von Authentifizierungstoken für Anforderungen an die Azure-REST-API verwendet wird. Weitere Informationen zur Azure CLI 2.0-Vorschau finden Sie unter [Erste Schritte mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

* **cURL**. Dieses Hilfsprogramm ist über Ihr Paketverwaltungssystem verfügbar oder kann hier heruntergeladen werden: [http://curl.haxx.se/](http://curl.haxx.se/).

  > [!NOTE]
  > Wenn Sie PowerShell zum Ausführen der Befehle in diesem Dokument verwenden, müssen Sie zuerst den `curl`-Alias entfernen, der standardmäßig erstellt wird. Wenn Sie den `curl` -Befehl von einer PowerShell-Eingabeaufforderung ausführen, verwendet dieser Alias das PowerShell-Cmdlet "Invoke-WebRequest" anstelle von cURL und gibt für viele der in diesem Dokument angegebenen Befehle Fehler zurück.
  >
  > Um diesen Alias zu entfernen, verwenden Sie folgenden Befehl in der PowerShell-Eingabeaufforderung:
  >
  > `Remove-item alias:curl`
  >
  > Nachdem der Alias entfernt wurde, können Sie die auf Ihrem System installierte cURL-Version verwenden.

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-template"></a>Erstellen einer Vorlage

Bei Azure Resource Manager-Vorlagen handelt es sich um JSON-Dokumente, mit denen eine **Ressourcengruppe** und alle darin enthaltenen Ressourcen (z.B. HDInsight) beschrieben werden. Dieser vorlagenbasierte Ansatz ermöglicht Ihnen das Definieren aller für HDInsight benötigten Ressourcen in einer einzigen Vorlage sowie das Verwalten von Änderungen an der gesamten Gruppe durch **Bereitstellungen**, mit denen Änderungen an der Gruppe vorgenommen werden.

Vorlagen werden üblicherweise in zwei Teilen bereitgestellt: die Vorlage selbst und eine Parameterdatei, die Sie mit Werten auffüllen, die speziell für Ihre Konfiguration gelten. Beispiel: Clustername, Administratorname und -kennwort. Wenn Sie die REST-API direkt verwenden, müssen Sie diese Werte in eine Datei kombinieren. Das Format dieses JSON-Dokuments sieht folgendermaßen aus:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Folgendes ist beispielsweise eine Kombination aus den Vorlagen- und Parameterdateien von [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password). Damit wird ein Linux-basierter Cluster erstellt, bei dem das SSH-Benutzerkonto durch ein Kennwort geschützt ist.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
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
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {

                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {

                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
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
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

Dieses Beispiel wird in den Schritten im vorliegenden Dokument verwendet. Sie müssen den Platzhalter *values* im Abschnitt **Parameters** am Ende des Dokuments durch die Werte ersetzen, die Sie für Ihren Cluster verwenden möchten.

## <a name="login-to-your-azure-subscription"></a>Anmelden bei Ihrem Azure-Abonnement

Führen Sie die in [Erste Schritte mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) dokumentierten Schritte aus, und stellen Sie mithilfe des Befehls `az login` eine Verbindung mit Ihrem Abonnement her.

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

> [!NOTE]
> Diese Schritte stellen eine verkürzte Version der Informationen im Abschnitt *Erstellen eines Dienstprinzipals mit Kennwort* des Dokuments [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dar. Mit diesen Schritten erstellen Sie einen neuen Dienstprinzipal, der zum Authentifizieren der REST-API-Anforderungen für das Erstellen von Azure-Ressourcen wie einem HDInsight-Cluster verwendet werden kann.

1. Führen Sie an einer Befehlszeile den folgenden Befehl aus, um Ihre Azure-Abonnements aufzulisten.

         az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table

    Wählen Sie in der Liste das Abonnement aus, das Sie verwenden möchten, und notieren Sie sich die Werte in den Spalten **Subscription_ID** und __Tenant_ID__. Speichern Sie diese Werte.

2. Führen Sie die folgenden Befehle zum Erstellen einer neuen Anwendung in Azure Active Directory aus.

        az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'

    Ersetzen Sie die Werte für `--display-name`, `--homepage` und `--identifier-uris` durch Ihre eigenen Werte. Geben Sie ein Kennwort für den neuen Active Directory-Eintrag an.

   > [!NOTE]
   > Da Sie diese Anwendung für die Authentifizierung über einen Dienstprinzipal erstellen, müssen die Werte `--home-page` und `--identifier-uris` nicht auf eine tatsächliche Webseite im Internet verweisen. Es muss sich lediglich um eindeutige URIs handeln.

   Der von diesem Befehl zurückgegebene Wert ist die __App-ID__ für die neue Anwendung. Speichern Sie diesen Wert.

3. Führen Sie den folgenden Befehl aus, um mithilfe der **App-ID** einen Dienstprinzipal zu erstellen.

        az ad sp create --id <App ID> --query 'objectId'

     Der von diesem Befehl zurückgegebene Wert ist die __Objekt-ID__. Speichern Sie diesen Wert.

4. Weisen Sie dem Dienstprinzipal mit dem **Objekt-ID**-Wert die Rolle **Besitzer** zu. Sie müssen außerdem die **Abonnement-ID** verwenden, die Sie zuvor erhalten haben.

        az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/

## <a name="get-an-authentication-token"></a>Abrufen eines Authentifizierungstokens

Führen Sie den folgenden Befehl aus, um ein Authentifizierungstoken abzurufen:

    curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
    -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    --data-urlencode "client_id=AppID" \
    --data-urlencode "grant_type=client_credentials" \
    --data-urlencode "client_secret=password" \
    --data-urlencode "resource=https://management.azure.com/"

    Replace **TenantID**, **AppID**, and **password** with the values obtained or used previously.

    If this request is successful, you will receive a 200 series response and the response body will contain a JSON document.

    The JSON document returned by this request will contain an element named **access_token**; the value of this element is the access token you must use to authentication the requests used in the next sections of this document.

        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Gehen Sie wie folgt vor, um eine neue Ressourcengruppe zu erstellen. Sie müssen zuerst die Gruppe erstellen, bevor Sie die einzelnen Ressourcen, wie z. B. den HDInsight-Cluster, erstellen können.

* Ersetzen Sie **SubscriptionID** durch die Abonnement-ID, die Sie während der Erstellung des Dienstprinzipals erhalten haben.
* Ersetzen Sie **AccessToken** durch das Zugriffstoken, das Sie im vorherigen Schritt erhalten haben.
* Ersetzen Sie **DataCenterLocation** durch das Rechenzentrum, in dem Sie die Ressourcengruppe und die Ressourcen erstellen möchten. Beispiel: "USA, Mitte/Süden".
* Ersetzen Sie **ResourceGroupName** durch den Namen, den Sie für diese Gruppe verwenden möchten.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Wenn die Anforderung erfolgreich ist, erhalten Sie eine 2xx-Antwort, deren Text ein JSON-Dokument mit Informationen zur Gruppe enthält. Das `"provisioningState"`-Element enthält den Wert `"Succeeded"`.

## <a name="create-a-deployment"></a>Erstellen einer Bereitstellung

Gehen Sie wie folgt vor, um die Clusterkonfiguration (Vorlage und Parameterwerte) in der Ressourcengruppe bereitzustellen.

* Ersetzen Sie **SubscriptionID** und **AccessToken** durch die oben verwendeten Werte.
* Ersetzen Sie **ResourceGroupName** durch den Namen der Ressourcengruppe, die Sie im vorherigen Abschnitt erstellt haben.
* Ersetzen Sie **DeploymentName** durch den Namen, den Sie für diese Bereitstellung verwenden möchten.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]
> Wenn Sie das JSON-Dokument mit der Vorlage und den Parametern in eine Datei gespeichert haben, können Sie anstelle von `-d "{ template and parameters}"` Folgendes verwenden:
>
> `--data-binary "@/path/to/file.json"`

Wenn die Anforderung erfolgreich ist, erhalten Sie eine 2xx-Antwort, deren Text ein JSON-Dokument mit Informationen zum Bereitstellungsvorgang enthält.

> [!IMPORTANT]
> Beachten Sie, dass die Bereitstellung zu diesem Zeitpunkt übermittelt, aber noch nicht abgeschlossen wurde. Es kann mehrere Minuten dauern (in der Regel etwa 15), bis die Bereitstellung abgeschlossen ist.

## <a name="check-the-status-of-a-deployment"></a>Überprüfen des Bereitstellungsstatus

Gehen Sie wie folgt vor, um den Status der Bereitstellung zu prüfen.

* Ersetzen Sie **SubscriptionID** und **AccessToken** durch die oben verwendeten Werte.
* Ersetzen Sie **ResourceGroupName** durch den Namen der Ressourcengruppe, die Sie im vorherigen Abschnitt erstellt haben.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Damit wird ein JSON-Dokument mit Informationen zum Bereitstellungsvorgang zurückgegeben. Das `"provisioningState"`-Element enthält den Status der Bereitstellung; wenn dieses Element den Wert `"Succeeded"` enthält, wurde die Bereitstellung erfolgreich abgeschlossen. Jetzt kann Ihr Cluster verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen HDInsight-Cluster erfolgreich erstellt haben, nutzen Sie die folgenden Informationen, um zu erfahren, wie Sie mit Ihrem Cluster arbeiten.

### <a name="hadoop-clusters"></a>Hadoop-Cluster

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-Cluster

* [Erste Schritte mit HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Entwickeln von Java-Anwendungen für HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-Cluster

* [Entwickeln von Java-Topologien für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden von Python-Komponenten in Storm in HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit Storm in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)



<!--HONumber=Dec16_HO3-->


