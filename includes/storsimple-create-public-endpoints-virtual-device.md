#### <a name="to-create-public-endpoints-on-the-virtual-device"></a>So erstellen Sie öffentliche Endpunkte auf dem virtuellen Gerät

1. Melden Sie sich am klassischen Azure-Portal an.
2. Klicken Sie auf **Virtuelle Computer**, und wählen Sie dann den als virtuelles Gerät zu verwendenden virtuellen Computer aus.
3. Klicken Sie auf **Endpunkte**. Auf der Seite **Endpunkte** werden alle Endpunkte für den virtuellen Computer aufgelistet.
4. Klicken Sie auf **Hinzufügen**. Das Dialogfeld **Endpunkt hinzufügen** wird angezeigt. Klicken Sie auf den Pfeil, um fortzufahren.
5. Geben Sie unter **Name** den folgenden Namen für den Endpunkt ein: **WinRMHttps**.
6. Geben Sie unter **Protokoll** den Wert **TCP** ein.
7. Geben Sie unter **Öffentlicher Port**die Portnummern für die Verbindung ein.
8. Geben Sie unter **Privater Port** den Wert **5986** ein.
9. Aktivieren Sie das Kontrollkästchen, um den Endpunkt zu erstellen.

Nach dem Erstellen des Endpunkts können Sie die Details zum Ermitteln der öffentlichen virtuellen IP-Adresse anzeigen. Notieren Sie sich diese Adresse.

