---
title: Attributbasierte dynamische Gruppenmitgliedschaft in Azure Active Directory | Microsoft-Dokumentation
description: "Erstellen erweiterter Regeln für eine dynamische Gruppenmitgliedschaft, einschließlich unterstützter Ausdrucksregeloperatoren und -parameter"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fb434cc2-9a91-4ebf-9753-dd81e289787e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 780f94f9863f73834ab72e9daf4362bea28242e9
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Erstellen attributbasierter Regeln für dynamische Gruppenmitgliedschaft in Azure Active Directory
In Azure Active Directory (Azure AD) können Sie erweiterte Regeln für die Aktivierung von komplexen, attributbasierten dynamischen Mitgliedschaften für Gruppen erstellen. In diesem Artikel werden Attribute und Syntax zum Erstellen der Regeln für dynamische Mitgliedschaft für Benutzer oder Geräte erläutert.

Wenn sich Attribute eines Benutzers oder Geräts ändern, bewertet das System alle dynamischen Gruppenregel in einem Verzeichnis, um zu ermitteln, ob die Änderung irgendwelche Vorgänge zum Hinzufügen oder Löschen von Gruppen auslöst. Falls ein Benutzer oder Gerät wird als Mitglied zu einer Gruppe hinzugefügt, wenn eine Regel dieser Gruppe erfüllt wird. Wenn sie diese Regel nicht mehr erfüllen, werden sie entfernt.

> [!NOTE]
> - Sie können eine Regel für die dynamische Mitgliedschaft für Sicherheits- oder Office 365-Gruppen einrichten.
>
> - Diese Funktion erfordert für jeden Benutzer, der mindestens einer dynamischen Gruppe hinzugefügt wurde, eine Azure AD Premium P1-Lizenz.
>
> - Sie können zwar eine dynamische Gruppe für Geräte oder Benutzer erstellen, jedoch können Sie keine Regel erstellen, die sowohl Benutzer- als auch Geräteobjekte enthält.

> - Derzeit ist es nicht möglich, eine Gerätegruppe basierend auf den Attributen zuständiger Benutzer zu erstellen. Die Regeln für die Gerätemitgliedschaft können nur auf unmittelbare Attribute von Geräteobjekten im Verzeichnis verweisen.

## <a name="to-create-an-advanced-rule"></a>Erstellen einer erweiterten Regel
1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator oder Benutzerkontoadministrator konfiguriert ist.
2. Wählen Sie **Benutzer und Gruppen**.
3. Wählen Sie **Alle Gruppen** aus.

   ![Öffnen des Blatts „Gruppen“](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)
4. Wählen Sie in **Alle Gruppen** die Option **Neue Gruppe** aus.

   ![Neue Gruppe hinzufügen](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)
5. Geben Sie auf dem Blatt **Gruppe** einen Namen und eine Beschreibung für die neue Gruppe ein. Wählen Sie als **Mitgliedschaftstyp** entweder **Dynamischer Benutzer** oder **Dynamisches Gerät** aus, je nachdem, ob Sie eine Regel für Benutzer oder Geräte erstellen möchten. Wählen Sie anschließend **Dynamische Abfrage hinzufügen** aus. Informationen zu den Attributen für Geräteregeln finden Sie unter [Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte](#using-attributes-to-create-rules-for-device-objects).

   ![Hinzufügen einer Regel für eine dynamische Mitgliedschaft](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)
6. Geben Sie auf dem Blatt **Regeln für dynamische Mitgliedschaft** Ihre Regel in das Feld **Erweiterte Regel für dynamische Mitgliedschaft hinzufügen** ein, drücken Sie die EINGABETASTE, und wählen Sie im unteren Bereich des Blatts die Option **Erstellen** aus.
7. Wählen Sie **Erstellen** on the **Erstellen** aus, um die Gruppe zu erstellen.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Erstellen des Texts einer erweiterten Regel
Die erweiterte Regel, die Sie für die dynamischen Mitgliedschaften für Gruppen erstellen können, ist im Wesentlichen ein binärer Ausdruck, der aus drei Teilen besteht und ein wahres oder falsches Ergebnis liefert. Die drei Teile sind folgende:

* Linker Parameter
* Binärer Operator
* Rechte Konstante

Eine vollständige erweiterte Regel sieht etwa wie folgt aus: (leftParameter binaryOperator "RightConstant"). Für den gesamten Binärausdruck sind öffnende und schließende Klammern optional. Doppelte Anführungszeichen sind ebenfalls optional und werden nur für die rechte Konstante benötigt, falls diese eine Zeichenfolge ist. Die Syntax für den linken Parameter lautet "user.property". Eine erweiterte Regel kann aus mehreren binären Ausdrücke bestehen, die durch die logischen Operatoren "-and", "-or" und "-not" getrennt sind.

Hier finden Sie Beispiele für eine richtig aufgebaute erweiterte Regel:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Eine vollständige Liste der unterstützten Parameter und Ausdrucksregeloperatoren finden Sie in den folgenden Abschnitten. Informationen zu den Attributen für Geräteregeln finden Sie unter [Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte](#using-attributes-to-create-rules-for-device-objects).

Die Gesamtlänge des Texts der erweiterten Regel darf 2048 Zeichen nicht überschreiten.

> [!NOTE]
> Bei Vorgängen mit Zeichenfolgen und regulären Ausdrücken wird die Groß- und Kleinschreibung nicht beachtet. Sie können auch NULL-Prüfungen durchführen, indem Sie "$null" als Konstante verwenden, z. B.: user.department -eq $null.
> Zeichenfolgen mit Anführungszeichen (") sollten mit einem Escapezeichen (') maskiert werden. Beispiel: user.department -eq \`"Sales".

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
| Geben Sie in | -in |
| Not In | -notIn |

## <a name="operator-precedence"></a>Rangfolge der Operatoren

Alle Operatoren werden unten nach Rangfolge von niedrig nach hoch aufgeführt. Operatoren in derselben Zeile haben denselben Rang:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Alle Operatoren können mit oder ohne vorangestellten Bindestrich verwendet werden. Klammern sind nur erforderlich, wenn die Rangfolge nicht Ihren Anforderungen entspricht.
Beispiel:
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
entspricht:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-the--in-and--notin-operators"></a>Mithilfe der Operatoren „-In“ und „-notIn“

Wenn Sie den Wert eines Benutzerattributs mit einer Reihe unterschiedlicher Werte vergleichen möchten, können Sie mit den Operatoren „-In“ oder „-notIn“. Hier ist ein Beispiel für den Operator „-In“:
```
    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]
```
Beachten Sie die Verwendung der eckigen Klammern „[“ und „]“ am Anfang und Ende der Werteliste. Diese Bedingung wertet nach TRUE aus, wenn der Wert von „user.department“ einem der Werte in der Liste entspricht.


## <a name="query-error-remediation"></a>Korrektur von Abfragefehlern
In der folgenden Tabelle sind mögliche Fehler und deren entsprechende Behebung aufgeführt.

| Abfrageanalysefehler | Fehlerverwendung | Korrigierte Verwendung |
| --- | --- | --- |
| Fehler: Das Attribut nicht unterstützt. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/>Die Eigenschaft sollte einer der unterstützten Eigenschaften aus der [Liste oben](#supported-properties). |
| Fehler: Der Operator wird für das Attribut nicht unterstützt. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/>Die Eigenschaft weist den Typ „boolesch“ auf. Verwenden Sie die unterstützten booleschen Operatoren (-eq oder -ne) aus der oben stehenden Liste. |
| Fehler: Abfragekompilierungsfehler. |(user.department -eq "Sales") -and (user.department -eq "Marketing")(user.userPrincipalName -match "*@domain.ext") |(user.department -eq "Sales") -and (user.department -eq "Marketing")<br/>Der logische Operator sollte mit einem der Operatoren in der obigen Liste der unterstützten Eigenschaften übereinstimmen.(user.userPrincipalName -match ".*@domain.ext")or(user.userPrincipalName -match "@domain.ext$")Fehler im regulären Ausdruck. |
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
| accountEnabled |true false |user.accountEnabled -eq true |
| dirSyncEnabled |true false |user.dirSyncEnabled -eq true |

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
* -in
* -notIn

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| city |Jeder string-Wert oder $null |(user.city -eq "value") |
| country |Jeder string-Wert oder $null |(user.country -eq "value") |
| companyName | Jeder string-Wert oder $null | (user.companyName -eq "value") |
| department |Jeder string-Wert oder $null |(user.department -eq "value") |
| displayName |Jeder string-Wert. |(user.displayName -eq "value") |
| facsimileTelephoneNumber |Jeder string-Wert oder $null |(user.facsimileTelephoneNumber -eq "value") |
| givenName |Jeder string-Wert oder $null |(user.givenName -eq "value") |
| jobTitle |Jeder string-Wert oder $null |(user.jobTitle -eq "value") |
| mail |Jeder string-Wert oder $null (SMTP-Adresse des Benutzers) |(user.mail -eq "value") |
| mailNickName |Jeder string-Wert (E-Mail-Alias des Benutzers) |(user.mailNickName -eq "value") |
| mobile |Jeder string-Wert oder $null |(user.mobile -eq "value") |
| objectId |GUID des Benutzerobjekts. |(user.objectId -eq "1111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | Lokale Sicherheits-ID (SID) für Benutzer, deren Daten von einem lokalen Standort in die Cloud synchronisiert wurden. |(user.onPremisesSecurityIdentifier -eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
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

## <a name="multi-value-properties"></a>Mehrwertige Eigenschaften
Zulässige Operatoren

* „-any“ (erfüllt, wenn mindestens ein Element in der Auflistung der Bedingung entspricht)
* „-all“ (erfüllt, wenn alle Elemente in der Auflistung der Bedingung entsprechen)

| Eigenschaften | Werte | Verwendung |
| --- | --- | --- |
| assignedPlans |Jedes Objekt in der Sammlung macht folgende Zeichenfolgeneigenschaften verfügbar: capabilityStatus, service, servicePlanId |user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled") |

Mehrwertige Eigenschaften sind Sammlungen von Objekten desselben Typs. Sie können die Operatoren „-any“ und „-all“ verwenden, um eine Bedingung auf ein Element oder alle Elemente in der Sammlung anzuwenden. Beispiel:

assignedPlans ist eine mehrwertige-Eigenschaft, die alle Service-Pläne auflistet, die dem Benutzer zugewiesen sind. Der folgende Ausdruck wird Benutzer auswählen, die über einen Service-Plan von Exchange Online (Plan 2) verfügen, der sich im Zustand „Enabled“ befindet:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(Der Guid-Bezeichner kennzeichnet den Service-Plan von Exchange Online (Plan 2))

> [!NOTE]
> Dieser Vorgang ist sehr nützlich, wenn Sie alle Benutzer identifizieren möchten, für die eine Funktion für Office 365 (oder einen anderen Microsoft-Onlinedienst) aktiviert wurde, um diesen beispielsweise einen bestimmten Satz von Richtlinien zuzuweisen.

Der folgende Ausdruck wählt alle Benutzer aus, die über einen Service-Plan verfügen, der mit dem Intune-Dienst (identifizierbar anhand des Dienstnamens „SCO“) verknüpft ist:
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Verwenden von NULL-Werten

Zum Angeben eines NULL-Werts in einer Regel können Sie „null“ oder „$null“ verwenden. Beispiel:
```
   user.mail –ne null
```
entspricht
```
   user.mail –ne $null
   ```

## <a name="extension-attributes-and-custom-attributes"></a>Erweiterungsattribute und benutzerdefinierte Attribute
Erweiterungsattribute und benutzerdefinierte Attribute werden in Regeln für dynamische Mitgliedschaft unterstützt.

Erweiterungsattribute werden von einer lokalen Windows Server AD-Instanz synchronisiert und erhalten folgendes Format: ExtensionAttributeX. Dabei entspricht X 1 bis 15.
Beispiel für eine Regel, die ein Erweiterungsattribut verwendet:
```
(user.extensionAttribute15 -eq "Marketing")
```
Benutzerdefinierte Attribute werden von einer lokalen Windows Server AD-Instanz oder von einer verbundenen SaaS-Anwendung aus synchronisiert und erhalten das Format „user.extension_[GUID]\__[Attribute]“. Dabei ist „[GUID]“ der eindeutige Bezeichner in AAD für die Anwendung, die das Attribut in AAD erstellt hat, und „[Attribute]“ ist der Name des Attributs bei seiner Erstellung.
Beispiel für eine Regel, die ein benutzerdefiniertes Attribut verwendet:
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
Den Namen des benutzerdefinierten Attributs finden Sie im Verzeichnis. Fragen Sie dazu das Attribut eines Benutzers mithilfe des Graph-Explorers ab, und suchen Sie nach dem Attributnamen.

## <a name="direct-reports-rule"></a>Mitarbeiterregel
Sie können eine Gruppe erstellen, die alle einem Manager direkt unterstellten Mitarbeiter enthält. Wenn sich die direkt unterstellten Mitarbeiter eines Managers in der Zukunft ändern, wird die Mitgliedschaft in der Gruppe automatisch angepasst.

> [!NOTE]
> 1. Damit diese Regel funktioniert, stellen Sie sicher, dass die Eigenschaft **Manager ID** (Manager-ID) für die Benutzer in Ihrem Mandanten korrekt festgelegt ist. Sie können den aktuellen Wert für einen Benutzer in deren **Registerkarte „Profil“** überprüfen.
> 2. Diese Regel gilt nur für **direct** (direkt) unterstellte Mitarbeiter. Es ist derzeit nicht möglich, eine Gruppe für eine geschachtelte Hierarchie zu erstellen, wie zum Beispiel eine Gruppe, die direkt unterstellte Mitarbeiter und deren unterstellte Mitarbeiter enthält.

**To configure the group** (Konfigurieren der Gruppe)

1. Führen Sie die Schritte 1 bis 5 im Abschnitt [To create the advanced rule (Erstellen einer erweiterten Regel)](#to-create-the-advanced-rule), aus, und wählen Sie als **Membership type** (Mitgliedschaftstyp) die Option **Dynamic User** (Dynamischer Benutzer) aus.
2. Geben Sie auf dem Blatt **Regeln für dynamische Mitgliedschaft** die Regel mit der folgenden Syntax ein:

    *Direct Reports for "{obectID_of_manager}"* (Direkt unterstellte Mitarbeiter für „{obectID_of_manager}“)

    Beispiel für eine gültige Regel:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Nachdem die Regel gespeichert wurde, werden alle Benutzer der Gruppe hinzugefügt, die über den festgelegten Wert der Manager-ID verfügen.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte
Sie können auch eine Regel erstellen, die Geräteobjekte für die Mitgliedschaft in einer Gruppe auswählt. Die folgenden Geräteattribute können verwendet werden.

 Geräteattribut  | Werte | Beispiel
 ----- | ----- | ----------------
 accountEnabled | true false | (device.accountEnabled -eq true)
 displayName | Jeder string-Wert. |(device.displayName -eq "Rob Iphone”)
 deviceOSType | Jeder string-Wert. | (device.deviceOSType -eq "IOS")
 deviceOSVersion | Jeder string-Wert. | (device.OSVersion -eq "9.1")
 deviceCategory | ein gültiger Gerätekategoriename | (device.deviceCategory -eq "BYOD")
 deviceManufacturer | Jeder string-Wert. | (device.deviceManufacturer -eq "Samsung")
 deviceModel | Jeder string-Wert. | (device.deviceModel -eq "iPad Air")
 deviceOwnership | Personal, Unternehmen, Unbekannt | (device.deviceOwnership -eq "Company")
 domainName | Jeder string-Wert. | (device.domainName -eq "contoso.com")
 enrollmentProfileName | Profilname für Apple-Geräteregistrierung | (device.enrollmentProfileName -eq "DEP iPhones")
 isRooted | true false | (device.isRooted -eq true)
 managementType | MDM (bei mobilen Geräten)<br>PC (bei Computern, die vom Intune-PC-Agent verwaltet werden) | (device.managementType -eq "MDM")
 organizationalUnit | ein beliebiger Zeichenfolgenwert, der mit dem Namen der Organisationseinheit, die von einem lokalen Active Directory festgelegt wurde, übereinstimmt | (device.organizationalUnit -eq "US PCs")
 deviceId | eine gültige Azure AD-Geräte-ID | (device.deviceId -eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectId | eine gültige Azure AD-Objekt-ID |  (device.objectId -eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")




## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Gruppen in Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

