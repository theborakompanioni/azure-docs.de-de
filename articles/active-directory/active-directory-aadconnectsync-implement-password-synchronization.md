<properties
	pageTitle="Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung | Microsoft Azure"
	description="Enthält Informationen zur Funktionsweise der Kennwortsynchronisierung sowie zu ihrer Aktivierung."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>
<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="markusvi;andkjell"/>


# Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung

Bei der Kennwortsynchronisierung können Sie Ihr lokales Active Directory-Kennwort auch verwenden, um sich an Ihrer Azure Active Directory-Instanz anzumelden.

Dieses Thema enthält Informationen, die Sie benötigen, um die Kennwortsynchronisierung in Ihrer Umgebung zu aktivieren und die Problembehandlung dafür durchzuführen.


## Was ist die Kennwortsynchronisierung?

Die Kennwortsynchronisierung ist ein Feature zum Synchronisieren von Benutzerkennwörtern aus einer lokalen Active Directory-Instanz für eine cloudbasierte Azure Active Directory-Instanz (Azure AD). Dieses Feature ermöglicht es Ihnen, sich bei Ihren Azure Active Directory-Diensten (wie Office 365, Microsoft Intune und CRM Online) mit dem gleichen Kennwort anzumelden, mit dem Sie sich am lokalen Netzwerk anmelden. Um dieses Feature verwenden zu können, müssen Sie die Azure Active Directory Connect-Synchronisierungsdienste (Azure AD Connect Sync) installieren.


> [AZURE.NOTE] Weitere Informationen zu Active Directory-Domänendiensten, die für FIPS und Kennwortsynchronisierung konfiguriert sind, finden Sie unter [Kennwortsynchronisierung und FIPS](#password-synchronization-and-fips).

## So funktioniert die Kennwortsynchronisierung

Die Kennwortsynchronisierung ist eine Erweiterung des durch die Azure AD Connect-Synchronisierung implementierten Verzeichnissynchronisierungsfeatures. Für dieses Feature muss die Verzeichnissynchronisierung zwischen Ihrer lokalen AD-Instanz und Azure Active Directory konfiguriert werden.

Der Active Directory-Domänendienst speichert Kennwörter in Form einer Hashwertdarstellung des tatsächlichen Benutzerkennworts. Sie können einen Kennworthash nicht für folgende Zwecke nutzen:

- Anmelden am lokalen Netzwerk

- Wiederherstellen der Klartextversion eines Kennworts

Um Ihr Kennwort zu synchronisieren, extrahiert Azure AD Connect Sync den Kennworthash aus der lokalen Active Directory-Instanz. Vor der Synchronisierung mit dem Azure Active Directory-Authentifizierungsdienst wird der Kennworthash einer zusätzlichen Sicherheitsverarbeitung unterzogen. Der tatsächliche Datenfluss des Kennwortsynchronisierungsvorgangs ähnelt der Synchronisierung von Benutzerdaten, z. B. des Anzeigenamens oder von E-Mail-Adressen.

Kennwörter werden wie folgt synchronisiert:

- Pro Benutzer

- In chronologischer Reihenfolge

- Häufiger als es das Standardzeitfenster für die Verzeichnissynchronisierung für andere Attribute vorsieht

Mit einem synchronisierten Kennwort wird das vorhandene Cloudkennwort überschrieben.

Bei der ersten Aktivierung der Kennwortsynchronisierung wird eine anfängliche Synchronisierung der Kennwörter aller im Bereich befindlichen Benutzer durchgeführt. Es ist nicht möglich, explizit eine Teilmenge der Benutzerkennwörter zu definieren, die Sie synchronisieren möchten.

Wenn Sie ein lokales Kennwort ändern, wird das aktualisierte Kennwort synchronisiert. Dies dauert meist nur wenige Minuten. Das Kennwortsynchronisierungsfeature versucht automatisch, fehlerhafte Kennwortsynchronisierungen erneut auszuführen. Wenn beim Versuch, ein Kennwort zu synchronisieren, ein Fehler auftritt, wird der Fehler in der Ereignisanzeige protokolliert.

Die Synchronisierung eines Kennworts hat keinen Einfluss auf den derzeit angemeldeten Benutzer. Wenn eine synchronisierte Kennwortänderung durchgeführt wird, während Sie an einem Clouddienst angemeldet sind, wirkt sich dies nicht direkt auf Ihre aktuelle Clouddienstsitzung aus. Wenn aber für den Clouddienst eine erneute Authentifizierung erforderlich ist, müssen Sie Ihr neues Kennwort angeben.

> [AZURE.NOTE] Die Kennwortsynchronisierung wird nur für Objekttyp-Benutzer in Active Directory unterstützt. Sie wird vom iNetOrgPerson-Objtktyp nicht unterstützt.

### So funktioniert die Kennwortsynchronisierung mit Azure AD-Domänendiensten

Wenn Sie diesen Dienst in Azure AD aktivieren, müssen Sie die Option für die Kennwortsynchronisierung festlegen, damit ein einmaliges Anmelden für die Benutzer möglich wird. Wenn dieser Dienst aktiviert wird, ändert sich das Verhalten der Kennwortsynchronisierung. Die Kennworthashes werden aus Ihrer lokalen Active Directory-Instanz unverändert mit den Azure AD-Domänendiensten synchronisiert. Die Funktion ähnelt dem Active Directory-Migrationsprogramm ( Active Directory Migration Tool, ADMT). Sie ermöglicht es den Azure AD-Domänendiensten, Benutzer mit allen Methoden zu authentifizieren, die auch in der lokalen Active Directory-Instanz zur Verfügung stehen.

### Sicherheitshinweise

Beim Synchronisieren von Kennwörtern wird die Klartextversion Ihres Kennworts gegenüber dem Kennwortsynchronisierungsfeature, Azure AD oder einem der zugehörigen Dienste nicht offengelegt.

Darüber hinaus besteht keine Notwendigkeit, dass das Kennwort in der lokalen Active Directory-Instanz in einem Format mit umkehrbarer Verschlüsselung gespeichert wird. Ein Digest des Active Directory-Kennworthashs wird zur Übertragung zwischen dem lokalen Active Directory und Azure Active Directory verwendet. Der Digest des Kennworthashs kann nicht für den Zugriff auf Ressourcen in Ihrer lokalen Umgebung verwendet werden.

### Überlegungen zur Kennwortrichtlinie

Es gibt zwei Arten von Kennwortrichtlinien, die von der Aktivierung der Kennwortsynchronisierung betroffen sind:

1. Kennwortkomplexitätsrichtlinie
2. Kennwortablaufrichtlinie

**Kennwortkomplexitätsrichtlinie**

Aktivieren der Kennwortsynchronisierung

- Die Kennwortkomplexitätsrichtlinien in Ihrer lokalen Active Directory-Instanz setzen für synchronisierte Benutzer die Komplexitätsrichtlinien in der Cloud außer Kraft. 

- Sie können alle gültigen Kennwörter Ihrer lokalen Active Directory-Instanz für den Zugriff auf Azure AD-Dienste verwenden.

> [AZURE.NOTE] Kennwörter für Benutzer, die direkt in der Cloud erstellt werden, unterliegen auch weiterhin in der Cloud definierten Kennwortrichtlinien.

**Kennwortablaufrichtlinie**

Für Benutzer im Bereich der Kennwortsynchronisierung gilt Folgendes:

- Das Cloudkontokennwort ist auf *Läuft nie ab* festgelegt. 

- Sie können sich mit einem synchronisierten Kennwort, das in der lokalen Umgebung abgelaufen ist, weiterhin an Clouddiensten anmelden.

Ihr Cloudkennwort wird aktualisiert, wenn Sie das Kennwort in der lokalen Umgebung das nächste Mal ändern.

### Überschreiben synchronisierter Kennwörter

Ein Administrator kann Ihr Kennwort mit PowerShell manuell zurücksetzen.

In diesem Fall überschreibt das neue Kennwort Ihr synchronisiertes Kennwort, und alle in der Cloud definierten Kennwortrichtlinien gelten für das neue Kennwort.

Wenn Sie das lokale Kennwort erneut ändern, wird das neue Kennwort mit der Cloud synchronisiert, und das manuell aktualisierte Kennwort wird überschrieben.


## Aktivieren der Kennwortsynchronisierung

Es gibt zwei Optionen zum Aktivieren der Kennwortsynchronisierung:

- Wenn Sie beim Installieren von Azure AD Connect die Express-Einstellungen verwenden, wird die Kennwortsynchronisierung standardmäßig aktiviert.

- Wenn Sie beim Installieren von Azure AD Connect benutzerdefinierte Einstellungen verwenden, aktivieren Sie die Kennwortsynchronisierung auf der Seite „Benutzeranmeldung“.


![Aktivieren der Kennwortsynchronisierung](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)


Wenn Sie die Option **Verbund mit AD FS** auswählen, können Sie die Kennwortsynchronisierung optional als zusätzliche Sicherheit für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt. Sie können sie auch aktivieren, wenn Sie vorhaben, die Azure AD-Domänendienste zu verwenden.

### Kennwortsynchronisierung und FIPS

Wenn Ihr Server wegen FIPS (Federal Information Processing Standard) gesperrt ist, wurde MD5 deaktiviert. Um MD5 für die Kennwortsynchronisierung zu aktivieren, fügen Sie im Verzeichnis „C:\\Programme\\Azure AD Sync\\Bin“ in der Datei „miiserver.exe.config“ den Schlüssel „enforceFIPSPolicy“ ein.

```
<configuration>
    <runtime>
        <enforceFIPSPolicy enabled="false"/>
    </runtime>
</configuration>
```

Den Knoten „configuration/runtime“ finden Sie am Ende der config-Datei.

Informationen über Sicherheitsfragen und FIPS finden Sie im Blogbeitrag [AAD Password Sync, Encryption and FIPS compliance](http://blogs.technet.com/b/ad/archive/2014/06/28/aad-password-sync-encryption-and-and-fips-compliance.aspx) (in englischer Sprache).


## Problembehandlung bei der Kennwortsynchronisierung

**Führen Sie zum Beheben von Problemen bei der Kennwortsynchronisierung die folgenden Schritte aus:**

1. Öffnen Sie den **Synchronization Service Manager**.

2. Klicken Sie auf **Connectors**.

3. Wählen Sie den Active Directory-Connector aus, in dem sich der Benutzer befindet.

4. Wählen Sie **Connectorbereich durchsuchen**.

5. Suchen Sie den gewünschten Benutzer.

6. Wählen Sie die Registerkarte **Herkunft**, und stellen Sie sicher, dass für mindestens eine Synchronisierungsregel die **Kennwortsynchronisierung** als **Wahr** angezeigt wird. In der Standardkonfiguration lautet der Name der Synchronisierungsregel **Eingehend von AD – Benutzerkonto aktiviert**.

    ![Herkunftsinformationen eines Benutzers](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)

7. Außerdem sollten Sie [dem Benutzer durch das Metaverse zum Azure AD-Connectorbereich folgen](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system). Das Connectorbereichsobjekt sollte über eine ausgehende Regel verfügen, für die **Kennwortsynchronisierung** auf **Wahr** festgelegt ist. In der Standardkonfiguration lautet der Name der Synchronisierungsregel **Ausgehend von AAD – Benutzerverknüpfung**.

    ![Connectorbereichseigenschaften eines Benutzers](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)

8. Um die Details zur Kennwortsynchronisierung der letzten Woche für das Objekt anzuzeigen, klicken Sie auf **Protokoll...**.

    ![Objektprotokolldetails](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)

Die Statusspalte kann die folgenden Werte enthalten:

| Status | Beschreibung |
| ---- | ----- |
| Erfolgreich | Das Kennwort wurde erfolgreich synchronisiert. |
| FilteredByTarget | Das Kennwort wird auf **Benutzer muss Kennwort bei der nächsten Anmeldung ändern** festgelegt. Das Kennwort wurde nicht synchronisiert. |
| NoTargetConnection | Im Metaverse oder im Azure AD-Connectorbereich befindet sich kein Objekt. |
| SourceConnectorNotPresent | Im lokalen Active Directory Connector-Bereich wurde kein Objekt gefunden. |
| TargetNotExportedToDirectory | Das Objekt im Azure AD-Connectorbereich wurde noch nicht exportiert. |
| MigratedCheckDetailsForMoreInfo | Der Protokolleintrag wurde vor Build 1.0.9125.0 erstellt und wird im Zustand der Vorversion angezeigt. |


## Auslösen einer vollständigen Synchronisierung aller Kennwörter

Normalerweise ist es nicht erforderlich, eine vollständige Synchronisierung aller Kennwörter zu erzwingen. Bei Bedarf können Sie eine vollständige Synchronisierung aller Kennwörter aber mit dem folgenden Skript auslösen:

    $adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
    $aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
    Import-Module adsync
    $c = Get-ADSyncConnector -Name $adConnector
    $p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter “Microsoft.Synchronize.ForceFullPasswordSync”, String, ConnectorGlobal, $null, $null, $null
    $p.Value = 1
    $c.GlobalParameters.Remove($p.Name)
    $c.GlobalParameters.Add($p)
    $c = Add-ADSyncConnector -Connector $c
    Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
    Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true




## Nächste Schritte

* [Azure AD Connect-Synchronisierung: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0427_2016-->