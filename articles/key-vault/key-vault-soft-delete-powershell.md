---
ms.assetid: 
title: "Azure Key Vault: Verwenden des vorläufigen Löschens mit PowerShell"
description: "Beispiele für Anwendungsfälle für vorläufiges Löschen mit PowerShell-Codeausschnitten"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 1775902aa7ec820c3b6c34eda60961eecd27f014
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Verwenden des vorläufigen Löschens in Key Vault mit PowerShell

Das Azure Key Vault-Feature für vorläufiges Löschen ermöglicht die Wiederherstellung gelöschter Tresore und Tresorobjekte. Insbesondere werden bei vorläufigem Löschen die folgenden Szenarien behandelt:

- Unterstützung für das wiederherstellbare Löschen von Schlüsseltresoren
- Unterstützung für das wiederherstellbare Löschen von Key Vault-Objekten, Schlüsseln, Geheimnissen und Zertifikaten

## <a name="prerequisites"></a>Voraussetzungen

- Azure PowerShell 4.0.0 oder höher: Sie müssen Azure PowerShell installieren und Ihrem Azure-Abonnement zuweisen, sofern noch nicht geschehen. Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Eine veraltete Version unserer Key Vault PowerShell-Ausgabeformatierungsdatei wurde möglicherweise anstelle der richtigen Version in Ihre Umgebung geladen. Sie können einer der anderen Lösungen verwenden:
> 
> - Führen Sie zur Behebung dieses Problems ein Upgrade auf [Azure PowerShell Version 4.4.0 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.2.0) durch.
> - Wenn Sie feststellen, dass die zum vorläufigen Löschen fähige Eigenschaft, die in diesem Thema beschrieben wird, nicht angezeigt wird, verwenden Sie die folgende Abfrage: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.

Spezielle Key Vault-Referenzinformationen für PowerShell finden Sie in der [PowerShell-Referenz für Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Key Vault-Vorgänge werden wie folgt separat über RBAC-Berechtigungen (Role-Based Access Control, rollenbasierte Zugriffssteuerung) verwaltet:

| Vorgang | Beschreibung | Benutzerberechtigung |
|:--|:--|:--|
|Auflisten|Listet gelöschte Schlüsseltresore auf.|Microsoft.KeyVault/deletedVaults/read|
|Wiederherstellen|Stellt einen gelöschten Schlüsseltresor wieder her.|Microsoft.KeyVault/vaults/write|
|Bereinigen|Entfernt einen gelöschten Schlüsseltresor und seine Inhalte endgültig.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Weitere Informationen zu Berechtigungen und Zugriffssteuerung finden Sie unter [Schützen einer Key Vault-Instanz](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Aktivieren des vorläufigen Löschens

Damit Sie einen gelöschten Schlüsseltresor oder in einem Schlüsseltresor gespeicherte Objekte wiederherstellen können, müssen Sie für diesen Schlüsseltresor zunächst vorläufiges Löschen aktivieren.

### <a name="existing-key-vault"></a>Vorhandener Schlüsseltresor

Für einen vorhandenen Schlüsseltresor mit dem Namen „ContosoVault“ aktivieren Sie vorläufiges Löschen wie folgt: 

>[!NOTE]
>Derzeit benötigen Sie die Azure Resource Manager-Ressourcenmanipulation, um die *enableSoftDelete*-Eigenschaft direkt in die Key Vault-Ressource zu schreiben.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Neuer Schlüsseltresor

Für einen neuen Schlüsseltresor wird vorläufiges Löschen bei der Erstellung aktiviert. Dazu wird das Flag für die Aktivierung des vorläufigen Löschens zum Erstellungsbefehl hinzugefügt.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Überprüfen der Aktivierung des vorläufigen Löschens

Um sicherzustellen, dass für einen Schlüsseltresor vorläufiges Löschen aktiviert ist, führen Sie den *get*-Befehl aus, und suchen Sie nach dem Attribut „Soft Delete Enabled?“ und seiner Einstellung: „true“ oder „false“.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Löschen eines mit vorläufigem Löschen geschützten Schlüsseltresors

Der Befehl zum Löschen (oder Entfernen) eines Schlüsseltresors bleibt gleich. Sein Verhalten ändert sich jedoch abhängig davon, ob vorläufiges Löschen aktiviert ist oder nicht.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Wenn Sie den vorherigen Befehl für einen Schlüsseltresor ausführen, für den vorläufiges Löschen nicht aktiviert ist, werden dieser Schlüsseltresor und sein gesamter Inhalt ohne Optionen für die Wiederherstellung endgültig gelöscht.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Schützen der Schlüsseltresore mit vorläufigem Löschen

Vorläufiges Löschen ist aktiviert:

- Wenn ein Schlüsseltresor gelöscht wird, wird er aus der Ressourcengruppe entfernt und in einem reservierten Namespace abgelegt, der nur dem Speicherort seiner Erstellung zugeordnet ist. 
- Auf Objekte in einem gelöschten Schlüsseltresor, beispielsweise Schlüssel, Geheimnisse und Zertifikate, kann nicht zugegriffen werden. Das ändert sich nicht, solange sich ihr Schlüsseltresor im gelöschten Zustand befindet. 
- Der DNS-Name für einen Schlüsseltresor in einem gelöschten Zustand bleibt reserviert, daher kann kein neuer Schlüsseltresor mit demselben Namen erstellt werden.  

Sie können Schlüsseltresore im gelöschten Zustand für Ihr Abonnement mit dem folgenden Befehl anzeigen:

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

Die *Ressourcen-ID* in der Ausgabe bezieht sich auf die ursprüngliche Ressourcen-ID dieses Tresors. Da sich dieser Schlüsseltresor nun in einem gelöschten Zustand befindet, ist keine Ressource mit dieser Ressourcen-ID vorhanden. Anhand des Felds *Id* kann beim Wiederherstellen oder Bereinigen die Ressource ermittelt werden. Im Feld *Scheduled Purge Date* (Geplantes Datum für die Bereinigung) ist angegeben, wann der Tresor endgültig gelöscht (bereinigt) wird, wenn für diesen gelöschten Tresor keine Aktion ausgeführt wird. Die Standardaufbewahrungsdauer, die zum Berechnen von *Scheduled Purge Date* (Geplantes Datum für die Bereinigung) verwendet wird, ist 90 Tage.

## <a name="recovering-a-key-vault"></a>Wiederherstellen eines Schlüsseltresors

Um einen Schlüsseltresor wiederherzustellen, müssen Sie den Schlüsseltresornamen, die Ressourcengruppe und den Speicherort angeben. Notieren Sie sich den Speicherort und die Ressourcengruppe des gelöschten Schlüsseltresors, da Sie diese Angaben für die Wiederherstellung eines Schlüsseltresors benötigen.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Wenn ein Schlüsseltresor wiederhergestellt wird, ist das Ergebnis eine neue Ressource mit der ursprünglichen Ressourcen-ID des Schlüsseltresors. Wenn die Ressourcengruppe, in der sich der Schlüsseltresor befand, entfernt wurde, muss eine neue Ressourcengruppe mit dem gleichen Namen erstellt werden, bevor der Schlüsseltresor wiederhergestellt werden kann.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault-Objekte und vorläufiges Löschen

Den Schlüssel „ContosoFirstKey“ in einem Schlüsseltresor namens „ContosoVault“, für den vorläufiges Löschen aktiviert ist, löschen Sie wie folgt:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Wenn für einen Schlüsseltresor vorläufiges Löschen aktiviert ist und ein Schlüssel gelöscht wird, sieht es weiterhin so aus, als ob der Schlüssel gelöscht wurde – außer beim expliziten Auflisten oder Abrufen gelöschter Schlüssel. Bei den meisten Vorgängen für Schlüssel im gelöschten Zustand tritt ein Fehler auf, außer beim Auflisten, Wiederherstellen oder Bereinigen eines gelöschten Schlüssels. 

Um die Auflistung der gelöschten Schlüssel in einem Schlüsseltresor anzufordern, verwenden Sie beispielsweise den folgenden Befehl:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Übergangsstatus 

Wenn Sie einen Schlüssel in einem Schlüsseltresor löschen, für den vorläufiges Löschen aktiviert ist, dauert es unter Umständen einige Sekunden, bis der Übergang abgeschlossen ist. Während dieses Übergangs sieht es möglicherweise so aus, als ob sich der Schlüssel nicht im aktiven oder gelöschten Zustand befindet. Dieser Befehl listet alle gelöschten Schlüssel im Schlüsseltresor mit dem Namen „ContosoVault“ auf.

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Verwenden des vorläufigen Löschens mit Schlüsseltresorobjekten

Wie bei Schlüsseltresoren verbleiben gelöschte Schlüssel, Geheimnisse oder Zertifikate 90 Tage lang im gelöschten Zustand, es sei denn, Sie stellen sie wieder her oder bereinigen sie. 

#### <a name="keys"></a>Schlüssel

So stellen Sie einen gelöschten Schlüssel wieder her:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

So löschen Sie einen Schlüssel endgültig:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Bei der Bereinigung eines Schlüssels wird er endgültig gelöscht. Das bedeutet, dass er nicht wiederhergestellt werden kann.

Den Aktionen zum **Wiederherstellen** und **Bereinigen** sind in einer Schlüsseltresor-Zugriffsrichtlinie eigene Berechtigungen zugewiesen. Damit ein Benutzer oder Dienstprinzipal eine Aktion zum **Wiederherstellen** oder **Bereinigen** ausführen kann, muss er über die entsprechende Berechtigung für dieses Objekt (Schlüssel oder Geheimnis) in der Schlüsseltresor-Zugriffsrichtlinie verfügen. Die Berechtigung zum **Bereinigen** wird standardmäßig nicht zur Zugriffsrichtlinie eines Schlüsseltresors hinzugefügt, wenn einem Benutzer mit der Verknüpfung „Alle“ alle Berechtigungen gewährt werden. Die Berechtigung zum **Bereinigen** muss explizit gewährt werden. Beispiel: Der folgende Befehl erteilt user@contoso.com die Berechtigung zum Ausführen verschiedener Vorgänge für Schlüssel in *ContosoVault*, einschließlich **Bereinigen**.

#### <a name="set-a-key-vault-access-policy"></a>Festlegen einer Schlüsseltresor-Zugriffsrichtlinie

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Für einen vorhandenen Schlüsseltresor, für den gerade vorläufiges Löschen aktiviert wurde, besitzen Sie unter Umständen keine Berechtigungen zum **Wiederherstellen** und **Bereinigen**.

#### <a name="secrets"></a>Geheimnisse

Aktionen für Geheimnisse in einem Schlüsseltresor werden genau wie bei Schlüsseln über eigene Befehle ausgeführt. Nachfolgend sind die Befehle zum Löschen, Auflisten, Wiederherstellen und Bereinigen von Geheimnissen aufgeführt.

- Löschen eines Geheimnisses mit dem Namen „SQLPassword“: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Auflisten aller gelöschten Geheimnisse in einem Schlüsseltresor: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Wiederherstellen eines Geheimnisses im gelöschten Zustand: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Bereinigen eines Geheimnisses im gelöschten Zustand: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Bei der Bereinigung eines Geheimnisses wird es endgültig gelöscht. Das bedeutet, dass es nicht wiederhergestellt werden kann.

## <a name="purging-and-key-vaults"></a>Bereinigen und Schlüsseltresore

### <a name="key-vault-objects"></a>Schlüsseltresorobjekte

Bei der Bereinigung von Schlüsseln, Geheimnissen oder Zertifikaten werden diese endgültig gelöscht. Das bedeutet, dass sie nicht wiederhergestellt werden können. Der Schlüsseltresor, der das gelöschte Objekt enthielt, bleibt jedoch genau wie alle anderen Objekte im Schlüsseltresor intakt. 

### <a name="key-vaults-as-containers"></a>Schlüsseltresore als Container
Wenn ein Schlüsseltresor bereinigt wird, wird sein gesamter Inhalt, d.h. Schlüssel, Geheimnisse und Zertifikate, endgültig gelöscht. Verwenden Sie zum Bereinigen eines Schlüsseltresors den Befehl `Remove-AzureRmKeyVault` mit der Option `-InRemovedState`. Geben Sie dabei den Speicherort des gelöschten Schlüsseltresors mit dem Argument `-Location location` an. Den Speicherort eines gelöschten Tresor ermitteln Sie mit dem Befehl `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Bei der Bereinigung eines Schlüsseltresors wird er endgültig gelöscht. Das bedeutet, dass er nicht wiederhergestellt werden kann.

### <a name="purge-permissions-required"></a>Berechtigungen zum Bereinigen erforderlich
- Wenn ein Benutzer einen gelöschten Schlüsseltresor bereinigen möchten, sodass der Tresor und sein gesamter Inhalt endgültig entfernt werden, benötigt er die RBAC-Berechtigung zum Ausführen des Vorgangs *Microsoft.KeyVault/locations/deletedVaults/purge/action*. 
- Zum Auflisten des gelöschten Schlüsseltresors benötigt der Benutzer die RBAC-Berechtigung zum Ausführen des Vorgangs *Microsoft.KeyVault/deletedVaults/read*. 
- Standardmäßig besitzt nur ein Abonnementadministrator diese Berechtigungen. 

### <a name="scheduled-purge"></a>Geplante Bereinigung

Beim Auflisten der gelöschten Schlüsseltresorobjekte wird angezeigt, wann ihre Bereinigung durch Key Vault geplant ist. Im Feld *Scheduled Purge Date* (Geplantes Datum für die Bereinigung) ist angegeben, wann ein Schlüsseltresorobjekt endgültig gelöscht wird, wenn keine Aktion ausgeführt wird. Die Aufbewahrungsdauer für ein gelöschtes Schlüsseltresorobjekt ist standardmäßig 90 Tage.

>[!NOTE]
>Ein Tresorobjekt, dessen Bereinigung auf der Grundlage des Felds *Scheduled Purge Date* (Geplantes Datum für Bereinigung) veranlasst wurde, wird endgültig gelöscht. Es kann nicht wiederhergestellt werden.

## <a name="other-resources"></a>Weitere Ressourcen

- Eine Übersicht über das Feature für vorläufiges Löschen finden Sie unter [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](key-vault-ovw-soft-delete.md).
- Eine allgemeine Übersicht über die Nutzung von Azure Key Vault finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md).


