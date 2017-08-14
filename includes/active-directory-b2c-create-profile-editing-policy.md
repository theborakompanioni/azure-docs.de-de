Um die Profilbearbeitung in Ihrer Anwendung zu ermöglichen, müssen Sie eine Richtlinie für die Profilbearbeitung erstellen. Diese Richtlinie beschreibt die Benutzeroberflächen, die Consumer bei der Profilbearbeitung durchgehen, und den Inhalt der Token, die die Anwendung beim erfolgreichen Abschluss erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie im Richtlinienabschnitt der Einstellungen die Option **Richtlinien zur Profilbearbeitung** aus, und klicken Sie auf **+ Hinzufügen**.

![„Richtlinien zur Profilbearbeitung“ auswählen und auf die Schaltfläche „Hinzufügen“ klicken](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

Geben Sie einen Richtliniennamen**** an, auf den Ihre App verweisen kann. Geben Sie z. B. Folgendes ein: `SiPe`.

Klicken Sie auf **Identitätsanbieter**, und aktivieren Sie das Kontrollkästchen **Anmeldung für lokales Konto**. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.

![„Anmeldung für lokales Konto“ als Identitätsanbieter auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

Wählen Sie **Profilattribute** aus. Wählen Sie Attribute aus, die der Kunde in seinem Profil anzeigen und bearbeiten kann. Wählen Sie beispielsweise **Land/Region**, **Anzeigename** und **Postleitzahl** aus. Klicken Sie auf **OK**.

![Einige Attribute auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

Wählen Sie **Anwendungsansprüche** aus. Wählen Sie die Ansprüche aus, die nach einer erfolgreichen Profilbearbeitung in den an die Anwendung gesendeten Autorisierungstoken zurückgegeben werden sollen. Wählen Sie beispielsweise **Anzeigename** und **Postleitzahl** aus.

![Einige Anwendungsansprüche auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

Klicken Sie auf **Erstellen**, um die Richtlinie hinzuzufügen. Die Richtlinie wird als **B2C_1_SiPe** aufgeführt. Das Präfix **B2C_1_** wird an den Namen angehängt.

Öffnen Sie die Richtlinie, indem Sie **B2C_1_SiPe** auswählen. Überprüfen Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Jetzt ausführen**.

![Richtlinie auswählen und ausführen](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| Einstellung      | Wert  |
| ------------ | ------ |
| **Anwendungen** | Contoso B2C-App |
| **Antwort-URL auswählen** | `https://localhost:44316/` |

Eine neue Browserregisterkarte wird geöffnet, auf der Sie die Benutzeroberfläche für die konfigurierte Profilbearbeitung überprüfen können.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
>