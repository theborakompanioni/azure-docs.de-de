## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

In diesem Abschnitt wird die Einrichtung Ihrer Entwicklungsumgebung erläutert. Diese umfasst das Erstellen einer ASP.NET MVC-App, das Hinzufügen einer Verbindung mit verbundenen Diensten, das Hinzufügen eines Controllers und das Angeben der erforderlichen Namespace-Direktiven.

### <a name="create-an-aspnet-mvc-app-project"></a>Erstellen einer ASP.NET MVC-App

1. Öffnen Sie Visual Studio.

1. Wählen Sie im Hauptmenü **Datei->Neu->Projekt**.

1. Legen Sie im Dialogfeld **Neues Projekt** die in der folgenden Abbildung hervorgehobenen Optionen fest:

    ![Erstellen eines ASP.NET-Projekts](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. Klicken Sie auf **OK**.

1. Legen Sie im Dialogfeld **Neues ASP.NET-Projekt** die in der folgenden Abbildung hervorgehobenen Optionen fest:

    ![Angeben von MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. Klicken Sie auf **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einem Azure-Speicherkonto mithilfe von Verbundene Dienste

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **Hinzufügen->Verbundener Dienst** aus.

1. Wählen Sie im Dialogfeld **Verbundenen Dienst hinzufügen** die Option **Azure Storage** aus, und klicken Sie dann auf die Schaltfläche **Konfigurieren**.

    ![Dialogfeld „Verbundener Dienst“](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Wählen Sie im Dialogfeld **Azure Storage** das gewünschte Azure-Speicherkonto aus, mit dem Sie arbeiten möchten, und wählen Sie **Hinzufügen**.
