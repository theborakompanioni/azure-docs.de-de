1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

1. Klicken Sie auf **Compute** > **Function App** (Rechnen -> Funktionen-App), und wählen Sie **Abonnement** aus. Verwenden Sie dann die in der Tabelle angegebenen Einstellungen für die Funktionen-App.

    ![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App-Name** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet | 
    | **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird | 
    | **[Hostingplan](../articles/azure-functions/functions-scale.md)** |   Verbrauchsplan | Der Hostingplan, der definiert, wie Ihre Ressourcen der Funktionen-App zugewiesen werden Im **Standard-Verbrauchstarif** werden Ressourcen je nach Bedarf der Funktionen dynamisch hinzugefügt. Sie bezahlen nur die Zeit, in der Ihre Funktionen ausgeführt werden.   |
    | **Standort** | Westeuropa | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |
    | **[Speicherkonto](../articles/storage/storage-create-storage-account.md#create-a-storage-account)** |  Global eindeutiger Name |  Der Name des neuen Speicherkontos, das von Ihrer Funktionen-App verwendet wird. Sie können außerdem ein vorhandenes Konto verwenden. |

1. Klicken Sie auf **Erstellen** , um die neue Funktionen-App bereitzustellen.