---
title: Integrieren von Protokollen aus Azure Key Vault mithilfe von Event Hubs | Microsoft-Dokumentation
description: "Dieses Tutorial zeigt die nötigen Schritte, um Key Vault-Protokolle mithilfe der Azure-Protokollintegration für SIEM verfügbar zu machen"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 4503234080e0bf737dad2e18907b47c3bf39d9da
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Tutorial zur Azure-Protokollintegration: Verarbeiten von Azure Key Vault-Ereignissen mithilfe von Event Hubs

Mit der Azure-Protokollintegration (AzLog) können Sie protokollierte Ereignisse abrufen und für Ihr Sicherheits- und Ereignisverwaltungssystem (SIEM) verfügbar machen. Dieses Tutorial veranschaulicht anhand eines Beispiels, wie Sie über Event Hubs bezogene Protokolle mithilfe der Azure-Protokollintegration verarbeiten können. Führen Sie die einzelnen Schritte des Beispiels in diesem Artikel aus, um zu erfahren, wie die Azure-Protokollintegration und Event Hubs zusammenarbeiten und wie der jeweilige Schritt die Lösung unterstützt. Auf der Grundlage dieser Erkenntnisse können Sie dann eigene Schritte entwickeln, die die speziellen Anforderungen Ihres Unternehmens unterstützen.

>[!WARNING]
Die Schritte und Befehle in diesem Tutorial dienen lediglich als Beispiele und sollten nicht direkt übernommen werden. Verwenden Sie die PowerShell-Befehle nicht unverändert in Ihrer Liveumgebung. Die Befehle müssen an die jeweilige Umgebung angepasst werden.


Dieses Tutorial führt Sie durch den Prozess, die Azure Key Vault-Aktivitäten, die in einem Event Hub protokolliert sind, für Ihr SIEM-System als JSON-Dateien verfügbar zu machen. Sie können dann Ihr SIEM-System so konfigurieren, dass es die JSON-Dateien verarbeiten kann.

>[!NOTE]
>Die meisten Schritte in diesem Tutorial beinhalten die Konfiguration von Schlüsseltresoren, Speicherkonten und Event Hubs. Die entsprechenden Schritte für die Azure-Protokollintegration befinden sich am Ende dieses Tutorials. Führen Sie diese Schritte nicht in einer Produktionsumgebung aus; sie sind ausschließlich für eine Laborumgebung vorgesehen. Vor der Verwendung in einer Produktionsumgebung müssen die Schritte entsprechend angepasst werden.

Die Informationen, die Ihnen währenddessen bereitgestellt werden, helfen Ihnen dabei, die Gründe für jeden Schritt nachzuvollziehen. Links zu anderen Artikeln liefern Ihnen weitere Details zu bestimmten Themen.

Weitere Informationen zu den Diensten, die in diesem Tutorial verwendet werden, finden Sie hier: 

- [Azure-Schlüsseltresor](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure-Protokollintegration](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Anfangssetup

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

1. Ein Azure-Abonnement und ein zum Abonnement gehöriges Konto mit Administratorrechten. Falls Sie über kein Abonnement verfügen, können Sie [ein kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen.
 
2. Ein System mit Zugriff auf das Internet, das die Voraussetzungen für die Installation von Azure-Protokollintegration erfüllt. Das System kann sich auf einem Clouddienst befinden oder lokal gehostet sein.

3. [Azure-Protokollintegration](https://www.microsoft.com/download/details.aspx?id=53324) muss installiert sein. So installieren Sie sie:
   1. Stellen Sie über Remotedesktop eine Verbindung zu dem in Schritt 2 erwähnten System her.
   2. Kopieren Sie das Installationsprogramm für die Azure-Protokollintegration auf das System. Sie können [die Installationsdateien herunterladen](https://www.microsoft.com/download/details.aspx?id=53324).
   3. Starten Sie das Installationsprogramm und akzeptieren Sie die Microsoft-Software-Lizenzbedingungen.
   4. Wenn Sie Telemetriedaten bereitstellen möchten, lassen Sie das Kontrollkästchen aktiviert. Wenn Sie keine Nutzungsinformationen an Microsoft senden möchten, deaktivieren Sie das Kontrollkästchen.
   
   Weitere Informationen zur Azure-Protokollintegration und wie Sie diese installieren, finden Sie unter [Azure Log Integration Azure Diagnostics logging and Windows Event Forwarding (Azure-Diagnoseprotokollierung der Azure-Protokollintegration und Windows-Ereignisweiterleitung)](security-azure-log-integration-get-started.md).

4. Die neueste Version von PowerShell
 
   Wenn Sie Windows Server 2016 installiert haben, haben Sie mindestens PowerShell 5.0. Wenn Sie eine andere Version von Windows Server verwenden, haben Sie möglicherweise eine frühere Version von PowerShell installiert. Sie können die Version überprüfen, indem Sie ```get-host``` in ein PowerShell-Fenster eingeben. Wenn Sie nicht PowerShell 5.0 installiert haben, können Sie [es herunterladen](https://www.microsoft.com/download/details.aspx?id=50395).

   Nachdem Sie mindestens PowerShell 5.0 haben, können Sie damit fortfahren, die neueste Version zu installieren:
   1. Geben Sie in einem PowerShell-Fenster ```Install-Module Azure``` ein und drücken Sie die EINGABETASTE. Führen Sie die Installationsschritte durch. 
   2. Geben Sie ```Install-Module AzureRM``` ein und drücken Sie die EINGABETASTE. Führen Sie die Installationsschritte durch.

   Weitere Informationen finden Sie unter [Installieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## <a name="create-supporting-infrastructure-elements"></a>Erstellen der unterstützenden Infrastrukturelemente

1. Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und navigieren Sie zu **C:\Programme\Microsoft Azure Log Integration**.
2. Importieren Sie die AzLog-Cmdlets, indem Sie das Script „LoadAzLogModule.ps1“ ausführen. (Beachten Sie „.\“ im folgenden Befehl.) Geben Sie `.\LoadAzLogModule.ps1` ein und drücken Sie die EINGABETASTE.
Die Ausgabe sollte folgendermaßen aussehen:</br>

   ![Liste der geladenen Module](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Geben Sie `Login-AzureRmAccount` ein und drücken Sie die EINGABETASTE. Geben Sie im Anmeldefenster die Anmeldeinformationen für das Abonnement ein, das Sie für dieses Tutorial verwenden.

   >[!NOTE]
   >Wenn Sie sich von diesem Computer aus zum ersten Mal bei Azure anmelden, wird Ihnen eine Meldung angezeigt, um Microsoft zu gestatten, Ihre PowerShell-Nutzungsdaten zu sammeln. Es wird empfohlen, die Sammlung der Daten zu aktivieren, da diese zur Verbesserung von Azure PowerShell verwendet werden.

4. Nach der erfolgreichen Authentifizierung sind Sie angemeldet und sehen die Informationen im folgenden Screenshot. Notieren Sie sich die Abonnement-ID und den Abonnementnamen, da Sie diese benötigen, um spätere Schritte abzuschließen.

   ![PowerShell-Fenster](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Erstellen Sie Variablen zum Speichern von Werten, die später verwendet werden. Geben Sie jede der folgenden PowerShell-Zeilen ein und drücken Sie nach jeder die EINGABETASTE. Möglicherweise müssen Sie die Werte entsprechend Ihrer Umgebung anpassen.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Ihr Abonnementname lautet möglicherweise anders. Er wird in der Ausgabe des vorherigen Befehls angezeigt.)
    - ```$location = 'West US'``` (Diese Variable dient zum Übergeben des gewünschten Erstellungsorts für Ihre Ressourcen. Sie kann auf einen beliebigen anderen Standort festgelegt werden.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random``` (Der Name ist frei wählbar, sollte jedoch ausschließlich aus Kleinbuchstaben und Zahlen bestehen.)
    - ``` $storageName = $name``` (Diese Variable wird für den Speicherkontonamen verwendet.)
    - ```$rgname = $name ``` (Diese Variable wird für den Ressourcengruppennamen verwendet.)
    - ``` $eventHubNameSpaceName = $name``` (Dies ist der Name des Event Hub-Namespace.)
6. Geben Sie das Abonnement an, das Sie verwenden möchten:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Erstellen Sie eine Ressourcengruppe:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   Wenn Sie `$rg` eingeben und an diesem Punkt die EINGABETASTE drücken, sollte die Ausgabe in etwa wie im folgenden Screenshot aussehen:

   ![Ausgabe nach der Erstellung einer Ressourcengruppe](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Erstellen Sie ein Speicherkonto, das verwendet wird, um Statusinformationen zu verfolgen:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Erstellen Sie den Event Hub-Namespace. Dieser ist erforderlich, um einen Event Hub zu erstellen.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. Rufen Sie die Regel-ID ab, die für den Insights-Anbieter verwendet wird:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Rufen Sie alle möglichen Azure-Standorte ab und fügen Sie deren Namen zu einer Variable hinzu, die in einem späteren Schritt verwendet werden kann:
    
    1. ```$locationObjects = Get-AzureRMLocation```    
    2. ```$locations = @('global') + $locationobjects.location```
    
    Wenn Sie `$locations` eingeben und an diesem Punkt die EINGABETASTE drücken, werden Ihnen die Standortnamen ohne die zusätzlichen Informationen angezeigt, die von Get-AzureRmLocation zurückgegeben werden.
12. Erstellen eines Azure Resource Manager-Protokollprofils: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Weitere Informationen zu Azure-Protokollprofilen finden Sie unter [Overview of the Azure Activity Log (Übersicht über das Azure-Aktivitätsprotokoll)](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

>[!NOTE]
>Möglicherweise erhalten Sie eine Fehlermeldung, wenn Sie versuchen, ein Protokollprofil zu erstellen. Sie können dann die Dokumentation zu Get-AzureRmLogProfile und Remove-AzureRmLogProfile konsultieren. Wenn Sie Get-AzureRmLogProfile ausführen, sehen Sie die Informationen zum Protokollprofil. Sie können das vorhandene Protokollprofil löschen, indem Sie ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` eingeben und die EINGABETASTE drücken.
>
>![Fehler beim Profil des Ressourcen-Manager](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

1. Erstellen des Schlüsseltresors:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Konfigurieren der Protokollierung für den Schlüsseltresor:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Generieren der Protokollaktivität

Anforderungen müssen an Key Vault gesendet werden, um Protokollaktivität zu generieren. Aktionen wie z.B. das Generieren eines Schlüssels und das Speichern oder Lesen von Geheimnissen aus Key Vault erstellen Protokolleinträge.

1. Anzeigen der aktuellen Speicherschlüssel:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Generieren Sie einen neuen **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. Lassen Sie den Schlüssel erneut anzeigen und Sie sehen, dass **key2** einen anderen Wert enthält:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Legen Sie ein Geheimnis fest und lassen Sie es lesen, um weitere Protokolleinträge zu erzeugen:
    
   1. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)```    
   2. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Zurückgegebenes Geheimnis](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Konfigurieren der Azure-Protokollintegration

Nachdem Sie nun alle erforderlichen Elemente konfiguriert haben, damit Key Vault in einen Event Hub protokolliert, müssen Sie die Azure-Protokollintegration konfigurieren:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Führen Sie den AzLog-Befehl für jeden Event Hub aus:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Etwa eine Minute, nachdem Sie die letzten beiden Befehle ausgeführt haben, sollten Sie sehen, wie die JSON-Dateien erzeugt werden. Sie können das bestätigen, indem Sie das Verzeichnis **C:\users\AzLog\EventHubJson** überwachen.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Log Integration FAQ (Azure-Protokollintegration (FAQ))](security-azure-log-integration-faq.md)
- [Get started with Azure Log Integration (Erste Schritte mit der Azure-Protokollintegration)](security-azure-log-integration-get-started.md)
- [Integrate logs from Azure resources into your SIEM systems (Integrieren von Protokollen aus Azure-Ressourcen in Ihre SIEM-Systeme)](security-azure-log-integration-overview.md)

