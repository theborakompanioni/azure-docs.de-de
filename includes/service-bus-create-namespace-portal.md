1. Melden Sie sich beim klassischen Azure-Portal an.

2. Klicken Sie im linken Navigationsbereich des Portals auf **Service Bus**.

3. Klicken Sie im unteren Bereich des Portals auf **Erstellen**.

    ![„Erstellen“ wählen][select-create]
   
4. Geben Sie im Dialogfeld **Add a new namespace** einen Namen für den Namespace ein. Das System überprüft sofort, ob dieser Name verfügbar ist.

    ![Namespacename][namespace-name]
  
5. Nachdem Sie sichergestellt haben, dass der Namespacename verfügbar ist, wählen Sie das Land oder die Region aus, in dem bzw. der Ihr Namespace gehostet werden soll.

6. Übernehmen Sie für die weiteren Felder im Dialogfeld die Standardwerte (**Messaging** und **Standardstufe**), und klicken Sie anschließend auf das Häkchen für "OK". Ihr Dienstnamespace wird nun erstellt und aktiviert. Ggf. müssen Sie einige Minuten warten, bis die Ressourcen für Ihr Konto durch das System bereitgestellt werden.
 
    ![Erfolgreich erstellt][created-successfully]

###Abrufen der Anmeldeinformationen
1. Klicken Sie im linken Navigationsbereich auf den Knoten **Servicebus**, um die Liste verfügbarer Namespaces anzuzeigen:
 
    ![„Service Bus“ wählen][select-service-bus]
  
2. Wählen Sie in der angezeigten Liste den Namespace, den Sie gerade erstellt haben:
 
    ![„Namespace“ wählen][select-namespace]
 
3. Klicken Sie auf **Verbindungsinformationen**.

    ![Verbindungsinformationen][connection-information]
  
4. Im Bereich **Zugriff auf die Verbindungsinformationen** finden Sie die Verbindungszeichenfolge, die den SAS-Schlüssel und den Schlüsselnamen enthält.

    ![Auf Verbindungsinformationen zugreifen][access-connection-information]
  
5. Notieren Sie den Schlüssel oder kopieren Sie ihn in die Zwischenablage.

<!--Image references-->

[select-create]: ./media/service-bus-create-namespace-portal/select-create.png
[namespace-name]: ./media/service-bus-create-namespace-portal/namespace-name.png
[created-successfully]: ./media/service-bus-create-namespace-portal/created-successfully.png
[select-service-bus]: ./media/service-bus-create-namespace-portal/select-service-bus.png
[select-namespace]: ./media/service-bus-create-namespace-portal/select-namespace.png
[connection-information]: ./media/service-bus-create-namespace-portal/connection-information.png
[access-connection-information]: ./media/service-bus-create-namespace-portal/access-connection-information.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[classic-portal]: https://manage.windowsazure.com

<!---HONumber=AcomDC_0615_2016-->