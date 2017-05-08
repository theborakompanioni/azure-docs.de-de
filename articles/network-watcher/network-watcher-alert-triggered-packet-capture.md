---
title: "Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Warnungen und Azure Functions | Microsoft-Dokumentation"
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
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 5fd017b6f7645220ee7572e50c02265de41e938c
ms.lasthandoff: 04/27/2017


---
# <a name="use-packet-capture-to-do-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Warnungen und Azure Functions

Mit der Network Watcher-Paketerfassung werden Erfassungssitzungen zum Nachverfolgen des eingehenden und ausgehenden Datenverkehrs eines virtuellen Computers erstellt. Die Erfassungsdatei kann über einen Filter verfügen, der so definiert ist, dass nur der Datenverkehr nachverfolgt wird, den Sie überwachen möchten. Diese Daten werden dann in einem Speicherblob oder lokal auf dem Gastcomputer gespeichert.

Diese Funktion kann per Remotezugriff über Automationsszenarien wie Azure Functions gestartet werden. Mit der Paketerfassung können Sie proaktive Erfassungen basierend auf definierten Netzwerkanomalien durchführen. Weitere Verwendungszwecke sind das Erfassen von Netzwerkstatistiken, das Gewinnen von Informationen zu Netzwerkangriffen, das Debuggen der Kommunikation zwischen Client und Server und vieles mehr.

In Azure bereitgestellte Ressourcen werden rund um die Uhr ausgeführt. Für Sie bzw. Ihre Mitarbeiter ist es nicht möglich, den Status aller Ressourcen rund um die Uhr lückenlos aktiv zu überwachen. Was passiert, wenn nachts um 2 Uhr ein Problem auftritt?

Durch den Einsatz von Network Watcher, Warnungen und Functions im Azure-Ökosystem können Sie mit den entsprechenden Daten und Tools proaktiv auf Probleme im Netzwerk reagieren, um diese zu lösen.

![Szenario][scenario]

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-azurerm-ps).
* [Erstellen Sie eine Instanz von Network Watcher](network-watcher-create.md) (sofern noch nicht vorhanden).
* Halten Sie einen virtuellen Computer in der gleichen Region bereit, in der sich auch die Network Watcher-Instanz befindet – entweder mit der [Windows-Erweiterung](../virtual-machines/windows/extensions-nwa.md) oder mit der [Erweiterung für virtuelle Linux-Computer](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Szenario

In diesem Beispiel werden von Ihrer VM ungewöhnlich viele TCP-Segmente gesendet, und Sie möchten dazu eine Warnung erhalten. Hier werden als Beispiel TCP-Segmente verwendet, Sie können jedoch eine beliebige Warnungsbedingung nutzen.

Bei einer Warnung möchten Sie Daten auf Paketebene erhalten, und die Erhöhung des Kommunikationsvolumens nachvollziehen zu können. So können Sie Maßnahmen ergreifen, die dafür sorgen, dass der Computer zur regulären Kommunikation zurückkehrt.

Bei diesem Szenario wird vorausgesetzt, dass Sie über eine Instanz von Network Watcher sowie über eine Ressourcengruppe mit einem gültigen virtuellen Computer verfügen, den Sie verwenden können.

In diesem Beispiel werden von Ihrer VM ungewöhnlich viele TCP-Segmente gesendet, und Sie möchten dazu eine Warnung erhalten. Als Beispiel werden TCP-Segmente verwendet, aber Sie können eine beliebige Warnungsbedingung nutzen. Bei einer Warnung möchten Sie über Paketebenendaten verfügen, um zu verstehen, warum es zu einem Anstieg der Kommunikation gekommen ist. Sie können dann Maßnahmen einleiten, um die reguläre Kommunikation des Computers wiederherzustellen.

Die folgende Liste enthält eine Übersicht über den Workflow:

1. Auf Ihrer VM wird eine Warnung ausgelöst.
1. Die Warnung ruft über einen Webhook Ihre Azure-Funktion auf.
1. Ihre Azure-Funktion verarbeitet die Warnung und startet eine Network Watcher-Paketerfassungssitzung.
1. Die Paketerfassung wird auf dem virtuellen Computer ausgeführt, und der Datenverkehr wird erfasst.
1. Die Paketerfassungsdatei wird zur Überprüfung und Diagnose in ein Speicherkonto hochgeladen.

Zur Automatisierung dieses Prozesses erstellen und verbinden wir eine Warnung auf unserem virtuellen Computer, die ausgelöst wird, wenn der Vorfall eintritt, sowie eine Funktion für den Aufruf von Network Watcher.

In diesem Szenario passiert Folgendes:

* Es wird eine Azure-Funktion erstellt, die eine Paketerfassung startet.
* Es wird eine Warnungsregel auf einem virtuellen Computer erstellt, und die Warnungsregel wird zum Aufrufen der Azure-Funktion konfiguriert.

## <a name="creating-an-azure-function"></a>Erstellen einer Azure-Funktion

Im ersten Schritt wird eine Azure-Funktion zum Verarbeiten der Warnung und Erstellen einer Paketerfassung erstellt.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Neu** > **Compute** > **Funktionen-App**.

    ![Erstellen einer Funktionen-App][1-1]

2. Geben Sie unter **Funktionen-App** die folgenden Werte ein, und klicken Sie anschließend auf **OK**, um die Funktionen-App zu erstellen:

    |**Einstellung** | **Wert** | **Details** |
    |---|---|---|
    |**App-Name**|PacketCaptureExample|Der Name der Funktionen-App.|
    |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie die Funktionen-App erstellen möchten.||
    |**Ressourcengruppe**|PacketCaptureRG|Der Name der Ressourcengruppe für die Funktionen-App.|
    |**Hostingplan**|Verbrauchstarif| Die Art des Plans, der für Ihre Funktionen-App verwendet wird. Zur Auswahl stehen „Verbrauchstarif“ und „App Service-Plan“. |
    |**Standort**|USA, Mitte| Die Region, in der Sie die Funktionen-App erstellen möchten.|
    |**Speicherkonto**|{automatisch generiert}| Das Speicherkonto, das von Azure Functions für allgemeine Speicherzwecke benötigt wird.|

3. Klicken Sie auf dem Blatt der Funktionen-App **PacketCaptureExample** unter **Funktionen** > **Benutzerdefinierte Funktionen** auf **+**. Wählen Sie **HttpTrigger-Powershell** aus, geben Sie die restlichen Informationen an, und klicken Sie anschließend auf **Erstellen**, um die Funktion zu erstellen.

    |**Einstellung** | **Wert** | **Details** |
    |---|---|---|
    |**Szenario**|Experimentell|Art des Szenarios|
    |**Name Ihrer Funktion**|AlertPacketCapturePowerShell|Name der Funktion|
    |**Autorisierungsstufe**|Funktion|Autorisierungsstufe für die Funktion.|

![Funktionsbeispiel][functions1]

> [!NOTE]
> Die PowerShell-Vorlage ist experimentell und nicht in vollem Umfang unterstützt.

Für dieses Beispiel sind Anpassungen erforderlich, die in den folgenden Schritten erläutert werden:

### <a name="adding-modules"></a>Hinzufügen von Modulen

Um die Network Watcher-PowerShell-Cmdlets zu verwenden, muss das neueste PowerShell-Modul in die Funktionen-App hochgeladen worden sein.

1. Führen Sie auf dem lokalen Computer mit installierten aktuellsten Azure PowerShell-Modulen den folgenden PowerShell-Befehl aus:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    In diesem Beispiel wird der lokale Pfad Ihrer Azure PowerShell-Module angegeben. Diese Ordner werden in einem späteren Schritt verwendet. Dies sind die in diesem Szenario verwendeten Module:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![PowerShell-Ordner][functions5]

1. Navigieren Sie zu **Funktionen-App-Einstellungen** > **Zum App Service-Editor wechseln**.

    ![Kudu-Funktionen][functions2]

1. Klicken Sie mit der rechten Maustaste auf den Ordner „AlertPacketCapturePowershell“, und erstellen Sie einen Ordner mit dem Namen **azuremodules**. Fahren Sie mit dem Erstellen von Unterordnern für jedes benötigte Modul fort.

    ![Kudu-Funktionen][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Klicken Sie mit der rechten Maustaste auf den Unterordner **AzureRM.Network**, und klicken Sie auf **Dateien hochladen**. Navigieren Sie zum Installationsspeicherort Ihrer Azure-Module, wählen Sie im lokalen AzureRM.Network-Ordner alle Dateien aus, und klicken Sie auf **Ok**.  Wiederholen Sie diese Schritte für AzureRM.Profile und AzureRM.Resources.

    ![Hochladen von Dateien][functions6]

1. Nach dem Abschluss dieses Vorgangs sollte jeder Ordner die PowerShell-Moduldateien von Ihrem lokalen Computer enthalten.

    ![PowerShell-Dateien][functions7]

### <a name="authentication"></a>Authentifizierung

Um die PowerShell-Cmdlets zu verwenden, müssen Sie sich authentifizieren. Die Authentifizierung muss in der Funktionen-App konfiguriert werden. Zum Konfigurieren der Authentifizierung werden Umgebungsvariablen konfiguriert, und eine verschlüsselte Schlüsseldatei muss in die Funktionen-App hochgeladen werden.

> [!NOTE]
> Dieses Szenario stellt lediglich ein Beispiel für das Implementieren von Authentifizierung für Azure-Funktionen dar, es gibt andere Möglichkeiten, dieses Ziel zu erreichen.

#### <a name="encrypted-credentials"></a>Verschlüsselte Anmeldeinformationen

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

### <a name="retrieve-values-for-environment-variables"></a>Abrufen von Werten für Umgebungsvariablen

Die erforderliche endgültige Konfiguration legt die Umgebungsvariablen fest, die für den Zugriff auf die Werte für die Authentifizierung erforderlich sind. In der folgenden Liste werden die erstellten Umgebungsvariablen aufgeführt:

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

### <a name="store-the-environment-variables"></a>Speichern der Umgebungsvariablen

1. Navigieren Sie zur Funktionen-App, klicken Sie auf **Funktionen-App-Einstellungen** > **App-Einstellungen konfigurieren**

    ![Konfigurieren von App-Einstellungen][functions11]

1. Fügen Sie den App-Einstellungen die Umgebungsvariablen und ihre Werte hinzu, und klicken Sie auf **Speichern**

    ![app settings][functions12]

### <a name="add-powershell-to-the-function"></a>Hinzufügen von PowerShell zur Funktion

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

Konfigurieren Sie nach dem Erstellen der Funktion Ihre Warnung, um die der Funktion zugeordnete URL aufzurufen. Diesen Wert erhalten Sie, indem Sie die Funktions-URL aus Ihrer Funktionen-App kopieren.

![Ermitteln der Funktions-URL 1][functions13]

Kopieren Sie die Funktions-URL für Ihre Funktionen-App.

![Ermitteln der Funktions-URL 2][2]

Wenn Sie in der Nutzlast der Webhook-POST-Anforderung benutzerdefinierte Eigenschaften benötigen, helfen Ihnen die Informationen unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md) weiter.

## <a name="configure-an-alert-on-a-vm"></a>Konfigurieren einer Warnung auf einem virtuellen Computer

Warnungen können konfiguriert werden, um Personen zu benachrichtigen, wenn eine bestimmte Metrik einen zugewiesenen Schwellenwert überschreitet. In diesem Beispiel wird die Warnung für die gesendeten TCP-Segmente ausgelöst, sie kann jedoch auch für viele andere Metriken ausgelöst werden. Hier wird eine Warnung konfiguriert, um einen Webhook zum Aufrufen der Funktion aufzurufen.

### <a name="create-the-alert-rule"></a>Erstellen der Warnungsregel

Navigieren Sie zu einem vorhandenen virtuellen Computer, und fügen Sie eine Warnungsregel hinzu. Ausführlichere Informationen zum Konfigurieren von Warnungen finden Sie unter [Erstellen von Metrikwarnungen in Azure Monitor für Azure-Dienste – Azure-Portal](../monitoring-and-diagnostics/insights-alerts-portal.md). Geben Sie auf dem Blatt die folgenden Werte ein, und klicken Sie anschließend auf **OK**.

  |**Einstellung** | **Wert** | **Details** |
  |---|---|---|
  |**Name**|TCP_Segments_Sent_Exceeded|Der Name der Warnungsregel.|
  |**Beschreibung**|Schwellenwertüberschreitung durch gesendete TCP-Segmente|Die Beschreibung für die Warnungsregel.||
  |**Metrik**|Gesendete TCP-Segmente| Die Metrik zur Auslösung der Warnung. |
  |**Bedingung**|Größer als| Die Bedingung, die bei der Auswertung der Metrik verwendet werden soll.|
  |**Schwellenwert**|100| Der Wert der Metrik für die Warnungsauslösung. Muss auf einen gültigen Wert für Ihre Umgebung festgelegt werden.|
  |**Zeitraum**|Innerhalb der letzten fünf Minuten| Bestimmt den Zeitraum, in dem der Schwellenwert für die Metrik geprüft werden soll.|
  |**Webhook**|[Webhook-URL aus der Funktionen-App]| Die Webhook-URL aus der Funktionen-App, die in den vorherigen Schritten erstellt wurde.|

> [!NOTE]
> Die Metrik der TCP-Segmente ist in der Standardeinstellung nicht aktiviert. Weitere Informationen dazu, wie Sie weitere Metriken aktivieren, finden Sie unter [Aktivieren von Überwachung und Diagnose](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-the-results"></a>Überprüfen der Ergebnisse

Wenn die Kriterien für die Warnung erfüllt sind, wird eine Paketerfassung erstellt. Navigieren Sie zu Ihrer Network Watcher-Instanz, und klicken Sie auf **Paketerfassung**. Auf dieser Seite können Sie auf den Link für die Paketerfassungsdatei klicken, um die Paketerfassung herunterzuladen.

![Anzeigen der Paketerfassung][functions14]

Wenn die Erfassungsdatei lokal gespeichert ist, wird sie abgerufen, indem die Anmeldung am virtuellen Computer durchgeführt wird.

Eine Anleitung zum Herunterladen von Dateien aus Azure-Speicherkonten finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../storage/storage-dotnet-how-to-use-blobs.md). Sie können auch den Speicher-Explorer verwenden. Weitere Informationen zum Speicher-Explorer finden Sie unter [Speicher-Explorer](http://storageexplorer.com/).

Nach dem Herunterladen Ihrer Erfassung können Sie diese mit einem beliebigen Tool anzeigen, das für Dateien vom Typ **.cap** geeignet ist. Hier finden Sie zwei Links zu geeigneten Tools:

- [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anzeigen der Paketerfassungen finden Sie unter [Packet capture analysis with Wireshark](network-watcher-alert-triggered-packet-capture.md) (Paketerfassungsanalyse mit Wireshark).

[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
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
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png

