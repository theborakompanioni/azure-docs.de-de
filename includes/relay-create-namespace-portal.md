1. Melden Sie sich beim [Azure-Portal][Azure-Portal] an.
2. Klicken Sie im linken Navigationsbereich des Portals auf **Neu**, dann auf **Enterprise Integration** und dann auf **Relay**.
3. Geben Sie im Dialogfeld **Namespace erstellen** einen Namen für den Namespace ein. Das System überprüft sofort, ob dieser Name verfügbar ist.
4. Wählen Sie im Feld **Abonnement** ein Azure-Abonnement aus, in dem der Namespace erstellt werden soll.
5. Wählen Sie im Feld **[Ressourcengruppe](../articles/azure-portal/resource-group-portal.md)** eine vorhandene Ressourcengruppe für den Namespace aus, oder erstellen Sie eine neue Ressourcengruppe.      
6. Wählen Sie im Feld **Standort** das Land oder die Region aus, in dem bzw. in der Ihr Namespace gehostet werden soll.
   
    ![Namespace erstellen][create-namespace]
7. Klicken Sie auf **Erstellen**. Ihr Dienstnamespace wird nun erstellt und aktiviert. Ggf. müssen Sie einige Minuten warten, bis die Ressourcen für Ihr Konto durch das System bereitgestellt werden.

### <a name="obtain-the-management-credentials"></a>Abrufen der Verwaltungsanmeldeinformationen
1. Klicken Sie in der Liste mit den Namespaces auf den neu erstellten Namespacenamen.
2. Klicken Sie auf dem Blatt „Namespace“ auf **Richtlinien für gemeinsamen Zugriff**.
3. Klicken Sie auf dem Blatt **Richtlinien für gemeinsamen Zugriff** auf **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. Klicken Sie auf dem Blatt **Richtlinie: RootManageSharedAccessKey** neben **Verbindungszeichenfolge – Primärschlüssel** auf die Kopierschaltfläche, um die Verbindungszeichenfolge zur späteren Verwendung in die Zwischenablage zu kopieren. Fügen Sie diesen Wert in den Editor oder an einem anderen temporären Speicherort ein.
   
    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure-Portal]: https://portal.azure.com


<!--HONumber=Nov16_HO2-->


