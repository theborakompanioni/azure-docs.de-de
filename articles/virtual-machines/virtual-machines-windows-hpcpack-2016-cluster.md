---
title: HPC Pack 2016-Cluster in Azure | Microsoft Docs
description: Erfahren Sie, wie Sie einen Cluster mit HPC Pack 2016 in Azure bereitstellen.
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 7e7dc6b6d58da556dfa07d5d21b3e70483d36ef9
ms.openlocfilehash: 688f3f0885606949a265300af215f416e8f94155


---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>Bereitstellen eines HPC Pack 2016-Clusters in Azure

Befolgen Sie die in diesem Artikel beschriebenen Schritte, um einen [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029)-Cluster auf virtuellen Azure-Computern bereitzustellen. HPC Pack ist die kostenlose HPC-Lösung von Microsoft, die auf Microsoft Azure und Windows Server-Technologien basiert und eine breite Palette von HPC-Workloads unterstützt.

Verwenden Sie eine der [Azure Resource Manager-Vorlagen](https://github.com/MsHpcPack/HPCPack2016), um den HPC Pack 2016-Cluster bereitzustellen. Sie haben die Wahl zwischen verschiedenen Clustertopologien mit unterschiedlich vielen Clusterhauptknoten und können zwischen Linux- und Windows-Computeknoten wählen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="pfx-certificate"></a>PFX-Zertifikat

Für einen Microsoft HPC Pack 2016-Cluster ist ein Personal Information Exchange-Zertifikat (PFX) erforderlich, um die Kommunikation zwischen den HPC-Knoten zu schützen. Das Zertifikat muss die folgenden Anforderungen erfüllen:

* Privater Schlüssel mit der Fähigkeit zum Schlüsselaustausch
* Schlüsselverwendung mit digitale Signatur und Schlüsselchiffrierung
* Erweiterte Schlüsselverwendung mit Clientauthentifizierung und die Serverauthentifizierung

Laden Sie vor dem Bereitstellen des HPC-Clusters das Zertifikat in den [Azure Key Vault](../key-vault/index.md) als Geheimnis hoch, und notieren Sie sich die folgenden Informationen für die Nutzung während der Bereitstellung: **Tresorname**, **Tresorressourcengruppe**, **Zertifikats-URL** und **Zertifikatfingerabdruck**.

Nachfolgend sehen Sie beispielhaftes PowerShell-Skript zum Hochladen des Zertifikats. Weitere Informationen zum Hochladen eines Zertifikats in einen Azure Key Vault finden Sie unter [Erste Schritte mit Azure Key Vault](../key-vault/key-vault-get-started.md).

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Unterstützte Topologien

Wählen Sie eine der [Azure Resource Manager-Vorlagen](https://github.com/MsHpcPack/HPCPack2016), um den HPC Pack 2016-Cluster bereitzustellen. Die folgenden Abbildungen zeigen die allgemeine Architektur von drei unterstützten Clustertopologien. Hochverfügbarkeitstopologien umfassen mehrere Clusterhauptknoten.

1. Hochverfügbarkeitscluster mit Active Directory-Domäne (AD)

    ![Hochverfügbarkeitscluster in einer AD-Domäne](./media/virtual-machines-windows-hpcpack-2016-cluster/haad.png)



2. Hochverfügbarkeitscluster ohne Active Directory-Domäne

    ![Hochverfügbarkeitscluster ohne AD-Domäne](./media/virtual-machines-windows-hpcpack-2016-cluster/hanoad.png)

3. Cluster mit einem einzelnen Hauptknoten

   ![Cluster mit einzelnem Hauptknoten](./media/virtual-machines-windows-hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Bereitstellen eines Clusters

Wählen Sie zum Erstellen eines Clusters eine Vorlage aus, und klicken Sie auf **In Azure bereitstellen**. Geben Sie für die Vorlage im [Azure-Portal](https://portal.azure.com) die Parameter an – wie in den folgenden Schritte beschrieben. Mit jeder Vorlage können alle Azure-Ressourcen erstellt werden, die für die HPC-Clusterinfrastruktur erforderlich sind. Zu den Ressourcen zählen virtuelle Azure-Netzwerke, öffentliche IP-Adressen, Load Balancer (nur für Hochverfügbarkeitscluster), Netzwerkschnittstellen, Verfügbarkeitsgruppen, Speicherkonten und virtuelle Computer.

### <a name="step-1-select-the-subscription-location-and-resource-group"></a>Schritt 1: Auswählen von Abonnement, Speicherort und Ressourcengruppe

**Abonnement** und **Speicherort** müssen den Angaben entsprechen, die Sie beim Hochladen Ihres PFX-Zertifikats gemacht haben (siehe Voraussetzungen). Es wird empfohlen, eine **Ressourcengruppe** für die Bereitstellung zu erstellen.

### <a name="step-2-specify-the-parameter-settings"></a>Schritt 2: Festlegen der Parametereinstellungen

Geben Sie Werte für die Vorlagenparameter ein, oder ändern Sie diese. Klicken Sie auf das Symbol neben jedem Parameter, um Hilfeinformationen anzuzeigen. Siehe dazu auch den Leitfaden für [verfügbare VM-Größen](virtual-machines-windows-sizes.md).

Geben Sie die Werte an, die Sie für die folgenden Parameter in den Voraussetzungen festgelegt haben: **Tresorname**, **Tresorressourcengruppe**, **Zertifikats-URL** und **Zertifikatfingerabdruck**.

###<a name="step-3-review-legal-terms-and-create"></a>Schritt 3: Prüfen der rechtlichen Bedingungen und Starten der Erstellung
Klicken Sie **Rechtliche Bedingungen prüfen**, um die Bedingungen zu prüfen. Wenn Sie zustimmen, klicken Sie auf **Kaufen**, und klicken Sie dann auf **Erstellen**, um mit der Bereitstellung zu beginnen.

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster
1. Nachdem der HPC Pack-Cluster bereitgestellt wurde, navigieren Sie zum [Azure-Portal](https://portal.azure.com). Klicken Sie auf **Ressourcengruppen**, und suchen Sie nach der Ressourcengruppe, in der der Cluster bereitgestellt wurde. Sie können hier nun die Hauptknoten der virtuellen Computer sehen.

    ![Clusterhauptknoten im Portal](./media/virtual-machines-windows-hpcpack-2016-cluster/clusterhns.png)

2. Klicken Sie auf einen Hauptknoten (klicken Sie in einem Hochverfügbarkeitscluster auf einen beliebigen Hauptknoten). Unter **Zusammenfassung** finden Sie die öffentliche IP-Adresse oder den vollständigen DNS-Namen des Clusters.

    ![Clusterverbindungseinstellungen](./media/virtual-machines-windows-hpcpack-2016-cluster/clusterconnect.png)

3. Klicken Sie auf **Verbinden**, um sich über Remotedesktop mit dem angegebenen Administratorbenutzernamen bei einem der Hauptknoten anzumelden. Wenn sich der von Ihnen bereitgestellte Cluster in einer Active Directory-Domäne befindet, weist der Benutzername das folgende Format auf: <privateDomainName>\<Administratorbenutzername> (zum Beispiel „hpc.local\hpcadmin“).

## <a name="next-steps"></a>Nächste Schritte
* Senden Sie Aufträge an Ihren Cluster. Weitere Informationen finden Sie unter [Übermitteln von HPC-Aufträgen von einem lokalen Computer an einen in Azure bereitgestellten HPC Pack-Cluster](virtual-machines-windows-hpcpack-cluster-submit-jobs.md) und [Verwalten eines HPC Pack 2016-Clusters in Azure mithilfe von Azure Active Directory](virtual-machines-windows-hpcpack-cluster-active-directory.md).




<!--HONumber=Nov16_HO3-->


