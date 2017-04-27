---
title: "Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory | Microsoft Docs"
description: "Erfahren Sie, wie Ausdruckszuordnungen verwendet werden können, um Attributwerte während der automatisierten Bereitstellung von SaaS-App-Objekten in Azure Active Directory in ein akzeptables Format zu transformieren."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: d2d7f5b00039cd1beab009b78b273ec4dffaab47
ms.lasthandoff: 04/06/2017


---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory
Wenn Sie die Bereitstellung für eine SaaS-Anwendung konfigurieren, ist einer der Attributzuordnungstypen, die Sie angeben können, eine Ausdruckszuordnung. Für diese müssen Sie einen skriptartigen Ausdruck schreiben, mit dem Sie die Daten Ihrer Benutzer in Formate umwandeln können, die für die SaaS-Anwendung einfacher zu akzeptieren sind.

## <a name="syntax-overview"></a>Syntaxübersicht
Die Syntax für die Ausdrücke für Attributzuordnungen ist den Funktionen von Visual Basic for Applications (VBA) ähnlich.

* Der gesamte Ausdruck muss mittels Funktionen definiert werden, die aus einem Namen mit darauffolgenden Argumenten in Klammern bestehen:  <br>
  *Funktionsname(<<Argument 1>>,<<argument N>>)*
* Sie können Funktionen ineinander verschachteln. Beispiel: <br> *FunktionEins(FunktionZwei(<<argument1>>))*
* Sie können drei verschiedene Argumententypen an die Funktionen übergeben:
  
  1. Attribute, die in eckige Klammern eingeschlossen werden müssen. Beispiel: [Attributname]
  2. Zeichenfolgenkonstanten, die in doppelte Anführungszeichen eingeschlossen werden müssen. Beispiel: "USA"
  3. Andere Funktionen Beispiel: FunktionEins(<<argument1>>, FunktionZwei(<<argument2>>))
* Bei Zeichenfolgenkonstanten, in denen ein umgekehrter Schrägstrich ( \ ) oder ein Anführungszeichen ( " ) benötigt wird, muss dieser bzw. dieses mit einem umgekehrten Schrägstrichsymbol ( \ ) versehen werden. Beispiel: "Firmenname: \"Contoso\""

## <a name="list-of-functions"></a>Liste der Funktionen
[Anfügen](#append) &nbsp;&nbsp;&nbsp;&nbsp; [FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp;&nbsp;&nbsp; [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)

- - -
### <a name="append"></a>Anfügen
**Funktion:**<br> Append(Quelle, Suffix)

**Beschreibung:**<br> Fügt das Suffix am Ende eines angegebenen Quellzeichenfolgenwerts an.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |String |Normalerweise der Name des Attributs aus dem Quellobjekt |
| **Suffix** |Erforderlich |String |Die Zeichenfolge, die Sie am Ende des Quellwerts anfügen möchten |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Funktion:**<br> FormatDateTime(Quelle, Eingabeformat, Ausgabeformat)

**Beschreibung:**<br> Konvertiert eine Datumszeichenfolge aus einem Format in ein anderes Format.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |String |Normalerweise der Name des Attributs aus dem Quellobjekt |
| **Eingabeformat** |Erforderlich |String |Erwartetes Format des Quellwerts. Unterstützte Formate finden Sie unter [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **Ausgabeformat** |Erforderlich |String |Format des Ausgabedatums. |

- - -
### <a name="join"></a>Join
**Funktion:**<br> Join(Trennzeichen, Quelle1, Quelle2, …)

**Beschreibung:**<br> „Join()“ ist vergleichbar mit „Append()“, kann jedoch mehrere **Quellzeichenfolgenwerte** in einer einzelnen Zeichenfolge kombinieren, wobei die Werte jeweils durch eine **Trennzeichenfolge** getrennt werden.

Wenn einer der Quellwerte ein mehrwertiges Attribut ist, werden die einzelnen Werte in diesem Attribut miteinander verknüpft und dabei durch den Trennzeichenwert getrennt.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Trennzeichen** |Erforderlich |String |Zeichenfolge, die zur Trennung von Quellwerten verwendet wird, wenn diese zu einer einzelnen Zeichenfolge zusammengesetzt werden. Kann "" sein, wenn kein Trennzeichen erforderlich ist. |
| **Quelle1  … QuelleN ** |Erforderlich, unterschiedlich oft |String |Zeichenfolgenwerte, die zusammengesetzt werden sollen. |

- - -
### <a name="mid"></a>Mid
**Funktion:**<br> Mid(Quelle, Start, Länge)

**Beschreibung:**<br> Gibt eine Teilzeichenfolge des Quellwerts zurück. Eine Teilzeichenfolge ist eine Zeichenfolge, die nur einige der Zeichen aus der Quellzeichenfolge enthält.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |String |Normalerweise der Name des Attributs. |
| **start** |Erforderlich |Ganze Zahl |Index in der **Quellzeichenfolge** , an dem die Teilzeichenfolge beginnen soll. Das erstes Zeichen in der Zeichenfolge hat den Index 1, das zweite Zeichen hat den Index 2 usw. |
| **Länge** |Erforderlich |Ganze Zahl |Die Länge der Teilzeichenfolge. Wenn die Länge außerhalb der **Quellzeichenfolge** endet, gibt die Funktion die Teilzeichenfolge zwischen **Startindex** und dem Ende der **Quellzeichenfolge** zurück. |

- - -
### <a name="not"></a>not
**Funktion:**<br> Not(Quelle)

**Beschreibung:**<br> Kehrt den booleschen Wert der **Quelle** um. Lautet der **Quellwert** also *True*, gibt die Funktion *False* zurück. Andernfalls gibt sie "*True*" zurück.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |Boolesche Zeichenfolge |Die erwarteten **Quellwerte** sind "True" oder "False". |

- - -
### <a name="replace"></a>Ersetzen von
**Funktion:**<br> ObsoleteReplace(Quelle, AlterWert, RegexMuster, RegexGruppenname, Ersatzwert, Ersatzattributname, Vorlage)

**Beschreibung:**<br>
Ersetzt Werte in einer Zeichenfolge. Sie funktioniert unterschiedlich, je nachdem, welche Parameter angegeben werden:

* Bei Angabe von **AlterWert** und **Ersatzwert**:
  
  * Ersetzt alle Vorkommen von „AlterWert“ in der Quelle durch „Ersatzwert“.
* Bei Angabe von **AlterWert** und **Vorlage**:
  
  * Ersetzt alle Vorkommen von **AlterWert** in **Vorlage** durch den **Quellwert**).
* Bei Angabe von **AlterWertRegexMuster**, **AlterWertRegexGruppenname** und **Ersatzwert**:
  
  * Ersetzt alle Werte, die mit dem "AlterWertRegexMuster" in der Quellzeichenfolge übereinstimmen, durch den "Ersatzwert".
* Bei Angabe von **AlterWertRegexMuster**, **AlterWertRegexGruppenname** und **Ersatzeigenschaftsname**:
  
  * Falls ein Wert für **Quelle** vorhanden ist, wird der **Quellwert** zurückgegeben.
  * Ist kein Wert für **Quelle** vorhanden, wird der Ersatzwert aus der Eigenschaft mit **Ersatzeigenschaftsname** unter Verwendung von **AlterWertRegexMuster** und **AlterWertRegexGruppenname** extrahiert. Der Ersatzwert wird als Ergebnis zurückgegeben.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |String |Normalerweise der Name des Attributs aus dem Quellobjekt |
| **AlterWert** |Optional |String |Wert, der in **Quelle** oder **Vorlage** ersetzt werden soll. |
| **RegexMuster** |Optional |String |Regex-Muster für den Wert, der in der **Quelle**ersetzt wird. Wenn "Ersatzeigenschaftsname" verwendet wird, das Muster, das zum Extrahieren des Werts aus der Ersatzeigenschaft verwendet wird. |
| **RegexGruppenname** |Optional |String |Name der Gruppe im **RegexMuster**. Nur bei Verwendung von „Ersatzeigenschaftsname“ wird der Wert dieser Gruppe als „Ersatzwert“ aus der Ersatzeigenschaft extrahiert. |
| **Ersatzwert** |Optional |String |Neuer Wert, durch den der alte Wert ersetzt wird. |
| **Ersatzattributname** |Optional |String |Name des Attributs, das für den Ersatzwert verwendet werden soll, wenn die Quelle keinen Wert besitzt. |
| **Vorlage** |Optional |String |Bei Angabe des Werts **Vorlage** wird **AlterWert** in der Vorlage gesucht und durch den Quellwert ersetzt. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Funktion:**<br> StripSpaces(Quelle)

**Beschreibung:**<br> Entfernt alle Leerzeichen (" ") aus der Quellzeichenfolge.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |String |**Quelle** , der aktualisiert werden soll. |

- - -
### <a name="switch"></a>Switch
**Funktion:**<br> Switch(Quelle, Standardwert, Schlüssel1, Wert1, Schlüssel2, Wert2, …)

**Beschreibung:**<br> Wenn der **Quellwert** einem **Schlüssel** entspricht, wird der **Wert** für diesen **Schlüssel** zurückgegeben. Wenn der **Quellwert** keinem Schlüssel entspricht, wird der **Standardwert** zurückgegeben.  **Schlüssel-** und **Wertparameter** müssen immer paarweise angegeben werden. Die Funktion erwartet immer eine gerade Anzahl von Parametern.

**Parameter:**<br> 

| Name | Erforderlich/wiederholt | Typ | Hinweise |
| --- | --- | --- | --- |
| **Quelle** |Erforderlich |String |**Source** , der aktualisiert werden soll. |
| **defaultValue** |Optional |String |Der Standardwert, der verwendet werden soll, wenn die Quelle mit keinem Schlüssel übereinstimmt. Kann eine leere Zeichenfolge ("") sein. |
| **key** |Erforderlich |String |**Schlüssel**, der mit dem **Quellwert** verglichen werden soll. |
| **value** |Erforderlich |String |Der Ersatzwert für die **Quelle** , die mit dem Schlüssel übereinstimmt. |

## <a name="examples"></a>Beispiele
### <a name="strip-known-domain-name"></a>Entfernen eines bekannten Domänennamens
Sie müssen einen bekannten Domänennamen aus der E-Mail-Adresse eines Benutzers entfernen, um einen Benutzernamen zu erhalten. <br>
Wenn die Domäne beispielsweise "contoso.com" lautet, können Sie den folgenden Ausdruck verwenden:

**Ausdruck:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Beispieleingabe/-ausgabe:** <br>

* **EINGABE** (mail): "john.doe@contoso.com"
* **AUSGABE**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Anfügen eines konstanten Suffixes an einen Benutzernamen
Wenn Sie eine Salesforce Sandbox verwenden, müssen Sie möglicherweise ein weiteres Suffix an alle Benutzernamen anfügen, bevor Sie diese synchronisieren.

**Ausdruck:** <br>
`Append([userPrincipalName], ".test"))`

**Beispieleingabe/-ausgabe:** <br>

* **EINGABE**: (userPrincipalName): "John.Doe@contoso.com"
* **AUSGABE**:  "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generieren eines Benutzeralias durch Verketten von Teilen des Vor- und Nachnamens
Sie müssen einen Benutzeralias generieren, indem Sie die ersten drei Buchstaben des Vornamens und die ersten fünf Buchstaben des Nachnamens des Benutzers verwenden.

**Ausdruck:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Beispieleingabe/-ausgabe:** <br>

* **EINGABE** (givenName): "John"
* **EINGABE** (surname): "Doe"
* **AUSGABE**: "JohDoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Ausgabedatum eines Datums als Zeichenfolge in einem bestimmten Format
Sie möchten Datumsangaben in einem bestimmten Format an eine SaaS-Anwendung senden. <br>
Beispielsweise möchten Sie Datumsangaben für ServiceNow formatieren.

**Ausdruck:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Beispieleingabe/-ausgabe:**

* **EINGABE** (extensionAttribute1): "20150123105347.1Z"
* **AUSGABE**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Ersetzen eines Werts anhand eines vordefinierten Satzes von Optionen
Sie müssen die Zeitzone des Benutzers anhand des Bundesstaatscodes festlegen, der in Azure AD gespeichert ist. <br>
Wenn der Bundesstaatscode keiner der vordefinierten Optionen entspricht, soll der Standardwert "Australien/Sydney" verwendet werden.

**Ausdruck:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Beispieleingabe/-ausgabe:**

* **EINGABE** (state): "QLD"
* **AUSGABE**: "Australien/Brisbane"

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Automatisieren der Bereitstellung/Bereitstellungsaufhebung von Benutzern für SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Kontobereitstellungsbenachrichtigungen](active-directory-saas-account-provisioning-notifications.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)


