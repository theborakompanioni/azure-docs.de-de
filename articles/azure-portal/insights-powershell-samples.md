<properties
	pageTitle="Azure Insights: PowerShell-Schnellstartbeispiele für Azure Insights | Microsoft Azure"
	description="Mit den PowerShell-Schnellstart-Beispielbefehlen für Azure Insights können Sie schnell auf Azure Insights-Überwachungsfunktionen zugreifen."
	authors="kamathashwin"
	manager=""
	editor=""
	services="monitoring"
	documentationCenter="monitoring"/>

<tags
	ms.service="monitoring"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/30/2016"
	ms.author="ashwink"/>

# Azure Insights – PowerShell-Schnellstartbeispiele

In diesem Artikel lernen Sie PowerShell-Beispielbefehle kennen, mit denen Sie schnell auf Azure Insights-Überwachungsfunktionen zugreifen können. Azure Insights ermöglicht die automatische Skalierung von Clouddiensten, virtuellen Computern und Web-Apps sowie das Senden von Warnbenachrichtigungen oder das Aufrufen von Web-URLs basierend auf Werten konfigurierter Telemetriedaten.

## Einrichten von PowerShell
Sofern dies noch nicht geschehen ist, richten Sie PowerShell auf Ihrem Computer ein. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Beispiele in diesem Artikel

Mit den Beispielen in diesem Artikel wird veranschaulicht, wie Sie Azure Insights-Cmdlets verwenden können. Die vollständige Liste der Azure Insights-Cmdlets (Überwachung) finden Sie unter [Azure Insights-Cmdlets](https://msdn.microsoft.com/library/mt282452.aspx).


## Anmelden und Verwenden von Abonnements

Melden Sie sich zuerst bei Ihrem Azure-Abonnement an.

```
Login-AzureRmAccount
```

Dazu müssen Sie sich anmelden. Anschließend werden Ihr Konto, die Mandanten-ID und die ID des Standardabonnements angezeigt. Alle Azure-Cmdlets werden im Kontext des Standardabonnements verwendet. Verwenden Sie den folgenden Befehl, um die Liste der Abonnements anzuzeigen, auf die Sie zugreifen können.

```
Get-AzureRmSubscription
```

Verwenden Sie den folgenden Befehl, um den Arbeitskontext in ein anderes Abonnement zu ändern.

```
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## Abrufen von Überwachungsprotokollen für ein Abonnement
Verwenden Sie das Cmdlet `Get-AzureRmLog`. Im Folgenden sind einige allgemeine Beispiele aufgeführt.

Abrufen von Protokolleinträgen ab dieser Uhrzeit-/Datumsangabe bis heute:

```
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Abrufen von Protokolleinträgen im Uhrzeit-/Datumsbereich:

```
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Abrufen von Protokolleinträgen aus einer bestimmten Ressourcengruppe:

```
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Abrufen von Protokolleinträgen von einem bestimmten Ressourcenanbieter im Uhrzeit-/Datumsbereich:

```
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Abrufen aller Protokolleinträge mit einem bestimmten Aufrufer:

```
Get-AzureRmLog -Caller 'myname@company.com'
```

Der folgende Befehl ruft die letzten 1000 Ereignisse aus dem Überwachungsprotokoll ab:

```
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog` unterstützt viele weitere Parameter. Weitere Informationen finden Sie in der `Get-AzureRmLog`-Referenz.

>[AZURE.NOTE] `Get-AzureRmLog` stellt nur den Verlauf für 15 Tage bereit. Mithilfe des **-MaxEvents**-Parameters können Sie die letzten N Ereignisse für mehr als 15 Tage abfragen. Verwenden Sie für den Zugriff auf Ereignisse, die älter als 15 Tage sind, die REST-API oder das SDK (C#-Beispiel mit dem SDK). Wenn Sie **StartTime** nicht angeben, ist der Standardwert **EndTime** minus 1 Stunde. Wenn Sie **EndTime** nicht angeben, ist der Standardwert die aktuelle Zeit. Alle Zeitangaben sind in UTC.

## Abrufen des Verlaufs von Warnungen
Um alle Warnereignisse anzuzeigen, können Sie die Azure Resource Manager-Protokolle (ARM) mit den folgenden Beispielen abfragen.

```
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Um den Verlauf einer bestimmten Warnungsregel anzuzeigen, können Sie das Cmdlet `Get-AzureRmAlertHistory` verwenden und dabei die Ressourcen-ID der Warnungsregel übergeben.

```
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Das Cmdlet `Get-AzureRmAlertHistory` unterstützt verschiedene Parameter. Weitere Informationen finden Sie unter [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## Abrufen von Informationen zu Warnungsregeln
Die folgenden Befehle beziehen sich auf eine Ressourcengruppe mit dem Namen „montest“.

Anzeigen aller Eigenschaften der Warnungsregel:

```
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Abrufen aller Warnungen für eine Ressourcengruppe:

```
Get-AzureRmAlertRule -ResourceGroup montest
```

Abrufen aller Warnungsregeln, die für eine Zielressource festgelegt wurden. Beispiel: Alle Warnungsregeln, die für einen virtuellen Computer festgelegt wurden.

```
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule` unterstützt weitere Parameter. Weitere Informationen finden Sie unter [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx).

## Erstellen von Warnungsregeln
Sie können das Cmdlet `Add-AlertRule` zum Erstellen, Aktualisieren oder Deaktivieren einer Warnungsregel verwenden.

Sie können die Eigenschaften für E-Mails und Webhooks mit `New-AzureRmAlertRuleEmail` bzw. `New-AzureRmAlertRuleWebhook` erstellen. Weisen Sie diese Elemente im Cmdlet für Warnungsregel als Aktionen der Eigenschaft **Aktionen** der Warnungsregel zu.

Der nächste Abschnitt enthält ein Beispiel, das zeigt, wie eine Warnungsregel mit verschiedenen Parametern erstellt wird.

### Warnungsregel für eine Metrik
In der folgenden Tabelle werden die Parameter und Werte beschrieben, die zum Erstellen einer Warnung mithilfe einer Metrik verwendet werden.


|Parameter|value|
|---|---|
|Name|	simpletestdiskwrite|
|Standort für diese Warnungsregel|	USA (Ost)|
|ResourceGroup|	montest|
|TargetResourceId|	/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig|
|„MetricName“ für die Warnung, die erstellt wird|	\\PhysicalDisk(\_Total)\\Schreibvorgänge/s. Unter den Informationen zum Cmdlet `Get-MetricDefinitions` weiter unten finden Sie Details dazu, wie Sie die genauen Metriknamen abrufen.|
|operator|	GreaterThan|
|Schwellenwert (Anzahl/s für diese Metrik)|	1|
|WindowSize (Format: hh:mm:ss)|	00:05:00|
|Aggregator (Statistik der Metrik, die in diesem Fall die durchschnittliche Anzahl verwendet)|	Durchschnitt|
|Benutzerdefinierte E-Mail-Adressen (Zeichenfolgenarray)|'foo@example.com','bar@example.com'|
|E-Mail an Besitzer, Mitwirkende und Leser senden|	-SendToServiceOwners|

Erstellen einer E-Mail-Aktion

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Erstellen einer Webhookaktion

```
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Erstellen der Warnungsregel für die Metrik zum CPU-Prozentsatz auf einem klassischen virtuellen Computer

```
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Abrufen der Warnungsregel

```
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Das Cmdlet zum Hinzufügen einer Warnung aktualisiert die Regel auch, wenn für die angegebenen Eigenschaften bereits eine Warnungsregel vorhanden ist. Um eine Warnungsregel zu deaktivieren, fügen Sie den **-DisableRule**-Parameter hinzu.

### Warnung zum Überwachungsprotokollereignis

>[AZURE.NOTE] Diese Funktion befindet sich noch in der Vorschauphase.

In diesem Szenario senden Sie eine E-Mail, wenn eine Website im Abonnement in der Ressourcengruppe *abhingrgtest123* erfolgreich gestartet wurde.

Einrichten einer E-Mail-Regel

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Einrichten einer Webhookregel

```
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Erstellen einer Regel für das Ereignis

```
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Abrufen der Warnungsregel

```
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

Mit dem Cmdlet `Add-AlertRule` können verschiedene weitere Parameter verwendet werden. Weitere Informationen finden Sie unter [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## Abrufen einer Liste der verfügbaren Metriken für Warnungen
Sie können das Cmdlet `Get-AzureRmMetricDefinition` zum Anzeigen der Liste aller Metriken für eine bestimmte Ressource verwenden.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Im folgenden Beispiel wird eine Tabelle mit den zugehörigen Metriken „Name“ und „Unit“ erstellt.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Eine vollständige Liste der verfügbaren Optionen für `Get-AzureRmMetricDefinition` finden Sie unter [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## Erstellen und Verwalten von Einstellungen zur automatischen Skalierung
Für eine Ressource wie eine Web-App, eine VM, ein Clouddienst oder eine VM-Skalierungsgruppe kann nur eine Einstellung zur automatischen Skalierung konfiguriert werden. Allerdings kann jede Einstellung zur automatischen Skalierung mehrere Profile aufweisen. Beispielsweise kann ein Profil ein leistungsbasierte Skalierungsprofil und ein zweites ein zeitplanbasiertes Profil sein. Für jedes Profil können mehrere Regeln konfiguriert werden. Weitere Informationen zur automatischen Skalierung finden Sie unter [Automatisches Skalieren einer Anwendung](../cloud-services/cloud-services-how-to-scale.md).

Führen Sie die folgenden Schritte aus:

1. Erstellen Sie Regeln.
2. Erstellen Sie Profile, und ordnen Sie die zuvor erstellten Regeln den Profilen zu.
3. Optional: Erstellen Sie Benachrichtigungen für die automatische Skalierung, indem Sie Webhook- und E-Mail-Eigenschaften konfigurieren.
4. Erstellen Sie eine Einstellung zur automatischen Skalierung mit einem Namen für die Zielressource, indem Sie die Profile und Benachrichtigungen zuordnen, die Sie in den vorherigen Schritten erstellt haben.

Mit den folgenden Beispielen wird veranschaulicht, wie Sie eine Einstellung zur automatischen Skalierung einer VM-Skalierungsgruppe für ein Windows-Betriebssystem basierend auf der Metrik für die CPU-Auslastung erstellen können.

Erstellen Sie zunächst eine Regel für das horizontale Hochskalieren mit einer Erhöhung der Anzahl der Instanzen.

```
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```		

Erstellen Sie dann eine Regel für das horizontale Herunterskalieren mit einer Verringerung der Anzahl der Instanzen.

```
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Erstellen Sie anschließend ein Profil für die Regeln.

```
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Erstellen Sie eine Webhookeigenschaft.

```
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Erstellen Sie die Benachrichtigungseigenschaft für die Einstellung zur automatischen Skalierung, binden Sie dabei die zuvor erstellen Eigenschaften für E-Mail-Adresse und Webhook ein.

```
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Erstellen Sie schließlich die Einstellung zur automatischen Skalierung, um das Profil hinzuzufügen, das Sie soeben erstellt haben.

```
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Weitere Informationen zum Verwalten von Einstellungen zur automatischen Skalierung finden Sie unter [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## Verlauf der automatischen Skalierung
Im folgende Beispiel wird gezeigt, wie Sie aktuelle Ereignisse zur automatischen Skalierung und Warnereignisse anzeigen können. Zeigen Sie über die Suche im Überwachungsprotokoll den Verlauf der automatischen Skalierung an.

```
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Sie können das Cmdlet `Get-AzureRmAutoScaleHistory` zum Abrufen des Verlaufs der automatischen Skalierung verwenden.

```
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Weitere Informationen finden Sie unter [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### Anzeigen der Details für eine Einstellung zur automatischen Skalierung
Sie können das Cmdlet `Get-Autoscalesetting` zum Abrufen von weiteren Informationen über die Einstellung zur automatischen Skalierung verwenden.

Mit dem folgenden Beispiel werden die Details über alle Einstellungen zur automatischen Skalierung in der Ressourcengruppe „myrg1“ angezeigt.

```
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Mit dem folgenden Beispiel werden die Details über alle Einstellungen zur automatischen Skalierung in der Ressourcengruppe „myrg1“ angezeigt, insbesondere die Einstellung mit dem Namen „MyScaleVMSSSetting“.

```
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### Entfernen einer Einstellung zur automatischen Skalierung
Sie können das Cmdlet `Remove-Autoscalesetting` zum Löschen einer Einstellung zur automatischen Skalierung verwenden.

```
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## Verwalten von Protokollprofilen für Überwachungsprotokolle

Sie können ein *Protokollprofil* erstellen, Daten aus Ihren Überwachungsprotokollen in ein Speicherkonto exportieren und die Aufbewahrungsdauer für diese festlegen. Optional können Sie die Daten auch an Ihren Event Hub streamen. Beachten Sie, dass sich diese Funktion derzeit in Vorschauphase befindet und Sie nur ein Protokollprofil pro Abonnement erstellen können. Sie können die folgenden Cmdlets mit Ihrem aktuellen Abonnement verwenden, um Protokollprofile zu erstellen und zu verwalten. Sie können auch ein bestimmtes Abonnement auswählen. Obwohl in PowerShell standardmäßig das aktuelle Abonnement verwendet wird, können Sie dies jederzeit mit `Set-AzureRmContext` ändern. Sie können Überwachungsprotokolle so konfigurieren, dass Daten an ein beliebiges Speicherkonto oder einen Event Hub im Abonnement weitergeleitet werden. Daten werden als Blobdateien im JSON-Format geschrieben.

### Abrufen eines Protokollprofils
Verwenden Sie zum Abrufen Ihrer vorhandenen Protokollprofile das Cmdlet `Get-AzureRmLogProfile`.

### Hinzufügen eines Protokollprofils ohne Datenaufbewahrung

```
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### Entfernen eines Protokollprofils

```
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### Hinzufügen eines Protokollprofils mit Datenaufbewahrung

Sie können die **-RetentionInDays**-Eigenschaft mit der Anzahl von Tagen (als positive ganze Zahl) für die Aufbewahrung der Daten angeben.

```
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### Hinzufügen eines Protokollprofils mit Aufbewahrung und Event Hub
Neben dem Weiterleiten Ihrer Daten an ein Speicherkonto können Sie sie auch an einen Event Hub streamen. Beachten Sie, dass in dieser Vorschauversion die Konfiguration des Speicherkontos obligatorisch ist, die Event Hub-Konfiguration ist jedoch optional.

```
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## Konfigurieren von Diagnoseprotokollen
Viele Azure-Dienste bieten zusätzliche Protokolle und Telemetriedaten. Dazu gehören Azure-Netzwerk-Sicherheitsgruppen, Software-Lastenausgleichsmodule, Key Vault, Azure Search-Dienste und Logik-Apps. Diese Dienste können zum Speichern von Daten in Ihrem Azure-Speicherkonto konfiguriert werden. Dieser Vorgang kann nur auf Ressourcenebene ausgeführt werden, und das Speicherkonto sollte in der gleichen Region wie die Zielressource, in der die Diagnoseeinstellung konfiguriert ist, vorhanden sein.

### Abrufen der Diagnoseeinstellung

```
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Deaktivieren der Diagnoseeinstellung

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Aktivieren der Diagnoseeinstellung ohne Aufbewahrung

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Aktivieren der Diagnoseeinstellung mit Aufbewahrung

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Aktivieren der Diagnoseeinstellung mit Aufbewahrung für eine bestimmte Protokollkategorie

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<!---HONumber=AcomDC_0803_2016-->