[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Verwenden Sie zum Registrieren der Webanwendung die in der Tabelle angegebenen Einstellungen.

![Beispielregistrierungseinstellungen für neue Web-App](./media/active-directory-b2c-register-web-app/b2c-new-app-settings.png)

| Einstellung      | Beispielwert  | Beschreibung                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Name** | Contoso B2C-App | Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. | 
| **Web-App/Web-API einschließen** | Ja | Wählen Sie **Ja** für eine Webanwendung. |
| **Impliziten Fluss zulassen** | Ja | Wählen Sie **Ja**, wenn Ihre Anwendung [OpenID Connect-Anmeldung](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) verwendet. |
| **Antwort-URL** | `https://localhost:44316` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie [eine passende](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **Antwort-URL** ein. In diesem Beispiel ist Ihre App lokal und lauscht an Port 44316. |

Klicken Sie auf **Erstellen** , um Ihre Anwendung zu registrieren.

Die neu registrierte Anwendung wird in der Anwendungsliste für den B2C-Mandanten angezeigt. Wählen Sie Ihre Web-App aus der Liste aus. Der Eigenschaftenbereich für die Webanwendung wird angezeigt.

![Web-App-Eigenschaften](./media/active-directory-b2c-register-web-app/b2c-web-app-properties.png)

Notieren Sie sich die global eindeutige **Anwendungsclient-ID**. Die ID wird im Code Ihrer Anwendung verwendet.