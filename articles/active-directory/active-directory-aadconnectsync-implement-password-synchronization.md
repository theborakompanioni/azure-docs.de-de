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
	ms.date="05/02/2016"
	ms.author="markusvi;andkjell"/>


# Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung

Dieses Thema bietet Ihnen die Informationen, die Sie benötigen, um Benutzerkennwörter aus einem lokalen Active Directory (AD) mit einem cloudbasierten Azure Active Directory (Azure AD) zu synchronisieren.


## Was ist die Kennwortsynchronisierung?

Die Wahrscheinlichkeit, dass Sie nicht weiterarbeiten können, weil Sie ein Kennwort vergessen haben, hängt mit der Anzahl der verschiedenen Kennwörter zusammen, die Sie sich merken müssen. Je mehr Kennwörter Sie sich merken müssen, desto höher ist die Wahrscheinlichkeit, dass Sie eins vergessen. Fragen und Anrufe zum Zurücksetzen von Kennwörtern sowie zu weiteren Problemen in Zusammenhang mit Kennwörtern erfordern die meisten Helpdeskressourcen.

Die Kennwortsynchronisierung ist ein Feature zum Synchronisieren von Benutzerkennwörtern aus einer lokalen Active Directory-Instanz für eine cloudbasierte Azure Active Directory-Instanz (Azure AD). Dieses Feature ermöglicht es Ihnen, sich bei Ihren Azure Active Directory-Diensten (wie Office 365, Microsoft Intune, CRM Online und den Azure AD-Domänendiensten) mit dem gleichen Kennwort anzumelden, mit dem Sie sich bei Ihrem lokalen Active Directory anmelden.

![Was ist Azure AD Connect?](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Die Kennwortsynchronisierung verringert die Anzahl von Kennwörtern, die Benutzer sich merken müssen, auf ein einziges Kennwort. So unterstützt dieses Feature Sie bei Folgendem:

- Verbessern der Produktivität der Benutzer 
- Senken der Kosten in Verbindung mit dem Helpdesk  

Wenn Sie die Option [**Verbund mit AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) auswählen, können Sie die Kennwortsynchronisierung optional auch als zusätzliche Sicherheit für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt.

Die Kennwortsynchronisierung ist eine Erweiterung des durch die Azure AD Connect-Synchronisierung implementierten Verzeichnissynchronisierungsfeatures. Um die Kennwortsynchronisierung in Ihrer Umgebung zu verwenden, müssen Sie Folgendes ausführen:

- Installieren von Azure AD Connect  

- Konfigurieren der Verzeichnissynchronisierung zwischen Ihrem lokalen Active Directory und Ihrem Azure Active Directory

- Aktivieren der Kennwortsynchronisierung


Ausführlichere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).



> [AZURE.NOTE] Weitere Informationen zu Active Directory-Domänendiensten, die für FIPS und Kennwortsynchronisierung konfiguriert sind, finden Sie unter [Kennwortsynchronisierung und FIPS](#password-synchronization-and-fips).

## So funktioniert die Kennwortsynchronisierung

Der Active Directory-Domänendienst speichert Kennwörter in Form einer Hashwertdarstellung des tatsächlichen Benutzerkennworts. Ein Hashwert ist das Ergebnis einer unidirektionalen mathematischen Funktion (des „*Hashalgorithmus*“). Es gibt keine Methode, das Ergebnis einer unidirektionalen Funktion in die Nur-Text-Version eines Kennworts umzukehren. Sie können einen Kennworthash nicht zur Anmeldung in Ihrem lokalen Netzwerk verwenden.

Um Ihr Kennwort zu synchronisieren, extrahiert Azure AD Connect Sync den Kennworthash aus der lokalen Active Directory-Instanz. Vor der Synchronisierung mit dem Azure Active Directory-Authentifizierungsdienst wird der Kennworthash einer zusätzlichen Sicherheitsverarbeitung unterzogen. Kennwörter werden pro Benutzer und in chronologischer Reihenfolge synchronisiert.

Der tatsächliche Datenfluss des Kennwortsynchronisierungsvorgangs ähnelt der Synchronisierung von Benutzerdaten, z. B. des Anzeigenamens oder von E-Mail-Adressen. Kennwörter werden jedoch häufiger synchronisiert als vom Standardzeitfenster für die Verzeichnissynchronisierung für andere Attribute vorgesehen. Der Prozess der Kennwortsynchronisierung wird alle zwei Minuten ausgeführt. Sie können die Häufigkeit der Ausführung nicht ändern. Wenn Sie ein Kennwort synchronisieren, wird das vorhandene Cloudkennwort überschrieben.

Bei der ersten Aktivierung der Kennwortsynchronisierung wird eine anfängliche Synchronisierung der Kennwörter aller im Bereich befindlichen Benutzer durchgeführt. Es ist nicht möglich, explizit eine Teilmenge der Benutzerkennwörter zu definieren, die Sie synchronisieren möchten.

Wenn Sie ein lokales Kennwort ändern, wird das aktualisierte Kennwort synchronisiert. Dies dauert meist nur wenige Minuten. Das Feature der Kennwortsynchronisierung versucht automatisch, fehlerhafte Synchronisierungsversuche erneut auszuführen. Wenn beim Versuch, ein Kennwort zu synchronisieren, ein Fehler auftritt, wird der Fehler in der Ereignisanzeige protokolliert.

Die Synchronisierung eines Kennworts hat keinen Einfluss auf den derzeit angemeldeten Benutzer. Wenn eine synchronisierte Kennwortänderung durchgeführt wird, während Sie an einem Clouddienst angemeldet sind, wirkt sich dies nicht direkt auf Ihre aktuelle Clouddienstsitzung aus. Wenn aber für den Clouddienst eine erneute Authentifizierung erforderlich ist, müssen Sie Ihr neues Kennwort angeben.

> [AZURE.NOTE] Die Kennwortsynchronisierung wird nur für Objekttyp-Benutzer in Active Directory unterstützt. Sie wird vom iNetOrgPerson-Objtktyp nicht unterstützt.

### So funktioniert die Kennwortsynchronisierung mit Azure AD-Domänendiensten

Sie können die Kennwortsynchronisierung auch dazu verwenden, Ihre lokalen Kennwörter mit den [Azure AD-Domänendiensten](../active-directory-domain-services/active-directory-ds-overview.md) zu synchronisieren. Dieses Szenario ermöglicht es den Azure AD-Domänendiensten, Ihre Benutzer in der Cloud mit allen Methoden zu authentifizieren, die auch in Ihrem lokalen Active Directory zur Verfügung stehen. Dieses Szenario ähnelt der Verwendung des Active Directory-Migrationsprogramms in einer lokalen Umgebung.


### Sicherheitshinweise

Beim Synchronisieren von Kennwörtern wird die Klartextversion Ihres Kennworts gegenüber dem Kennwortsynchronisierungsfeature, Azure AD oder einem der zugehörigen Dienste nicht offengelegt.

Darüber hinaus besteht keine Notwendigkeit, dass das Kennwort in der lokalen Active Directory-Instanz in einem Format mit umkehrbarer Verschlüsselung gespeichert wird. Ein Digest des Active Directory-Kennworthashs wird zur Übertragung zwischen dem lokalen Active Directory und Azure Active Directory verwendet. Der Digest des Kennworthashs kann nicht für den Zugriff auf Ressourcen in Ihrer lokalen Umgebung verwendet werden.

### Überlegungen zur Kennwortrichtlinie

Es gibt zwei Arten von Kennwortrichtlinien, die von der Aktivierung der Kennwortsynchronisierung betroffen sind:

1. Kennwortkomplexitätsrichtlinie
2. Kennwortablaufrichtlinie

**Kennwortkomplexitätsrichtlinie**

Wenn Sie die Kennwortsynchronisierung aktivieren, setzen die Kennwortkomplexitätsrichtlinien in Ihrem lokalen Active Directory für synchronisierte Benutzer die Komplexitätsrichtlinien in der Cloud außer Kraft. Sie können alle gültigen Kennwörter Ihrer lokalen Active Directory-Instanz für den Zugriff auf Azure AD-Dienste verwenden.

> [AZURE.NOTE] Kennwörter für Benutzer, die direkt in der Cloud erstellt werden, unterliegen auch weiterhin in der Cloud definierten Kennwortrichtlinien.

**Kennwortablaufrichtlinie**

Wenn ein Benutzer sich im Bereich der Kennwortsynchronisierung befindet, wird das Kennwort für das Cloudkonto auf *Läuft nie ab* festgelegt. Sie können sich mit einem synchronisierten Kennwort, das in Ihrer lokalen Umgebung bereits abgelaufen ist, weiterhin bei Clouddiensten anmelden. Ihr Cloudkennwort wird aktualisiert, wenn Sie das Kennwort in der lokalen Umgebung das nächste Mal ändern.

### Überschreiben synchronisierter Kennwörter

Ein Administrator kann Ihr Kennwort mithilfe von Windows PowerShell manuell zurücksetzen.

In diesem Fall überschreibt das neue Kennwort Ihr synchronisiertes Kennwort, und alle in der Cloud definierten Kennwortrichtlinien gelten für das neue Kennwort.

Wenn Sie das lokale Kennwort erneut ändern, wird das neue Kennwort mit der Cloud synchronisiert, und das manuell aktualisierte Kennwort wird überschrieben.


## Aktivieren der Kennwortsynchronisierung

Die Kennwortsynchronisierung wird automatisch aktiviert, wenn Sie Azure AD Connect mit den **Expresseinstellungen** installieren. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Connect mit Expresseinstellungen](active-directory-aadconnect-get-started-express.md).

Wenn Sie beim Installieren von Azure AD Connect benutzerdefinierte Einstellungen verwenden, aktivieren Sie die Kennwortsynchronisierung auf der Seite „Benutzeranmeldung“. Weitere Informationen finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md)


![Aktivieren der Kennwortsynchronisierung](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)


### Kennwortsynchronisierung und FIPS

Wenn Ihr Server wegen FIPS (Federal Information Processing Standard) gesperrt ist, wurde MD5 deaktiviert.


**Führen Sie zum Aktivieren von MD5 für die Kennwortsynchronisierung die folgenden Schritte aus:**

    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>

1. Wechseln Sie zu **%programfiles%\\Azure AD Sync\\Bin**.
2. Öffnen Sie **miiserver.exe.config**.
2. Navigieren Sie zum Knoten **configuration/runtime** (am Ende der Datei). 
3. Fügen Sie den folgenden Knoten hinzu: **<enforceFIPSPolicy enabled="false"/>** 
4. Speichern Sie die Änderungen.

Informationen über Sicherheitsfragen und FIPS finden Sie im Blogbeitrag [AAD Password Sync, Encryption and FIPS compliance](http://blogs.technet.com/b/ad/archive/2014/06/28/aad-password-sync-encryption-and-and-fips-compliance.aspx) (in englischer Sprache).


## Problembehandlung bei der Kennwortsynchronisierung

Sie können Probleme mit der Kennwortsynchronisierung einfach beheben, indem Sie den aktuellen Status eines Objekts überprüfen.


**Führen Sie zum Beheben von Problemen bei der Kennwortsynchronisierung die folgenden Schritte aus:**

1. Starten Sie den **[Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md)**.

2. Klicken Sie auf **Connectors**.

3. Wählen Sie den **Active Directory-Connector** aus, in dem sich der Benutzer befindet.

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

<!---HONumber=AcomDC_0518_2016-->