
<properties
	pageTitle="Verwenden von Attributen zum Erstellen erweiterter Regeln für Gruppenmitgliedschaften in der Azure Active Directory-Vorschau | Microsoft Azure"
	description="Erstellen erweiterter Regeln für eine dynamische Gruppenmitgliedschaft, einschließlich unterstützter Ausdrucksregeloperatoren und -parameter"
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="curtand"/>


# Verwenden von Attributen zum Erstellen erweiterter Regeln für Gruppenmitgliedschaften in der Azure Active Directory-Vorschau

Im Azure-Portal haben Sie die Möglichkeit, erweiterte Regeln zu erstellen, mit denen Sie komplexere attributbasierte, dynamische Mitgliedschaften für Gruppen in der Azure Active Directory-Vorschau (Azure AD) aktivieren können. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) In diesem Artikel werden die Regelattribute und die Syntax zum Erstellen dieser erweiterten Regeln erläutert.

## So erstellen Sie eine erweiterte Regel

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2.  Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

  ![Öffnen der Benutzerverwaltung](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus.

  ![Öffnen des Blatts „Gruppen“](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen** den Befehl **Hinzufügen** aus.

  ![Hinzufügen einer neuen Gruppe](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Geben Sie auf dem Blatt **Gruppe** einen Namen und eine Beschreibung für die neue Gruppe ein. Wählen Sie als **Mitgliedschaftstyp** entweder **Dynamischer Benutzer** oder **Dynamisches Gerät** aus, je nachdem, ob Sie eine Regel für Benutzer oder Geräte erstellen möchten. Wählen Sie anschließend **Dynamische Abfrage hinzufügen** aus. Informationen zu den Attributen für Geräteregeln finden Sie unter [Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte](#using-attributes-to-create-rules-for-device-objects).

  ![Hinzufügen einer Regel für eine dynamische Mitgliedschaft](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Geben Sie auf dem Blatt **Regeln für dynamische Mitgliedschaft** Ihre Regel in das Feld **Erweiterte Regel für dynamische Mitgliedschaft hinzufügen** ein, drücken Sie die EINGABETASTE, und wählen Sie im unteren Bereich des Blatts die Option **Erstellen** aus.

7. Wählen Sie auf dem Blatt **Gruppe** die Option **Erstellen** aus, um die Gruppe zu erstellen.

## Erstellen des Texts einer erweiterten Regel

Die erweiterte Regel, die Sie für die dynamischen Mitgliedschaften für Gruppen erstellen können, ist im Wesentlichen ein binärer Ausdruck, der aus drei Teilen besteht und ein wahres oder falsches Ergebnis liefert. Die drei Teile sind folgende:

- Linker Parameter
- Binärer Operator
- Rechte Konstante

Eine vollständige erweiterte Regel sieht etwa wie folgt aus: (leftParameter binaryOperator "RightConstant"). Für den gesamten Ausdruck sind öffnende und schließende Klammern erforderlich, die rechte Konstante muss in doppelte gerade Anführungszeichen eingeschlossen werden, und die Syntax für den linken Parameter lautet "user.property". Eine erweiterte Regel kann aus mehreren binären Ausdrücke bestehen, die durch die logischen Operatoren "-and", "-or" und "-not" getrennt sind.

Hier finden Sie Beispiele für eine richtig aufgebaute erweiterte Regel:

- (user.department -eq "Sales") -or (user.department -eq "Marketing")
- (user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")

Eine vollständige Liste der unterstützten Parameter und Ausdrucksregeloperatoren finden Sie in den folgenden Abschnitten. Informationen zu den Attributen für Geräteregeln finden Sie unter [Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte](#using-attributes-to-create-rules-for-device-objects).

Die Gesamtlänge des Texts der erweiterten Regel darf 2048 Zeichen nicht überschreiten.

> [AZURE.NOTE]
Bei string- und regex-Vorgängen wird die Groß-und Kleinschreibung nicht beachtet. Sie können auch NULL-Prüfungen durchführen, indem Sie "$null" als Konstante verwenden, z. B.: user.department -eq $null. Zeichenfolgen mit Anführungszeichen (") sollten mit einem Escapezeichen (') maskiert werden, z.B.: user.department -eq '"Sales".

## Unterstützte Ausdrucksregeloperatoren
Die folgende Tabelle enthält alle Ausdrucksregeloperatoren und ihre Syntax zur Verwendung im Text der erweiterten Regel:

| Operator | Syntax |
|-----------------|----------------|
| Not Equals | -ne |
| Equals | -eq |
| Not Starts With | -notStartsWith |
| Starts With | -startsWith |
| Not Contains | -notContains |
| Contains | -contains |
| Not Match | -notMatch |
| Match | -match |


## Korrektur von Abfragefehlern
In der folgenden Tabelle sind mögliche Fehler und deren entsprechende Behebung aufgeführt.

| Abfrageanalysefehler | Fehlerverwendung | Korrigierte Verwendung |
|-----------------------|-------------------|-----------------------------|
| Fehler: Das Attribut nicht unterstützt. | (user.invalidProperty -eq "Value") | (user.department -eq "value")<br/>Die Eigenschaft sollte einer der unterstützten Eigenschaften aus der [Liste oben](#supported-properties) entsprechen. |
| Fehler: Der Operator wird für das Attribut nicht unterstützt. | (user.accountEnabled -contains true) | (user.accountEnabled -eq true)<br/>Die Eigenschaft weist den Typ „boolesch“ auf. Verwenden Sie die unterstützten booleschen Operatoren (-eq oder -ne) aus der oben stehenden Liste. |
| Fehler: Abfragekompilierungsfehler. | (user.department -eq "Sales") -and (user.department -eq "Marketing")(user.userPrincipalName -match "*@domain.ext") | (user.department -eq "Sales") -and (user.department -eq "Marketing")<br/>Der logische Operator sollte einer der unterstützten Eigenschaften aus der obigen Liste entsprechen. (user.userPrincipalName -match ".*@domain.ext") oder (user.userPrincipalName -match "@domain.ext$") Fehler im regulären Ausdruck. |
| Fehler: Die Binärausdruck weist nicht das richtige Format auf. | (user.department –eq “Sales”) (user.department -eq "Sales")(user.department-eq"Sales") | (user.accountEnabled -eq true) -and (user.userPrincipalName -contains "alias@domain")<br/>Abfrage enthält mehrere Fehler. Die Klammern befinden sich nicht an der richtigen Stelle. |
| Fehler: Unbekannter Fehler beim Einrichten dynamischer Mitgliedschaften. | (user.accountEnabled -eq "True" AND user.userPrincipalName -contains "alias@domain") | (user.accountEnabled -eq true) -and (user.userPrincipalName -contains "alias@domain")<br/>Abfrage enthält mehrere Fehler. Die Klammern befinden sich nicht an der richtigen Stelle. |

## Unterstützte Eigenschaften
Im Folgenden werden alle Benutzereigenschaften aufgelistet, die Sie in der erweiterten Regel verwenden können:

### Eigenschaften vom Typ "boolesch"

Zulässige Operatoren

* -eq


* -ne


| Eigenschaften | Zulässige Werte | Verwendung |
|----------------|-----------------|--------------------------------|
| accountEnabled | true false | user.accountEnabled -eq true) |
| dirSyncEnabled | true false null | (user.dirSyncEnabled -eq true) |

### Eigenschaften vom Typ "string"

Zulässige Operatoren

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -contains


* -notContains


* -match


* -notMatch

| Eigenschaften | Zulässige Werte | Verwendung |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| city | Jeder string-Wert oder $null | (user.city -eq "value") |
| country | Jeder string-Wert oder $null | (user.country -eq "value") |
| department | Jeder string-Wert oder $null | (user.department -eq "value") |
| displayName | Jeder string-Wert. | (user.displayName -eq "value") |
| facsimileTelephoneNumber | Jeder string-Wert oder $null | (user.facsimileTelephoneNumber -eq "value") |
| givenName | Jeder string-Wert oder $null | (user.givenName -eq "value") |
| jobTitle | Jeder string-Wert oder $null | (user.jobTitle -eq "value") |
| mail | Jeder string-Wert oder $null (SMTP-Adresse des Benutzers) | (user.mail -eq "value") |
| mailNickName | Jeder string-Wert (E-Mail-Alias des Benutzers) | (user.mailNickName -eq "value") |
| mobile | Jeder string-Wert oder $null | (user.mobile -eq "value") |
| objectId | GUID des Benutzerobjekts. | (user.objectId -eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies | None DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword | (user.passwordPolicies -eq "DisableStrongPassword") |
| physicalDeliveryOfficeName | Jeder string-Wert oder $null | (user.physicalDeliveryOfficeName -eq "value") |
| postalCode | Jeder string-Wert oder $null | (user.postalCode -eq "value") |
| preferredLanguage | ISO 639-1 code | (user.preferredLanguage -eq "de-DE") |
| sipProxyAddress | Jeder string-Wert oder $null | (user.sipProxyAddress -eq "value") |
| state | Jeder string-Wert oder $null | (user.state -eq "value") |
| streetAddress | Jeder string-Wert oder $null | (user.streetAddress -eq "value") |
| surname | Jeder string-Wert oder $null | (user.surname -eq "value") |
| telephoneNumber | Jeder string-Wert oder $null | (user.telephoneNumber -eq "value") |
| usageLocation | Aus zwei Buchstaben bestehender Ländercode. | (user.usageLocation -eq "US") |
| userPrincipalName | Jeder string-Wert. | (user.userPrincipalName -eq "alias@domain") |
| userType | member guest $null | (user.userType -eq "Member") |

### Eigenschaften vom Typ "string collection"

Zulässige Operatoren

* -contains


* -notContains

| Eigenschaften | Zulässige Werte | Verwendung |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails | Jeder string-Wert. | (user.otherMails -contains "alias@domain") |
| proxyAddresses | SMTP: alias@domain smtp: alias@domain | (user.proxyAddresses -contains "SMTP: alias@domain") |

## Erweiterungsattribute und benutzerdefinierte Attribute
Erweiterungsattribute und benutzerdefinierte Attribute werden in Regeln für dynamische Mitgliedschaft unterstützt.

Erweiterungsattributes werden von einer lokalen Windows Server AD-Instanz synchronisiert und erhalten folgendes Format: ExtensionAttributeX. Dabei entspricht X 1 bis 15. Beispiel für eine Regel, die ein Erweiterungsattribut verwendet:

(user.extensionAttribute15 -eq "Marketing")

Benutzerdefinierte Attribute werden von einer lokalen Windows Server AD-Instanz oder von einer verbundenen SaaS-Anwendung aus synchronisiert und erhalten das Format „user.extension_[GUID]\__[Attribute]“. Dabei ist „[GUID]“ der eindeutige Bezeichner in AAD für die Anwendung, die das Attribut in AAD erstellt hat, und „[Attribute]“ ist der Name des Attributs bei seiner Erstellung. Beispiel für eine Regel, die ein benutzerdefiniertes Attribut verwendet:

user.extension\_c272a57b722d4eb29bfe327874ae79cb\_\_OfficeNumber

Den Namen des benutzerdefinierten Attributs finden Sie im Verzeichnis. Fragen Sie dazu das Attribut eines Benutzers mithilfe des Graph-Explorers ab, und suchen Sie nach dem Attributnamen.

## Mitarbeiterregel
Sie können Mitglieder einer Gruppe jetzt basierend auf dem manager-Attribut eines Benutzers auffüllen.

**So konfigurieren Sie eine Gruppe als Gruppe mit "Vorgesetzten"**

1. Führen Sie die Schritte 1 bis 5 unter [So erstellen Sie eine erweiterte Regel](#to-create-the-advanced-rule) aus, und wählen Sie als **Mitgliedschaftstyp** die Option **Dynamischer Benutzer**.

2. Geben Sie auf dem Blatt **Regeln für dynamische Mitgliedschaft** die Regel mit der folgenden Syntax ein:

	Mitarbeiter von *Mitarbeiter von {Objekt\_ID\_des\_Managers}*. Beispiel für eine gültige Regel für Mitarbeiter:

					Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

	Dabei ist „62e19b97-8b3d-4d4a-a106-4ce66896a863“ die Objekt-ID des Managers. Die Objekt-ID kann in Azure AD auf der Registerkarte **Profil** der Benutzerseite desjenigen Benutzers gefunden werden, der Manager ist.

3. Nach dem Speichern dieser Regel werden alle Benutzer, die diese Regel erfüllen, als Mitglieder dieser Gruppe eingetragen. Das erste Auffüllen der Gruppe kann einige Minuten dauern.


## Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte

Sie können auch eine Regel erstellen, die Geräteobjekte für die Mitgliedschaft in einer Gruppe auswählt. Die folgenden Geräteattribute können verwendet werden:

| Eigenschaften | Zulässige Werte | Verwendung |
|----------------------|---------------------------------|------------------------------------------------------|
| displayName | Jeder string-Wert | (device.displayName -eq "Rob Iphone”) |
| deviceOSType | Jeder string-Wert | (device.deviceOSType -eq "IOS") |
| deviceOSVersion | Jeder string-Wert | (device.OSVersion -eq "9.1") |
| isDirSynced | true false null | (device.isDirSynced -eq "true") |
| isManaged | true false null | (device.isManaged -eq "false") |
| isCompliant | true false null | (device.isCompliant -eq "true") |


## Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Gruppen in Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->