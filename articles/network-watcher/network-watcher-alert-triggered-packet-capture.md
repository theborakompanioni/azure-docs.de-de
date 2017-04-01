---
title: "Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Azure Functions | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie eine per Warnung ausgelöste Paketerfassung mit Azure Network Watcher erstellen."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 5f31b24a0d46b9d557a55b3c9d0cd7748ecb9c33
ms.lasthandoff: 03/21/2017


---
# <a name="use-packet-capture-to-do-proactive-network-monitoring-with-azure-functions"></a>Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Azure Functions

Mit der Network Watcher-Paketerfassung werden Erfassungssitzungen zum Nachverfolgen des eingehenden und ausgehenden Datenverkehrs eines virtuellen Computers erstellt. Die Erfassungsdatei kann über einen Filter verfügen, der so definiert ist, dass nur der Datenverkehr nachverfolgt wird, den Sie überwachen möchten. Diese Daten werden dann in einem Speicherblob oder lokal auf dem Gastcomputer gespeichert. Diese Funktion kann per Remotezugriff über Automationsszenarien wie Azure Functions gestartet werden. Mit der Paketerfassung können Sie proaktive Erfassungen basierend auf definierten Netzwerkanomalien durchführen. Weitere Verwendungszwecke sind das Erfassen von Netzwerkstatistiken, das Gewinnen von Informationen zu Netzwerkangriffen, das Debuggen der Kommunikation zwischen Client und Server und vieles mehr.

In Azure bereitgestellte Ressourcen werden rund um die Uhr ausgeführt. Für Sie bzw. Ihre Mitarbeiter ist es nicht möglich, den Status aller Ressourcen rund um die Uhr lückenlos aktiv zu überwachen. Was passiert, wenn nachts um 2 Uhr ein Problem auftritt?

Durch den Einsatz von Network Watcher, Warnungen und Functions im Azure-Ökosystem können Sie mit den entsprechenden Daten und Tools proaktiv auf Probleme im Netzwerk reagieren, um diese zu lösen.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Beispiel werden von Ihrer VM ungewöhnlich viele TCP-Segmente gesendet, und Sie möchten dazu eine Warnung erhalten. Als Beispiel werden TCP-Segmente verwendet, aber Sie können eine beliebige Warnungsbedingung nutzen. Bei einer Warnung möchten Sie über Paketebenendaten verfügen, um zu verstehen, warum es zu einem Anstieg der Kommunikation gekommen ist. Sie können dann Maßnahmen einleiten, um die reguläre Kommunikation des Computers wiederherzustellen.
Bei diesem Szenario wird angenommen, dass Sie über eine vorhandene Instanz von Network Watcher und eine Ressourcengruppe mit einem gültigen virtuellen Computer verfügen, der genutzt werden kann.

## <a name="scenario"></a>Szenario

Zur Automatisierung dieses Prozesses erstellen und verbinden wir eine Warnung auf unserer VM, die ausgelöst wird, wenn der Vorfall eintritt, sowie eine Azure-Funktion für den Aufruf von Network Watcher.

Dieses Szenario:

* Erstellt eine Azure-Funktion, die eine Paketerfassung startet.
* Erstellt eine Warnungsregel auf einem virtuellen Computer
* Konfiguriert die Warnungsregel für den Aufruf der Azure-Funktion

## <a name="creating-an-azure-function-and-overview"></a>Erstellen einer Azure-Funktion und Übersicht

Im ersten Schritt wird eine Azure-Funktion zum Verarbeiten der Warnung und Erstellen einer Paketerfassung erstellt. 

Die folgende Liste enthält eine Übersicht über den Workflow.

1. Auf Ihrer VM wird eine Warnung ausgelöst.
1. Für die Warnung wird Ihre Azure-Funktion per Webhook aufgerufen.
1. Ihre Azure-Funktion verarbeitet die Warnung und startet eine Network Watcher-Paketerfassungssitzung.
1. Die Paketerfassung wird auf dem virtuellen Computer ausgeführt, und der Datenverkehr wird erfasst. 
1. Die Erfassungsdatei wird für die Überprüfung und Diagnose in ein Speicherkonto hochgeladen. 

Die Erstellung einer Azure-Funktion ist im Portal möglich. Informationen hierzu finden Sie unter [Erstellen Sie Ihre erste Funktion in Azure Functions](../azure-functions/functions-create-first-azure-function.md). Für dieses Beispiel haben wir eine PowerShell-Funktion vom Typ „HttpTrigger“ ausgewählt. Für dieses Beispiel sind Anpassungen erforderlich, die in den folgenden Schritten erläutert werden:

![Funktionsbeispiel][functions1]

> [!NOTE]
> Die PowerShell-Vorlage ist experimentell und nicht in vollem Umfang unterstützt.

## <a name="adding-modules"></a>Hinzufügen von Modulen

Um die Network Watcher-PowerShell-Cmdlets zu verwenden, muss das neueste PowerShell-Modul in die Funktionen-App hochgeladen worden sein.

1. Führen Sie auf dem lokalen Computer mit installierten aktuellsten Azure PowerShell-Modulen den folgenden PowerShell-Befehl aus:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    Dadurch erhalten Sie den lokalen Pfad Ihrer Azure PowerShell-Module. Diese Ordner werden in einem späteren Schritt verwendet. Dies sind die in diesem Szenario verwendeten Module:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![PowerShell-Ordner][functions5]

1. Navigieren Sie zu **Funktionen-App-Einstellungen** > **Zum App Service-Editor wechseln**.

    ![Funktionen-App][functions2]

1. Klicken Sie mit der rechten Maustaste auf den AlertPacketCapturePowershell-Ordner, und erstellen Sie einen Ordner mit dem Namen **azuremodules**. Fahren Sie mit dem Erstellen von Unterordnern für jedes benötigte Modul fort.

    ![Kudu-Funktionen][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Klicken Sie mit der rechten Maustaste auf den Unterordner **AzureRM.Network**, und klicken Sie auf **Dateien hochladen**. Navigieren Sie zum Installationsspeicherort Ihrer Azure-Module, wählen Sie im lokalen AzureRM.Network-Ordner alle Dateien aus, und klicken Sie auf **Ok**.  Wiederholen Sie diese Schritte für AzureRM.Profile und AzureRM.Resources.

    ![Hochladen von Dateien][functions6]

1. Nach dem Abschluss dieses Vorgangs sollte jeder Ordner die PowerShell-Moduldateien von Ihrem lokalen Computer enthalten.

    ![PowerShell-Dateien][functions7]

## <a name="authentication"></a>Authentifizierung

Um die PowerShell-Cmdlets zu verwenden, müssen Sie sich authentifizieren. Die Authentifizierung muss in der Funktionen-App konfiguriert werden. Zu diesem Zweck werden Umgebungsvariablen konfiguriert, und eine verschlüsselte Schlüsseldatei muss in die Funktionen-App hochgeladen werden.

> [!note]
> Dieses Szenario stellt lediglich ein Beispiel für das Implementieren von Authentifizierung für Azure-Funktionen dar, es gibt andere Möglichkeiten, dieses Ziel zu erreichen.

### <a name="encrypted-credentials"></a>Verschlüsselte Anmeldeinformationen

Das folgende PowerShell-Skript erstellt eine Schlüsseldatei mit dem Namen **PassEncryptKey.key** und stellt eine verschlüsselte Version des angegebenen Kennworts bereit.  Dieses Kennwort ist das gleiche, das für die Azure AD-Anwendung definiert ist, die zur Authentifizierung verwendet wird.

```powershell
#variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

Erstellen Sie im App Service-Editor der Funktionen-App einen Ordner mit dem Namen **keys** unter **AlertPacketCapturePowerShell**, und laden Sie die Datei **PassEncryptKey.key** hoch, die im vorhergehenden PowerShell-Beispiel erstellt wurde.

![Funktionsschlüssel][functions8]

### <a name="retrieving-values-for-environment-variables"></a>Abrufen von Werten für Umgebungsvariablen

Die erforderliche endgültige Konfiguration legt die Umgebungsvariablen fest, die für den Zugriff auf die Werte für die Authentifizierung erforderlich sind. Es folgt eine Liste der Umgebungsvariablen, die erstellt werden.

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

Die Client-ID ist die Anwendungs-ID einer Anwendung in Azure Active Directory.

1. Wenn Sie noch nicht über eine zu verwendende Anwendung verfügen, führen Sie das folgende Beispiel aus, um eine Anwendung zu erstellen.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Das beim Erstellen der Anwendung verwendete Kennwort sollte das gleiche sein, das zuvor beim Speichern der Schlüsseldatei erstellt wurde.

1. Navigieren Sie im Azure-Portal zu **Abonnements** > Zu verwendendes Abonnement auswählen > **Zugriffssteuerung (IAM)**.

    ![IAM-Funktionen][functions9]

1. Wählen Sie das zu verwendende Konto aus, und klicken Sie auf „Eigenschaften“. Kopieren Sie die Anwendungs-ID.

    ![Funktionsanwendungs-ID][functions10]

#### <a name="azuretenant"></a>AzureTenant

Die Mandaten-ID wird durch Ausführen des folgenden PowerShell-Beispiels abgerufen:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

Der Wert der AzureCredPassword-Umgebungsvariablen ist der Wert aus der Ausführung des folgenden PowerShell-Beispiels. Dies ist das gleiche Beispiel, das im vorhergehenden Abschnitt **Verschlüsselte Anmeldeinformationen** verwendet wurde. Der erforderliche Wert ist die Ausgabe der `$Encryptedpassword`-Variablen.  Dies ist das Dienstprinzipalkennwort, das mithilfe des PowerShell-Skripts verschlüsselt wurde.

```powershell
#variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="storing-the-environment-variables"></a>Speichern der Umgebungsvariablen

1. Navigieren Sie zur Funktionen-App, klicken Sie auf **Funktionen-App-Einstellungen** > **App-Einstellungen konfigurieren**

    ![Konfigurieren von App-Einstellungen][functions11]

1. Fügen Sie den App-Einstellungen die Umgebungsvariablen und ihre Werte hinzu, und klicken Sie auf **Speichern**

    ![app settings][functions12]

## <a name="processing-the-alert-and-starting-a-packet-capture-session"></a>Verarbeiten der Warnung und Starten einer Paketerfassungssitzung

Nun können Sie über die Azure-Funktion Aufrufe an Network Watcher senden. Die Implementierung dieser Funktion unterscheidet sich je nach den geltenden Anforderungen. Für den allgemeinen Datenfluss des Codes gilt aber folgender Ablauf:

1. Verarbeiten von Eingabeparametern
2. Abfragen von vorhandenen Paketerfassungen, um Grenzwerte zu überprüfen und Namenskonflikte zu lösen
3. Erstellen einer Paketerfassung mit den geeigneten Parametern
4. Regelmäßiges Abfragen der Paketerfassung bis zum Abschluss des Vorgangs
5. Benachrichtigen des Benutzers über den Abschluss der Paketerfassungssitzung

Das folgende Beispiel stellt PowerShell dar und kann in der Azure-Funktion verwendet werden. Es gibt Werte, durch die subscriptionId, resourceGroupName und storageAccountName ersetzt werden müssen.

```powershell
#Import Azure PowerShell modules required to make calls to Network Watcher
Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

#Process Alert Request Body
$requestBody = Get-Content $req -Raw | ConvertFrom-Json

#Storage Account Id to save captures in
$storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

#Packet Capture Vars
$packetcapturename = "PSAzureFunction"
$packetCaptureLimit = 10
$packetCaptureDuration = 10

#Credentials
$tenant = $env:AzureTenant
$pw = $env:AzureCredPassword
$clientid = $env:AzureClientId
$keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

#Authentication
$secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
$credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


#Get the VM that fired the Alert
if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
{
    Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
    Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
    Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
    Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

    #Get the Network Watcher in the VM's Region
    $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
    $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

    #Get existing packetCaptures
    $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

    #Remove existing packet capture created by the function if it exists
    $packetCaptures | %{if($_.Name -eq $packetCaptureName)
    { 
        Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
    }}

    #Initiate Packet Capture on the VM that fired the alert
    if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
        echo "Initiating Packet Capture"
        New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
        Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
    }
} 
``` 

Nachdem Sie die Funktion erstellt haben, müssen Sie Ihre Warnung konfigurieren, um die URL aufzurufen, die der Funktion zugeordnet ist. Klicken Sie zum Abrufen dieses Werts auf **</> Funktions-URL abrufen** 

![Ermitteln der Funktions-URL 1][functions13]

Kopieren Sie die Funktions-URL für Ihre Funktionen-App.

![Ermitteln der Funktions-URL 2][2]

Wenn Sie in der Nutzlast der Webhook-POST-Anforderung benutzerdefinierte Eigenschaften benötigen, helfen Ihnen die Informationen unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md) weiter.

## <a name="configure-an-alert-on-a-vm"></a>Konfigurieren einer Warnung auf einem virtuellen Computer

Warnungen können konfiguriert werden, um Personen zu benachrichtigen, wenn eine bestimmte Metrik einen zugewiesenen Schwellenwert überschreitet. In diesem Beispiel wird die Warnung für die gesendeten TCP-Segmente ausgelöst, aber sie kann auch für viele andere Metriken ausgelöst werden. Hier wird eine Warnung konfiguriert, um einen Webhook zum Aufrufen der Funktion aufzurufen.

### <a name="create-the-alert-rule"></a>Erstellen der Warnungsregel

Navigieren Sie zu einem vorhandenen virtuellen Computer, und fügen Sie eine Warnungsregel hinzu. Eine ausführlichere Dokumentation zur Konfiguration von Warnungen finden Sie unter [Erstellen von Warnungen in Azure Monitor für Azure-Dienste – Azure-Portal](../monitoring-and-diagnostics/insights-alerts-portal.md). 

![Hinzufügen einer VM-Warnungsregel zu einem virtuellen Computer][1]

> [!NOTE]
> Einige Metriken sind standardmäßig nicht aktiviert. Weitere Informationen dazu, wie Sie weitere Metriken aktivieren, finden Sie unter [Aktivieren von Überwachung und Diagnose](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

Fügen Sie abschließend die URL aus dem vorherigen Schritt in das Textfeld „Webhook“ Ihrer Warnung ein. Klicken Sie auf **OK**, um die Warnungsregel zu speichern.

![Einfügen der URL in die Warnungsregel][3]

## <a name="downloading-and-viewing-the-capture-file"></a>Herunterladen und Anzeigen der Erfassungsdatei

Wenn Sie Ihre Erfassung in einem Speicherkonto speichern, kann die Erfassungsdatei über das Portal oder programmgesteuert heruntergeladen werden. Wenn die Erfassungsdatei lokal gespeichert ist, wird sie abgerufen, indem die Anmeldung am virtuellen Computer durchgeführt wird. 

Anweisungen zum Herunterladen von Dateien von Azure-Speicherkonten finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../storage/storage-dotnet-how-to-use-blobs.md). Ein weiteres Tool, das verwendet werden kann, ist der Speicher-Explorer. Weitere Informationen zum Speicher-Explorer finden Sie unter [Speicher-Explorer](http://storageexplorer.com/).

Nach dem Herunterladen Ihrer Erfassung können Sie diese mit einem beliebigen Tool anzeigen, das für **CAP**-Dateien geeignet ist. Hier sind Links zu zwei Tools dieser Art angegeben:

[Microsoft Message Analyzer](https://technet.microsoft.com/en-us/library/jj649776.aspx)  
[WireShark](https://www.wireshark.org/)  

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anzeigen der Paketerfassungen finden Sie unter [Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Azure Functions](network-watcher-alert-triggered-packet-capture.md).

[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
