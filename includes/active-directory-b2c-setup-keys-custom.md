> [!NOTE]
> Eine Optimierung dieser Umgebung ist in Planung. Dabei sollen folgende Schritte wegfallen:

### <a name="create-an-administrator-credential-in-the-azure-ad-b2c-tenant"></a>Erstellen Sie Administratoranmeldeinformationen im Azure AD B2C-Mandanten.

Für den nächsten Abschnitt müssen Sie Anmeldeinformationen verwenden, die die Domäne des Azure AD B2C-Mandanten nutzt. Erstellen Sie zu diesem Zweck ein Administratorkonto mit solchen Anmeldeinformationen. Gehen Sie dazu wie folgt vor:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den Kontext Ihres Azure AD B2C-Mandanten, und öffnen Sie das Blatt für Azure AD B2C. [So wird‘s gemacht.](..\articles\active-directory-b2c\active-directory-b2c-navigate-to-b2c-context.md)
1. Wählen Sie **Benutzer und Gruppen**.
1. Wählen Sie **Alle Benutzer**.
1. Klicken Sie auf **+ Neuer Benutzer**.
    * Legen Sie **Name** = `Admin` fest.
    * Legen Sie **Benutzername** = `admin@{tenantName}.onmicrosoft.com` fest. Dabei ist `{tenantName}` der Name Ihres Azure AD B2C-Mandanten.
1. Wählen Sie unter **Verzeichnisrolle** die Option **Globaler Administrator** und anschließend **OK**.
1. Klicken Sie auf **Erstellen**, um den Administratorbenutzer zu erstellen.
1. Aktivieren Sie **Kennwort anzeigen**, und kopieren Sie das Kennwort.

### <a name="set-up-the-key-container"></a>Einrichten des Schlüsselcontainers

Der Schlüsselcontainer dient zum Speichern von Schlüsseln. So richten Sie einen Schlüsselcontainer ein:

1. Öffnen Sie eine neue PowerShell-Eingabeaufforderung.  Eine Methode: **Windows-Logo-Taste+R** drücken, `powershell` eingeben und die EINGABETASTE drücken.
1. Laden Sie diese Reproduktion herunter, um das ExploreAdmin-Tool von PowerShell abzurufen.

    ```powershell
    git clone https://github.com/Azure-Samples/active-directory-b2c-advanced-policies
    ```

1. Wechseln Sie in den Ordner mit dem ExploreAdmin-Tool.

    ```powershell
    cd active-directory-b2c-advanced-policies\ExploreAdmin
    ```

1. Importieren Sie das ExploreAdmin-Tool in PowerShell.

    ```powershell
    Import-Module .\ExploreAdmin.dll
    ```

1. Vergewissern Sie sich, dass `b2c_1a_TokenSigningKeyContainer` noch nicht vorhanden ist.  Ersetzen Sie `{tenantName}` durch den Namen Ihres Mandanten.

    ```powershell
    Get-CpimKeyContainer -TenantId {tenantName}.onmicrosoft.com -StorageReferenceId b2c_1a_TokenSigningKeyContainer -ForceAuthenticationPrompt
    ```

    a. Wenn die Anmeldeaufforderung angezeigt wird, verwenden Sie das im vorherigen Abschnitt erstellte Administratorkonto.

    b. Geben Sie bei entsprechender Aufforderung Ihre Telefonnummer ein, um die mehrstufige Authentifizierung einzurichten.

    c. Ändern Sie Ihr Kennwort, wenn Sie dazu aufgefordert werden.

    d. **Ein Fehler wird erwartet.**  Im Fehler sollte angegeben sein, dass `b2c_1a_TokenSigningKeyContainer` nicht gefunden wurde.  Falls kein Fehler auftritt, weil Sie diese Schritte bereits ausgeführt haben, überspringen Sie den restlichen Teil des Abschnitts.

1. Erstellen Sie `b2c_1a_TokenSigningKeyContainer`.  Ersetzen Sie `{tenantName}` durch den Namen Ihres Mandanten.

    ```powershell
    New-CpimKeyContainer {tenantName}.onmicrosoft.com  b2c_1a_TokenSigningKeyContainer  b2c_1a_TokenSigningKeyContainer rsa 2048 0 0
    ```

1. Erstellen Sie `b2c_1a_TokenEncryptionKeyContainer`.  Ersetzen Sie `{tenantName}` durch den Namen Ihres Mandanten.

    ```powershell
    New-CpimKeyContainer {tenantName}.onmicrosoft.com b2c_1a_TokenEncryptionKeyContainer b2c_1a_TokenEncryptionKeyContainer rsa 2048 0 0
    ```

1. Erstellen Sie `b2c_1a_FacebookSecret`.  Ersetzen Sie `{tenantName}` durch den Namen Ihres Mandanten.

    ```powershell
    New-CpimKeyContainer {tenantName}.onmicrosoft.com  b2c_1a_FacebookSecret  b2c_1a_FacebookSecret rsa 2048 0 0
    ```
