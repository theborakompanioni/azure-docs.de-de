---
title: Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung | Microsoft Docs
description: "Enthält Informationen zur Funktionsweise der Kennwortsynchronisierung sowie zu ihrer Aktivierung."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2016
ms.author: markvi;andkjell
translationtype: Human Translation
ms.sourcegitcommit: 77031b3bbe2372bf2cac34bac45368ac40911641
ms.openlocfilehash: d267a060c9f80391c3edf9bb4b942b4e5d9201ca


---
# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung
In diesem Thema finden Sie alle Informationen, die Sie benötigen, um Benutzerkennwörter aus einem lokalen Active Directory (AD) mit einem cloudbasierten Azure Active Directory (Azure AD) zu synchronisieren.

## <a name="what-is-password-synchronization"></a>Was ist die Kennwortsynchronisierung?
Die Wahrscheinlichkeit, dass Sie nicht weiterarbeiten können, weil Sie ein Kennwort vergessen haben, hängt mit der Anzahl der verschiedenen Kennwörter zusammen, die Sie sich merken müssen. Je mehr Kennwörter Sie sich merken müssen, desto höher ist die Wahrscheinlichkeit, dass Sie eins vergessen. Fragen und Anrufe zum Zurücksetzen von Kennwörtern sowie zu weiteren Problemen in Zusammenhang mit Kennwörtern erfordern die meisten Helpdeskressourcen.

Die Kennwortsynchronisierung ist ein Feature zum Synchronisieren von Benutzerkennwörtern aus einer lokalen Active Directory-Instanz für eine cloudbasierte Azure Active Directory-Instanz (Azure AD).
Dieses Feature ermöglicht es Ihnen, sich mit demselben Kennwort, das Sie zur Anmeldung bei Ihrem lokalen Active Directory verwenden, bei Ihren Azure Active Directory-Diensten (z.B. Office 365, Microsoft Intune, CRM Online und Azure AD-Domänendienste) anzumelden.

![Was ist Azure AD Connect?](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Die Kennwortsynchronisierung verringert die Anzahl von Kennwörtern, die Benutzer sich merken müssen, auf ein einziges Kennwort. So unterstützt dieses Feature Sie bei Folgendem:

* Verbessern der Produktivität der Benutzer
* Reduzieren der Helpdeskkosten  

Wenn Sie die Option [**Verbund mit AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) auswählen, können Sie optional die Kennwortsynchronisierung als zusätzliche Sicherheit für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt.

Die Kennwortsynchronisierung ist eine Erweiterung des durch die Azure AD Connect-Synchronisierung implementierten Verzeichnissynchronisierungsfeatures. Um die Kennwortsynchronisierung in Ihrer Umgebung zu verwenden, müssen Sie Folgendes ausführen:

* Installieren von Azure AD Connect  
* Konfigurieren der Verzeichnissynchronisierung zwischen Ihrem lokalen Active Directory und Ihrem Azure Active Directory
* Aktivieren der Kennwortsynchronisierung

Ausführlichere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

> [!NOTE]
> Weitere Informationen zu Active Directory-Domänendiensten, die für FIPS und Kennwortsynchronisierung konfiguriert sind, finden Sie unter [Kennwortsynchronisierung und FIPS](#password-synchronization-and-fips).
> 
> 

## <a name="how-password-synchronization-works"></a>So funktioniert die Kennwortsynchronisierung
Der Active Directory-Domänendienst speichert Kennwörter in Form einer Hashwertdarstellung des tatsächlichen Benutzerkennworts. Ein Hashwert ist das Ergebnis einer unidirektionalen mathematischen Funktion (des „*Hashalgorithmus*“). Es ist nicht möglich, das Ergebnis einer unidirektionalen Funktion in die Nur-Text-Version eines Kennworts umzukehren. Sie können einen Kennworthash nicht zur Anmeldung in Ihrem lokalen Netzwerk verwenden.

Um Ihr Kennwort zu synchronisieren, extrahiert Azure AD Connect Sync den Kennworthash aus der lokalen Active Directory-Instanz. Vor der Synchronisierung mit dem Azure Active Directory-Authentifizierungsdienst wird der Kennworthash einer zusätzlichen Sicherheitsverarbeitung unterzogen. Kennwörter werden pro Benutzer und in chronologischer Reihenfolge synchronisiert.

Der tatsächliche Datenfluss des Kennwortsynchronisierungsvorgangs ähnelt der Synchronisierung von Benutzerdaten, z. B. des Anzeigenamens oder von E-Mail-Adressen. Kennwörter werden jedoch häufiger synchronisiert als vom Standardzeitfenster für die Verzeichnissynchronisierung für andere Attribute vorgesehen. Der Prozess der Kennwortsynchronisierung wird alle zwei Minuten ausgeführt. Sie können die Häufigkeit der Ausführung nicht ändern. Wenn Sie ein Kennwort synchronisieren, wird das vorhandene Cloudkennwort überschrieben.

Bei der ersten Aktivierung der Kennwortsynchronisierung wird eine anfängliche Synchronisierung der Kennwörter aller im Bereich befindlichen Benutzer durchgeführt. Es ist nicht möglich, explizit eine Teilmenge der Benutzerkennwörter zu definieren, die Sie synchronisieren möchten.

Wenn Sie ein lokales Kennwort ändern, wird das aktualisierte Kennwort synchronisiert. Dies dauert meist nur wenige Minuten.
Das Feature der Kennwortsynchronisierung versucht automatisch, fehlerhafte Synchronisierungsversuche erneut auszuführen. Wenn beim Versuch, ein Kennwort zu synchronisieren, ein Fehler auftritt, wird der Fehler in der Ereignisanzeige protokolliert.

Die Synchronisierung eines Kennworts hat keinen Einfluss auf den derzeit angemeldeten Benutzer.
Wenn eine synchronisierte Kennwortänderung durchgeführt wird, während Sie an einem Clouddienst angemeldet sind, wirkt sich dies nicht direkt auf Ihre aktuelle Clouddienstsitzung aus. Wenn aber für den Clouddienst eine erneute Authentifizierung erforderlich ist, müssen Sie Ihr neues Kennwort angeben.

> [!NOTE]
> Die Kennwortsynchronisierung wird nur für Objekttyp-Benutzer in Active Directory unterstützt. Sie wird vom iNetOrgPerson-Objtktyp nicht unterstützt.
> 
> 

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>So funktioniert die Kennwortsynchronisierung mit Azure AD-Domänendiensten
Sie können die Kennwortsynchronisierung auch dazu verwenden, Ihre lokalen Kennwörter mit den [Azure AD-Domänendiensten](../active-directory-domain-services/active-directory-ds-overview.md)zu synchronisieren. Dieses Szenario ermöglicht es den Azure AD-Domänendiensten, Ihre Benutzer in der Cloud mit allen Methoden zu authentifizieren, die auch in Ihrem lokalen Active Directory zur Verfügung stehen. Dieses Szenario ähnelt der Verwendung des Active Directory-Migrationsprogramms in einer lokalen Umgebung.

### <a name="security-considerations"></a>Sicherheitshinweise
Beim Synchronisieren von Kennwörtern wird die Nur-Text-Version Ihres Kennworts gegenüber dem Kennwortsynchronisierungsfeature, Azure AD oder einem der zugehörigen Dienste nicht offengelegt.

Darüber hinaus besteht keine Notwendigkeit, dass das Kennwort in der lokalen Active Directory-Instanz in einem Format mit umkehrbarer Verschlüsselung gespeichert wird. Ein Digest des Active Directory-Kennworthashs wird zur Übertragung zwischen dem lokalen Active Directory und Azure Active Directory verwendet. Der Digest des Kennworthashs kann nicht für den Zugriff auf Ressourcen in Ihrer lokalen Umgebung verwendet werden.

### <a name="password-policy-considerations"></a>Überlegungen zur Kennwortrichtlinie
Es gibt zwei Arten von Kennwortrichtlinien, die von der Aktivierung der Kennwortsynchronisierung betroffen sind:

1. Kennwortkomplexitätsrichtlinie
2. Kennwortablaufrichtlinie

**Password complexity policy**  
 Wenn Sie die Kennwortsynchronisierung aktivieren, setzen die Kennwortkomplexitätsrichtlinien in Ihrem lokalen Active Directory die Komplexitätsrichtlinien in der Cloud für synchronisierte Benutzer außer Kraft. Sie können alle gültigen Kennwörter Ihrer lokalen Active Directory-Instanz für den Zugriff auf Azure AD-Dienste verwenden.

> [!NOTE]
> Kennwörter für Benutzer, die direkt in der Cloud erstellt werden, unterliegen auch weiterhin in der Cloud definierten Kennwortrichtlinien.
> 
> 

**Password expiration policy**  
Wenn sich ein Benutzer im Bereich der Kennwortsynchronisierung befindet, wird das Cloudkontokennwort auf „*Läuft nie ab*“ festgelegt.
Sie können sich mit einem synchronisierten Kennwort, das in der lokalen Umgebung abgelaufen ist, weiterhin bei Ihren Clouddiensten anmelden. Ihr Cloudkennwort wird aktualisiert, wenn Sie das Kennwort in der lokalen Umgebung das nächste Mal ändern.

### <a name="overwriting-synchronized-passwords"></a>Überschreiben synchronisierter Kennwörter
Ein Administrator kann Ihr Kennwort mithilfe von Windows PowerShell manuell zurücksetzen.

In diesem Fall überschreibt das neue Kennwort Ihr synchronisiertes Kennwort, und alle in der Cloud definierten Kennwortrichtlinien gelten für das neue Kennwort.

Wenn Sie das lokale Kennwort erneut ändern, wird das neue Kennwort mit der Cloud synchronisiert, und das manuell aktualisierte Kennwort wird überschrieben.

## <a name="enabling-password-synchronization"></a>Aktivieren der Kennwortsynchronisierung
Die Kennwortsynchronisierung wird automatisch aktiviert, wenn Sie Azure AD Connect mit den **Expresseinstellungen**installieren. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Connect mit Expresseinstellungen](connect/active-directory-aadconnect-get-started-express.md).

Wenn Sie beim Installieren von Azure AD Connect benutzerdefinierte Einstellungen verwenden, aktivieren Sie die Kennwortsynchronisierung auf der Seite „Benutzeranmeldung“. Weitere Informationen finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](connect/active-directory-aadconnect-get-started-custom.md).

![Aktivieren der Kennwortsynchronisierung](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Kennwortsynchronisierung und FIPS
Wenn Ihr Server wegen FIPS (Federal Information Processing Standard) gesperrt ist, wurde MD5 deaktiviert.

**Führen Sie zum Aktivieren von MD5 für die Kennwortsynchronisierung die folgenden Schritte aus:**

1. Wechseln Sie zu **%programfiles%\Azure AD Sync\Bin**.
2. Öffnen Sie **miiserver.exe.config**.
3. Wechseln Sie zum Knoten **configuration/runtime** (am Ende der Datei).
4. Fügen Sie den folgenden Knoten hinzu: `<enforceFIPSPolicy enabled="false"/>`
5. Speichern Sie die Änderungen.

Der folgende Codeausschnitt zeigt, wie dies aussehen sollte:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Informationen über Sicherheitsfragen und FIPS finden Sie im Blogbeitrag [AAD Password Sync, Encryption and FIPS compliance](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Problembehandlung bei der Kennwortsynchronisierung
Wenn Kennwörter nicht wie erwartet synchronisiert werden, kann dies für eine Teilmenge von Benutzern oder für alle Benutzer der Fall sein.

* Gehen Sie bei einem Problem mit einzelnen Objekten wie unter [Problembehandlung: Ein einzelnes Objekt synchronisiert Kennwörter nicht](#troubleshoot-one-object-that-is-not-synchronizing-passwords)beschrieben vor.
* Wenn das Problem darin besteht, dass gar keine Kennwörter synchronisiert werden, gehen Sie wie unter [Problembehandlung: Es werden keine Kennwörter synchronisiert](#troubleshoot-issues-where-no-passwords-are-synchronized)beschrieben vor.

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Problembehandlung: Ein einzelnes Objekt synchronisiert Kennwörter nicht
Sie können Probleme mit der Kennwortsynchronisierung einfach beheben, indem Sie den Status eines Objekts überprüfen.

Beginnen Sie in **Active Directory-Benutzer und -Computer**. Suchen Sie nach dem Benutzer, und überprüfen Sie, ob **Benutzer muss Kennwort bei der nächsten Anmeldung ändern** deaktiviert ist.
![Produktive Active Directory-Kennwörter](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
 Falls die Option aktiviert ist, bitten Sie den Benutzer, sich anzumelden und das Kennwort zu ändern. Temporäre Kennwörter werden nicht mit Azure AD synchronisiert.

Wenn in Active Directory alles korrekt aussieht, besteht der nächste Schritt darin, dem Benutzer im Synchronisierungsmodul zu folgen. Indem Sie dem Benutzer vom lokalen Active Directory nach Azure AD folgen, können Sie feststellen, ob eine beschreibende Fehlermeldung für das Objekt vorhanden ist.

1. Starten Sie den **[Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Klicken Sie auf **Connectors**.
3. Wählen Sie den **Active Directory Connector** aus, in dem sich der Benutzer befindet.
4. Wählen Sie **Search Connector Space**(Connectorbereich durchsuchen) aus.
5. Suchen Sie den gewünschten Benutzer.
6. Klicken Sie auf die Registerkarte **Lineage** (Herkunft), und stellen Sie sicher, dass für mindestens eine Synchronisierungsregel die Einstellung **PasswordSync** (Kennwortsynchronisierung) auf **True** (Wahr) festgelegt ist. In der Standardkonfiguration lautet der Name der Synchronisierungsregel **In from AD - User AccountEnabled**(Eingehend von AD – Benutzerkonto aktiviert).  
    ![Herkunftsinformationen zu einem Benutzer](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. [Folgen Sie dem Benutzer](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) dann durch die Metaverse zum Azure AD-Connectorbereich. Das Connectorbereichsobjekt sollte über eine ausgehende Regel verfügen, für die die Einstellung **PasswordSync** (Kennwortsynchronisierung) auf **True** (Wahr) festgelegt ist. In der Standardkonfiguration lautet der Name der Synchronisierungsregel **Out to AAD - User Join**(Ausgehend an AAD – Benutzerverknüpfung).  
    ![Connectorbereichseigenschaften eines Benutzers](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Klicken Sie auf **Protokoll**, um die Details zur Kennwortsynchronisierung der letzten Woche für das Objekt anzuzeigen.  
    ![Objektprotokolldetails](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Die Statusspalte kann die folgenden Werte enthalten:

| Status | Beschreibung |
| --- | --- |
| Erfolgreich |Das Kennwort wurde erfolgreich synchronisiert. |
| FilteredByTarget |Das Kennwort wird auf **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**festgelegt. Das Kennwort wurde nicht synchronisiert. |
| NoTargetConnection |Im Metaverse oder im Azure AD-Connectorbereich befindet sich kein Objekt. |
| SourceConnectorNotPresent |Im lokalen Active Directory Connector-Bereich wurde kein Objekt gefunden. |
| TargetNotExportedToDirectory |Das Objekt im Azure AD-Connectorbereich wurde noch nicht exportiert. |
| MigratedCheckDetailsForMoreInfo |Der Protokolleintrag wurde vor Build 1.0.9125.0 erstellt und wird im Zustand der Vorversion angezeigt. |

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Problembehandlung: Es werden keine Kennwörter synchronisiert
Führen Sie zunächst das Skript im Abschnitt [Abrufen des Status der Kennwortsynchronisierungseinstellungen](#get-the-status-of-password-sync-settings)aus. Dadurch erhalten Sie eine Übersicht über die Konfiguration der Kennwortsynchronisierung.  
![PowerShell-Skriptausgabe für die Kennwortsynchronisierungseinstellungen](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
 Wenn das Feature nicht in Azure AD aktiviert ist oder der Synchronisierungskanalstatus nicht aktiviert ist, führen Sie den Connect-Installations-Assistenten aus. Wählen Sie **Synchronisierungsoptionen anpassen** aus, und deaktivieren Sie die Kennwortsynchronisierung. Durch diese Änderung wird das Feature vorübergehend deaktiviert. Führen Sie anschließend den Assistenten erneut aus, und aktivieren Sie die Kennwortsynchronisierung wieder. Führen Sie das Skript noch einmal aus, um sicherzustellen, dass die Konfiguration korrekt ist.

Wenn der Skriptausgabe zufolge kein Takt vorhanden ist, führen Sie das Skript unter [Auslösen einer vollständigen Synchronisierung aller Kennwörter](#trigger-a-full-sync-of-all-passwords)aus. Dieses Skript kann auch für andere Szenarien verwendet werden, in denen die Konfiguration korrekt ist, aber keine Kennwörter synchronisiert werden.

#### <a name="get-the-status-of-password-sync-settings"></a>Abrufen des Status der Kennwortsynchronisierungseinstellungen
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Azure Active Directory (Microsoft)"}
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
* [Azure AD Connect-Synchronisierung: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)




<!--HONumber=Nov16_HO3-->


