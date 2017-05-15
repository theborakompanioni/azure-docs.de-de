
### <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>Hinzufügen von Signatur- und Verschlüsselungsschlüsseln im B2C-Mandanten zur Verwendung durch benutzerdefinierte Richtlinien

1. Navigieren Sie in den Einstellungen für den Azure AD B2C-Mandanten zum Blatt „Identity Experience Framework“.
1. Wählen Sie `Policy Keys`, um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen. Wenn `B2C_1A_TokenSigningKeyContainer` vorhanden ist, überspringen Sie diesen Schlüssel.
1. Erstellen Sie `TokenSigningKeyContainer`.  
 * Klicken Sie auf `+Add`.
 * Optionen > `Generate`
 * Name > `TokenSigningKeyContainer`; das Präfix „B2C_1A_“ wird möglicherweise automatisch hinzugefügt.
 * Schlüsseltyp > `RSA`
 * Daten – Standardwerte verwenden
 * Schlüsselverwendung > `Signature`
1. Klicken Sie auf `Create`.
1. Wenn ein Schlüssel mit dem Namen `B2C_1A_TokenEncryptionKeyContainer` vorhanden ist, überspringen Sie diesen Schlüssel.
1. Erstellen Sie `TokenEncryptionKeyContainer`.
 * Optionen > `Generate`
 * Name > `TokenSigningKeyContainer`; das Präfix „B2C_1A_“ wird möglicherweise automatisch hinzugefügt.
 * Schlüsseltyp > `RSA`
 * Daten – Standardwerte verwenden
 * Schlüsselverwendung > `Encryption`
1. Klicken Sie auf `Create`.


[!TIP]
Die nächsten Schritte sind OPTIONAL, wenn Sie für Ihre Endbenutzer Identitätsanbieter sozialer Netzwerke oder Verbundidentitätsanbieter anbieten möchten.  Facebook bietet einen guten Ausgangspunkt, um mehr zu externen Identitätsanbietern mit Azure AD B2C zu erfahren, die benutzerdefinierte Richtlinien verwenden.

1. Erstellen Sie `FacebookSecret`.  Dieser Schritt ist zwar optional, wird jedoch für das einfache Testen der Fähigkeit zum Erstellen externer Verbunde empfohlen.  Damit wird ein fundierter Ausgangspunkt für die Weiterentwicklung Ihrer Richtlinien mit anderen ID-Anbietern erstellt.
 * Klicken Sie auf `+Add`.
 * Optionen > `Manual`
 * Name > `FacebookSecret`; das Präfix „B2C_1A_“ wird möglicherweise automatisch hinzugefügt.
 * Geheimnis > Geben Sie Ihr FacebookSecret aus „developers.facebook.com“ ein.  *Dies ist nicht Ihre Facebook-App-ID.*
 * Schlüsselverwendung > Signatur
1. Klicken Sie auf `Create`, bestätigen Sie die Erstellung, und notieren Sie den Namen.

[!NOTE]
Wenn Sie integrierte Richtlinien für Azure AD B2C verwenden, verwenden Sie in der Regel dasselbe Geheimnis für integrierte und benutzerdefinierte Richtlinien. 
