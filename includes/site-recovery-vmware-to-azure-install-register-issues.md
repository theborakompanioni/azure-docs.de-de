### <a name="installation-failures"></a>Fehler bei der Installation
| **Beispiel für Fehlermeldung** | **Empfohlene Maßnahme** |
|--------------------------|------------------------|
|ERROR   Failed to load Accounts. (FEHLER   Fehler beim Laden von Konten.) Error: System.IO.IOException: Unable to read data from the transport connection" when installing and registering the CS server (Fehler: System.IO.IOException: Von der Übertragungsverbindung können keine Daten gelesen werden, wenn der CS-Server installiert und registriert wird)| Ensure TLS 1.0 is enabled on the computer (Stellen Sie sicher, dass TLS 1.0 auf dem Computer aktiviert ist.) |

### <a name="registration-failures"></a>Fehler bei der Registrierung
Sie können Fehler bei der Registrierung debuggen, indem Sie sich die Protokolle im Ordner **%ProgramData%\ASRLogs** ansehen.

| **Beispiel für Fehlermeldung** | **Empfohlene Maßnahme** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Message: ACS50008: SAML token is invalid. (Nachricht: ACS50008: SAML-Token ist ungültig.)<br>Ablaufverfolgungs-ID: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Korrelations-ID: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Zeitstempel: **2016-12-12 14:50:08Z<br>** | Stellen Sie sicher, dass die Uhrzeit auf Ihrer **Systemuhr** im Vergleich zur **Ortszeit** nicht mehr als 15 Minuten vor- oder nachgeht. Führen Sie das Installationsprogramm erneut aus, um die Registrierung durchzuführen.|
|**09:35:27** :DRRegistrationException while trying to get all disaster recovery vault for the selected certificate: : Threw Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message: ACS50008: SAML token is invalid. (:DRRegistrationException beim Abrufen aller Notfallwiederherstellungstresore für das ausgewählte Zertifikat: : Auslösung von Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message: ACS50008: SAML-Token ist ungültig.)<br>Ablaufverfolgungs-ID: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Korrelations-ID: abe9deb8-3e64-464d-8375-36db9816427a<br>Zeitstempel: **2016-05-19 01:35:39Z**<br> | Stellen Sie sicher, dass die Uhrzeit auf Ihrer **Systemuhr** im Vergleich zur **Ortszeit** nicht mehr als 15 Minuten vor- oder nachgeht. Führen Sie das Installationsprogramm erneut aus, um die Registrierung durchzuführen.|
|06:28:45:Fehler beim Erstellen des Zertifikats.<br>06:28:45:Setup kann nicht fortgesetzt werden. Ein Zertifikat kann nicht erstellt werden, das für die Authentifizierung für Site Recovery erforderlich ist. Führen Sie Setup erneut aus. | Stellen Sie sicher, dass Sie das Setup als **Lokaler Administrator** ausführen. |


<!--HONumber=Jan17_HO3-->


