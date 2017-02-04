---
title: Verwalten von Azure Media Services-Konten mit PowerShell
description: Erfahren Sie, wie Sie Azure Media Services-Konten mit PowerShell-Cmdlets verwalten.
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 2549c876ee35d6a7fa425d43e2f86d24131ca791
ms.openlocfilehash: 3d999d9e27844bc0164cc3572522b9ec022118a1


---
# <a name="manage-azure-media-services-accounts-with-powershell"></a>Verwalten von Azure Media Services-Konten mit PowerShell
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> Um ein Azure Media Services-Konto erstellen zu können, müssen Sie ein Azure-Konto besitzen. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Kostenlose Azure-Testversion</a>.
> 
> 

## <a name="overview"></a>Übersicht
In diesem Artikel sind die Azure PowerShell-Cmdlets für Azure Media Services (AMS) im Azure Resource Manager-Framework aufgeführt. Die Cmdlets sind im **Microsoft.Azure.Commands.Media** -Namespace vorhanden.

## <a name="versions"></a>Versionen
**ApiVersion**:   "2015-10-01"

## <a name="new-azurermmediaservice"></a>New-AzureRmMediaService
Erstellt einen Mediendienst.

### <a name="syntax"></a>Syntax
Parametersatz: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parametersatz: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parameter
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

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

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

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService
Aktualisiert einen Mediendienst.

### <a name="syntax"></a>Syntax
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parameter
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
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |False |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Gibt Speicherkonten an, die dem Mediendienst zugeordnet sind.

* Nur das neue Speicherkonto (per Resource Manager-API erstellt) wird unterstützt.
* Das Speicherkonto muss vorhanden sein und verfügt über den gleichen Speicherort wie der Mediendienst.
* Nur ein Speicherkonto kann als primäres Konto angegeben werden.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |false |
| Position? |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |StorageAccountsParamSet |
| Platzhalterzeichen akzeptieren? |false |

**-Tags &lt;Hashtable&gt;**

Gibt eine Hashtabelle mit den Tags an, die diesem Mediendienst zugeordnet sind.

* Die Tags, die dem Mediendienst zugeordnet sind, werden durch den vom Kunden angegebenen Wert ersetzt.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |false |
| Position? |benannt |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |false |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="remove-azurermmediaservice"></a>Remove-AzureRmMediaService
Entfernt einen Mediendienst.

### <a name="syntax"></a>Syntax
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter
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
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Platzhalterzeichen akzeptieren? |False |

**&lt;CommandParameters&gt;**

Dieses Cmdlet unterstützt diese gängigen Parameter: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction und -WarningVariable.

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService
Ruft alle Mediendienste in einer Ressourcengruppe oder einen Mediendienst mit einem bestimmten Namen ab.

### <a name="syntax"></a>Syntax
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter
**-ResourceGroupName &lt;String&gt;**

Gibt den Namen der Ressourcengruppe an, zu der der Mediendienst gehört.

| Aliase | (Keine) |
| --- | --- |
| Erforderlich |true |
| Position? |0 |
| Standardwert |(Keine) |
| Pipelineeingabe akzeptieren? |true(ByPropertyName) |
| Parametersatzname |ResourceGroupParameterSet, AccountNameParameterSet |

Platzhalterzeichen akzeptieren?   false

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

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys
Ruft die Schlüssel eines Mediendiensts ab.

### <a name="syntax"></a>Syntax
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter
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

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey
Generiert einen primären oder sekundären Schlüssel eines Mediendiensts neu.

### <a name="syntax"></a>Syntax
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parameter
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

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys
Synchronisiert Speicherkontoschlüssel für ein Speicherkonto, das dem Mediendienst zugeordnet ist.

### <a name="syntax"></a>Syntax
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter
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

### <a name="inputs"></a>Eingaben
Der Eingabetyp ist der Typ der Objekte, die Sie an das Cmdlet übergeben können.

### <a name="outputs"></a>Ausgaben
Der Ausgabetyp ist der Typ der Objekte, die vom Cmdlet ausgegeben werden.

## <a name="next-step"></a>Nächster Schritt
Informieren Sie sich über die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Nov16_HO5-->


