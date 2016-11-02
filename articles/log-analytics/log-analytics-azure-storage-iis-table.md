<properties
    pageTitle="Verwenden von Blob Storage für IIS und Table Storage für Ereignisse | Microsoft Azure"
    description="Log Analytics kann die Protokolle für Azure-Dienste, die Diagnosedaten in Table Storage schreiben, sowie die IIS-Protokolle, die in Blob Storage geschrieben werden, lesen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>



# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Verwenden von Blob Storage für IIS und Table Storage für Ereignisse

Log Analytics kann die Protokolle für die folgenden Dienste, die Diagnosedaten in Table Storage schreiben, sowie die IIS-Protokolle, die in Blob Storage geschrieben werden, lesen:

+ Service Fabric-Cluster (Vorschau)
+ Virtual Machines
+ Web-/Workerrollen

Bevor Log Analytics Daten für diese Ressourcen sammeln kann, muss Azure-Diagnose aktiviert sein.

Nachdem die Diagnose aktiviert wurde, können Sie das Azure-Portal oder PowerShell zum Konfigurieren von Log Analytics für das Sammeln der Protokolle verwenden.

Die Azure-Diagnose ist eine Azure-Erweiterung, mit der Sie Diagnosedaten von einer Worker- oder Webrolle oder einem virtuellen Computer erfassen können, die bzw. der in Azure ausgeführt wird. Die Daten werden in einem Azure-Speicherkonto gespeichert und können dann von Log Analytics gesammelt werden.

Damit Log Analytics die Protokolle von Azure-Diagnose sammeln kann, müssen diese sich an den folgenden Speicherorten befinden:

| Protokolltyp | Ressourcentyp | Standort |
| -------- | ------------- | -------- |
| IIS-Protokolle | Virtual Machines <br> Webrollen <br> Workerrollen | wad-iis-logfiles (Blob Storage) |
| syslog | Virtual Machines | LinuxsyslogVer2v0 (Table Storage) |
| Service Fabric – operative Ereignisse | Service Fabric-Knoten | WADServiceFabricSystemEventTable |
| Service Fabric Reliable Actor-Ereignisse | Service Fabric-Knoten | WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Service-Ereignisse | Service Fabric-Knoten | WADServiceFabricReliableServiceEventTable |
| Windows-Ereignisprotokolle | Service Fabric-Knoten <br> Virtual Machines <br> Webrollen <br> Workerrollen | WADWindowsEventLogsTable (Table Storage) |
| Windows-ETW-Protokolle | Service Fabric-Knoten <br> Virtual Machines <br> Webrollen <br> Workerrollen | WADETWEventTable (Table Storage) |

>[AZURE.NOTE] IIS-Protokolle von Azure-Websites werden derzeit nicht unterstützt.

Bei virtuellen Computern haben Sie auch die Möglichkeit, [Microsoft Monitoring Agent](http://go.microsoft.com/fwlink/?LinkId=517269) auf Ihrem virtuellen Computer zu installieren, um weitere Einblicke zu aktivieren. Auf diese Weise können Sie nicht nur IIS-Protokolle und Ereignisprotokolle analysieren, sondern auch zusätzliche Analysen durchführen, einschließlich der Nachverfolgung von Konfigurationsänderungen, SQL-Bewertung und der Bewertung von Updates.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Aktivieren der Azure-Diagnose auf einem virtuellen Computer zur Erfassung von Ereignis- und IIS-Protokollen

Gehen Sie folgendermaßen vor, um die Azure-Diagnose mithilfe des Microsoft Azure-Verwaltungsportals auf einem virtuellen Computer für die Erfassung von Ereignis- und IIS-Protokollen zu aktivieren.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-management-portal"></a>So aktivieren Sie die Azure-Diagnose auf einem virtuellen Computer mit dem Azure-Verwaltungsportal:

1. Installieren Sie den VM-Agent, wenn Sie einen virtuellen Computer erstellen. Ist der virtuelle Computer bereits vorhanden, stellen Sie sicher, dass der VM-Agent bereits installiert ist.
    - Navigieren Sie im Azure-Verwaltungsportal zu dem virtuellen Computer, wählen Sie **Optionale Konfiguration** und dann **Diagnose** aus, und legen Sie **Status** auf **Ein** fest.

    Nach Abschluss des Vorgangs wird auf dem virtuellen Computer automatisch die Azure-Diagnoseerweiterung installiert und ausgeführt. Diese ist für das Sammeln von Diagnosedaten zuständig.

2. Aktivieren Sie die Überwachung, und konfigurieren Sie die Ereignisprotokollierung für einen vorhandenen virtuellen Computer. Sie können die Diagnose auf VM-Ebene aktivieren. Zum Aktivieren der Diagnose und zur anschließenden Konfiguration der Ereignisprotokollierung führen Sie die folgenden Schritte aus:
    1. Wählen Sie den virtuellen Computer aus.
    2. Klicken Sie auf **Überwachen**.
    3. Klicken Sie auf **Diagnose**.
    4. Legen Sie den **Status** auf **EIN** fest.
    5. Wählen Sie alle Diagnoseprotokolle aus, die Sie sammeln möchten.
    7. Klicken Sie auf **OK**.

Mit Azure PowerShell können Sie die Ereignisse genauer angeben, die in Azure Storage geschrieben werden. Weitere Informationen finden Sie unter [Sammeln von Daten mithilfe von Azure-Diagnose, die in Table Storage oder IIS-Protokollen in Blobs geschrieben sind](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Aktivieren der Azure-Diagnose in einer Webrolle für die Sammlung von IIS-Protokollen und -Ereignissen

Informationen hierzu finden Sie unter [Aktivieren der Diagnose in einem Clouddienst](../cloud-services/cloud-services-dotnet-diagnostics.md). Sie verwenden die grundlegenden Informationen von dort und passen hier die Schritte für die Verwendung mit Log Analytics an.

Bei aktivierter Azure-Diagnose:

- IIS-Protokolle werden standardmäßig gespeichert, und Protokolldaten werden im Übertragungsintervall ScheduledTransferPeriod übertragen.
- Windows-Ereignisprotokolle werden standardmäßig nicht übertragen.

### <a name="to-enable-diagnostics"></a>So aktivieren Sie die Diagnose

Zum Aktivieren der Windows-Ereignisprotokolle oder zum Ändern von scheduledTransferPeriod konfigurieren Sie Azure-Diagnose mithilfe der XML-Konfigurationsdatei („diagnostics.wadcfg“), wie in [Schritt 4: Erstellen der Diagnosekonfigurationsdatei und Installieren der Erweiterung](../cloud-services/cloud-services-dotnet-diagnostics.md) beschrieben.

Die folgende Beispielkonfigurationsdatei sammelt IIS-Protokolle und alle Ereignisse aus dem Anwendungs- und dem Systemprotokoll:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Stellen Sie sicher, dass ConfigurationSettings ein Speicherkonto angibt, wie im folgenden Beispiel gezeigt:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Die Werte für **AccountName** und **AccountKey** finden Sie im Azure-Verwaltungsportal im Speicherkonto-Dashboard unter „Zugriffsschlüssel verwalten“. Als Protokoll für die Verbindungszeichenfolge muss **https**verwendet werden.

Sobald die aktualisierte Diagnosekonfiguration auf den Clouddienst angewendet wurde und die Diagnosedaten in Azure Storage geschrieben werden, können Sie mit der Konfiguration von Log Analytics beginnen.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Verwenden des Azure-Portals zum Sammeln von Protokollen aus Azure Storage

Sie können Log Analytics im Azure-Portal konfigurieren, um Protokolle für die folgenden Azure-Dienste zu sammeln:

+ Service Fabric-Cluster
+ Virtual Machines
+ Web-/Workerrollen

Navigieren Sie im Azure-Portal zum Log Analytics-Arbeitsbereich, und führen Sie die folgenden Aufgaben aus:

1. Klicken Sie auf *Storage accounts logs* (Speicherkontoprotokolle).
2. Klicken Sie auf die Aufgabe *Hinzufügen*.
3. Wählen Sie das Speicherkonto mit den Diagnoseprotokollen aus.
  - Dies kann ein klassisches Speicherkonto oder ein Azure Resource Manager-Speicherkonto sein.
4. Wählen Sie den Datentyp aus, für den Sie Protokolle sammeln möchten.
  - Dies können IIS-Protokolle, Ereignisse, Syslog-Daten (Linux), ETW-Protokolle oder Service Fabric-Ereignisse sein.
5. Der Wert für die Quelle wird automatisch basierend auf dem Datentyp ausgefüllt und kann nicht geändert werden.
6. Klicken Sie auf „OK“, um die Konfiguration zu speichern.

Wiederholen Sie die Schritte 2 bis 6 für weitere Speicherkonten und Datentypen, die Log Analytics sammeln sollen.

Nach etwa 30 Minuten können Sie die ersten Daten vom Speicherkonto in Log Analytics sehen. Es werden nur Daten angezeigt, die nach dem Anwenden der Konfiguration in den Speicher geschrieben wurden. Log Analytics liest keine bereits vorher im Speicherkonto vorhandenen Daten.

>[AZURE.NOTE] Das Portal überprüft nicht, ob die Quelle im Speicherkonto vorhanden ist oder ob neue Daten geschrieben werden.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Aktivieren der Azure-Diagnose auf einem virtuellen Computer zur Erfassung von Ereignis- und IIS-Protokollen mit PowerShell

Mit Azure PowerShell können Sie die Ereignisse genauer angeben, die in Azure Storage geschrieben werden.
Weitere Informationen finden Sie unter [Aktivieren der Diagnose in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md) .

Sie können den Agent mit dem folgenden PowerShell-Skript aktivieren und aktualisieren.
Sie können dieses Skript auch mit benutzerdefinierter Protokollierungskonfiguration verwenden.
Sie müssen das Skript ändern, um das Speicherkonto, den Dienstnamen und den Namen des virtuellen Computers festzulegen.
Das Skript verwendet Cmdlets für klassische virtuelle Computer.

Überprüfen Sie das folgende Beispielskript, kopieren Sie es, ändern Sie es nach Bedarf, speichern Sie das Beispiel als PowerShell-Skriptdatei, und führen Sie das Skript aus.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden Sie JSON-Dateien in Blob Storage](log-analytics-azure-storage-json.md), um Protokolle von Azure-Diensten zu lesen, die Diagnosedaten im JSON-Format in Blob Storage schreiben.
- [Aktivieren Sie Lösungen](log-analytics-add-solutions.md) , um Einblick in die Daten bereitzustellen.
- [Erstellen Sie Suchabfragen](log-analytics-log-searches.md) , um die Daten zu analysieren.



<!--HONumber=Oct16_HO2-->


