
Im vorhergehenden Beispiel wurde eine Standardanmeldung gezeigt, bei der der Client bei jedem Start der App sowohl den Identitätsanbieter als auch den Azure-Back-End-Dienst kontaktieren muss. Diese Methode ist allerdings ineffizient und kann zu Problemen bei der Nutzung führen, wenn viele Kunden die App gleichzeitig starten möchten. Daher empfiehlt es sich, das vom Azure-Dienst zurückgegebene Authentifizierungstoken zwischenzuspeichern und vor einer anbieterbasierten Anmeldung zu verwenden.

> [!NOTE]
> Für die Zwischenspeicherung des vom Azure-Back-End-Dienst zurückgegebenen Authentifizierungstokens spielt es keine Rolle, ob Sie die clientverwaltete oder die dienstverwaltete Authentifizierung verwenden. In diesem Lernprogramm wird die dienstverwaltete Authentifizierung verwendet.
>
>

1. Öffnen Sie die Datei "ToDoActivity.java", und fügen Sie die folgenden import-Anweisungen ein:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Fügen Sie die folgenden Mitglieder zur `ToDoActivity` -Klasse hinzu:

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. Fügen Sie der Datei „ToDoActivity.java“ die folgende Definition für die `cacheUserToken`-Methode hinzu.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Diese Methode speichert die Benutzer-ID und das Token in einer als privat gekennzeichneten Einstellungsdatei. Dadurch wird der Zugriff auf den Cache geschützt, sodass andere Apps auf dem Gerät nicht auf das Token zugreifen können. Die Einstellung ist für die App in einer Sandbox isoliert. Trotzdem ist es möglich, dass jemand, der Zugriff auf das Gerät erlangt hat, auf andere Weise auf den Tokencache zugreift.

   > [!NOTE]
   > Falls es sich um besonders sensible Daten handelt und die Gefahr besteht, dass andere Benutzer auf das Gerät zugreifen, können Sie das Token zu seinem Schutz zusätzlich verschlüsseln. Eine absolut sichere Lösung geht allerdings über den Rahmen dieses Tutorial hinaus und hängt zudem von Ihren Sicherheitsanforderungen ab.
   >
   >
4. Fügen Sie der Datei „ToDoActivity.java“ die folgende Definition für die `loadUserTokenCache`-Methode hinzu.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. Ersetzen Sie in der Datei *ToDoActivity.java* die `authenticate`-Methode durch die folgende Methode, die einen Tokencache verwendet. Wechseln Sie den Login-Anbieter, wenn Sie ein anderes Konto als Google verwenden möchten.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. Erstellen Sie die App und testen Sie die Authentifizierung mit einem gültigen Konto. Führen Sie sie mindestens zweimal aus. Bei der ersten Ausführung sollten Sie eine Aufforderung zur Anmeldung und Erstellung des Tokencaches erhalten. Danach wird bei jeder Ausführung versucht, den Tokencache für die Authentifizierung zu laden. Nun sollte keine Anmeldung mehr erforderlich sein.
