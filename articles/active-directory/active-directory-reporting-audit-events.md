<properties
   pageTitle="Azure Active Directory-Überwachungsberichtsereignisse | Microsoft Azure"
   description="Überwachte Ereignisse, die zum Anzeigen und Herunterladen aus Azure Active Directory verfügbar sind"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# Azure Active Directory-Überwachungsberichtsereignisse

*Diese Dokumentation ist Teil des [Handbuchs für Azure Active Directory Reporting](active-directory-reporting-guide.md).*

Der Azure Active Directory-Überwachungsbericht hilft Kunden, privilegierte Aktionen zu bestimmen, die in ihrem Azure Active Directory aufgetreten sind. Privilegierte Aktionen umfassen Änderungen zur Rechteerweiterung (z. B. das Erstellen von Rollen oder Zurücksetzen von Kennwörtern), das Ändern von Richtlinienkonfigurationen (z. B. Kennwortrichtlinien) oder Änderungen an der Verzeichniskonfiguration (z. B. Änderungen an Domänenverbundeinstellungen). Die Berichte enthalten den Überwachungsdatensatz für den Ereignisnamen, den Akteur, der die Aktion ausgeführt hat, die von der Änderung betroffene Zielressource sowie Datum und Uhrzeit (in UTC). Kunden können die Liste mit den Überwachungsereignissen für ihre Azure Active Directory-Instanz über das [Azure-Portal](https://portal.azure.com/) abrufen, wie im Artikel zur Anzeige von Überwachungsprotokollen ([Azure Active Directory-Berichterstellung – Vorschau](active-directory-reporting-azure-portal.md)) beschrieben.


## Liste der Überwachungsberichtsereignisse
<!--- audit event descriptions should be in the past tense --->

Ereignisse | Beschreibung des Ereignisses
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Benutzerereignisse** |
Benutzer hinzufügen | Einen Benutzer zum Verzeichnis hinzugefügt.
Benutzer löschen | Einen Benutzer aus dem Verzeichnis gelöscht.
Lizenzeigenschaften festlegen | Lizenzeigenschaften für einen Benutzer im Verzeichnis festgelegt.
Benutzerkennwort zurücksetzen | Kennwort eines Benutzers im Verzeichnis zurückgesetzt.
Benutzerkennwort ändern | Kennwort eines Benutzers im Verzeichnis geändert.
Benutzerlizenz ändern | Die einem Benutzer im Verzeichnis zugewiesene Lizenz geändert. Unten unter den Eigenschaften von [Benutzer aktualisieren](#update-user-attributes) können Sie sehen, welche Lizenzen aktualisiert wurden.
Benutzer aktualisieren | Einen Benutzer im Verzeichnis aktualisiert. Informationen zu Attributen, die aktualisiert werden können, [finden Sie weiter unter](#update-user-attributes).
Änderung des Benutzerkennworts erzwingen | Eigenschaft festgelegt, die einen Benutzer zwingt, sein Kennwort bei der Anmeldung zu ändern.
Benutzeranmeldeinformationen aktualisieren | Der Benutzer hat das Kennwort geändert.  
**Gruppenereignisse** |
Gruppe hinzufügen | Eine Gruppe wurde im Verzeichnis erstellt.
Gruppe aktualisieren | Eine Gruppe wurde im Verzeichnis aktualisiert. Unter [Überprüfte Gruppeneigenschaften](#update-group-attributes) im Abschnitt unten können Sie sehen, welche Gruppeneigenschaften aktualisiert wurden.
Gruppe löschen | Eine Gruppe wurde im Verzeichnis gelöscht.
Mitglied zur Gruppe hinzufügen | Einer Gruppe im Verzeichnis wurde ein Mitglied hinzugefügt.
Mitglied aus Gruppe entfernen | Ein Mitglied wurde aus einer Gruppe im Verzeichnis entfernt.
CreateGroupSettings | Gruppeneinstellungen erstellt
UpdateGroupSettings | Gruppeneinstellungen aktualisiert. Unter [Überprüfte Gruppeneigenschaften](#update-group-attributes) im Abschnitt unten können Sie sehen, welche Gruppeneinstellungen aktualisiert wurden.
DeleteGroupSettings | Gruppeneinstellungen gelöscht
SetGroupLicense | Gruppenlizenz festgelegt
SetGroupManagedBy | Gruppe für Verwaltung durch Benutzer festgelegt
AddGroupMember | Mitglied der Gruppe hinzugefügt
RemoveGroupMember | Mitglied aus Gruppe entfernen
AddGroupOwner | Besitzer der Gruppe hinzugefügt
RemoveGroupOwner | Besitzer aus Gruppe entfernt
**Anwendungsereignisse** |
Dienstprinzipal hinzufügen | Dienstprinzipal zum Verzeichnis hinzugefügt.
Dienstprinzipal entfernen | Dienstprinzipal aus dem Verzeichnis entfernt.
Anmeldeinformationen für Dienstprinzipal hinzufügen | Anmeldeinformationen für einen Dienstprinzipal hinzugefügt.
Anmeldeinformationen für Dienstprinzipal entfernen | Anmeldeinformationen für einen Dienstprinzipal entfernt.
Delegierungseintrag hinzufügen | [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis erstellt.
Delegierungseintrag festlegen | [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis aktualisiert.
Delegierungseintrag entfernen | [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis gelöscht.
**Rollenereignisse** |
Rollenmitglied zur Rolle hinzufügen | Benutzer einer Verzeichnisrolle hinzugefügt.
Rollenmitglied aus Rolle entfernen | Benutzer aus einer Verzeichnisrolle entfernt.
Kontaktinformationen für das Unternehmen festlegen | Kontakteinstellungen auf Unternehmensebene festgelegt. Dazu gehören E-Mail-Adressen für das Marketing sowie technische Benachrichtigungen zu Microsoft Online Services.
Delegierungseintrag hinzufügen | [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis erstellt.
Delegierungseintrag festlegen | [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis aktualisiert.
Delegierungseintrag entfernen | [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis gelöscht.
AddServicePrincipalOwner | Besitzer dem Dienstprinzipal hinzugefügt
RemoveSevicePrincipalOwner | Besitzer aus Dienstprinzipal entfernt
AddApplication | Anwendung hinzufügen
UpdateApplication | Anwendung aktualisieren. Unter [Überprüfte Anwendungseigenschaften](#update-application-attributes) im Abschnitt unten können Sie sehen, welche App-Einstellungen aktualisiert wurden.
DeleteApplication | Anwendung löschen
RestoreApplication|Anwendung wiederherstellen
AddApplicationOwner | 	Besitzer der Anwendung hinzufügen
RemoveApplicationOwner | 	Besitzer aus Anwendung entfernen
**Rollenereignisse** |
Rollenmitglied zur Rolle hinzufügen | Benutzer einer Verzeichnisrolle hinzugefügt.
Rollenmitglied aus Rolle entfernen | Benutzer aus einer Verzeichnisrolle entfernt.
AddRoleDefinition | Rollendefinition hinzugefügt
UpdateRoleDefinition | Rollendefinition aktualisiert. Unter [Überprüfte Eigenschaften von Rollendefinitionen](#update-role-definition-attributes) im Abschnitt unten können Sie sehen, welche Rolleneinstellungen aktualisiert wurden.
DeleteRoleDefinition | Rollendefinition gelöscht
AddRoleAssignmentToRoleDefinition | Rollenzuweisung der Rollendefinition hinzugefügt
RemoveRoleAssignmentFromRoleDefinition | Rollenzuweisung aus Rollendefinition entfernt
AddRoleFromTemplate | Rolle aus Vorlage hinzugefügt
UpdateRole | Rolle aktualisiert
AddRoleScopeMemberToRole | Zugeordnetes Mitglied der Rolle hinzugefügt
RemoveRoleScopedMemberFromRole | Zugeordnetes Mitglied aus Rolle entfernt
**Geräteereignisse (alle neuen Ereignisse)** |
AddDevice | Gerät hinzugefügt
UpdateDevice | Gerät aktualisiert. Unter [Überprüfte Geräteeigenschaften](#update-device-attributes) im Abschnitt unten können Sie sehen, welche Geräteeigenschaften aktualisiert wurden.
DeleteDevice | Gerät gelöscht
AddDeviceConfiguration | Gerätekonfiguration hinzugefügt
UpdateDeviceConfiguration | Gerätekonfiguration aktualisiert. Unter [Überprüfte Eigenschaften der Gerätekonfiguration](#update-device-configuration-attributes) im Abschnitt unten können Sie sehen, welche Eigenschaften der Gerätekonfiguration aktualisiert wurden.
DeleteDeviceConfiguration | Gerätekonfiguration gelöscht
AddRegisteredOwner | Registrierten Besitzer dem Gerät hinzugefügt
AddRegisteredUsers | Registrierte Benutzer dem Gerät hinzugefügt
RemoveRegisteredOwner | Registrierten Besitzer aus Gerät entfernen
RemoveRegisteredUsers | Registrierte Benutzer aus Gerät entfernen
RemoveDeviceCredentials | Geräteanmeldeinformationen entfernen
**B2B-Ereignisse** |
Batch-Einladungen hochgeladen. | Ein Administrator hat eine Datei mit Einladungen an Partnerbenutzer hochgeladen.
Verarbeitete Batch-Einladungen | Eine Datei mit Einladungen an Partnerbenutzer wurde verarbeitet.
Externen Benutzer einladen. | Ein externer Benutzer wurde in das Verzeichnis eingeladen.
Einlösen einer externen Benutzereinladung. | Ein externer Benutzer hat seine Einladung zum Verzeichnis eingelöst.
Externen Benutzer zur Gruppe hinzufügen. | Einem externen Benutzer wurde die Mitgliedschaft in einer Gruppe innerhalb dieses Verzeichnisses zugewiesen.
Externen Benutzer einer Anwendung zuweisen. | Einem externen Benutzer wurde direkter Zugriff auf eine Anwendung zugewiesen.
Erstellen eines viralen Mandanten. | Durch Einlösung der Einladung wurde ein neuer Mandant in Azure AD erstellt.
Erstellen eines viralen Benutzers. | Durch Einlösung der Einladung wurde in einem in Azure AD bestehenden Mandanten ein neuer Benutzer erstellt.
**Verwaltungseinheiten (alle neuen Ereignisse)** |
AddAdministrativeUnit | Verwaltungseinheit hinzufügen
UpdateAdministrativeUnit| 	Verwaltungseinheit aktualisieren. Unter [Überprüfte Eigenschaften von Verwaltungseinheiten](#update-administrative-unit-attributes) im Abschnitt unten können Sie sehen, welche Eigenschaften von Verwaltungseinheiten aktualisiert wurden.
DeleteAdministrativeUnit| 	Verwaltungseinheit löschen
AddMemberToAdministrativeUnit| 	Mitglied der Verwaltungseinheit hinzufügen
RemoveMemberFromAdministrativeUnit| 	Mitglied aus Verwaltungseinheit entfernen
**Verzeichnisereignisse** |
Partner zu Unternehmen hinzufügen | Partner zum Verzeichnis hinzugefügt.
Partner aus Unternehmen entfernen | Partner aus dem Verzeichnis entfernt.
DemotePartner | Partner tiefer stufen
Domäne zu Unternehmen hinzufügen | Domäne zum Verzeichnis hinzugefügt.
Domäne aus Unternehmen entfernen | Domäne aus dem Verzeichnis entfernt.
Domäne aktualisieren | Domäne im Verzeichnis aktualisiert. Unter [Überprüfte Domäneneigenschaften](#update-domain-attributes) im Abschnitt unten können Sie sehen, welche Domäneneigenschaften aktualisiert wurden.
Domänenauthentifizierung festlegen | Standarddomäneneinstellung für das Unternehmen geändert.
Kontaktinformationen für das Unternehmen festlegen | Kontakteinstellungen auf Unternehmensebene festgelegt. Dazu gehören E-Mail-Adressen für das Marketing sowie technische Benachrichtigungen zu Microsoft Online Services.
Verbundeinstellungen für Domäne festlegen | Verbundeinstellungen für eine Domäne aktualisiert.
Domäne überprüfen | Domäne im Verzeichnis überprüft.
Per E-Mail verifizierte Domäne überprüfen | Domäne im Verzeichnis mithilfe der E-Mail-Verifizierung überprüft.
Kennzeichen "DirSyncEnabled" für Unternehmen festlegen | Eigenschaft festgelegt, die ein Verzeichnis für die Azure AD-Synchronisierung aktiviert.
Kennwortrichtlinie festlegen | Längen- und Zeicheneinschränkungen für Benutzerkennwörter festgelegt.
Unternehmensinformationen festlegen | Informationen auf Unternehmensebene aktualisiert. Im Artikel zum PowerShell-Cmdlet [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) finden Sie weitere Details.
SetCompanyAllowedDataLocation | 	Zulässigen Datenspeicherort für Unternehmen festlegen
SetCompanyDirSyncEnabled | DirSyncEnabled-Flag festlegen
SetCompanyDirSyncFeature | 	DirSync-Funktion festlegen
SetCompanyInformation | Unternehmensinformationen festlegen
SetCompanyMultiNationalEnabled | 	Funktion für multinationales Unternehmen auf „Aktiviert“ festlegen
SetDirectoryFeatureOnTenant | 	Verzeichnisfunktion für Mandanten festlegen
SetTenantLicenseProperties | Lizenzeigenschaften für Mandanten festlegen
CreateCompanySettings | Unternehmenseinstellungen erstellen
UpdateCompanySettings | Unternehmenseinstellungen aktualisieren. Unter [Überprüfte Unternehmenseigenschaften](#update-company-attributes) im Abschnitt unten können Sie sehen, welche Unternehmenseigenschaften aktualisiert wurden.
DeleteCompanySettings | Unternehmenseinstellungen löschen
SetAccidentalDeletionThreshold | 	Schwellenwert für versehentliches Löschen festlegen
SetRightsManagementProperties | Eigenschaften für Rights Management festlegen
PurgeRightsManagementProperties | Rights Management-Eigenschaften bereinigen
UpdateExternalSecrets | Externe geheime Schlüssel aktualisieren
**Richtlinienereignisse (alle neuen Ereignisse)** |
AddPolicy | 	Richtlinie hinzufügen
UpdatePolicy |	Richtlinie aktualisieren
DeletePolicy |	Richtlinie löschen
AddDefaultPolicyApplication |	Richtlinie der Anwendung hinzufügen
AddDefaultPolicyServicePrincipal |	Richtlinie dem Dienstprinzipal hinzufügen
RemoveDefaultPolicyApplication |	Richtlinie aus Anwendung entfernen
RemoveDefaultPolicyServicePrincipal |	Richtlinie aus Dienstprinzipal entfernen
RemovePolicyCredentials |	Richtlinien-Anmeldeinformationen entfernen

## Aufbewahrung von Überwachungsberichten
Ereignisse im Azure AD-Überwachungsbericht werden für 180 Tage beibehalten. Weitere Informationen zur Aufbewahrung von Berichten finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md).

Kunden, die ihre Überwachungsereignisse für längere Zeit speichern möchten, können mithilfe der Reporting-API regelmäßig Überwachungsereignisse in einen separaten Datenspeicher abrufen. Ausführliche Informationen finden Sie unter [Erste Schritte mit der Azure AD Reporting-API](active-directory-reporting-api-getting-started.md).

## In jedem Überwachungsereignis enthaltene Eigenschaften

Eigenschaft | Beschreibung
------------- | --------------------------------------------------------------
Datum und Uhrzeit | Datum und Uhrzeit des Auftretens des Überwachungsereignisses
Akteur | Der Benutzer oder Dienstprinzipal, der die Aktion ausgeführt hat
Aktion | Die Aktion, die ausgeführt wurde
Ziel | Der Benutzer oder Dienstprinzipal, auf den die Aktion angewendet wurde


## Attribute von "Benutzer aktualisieren"
Das Überwachungsereignis "Benutzer aktualisieren" enthält Informationen dazu, welche Benutzerattribute aktualisiert wurden. Für jedes Attribut sind der vorherige Wert und der neue Wert enthalten.

Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Der Benutzer kann sich anmelden.
AssignedLicense | Alle Lizenzen, die dem Benutzer zugewiesen wurden.
AssignedPlan | Servicepläne, die sich aus dem Benutzer zugewiesenen Lizenzen ergeben.
LicenseAssignmentDetail | Details zu den dem Benutzer zugewiesenen Lizenzen. Bei einer gruppenbasierten Lizenzierung schließt dies die Gruppe ein, die die Lizenz erteilt hat.
Mobile | Die Mobiltelefonnummer des Benutzers.
OtherMail | Die alternative E-Mail-Adresse des Benutzers.
OtherMobile | Die alternative Mobiltelefonnummer des Benutzers.
StrongAuthenticationMethod | Eine Liste von Überprüfungsmethoden, die vom Benutzer für die Multi-Factor Authentication konfiguriert wurden, z. B. Sprachanruf, SMS oder Verifizierungscode aus einer mobilen App.
StrongAuthenticationRequirement | Wenn die Multi-Factor Authentication erzwungen wird, wird diese Einstellung für den Benutzer aktiviert oder deaktiviert.
StrongAuthenticationUserDetails | Die Telefonnummer des Benutzers, alternative Telefonnummer und E-Mail-Adresse, die für die Multi-Factor Authentication und Überprüfung der Kennwortzurücksetzung verwendet werden.
StrongAuthenticationPhoneAppDetail | Details von Telefon-Apps, die für 2FA-Anmeldung registriert sind.
TelephoneNumber | Die Telefonnummer des Benutzers.
AlternativeSecurityId | Alternative Sicherheits-ID für das Objekt.
CreationType |Erstellungsmethode des Benutzers (entweder per Einladung oder viral).
InviteTicket |Liste mit Einladungstickets für den Benutzer.
InviteReplyUrl |Liste mit URLs für die Antwort nach Annahme der Einladung.
InviteResources |Liste mit Ressourcen, für die der Benutzer eine Einladung erhalten hat.
LastDirSyncTime |Letzte Aktualisierung des Objekts aufgrund der Synchronisierung des autoritativen Verzeichnisses (Kunde, lokal).
MSExchRemoteRecipientType |Führt die Zuordnung zu MSO-Empfängertypen durch. Empfängertypen finden Sie unter [MSO-Empfängertypen] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx.
PreferredDataLocation |Der bevorzugte Speicherort für die Daten von Benutzern, Gruppen, Kontakten, öffentlichen Ordnern oder Geräten.
ProxyAddresses |Die Adresse, unter der ein Exchange Server-Empfängerobjekt in einem fremden E-Mail-System erkannt wird.
StsRefreshTokensValidFrom |Enthält Informationen zum Widerrufen von Aktualisierungstoken. Alle STS-Aktualisierungstoken, die vor diesem Zeitpunkt ausgestellt wurden, werden als abgelaufen angesehen.
UserPrincipalName |Der UPN ist ein Anmeldename im Internetformat für einen Benutzer.
UserState |Status des Benutzers: PendingApproval/PendingAcceptance/Accepted/PendingVerification.
UserStateChangedOn |TimeStamp (Zeitstempel) der letzten Änderung von UserState. Wird verwendet, um Lebenszyklus-Workflows auszulösen.
UserType |Typ des Benutzers: Mitglied (0), Gast (1), Viral (2).

## Attribute vom Typ „Gruppe aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Classification | Die Klassifizierung für eine einheitliche Gruppe (HBI, MBI usw.).
Description | Für Menschen lesbare Beschreibungen des Objekts.  
DisplayName |Der Anzeigename für ein Objekt.
DirSyncEnabled |Gibt an, ob die Synchronisierung über ein autoritatives Verzeichnis (Kunde, lokal) erfolgt.
GroupLicenseAssignment |Die Lizenzzuweisung einer Gruppe.
GroupType |Typ der Gruppe, vereinheitlicht (0).
IsMembershipRuleLocked |Gibt an, dass die MembershipRule-Eigenschaft vom Self-Service-Gruppenverwaltungsdienst festgelegt wird und von Benutzern nicht geändert werden kann. Gilt nur für Gruppen, bei denen GroupType das Element „GroupType.DynamicMembership“ enthält.
IsPublic |Flag, mit dem angegeben wird, ob die Gruppe öffentlich oder privat ist.
LastDirSyncTime |Letzte Aktualisierung des Objekts aufgrund der Synchronisierung des autoritativen Verzeichnisses (Kunde, lokal).
Mail |Die primäre E-Mail-Adresse.
MailEnabled |Gibt an, ob diese Gruppe über die E-Mail-Funktion verfügt.
MailNickname |Moniker für ein Adressbuchobjekt, normalerweise der Teil des E-Mail-Namens vor dem Symbol „@“.   
MembershipRule |Eine Zeichenfolge, mit der die vom Self-Service-Gruppenverwaltungsdienst verwendeten Kriterien ausgedrückt werden, um zu ermitteln, welche Mitglieder der Gruppe angehören sollen. Siehe auch „IsMembershipRuleLocked“. Gilt nur für Gruppen, bei denen GroupType das Element „GroupType.DynamicMembership“ enthält.
MembershipRuleProcessingState| Ein vom Self-Service-Gruppenverwaltungsdienst definierter Wert, mit dem der Status der Mitgliedschaftsverarbeitung für diese Gruppe festgelegt wird. Gilt nur für Gruppen, bei denen GroupType das Element „GroupType.DynamicMembership“ enthält.
ProxyAddresses |Die Adresse, unter der ein Exchange Server-Empfängerobjekt in einem fremden E-Mail-System erkannt wird.
RenewedDateTime |Zeitstempel-Datensatz für den Zeitpunkt, zu dem eine Gruppe zuletzt erneuert wurde.   
SecurityEnabled |Gibt an, ob sich die Mitgliedschaft in der Gruppe auf Autorisierungsentscheidungen auswirken kann.
WellKnownObject |Bezeichnet ein Verzeichnisobjekt, indem es als Teil einer vordefinierten Gruppe angegeben wird.

## Attribute vom Typ „Gerät aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Gibt an, ob für einen Sicherheitsprinzipal eine Authentifizierung möglich ist.
CloudAccountEnabled | Gibt an, ob für einen Sicherheitsprinzipal eine Authentifizierung möglich ist. Wird von InTune geschrieben, wenn das Gerät lokal verwaltet wird.
CloudDeviceOSType | Typ des Geräts basierend auf dem Betriebssystem, z.B. Windows RT, iOS. Wenn die Festlegung mit einem Clouddienst (z.B. Intune) durchgeführt wird, wird dieses Attribut im Verzeichnis für DeviceOSType autoritativ.
CloudDeviceOSVersion | Version des Betriebssystems. Wenn die Festlegung mit einem Clouddienst (z.B. Intune) durchgeführt wird, wird dieses Attribut im Verzeichnis für DeviceOSVersion autoritativ.
CloudDisplayName | Wert des LDAP-Attributs „displayName“. Wenn die Festlegung mit einem Clouddienst (z.B. Intune) durchgeführt wird, wird dieses Attribut im Verzeichnis für displayName autoritativ.
CloudCreated |Gibt an, ob das Objekt von Clouddiensten erstellt wurde.
CompliantUntil | Gibt an, bis wann das Gerät als kompatibel angesehen wird.
DeviceMetadata |Benutzerdefinierte Metadaten für das Gerät.
DeviceObjectVersion | Dieses Attribut wird verwendet, um die Schemaversion des Geräts zu identifizieren.
DeviceOSType |Typ des Geräts basierend auf dem Betriebssystem, z.B. Windows RT, iOS. Wird vom Registrierungsdienst geschrieben und ist für die Aktualisierung durch den MDM-Verwaltungsdienst oder den STS-Dienst für die einfache Verwaltung bestimmt.
DeviceOSVersion |Version des Betriebssystems. Wird vom Registrierungsdienst geschrieben und ist für die Aktualisierung durch den MDM-Verwaltungsdienst oder den STS-Dienst für die einfache Verwaltung bestimmt.
DevicePhysicalIds |Mehrwertiges Attribut zum Speichern von Bezeichnern des physischen Geräts. Kann BIOS-IDs, TPM-Fingerabdrücke, hardwarespezifische IDs usw. umfassen.
DirSyncEnabled | Gibt an, ob die Synchronisierung über ein autoritatives Verzeichnis (Kunde, lokal) erfolgt.
DisplayName |Der Anzeigename für ein Objekt.
IsCompliant | Dieses Attribut wird verwendet, um den Status der Mobilgerätverwaltung für das Gerät zu verwalten.
IsManaged |Mit diesem Attribut wird angegeben, dass das Gerät per Cloud-MDM verwaltet wird.
LastDirSyncTime |Letzte Aktualisierung des Objekts aufgrund der Synchronisierung des autoritativen Verzeichnisses (Kunde, lokal).

## Attribute vom Typ „Gerätekonfiguration aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Die maximale Anzahl von Tagen, über die ein Gerät inaktiv sein kann, bevor es für die Entfernung vorgesehen wird.
RegistrationQuota |Richtlinie zum Beschränken der Anzahl von Registrierungen, die für einen einzelnen Benutzer zulässig sind.

## Attribute vom Typ „Dienstprinzipalkonfiguration aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Gibt an, ob für einen Sicherheitsprinzipal eine Authentifizierung möglich ist.
AppPrincipalId | Externe anwendungsdefinierte Identität für einen Sicherheitsprinzipal.
DisplayName |Der Anzeigename für ein Objekt.
ServicePrincipalName | Ein Dienstprinzipalname mit „name/authority“. Hierbei wird mit „name“ ein Anwendungsklassenwert angegeben, und „authority“ enthält mindestens „hostname[:port]“ oder „name“, um einen Bezeichner für den Dienstprinzipal anzugeben.

## Attribute vom Typ „App aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Der Satz mit den Adressen (Umleitungs-URLs), die einem Dienstprinzipal zugewiesen sind.
AppId |Die Anwendungs-ID der App.   
AppIdentifierUri | Anwendungs-URI, mit dem die Anwendung identifiziert wird. Dies ist normalerweise die URL für den Anwendungszugriff.
AppLogoUrl |Die URL für das Anwendungslogobild, das in einem CDN gespeichert ist.
AvailableToOtherTenants | Bei „True“ ist die Anwendung mehrinstanzenfähig (sie kann also von anderen Mandanten verwendet werden).
DisplayName | Der Anzeigename für einen Anwendungsnamen.
Entitlement |Liste mit den Anwendungsberechtigungen.
ExternalUserAccountDelegationsAllowed |Mit diesem Flag wird angegeben, ob die Ressourcenanwendung vertrauenswürdig ist und Delegationseinträge für externe Benutzerkonten erstellt werden können.
GroupMembershipClaims |Die Anspruchsrichtlinie der Gruppenmitgliedschaft.
PublicClient | „True“, wenn der Client nicht geheim bleiben kann (nicht vertraulicher Client in OAuth2.0).
RecordConsentConditions | Arten von Zustimmungsbedingungen, wie durch die Vertragsbedingungen definiert: None (0), SilentConsentForPartnerManagedApp(1). Dieser Wert wird im Graph-API-Schema verfügbar gemacht und kann nur von Mandantenadministratoren festgelegt oder geändert werden.
RequiredResourceAccess |XML-Inhalt eines Werts der RequiredResourceAccess-Eigenschaft.
WebApp |Mit der Einstellung „True“ wird angegeben, dass die Anwendung eine Web-App ist.
WwwHomepage |Die primäre Webseite.

## Attribute vom Typ „Rolle aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Der Satz mit den Adressen (Umleitungs-URLs), die einem Dienstprinzipal zugewiesen sind.
BelongsToFirstLoginObjectSet |Mit „True“ wird angegeben, dass dieses Objekt zu der Gruppe von Objekten gehört, die zum Aktivieren der Anmeldung für den ersten Administrator eines neuen Mandanten erforderlich sind.
Builtin |Gibt an, ob sich die Lebensdauer eines Objekts im Besitz des Systems befindet.
Description | Für Menschen lesbare Beschreibungen des Objekts.  
DisplayName |Der Anzeigename für ein Objekt.
MailNickname | Moniker für ein Adressbuchobjekt, normalerweise der Teil des E-Mail-Namens vor dem Symbol „@“.
RoleDisabled |Gibt an, ob die Rolle zum Zweck der Zugriffsüberprüfung ignoriert werden soll.
RoleTemplateId | Die Identität der Rollenvorlage.
ServiceInfo |Dienstspezifische Bereitstellungsinformationen, die von MOAC und/oder anderen Dienstinstanzen (mit den gleichen oder anderen Diensttypen) genutzt werden können.
TaskSetScopeReference |Identifiziert ein TaskSet-Element und eine Gruppe von Bereichen, die einer Rolle oder Rollenvorlage zugeordnet sind.
ValidationError |Von einem Verbunddienst veröffentlichte Informationen zum Beschreiben eines dauerhaften dienstspezifischen Fehlers in Bezug auf die Eigenschaften oder einen Link von einer Objektadministratoraktion, die gelöst werden soll.
WellKnownObject |Bezeichnet ein Verzeichnisobjekt, indem es als Teil einer vordefinierten Gruppe angegeben wird.

## Attribute vom Typ „Rollendefinition aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes |Sammlung mit Autorisierungsbereichen, auf die verwiesen werden kann, wenn das RoleDefinition-Element einem Dienstprinzipal zugewiesen wird.
DisplayName |Der Anzeigename für ein Objekt.
GrantedPermissions |Vom RoleDefinition-Element gewährte Berechtigungen.


## Attribute vom Typ „Verwaltungseinheit aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Beschreibung |	Diese Eigenschaft wird aktualisiert, wenn Sie die Beschreibung einer Verwaltungseinheit ändern.
DisplayName |	Diese Eigenschaft wird aktualisiert, wenn Sie den Namen einer Verwaltungseinheit ändern.

## Attribute vom Typ „Unternehmen aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation | Ein Standort, an dem die Benutzer des Unternehmens bereitgestellt werden können.
AuthorizedServiceInstance | Namen der Dienstinstanzen, auf denen ein Plan bereitgestellt werden kann.
DirSyncEnabled |Gibt an, ob die Synchronisierung über ein autoritatives Verzeichnis (Kunde, lokal) erfolgt.
DirSyncStatus |Gibt an, ob die Synchronisierung von Adressbuchobjekten in diesem Mandantenkontext über ein autoritatives Verzeichnis (Kunde, lokal) erfolgt. Hierbei handelt es sich um eine Erweiterung der DirSyncEnabled-Eigenschaft von Company-Objekten.
DirSyncFeatures |Bitflag zum Nachverfolgen von aktivierten und deaktivierten DirSync-Features für den Mandanten.
DirectoryFeatures |Aktivierte/deaktivierte Verzeichnisfeatures.
DirSyncConfiguration |Enthält die gesamte spezifische DirSync-Konfiguration für den aktuellen Mandanten.
DisplayName |Der Anzeigename für ein Objekt.
IsMnc |Boolesches Flag, das auf „True“ festgelegt ist, wenn für das Unternehmen das Feature für multinationale Unternehmen aktiviert ist.
ObjectSettings | Eine Sammlung mit Einstellungen, die für den Bereich des Objekts gelten.
PartnerCommerceUrl |URL der kommerziellen Website des Partners.
PartnerHelpUrl |URL der Hilfe-Website des Partners.
PartnerSupportEmail | URL zur Support-E-Mail-Adresse des Partners.
PartnerSupportTelephone |URL zur Support-Telefonnummer des Partners.
PartnerSupportUrl |URL zur Support-Website des Partners.
StrongAuthenticationDetails |Details des StrongAuthentication-Elements.
StrongAuthenticationPolicy |Strenge Authentifizierungsrichtlinie für das Unternehmen.
TechnicalNotificationMail |E-Mail-Adresse zum Senden von Benachrichtigungen über technische Probleme eines Unternehmens.
TelephoneNumber |Telefonnummern, die die Anforderungen von ITU Recommendation E.123 erfüllen.
TenantType |Der Typ eines Mandanten. Wenn dieser Wert nicht angegeben wird, ist der Mandant ein Unternehmen (Company). Andere mögliche Werte sind: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2), BreadthPartnerDelegatedAdmin (3), ResellerPartnerDelegatedAdmin (4), ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Ein Satz mit DNS-Domänennamen, die an ein Unternehmen gebunden sind.

## Attribute vom Typ „Domäne aktualisieren“
Attribut | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Funktionen |Bitflags, mit denen die Funktionen der Domäne beschrieben werden, falls vorhanden.
Standard |Gibt an, ob die Domäne der Standardwert ist. Beispiel: UserPrincipalName-Standardsuffix, wenn ein Administrator einen neuen Benutzer in MOAC erstellt.
Initial |Gibt an, ob sich die Domäne gemäß OCP-Zuordnung in der Anfangsdomäne für das Unternehmen befindet. Die Anfangsdomäne ist eine eindeutige Unterdomäne einer Microsoft Online-Domäne, z.B. „contoso3.microsoftonline.com“.
LiveType |Der Typ des entsprechenden Windows Live-Namespace, falls vorhanden.
Name | Ein Bezeichner für den Endpunkt.
PasswordNotificationWindowDays |Die Anzahl von Tagen für die Benachrichtigung eines Benutzers vor Ablauf des Kennworts.
PasswordValidityPeriodDays | Die Anzahl von Tagen, nach denen ein Kennwort geändert werden muss.

Überwachungsdatensätze sind ein erforderliches Steuerelement in vielen Richtlinien zur Einhaltung von Vorschriften. Kunden, die den Azure Active Directory-Überwachungsbericht zum Erfüllen dieser Vorschriften nutzen, wird empfohlen, der Kopie des exportierten Überwachungsberichts eine Kopie dieses Hilfethemas zur Erläuterung der Berichtsdetails hinzuzufügen. Wenn der Prüfer die Richtlinien zur Vorschrifteneinhaltung bestimmen möchte, die Azure derzeit erfüllt, leiten Sie ihn zur [Seite "Compliance"](https://azure.microsoft.com/support/trust-center/compliance/) im Microsoft Azure Trust Center.

<!---HONumber=AcomDC_0921_2016-->