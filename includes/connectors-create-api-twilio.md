### Voraussetzungen
- Ein Twilio-Konto
- Eine verifizierte Twilio-Telefonnummer, die SMS empfangen kann
- Eine verifizierte Twilio-Telefonnummer, die SMS senden kann

>[AZURE.NOTE] Wenn Sie ein Twilio-Testkonto verwenden, können Sie SMS nur an **verifizierte** Telefonnummern senden.

Bevor Sie Ihr Twilio-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für Verbindungen mit Ihrem Twilio-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem Twilio-Konto:

1. Um eine Verbindung mit Twilio herzustellen, wählen Sie im Logik-App-Designer in der Dropdownliste **Verwaltete Microsoft-APIs anzeigen** aus, und geben Sie dann *Twilio* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die Aktion aus: ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Wenn Sie noch keine Verbindungen mit Twilio hergestellt haben, werden Sie aufgefordert, Ihre Twilio-Anmeldeinformationen anzugeben. Mithilfe dieser Anmeldeinformationen wird Ihre Logik-App autorisiert, die eine Verbindung herstellen und auf Ihr Twilio-Konto zugreifen soll: ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Sie benötigen die **Twilio-Konto-ID** und das **Twilio-Zugriffstoken** vom Dashboard in Twilio. Melden Sie sich daher jetzt bei Ihrem Twilio-Konto an, und notieren Sie sich diese beiden Daten: ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio und Logik-Apps verwenden unterschiedliche Bezeichnungen für diese beiden Angaben. So ordnen Sie die Daten im Logik-Apps-Dialogfeld richtig zu ![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Klicken Sie auf die Schaltfläche **Verbindung erstellen**: ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit anderen Schritten in Ihrer Logik-App fortfahren können: ![](./media/connectors-create-api-twilio/twilio-5.png)