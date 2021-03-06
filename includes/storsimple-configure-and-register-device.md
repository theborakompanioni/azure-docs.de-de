<!--author=alkohli last changed: 12/01/15-->


#### <a name="to-configure-and-register-the-device"></a>So konfigurieren und registrieren Sie das Gerät
1. Greifen Sie an der seriellen Konsole Ihres StorSimple-Geräts auf die Windows PowerShell-Benutzeroberfläche zu. Anweisungen dazu finden Sie unter [Verwenden von PuTTY für das Herstellen einer Verbindung mit der seriellen Gerätekonsole](#use-putty-to-connect-to-the-device-serial-console) . **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen. Andernfalls sind Sie nicht in der Lage, auf die Konsole zuzugreifen.**
2. Drücken Sie in der Sitzung, die geöffnet wird, einmal die EINGABETASTE, um eine Eingabeaufforderung zu öffnen. 
3. Sie werden aufgefordert, die Sprache auszuwählen, die für Ihr Gerät festgelegt werden soll. Geben Sie die Sprache an, und drücken Sie dann die EINGABETASTE. 
   
    ![StorSimple – Konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)
4. Wählen Sie im Menü der seriellen Konsole, das angezeigt wird, Option 1 aus, d. h. die Anmeldung mit Vollzugriff. 
   
    ![StorSimple – Registrieren des Geräts 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
   
     Führen Sie die Schritte 5 bis 12 aus, um die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren. **Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden.** Das Menü der seriellen Konsole gibt den Zustand des Controllers in der Bannermeldung an. Wenn Sie nicht mit dem aktiven Controller verbunden sind, trennen Sie die Verbindung, und stellen Sie dann eine Verbindung mit dem aktiven Controller her.
5. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät lautet **Password1**.
6. Geben Sie den folgenden Befehl ein:
   
     `Invoke-HcsSetupWizard` 
7. Es wird ein Installations-Assistent angezeigt, der Sie beim Konfigurieren der Netzwerkeinstellungen für das Gerät unterstützt. Geben Sie die folgenden Informationen an: 
   
   * IP-Adresse für die Netzwerkschnittstelle DATA 0
   * Subnetzmaske
   * Gateway
   * IP-Adresse für den primären DNS-Server
   * IP-Adresse für den sekundären DNS-Server
     
     > [!NOTE]
     > Sie müssen ggf. einige Minuten warten, bis die Subnetzmaske und die DNS-Einstellungen übernommen werden. Wenn die Fehlermeldung "Das Gerät ist nicht bereit" angezeigt wird, überprüfen Sie die physische Netzwerkverbindung für die Netzwerkschnittstelle DATA 0 Ihres aktiven Controllers.
     > 
     > 
8. (Optional) Konfigurieren Sie Ihren Webproxyserver. Die Webproxykonfiguration ist optional. **Achten Sie jedoch bei Verwendung eines Webproxys darauf, dass dieser nur hier konfiguriert werden kann**. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md). Wenn während dieses Schritts Probleme auftreten, finden Sie Informationen zur Problembehandlung unter [Fehler während der Webproxykonfiguration](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings).

     > [!NOTE]
     > Sie können jederzeit STRG+C drücken, um den Installations-Assistenten zu beenden. Alle Einstellungen, die Sie vor dem Ausgeben dieses Befehls angewendet haben, werden beibehalten.

1. Aus Sicherheitsgründen läuft das Standardadministratorkennwort für das Gerät nach der ersten Sitzung ab, und Sie müssen es für die nachfolgenden Sitzungen ändern. Geben Sie, wenn Sie dazu aufgefordert werden, ein Administratorkennwort für das Gerät an. Ein gültiges Administratorkennwort für das Gerät muss zwischen 8 und 15 Zeichen lang sein. Das Kennwort muss eine Kombination aus Großbuchstaben, Kleinbuchstaben, Zahlen und Sonderzeichen enthalten.
2. Das Kennwort für den StorSimple-Momentaufnahme-Manager wird ebenfalls hier festgelegt. Verwenden Sie dieses Kennwort, wenn Sie ein Gerät bei Ihrem Windows-Host authentifizieren, der den StorSimple-Momentaufnahme-Manager ausführt. Wenn Sie dazu aufgefordert werden, geben Sie ein Kennwort ein, das 14 bis 15 Zeichen lang ist. Das Kennwort muss eine Kombination aus den folgenden Kategorien enthalten: Kleinbuchstaben, Großbuchstaben, Zahlen und Sonderzeichen. 
   
   ![StorSimple – Registrieren des Geräts 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)
   
   Sie können das Kennwort für den StorSimple-Momentaufnahme-Manager auch über die Benutzeroberfläche des StorSimple-Manager-Diensts zurückzusetzen. Die ausführlichen Schritte finden Sie unter [Ändern der StorSimple-Kennwörter mithilfe des StorSimple Manager-Diensts](../articles/storsimple/storsimple-change-passwords.md).
   
   Wenn während dieses Schritts Probleme auftreten, finden Sie Informationen zur Problembehandlung unter [Fehler bei Kennwörtern](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords).
3. Der letzte Schritt im Installations-Assistenten besteht im Registrieren Ihres Geräts beim StorSimple-Manager-Dienst. Zu diesem Zweck benötigen Sie den Dienstregistrierungsschlüssel, den Sie in Schritt 2 abgerufen haben. Nachdem Sie den Registrierungsschlüssel bereitgestellt haben, müssen Sie ggf. einige Minuten warten, bis das Gerät registriert wurde. 
   
   Informationen zum Beheben möglicher Fehler bei der Geräteregistrierung finden Sie unter [Fehler während der Geräteregistrierung](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-device-registration). Ausführliche Informationen zur Problembehandlung finden Sie unter [Schrittanleitung mit Beispielen zur Problembehandlung](../articles/storsimple/storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example).
4. Nachdem das Gerät registriert wurde, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und bewahren Sie ihn an einem sicheren Ort auf.
   
   > [!WARNING]
   > Dieser Schlüssel ist mit dem Dienstregistrierungsschlüssel zum Registrieren weiterer Geräte bei StorSimple-Manager-Dienst erforderlich. Weitere Informationen zu diesem Schlüssel finden Sie unter [StorSimple-Sicherheit](../articles/storsimple/storsimple-security.md) .
   > 
   > 
   
    ![StorSimple – Registrieren des Geräts 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)
   
    Markieren Sie zum Kopieren des Texts aus dem Fenster der seriellen Konsole den Text einfach. Sie sollten damit in der Lage sein, ihn in die Zwischenablage oder einen beliebigen Texteditor zu kopieren. Verwenden Sie NICHT STRG+C zum Kopieren des Schlüssels für die Dienstdatenverschlüsselung. STRG + C bewirkt, dass der Installations-Assistent beendet wird. Als Ergebnis werden das Geräteadministratorkennwort und das Kennwort des StorSimple-Momentaufnahme-Managers nicht geändert, und das Gerät wird auf die Standardkennwörter zurückgesetzt.
5. Schließen Sie die serielle Konsole.
6. Kehren Sie zum klassischen Azure-Portal zurück, und führen Sie die folgenden Schritte aus:
   
   1. Doppelklicken auf Ihren StorSimple-Manager-Dienst, um auf die Seite **Schnellstart** zuzugreifen.
   2. Klicken Sie auf **View connected devices**.
   3. Vergewissern Sie sich auf der Seite **Geräte** , dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status überprüfen. Als Gerätestatus sollte **Online**angezeigt werden. Wenn der Gerätestatus **Offline**ist, warten Sie einige Minuten, bis das Gerät online geschaltet wird.
   
   ![StorSimple – Seite "Geräte"](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
   
   > [!IMPORTANT]
   > Nachdem das Gerät online geschaltet wurde, schließen Sie die Netzwerkkabel wieder an, die Sie am Anfang dieses Schritts ausgesteckt haben.
   > 
   > 

Wenn das Gerät erfolgreich registriert wurde, jedoch nicht online geschaltet wird, führen Sie `Test-HcsmConnection -Verbose` aus, um sicherzustellen, dass die Netzwerkverbindungen funktionieren. Ausführliche Informationen zur Verwendung dieses Cmdlets finden Sie in der [Cmdlet-Referenz für "Test-HcsmConnection"](https://technet.microsoft.com/library/dn715782.aspx).

![Video verfügbar](./media/storsimple-configure-and-register-device/Video_icon.png) **Video verfügbar**

Sie können sich [hier](https://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/)ein Video anschauen, dass die Konfiguration und Registrierung Ihres Geräts über Windows PowerShell für StorSimple demonstriert.

