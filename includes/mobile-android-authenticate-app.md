
1. Öffnen Sie die Datei "ToDoActivity.java" im **Projektexplorer** in Android Studio, und fügen Sie die folgenden import-Anweisungen hinzu.
   
        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
   
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
2. Fügen Sie der **ToDoActivity** -Klasse die folgende Methode hinzu: 
   
        private void authenticate() {
            // Login using the Google provider.
   
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
   
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();    
                }
            });       
        }

    Diese neue Methode erledigt den Authentifizierungsprozess. Der Benutzer wird mithilfe einer Google-Anmeldung authentifiziert. Ein Dialogfeld mit der ID des authentifizierten Benutzers wird eingeblendet. Ohne erfolgreiche Authentifizierung können Sie nicht fortfahren.

    > [AZURE.NOTE] Falls Sie einen anderen Identitätsanbieter als Facebook verwenden, ändern Sie den an die **login**-Methode übergebenen Wert in einen der folgenden Werte: _MicrosoftAccount_, _Facebook_, _Twitter_ oder _windowsazureactivedirectory_.

1. Fügen Sie in der **onCreate**-Methode die folgende Codezeile im Anschluss an den Code hinzu, der das `MobileServiceClient`-Objekt instanziiert.
   
        authenticate();
   
    Dieser Aufruf startet den Authentifizierungsprozess.
2. Verschieben Sie den verbleibenden Code nach `authenticate();` in der **OnCreate**-Methode in eine neue **CreateTable**-Methode mit dem folgenden Code:
   
        private void createTable() {
   
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
   
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
   
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
   
            // Load the items from Azure.
            refreshItemsFromTable();
        }
3. Klicken Sie im Menü **Ausführen** auf **App ausführen**, um die App zu starten, und melden Sie sich mit dem Identitätsanbieter Ihrer Wahl an. 
   
       When you are successfully logged-in, the app should run without errors, and you should be able to query the backend service and make updates to data.



<!--HONumber=Nov16_HO3-->


