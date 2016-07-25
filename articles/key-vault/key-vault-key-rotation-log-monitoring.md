<properties
	pageTitle="Einrichten des Schlüsseltresors mit End-to-End-Schlüsselrotation und Überwachung | Microsoft Azure"
	description="Dieser Artikel bietet Informationen zum Einrichten der Schlüsselrotation und Überwachen von Schlüsseltresorprotokollen."
	services="key-vault, automation,app-service\logic"
	documentationCenter=""
	authors="swgriffith"
	manager=""
	tags=""/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/05/2016"
	ms.author="jodehavi;stgriffi"/>
#Einrichten des Schlüsseltresors mit End-to-End-Schlüsselrotation und Überwachung

##Einführung

Nach Erstellen Ihres Azure Key Vault-Schlüsseltresors können Sie Ihre Schlüssel und geheimen Schlüssel in diesem Tresor speichern. Ihre Anwendungen müssen Ihre Schlüssel und geheimen Schlüssel nicht mehr dauerhaft speichern, sondern fordern diese bei Bedarf aus dem Schlüsseltresor an. Dadurch können Sie Schlüssel und geheime Schlüssel ohne Auswirkungen auf das Verhalten Ihrer Anwendung aktualisieren, was eine Vielzahl von Möglichkeiten für das Verwaltungsverhalten von Schlüsseln und geheimen Schlüsseln eröffnet.

Dieser Artikel bietet eine exemplarische Vorgehensweise anhand eines Beispiels der Nutzung von Azure Key Vault zum Speichern eines geheimen Schlüssels. In diesem Fall handelt es sich um einen Azure Storage-Kontoschlüssel, auf den eine Anwendung zugreift. Außerdem wird die Implementierung einer geplanten Rotation dieses Speicherkontoschlüssels demonstriert. Schließlich werden Sie durch eine Demonstration der Überwachung der Schlüsseltresorprotokolle und Auslösung von Warnungen begleitet, wenn unerwartete Anforderungen erfolgen.

> [AZURE.NOTE] In diesem Tutorial werden nicht die Einzelheiten der erstmaligen Einrichtung Ihres Azure Key Vault-Schlüsseltresors behandelt. Informationen hierzu finden Sie unter [Erste Schritte mit Azure Key Vault](key-vault-get-started.md). Anleitungen für die plattformübergreifende Befehlszeilenschnittstelle finden Sie in [diesem entsprechenden Tutorial](key-vault-manage-with-cli.md).

##Einrichten des Schlüsseltresors

Damit eine Anwendung einen geheimen Schlüssel aus Azure Key Vault abrufen kann, müssen Sie zuerst den geheimen Schlüssel erstellen und ihn in den Tresor hochladen. Dies kann, wie nachstehend erläutert, problemlos über PowerShell erfolgen.

Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:

```powershell
Login-AzureRmAccount
```

Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. Azure PowerShell ruft alle Abonnements ab, die diesem Konto zugeordnet sind, und verwendet standardmäßig das erste Abonnement.

Wenn Sie über mehrere Abonnements verfügen, müssen Sie unter Umständen ein bestimmtes Abonnement angeben, das zum Erstellen des Azure-Schlüsseltresors verwendet wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto anzuzeigen:

```powershell
Get-AzureRmSubscription
```

Geben Sie dann Folgendes ein, um das Abonnement anzugeben, das dem zu protokollierenden Schlüsseltresor zugeordnet ist:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Da in diesem Artikel das Speichern eines Speicherkontoschlüssels als geheimer Schlüssel demonstriert wird, müssen Sie diesen Speicherkontoschlüssel abrufen.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Nachdem Sie Ihren geheimen Schlüssel, in diesem Fall Ihren Speicherkontoschlüssel, abgerufen haben, müssen Sie ihn in eine sichere Zeichenfolge konvertieren und anschließend einen geheimen Schlüssel mit diesem Wert in Ihrem Schlüsseltresor erstellen.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Als Nächstes müssen Sie den URI des geheimen Schlüssels abrufen, den Sie gerade erstellt haben. Dieser wird in einem späteren Schritt verwendet, wenn Sie den Schlüsseltresor zum Abrufen des geheimen Schlüssels aufrufen. Führen Sie den folgenden PowerShell-Befehl aus, und notieren Sie den Wert „Id“, bei dem es sich um den URI des geheimen Schlüssels handelt.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##Einrichten der Anwendung

Nachdem Sie einen geheimen Schlüssel gespeichert haben, möchten Sie diesen geheimen Schlüssel abrufen und in Code verwenden. Hierfür sind mehrere Schritte erforderlich. Der erste und wichtigste ist die Registrierung Ihrer Anwendung in Azure Active Directory. Danach müssen Sie an Azure Key Vault Informationen zu Ihrer Anwendung übermitteln, damit der Schlüsseltresor Anforderungen von Ihrer Anwendung empfangen kann.

> [AZURE.NOTE] Ihre Anwendung muss mit demselben Azure Active Directory-Mandanten wie Ihr Schlüsseltresor erstellt werden.

Öffnen Sie zunächst die Registerkarte „Anwendungen“ von Azure Active Directory (AD).

![Anwendungen in Azure AD öffnen](./media/keyvault-keyrotation/AzureAD_Header.png)

Wählen Sie „Hinzufügen“, um Ihrem Azure AD eine neue Anwendung hinzuzufügen.

![„Anwendung hinzufügen“ wählen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Belassen Sie den Anwendungstyp als „Webanwendung und/oder Web-API“, und versehen Sie Ihre Anwendung mit einen Namen.

![Die Anwendung benennen](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Geben Sie für Ihre Anwendung eine Anmelde-URL und einen App-ID-URI ein. Diese Angaben sind für diese Demo beliebig und können später bei Bedarf geändert werden.

![Erforderliche URIs bereitstellen](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Nachdem Azure AD die Anwendung hinzugefügt wurde, gelangen Sie auf die Anwendungsseite. Klicken Sie an dieser Stelle auf die Registerkarte „Konfigurieren“, suchen Sie den Wert „Client-ID“, und kopieren Sie ihn. Notieren Sie die Client-ID für spätere Schritte.

Als Nächstes müssen Sie einen Schlüssel für Ihre Anwendung erstellen, mit dessen Hilfe Sie mit Ihrem Azure AD interagieren. Sie können diesen auf der Registerkarte „Konfiguration“ im Abschnitt „Schlüssel“ erstellen. Notieren Sie in Ihrer Azure AD-Anwendung den neu generierten Schlüssel für die Verwendung in einem späteren Schritt.

![Azure AD-App-Schlüssel](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Ehe Ihre Anwendung Aufrufe an den Schlüsseltresor richten kann, müssen Sie den Schlüsseltresor über Ihre Anwendung und deren Berechtigungen informieren. Der folgende Befehl verwendet den Namen des Schlüsseltresors und die Client-ID aus Ihrer Azure AD-App und gewährt Ihrem Schlüsseltresor für die Anwendung den Zugriff „Abrufen“.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

An diesem Punkt können Sie beginnen, Anwendungsaufrufe zu entwickeln. In Ihrer Anwendung müssen Sie zunächst die NuGet-Pakete installieren, die für die Interaktion mit Azure Key Vault und Azure Active Directory benötigt werden. Geben Sie in der Paket-Manager-Konsole von Visual Studio die folgenden Befehle ein. Bei der Erstellung dieses Artikels lautete die aktuelle Version des ActiveDirectory-Pakets 3.10.305231913. Prüfen Sie, ob es eine neuere Version gibt, und führen Sie ggf. eine entsprechende Aktualisierung durch.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Erstellen Sie in Ihrem Anwendungscode eine Klasse, die die Methode für Ihre Active Directory-Authentifizierung enthalten soll. In diesem Beispiel heißt die Klasse „Utils“. Sie müssen dann die folgende „using“-Anweisung hinzufügen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Fügen Sie als Nächstes die folgende Methode hinzu, um das JWT-Token aus Azure AD abzurufen. Zur besseren Codepflege können Sie die hartcodierten Zeichenfolgenwerte in Ihre Web- oder Anwendungskonfiguration verschieben.

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

Schließlich können Sie den erforderlichen Code zum Aufrufen des Schlüsseltresors und Abrufen des Werts Ihres geheimen Schlüssels hinzufügen. Zuerst müssen Sie die folgende „using“-Anweisung hinzufügen.

```csharp
using Microsoft.Azure.KeyVault;
```

Als Nächstes fügen Sie die Methodenaufrufe zum Aufrufen des Schlüsseltresors und Abrufen Ihres geheimen Schlüssels hinzu. In dieser Methode geben Sie den URI Ihres geheimen Schlüssels an, den Sie in einem vorherigen Schritt gespeichert haben. Beachten die Verwendung der „GetToken“-Methode in der zuvor erstellten „Utils“-Klasse.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wenn Sie Ihre Anwendung ausführen, müssen Sie sich jetzt bei Azure Active Directory authentifizieren und dann den Wert Ihres geheimen Schlüssels aus Ihrem Azure Key Vault-Schlüsseltresor abrufen.

##Schlüsselrotation mithilfe von Azure Automation

Es gibt verschiedene Optionen für die Implementierung einer Strategie für die Rotation von Werten, die Sie als geheime Azure Key Vault-Schlüssel speichern. Die Rotation geheimer Schlüssel kann im Rahmen eines manuellen Prozesses, programmgesteuert mithilfe von API-Aufrufen oder mithilfe eines Automatisierungsskripts erfolgen. Für diesen Artikel nutzen wir Azure PowerShell zusammen mit Azure Automation, um einen Azure Storage-Zugriffsschlüssel zu ändern, und aktualisieren anschließend einen geheimen Schlüssel im Schlüsseltresor mit diesem neuen Schlüssel.

Damit Azure Automation geheime Schlüsselwerte in Ihrem Schlüsseltresor festlegen kann, müssen Sie die Client-ID der Verbindung namens „AzureRunAsConnection“ abrufen, die erstellt wurde, als Sie Ihre Azure Automation-Instanz erstellt haben. Sie können diese ID abrufen, indem Sie in Ihrer Azure Automation-Instanz „Assets“ auswählen. Dort wählen Sie „Verbindungen“ und dann den Dienstprinzipal „AzureRunAsConnection“ aus. Notieren Sie den Wert von „Anwendungs-ID“.

![Azure Automation-Client-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Im Fenster „Assets“ können Sie auch „Module“ auswählen. In „Module“ wählen Sie dann „Katalog“ aus. Anschließend suchen Sie nach aktualisierten Versionen der einzelnen Module, die Sie importieren.

	Azure
	Azure.Storage	
	AzureRM.Profile
	AzureRM.KeyVault
	AzureRM.Automation
	AzureRM.Storage
	
> [AZURE.NOTE] Zum Zeitpunkt der Erstellung dieses Artikels mussten nur die zuvor genannten Module für das nachstehende Skript aktualisiert werden. Wenn Sie feststellen, dass Ihr Automatisierungsauftrag misslingt, überprüfen Sie, ob Sie alle erforderlichen Module und ihre Abhängigkeiten importiert haben.

Nachdem Sie die Anwendungs-ID für Ihre Azure Automation-Verbindung abgerufen haben, müssen Sie Ihren Azure Key Vault-Schlüsseltresor informieren, dass diese Anwendung Zugriff zum Aktualisieren geheimer Schlüssel in Ihrem Tresor hat. Dies kann mit dem folgenden PowerShell-Befehl erreicht werden.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Als Nächstes wählen Sie die Ressource „Runbooks“ unter Ihrer Azure Automation-Instanz und dann „Runbook hinzufügen“ aus. Wählen Sie „Schnellerfassung“. Benennen Sie Ihr Runbook, und wählen Sie „PowerShell“ als Typs des Runbooks aus. Fügen Sie optional eine Beschreibung hinzu. Klicken Sie abschließend auf „Erstellen“.

![Runbook erstellen](./media/keyvault-keyrotation/Create_Runbook.png)

Im Editorbereich Ihres neuen Runbooks fügen Sie das folgende PowerShell-Skript ein.

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

Im Editorbereich können Sie „Testbereich“ wählen, um Ihr Skript zu testen. Sobald das Skript ohne Fehler ausgeführt wird, können Sie die Option „Veröffentlichen“ auswählen. Im Konfigurationsbereich für das Runbook können Sie dann einen Zeitplan dafür auswählen.

##Überwachungspipeline für den Schlüsseltresor

Bei der Einrichtung eines Azure Key Vault-Schlüsseltresors können Sie die Überwachung aktivieren, um Protokolle zu Zugriffsanforderungen zu sammeln, die an Ihren Schlüsseltresor gerichtet wurden. Diese Protokolle werden in einem festgelegten Azure Storage-Konto gespeichert und können dann abgerufen, überwacht und analysiert werden. Es folgt die Beschreibung eines Szenarios, in dem Azure Functions, Azure-Logik-Apps und Azure Key Vault-Überwachungsprotokolle zum Erstellen einer Pipeline genutzt werden, die eine E-Mail sendet, sobald geheime Schlüssel aus dem Tresor von einer App abgerufen werden, die nicht der App-ID der Web-App entspricht.

Zunächst müssen Sie die Protokollierung für Ihren Schlüsseltresor aktivieren. Dies kann über die folgenden PowerShell-Befehle erfolgen (Einzelheiten finden sie [hier](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Wenn diese Option aktiviert ist, beginnen Überwachungsprotokolle mit dem Sammeln von Daten im vorgesehenen Speicherkonto. Diese Protokolle enthalten Ereignisse dazu, wie, wann und von wem auf Ihre Schlüsseltresore zugegriffen wurde.

> [AZURE.NOTE] Sie können auf Ihre Protokollinformationen spätestens zehn Minuten nach dem Schlüsseltresorvorgang zugreifen. In den meisten Fällen geht es aber schneller.

Der nächste Schritt besteht im [Erstellen einer Azure Service Bus-Warteschlange](../service-bus/service-bus-dotnet-get-started-with-queues.md). In diese werden die Überwachungsprotokolle für den Schlüsseltresor übertragen. Sobald diese sich in der Warteschlange befindet, werden sie von der Logik-App ausgewählt, die diese entsprechend verarbeitet. Die Service Bus-Erstellung ist verhältnismäßig einfach. Es folgen die allgemeinen Schritte:

1. Erstellen Sie einen Service Bus-Namespace (wenn Sie bereits einen haben, verwenden Sie diesen, und fahren mit Schritt 2 fort).
2. Wechseln Sie im Portal zum Service Bus, und wählen Sie den Namespace aus, in dem die Warteschlange erstellt werden soll.
3. Wählen Sie „Neu“, dann „Service Bus“ -> „Warteschlange“ aus, und geben Sie die erforderlichen Details ein.
4. Rufen Sie die Service Bus-Verbindungsinformationen ab, indem Sie den Namespace auswählen und auf _Verbindungsinformationen_ klicken. Sie benötigen diese Informationen für den nächsten Teil.

Als Nächstes [erstellen Sie eine Azure-Funktion](../azure-functions/functions-create-first-azure-function.md) zum Abfragen der Schlüsseltresorprotokolle im Speicherkonto und Auswählen neuer Ereignisse. Dies ist eine Funktion, die gemäß einem Zeitplan ausgelöst wird.

Erstellen Sie eine Azure-Funktion („Neu“ -> „Funktionen-App“ im Portal). Während der Erstellung können Sie einen vorhandenen Hostingplan verwenden oder einen neuen erstellen. Sie können sich auch für dynamisches Hosten entscheiden. Weitere Informationen zu Optionen für das Hosten von Funktionen finden Sie [hier](../azure-functions/functions-scale.md).

Nachdem die Azure-Funktion erstellt wurde, navigieren Sie zu ihr. Wählen Sie eine Timerfunktion und C#, und klicken Sie dann auf dem Startbildschirm auf **Erstellen**.

![Blatt „Start“ von Azure Functions](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Ersetzen Sie auf der Registerkarte _Entwickeln_ den Code von „run.csx2“ durch Folgendes:

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
> [AZURE.NOTE] Stellen Sie sicher, dass die Variablen im obigen Code so ersetzt werden, dass sie auf Ihr Speicherkonto, in das die Schlüsseltresorprotokolle geschrieben werden, auf den Service Bus, den Sie zuvor erstellt haben, und auf den spezifischen Pfad zu den Speicherprotokollen für den Schlüsseltresor zeigen.

Die Funktion verwendet die neueste Protokolldatei im Speicherkonto, in das Schlüsseltresorprotokolle geschrieben werden, ruft die neuesten Ereignisse aus dieser Datei ab und überträgt sie in eine Service Bus-Warteschlange. Da eine einzelne Datei mehrere Ereignisse enthalten kann, z.B. im Verlauf einer ganzen Stunde, erstellen wir die Datei _sync.txt_, die die Funktion ebenfalls untersucht, um den Zeitstempel des letzten ausgewählten Ereignisses zu bestimmen. Dadurch wird sichergestellt, dass wir das gleiche Ereignis nicht mehrmals in die Warteschlange übertragen. Diese Datei _sync.txt_ enthält lediglich einen Zeitstempel des letzten aufgetreten Ereignisses. Die geladenen Protokolle müssen basierend auf dem Zeitstempel sortiert werde, um sicherzustellen, dass sie in die richtige Reihenfolge gebracht werden.

Hierfür verweisen wir auf verschiedene zusätzliche Bibliotheken, die in Azure Functions nicht standardmäßig verfügbar sind. Um sie einzubeziehen, muss Azure Functions sie über NuGet abrufen. Wählen Sie die Option _Dateien anzeigen_.

![Option „Dateien anzeigen“](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Fügen Sie eine neue Datei namens _project.json_ mit folgendem Inhalt hinzu:

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
Dadurch wird Azure Functions beim _Speichern_ so ausgelöst, dass die erforderlichen Binärdateien heruntergeladen werden.

Wechseln Sie zur Registerkarte **Integrieren**, und versehen Sie den Parameter „Timer“ mit einem aussagekräftigen Namen, der in der Funktion verwendet werden soll. Im obigen Code wird erwartet, dass der Timer _myTimer_ heißt. Geben Sie einen [CRON-Ausdruck](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) wie folgt an: 0 ***** für den Timer, der bewirkt, dass die Funktion einmal pro Minute ausgeführt wird.

Fügen Sie auf der Registerkarte **Integrieren** auch eine Eingabe des Typs _Azure Blob Storage_ hinzu. Diese verweist auf die Datei _sync.txt_, die den Zeitstempel des letzten Ereignisses enthält, das von der Funktion untersucht wurde. Diese Information wird in der Funktion mithilfe des Parameternamens verfügbar gemacht. Im obigen Code erwartet die Azure Blob Storage-Eingabe, dass der Parametername _inputBlob_ ist. Wählen Sie das Speicherkonto, in dem sich die Datei _sync.txt_ befinden soll. Dabei kann es sich um dasselbe oder ein anderes Speicherkonto handeln. Geben Sie im Feld „Pfad“ den Pfad der Datei im Format „{Containername}/path/to/sync.txt“ an.

Fügen Sie eine Ausgabe des Typs _Azure Blob Storage_ hinzu. Diese zeigt auch auf die Datei _sync.txt_, die Sie soeben in der Eingabe definiert haben. Diese wird von der Funktion zum Schreiben des Zeitstempel des letzten untersuchten Ereignisses genutzt. Der obige Code erwartet, dass dieser Parameter _outputBlob_ heißt.

An diesem Punkt ist die Funktion fertig. Wechseln Sie zurück zur Registerkarte **Entwickeln**, und _speichern_ Sie den Code. Überprüfen Sie das Ausgabefenster auf etwaige Kompilierungsfehler, und korrigieren Sie diese entsprechend. Falls die Kompilierung erfolgt, sollte der Code nun ausgeführt werden, die Schlüsseltresorprotokolle minütlich überprüfen und alle neuen Ereignisse in die definierte Service Bus-Warteschlange übertragen. Immer wenn die Funktion ausgelöst wird, sollten Protokollinformationen im Protokollfenster ausgegeben werden.

###Azure-Logik-App

Als Nächstes müssen wir eine Azure-Logik-App erstellen, welche die Ereignisse verarbeitet, die die Funktion in die Service Bus-Warteschlange überträgt, den Inhalt analysiert und basierend auf einer erfüllten Bedingung eine E-Mail sendet.

[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) über „Neu > Logik-App“.

Sobald die Logik-App erstellt wurde, navigieren Sie zu ihr und wählen _Bearbeiten_ aus. Wählen Sie im Logik-App-Editor, die verwaltete API _Service Bus-Warteschlange_ aus, und geben Sie Ihre Service Bus-Anmeldeinformationen ein, um sie mit der Warteschlange zu verbinden.

![Service Bus für Azure-Logik-App](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Wählen Sie anschließend _Bedingung hinzufügen_. Wechseln Sie in der Bedingung zum _erweiterten Editor_. Geben Sie Folgendes ein, und ersetzen Sie die APP\_ID durch die tatsächliche APP\_ID Ihrer Web-App:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Dieser Ausdruck gibt **false** zurück, wenn die **appid**-Eigenschaft im eingehenden Ereignis (dem Text der Service Bus-Nachricht) nicht die **appid** der App ist.

Erstellen Sie jetzt unter der Option _Falls nein, nichts tun..._ eine Aktion.

![Azure-Logik-App, Aktion wählen](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Wählen Sie als Aktion _Office 365 – E-Mail senden_. Füllen Sie die Felder aus, um eine E-Mail zu erstellen, wenn die definierte Bedingung „false“ zurückgibt. Wenn Sie Office 365 nicht haben, können Sie eine Alternative wählen, um dasselbe zu erreichen.

An diesem Punkt haben Sie eine lückenlose Pipeline, die einmal pro Minute eine Überprüfung auf neue Schlüsseltresor-Überwachungsprotokolle vornimmt. Alle neu gefundenen Protokolle werden in eine Service Bus-Warteschlange übertragen. Die Logik-App wird ausgelöst, sobald eine neue Nachricht in der Warteschlange eingeht. Wenn die App-ID im Ereignis nicht der App-ID der aufrufenden Anwendung entspricht, wird eine E-Mail gesendet.

<!---HONumber=AcomDC_0713_2016-->