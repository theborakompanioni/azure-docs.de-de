Nachstehend finden Sie die Verwendungs- und andere Einschränkungen für den Azure Active Directory-Dienst.

| Kategorie | Grenzen |
| --- | --- |
| Verzeichnisse |Ein einzelner Benutzer kann maximal 20 Azure Active Directory-Verzeichnissen zugeordnet werden.<br />Beispiele für mögliche Kombinationen: <ul> <li>Ein einzelner Benutzer erstellt 20 Verzeichnisse.</li><li>Ein einzelner Benutzer wird 20 Verzeichnissen als Mitglied hinzugefügt.</li><li>Ein einzelner Benutzer erstellt 10 Verzeichnisse und wird später von anderen Benutzern 10 weiteren Verzeichnissen hinzugefügt.</li></ul> |
| Objekte |<ul><li>Im Tarif „Free“ von Azure Active Directory kann ein Verzeichnis mit Benutzern maximal 500.000 Objekte enthalten.</li><li>Ein Benutzer ohne Administratorrechte kann maximal 250 Objekte erstellen.</li></ul> |
| Schemaerweiterungen |<ul><li>Erweiterungen des Typs "String" sind auf maximal 256 Zeichen begrenzt. </li><li>Erweiterungen des Typs "Binary" sind auf 256 Bytes beschränkt.</li><li>100 Erweiterungswerte (für ALLE Typen und ALLE Anwendungen) können in jedes einzelne Objekt geschrieben werden.</li><li>Nur die Entitäten „User“, „Group“, „TenantDetail“, „Device“, „Application“ und „ServicePrincipal“ mit dem Typ „String“ oder „Binary“ können mit Einzelwertattributen erweitert werden.</li><li>Schemaerweiterungen sind nur in der Graph-API-Version "1.21-preview" verfügbar. Der Anwendung muss Schreibzugriff zum Registrieren einer Erweiterung gewährt werden.</li></ul> |
| Anwendungen |Maximal 10 Benutzer können Besitzer einer einzelnen Anwendung sein. |
| Gruppen |<ul><li>Maximal 10 Benutzer können Besitzer einer einzelnen Gruppe sein.</li><li>Eine beliebige Anzahl von Objekten kann einer einzelnen Gruppe in Azure Active Directory angehören.</li><li>Die Anzahl von Mitgliedern einer Gruppe, die Sie über Ihre lokale Active Directory-Instanz mit Azure Active Directory synchronisieren können, ist auf 15.000 beschränkt (bei Verwendung von Azure Active Directory-Verzeichnissynchronisierung (DirSync)).</li><li>Die Anzahl von Mitgliedern einer Gruppe, die Sie über Ihre lokale Active Directory-Instanz mit Azure Active Directory synchronisieren können, ist auf 50.000 beschränkt (bei Verwendung von Azure AD Connect).</li></ul> |
| Anpassung des Zugriffsbereichs |<ul><li>Für Benutzer mit Lizenzen für Azure AD Premium oder Enterprise Mobility Suite gibt es keine Beschränkung der Anzahl von Anwendungen, die pro Endbenutzer im Zugriffsbereich angezeigt werden.</li><li>Für Benutzer mit Lizenzen für die Edition „Free“ oder „Azure AD Basic“ von Azure Active Directory werden maximal zehn App-Kacheln (z.B. „Box“, „Salesforce“ oder „Dropbox“) im Zugriffsbereich angezeigt. Diese Beschränkung gilt nicht für Administratorkonten.</li></ul> |
| Berichte |In einem Bericht können maximal 1.000 Zeilen angezeigt oder heruntergeladen werden. Weitere Daten werden abgeschnitten. |



<!--HONumber=Nov16_HO3-->


