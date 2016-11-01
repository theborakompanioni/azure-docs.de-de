<properties
	pageTitle="Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs und Firebase Cloud Messaging | Microsoft Azure"
	description="In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Firebase Cloud Messaging Pushbenachrichtigungen an Android-Geräte senden."
	services="notification-hubs"
	documentationCenter="android"
	keywords="Pushbenachrichtigungen,Pushbenachrichtigung,Android-Pushbenachrichtigung,FCM,Firebase Cloud Messaging"
	authors="wesmc7777"
	manager="erikre"
	editor=""/>
<tags
	ms.service="notification-hubs"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="java"
	ms.topic="hero-article"
	ms.date="07/14/2016"
	ms.author="wesmc"/>

# Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs

[AZURE.INCLUDE [Notification-Hubs-Auswahl-Erste-Schritte](../../includes/notification-hubs-selector-get-started.md)]

##Übersicht

> [AZURE.IMPORTANT] In diesem Thema werden Pushbenachrichtigungen mit Google Firebase Cloud Messaging (FCM) veranschaulicht. Falls Sie noch Google Cloud Messaging (GCM) verwenden, helfen Ihnen die Informationen unter [Sending push notifications to Android with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md) (Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs und GCM) weiter.

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Firebase Cloud Messaging Pushbenachrichtigungen an eine Android-Anwendung senden. Sie erstellen eine leere Android-App, die Pushbenachrichtigungen mithilfe von Firebase Cloud Messaging (FCM) empfängt.



[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Den vollständigen Code für dieses Tutorial können Sie [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase) bei GitHub herunterladen.


##Voraussetzungen

> [AZURE.IMPORTANT] Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fde-DE%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

- Zusätzlich zum oben erwähnten aktiven Azure-Konto ist für dieses Tutorial die aktuelle Version von [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797) erforderlich.
- Android 2.3 oder höher für Firebase Cloud Messaging.
- Google Repository Version 27 oder höher ist für Firebase Cloud Messaging erforderlich.
- Google Play Services 9.0.2 oder höher für Firebase Cloud Messaging.
- Das Abschließen dieses Tutorial ist eine Voraussetzung für alle anderen Notification Hubs-Tutorials für Android-Apps.


##Erstellen eines neuen Android Studio-Projekts

1. Starten Sie in Android Studio ein neues Android Studio-Projekt.

   	![Android Studio – Neues Projekt](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)

2. Wählen Sie den Formfaktor **Phone and Tablet** und das **Minimum SDK** aus, das unterstützt werden soll. Klicken Sie auf **Next**.

   	![Android Studio – Projekterstellungsworkflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)

3. Wählen Sie **Empty Activity** als Hauptaktivität aus, und klicken Sie auf **Next** und anschließend auf **Finish**.


##Erstellen eines Projekts, das Firebase Cloud Messaging unterstützt

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]


##Konfigurieren eines neuen Notification Hubs

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. Wählen Sie auf dem Blatt **Einstellungen** in Ihrem Notification Hub die Option **Notification Services** und dann **Google (GCM)**. Geben Sie den FCM-Serverschlüssel ein, den Sie zuvor in der [Firebase-Konsole](https://firebase.google.com/console/) kopiert haben, und klicken Sie auf **Save**.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Der Notification Hub ist jetzt für die Arbeit mit Firebase Cloud Messaging konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App für den Empfang und das Senden von Pushbenachrichtigungen zu registrieren.

##<a id="connecting-app"></a>Verbinden Ihrer App mit dem Notification Hub

###Hinzufügen von Google Play Services zum Projekt

[AZURE.INCLUDE [Hinzufügen von Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###Hinzufügen von Azure Notification Hubs-Bibliotheken


1. Fügen Sie in der Datei `Build.Gradle` für die **App** die folgenden Zeilen im Abschnitt **dependencies** hinzu.

		compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
		compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Fügen Sie das folgende Repository nach dem Abschnitt **dependencies** hinzu.

		repositories {
		    maven {
		        url "http://dl.bintray.com/microsoftazuremobile/SDK"
		    }
		}

### Aktualisieren Sie die Datei „AndroidManifest.xml“.


1. Für die Unterstützung von FCM müssen wir in unserem Code einen Instanz-ID-Listenerdienst implementieren, der zum [Beschaffen von Registrierungstoken](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) mit der [FirebaseInstanceId-API von Google](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) verwendet wird. In diesem Tutorial geben wir der Klasse den Namen `MyInstanceIDService`.
 
	Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Dienstdefinition hinzu.

        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>



2. Nachdem wir unser FCM-Registrierungstoken von der FirebaseInstanceId-API erhalten haben, verwenden wir es zum [Registrieren beim Azure Notification Hub](notification-hubs-push-notification-registration-management.md). Wir unterstützen diese Registrierung im Hintergrund mit einem `IntentService`-Element namens `RegistrationIntentService`. Dieser Dienst ist auch für das Aktualisieren des FCM-Registrierungstokens zuständig.
 
	Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Dienstdefinition hinzu.

        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>



3. Wir definieren auch einen Empfänger für die Benachrichtigungen. Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Empfängerdefinition hinzu. Ersetzen Sie den Platzhalter `<your package>` durch den tatsächlichen Namen des Pakets, der oben in der Datei `AndroidManifest.xml` angegeben ist.

		<receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
		    android:permission="com.google.android.c2dm.permission.SEND">
		    <intent-filter>
		        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
		        <category android:name="<your package name>" />
		    </intent-filter>
		</receiver>



4. Fügen Sie unterhalb des Tags `</application>` die folgenden erforderlichen FCM-bezogenen Berechtigungen hinzu. Ersetzen Sie dabei `<your package>` durch den oben in der Datei `AndroidManifest.xml` angegebenen Paketnamen.

	Weitere Informationen zu diesen Berechtigungen finden Sie unter [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) (Einrichten einer GCM-Client-App für Android) und [Migrate a GCM Client App for Android to Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm) (Migrieren einer GCM-Client-App für Android zu Firebase Cloud Messaging).

		<uses-permission android:name="android.permission.INTERNET"/>
		<uses-permission android:name="android.permission.GET_ACCOUNTS"/>
		<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />


### Hinzufügen von Code


1. Erweitern Sie in der Projektansicht die Knoten **app** > **src** > **main** > **java**. Klicken Sie mit der rechten Maustaste auf Ihren Paketordner unter **java**, klicken Sie auf **Neu** und dann auf **Java Class**. Fügen Sie eine neue Klasse namens `NotificationSettings` hinzu.

	![Android Studio – Neue Java-Klasse](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

	Aktualisieren Sie die drei folgenden Platzhalter im weiter unten angegebenen Code für die `NotificationSettings`-Klasse:
	* **SenderId**: Dies ist die Absender-ID, die Sie zuvor in den Projekteinstellungen der [Firebase-Konsole](https://firebase.google.com/console/) über die Registerkarte **Cloud Messaging** ermittelt haben.
	* **HubListenConnectionString**: Dies ist die **DefaultListenAccessSignature**-Verbindungszeichenfolge für Ihren Hub. Sie können diese Verbindungszeichenfolge kopieren, indem Sie im [Azure-Portal] auf dem Blatt **Einstellungen** Ihres Hub auf **Zugriffsrichtlinien** klicken.
	* **HubName**: Verwenden Sie den Namen Ihres Notification Hub, der im [Azure-Portal] auf dem Hub-Blatt angezeigt wird.

	`NotificationSettings`-Code:

		public class NotificationSettings {
		    public static String SenderId = "<Your project number>";
		    public static String HubName = "<Your HubName>";
		    public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
		}

2. Fügen Sie mit den oben angegebenen Schritten eine weitere neue Klasse namens `MyInstanceIDService` hinzu. Das ist unsere Implementierung des Instanz-ID-Listenerdiensts.

	Mit dem Code für diese Klasse wird unser `IntentService`-Element aufgerufen, um im Hintergrund [das FCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).

		import android.content.Intent;
		import android.util.Log;
		import com.google.firebase.iid.FirebaseInstanceIdService;
		
		
		public class MyInstanceIDService extends FirebaseInstanceIdService {
		
		    private static final String TAG = "MyInstanceIDService";
		
		    @Override
		    public void onTokenRefresh() {
		
		        Log.d(TAG, "Refreshing GCM Registration Token");
		
		        Intent intent = new Intent(this, RegistrationIntentService.class);
		        startService(intent);
		    }
		};


3. Fügen Sie dem Projekt eine weitere neue Klasse namens `RegistrationIntentService` hinzu. Dies ist die Implementierung für unser `IntentService`-Element zum [Aktualisieren des FCM-Tokens](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) und [Registrieren beim Notification Hub](notification-hubs-push-notification-registration-management.md).

	Verwenden Sie für diese Klasse den folgenden Code.

		import android.app.IntentService;
		import android.content.Intent;
		import android.content.SharedPreferences;
		import android.preference.PreferenceManager;
		import android.util.Log;		
		import com.google.firebase.iid.FirebaseInstanceId;
		import com.microsoft.windowsazure.messaging.NotificationHub;
		
		public class RegistrationIntentService extends IntentService {
		
		    private static final String TAG = "RegIntentService";
		
		    private NotificationHub hub;
		
		    public RegistrationIntentService() {
		        super(TAG);
		    }
		
		    @Override
		    protected void onHandleIntent(Intent intent) {
		
		        SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
		        String resultString = null;
		        String regID = null;
		        String storedToken = null;
		
		        try {
		            String FCM_token = FirebaseInstanceId.getInstance().getToken();
		            Log.d(TAG, "FCM Registration Token: " + FCM_token);
		
		            // Storing the registration id that indicates whether the generated token has been
		            // sent to your server. If it is not stored, send the token to your server,
		            // otherwise your server should have already received the token.
		            if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
		
		                NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
		                        NotificationSettings.HubListenConnectionString, this);
		                Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
		                regID = hub.register(FCM_token).getRegistrationId();
		
		                // If you want to use tags...
		                // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
		                // regID = hub.register(token, "tag1,tag2").getRegistrationId();
		
		                resultString = "New NH Registration Successfully - RegId : " + regID;
		                Log.d(TAG, resultString);
		
		                sharedPreferences.edit().putString("registrationID", regID ).apply();
		                sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
		            }
		
		            // Check if the token may have been compromised and needs refreshing.
		            else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
		
		                NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
		                        NotificationSettings.HubListenConnectionString, this);
		                Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
		                regID = hub.register(FCM_token).getRegistrationId();
		
		                // If you want to use tags...
		                // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
		                // regID = hub.register(token, "tag1,tag2").getRegistrationId();
		
		                resultString = "New NH Registration Successfully - RegId : " + regID;
		                Log.d(TAG, resultString);
		
		                sharedPreferences.edit().putString("registrationID", regID ).apply();
		                sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
		            }
		
		            else {
		                resultString = "Previously Registered Successfully - RegId : " + regID;
		            }
		        } catch (Exception e) {
		            Log.e(TAG, resultString="Failed to complete registration", e);
		            // If an exception happens while fetching the new token or updating our registration data
		            // on a third-party server, this ensures that we'll attempt the update at a later time.
		        }
		
		        // Notify UI that registration has completed.
		        if (MainActivity.isVisible) {
		            MainActivity.mainActivity.ToastNotify(resultString);
		        }
		    }
		}


		
4. Fügen Sie in der `MainActivity`-Klasse oberhalb der Klassendeklaration die folgenden `import`-Anweisungen hinzu.

		import com.google.android.gms.common.ConnectionResult;
		import com.google.android.gms.common.GoogleApiAvailability;
		import com.microsoft.windowsazure.notifications.NotificationsManager;
		import android.content.Intent;
		import android.util.Log;
		import android.widget.TextView;
		import android.widget.Toast;

5. Fügen Sie folgende private Member oben in der Klasse hinzu. Diese verwenden wir zum [Überprüfen der Verfügbarkeit von Google Play Services gemäß Empfehlung von Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

	    public static MainActivity mainActivity;
    	public static Boolean isVisible = false;	
	    private static final String TAG = "MainActivity";
	    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. Fügen Sie in der `MainActivity`-Klasse die folgende Methode zur Verfügbarkeit von Google Play Services hinzu.

	    /**
	     * Check the device to make sure it has the Google Play Services APK. If
	     * it doesn't, display a dialog that allows users to download the APK from
	     * the Google Play Store or enable it in the device's system settings.
	     */
	    private boolean checkPlayServices() {
	        GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
	        int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
	        if (resultCode != ConnectionResult.SUCCESS) {
	            if (apiAvailability.isUserResolvableError(resultCode)) {
	                apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
	                        .show();
	            } else {
	                Log.i(TAG, "This device is not supported by Google Play Services.");
	                ToastNotify("This device is not supported by Google Play Services.");
	                finish();
	            }
	            return false;
	        }
	        return true;
	    }


7. Fügen Sie in der `MainActivity`-Klasse den folgenden Code hinzu, mit dem eine Überprüfung auf Google Play Services durchgeführt wird, bevor das `IntentService`-Element aufgerufen wird, um Ihr FCM-Registrierungstoken abzurufen und die Registrierung beim Notification Hub vorzunehmen.

	    public void registerWithNotificationHubs()
	    {
	        if (checkPlayServices()) {
	            // Start IntentService to register this application with FCM.
	            Intent intent = new Intent(this, RegistrationIntentService.class);
	            startService(intent);
	        }
	    }


8. Fügen Sie in der `OnCreate`-Methode der `MainActivity`-Klasse den folgenden Code hinzu, um den Registrierungsprozess zu starten, wenn die Aktivität erstellt wird.

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	
	        mainActivity = this;
	        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
	        registerWithNotificationHubs();
	    }


9. Fügen Sie diese zusätzlichen Methoden dem `MainActivity`-Element hinzu, um den App-Zustand zu überprüfen und den Status in Ihrer App zu melden.

	    @Override
	    protected void onStart() {
	        super.onStart();
	        isVisible = true;
	    }
	
	    @Override
	    protected void onPause() {
	        super.onPause();
	        isVisible = false;
	    }
	
	    @Override
	    protected void onResume() {
	        super.onResume();
	        isVisible = true;
	    }
	
	    @Override
	    protected void onStop() {
	        super.onStop();
	        isVisible = false;
	    }
	
	    public void ToastNotify(final String notificationMessage) {
	        runOnUiThread(new Runnable() {
	            @Override
	            public void run() {
	                Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
	                TextView helloText = (TextView) findViewById(R.id.text_hello);
	                helloText.setText(notificationMessage);
	            }
	        });
	    }


10. Für die Methode `ToastNotify` wird das `TextView`-Steuerelement *„Hello World“* verwendet, um Status und Benachrichtigungen dauerhaft in der App zu melden. Fügen Sie im Layout von „activity\_main.xml“ die folgende ID für dieses Steuerelement hinzu.

        android:id="@+id/text_hello"

11. Als Nächstes fügen wir eine Unterklasse für unseren Empfänger hinzu, den wir in „AndroidManifest.xml“ definiert haben. Fügen Sie dem Projekt eine weitere neue Klasse namens `MyHandler` hinzu.

12. Fügen Sie am Anfang von `MyHandler.java` die folgenden Importanweisungen hinzu:

		import android.app.NotificationManager;
		import android.app.PendingIntent;
		import android.content.Context;
		import android.content.Intent;
		import android.media.RingtoneManager;
		import android.net.Uri;
		import android.os.Bundle;
		import android.support.v4.app.NotificationCompat;
		import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Fügen Sie für die `MyHandler`-Klasse den folgenden Code hinzu, um sie `com.microsoft.windowsazure.notifications.NotificationsHandler` unterzuordnen.

	Mit diesem Code wird die `OnReceive`-Methode überschrieben, sodass der Handler empfangene Benachrichtigungen meldet. Der Handler sendet die Pushbenachrichtigung mit der `sendNotification()`-Methode auch an den Android-Benachrichtigungs-Manager. Die `sendNotification()`-Methode muss ausgeführt werden, wenn die App nicht ausgeführt wird und eine Benachrichtigung eingeht.

		public class MyHandler extends NotificationsHandler {
		    public static final int NOTIFICATION_ID = 1;
		    private NotificationManager mNotificationManager;
		    NotificationCompat.Builder builder;
		    Context ctx;
		
		    @Override
		    public void onReceive(Context context, Bundle bundle) {
		        ctx = context;
		        String nhMessage = bundle.getString("message");
		        sendNotification(nhMessage);
		        if (MainActivity.isVisible) {
		            MainActivity.mainActivity.ToastNotify(nhMessage);
		        }
		    }
		
		    private void sendNotification(String msg) {
		
		        Intent intent = new Intent(ctx, MainActivity.class);
		        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
		
		        mNotificationManager = (NotificationManager)
		                ctx.getSystemService(Context.NOTIFICATION_SERVICE);
		
		        PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
		                intent, PendingIntent.FLAG_ONE_SHOT);
		
		        Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
		        NotificationCompat.Builder mBuilder =
		                new NotificationCompat.Builder(ctx)
		                        .setSmallIcon(R.mipmap.ic_launcher)
		                        .setContentTitle("Notification Hub Demo")
		                        .setStyle(new NotificationCompat.BigTextStyle()
		                                .bigText(msg))
		                        .setSound(defaultSoundUri)
		                        .setContentText(msg);
		
		        mBuilder.setContentIntent(contentIntent);
		        mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
		    }
		}


14. Klicken Sie in Android Studio auf der Menüleiste auf **Build** > **Rebuild Project** (Projekt neu erstellen), um sicherzustellen, dass im Code keine Fehler enthalten sind.
15. Führen Sie die App auf Ihrem Gerät aus, und vergewissern Sie sich, dass sie richtig beim Notification Hub registriert wird.

	> [AZURE.NOTE] Die Registrierung schlägt beim ersten Starten unter Umständen fehl, bis die `onTokenRefresh()`-Methode des Instanz-ID-Diensts aufgerufen wird. Durch die Aktualisierung sollte eine erfolgreiche Registrierung beim Notification Hub initiiert werden.

##Senden von Pushbenachrichtigungen

Sie können Pushbenachrichtigungen in Ihrer App testen, indem Sie sie über das [Azure-Portal] senden. Suchen Sie auf dem Hub-Blatt wie unten gezeigt nach dem Abschnitt **Problembehandlung**.

![Azure Notification Hubs – Testsendung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## (Optional) Senden von Pushbenachrichtigungen direkt aus der App

>[AZURE.IMPORTANT] Dieses Beispiel für das Senden von Benachrichtigungen über die Client-App ist nur zu Lernzwecken angegeben. Da hierfür das `DefaultFullSharedAccessSignature`-Element auf der Client-App vorhanden sein muss, besteht für Ihren Notification Hub das Risiko, dass ein Benutzer Zugriff auf das Senden von unberechtigten Benachrichtigungen an Ihre Clients erhält.

In der Regel werden Benachrichtigungen über einen Back-End-Server versendet. In einigen Fällen ist es jedoch wünschenswert, wenn Sie Pushbenachrichtigungen direkt aus der Clientanwendung senden können. In diesem Abschnitt wird erläutert, wie Sie mithilfe der [Azure Notification Hub-REST-API](https://msdn.microsoft.com/library/azure/dn223264.aspx) Benachrichtigungen vom Client senden können.

1. Erweitern Sie in der Projektansicht von Android Studio **App** > **src** > **main** > **res** > **layout**. Öffnen Sie die Layoutdatei `activity_main.xml`, und klicken Sie auf die Registerkarte **Text**, um den Textinhalt der Datei zu aktualisieren. Aktualisieren Sie ihn mit dem folgenden Code. Dadurch werden die neuen Steuerelemente `Button` und `EditText` hinzugefügt, um Nachrichten mit Pushbenachrichtigungen an den Notification Hub zu senden. Fügen Sie diesen Code am Ende der Datei unmittelbar vor `</RelativeLayout>` hinzu.

	    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

	    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Erweitern Sie in der Projektansicht von Android Studio **App** > **src** > **main** > **res** > **values**. Öffnen Sie die Datei `strings.xml`, und fügen Sie die Zeichenfolgenwerte hinzu, auf die von den neuen Steuerelementen `Button` und `EditText` verwiesen wird. Fügen Sie die Werte am Ende der Datei unmittelbar vor `</resources>` ein.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. Fügen Sie in der Datei `NotificationSetting.java` der `NotificationSettings`-Klasse die folgende Einstellung hinzu.

	Aktualisieren Sie `HubFullAccess` mit der Verbindungszeichenfolge **DefaultFullSharedAccessSignature** für Ihren Hub. Sie können diese Verbindungszeichenfolge im [Azure-Portal] kopieren, indem Sie auf dem Blatt **Einstellungen** für den Notification Hub auf **Zugriffsrichtlinien** klicken.

		public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";

4. Fügen Sie in der Datei `MainActivity.java` die folgenden `import`-Anweisungen über der `MainActivity`-Klasse hinzu.

		import java.io.BufferedOutputStream;
		import java.io.BufferedReader;
		import java.io.InputStreamReader;
		import java.io.OutputStream;
		import java.net.HttpURLConnection;
		import java.net.URL;
		import java.net.URLEncoder;
		import javax.crypto.Mac;
		import javax.crypto.spec.SecretKeySpec;
		import android.util.Base64;
		import android.view.View;
		import android.widget.EditText;

6. Fügen Sie in der Datei `MainActivity.java` im oberen Bereich der `MainActivity`-Klasse die folgenden Member hinzu.

	    private String HubEndpoint = null;
	    private String HubSasKeyName = null;
	    private String HubSasKeyValue = null;

6. Sie müssen ein Software Access Signature (SaS)-Token erstellen, um eine POST-Anforderung zum Senden von Nachrichten an den Notification Hub zu authentifizieren. Dies erfolgt durch das Analysieren der wichtigsten Daten aus der Verbindungszeichenfolge und dem anschließenden Erstellen des SaS-Tokens (siehe dazu die REST-API-Referenz unter [Allgemeine Konzepte](http://msdn.microsoft.com/library/azure/dn495627.aspx)). Der folgende Code ist ein Beispiel für eine Implementierung.

	Fügen Sie der `MainActivity`-Klasse in `MainActivity.java` die folgende Methode hinzu, um die Verbindungszeichenfolge zu analysieren.

	    /**
	     * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
	     * to parse the connection string so a SaS authentication token can be
	     * constructed.
	     *
	     * @param connectionString This must be the DefaultFullSharedAccess connection
	     *                         string for this example.
	     */
	    private void ParseConnectionString(String connectionString)
	    {
	        String[] parts = connectionString.split(";");
	        if (parts.length != 3)
	            throw new RuntimeException("Error parsing connection string: "
	                    + connectionString);
	
	        for (int i = 0; i < parts.length; i++) {
	            if (parts[i].startsWith("Endpoint")) {
	                this.HubEndpoint = "https" + parts[i].substring(11);
	            } else if (parts[i].startsWith("SharedAccessKeyName")) {
	                this.HubSasKeyName = parts[i].substring(20);
	            } else if (parts[i].startsWith("SharedAccessKey")) {
	                this.HubSasKeyValue = parts[i].substring(16);
	            }
	        }
	    }


7. Fügen Sie der `MainActivity`-Klasse in `MainActivity.java` die folgende Methode hinzu, um ein SAS-Authentifizierungstoken zu erstellen.

	    /**
	     * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
	     * construct a SaS token from the access key to authenticate a request.
	     *
	     * @param uri The unencoded resource URI string for this operation. The resource
	     *            URI is the full URI of the Service Bus resource to which access is
	     *            claimed. For example,
	     *            "http://<namespace>.servicebus.windows.net/<hubName>"
	     */
	    private String generateSasToken(String uri) {
	
	        String targetUri;
	        String token = null;
	        try {
	            targetUri = URLEncoder
	                    .encode(uri.toString().toLowerCase(), "UTF-8")
	                    .toLowerCase();
	
	            long expiresOnDate = System.currentTimeMillis();
	            int expiresInMins = 60; // 1 hour
	            expiresOnDate += expiresInMins * 60 * 1000;
	            long expires = expiresOnDate / 1000;
	            String toSign = targetUri + "\n" + expires;
	
	            // Get an hmac_sha1 key from the raw key bytes
	            byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
	            SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
	
	            // Get an hmac_sha1 Mac instance and initialize with the signing key
	            Mac mac = Mac.getInstance("HmacSHA256");
	            mac.init(signingKey);
	
	            // Compute the hmac on input data bytes
	            byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
	
	            // Using android.util.Base64 for Android Studio instead of
	            // Apache commons codec
	            String signature = URLEncoder.encode(
	                    Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
	
	            // Construct authorization string
	            token = "SharedAccessSignature sr=" + targetUri + "&sig="
	                    + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
	        } catch (Exception e) {
	            if (isVisible) {
	                ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
	            }
	        }
	
	        return token;
	    }



8. Fügen Sie der `MainActivity`-Klasse in `MainActivity.java` die folgende Methode hinzu, um das Click-Ereignis der Schaltfläche **Send Notification** zu verarbeiten und die Nachricht mit der Pushbenachrichtigung über die integrierte REST-API an den Hub zu senden.

	    /**
	     * Send Notification button click handler. This method parses the
	     * DefaultFullSharedAccess connection string and generates a SaS token. The
	     * token is added to the Authorization header on the POST request to the
	     * notification hub. The text in the editTextNotificationMessage control
	     * is added as the JSON body for the request to add a GCM message to the hub.
	     *
	     * @param v
	     */
	    public void sendNotificationButtonOnClick(View v) {
	        EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
	        final String json = "{"data":{"message":"" + notificationText.getText().toString() + ""}}";
	
	        new Thread()
	        {
	            public void run()
	            {
	                try
	                {
	                    // Based on reference documentation...
	                    // http://msdn.microsoft.com/library/azure/dn223273.aspx
	                    ParseConnectionString(NotificationSettings.HubFullAccess);
	                    URL url = new URL(HubEndpoint + NotificationSettings.HubName +
	                            "/messages/?api-version=2015-01");
	
	                    HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
	
	                    try {
	                        // POST request
	                        urlConnection.setDoOutput(true);
	
	                        // Authenticate the POST request with the SaS token
	                        urlConnection.setRequestProperty("Authorization", 
								generateSasToken(url.toString()));
	
	                        // Notification format should be GCM
	                        urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
	
	                        // Include any tags
	                        // Example below targets 3 specific tags
	                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
	                        // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
							//		"tag1 || tag2 || tag3");
	
	                        // Send notification message
	                        urlConnection.setFixedLengthStreamingMode(json.length());
	                        OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
	                        bodyStream.write(json.getBytes());
	                        bodyStream.close();
	
	                        // Get reponse
	                        urlConnection.connect();
	                        int responseCode = urlConnection.getResponseCode();
	                        if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
	                        }
	                    } finally {
	                        urlConnection.disconnect();
	                    }
	                }
	                catch(Exception e)
	                {
	                    if (isVisible) {
	                        ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
	                    }
	                }
	            }
	        }.start();
	    }



##Testen der App

####Pushbenachrichtigungen im Emulator

Wenn Sie Pushbenachrichtigungen in einem Emulator testen möchten, müssen Sie sicherstellen, dass das Emulatorabbild die Google-API-Ebene unterstützt, die Sie für die App ausgewählt haben. Wenn das Image keine nativen Google-APIs unterstützt, wird die Ausnahme **SERVICE\_NOT\_AVAILABLE** ausgegeben.

Stellen Sie zusätzlich zu den obigen Punkten sicher, dass Ihr Google-Konto dem ausgeführten Emulator unter **Einstellungen** > **Konten** hinzugefügt wurde. Andernfalls führt die Registrierung bei GCM möglicherweise zur Ausnahme **AUTHENTICATION\_FAILED**.

####Ausführen der Anwendung

1. Führen Sie die App aus, und prüfen Sie, ob die Registrierungs-ID als erfolgreiche Registrierung gemeldet wird.

   	![Testen unter Android – Kanalregistrierung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)

2. Geben Sie eine Benachrichtigungsmeldung ein, die an alle für den Hub registrierten Android-Geräte gesendet werden soll.

   	![Testen unter Android – Senden einer Nachricht](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)

3. Tippen Sie auf **Send Notification**. Auf allen Geräten, auf denen die App ausgeführt wird, wird eine `AlertDialog`-Instanz mit der Pushbenachrichtigungsnachricht angezeigt. Geräte, auf denen die App nicht ausgeführt wird, die aber zuvor für Pushbenachrichtigungen registriert wurden, erhalten im Android-Benachrichtigungs-Manager eine Benachrichtigung. Diese können durch Wischen von der linken oberen Ecke nach unten angezeigt werden.

   	![Testen unter Android – Benachrichtigungen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

##Nächste Schritte

Als nächster Schritt wird das Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer] empfohlen. Darin wird beschrieben, wie Sie Benachrichtigungen von einem ASP.NET-Back-End für bestimmte Benutzer mithilfe von Tags senden.

Wenn Sie Ihre Benutzer nach Interessengruppen unterteilen möchten, helfen Ihnen die Informationen im Tutorial [Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten] weiter.

Weitere allgemeine Informationen zu Notification Hubs finden Sie in unserem [Notification Hubs-Leitfaden].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs-Leitfaden]: notification-hubs-push-notification-overview.md
[Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure-Portal]: https://portal.azure.com

<!---HONumber=AcomDC_0907_2016-->