---
title: Problembehandlung der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung | Microsoft-Dokumentation
description: Bietet Informationen zur Behandlung von Problemen mit der Kennwortsynchronisierung
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: e7ad2cb4c464c7095f704cc137f1c42422fbb40e
ms.openlocfilehash: 4c42821b95d666721b84d4976966b4886e517193
ms.lasthandoff: 03/01/2017


---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a>Problembehandlung der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung
Dieses Thema enthält Schritte zum Behandeln von Problemen mit der Kennwortsynchronisierung. Wenn Kennwörter nicht wie erwartet synchronisiert werden, kann dies für eine Teilmenge von Benutzern oder für alle Benutzer der Fall sein.

* Wenn das Problem darin besteht, dass gar keine Kennwörter synchronisiert werden, gehen Sie wie unter [Problembehandlung: Es werden keine Kennwörter synchronisiert](#no-passwords-are-synchronized)beschrieben vor.
* Gehen Sie bei einem Problem mit einzelnen Objekten wie unter [Problembehandlung: Ein einzelnes Objekt synchronisiert Kennwörter nicht](#one-object-is-not-synchronizing-passwords)beschrieben vor.

## <a name="no-passwords-are-synchronized"></a>Kennwörter werden nicht synchronisiert
Führen Sie folgende Schritte aus, um herauszufinden, warum keine Kennwörter synchronisiert werden:

1. Ist der Connect-Server im [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode)? Ein Server im Stagingmodus synchronisiert keine Kennwörter.
2. Führen Sie das Skript im Abschnitt [Abrufen des Status der Kennwortsynchronisierungseinstellungen](#get-the-status-of-password-sync-settings)aus. Dadurch erhalten Sie eine Übersicht über die Konfiguration der Kennwortsynchronisierung.  
![PowerShell-Skriptausgabe für die Kennwortsynchronisierungseinstellungen](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  
3. Wenn das Feature nicht in Azure AD aktiviert ist oder der Synchronisierungskanalstatus nicht aktiviert ist, führen Sie den Connect-Installations-Assistenten aus. Wählen Sie **Synchronisierungsoptionen anpassen** aus, und deaktivieren Sie die Kennwortsynchronisierung. Durch diese Änderung wird das Feature vorübergehend deaktiviert. Führen Sie anschließend den Assistenten erneut aus, und aktivieren Sie die Kennwortsynchronisierung wieder. Führen Sie das Skript noch einmal aus, um sicherzustellen, dass die Konfiguration korrekt ist.
4. Suchen Sie im Ereignisprotokoll nach Fehlern. Suchen Sie nach den folgenden Ereignissen, die auf ein Problem hinweisen würden:
    1. Quelle: „Verzeichnissynchronisierung“ ID: 0, 611, 652, 655 Wenn dies angezeigt wird, haben Sie ein Verbindungsproblem. Die Ereignisprotokollmeldung enthält Gesamtstrukturinformationen, in dem Sie ein Problem haben. Weitere Informationen finden Sie unter [Verbindungsproblem](#connectivity problem).
5. Wenn Sie kein Takt sehen oder nichts funktioniert hat, dann führen Sie [Auslösen einer vollständigen Synchronisierung aller Kennwörter](#trigger-a-full-sync-of-all-passwords) aus. Dieses Skript darf nur einmal ausgeführt werden.
6. Lesen Sie den Abschnitt [Problembehandlung: Ein einzelnes Objekt, das Kennwörter nicht synchronisiert](#one-object-is-not-synchronizing-passwords).

### <a name="connectivity-problem"></a>Verbindungsproblem

1. Verfügen Sie über eine Verbindung mit Azure AD?
2. Verfügt das Konto über die erforderlichen Berechtigungen zum Lesen des Kennworthashes in allen Domänen? Wenn Sie Connect mit Express-Einstellungen installiert haben, sollten die Berechtigungen bereits richtig sein. Wenn Sie die benutzerdefinierte Installation verwendet haben, müssen Sie die Berechtigungen manuell festlegen.
    1. Um das von den Active Directory-Connector verwendete Konto zu suchen, starten Sie den **Synchronization Service Manager**. Navigieren Sie zu **Connectors** und suchen Sie die lokale Active Directory-Gesamtstruktur, für die Sie eine Problembehandlung durchführen. Wählen Sie den Connector aus, und klicken Sie auf **Eigenschaften**. Navigieren Sie zu **Mit Active Directory-Gesamtstruktur verbinden**.  
    ![Vom AD-Connector verwendetes Konto](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    Notieren Sie sich den Benutzernamen und die Domäne, in dem sich das Konto befindet.
    2. Starten Sie **Active Directory-Benutzer und -Computer**. Überprüfen Sie, ob das Konto, das Sie in den vorherigen Schritten gefunden haben, folgende Berechtigungen im Stamm aller Domänen in Ihrer Gesamtstruktur festgelegt hat:
        * Verzeichnisänderungen replizieren
        * Verzeichnisänderungen replizieren: Alle
3. Sind die Domänencontroller durch Azure AD Connect erreichbar? Wenn die Connect-Server keine Verbindung mit allen Domänencontrollern herstellen können, sollten Sie **nur bevorzugte Domänencontroller verwenden** konfigurieren.  
    ![Domänencontroller, der vom AD-Connector verwendet wird](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    Navigieren Sie zurück zum **Synchronization Service Manager** und **Verzeichnispartition konfigurieren**. Wählen Sie Ihre Domäne in **Verzeichnispartitionen auswählen**, aktivieren Sie das Kontrollkästchen **nur bevorzugte Domänencontroller verwenden**, und klicken Sie auf **Konfigurieren**. Geben Sie in der Liste den Domänencontroller an, den Connect zur Kennwortsynchronisierung verwenden soll. Die gleiche Liste wird ebenfalls für den Import und Export verwendet. Führen Sie diese Schritte für alle Ihre Domänen aus.
4. Wenn der Skriptausgabe zufolge kein Takt vorhanden ist, führen Sie das Skript unter [Auslösen einer vollständigen Synchronisierung aller Kennwörter](#trigger-a-full-sync-of-all-passwords)aus.

## <a name="one-object-is-not-synchronizing-passwords"></a>Ein einzelnes Objekt synchronisiert keine Kennwörter
Sie können Probleme mit der Kennwortsynchronisierung einfach beheben, indem Sie den Status eines Objekts überprüfen.

1. Beginnen Sie in **Active Directory-Benutzer und -Computer**. Suchen Sie nach dem Benutzer, und überprüfen Sie, ob **Benutzer muss Kennwort bei der nächsten Anmeldung ändern** deaktiviert ist.  
![Produktive Active Directory-Kennwörter](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  
Falls die Option aktiviert ist, bitten Sie den Benutzer, sich anzumelden und das Kennwort zu ändern. Temporäre Kennwörter werden nicht mit Azure AD synchronisiert.
2. Wenn in Active Directory alles korrekt aussieht, besteht der nächste Schritt darin, dem Benutzer im Synchronisierungsmodul zu folgen. Indem Sie dem Benutzer vom lokalen Active Directory nach Azure AD folgen, können Sie feststellen, ob eine beschreibende Fehlermeldung für das Objekt vorhanden ist.
    1. Starten Sie den **[Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md)**.
    2. Klicken Sie auf **Connectors**.
    3. Wählen Sie den **Active Directory Connector** aus, in dem sich der Benutzer befindet.
    4. Wählen Sie **Search Connector Space**(Connectorbereich durchsuchen) aus.
    5. Wählen Sie im **Bereich** **DN oder Anker** aus. Geben Sie die vollständige DN des Benutzers ein, die Sie behandeln möchten.
    ![Suchen Sie für Benutzer in CS mit DN](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  
    6. Lokalisieren Sie den gesuchten Benutzer, und klicken Sie auf **Eigenschaften**, um alle Attribute zu sehen. Wenn der Benutzer nicht im Suchergebnis ist, dann überprüfen Sie Ihre [Filterregeln](active-directory-aadconnectsync-configure-filtering.md) und stellen Sie sicher, dass Sie [Anwenden und Überprüfen von Änderungen](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) für den Benutzer ausführen, der in Connect angezeigt werden soll.
    7. Klicken Sie auf **Protokoll**, um die Details zur Kennwortsynchronisierung der letzten Woche für das Objekt anzuzeigen.  
    ![Objektprotokolldetails](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  
    Wenn das Objektprotokoll leer ist, konnte Azure AD Connect den Kennworthash nicht in Active Directory lesen. Führen Sie Ihre Problembehandlung mit [Verbindungsfehlern](#connectivity-errors) fort. Wenn Sie einen anderen Wert als **Erfolg** sehen, dann schauen Sie in der Tabelle [Kennwortsynchronisierungsprotokoll](#password-sync-log) nach.
    8. Klicken Sie auf die Registerkarte **Lineage** (Herkunft), und stellen Sie sicher, dass für mindestens eine Synchronisierungsregel die Einstellung **PasswordSync** (Kennwortsynchronisierung) auf **True** (Wahr) festgelegt ist. In der Standardkonfiguration lautet der Name der Synchronisierungsregel **In from AD - User AccountEnabled**(Eingehend von AD – Benutzerkonto aktiviert).  
    ![Herkunftsinformationen zu einem Benutzer](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  
    9. Klicken Sie auf **Metaverse-Objekteigenschaften**. Eine Liste der Attribute wird angezeigt.  
    ![Metaverse-Information](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  
    Überprüfen Sie, ob kein **cloudFiltered**-Attribut vorhanden ist. Stellen Sie sicher, dass die Domänenattribute (domainFQDN und domainNetBios) über die erwarteten Werte verfügen.
    10. Klicken Sie auf die Registerkarte **Connectors**. Stellen Sie sicher, dass Connectors sowohl auf Ihrem lokalen AD als auch auf Azure AD angezeigt werden.
    ![Metaverse-Information](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  
    11. Wählen Sie die Zeile, die Azure AD darstellt, und klicken Sie auf **Eigenschaften**. Klicken Sie auf die Registerkarte **Herkunft**. Das Connectorbereichsobjekt sollte über eine ausgehende Regel verfügen, für die die Einstellung **PasswordSync** (Kennwortsynchronisierung) auf **True** (Wahr) festgelegt ist. In der Standardkonfiguration lautet der Name der Synchronisierungsregel **Out to AAD - User Join**(Ausgehend an AAD – Benutzerverknüpfung).  
    ![Connectorbereichseigenschaften eines Benutzers](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Kennwortsynchronisierungsprotokoll
Die Statusspalte kann die folgenden Werte enthalten:

| Status | Beschreibung |
| --- | --- |
| Erfolgreich |Das Kennwort wurde erfolgreich synchronisiert. |
| FilteredByTarget |Das Kennwort wird auf **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**festgelegt. Das Kennwort wurde nicht synchronisiert. |
| NoTargetConnection |Im Metaverse oder im Azure AD-Connectorbereich befindet sich kein Objekt. |
| SourceConnectorNotPresent |Im lokalen Active Directory Connector-Bereich wurde kein Objekt gefunden. |
| TargetNotExportedToDirectory |Das Objekt im Azure AD-Connectorbereich wurde noch nicht exportiert. |
| MigratedCheckDetailsForMoreInfo |Der Protokolleintrag wurde vor Build 1.0.9125.0 erstellt und wird im Zustand der Vorversion angezeigt. |

## <a name="scripts-to-help-troubleshooting"></a>Hilfe zur Problembehandlung durch Skripts

### <a name="get-the-status-of-password-sync-settings"></a>Abrufen des Status der Kennwortsynchronisierungseinstellungen
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Auslösen einer vollständigen Synchronisierung aller Kennwörter
> [!NOTE]
> Dieses Skript darf nur einmal ausgeführt werden. Wenn Sie dies mehrere Male ausführen müssen, ist etwas anderes das Problem. Wenden Sie sich an Microsoft Support, um Hilfe bei der Fehlerbehebung zu erhalten.

Mit dem folgenden Skript können Sie eine vollständige Synchronisierung aller Kennwörter auslösen:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Nächste Schritte
* [Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md)
* [Azure AD Connect-Synchronisierung: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

