Um eine differenzierte Kennwortrücksetzung in Ihrer Anwendung zu aktivieren, müssen Sie eine Richtlinie zur Kennwortrücksetzung erstellen. Beachten Sie, dass die Option zur mandantenweiten Kennwortzurücksetzung [hier](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md) angegeben ist. Diese Richtlinie beschreibt die Benutzeroberflächen, die die Kunden während der Kennwortrücksetzung durchlaufen, und die Inhalte der Token, die die Anwendung beim erfolgreichen Abschluss erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie im Richtlinienabschnitt der Einstellungen die Option **Richtlinien für die Kennwortzurücksetzung**, und klicken Sie auf **+ Hinzufügen**.

![Registrierungs- oder Anmelderichtlinien auswählen und auf die Schaltfläche „Hinzufügen“ klicken](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Geben Sie einen Richtliniennamen**** ein, auf den Ihre App verweisen kann. Geben Sie z. B. Folgendes ein: `SSPR`.

Klicken Sie auf **Identitätsanbieter**, und aktivieren Sie **Kennwort mittels E-Mail-Adresse zurücksetzen**. Klicken Sie auf **OK**.

![„Kennwort mittels E-Mail-Adresse zurücksetzen“ als Identitätsanbieter auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Wählen Sie **Anwendungsansprüche** aus. Wählen Sie die Ansprüche aus, die in den zurückgegebenen Autorisierungstoken nach einer erfolgreichen Kennwortzurücksetzung an die Anwendung zurückgegeben werden sollen. Wählen Sie beispielsweise **Objekt-ID des Benutzers**.

![Einige Anwendungsansprüche auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Klicken Sie auf **Erstellen**, um die Richtlinie hinzuzufügen. Die Richtlinie wird als **B2C_1_SSPR** aufgeführt. Das Präfix **B2C_1_** wird an den Namen angehängt.

Öffnen Sie die Richtlinie, indem Sie **B2C_1_SSPR** auswählen. Überprüfen Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Jetzt ausführen**.

![Richtlinie auswählen und ausführen](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Einstellung      | Wert  |
| ------------ | ------ |
| **Anwendungen** | Contoso B2C-App |
| **Antwort-URL auswählen** | `https://localhost:44316/` |

Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Kennwortzurücksetzung in Ihrer Anwendung überprüfen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
>
