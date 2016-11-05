---
title: Verwalten von Azure Media Services-Konten mit PowerShell
description: Erfahren Sie, wie Sie Azure Media Services-Konten mit PowerShell-Cmdlets verwalten.
author: Juliako
manager: erikre
editor: ''
services: media-services
documentationcenter: ''

ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2016
ms.author: juliako

---
# Verwalten von Azure Media Services-Konten mit PowerShell
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)
> 
> [!NOTE]
> Um ein Azure Media Services-Konto erstellen zu können, müssen Sie ein Azure-Konto besitzen. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Kostenlose Azure-Testversion</a>.
> 
> 

## Übersicht
In diesem Artikel sind die Azure PowerShell-Cmdlets für Azure Media Services (AMS) im Azure Resource Manager-Framework aufgeführt. Die Cmdlets sind im **Microsoft.Azure.Commands.Media**-Namespace vorhanden.

## Versionen
**ApiVersion**: „2015-10-01“

## New-AzureRmMediaService
Erstellt einen Mediendienst.

### Syntax
Parametersatz: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parametersatz: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | Name |
| --- | --- |
| Erforderlich |true |
| Position? |1 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

**-Location &lt;String&gt;**

Gibt den Ressourcenspeicherort des Mediendiensts an.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |2 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-StorageAccountId &lt;String&gt;**

Gibt ein primäres Speicherkonto an, das dem Mediendienst zugeordnet ist.

* Nur das neue Speicherkonto (per Resource Manager-API erstellt) wird unterstützt.
* Das Speicherkonto muss vorhanden sein und verfügt über den gleichen Speicherort wie der Mediendienst.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |3 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |StorageAccountIdParamSet |
| Platzhalterzeichen akzeptieren? |false |

**-StorageAccounts &lt;PSStorageAccount[]&gt;**

Gibt Speicherkonten an, die dem Mediendienst zugeordnet sind.

* Nur das neue Speicherkonto (per Resource Manager-API erstellt) wird unterstützt.
* Das Speicherkonto muss vorhanden sein und verfügt über den gleichen Speicherort wie der Mediendienst.
* Nur ein Speicherkonto kann als primäres Konto angegeben werden.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |3 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |StorageAccountsParamSet |
| Platzhalterzeichen akzeptieren? |false |

**-Tags &lt;Hashtable&gt;**

Gibt eine Hashtabelle mit den Tags an, die dem Mediendienst zugeordnet sind.

* Beispiel: @{"tag1"="value1";"tag2"=:value2"}

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |false |
| Position? |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Set-AzureRmMediaService
Aktualisiert einen Mediendienst.

### Syntax
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Akzeptiert Pipeline-Eingabe? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | Name |
| --- | --- |
| Erforderlich |True |
| Position? |1 |
| Standardwert |Keine |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |False |

**-StorageAccounts &lt;PSStorageAccount[]&gt;**

Gibt Speicherkonten an, die dem Mediendienst zugeordnet sind.

* Nur das neue Speicherkonto (per Resource Manager-API erstellt) wird unterstützt.
* Das Speicherkonto muss vorhanden sein und verfügt über den gleichen Speicherort wie der Mediendienst.
* Nur ein Speicherkonto kann als primäres Konto angegeben werden.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |false |
| Position? |Benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |StorageAccountsParamSet |
| Platzhalterzeichen akzeptieren? |false |

**-Tags &lt;Hashtable&gt;**

Gibt eine Hashtabelle mit den Tags an, die diesem Mediendienst zugeordnet sind.

* Die Tags, die dem Mediendienst zugeordnet sind, werden durch den vom Kunden angegebenen Wert ersetzt.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |False |
| Position? |Benannt |
| Standardwert |Keine |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Remove-AzureRmMediaService
Entfernt einen Mediendienst.

### Syntax
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |2 |
| Standardwert |Keine |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |False |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Get-AzureRmMediaService
Ruft alle Mediendienste in einer Ressourcengruppe oder einen Mediendienst mit einem bestimmten Namen ab.

### Syntax
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |ResourceGroupParameterSet, AccountNameParameterSet |

Platzhalterzeichen akzeptieren? false

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |1 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |AccountNameParameterSet |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Get-AzureRmMediaServiceKeys
Ruft die Schlüssel eines Mediendiensts ab.

### Syntax
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |1 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Set-AzureRmMediaServiceKey
Generiert einen primären oder sekundären Schlüssel eines Mediendiensts neu.

### Syntax
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |1 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-KeyType &lt;KeyType&gt;**

Gibt den Schlüsseltyp des Mediendiensts an.

* Primär oder sekundär

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |2 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Sync-AzureRmMediaServiceStorageKeys
Synchronisiert Speicherkontoschlüssel für ein Speicherkonto, das dem Mediendienst zugeordnet ist.

### Syntax
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountName] <string>  [<CommonParameters>]

### Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-AccountName &lt;String&gt;**

Gibt den Namen des Mediendiensts an.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |1 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**-StorageAccountId &lt;String&gt;**

Gibt das Speicherkonto an, das dem Mediendienst zugeordnet ist.

| Aliase | ID |
| --- | --- |
| Erforderlich |true |
| Position? |2 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## Nächster Schritt
Informieren Sie sich über die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0907_2016-->