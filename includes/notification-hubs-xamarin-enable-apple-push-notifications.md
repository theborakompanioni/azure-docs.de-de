
Um Ihre App für Pushbenachrichtigungen über den Apple Push Notification Service (APNS) zu registrieren, müssen Sie ein Pushzertifikat, eine neue App-ID und ein neues Bereitstellungsprofil für das Projekt im Apple-Entwicklerportal erstellen.

Die App-ID enthält die Konfigurationsinformationen, die es Ihrer App ermöglichen, Pushbenachrichtigungen zu senden und zu empfangen. Diese Einstellungen enthalten das Pushbenachrichtigungszertifikat, das für die Authentifizierung beim APNS (Apple Push Notification Service) erforderlich ist, wenn die App Pushbenachrichtigungen sendet und empfängt.

Weitere Informationen zu diesen Konzepten finden Sie in der offiziellen Dokumentation zum [Apple Push Notification Service](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

## <a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Generieren der Zertifikatsignieranforderungsdatei für das Pushzertifikat
Mit diesen Schritten wird die Erstellung der Zertifikatsignieranforderung beschrieben. Es wird ein Pushzertifikat generiert, das mit APNS verwendet wird.

1. Führen Sie auf Ihrem Mac das Tool "Schlüsselbundverwaltung" aus. Es kann im Ordner **Dienstprogramme** oder im Ordner **Andere** auf dem Launchpad geöffnet werden.

2. Wählen Sie die Option **Schlüsselbundverwaltung**, erweitern Sie **Zertifikatsassistent**, und wählen Sie **Zertifikat einer Zertifizierungsinstanz anfordern...**.

      ![Anfordern eines Zertifikats](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Wählen Sie Ihre **E-Mail-Adresse des Benutzers** und den **Allgemeinen Namen** aus.

4. Stellen Sie sicher, dass **Saved to disk** (Auf Datenträger gespeichert) ausgewählt ist, und wählen Sie dann **Weiter**. Lassen Sie das Feld **E-Mail der Zert.-Instanz** leer, da es nicht erforderlich ist.

      ![Zertifikatinformationen](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Geben Sie unter **Speichern unter** einen Namen für die Zertifikatsignieranforderung-Datei (Certificate Signing Request, CSR) ein.
5. Wählen Sie unter **Wo** den Standort aus, und wählen Sie anschließend die Option **Speichern**.

      ![Speichern der Zertifikatsignieranforderung](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

      Dadurch wird die CSR-Datei am ausgewählten Speicherort gespeichert. (Der Standardspeicherort ist Ihr Desktop.) Merken Sie sich den für diese Datei gewählten Speicherort.

## <a name="register-your-app-for-push-notifications"></a>Registrieren der App für Pushbenachrichtigungen
Erstellen Sie eine neue explizite App-ID für Ihre Anwendung bei Apple, und konfigurieren Sie sie anschließend für Pushbenachrichtigungen.  

1. Navigieren Sie zum [iOS-Bereitstellungsportal](http://go.microsoft.com/fwlink/p/?LinkId=272456) im Apple Developer Center, und melden Sie sich mit Ihrer Apple-ID an.

2. Wählen Sie **Bezeichner** und dann **App-IDs**.

3. Wählen Sie das Pluszeichen (**+**), um eine neue App zu registrieren.

      ![Registrieren einer neuen App](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

4. Aktualisieren Sie die folgenden drei Felder für Ihre neue App, und wählen Sie dann **Weiter**:

   * **Name**: Geben Sie im Abschnitt **App ID Description** einen aussagekräftigen Namen für Ihre App ein.

   * **Bundle ID**: Geben Sie im Abschnitt **Explicit App ID** eine **Bundle ID** im Format `<Organization Identifier>.<Product Name>` (wie im [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8) beschrieben) ein. Dieser muss mit den Angaben übereinstimmen, die im XCode-, Xamarin- oder Cordova-Projekt für Ihre App verwendet werden.

   * **Pushbenachrichtigungen**: Aktivieren Sie die Option **Push Notifications** im Abschnitt **App Services**.

     ![Registrieren der App-ID](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

5. Überprüfen Sie auf dem Bildschirm **Confirm your App ID** die Einstellungen. Wählen Sie anschließend die Option **Register**.

6. Nach dem Übermitteln der neuen App-ID wird der Bildschirm **Registration complete** angezeigt. Wählen Sie **Fertig**aus.

7. Suchen Sie im Developer Center unter **App IDs** nach der erstellten App-ID, und klicken Sie in die entsprechende Zeile. Durch das Auswählen der Zeile mit der App-ID werden Einzelheiten zur App angezeigt. Wählen Sie unten auf dem Bildschirm anschließend die Option **Bearbeiten**.

8. Scrollen Sie bis zum Ende des Bildschirms, und wählen Sie dann die Schaltfläche **Create Certificate...** unter dem Abschnitt **Development SSL Certificate**.

      ![Development SSL Certificate – Push](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

 Hierdurch wird der Assistent "Add iOS Certificate" zur Anzeige gebracht.

   > [!NOTE]
   > In diesem Lernprogramm wird ein Entwicklungszertifikat verwendet. Sie verwenden den gleichen Prozess, wenn Sie ein Zertifikat für die Produktion registrieren. Achten Sie darauf, dass Sie denselben Zertifikattyp beim Senden von Benachrichtigungen verwenden.
   >

9. Wählen Sie die Option **Choose File**, und navigieren Sie dann zum Speicherort der CSR-Datei für Ihr Pushzertifikat. Wählen Sie anschließend die Option **Generate**.

      ![Generieren eines Zertifikats](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

10. Wählen Sie nach der Erstellung des Zertifikats im Portal die Schaltfläche **Download**.

      ![Zertifikat bereit](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

       Hierdurch wird das Signierzertifikat heruntergeladen und auf Ihrem Computer in Ihrem Download-Ordner gespeichert.

      ![Downloadordner](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

   > [!NOTE]
   > Standardmäßig ist die heruntergeladene Datei ein Entwicklungszertifikat mit dem Namen **aps_development.cer**.
   >
   >
11. Doppelklicken Sie auf dem heruntergeladenen Pushzertifikat **aps_development.cer**. Das neue Zertifikat wird im Schlüsselbund installiert, wie im folgenden Screenshot dargestellt:

       ![Schlüsselbundverwaltung (Keychain Access)](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

   > [!NOTE]
   > Der Name in Ihrem Zertifikat kann sich davon unterscheiden, aber er enthält das Präfix **Apple Development iOS Push Services**.
   >
   >
12. Wählen Sie in der **Schlüsselbundverwaltung** das neue Pushzertifikat aus, das Sie in der Kategorie **Zertifikate** erstellt haben. Wählen Sie die Option **Exportieren**, benennen Sie die Datei, wählen Sie das Format **.p12** aus, und wählen Sie dann die Option **Speichern**.

    Notieren Sie sich den Dateinamen und den Speicherort des exportierten P12-Zertifikats. Sie verwenden diese Angaben zum Aktivieren der Authentifizierung mit APNS, indem Sie sie im klassischen Azure-Portal hochladen. Wenn die Formatoption **.p12** nicht verfügbar ist, müssen Sie die Schlüsselbundverwaltung unter Umständen neu starten.

## <a name="create-a-provisioning-profile-for-the-app"></a>Erstellen eines Bereitstellungsprofils für die App
1. Zurück im <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> wählen Sie **Provisioning Profiles** und dann **All** und die Schaltfläche **+**, um ein Profil zu erstellen. Das Profiltool **Add iOS Provisioning Profile** wird gestartet.

      ![Bereitstellungsprofile](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Wählen Sie unter **Development**, die Option **iOS App Development** als Bereitstellungsprofiltyp aus, und wählen Sie anschließend **Continue**.

3. Wählen Sie in der Dropdownliste **App ID** die von Ihnen erstellte App-ID aus, und wählen Sie **Continue**.

      ![Auswählen der App-ID](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

4. Wählen Sie auf dem Bildschirm **Select certificates** das Entwicklungszertifikat aus, das Sie für die Codesignierung verwenden, und wählen Sie anschließend **Continue**. Dabei handelt es sich um ein Signaturzertifikat, nicht um das erstellte Pushzertifikat.

       ![Signing certificate](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

5. Wählen Sie die Geräte (**Devices**) aus, die zum Testen verwendet werden sollen, und wählen Sie dann **Continue**.

     ![Hinzufügen eines Bereitstellungsprofils](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

6. Wählen Sie abschließend unter **Profile Name** einen Namen für das Profil aus, und wählen Sie dann die Option **Generate**.

      ![Benennen des Profils](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
