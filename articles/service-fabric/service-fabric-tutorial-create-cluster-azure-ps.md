---
title: Erstellen eines Service Fabric-Clusters in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Windows- oder Linux-Service Fabric-Cluster in Azure mithilfe von PowerShell erstellen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 80c5a2a43302e1cc8ec3b4298eb393a1861252d3
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="create-a-secure-cluster-on-azure-using-powershell"></a>Erstellen eines sicheren Clusters in Azure mithilfe von PowerShell
Dieses Tutorial zeigt Ihnen, wie Sie einen Service Fabric-Cluster (Windows oder Linux) erstellen können, der in Azure ausgeführt wird. Wenn Sie fertig sind, verfügen Sie über einen Cluster, der in der Cloud ausgeführt wird und für den Sie Anwendungen bereitstellen können.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines sicheren Service Fabric-Clusters in Azure mithilfe von PowerShell
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster über PowerShell
> * Entfernen eines Clusters

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie das [Service Fabric SDK und das PowerShell-Modul](service-fabric-get-started.md).
- Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Mit dem folgenden Verfahren wird ein Service Fabric-Vorschaucluster mit einem einzelnen Knoten (einzelner virtueller Computer) erstellt. Der Cluster wird durch ein selbstsigniertes Zertifikat geschützt, das zusammen mit dem Cluster erstellt und anschließend in einem Schlüsseltresor platziert wird. Einzelknotencluster können nicht über einen virtuellen Computer hinaus skaliert werden, und Vorschaucluster können nicht auf neuere Versionen aktualisiert werden.

Zum Berechnen der Kosten, die durch das Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).
Weitere Informationen zum Erstellen von Service Fabric-Clustern finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="create-the-cluster-using-azure-powershell"></a>Erstellen des Clusters mithilfe von Azure PowerShell
1. Laden Sie eine lokale Kopie der Azure Resource Manager-Vorlage und die Parameterdatei aus folgendem GitHub-Repository herunter: [Azure Resource Manager template for Service Fabric (Azure Resource Manager-Vorlage für Service Fabric)](https://aka.ms/securepreviewonelineclustertemplate).  *azuredeploy.json* ist die Azure Resource Manager-Vorlage, die einen Service Fabric-Cluster definiert. *azuredeploy.parameters.json* ist eine Parameterdatei, mit der Sie die Bereitstellung des Clusters anpassen können.

2. Passen Sie die folgenden Parameter in der Parameterdatei *azuredeploy.parameters.json* an:

   | Parameter       | Beschreibung | Empfohlener Wert |
   | --------------- | ----------- | --------------- |
   | clusterLocation | Die Azure-Region, in der der Cluster bereitgestellt werden soll. | *Beispielsweise westeurope, eastasia, eastus* |
   | clusterName     | Der Name des Clusters, den Sie erstellen möchten. | *Beispielsweise bobs-sfpreviewcluster* |
   | adminUserName   | Das lokale Administratorkonto auf dem virtuellen Clustercomputer. | *Alle gültigen Windows Server-Benutzernamen* |
   | adminPassword   | Das Kennwort des lokalen Administratorkontos auf dem virtuellen Clustercomputer. | *Alle gültigen Windows Server-Kennwörter* |
   | clusterCodeVersion | Die auszuführende Service Fabric-Version. (255.255.X.255 sind Vorschauversionen.) | **255.255.5718.255** |
   | vmInstanceCount | Die Anzahl der virtuellen Computer in Ihrem Cluster (kann 1 oder 3-99 sein). | **1** | *Geben Sie bei einem Vorschaucluster nur einen einzelnen virtuellen Computer an.* |

3. Öffnen Sie eine PowerShell-Konsole, melden Sie sich bei Azure an, und wählen Sie das Abonnement aus, in dem Sie den Cluster bereitstellen möchten:

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. Erstellen und verschlüsseln Sie ein Kennwort für das von Service Fabric zu verwendende Zertifikat.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. Erstellen Sie den Cluster und das dazugehörige Zertifikat durch Ausführen des folgenden Befehls:

   ```powershell
      New-AzureRmServiceFabricCluster
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >Der Parameter `-CertificateSubjectName` muss auf den in der Parameterdatei angegebenen Parameter „clusterName“ sowie auf die mit der gewählten Azure-Region verknüpfte Domäne abgestimmt sein (Beispiel: `clustername.eastus.cloudapp.azure.com`).

Nach Abschluss des Konfigurationsvorgangs werden Informationen zum in Azure erstellten Cluster ausgegeben. Darüber hinaus wird das Clusterzertifikat in das Verzeichnis „-CertificateOutputFolder“ an dem Pfad kopiert, den Sie für diesen Parameter angegeben haben. Dieses Zertifikat wird für den Zugriff auf Service Fabric Explorer und zum Anzeigen der Clusterintegrität benötigt.

Notieren Sie sich die URL für Ihren Cluster. Diese kann in etwa wie folgt aussehen: *https://mycluster.westeurope.cloudapp.azure.com:19080*

## <a name="modify-the-certificate--access-service-fabric-explorer"></a>Ändern des Zertifikats und Zugreifen auf Service Fabric Explorer ##

1. Doppelklicken Sie auf das Zertifikat, um den Zertifikatimport-Assistenten zu öffnen.

2. Verwenden Sie die Standardeinstellungen, aktivieren Sie jedoch das Kontrollkästchen **Schlüssel als exportierbar markieren.** im Schritt **Schutz für den privaten Schlüssel**. Bei der Konfiguration von Azure Container Registry für die Authentifizierung des Service Fabric-Clusters im weiteren Verlauf des Tutorials muss das Zertifikat in Visual Studio exportiert werden.

3. Sie können nun Service Fabric Explorer in einem Browser öffnen. Navigieren Sie hierzu in einem Webbrowser zur **ManagementEndpoint**-URL für Ihren Cluster, und wählen Sie das auf Ihrem Computer gespeicherte Zertifikat aus.

>[!NOTE]
>Beim Öffnen von Service Fabric Explorer wird ein Zertifikatfehler angezeigt, da Sie ein selbstsigniertes Zertifikat verwenden. In Edge müssen Sie auf *Details* und dann auf den Link *Webseite trotzdem laden* klicken. In Chrome müssen Sie auf *Advanced* (Erweitert) und dann auf den Link *proceed* (Fortfahren) klicken.

>[!NOTE]
>Wenn bei der Clustererstellung ein Fehler auftritt, können Sie den Befehl, mit dem die bereits bereitgestellten Ressourcen aktualisiert werden, erneut ausführen. Wenn ein Zertifikat als Teil der fehlerhaften Bereitstellung erstellt wurde, wird ein neues generiert. Informationen zum Beheben von Fehlern bei der Clustererstellung finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="connect-to-the-secure-cluster"></a>Herstellen einer Verbindung mit dem sicheren Cluster
Stellen Sie mithilfe des Service Fabric-PowerShell-Moduls, das zusammen mit dem Service Fabric SDK installiert wird, eine Verbindung mit dem Cluster her.  Zunächst installieren Sie das Zertifikat im persönlichen Speicher des aktuellen Benutzers auf Ihrem Computer.  Führen Sie den folgenden PowerShell-Befehl aus:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Sie können nun die Verbindung mit Ihrem sicheren Cluster herstellen.

Das **Service Fabric**-PowerShell-Modul bietet viele Cmdlets zum Verwalten von Service Fabric-Clustern, Anwendungen und Diensten.  Verwenden Sie das Cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster), um eine Verbindung mit dem Cluster herzustellen. Die Informationen zum Zertifikatfingerabdruck und zum Verbindungsendpunkt finden Sie in der Ausgabe aus einem vorherigen Schritt.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Überprüfen Sie mithilfe des Cmdlets [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth), ob Sie verbunden sind und der Cluster fehlerfrei ist.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ein Cluster besteht neben der Clusterressource selbst noch aus weiteren Azure-Ressourcen. Die einfachste Möglichkeit zum Löschen des Clusters und aller darin genutzten Ressourcen besteht darin, die Ressourcengruppe zu löschen.

Melden Sie sich bei Azure an, und wählen Sie die Abonnement-ID aus, mit der Sie den Cluster entfernen möchten.  Sie finden Ihre Abonnement-ID, indem Sie sich beim [Azure-Portal](http://portal.azure.com) anmelden. Löschen Sie die Ressourcengruppe und alle Clusterressourcen mithilfe des Cmdlets [Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Service Fabric-Clusters in Azure
> * Sichern des Clusters mit einem X.509-Zertifikat
> * Herstellen einer Verbindung mit dem Cluster über PowerShell
> * Entfernen eines Clusters

Als Nächstes fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie eine vorhandene Anwendung bereitstellen.
> [!div class="nextstepaction"]
> [Bereitstellen einer .NET-Anwendung mit Docker Compose](service-fabric-host-app-in-a-container.md)

