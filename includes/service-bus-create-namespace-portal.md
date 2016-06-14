1. Melden Sie sich beim klassischen Azure-Portal an.

2. Klicken Sie im linken Navigationsbereich des Portals auf **Service Bus**.

3. Klicken Sie im unteren Bereich des Portals auf **Erstellen**.

    ![„Erstellen“ wählen][1]
   
4. Geben Sie im Dialogfeld **Add a new namespace** einen Namen für den Namespace ein. Das System überprüft sofort, ob dieser Name verfügbar ist.

    ![Namespacename][2]
  
5. Nachdem Sie sichergestellt haben, dass der Namespacename verfügbar ist, wählen Sie das Land oder die Region aus, in dem bzw. der Ihr Namespace gehostet werden soll.

6. Übernehmen Sie für die weiteren Felder im Dialogfeld die Standardwerte (**Messaging** und **Standardstufe**), und klicken Sie anschließend auf das Häkchen für "OK". Ihr Dienstnamespace wird nun erstellt und aktiviert. Ggf. müssen Sie einige Minuten warten, bis die Ressourcen für Ihr Konto durch das System bereitgestellt werden.
 
    ![Erfolgreich erstellt][3]

###Abrufen der Anmeldeinformationen
1. Klicken Sie im linken Navigationsbereich auf den Knoten **Servicebus**, um die Liste verfügbarer Namespaces anzuzeigen:
 
    ![„Service Bus“ wählen][4]
  
2. Wählen Sie in der angezeigten Liste den Namespace, den Sie gerade erstellt haben:
 
    ![„Namespace“ wählen][5]
 
3. Klicken Sie auf **Verbindungsinformationen**.

    ![Verbindungsinformationen][6]
  
4. Im Bereich **Zugriff auf die Verbindungsinformationen** finden Sie die Verbindungszeichenfolge, die den SAS-Schlüssel und den Schlüsselnamen enthält.

    ![Auf Verbindungsinformationen zugreifen][7]
  
5. Notieren Sie den Schlüssel oder kopieren Sie ihn in die Zwischenablage.

<!--Image references-->

[1]: ./media/service-bus-create-namespace-portal/select-create.png
[2]: ./media/service-bus-create-namespace-portal/namespace-name.png
[3]: ./media/service-bus-create-namespace-portal/created-successfully.png
[4]: ./media/service-bus-create-namespace-portal/select-service-bus.png
[5]: ./media/service-bus-create-namespace-portal/select-namespace.png
[6]: ./media/service-bus-create-namespace-portal/connection-information.png
[7]: ./media/service-bus-create-namespace-portal/access-connection-information.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[classic-portal]: https://manage.windowsazure.com

<!---HONumber=AcomDC_0608_2016-->