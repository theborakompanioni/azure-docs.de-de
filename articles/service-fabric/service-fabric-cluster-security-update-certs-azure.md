<properties
   pageTitle="Hinzufügen, Rollover und Entfernen von Zertifikaten, die in einem Service Fabric-Cluster in Azure verwendet werden | Microsoft Azure"
   description="Beschreibt das Hochladen eines sekundären Clusterzertifikats und dann das Rollover des alten primären Zertifikats."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="chackdan"/>

# Hinzufügen oder Entfernen von Zertifikaten für einen Service Fabric-Cluster in Azure

Um sich mit der Art und Weise vertraut zu machen, in der Service Fabric x. 509-Zertifikate verwendet, sollten Sie [Szenarien für die Clustersicherheit in Service Fabric](service-fabric-cluster-security.md) lesen. Sie müssen verstehen, was ein Clusterzertifikat ist, und wofür es verwendet wird, bevor Sie den Vorgang fortsetzen.

Service Fabric ermöglicht Ihnen, zwei Clusterzertifikate anzugeben – ein primäres und ein sekundäres – wenn Sie die Zertifikatsicherheit während der Erstellung des Clusters konfigurieren. Weitere Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters im Azure-Portal](service-fabric-cluster-creation-via-portal.md) oder [Creating an Azure Cluster via Azure Resource Manager] \(Erstellen eines Azure-Clusters über den Azure Resource Manager) (service-fabric-cluster-creation-via-Resource Manager.md). Bei der Bereitstellung über den Resource Manager, und wenn Sie nur ein Clusterzertifikat angeben, wird es als primäres Zertifikat verwendet. Nach der Erstellung des Clusters können Sie ein neues Zertifikat als sekundäres hinzufügen.

>[AZURE.NOTE] Für einen sicheren Cluster muss stets mindestens ein gültiges (nicht widerrufenes oder abgelaufenes) Zertifikat (primär oder sekundär) bereitgestellt sein. Andernfalls funktioniert der Cluster nicht mehr. 90 Tage, bevor alle gültigen Zertifikate ablaufen, generiert das System eine Warnungsablaufverfolgung sowie ein Warnungsintegritätsereignis auf dem Knoten. Zurzeit sendet Service Fabric zu diesem Thema weder E-Mails noch sonstige Benachrichtigungen.


## Hinzufügen eines sekundären Zertifikats mithilfe des Portals
Um ein anderes Zertifikat als sekundäres hinzuzufügen, müssen Sie das Zertifikat in einen Azure-Schlüsseltresor hochladen und dann für die virtuellen Computer im Cluster bereitstellen. Weitere Informationen finden Sie unter [Deploy certificates to VMs from a customer-managed Key Vault](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) (Bereitstellen von Zertifikaten für VMs über einen vom Kunden verwalteten Schlüsseltresor).

1. Unter [Schützen eines Service Fabric-Clusters in Azure mit Zertifikaten](service-fabric-secure-azure-cluster-with-certs.md#step-2-upload-the-x509-certificate-to-the-key-vault) erfahren Sie, wie Sie ein X.509-Zertifikat in den Schlüsseltresor hochladen.

2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wechseln Sie zu der Clusterressource, der Sie dieses Zertifikat hinzufügen möchten.
3. Klicken Sie unter **EINSTELLUNGEN** auf **Sicherheit**, um das Blatt zur Clustersicherheit anzuzeigen.
4. Klicken Sie auf die Schaltfläche **+Zertifikat** am oberen Rand des Blatts, um das Blatt **Zertifikat hinzufügen** abzurufen.
5. Wählen Sie „Fingerabdruck des sekundären Zertifikats“ in der Dropdownliste aus, und füllen Sie den Zertifikatfingerabdruck des sekundären Zertifikats aus, das Sie in den Schlüsseltresor hochgeladen haben.

>[AZURE.NOTE]
Anders als während des Workflows der Clustererstellung beziehen wir hier nicht die Details der Informationen zum Schlüsseltresor ein, denn wir gehen davon aus, dass Sie das Zertifikat bereits für die virtuellen Computer bereitgestellt haben, wenn Sie dieses Blatt geöffnet haben, und das Zertifikat bereits im lokalen Zertifikatspeicher in der VMSS-Instanz verfügbar ist.

Klicken Sie auf **Zertifikat**. Eine Bereitstellung wird gestartet, und eine blaue Statusleiste wird im Blatt „Clustersicherheit“ angezeigt.

![Screenshot mit Zertifikatfingerabdrücken im Portal][SecurityConfigurations_02]

Nach erfolgreichem Abschluss der Bereitstellung können Sie entweder das primäre oder sekundäre Zertifikat verwenden, um Verwaltungsvorgänge auf dem Cluster auszuführen.

![Screenshot des Verlaufs der Zertifikatbereitstellung][SecurityConfigurations_03]

Dieser Screenshot zeigt, wie das Blatt „Sicherheit“ nach Abschluss der Bereitstellung aussieht.

![Screenshot des Zertifikatfingerabdrucks nach der Bereitstellung][SecurityConfigurations_08]


Sie können nun das neue Zertifikat verwenden, das Sie gerade hinzugefügt haben, um eine Verbindung mit dem Cluster herzustellen und Vorgänge darauf auszuführen.

>[AZURE.NOTE]
Derzeit besteht keine Möglichkeit zum Austausch von primären und sekundären Zertifikate im Verwaltungsportal, dieses Feature ist in Arbeit. Solange ein gültiges Clusterzertifikat vorhanden ist, funktioniert der Cluster einwandfrei.

## Hinzufügen eines sekundären Zertifikats und dessen Austausch, sodass es zum primären Zertifikat wird, mithilfe von Resource Manager Powershell

Diese Schritte setzen voraus, dass Sie mit der Funktionsweise des Resource Managers vertraut sind und mindestens einen Service Fabric-Cluster mithilfe einer Resource Manager-Vorlage bereitgestellt haben, und die Vorlage, die Sie zum Einrichten des Clusters verwendet haben, zur Hand haben. Außerdem wird vorausgesetzt, dass Sie mit der Anwendung von JSON vertraut sind.

>[AZURE.NOTE]
Wenn Sie eine Beispielvorlage und Parameter suchen, die Sie als Leitlinie oder Ausgangspunkt verwenden können, laden Sie sie aus diesem [Git-Repository] herunter. (https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

#### Bearbeiten Ihrer Resource Manager-Vorlage 

Wenn Sie das Beispiel aus dem [Git-Repository](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) als Leitlinie verwenden, finden Sie diese Änderungen im Beispiel 5-VM-1-NodeTypes-Secure\_Step2.JSON. Verwenden Sie zum Bereitstellen eines sicheren Clusters 5-VM-1-NodeTypes-Secure\_Step1.JSON.

1. Öffnen Sie die Resource Manager-Vorlage, die Sie verwendet haben, um den Cluster bereitzustellen.
2. Fügen Sie einen neuen Parameter secCertificateThumbprint vom Typ „string“ hinzu. Wenn Sie die Resource Manager-Vorlage verwenden, die Sie während der Erstellung aus dem Portal oder aus den Schnellstartvorlagen heruntergeladen haben, dann suchen Sie einfach nach diesem Parameter – er sollte bereits definiert sein.
3. Suchen Sie die Ressourcendefinition Microsoft.ServiceFabric/clusters. Unter den Eigenschaften finden Sie das JSON-Tag „Certificate“, das dem folgenden JSON-Codeausschnitt ähneln sollte.
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
        }
``` 

4. Fügen Sie ein neues Tag thumbprintSecondary hinzu, und weisen Sie ihm den Wert [parameters('secCertificateThumbprint')] zu.

Nun sollte die Ressourcendefinition ungefähr folgendermaßen aussehen (je nach Ihrer Quelle der Vorlage möglicherweise nicht genauso wie der Codeausschnitt unten). Wie Sie im Folgenden sehen, geben Sie hier ein neues Zertifikat als primär an und machen das aktuelle primäre Zertifikat zum sekundären Zertifikat. Dies führt zum Rollover des aktuellen Zertifikats zum neuen Zertifikat in einem einzigen Bereitstellungsschritt.

```JSON

      "properties": {
        "certificate": {
            "thumbprint": "[parameters('certificateThumbprint')]",
            "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
            "x509StoreName": "[parameters('certificateStoreValue')]"
        },

```

#### Bearbeiten Ihrer Vorlagendatei gemäß der neuen Parameter, die Sie oben hinzugefügt haben

Wenn Sie das Beispiel aus dem [Git-Repository](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) als Leitlinie verwendet haben, können Sie beginnen, Änderungen im Beispiel 5-VM-1-NodeTypes-Secure.paramters\_Step2.JSON vorzunehmen.


Bearbeiten Sie die Resource Manager-Vorlagen-Parameterdatei, fügen Sie die neuen Parameter für das secCertificate hinzu, tauschen Sie die Details des vorhandenen primären Zertifikats gegen die des sekundären Zertifikats aus, und ersetzen Sie die Details des primären Zertifikats durch die Details des neuen Zertifikats.

```JSON
    "secCertificateThumbprint": {
      "value": "OLD Primary Certificate Thumbprint"
    },
   "secSourceVaultValue": {
      "value": "OLD Primary Certificate Key Vault location"
    },
    "secCertificateUrlValue": {
      "value": "OLD Primary Certificate location in the key vault"
     },
    "certificateThumbprint": {
      "value": "New Certificate Thumbprint"
    },
    "sourceVaultValue": {
      "value": "New Certificate Key Vault location"
    },
    "certificateUrlValue": {
      "value": "New Certificate location in the key vault"
     },

```

### Bereitstellen der Vorlage in Azure

1. Sie können nun Ihre Vorlage in Azure bereitstellen. Öffnen Sie eine „Azure PowerShell Version 1+“-Eingabeaufforderung.
2. Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie das spezifische Azure-Abonnement. Dies ist ein wichtiger Schritt für Sie, falls Sie auf mehrere Azure-Abonnements zugreifen können.


```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Testen Sie die Vorlage vor der Bereitstellung. Verwenden Sie die gleiche Ressourcengruppe, für die Ihr Cluster derzeit bereitgestellt wird.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Stellen Sie die Vorlage für Ihre Ressourcengruppe bereit. Verwenden Sie die gleiche Ressourcengruppe, für die Ihr Cluster derzeit bereitgestellt wird. Führen Sie den Befehl New-AzureRmResourceGroupDeployment aus. Sie müssen nicht den Modus angeben, da der Standardwert **incremental** ist.

>[AZURE.NOTE]
Wenn Sie für „Mode“ den Wert „Complete“ festlegen, können Sie versehentlich Ressourcen löschen, die nicht in Ihrer Vorlage enthalten sind. Darum tun Sie es in diesem Szenario nicht.
   

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

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie unter Verwendung des neuen Zertifikats eine Verbindung mit Ihrem Cluster her, und führen Sie einige Abfragen aus. Sofern es Ihnen möglich ist. Anschließend können Sie das alte primäre Zertifikat löschen.

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
 
## Entfernen des alten Zertifikats mithilfe des Portals
Es folgt der Prozess zum Entfernen eines alten Zertifikats, damit der Cluster es nicht mehr verwendet:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu den Sicherheitseinstellungen Ihres Clusters.
2. Klicken Sie mit der rechten Maustaste auf das Zertifikat, das Sie entfernen möchten.
3. Wählen Sie „Löschen“, und folgen Sie den Anweisungen.

[SecurityConfigurations_05]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_05.png


## Nächste Schritte
Lesen Sie die folgenden Artikel, um weitere Informationen zur Clusterverwaltung zu enthalten:

- [Service Fabric-Cluster-Upgradeprozess und Erwartungen](service-fabric-cluster-upgrade.md)
- [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md)


<!--Image references-->
[SecurityConfigurations_02]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_02.png
[SecurityConfigurations_03]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_03.png
[SecurityConfigurations_05]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_05.png
[SecurityConfigurations_08]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_08.png

<!---HONumber=AcomDC_0817_2016-->

