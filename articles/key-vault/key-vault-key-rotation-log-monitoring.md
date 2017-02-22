---
title: "Einrichten von Azure Key Vault mit End-to-End-Schlüsselrotation und Überwachung | Microsoft Docs"
description: "Dieser Artikel bietet Informationen zum Einrichten der Schlüsselrotation und Überwachen von Schlüsseltresorprotokollen."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: cc872e8d9bc0662f46d5f394f9c98885e34fe67a


---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Einrichten von Azure Key Vault mit End-to-End-Schlüsselrotation und Überwachung
## <a name="introduction"></a>Einführung
Nach Erstellen Ihres Schlüsseltresors können Sie Ihre Schlüssel und geheimen Schlüssel in diesem Tresor speichern. Ihre Anwendungen müssen Ihre Schlüssel und geheimen Schlüssel nicht mehr dauerhaft speichern, sondern fordern diese bei Bedarf aus dem Schlüsseltresor an. Dadurch können Sie Schlüssel und geheime Schlüssel ohne Auswirkungen auf das Verhalten Ihrer Anwendung aktualisieren, was eine Vielzahl von Möglichkeiten für die Verwaltung von Schlüsseln und geheimen Schlüsseln eröffnet.

Dieser Artikel bietet eine exemplarische Vorgehensweise anhand eines Beispiels der Nutzung von Azure Key Vault zum Speichern eines geheimen Schlüssels. In diesem Fall handelt es sich um einen Azure Storage-Kontoschlüssel, auf den eine Anwendung zugreift. Außerdem wird die Implementierung einer geplanten Rotation dieses Speicherkontoschlüssels demonstriert. Schließlich werden Sie durch eine Demonstration der Überwachung der Schlüsseltresorprotokolle und Auslösung von Warnungen begleitet, wenn unerwartete Anforderungen erfolgen.

> [!NOTE]
> In diesem Tutorial werden nicht die Einzelheiten der erstmaligen Einrichtung Ihres Schlüsseltresors behandelt. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md). Anleitungen für die plattformübergreifende Befehlszeilenschnittstelle finden Sie unter [Verwalten von Schlüsseltresor mit CLI](key-vault-manage-with-cli.md).
>
>

## <a name="set-up-key-vault"></a>Einrichten von Key Vault
Damit eine Anwendung einen geheimen Schlüssel aus Key Vault abrufen kann, müssen Sie zuerst den geheimen Schlüssel erstellen und ihn in den Tresor hochladen. Starten Sie hierfür eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:

```powershell
Login-AzureRmAccount
```

Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. PowerShell ruft alle Abonnements ab, die diesem Konto zugeordnet sind. PowerShell verwendet standardmäßig das erste Abonnement.

Wenn Sie über mehrere Abonnements verfügen, müssen Sie unter Umständen das Abonnement angeben, das zum Erstellen des Schlüsseltresors verwendet wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto anzuzeigen:

```powershell
Get-AzureRmSubscription
```

Geben Sie Folgendes ein, um das Abonnement anzugeben, das dem zu protokollierenden Schlüsseltresor zugeordnet ist:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Da in diesem Artikel das Speichern eines Speicherkontoschlüssels als geheimer Schlüssel demonstriert wird, müssen Sie diesen Speicherkontoschlüssel abrufen.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Nachdem Sie Ihren geheimen Schlüssel (in diesem Fall Ihren Speicherkontoschlüssel) abgerufen haben, müssen Sie ihn in eine sichere Zeichenfolge konvertieren und anschließend einen geheimen Schlüssel mit diesem Wert in Ihrem Schlüsseltresor erstellen.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Rufen Sie als Nächstes den URI für den erstellten geheimen Schlüssel ab. Dieser wird in einem späteren Schritt verwendet, wenn Sie den Schlüsseltresor zum Abrufen des geheimen Schlüssels aufrufen. Führen Sie den folgenden PowerShell-Befehl aus, und notieren Sie den Wert „ID“, bei dem es sich um den URI des geheimen Schlüssels handelt:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Einrichten der Anwendung
Da Sie nun über einen gespeicherten geheimen Schlüssel verfügen, können Sie diesen mithilfe von Code abrufen und verwenden. Hierfür sind mehrere Schritte erforderlich. Der erste und wichtigste Schritt ist die Registrierung Ihrer Anwendung in Azure Active Directory. Danach müssen Sie an Key Vault Informationen zu Ihrer Anwendung übermitteln, damit der Schlüsseltresor Anforderungen von Ihrer Anwendung empfangen kann.

> [!NOTE]
> Ihre Anwendung muss mit demselben Azure Active Directory-Mandanten wie Ihr Schlüsseltresor erstellt werden.
>
>

Öffnen Sie die Registerkarte „Anwendungen“ von Azure Active Directory (AD).

![„Anwendungen“ in Azure Active Directory öffnen](./media/keyvault-keyrotation/AzureAD_Header.png)

Wählen Sie **Hinzufügen**, um Azure Active Directory eine Anwendung hinzuzufügen.

![„Hinzufügen“ auswählen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Belassen Sie den Anwendungstyp als **Webanwendung und/oder Web-API**, und versehen Sie Ihre Anwendung mit einem Namen.

![Die Anwendung benennen](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Geben Sie für Ihre Anwendung eine **Anmelde-URL** und einen **App-ID-URI** ein. Diese Angaben sind für diese Demo beliebig und können später bei Bedarf geändert werden.

![Erforderliche URIs bereitstellen](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Nachdem Azure Active Directory die Anwendung hinzugefügt wurde, gelangen Sie auf die Anwendungsseite. Klicken Sie auf die Registerkarte **Konfigurieren**, suchen Sie den Wert **Client-ID**, und kopieren Sie ihn. Notieren Sie die Client-ID für spätere Schritte.

Generieren Sie dann einen Schlüssel für die Anwendung, damit diese mit Azure Active Directory interagieren kann. Sie können diesen im Abschnitt **Schlüssel** auf der Registerkarte **Konfiguration** erstellen. Notieren Sie in Ihrer Azure Active Directory-Anwendung den neu generierten Schlüssel für die Verwendung in einem späteren Schritt.

![Azure Active Directory-App-Schlüssel](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Bevor Ihre Anwendung Aufrufe an den Schlüsseltresor durchführen kann, müssen Sie den Schlüsseltresor über Ihre Anwendung und deren Berechtigungen informieren. Der folgende Befehl verwendet den Namen des Schlüsseltresors und die Client-ID aus Ihrer Azure Active Directory-App und gewährt Ihrem Schlüsseltresor für die Anwendung den Zugriff **Abrufen**.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

An diesem Punkt können Sie beginnen, Anwendungsaufrufe zu erstellen. In Ihrer Anwendung müssen Sie die NuGet-Pakete installieren, die für die Interaktion mit Azure Key Vault und Azure Active Directory benötigt werden. Geben Sie in der Paket-Manager-Konsole von Visual Studio die folgenden Befehle ein. Bei der Erstellung dieses Artikels lautete die aktuelle Version des Azure Active Directory-Pakets 3.10.305231913. Prüfen Sie, ob es eine neuere Version gibt, und führen Sie ggf. eine entsprechende Aktualisierung durch.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Erstellen Sie in Ihrem Anwendungscode eine Klasse, die die Methode für Ihre Azure Active Directory-Authentifizierung enthalten soll. In diesem Beispiel heißt diese Klasse **Utils**. Fügen Sie die folgende using-Anweisung hinzu:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Fügen Sie als Nächstes die folgende Methode hinzu, um das JWT-Token aus Azure Active Directory abzurufen. Zur besseren Verwaltbarkeit können Sie die hartcodierten Zeichenfolgenwerte in Ihre Web- oder Anwendungskonfiguration verschieben.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Fügen Sie den erforderlichen Code zum Aufrufen von Key Vault und Abrufen des Werts Ihres geheimen Schlüssels hinzu. Zuerst müssen Sie die folgende „using“-Anweisung hinzufügen:

```csharp
using Microsoft.Azure.KeyVault;
```

Fügen Sie die Methodenaufrufe zum Aufrufen von Key Vault und Abrufen Ihres geheimen Schlüssels hinzu. In dieser Methode geben Sie den URI Ihres geheimen Schlüssels an, den Sie in einem vorherigen Schritt gespeichert haben. Beachten Sie die Verwendung der **GetToken**-Methode in der zuvor erstellten **Utils**-Klasse.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wenn Sie Ihre Anwendung ausführen, müssen Sie sich jetzt bei Azure Active Directory authentifizieren und dann den Wert Ihres geheimen Schlüssels aus Azure Key Vault abrufen.

## <a name="key-rotation-using-azure-automation"></a>Schlüsselrotation mithilfe von Azure Automation
Es gibt verschiedene Optionen für die Implementierung einer Strategie für die Rotation von Werten, die Sie als geheime Azure Key Vault-Schlüssel speichern. Die Rotation geheimer Schlüssel kann im Rahmen eines manuellen Prozesses, programmgesteuert mithilfe von API-Aufrufen oder mithilfe eines Automatisierungsskripts erfolgen. Für diesen Artikel nutzen Sie Azure PowerShell zusammen mit Azure Automation, um einen Zugriffsschlüssel für das Azure Storage-Konto zu ändern. Anschließend aktualisieren Sie einen geheimen Schlüssel im Schlüsseltresor mit diesem neuen Schlüssel.

Damit Azure Automation geheime Schlüsselwerte in Ihrem Schlüsseltresor festlegen kann, müssen Sie die Client-ID der Verbindung namens „AzureRunAsConnection“ abrufen, die erstellt wurde, als Sie Ihre Azure Automation-Instanz erstellt haben. Sie können diese ID abrufen, indem Sie in Ihrer Azure Automation-Instanz **Assets** auswählen. Dort wählen Sie **Verbindungen** und dann den Dienstprinzipal **AzureRunAsConnection** aus. Notieren Sie den Wert von **Anwendungs-ID**.

![Azure Automation-Client-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Wählen Sie in **Assets** die Option **Module** aus. In **Module** wählen Sie dann **Katalog** aus. Anschließend suchen Sie nach aktualisierten Versionen der einzelnen Module, die Sie **importieren**:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Artikels mussten nur die zuvor genannten Module für das folgende Skript aktualisiert werden. Wenn Sie feststellen, dass Ihr Automatisierungsauftrag fehlschlägt, überprüfen Sie, ob Sie alle erforderlichen Module und ihre Abhängigkeiten importiert haben.
>
>

Nachdem Sie die Anwendungs-ID für Ihre Azure Automation-Verbindung abgerufen haben, müssen Sie Ihren Schlüsseltresor informieren, dass diese Anwendung Zugriff zum Aktualisieren geheimer Schlüssel in Ihrem Tresor hat. Dies kann mit dem folgenden PowerShell-Befehl erreicht werden:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Als Nächstes wählen Sie die Ressource **Runbooks** unter Ihrer Azure Automation-Instanz und dann **Runbook hinzufügen** aus. Wählen Sie **Schnellerfassung**. Benennen Sie Ihr Runbook, und wählen Sie als Runbooktyp **PowerShell** aus. Optional können Sie eine Beschreibung hinzufügen. Klicken Sie abschließend auf **Erstellen**.

![Runbook erstellen](./media/keyvault-keyrotation/Create_Runbook.png)

Im Editorbereich Ihres neuen Runbooks fügen Sie das folgende PowerShell-Skript ein:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Im Editorbereich können Sie **Testbereich** wählen, um Ihr Skript zu testen. Sobald das Skript ohne Fehler ausgeführt wird, können Sie die Option **Veröffentlichen** auswählen. Im Konfigurationsbereich für das Runbook können Sie dann einen Zeitplan dafür auswählen.

## <a name="key-vault-auditing-pipeline"></a>Überwachungspipeline für den Schlüsseltresor
Bei der Einrichtung eines Schlüsseltresors können Sie die Überwachung aktivieren, um Protokolle zu Zugriffsanforderungen zu sammeln, die an Ihren Schlüsseltresor gerichtet wurden. Diese Protokolle werden in einem festgelegten Azure Storage-Konto gespeichert und können dann abgerufen, überwacht und analysiert werden. Im folgenden Szenario werden Azure-Funktionen, Azure-Logik-Apps und Key Vault-Überwachungsprotokolle zum Erstellen einer Pipeline verwendet, die eine E-Mail sendet, sobald geheime Schlüssel aus dem Tresor von einer App abgerufen werden, die nicht der App-ID der Web-App entspricht.

Zunächst müssen Sie die Protokollierung für Ihren Schlüsseltresor aktivieren. Dies kann über die folgenden PowerShell-Befehle erfolgen (Details finden Sie unter [Key Vault-Protokollierung](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Wenn diese Option aktiviert ist, beginnen Überwachungsprotokolle mit dem Sammeln von Daten im vorgesehenen Speicherkonto. Diese Protokolle enthalten Ereignisse dazu, wie, wann und von wem auf Ihre Schlüsseltresore zugegriffen wurde.

> [!NOTE]
> Sie können auf Ihre Protokollinformationen zehn Minuten nach dem Schlüsseltresorvorgang zugreifen. In der Regel ist dies schon früher möglich.
>
>

Der nächste Schritt besteht im [Erstellen einer Azure Service Bus-Warteschlange](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). In diese werden die Überwachungsprotokolle für den Schlüsseltresor übertragen. Wenn sich die Überwachungsprotokollmeldungen in der Warteschlange befinden, ruft die Logik-App diese ab und reagiert entsprechend. Führen Sie die folgenden Schritte aus, um einen Service Bus zu erstellen:

1. Erstellen Sie einen Service Bus-Namespace (falls ein solcher bereits vorhanden ist, den Sie verwenden möchten, fahren Sie mit Schritt 2 fort).
2. Wechseln Sie im Azure-Portal zum Service Bus, und wählen Sie den Namespace aus, in dem die Warteschlange erstellt werden soll.
3. Wählen Sie **Neu** und dann **Service Bus -> Warteschlange** aus, und geben Sie die erforderlichen Details ein.
4. Wählen Sie die Service Bus-Verbindungsinformationen aus, indem Sie den Namespace auswählen und auf **Verbindungsinformationen** klicken. Sie benötigen diese Informationen für den nächsten Teil.

Als Nächstes [erstellen Sie eine Azure-Funktion](../azure-functions/functions-create-first-azure-function.md) zum Abfragen der Schlüsseltresorprotokolle im Speicherkonto und Auswählen neuer Ereignisse. Dies ist eine Funktion, die gemäß einem Zeitplan ausgelöst wird.

Wählen Sie zum Erstellen einer Azure-Funktion im Azure-Portal **Neu -> Funktionen-App** aus. Während der Erstellung können Sie einen vorhandenen Hostingplan verwenden oder einen neuen erstellen. Sie können sich auch für dynamisches Hosten entscheiden. Weitere Informationen zu Optionen für das Hosten von Funktionen finden Sie unter [Skalieren von Azure Functions](../azure-functions/functions-scale.md).

Nachdem die Azure-Funktion erstellt wurde, navigieren Sie zu der Funktion. Wählen Sie eine Timerfunktion und C\# aus. Klicken Sie dann auf **Diese Funktion erstellen**.

![Blatt „Start“ von Azure Functions](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Ersetzen Sie auf der Registerkarte **Entwickeln** den Code von „run.csx“ durch Folgendes:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> Stellen Sie sicher, dass die Variablen im vorangegangenen Code so ersetzt werden, dass sie auf Ihr Speicherkonto, in das die Schlüsseltresorprotokolle geschrieben werden, auf den zuvor erstellten Service Bus und auf den spezifischen Pfad zu den Speicherprotokollen für den Schlüsseltresor zeigen.
>
>

Die Funktion verwendet die neueste Protokolldatei im Speicherkonto, in das Schlüsseltresorprotokolle geschrieben werden, ruft die neuesten Ereignisse aus dieser Datei ab und überträgt sie in eine Service Bus-Warteschlange. Da eine einzelne Datei mehrere Ereignisse enthalten kann, sollten Sie die Datei „sync.txt“ erstellen, die die Funktion ebenfalls untersucht, um den Zeitstempel des letzten ausgewählten Ereignisses zu bestimmen. Dadurch wird sichergestellt, dass Sie dasselbe Ereignis nicht mehrmals in die Warteschlange übertragen. Diese Datei „sync.txt“ enthält einen Zeitstempel des letzten eingetretenen Ereignisses. Die geladenen Protokolle müssen basierend auf dem Zeitstempel sortiert werden, um sicherzustellen, dass sie in die richtige Reihenfolge gebracht werden.

Hierfür verweisen wir auf verschiedene zusätzliche Bibliotheken, die in Azure Functions nicht standardmäßig verfügbar sind. Um sie einzubeziehen, muss Azure Functions sie über NuGet abrufen. Wählen Sie die Option **Dateien anzeigen** aus.

![Option „Dateien anzeigen“](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Fügen Sie eine Datei namens „project.json“ mit folgendem Inhalt hinzu:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Dadurch wird Azure Functions beim **Speichern** die erforderlichen Binärdateien herunterladen.

Wechseln Sie zur Registerkarte **Integrieren** , und versehen Sie den Parameter „Timer“ mit einem aussagekräftigen Namen, der in der Funktion verwendet werden soll. Im obigen Code wird erwartet, dass der Timer *myTimer*heißt. Geben Sie einen [CRON-Ausdruck](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) wie folgt an: 0 \* \* \* \* \* für den Timer, der bewirkt, dass die Funktion einmal pro Minute ausgeführt wird.

Fügen Sie auf der Registerkarte **Integrieren** auch eine Eingabe des Typs **Azure Blob Storage** hinzu. Diese verweist auf die Datei „sync.txt“, die den Zeitstempel des letzten Ereignisses enthält, das von der Funktion untersucht wurde. Diese Information wird in der Funktion mithilfe des Parameternamens verfügbar gemacht. Im obigen Code erwartet die Azure Blob Storage-Eingabe, dass der Parametername *inputBlob* ist. Wählen Sie das Speicherkonto aus, das die Datei „sync.txt“ enthält (dabei kann es sich um dasselbe oder ein anderes Speicherkonto handeln). Geben Sie im entsprechenden Feld den Pfad zu der Datei in diesem Format an: {container-name}/path/to/sync.txt.

Fügen Sie eine Ausgabe des Typs *Azure Blob Storage* hinzu. Diese zeigt auf die Datei „sync.txt“, die Sie in der Eingabe definiert haben. Dies wird von der Funktion zum Schreiben des Zeitstempels des letzten untersuchten Ereignisses verwendet. Der obige Code erwartet, dass dieser Parameter *outputBlob*heißt.

An diesem Punkt ist die Funktion fertig. Wechseln Sie zurück zur Registerkarte **Entwickeln**, und speichern Sie den Code. Überprüfen Sie das Ausgabefenster auf Kompilierungsfehler, und korrigieren Sie diese entsprechend. Falls die Kompilierung erfolgt, sollte der Code nun die Schlüsseltresorprotokolle minütlich überprüfen und alle neuen Ereignisse in die definierte Service Bus-Warteschlange übertragen. Immer wenn die Funktion ausgelöst wird, sollten Protokollinformationen im Protokollfenster ausgegeben werden.

### <a name="azure-logic-app"></a>Azure-Logik-App
Als Nächstes müssen Sie eine Azure-Logik-App erstellen, die die Ereignisse verarbeitet, die die Funktion in die Service Bus-Warteschlange überträgt, den Inhalt analysiert und basierend auf einer erfüllten Bedingung eine E-Mail sendet.

[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) über **Neu > Logik-App**.

Sobald die Logik-App erstellt wurde, navigieren Sie zu ihr und wählen **Bearbeiten**aus. Wählen Sie im Logik-App-Editor **Service Bus-Warteschlange** aus, und geben Sie Ihre Service Bus-Anmeldeinformationen ein, um sie mit der Warteschlange zu verbinden.

![Service Bus für Azure-Logik-App](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Wählen Sie anschließend **Bedingung hinzufügen** aus. Wechseln Sie in der Bedingung zum erweiterten Editor. Geben Sie den folgenden Code ein, und ersetzen Sie die APP_ID durch die tatsächliche APP_ID Ihrer Web-App:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Dieser Ausdruck gibt **false** zurück, wenn die *appid* im eingehenden Ereignis (dem Text der Service Bus-Nachricht) nicht die *appid* der App ist.

Erstellen Sie jetzt unter **Falls nein, nichts tun** eine Aktion.

![Azure-Logik-App, Aktion wählen](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Wählen Sie als Aktion **Office 365 – E-Mail senden**. Füllen Sie die Felder aus, um eine E-Mail zu erstellen, wenn die definierte Bedingung **false** zurückgibt. Wenn Sie Office 365 nicht haben, können Sie eine Alternative wählen, um dieselben Ergebnisse zu erzielen.

An diesem Punkt haben Sie eine lückenlose Pipeline, die einmal pro Minute eine Überprüfung auf neue Schlüsseltresor-Überwachungsprotokolle vornimmt. Die gefundenen neuen Protokolle werden in eine Service Bus-Warteschlange verschoben. Die Logik-App wird ausgelöst, wenn eine neue Nachricht in der Warteschlange eintrifft. Wenn die *appid* innerhalb des Ereignisses nicht der App-ID der aufrufenden Anwendung entspricht, wird eine E-Mail gesendet.



<!--HONumber=Feb17_HO3-->


