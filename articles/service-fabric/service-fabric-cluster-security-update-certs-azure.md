---
title: Verwalten von Zertifikaten in einem Azure Service Fabric-Cluster | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie neue Zertifikate hinzufügen, ein Rollover für Zertifikate durchführen und ein Zertifikat für einen Service Fabric-Cluster entfernen."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: c433e8683755e454f9561f094269c3daccf78a62
ms.lasthandoff: 03/10/2017


---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Hinzufügen oder Entfernen von Zertifikaten für einen Service Fabric-Cluster in Azure
Es wird empfohlen, dass Sie sich damit vertraut machen, wie Service Fabric X.509-Zertifikate verwendet werden, und dass Sie sich mit den [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md) auskennen. Sie müssen verstehen, was ein Clusterzertifikat ist, und wofür es verwendet wird, bevor Sie den Vorgang fortsetzen.

Service Fabric ermöglicht es Ihnen, zusätzlich zu den Clientzertifikaten zwei Clusterzertifikate anzugeben – ein primäres und ein sekundäres –, wenn Sie die Zertifikatsicherheit während der Erstellung des Clusters konfigurieren. Ausführliche Informationen zur Einrichtung während der Erstellung finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal](service-fabric-cluster-creation-via-portal.md) oder [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Wenn Sie während der Erstellung nur ein Clusterzertifikat angeben, wird es als primäres Zertifikat verwendet. Nach der Erstellung des Clusters können Sie ein neues Zertifikat als sekundäres hinzufügen.

> [!NOTE]
> Für einen sicheren Cluster muss stets mindestens ein gültiges (nicht widerrufenes oder abgelaufenes) Clusterzertifikat (primär oder sekundär) bereitgestellt sein. Andernfalls funktioniert der Cluster nicht mehr. 90 Tage, bevor alle gültigen Zertifikate ablaufen, generiert das System eine Warnungsablaufverfolgung sowie ein Warnungsintegritätsereignis auf dem Knoten. Zurzeit sendet Service Fabric zu diesem Thema weder E-Mails noch sonstige Benachrichtigungen. 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Hinzufügen eines sekundären Clusterzertifikats mithilfe des Portals

Das sekundäre Clusterzertifikat kann nicht über das Azure-Portal hinzugefügt werden. Hierfür müssen Sie Azure PowerShell verwenden. Der Prozess wird weiter unten in diesem Dokument beschrieben.

## <a name="swap-the-cluster-certificates-using-the-portal"></a>Austauschen der Clusterzertifikate mit dem Portal

Wenn Sie nach der erfolgreichen Bereitstellung eines sekundären Clusterzertifikats das primäre und das sekundäre Zertifikat austauschen möchten, können Sie zum Blatt „Sicherheit“ navigieren und im Kontextmenü die Option „Swap with primary“ (Tauschen gegen primär) wählen, um das sekundäre Zertifikat gegen das primäre Zertifikat zu tauschen.

![Tauschen des Zertifikats][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Entfernen eines Clusterzertifikats mit dem Portal

Für einen sicheren Cluster muss stets mindestens ein gültiges (nicht widerrufenes oder abgelaufenes) Zertifikat (primär oder sekundär) bereitgestellt sein. Andernfalls funktioniert der Cluster nicht mehr.

Zum Entfernen eines sekundären Zertifikats aus der Verwendung für die Clustersicherheit navigieren Sie zum Blatt „Sicherheit“ und wählen im Kontextmenü für das sekundäre Zertifikat die Option „Löschen“.

Wenn Sie das Zertifikat entfernen möchten, das als primär gekennzeichnet ist, müssen Sie es zuerst gegen das sekundäre Zertifikat tauschen und nach Abschluss des Upgradevorgangs dann das sekundäre Zertifikat löschen.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Hinzufügen eines sekundären Zertifikats per Resource Manager PowerShell

Diese Schritte setzen voraus, dass Sie mit der Funktionsweise des Resource Managers vertraut sind und mindestens einen Service Fabric-Cluster mithilfe einer Resource Manager-Vorlage bereitgestellt haben. Außerdem müssen Sie die Vorlage, die Sie zum Einrichten des Clusters verwendet haben, zur Hand haben. Außerdem wird vorausgesetzt, dass Sie mit der Anwendung von JSON vertraut sind.

> [!NOTE]
> Wenn Sie eine Beispielvorlage und Parameter suchen, die Sie als Leitlinie oder Ausgangspunkt verwenden können, laden Sie sie aus diesem [Git-Repository](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) herunter. 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Bearbeiten Ihrer Resource Manager-Vorlage

Damit Sie alles besser verfolgen können, enthält die Beispieldatei „5-VM-1-NodeTypes-Secure_Step2.JSON“ alle Bearbeitungsschritte, die wir ausführen. Die Beispieldatei finden Sie im [Git-Repository](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Achten Sie darauf, dass Sie alle Schritte ausführen**.

**Schritt 1:** Öffnen Sie die Resource Manager-Vorlage, die Sie verwendet haben, um den Cluster bereitzustellen. (Gehen Sie wie folgt vor, wenn Sie das Beispiel aus dem obigen Repository heruntergeladen haben: Verwenden Sie die Datei „5-VM-1-NodeTypes-Secure_Step1.JSON“, um einen sicheren Cluster bereitzustellen, und öffnen Sie anschließend diese Vorlage.)

**Schritt 2:** Fügen Sie die **beiden neuen Parameter** „secCertificateThumbprint“ und „secCertificateUrlValue“ vom Typ „Zeichenfolge“ dem Parameterabschnitt Ihrer Vorlage hinzu. Sie können den folgenden Codeausschnitt kopieren und der Vorlage hinzufügen. Je nach Quelle Ihrer Vorlage kann es sein, dass dies bereits definiert wurde. Wenn dies der Fall ist, können Sie mit dem nächsten Schritt fortfahren. 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

**Schritt 3:** Nehmen Sie Änderungen an der Ressource **Microsoft.ServiceFabric/clusters** vor. Suchen Sie in Ihrer Vorlage nach der Ressourcendefinition „Microsoft.ServiceFabric/clusters“. Unter den Eigenschaften dieser Definition finden Sie das JSON-Tag „Certificate“, das dem folgenden JSON-Codeausschnitt ähneln sollte:

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Fügen Sie ein neues Tag thumbprintSecondary hinzu, und weisen Sie ihm den Wert [parameters('secCertificateThumbprint')] zu.  

Nun sollte die Ressourcendefinition ungefähr wie folgt aussehen (je nach Ihrer Quelle der Vorlage unter Umständen nicht genauso wie der Codeausschnitt unten). 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Wenn Sie ein **Rollover für das Zertifikat** durchführen möchten, geben Sie das neue Zertifikat als primäres Zertifikat an und machen das primäre Zertifikat zum sekundären Zertifikat. Dies führt zum Rollover vom aktuellen primären Zertifikat zum neuen Zertifikat in einem einzigen Bereitstellungsschritt.

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


**Schritt 4:** Nehmen Sie Änderungen an **allen** **Microsoft.Compute/virtualMachineScaleSets**-Ressourcendefinitionen vor. Suchen Sie nach der Ressourcendefinition „Microsoft.Compute/virtualMachineScaleSets“. Scrollen Sie unter „virtualMachineProfile“ zu „publisher“: „Microsoft.Azure.ServiceFabric“.

In den Service Fabric-Herausgebereinstellungen sollte in etwa Folgendes angezeigt werden:

![Json_Pub_Setting1][Json_Pub_Setting1]

Fügen Sie die neuen Zertifikateinträge hinzu.

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

Die Eigenschaften sollten nun wie folgt aussehen:

![Json_Pub_Setting2][Json_Pub_Setting2]

Wenn Sie ein **Rollover für das Zertifikat** durchführen möchten, geben Sie das neue Zertifikat als primäres Zertifikat an und machen das primäre Zertifikat zum sekundären Zertifikat. Dies führt zum Rollover des aktuellen Zertifikats zum neuen Zertifikat in einem einzigen Bereitstellungsschritt. 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
Die Eigenschaften sollten nun wie folgt aussehen:

![Json_Pub_Setting3][Json_Pub_Setting3]


**Schritt 5:** Nehmen Sie Änderungen an **allen** **Microsoft.Compute/virtualMachineScaleSets**-Ressourcendefinitionen vor. Suchen Sie nach der Ressourcendefinition „Microsoft.Compute/virtualMachineScaleSets“. Scrollen Sie unter „OSProfile“ zu „vaultCertificates“. Dies sollte in etwa wie folgt aussehen:


![Json_Pub_Setting4][Json_Pub_Setting4]

Fügen Sie „secCertificateUrlValue“ hinzu. Verwenden Sie den folgenden Codeausschnitt:

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
Der sich ergebende JSON-Code sollte nun in etwa wie folgt aussehen:
![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Stellen Sie sicher, dass Sie die Schritte 4 und 5 für alle Nodetypes/Microsoft.Compute/virtualMachineScaleSets-Ressourcendefinitionen in Ihrer Vorlage wiederholt haben. Falls Sie eine Definition auslassen, wird das Zertifikat nicht auf der entsprechenden VMSS installiert, und Sie erhalten im Cluster unvorhersagbare Ergebnisse, z.B. einen Ausfall des Clusters (falls keine gültigen Zertifikate mehr vorhanden sind, die vom Cluster für Sicherheitszwecke verwendet werden können). Überprüfen Sie dies also noch einmal, bevor Sie fortfahren.
> 
> 


### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Bearbeiten Ihrer Vorlagendatei gemäß der neuen Parameter, die Sie oben hinzugefügt haben
Wenn Sie das Beispiel aus dem [Git-Repository](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) als Leitlinie verwendet haben, können Sie beginnen, Änderungen im Beispiel „5-VM-1-NodeTypes-Secure.paramters_Step2.JSON“ vorzunehmen. 

Bearbeiten Sie die Parameterdatei der Resource Manager-Vorlage, indem Sie die beiden neuen Parameter für „secCertificateThumbprint“ und „secCertificateUrlValue“ hinzufügen. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>Bereitstellen der Vorlage in Azure

- Sie können nun Ihre Vorlage in Azure bereitstellen. Öffnen Sie eine „Azure PowerShell Version 1+“-Eingabeaufforderung.
- Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie das spezifische Azure-Abonnement. Dies ist ein wichtiger Schritt für Sie, falls Sie auf mehrere Azure-Abonnements zugreifen können.

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Testen Sie die Vorlage vor der Bereitstellung. Verwenden Sie die gleiche Ressourcengruppe, für die Ihr Cluster derzeit bereitgestellt wird.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Stellen Sie die Vorlage für Ihre Ressourcengruppe bereit. Verwenden Sie die gleiche Ressourcengruppe, für die Ihr Cluster derzeit bereitgestellt wird. Führen Sie den Befehl New-AzureRmResourceGroupDeployment aus. Sie müssen nicht den Modus angeben, da der Standardwert **incremental**ist.

> [!NOTE]
> Wenn Sie für „Mode“ den Wert „Complete“ festlegen, können Sie versehentlich Ressourcen löschen, die nicht in Ihrer Vorlage enthalten sind. Darum tun Sie es in diesem Szenario nicht.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Hier finden Sie ein ausgefülltes Beispiel für den gleichen PowerShell-Befehl.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie unter Verwendung des neuen Zertifikats eine Verbindung mit Ihrem Cluster her, und führen Sie einige Abfragen aus. Sofern es Ihnen möglich ist. Anschließend können Sie das alte Zertifikat löschen. 

Wenn Sie ein selbstsigniertes Zertifikat verwenden, vergessen Sie nicht, es in Ihren lokalen TrustedPeople-Zertifikatspeicher zu importieren.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
In einer Kurzübersicht finden Sie hier den Befehl zum Herstellen der Verbindung mit einem sicheren Cluster: 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
In einer Kurzübersicht finden Sie hier den Befehl zum Abrufen der Clusterintegrität:

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-to-the-cluster"></a>Bereitstellen von Anwendungszertifikaten für den Cluster

Sie können die gleichen Schritte wie oben in Schritt 5 ausführen, um die Zertifikate aus einem Schlüsseltresor auf den Knoten bereitzustellen. Sie müssen nur die verschiedenen Parameter definieren und verwenden.


## <a name="adding-or-removing-client-certificates"></a>Hinzufügen oder Entfernen von Clientzertifikaten

Zusätzlich zu den Clusterzertifikaten können Sie Clientzertifikate hinzufügen, um Verwaltungsvorgänge für einen Service Fabric-Cluster durchzuführen.

Sie können zwei Arten von Clientzertifikaten hinzufügen: „Administrator“ oder „Schreibgeschützt“. Diese können dann zum Steuern des Zugriffs auf die Administratorvorgänge und Abfragevorgänge im Cluster verwendet werden. Standardmäßig werden die Clusterzertifikate der Liste mit den zulässigen Administratorzertifikaten hinzugefügt.

Sie können eine beliebige Zahl von Clientzertifikaten angeben. Jede Hinzufügung/Löschung führt zu einem Konfigurationsupdate des Service Fabric-Clusters.


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>Hinzufügen von Clientzertifikaten – „Administrator“ oder „Schreibgeschützt“ über das Portal

1. Navigieren Sie zum Blatt „Sicherheit“, und wählen Sie oben auf dem Blatt „Sicherheit“ die Schaltfläche „+ Authentifizierung“.
2. Wählen Sie auf dem Blatt „Authentifizierung hinzufügen“ die Option „Authentifizierungstyp“ > „Read-only client“ (Schreibgeschützter Client) oder „Admin client“ (Admin-Client).
3. Wählen Sie nun die Autorisierungsmethode. Gibt für Service Fabric an, ob dieses Zertifikat anhand des Antragstellernamens oder anhand des Fingerabdrucks gesucht werden soll. Es ist im Allgemeinen keine gute Vorgehensweise in Bezug auf die Sicherheit, die Autorisierungsmethode mit dem Antragstellernamen zu verwenden. 

![Hinzufügen des Clientzertifikats][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>Löschen von Clientzertifikaten – „Administrator“ oder „Schreibgeschützt“ über das Portal

Zum Entfernen eines sekundären Zertifikats aus der Verwendung für die Clustersicherheit navigieren Sie zum Blatt „Sicherheit“ und wählen im Kontextmenü für das spezifische Zertifikat die Option „Löschen“.



## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel, um weitere Informationen zur Clusterverwaltung zu enthalten:

* [Service Fabric-Cluster-Upgradeprozess und Erwartungen](service-fabric-cluster-upgrade.md)
* [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG



