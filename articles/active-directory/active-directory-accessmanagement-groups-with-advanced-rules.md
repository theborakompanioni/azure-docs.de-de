---
title: Verwenden von Attributen zum Erstellen erweiterter Regeln | Microsoft-Dokumentation
description: "Vorgehensweisen zum Erstellen erweiterter Regeln für eine Gruppe, einschließlich unterstützter Ausdrucksregeloperatoren und -parameter."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 04813a42-d40a-48d6-ae96-15b7e5025884
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: d83372fbce5f49d7cd038a15bd271e9d8a463b7b
ms.openlocfilehash: f1cff67f31da87d6361603f0216a68c55686db0e


---
# <a name="using-attributes-to-create-advanced-rules"></a>Verwenden von Attributen zum Erstellen erweiterter Regeln
Im klassischen Azure-Portal haben Sie die Möglichkeit, erweiterte Regeln zu erstellen, mit denen Sie komplexere attributbasierte, dynamische Mitgliedschaften für Azure Active Directory-Gruppen (Azure AD) aktivieren können.  

Wenn sich Attribute eines Benutzers ändern, bewertet das System alle dynamischen Gruppenregeln in einem Verzeichnis, um zu ermitteln, ob die Attributänderung des Benutzers Vorgänge zum Hinzufügen oder Löschen von Gruppen auslöst. Falls ein Benutzer eine Regel für eine Gruppe erfüllt, wird er als Mitglieder zu dieser Gruppe hinzugefügt. Wenn er die Regel einer Gruppe nicht mehr erfüllt, deren Mitglied er ist, wird er aus dieser Gruppe entfernt.

> [!NOTE]
> Sie können eine Regel für die dynamische Mitgliedschaft für Sicherheits- oder Office 365-Gruppen einrichten. Geschachtelte Gruppenmitgliedschaften werden für die gruppenbasierte Zuweisung zu Anwendungen derzeit nicht unterstützt.
> 
> Für dynamische Gruppenmitgliedschaften muss eine Azure AD Premium-Lizenz folgenden Personen zugewiesen werden:
> 
> * Dem Administrator, der die Regel für eine Gruppe verwaltet
> * Alle Mitglieder der Gruppe
> 

## <a name="to-create-the-advanced-rule"></a>So erstellen Sie eine erweiterte Regel
1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)die Option **Active Directory**aus, und öffnen Sie dann das Verzeichnis Ihrer Organisation.
2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie dann die Gruppe, die Sie bearbeiten möchten.
3. Klicken Sie auf die Registerkarte **Konfigurieren**, wählen Sie die Option **Erweiterte Regel** aus, und geben Sie die erweiterte Regel im Textfeld ein.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Erstellen des Texts einer erweiterten Regel
Die erweiterte Regel, die Sie für die dynamischen Mitgliedschaften für Gruppen erstellen können, ist im Wesentlichen ein binärer Ausdruck, der aus drei Teilen besteht und ein wahres oder falsches Ergebnis liefert. Die drei Teile sind folgende:

* Linker Parameter
* Binärer Operator
* Rechte Konstante

Eine vollständige erweiterte Regel sieht etwa wie folgt aus: (leftParameter binaryOperator "RightConstant"). Für den gesamten Ausdruck sind öffnende und schließende Klammern erforderlich, die rechte Konstante muss in doppelte gerade Anführungszeichen eingeschlossen werden, und die Syntax für den linken Parameter lautet "user.property". Eine erweiterte Regel kann aus mehreren binären Ausdrücke bestehen, die durch die logischen Operatoren "-and", "-or" und "-not" getrennt sind.
Hier finden Sie Beispiele für eine richtig aufgebaute erweiterte Regel:

* (user.department -eq "Sales") -or (user.department -eq "Marketing")
* (user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")

Eine vollständige Liste der unterstützten Parameter und Ausdrucksregeloperatoren finden Sie in den folgenden Abschnitten.

Beachten Sie, dass die Eigenschaft als Präfix über den richtigen Objekttyp verfügen muss: „user“ oder „device“.
Die folgende Regel führt bei der Überprüfung zu einem Fehler: mail –ne null

Die richtige Regel lautet: 

user.mail –ne null

Die Gesamtlänge des Texts der erweiterten Regel darf 2048 Zeichen nicht überschreiten.

> [!NOTE]
> Bei string- und regex-Vorgängen wird die Groß-und Kleinschreibung nicht  beachtet. Zeichenfolgen mit Anführungszeichen (") sollten mit einem Escapezeichen (') maskiert werden. Beispiel: user.department -eq \`"Sales".
> Verwenden Sie nur für Zeichenfolgentyp-Werte Anführungszeichen, und verwenden Sie nur englische Anführungszeichen (also Anführungszeichen oben).
> 
> 

## <a name="supported-expression-rule-operators"></a>Unterstützte Ausdrucksregeloperatoren
Die folgende Tabelle enthält alle Ausdrucksregeloperatoren und ihre Syntax zur Verwendung im Text der erweiterten Regel:

| Operator | Syntax |
| --- | --- |
| Not Equals |-ne |
| Equals |-eq |
| Not Starts With |-notStartsWith |
| Starts With |-startsWith |
| Not Contains |-notContains |
| Contains |-contains |
| Not Match |-notMatch |
| Match |-match |

## <a name="operator-precedence"></a>Rangfolge der Operatoren

Unten sind alle Operatoren nach ihrer Rangfolge von niedrig bis hoch aufgeführt (Operator in derselben Zeile haben die gleiche Rangfolge): -any -all -or -and -not -eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch
 
Alle Operatoren können mit oder ohne Bindestrich als Präfix verwendet werden.

Beachten Sie, dass die Klammern nicht immer benötigt werden. Das Einfügen von Klammern ist nur erforderlich, wenn die Rangfolge nicht Ihre Anforderungen erfüllt: Beispiel:

   user.department –eq "Marketing" –and user.country –eq "US" 
   
entspricht: 

   (user.department –eq "Marketing") –and (user.country –eq "US")

## <a name="query-error-remediation"></a>Korrektur von Abfragefehlern
In der folgenden Tabelle sind mögliche Fehler und deren entsprechende Behebung aufgeführt.

| Abfrageanalysefehler | Fehlerverwendung | Korrigierte Verwendung |
| --- | --- | --- |
| Fehler: Das Attribut nicht unterstützt. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/>Die Eigenschaft sollte einer der unterstützten Eigenschaften aus der [Liste oben](#supported-properties). |
| Fehler: Der Operator wird für das Attribut nicht unterstützt. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/>Die Eigenschaft weist den Typ „boolesch“ auf. Verwenden Sie die unterstützten booleschen Operatoren (-eq oder -ne) aus der oben stehenden Liste. |
| Fehler: Abfragekompilierungsfehler. |(user.department -eq "Sales") -and (user.department -eq "Marketing")(user.userPrincipalName -match "*@domain.ext") |(user.department -eq "Sales") -and (user.department -eq "Marketing")<br/>Der logische Operator sollte mit einem der Operatoren in der obigen Liste der unterstützten Eigenschaften übereinstimmen. (user.userPrincipalName -match ".*@domain.ext")or(user.userPrincipalName -match "@domain.ext$")Error Fehler im regulären Ausdruck. |
| Fehler: Die Binärausdruck weist nicht das richtige Format auf. |(user.department –eq “Sales”) (user.department -eq "Sales")(user.department-eq"Sales") |(user.accountEnabled -eq true) -and (user.userPrincipalName -contains "alias@domain")<br/>Abfrage enthält mehrere Fehler. Die Klammern befinden sich nicht an der richtigen Stelle. |
| Fehler: Unbekannter Fehler beim Einrichten dynamischer Mitgliedschaften. |(user.accountEnabled -eq "True" AND user.userPrincipalName -contains "alias@domain") |(user.accountEnabled -eq true) -and (user.userPrincipalName -contains "alias@domain")<br/>Abfrage enthält mehrere Fehler. Die Klammern befinden sich nicht an der richtigen Stelle. |

## <a name="supported-properties"></a>Unterstützte Eigenschaften
Im Folgenden werden alle Benutzereigenschaften aufgelistet, die Sie in der erweiterten Regel verwenden können:

### <a name="properties-of-type-boolean"></a>Eigenschaften vom Typ "boolesch"
Zulässige Operatoren

* -eq
* -ne

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| accountEnabled |true false |user.accountEnabled -eq true) |
| dirSyncEnabled |true false null |(user.dirSyncEnabled -eq true) |

### <a name="properties-of-type-string"></a>Eigenschaften vom Typ "string"
Zulässige Operatoren

* -eq
* -ne
* -notStartsWith
* -startsWith
* -contains
* -notContains
* -match
* -notMatch

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| city |Jeder string-Wert oder $null |(user.city -eq "value") |
| country |Jeder string-Wert oder $null |(user.country -eq "value") |
| department |Jeder string-Wert oder $null |(user.department -eq "value") |
| displayName |Jeder string-Wert. |(user.displayName -eq "value") |
| facsimileTelephoneNumber |Jeder string-Wert oder $null |(user.facsimileTelephoneNumber -eq "value") |
| givenName |Jeder string-Wert oder $null |(user.givenName -eq "value") |
| jobTitle |Jeder string-Wert oder $null |(user.jobTitle -eq "value") |
| mail |Jeder string-Wert oder $null (SMTP-Adresse des Benutzers) |(user.mail -eq "value") |
| mailNickName |Jeder string-Wert (E-Mail-Alias des Benutzers) |(user.mailNickName -eq "value") |
| mobile |Jeder string-Wert oder $null |(user.mobile -eq "value") |
| objectId |GUID des Benutzerobjekts. |(user.objectId -eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies |None DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies -eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Jeder string-Wert oder $null |(user.physicalDeliveryOfficeName -eq "value") |
| postalCode |Jeder string-Wert oder $null |(user.postalCode -eq "value") |
| preferredLanguage |ISO 639-1 code |(user.preferredLanguage -eq "en-US") |
| sipProxyAddress |Jeder string-Wert oder $null |(user.sipProxyAddress -eq "value") |
| state |Jeder string-Wert oder $null |(user.state -eq "value") |
| streetAddress |Jeder string-Wert oder $null |(user.streetAddress -eq "value") |
| surname |Jeder string-Wert oder $null |(user.surname -eq "value") |
| telephoneNumber |Jeder string-Wert oder $null |(user.telephoneNumber -eq "value") |
| usageLocation |Aus zwei Buchstaben bestehender Ländercode. |(user.usageLocation -eq "US") |
| userPrincipalName |Jeder string-Wert. |(user.userPrincipalName -eq "alias@domain") |
| userType |member guest $null |(user.userType -eq "Member") |

### <a name="properties-of-type-string-collection"></a>Eigenschaften vom Typ "string collection"
Zulässige Operatoren

* -contains
* -notContains

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| otherMails |Jeder string-Wert. |(user.otherMails -contains "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses -contains "SMTP: alias@domain") |

## <a name="use-of-null-values"></a>Verwenden von NULL-Werten

Zum Angeben eines NULL-Werts in einer Regel können Sie „null“ oder „$null“ verwenden. Beispiel: 

   user.mail –ne null ist äquivalent zu user.mail –ne $null

## <a name="extension-attributes-and-custom-attributes"></a>Erweiterungsattribute und benutzerdefinierte Attribute
Erweiterungsattribute und benutzerdefinierte Attribute werden in Regeln für dynamische Mitgliedschaft unterstützt.

Erweiterungsattributes werden von einer lokalen Windows Server AD-Instanz synchronisiert und erhalten folgendes Format: ExtensionAttributeX. Dabei entspricht X 1 bis 15.
Beispiel für eine Regel, die ein Erweiterungsattribut verwendet:

(user.extensionAttribute15 -eq "Marketing")

Benutzerdefinierte Attribute werden von einer lokalen Windows Server AD-Instanz oder von einer verbundenen SaaS-Anwendung aus synchronisiert und erhalten das Format „user.extension_[GUID]\__[Attribute]“. Dabei ist „[GUID]“ der eindeutige Bezeichner in AAD für die Anwendung, die das Attribut in AAD erstellt hat, und „[Attribute]“ ist der Name des Attributs bei seiner Erstellung.
Beispiel für eine Regel, die ein benutzerdefiniertes Attribut verwendet:

user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Den Namen des benutzerdefinierten Attributs finden Sie im Verzeichnis. Fragen Sie dazu das Attribut eines Benutzers mithilfe des Graph-Explorers ab, und suchen Sie nach dem Attributnamen.

## <a name="support-for-multi-value-properties"></a>Unterstützung von mehrwertigen Eigenschaften

Verwenden Sie den Operator „-any“, um eine mehrwertige Eigenschaft in eine Regel einzufügen. Beispiel:

  user.assignedPlans -any assignedPlan.service -startsWith "SCO"
  
## <a name="direct-reports-rule"></a>Mitarbeiterregel
Sie können Mitglieder einer Gruppe basierend auf dem manager-Attribut eines Benutzers auffüllen.

**So konfigurieren Sie eine Gruppe als Gruppe mit "Vorgesetzten"**

1. Klicken Sie im klassischen Azure-Portal auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.
2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie dann die Gruppe, die Sie bearbeiten möchten.
3. Wählen Sie die Registerkarte **Konfigurieren** und anschließend die Option **ERWEITERTE REGEL** aus.
4. Geben Sie die Regel mit der folgenden Syntax ein:
   
    Mitarbeiter von *Mitarbeiter von {Objekt_ID_des_Managers}*. Beispiel für eine gültige Regel für Mitarbeiter:
   
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”
   
    Dabei ist „62e19b97-8b3d-4d4a-a106-4ce66896a863“ die Objekt-ID des Managers. Die Objekt-ID kann in Azure AD auf der Registerkarte **Profil** der Benutzerseite desjenigen Benutzers gefunden werden, der Manager ist.
5. Nach dem Speichern dieser Regel werden alle Benutzer, die diese Regel erfüllen, als Mitglieder dieser Gruppe eingetragen. Das erste Auffüllen der Gruppe kann einige Minuten dauern.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte
Sie können auch eine Regel erstellen, die Geräteobjekte für die Mitgliedschaft in einer Gruppe auswählt. Die folgenden Geräteattribute können verwendet werden:

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| displayName |Jeder string-Wert. |(device.displayName -eq "Rob Iphone”) |
| deviceOSType |Jeder string-Wert. |(device.deviceOSType -eq "IOS") |
| deviceOSVersion |Jeder string-Wert. |(device.OSVersion -eq "9.1") |
| isDirSynced |true false null |(device.isDirSynced -eq true) |
| isManaged |true false null |(device.isManaged -eq false) |
| isCompliant |true false null |(device.isCompliant -eq true) |
| deviceCategory |Jeder string-Wert. |(device.deviceCategory -eq "") |
| deviceManufacturer |Jeder string-Wert. |(device.deviceManufacturer -eq "Microsoft") |
| deviceModel |Jeder string-Wert. |(device.deviceModel -eq "IPhone 7+") |
| deviceOwnership |Jeder string-Wert. |(device.deviceOwnership -eq "") |
| domainName |Jeder string-Wert. |(device.domainName -eq "contoso.com") |
| enrollmentProfileName |Jeder string-Wert. |(device.enrollmentProfileName -eq "") |
| isRooted |true false null |(device.isRooted -eq true) |
| managementType |Jeder string-Wert. |(device.managementType -eq "") |
| organizationalUnit |Jeder string-Wert. |(device.organizationalUnit -eq "") |
| deviceId |Eine gültige deviceId |(device.deviceId -eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [!NOTE]
> Diese Geräteregeln können nicht mithilfe der Dropdownliste mit einfachen Regeln im klassischen Azure-Portal erstellt werden.
> 
> 

## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Problembehandlung bei dynamischen Mitgliedschaften für Gruppen](active-directory-accessmanagement-troubleshooting.md)
* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)




<!--HONumber=Feb17_HO2-->


