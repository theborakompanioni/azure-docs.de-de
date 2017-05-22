## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Vorbereiten der Authentifizierung von Anforderungen von Azure Resource Manager
Sie müssen alle Vorgänge authentifizieren, die Sie mithilfe von [Azure Resource Manager][lnk-authenticate-arm] mit Azure Active Directory (AD) für die Ressourcen ausführen. Die einfachste Möglichkeit hierzu ist die Verwendung von PowerShell oder der Azure-Befehlszeilenschnittstelle.

Installieren Sie die [Azure PowerShell-Cmdlets][lnk-powershell-install], bevor Sie den Vorgang fortsetzen.

Die folgenden Schritte zeigen, wie Sie die Kennwortauthentifizierung für eine AD-Anwendung mithilfe von PowerShell einrichten. Sie können diese Befehle in einer standardmäßigen PowerShell-Sitzung ausführen.

1. Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Abonnement an:

    ```powershell
    Login-AzureRmAccount
    ```

1. Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch die Anmeldung bei Azure Zugriff auf alle Azure-Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Führen Sie den folgenden Befehl aus, um eine Liste der Azure-Abonnements anzuzeigen, die Sie verwenden können:

    ```powershell
    Get-AzureRMSubscription
    ```

    Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, das Sie zum Ausführen der Befehle zum Verwalten Ihres IoT Hubs verwenden möchten. Sie können entweder den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Notieren Sie sich Ihre **TenantId** und Ihre **SubscriptionId**. Diese werden später benötigt.
3. Erstellen Sie mit dem folgenden Befehl eine neue Azure Active Directory-Anwendung, und ersetzen Sie die Platzhalter:
   
   * **{Display name}:** ein Anzeigename für Ihre Anwendung, z.B. **MySampleApp**.
   * **{Home page URL}:** Die URL der Startseite der App, z.B. **http://mysampleapp/home**. Diese URL muss nicht auf eine echte Anwendung verweisen.
   * **{Application identifier}:** Ein eindeutiger Bezeichner, z.B. **http://meinebeispielapp**. Diese URL muss nicht auf eine echte Anwendung verweisen.
   * **{Password}:** Ein Kennwort, das Sie zur Authentifizierung mit Ihrer App verwenden werden.
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. Notieren Sie sich die **ApplicationId** der Anwendung, die Sie erstellt haben. Sie benötigen sie später.
5. Erstellen Sie einen neuen Dienstprinzipal mithilfe des folgenden Befehls, und ersetzen Sie dabei **{MyApplicationId}** durch die **ApplicationId** aus dem vorherigen Schritt:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Richten Sie eine Rollenzuweisung mithilfe des folgenden Befehls ein, und ersetzen Sie dabei **{MyApplicationId}** durch Ihre **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Sie haben jetzt eine Azure AD-Anwendung fertig erstellt, mit der die Authentifizierung aus einer benutzerdefinierten C#-Anwendung möglich ist. Die folgenden Werte benötigen Sie später in diesem Tutorial:

* TenantId
* SubscriptionId
* ApplicationId
* Kennwort

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
