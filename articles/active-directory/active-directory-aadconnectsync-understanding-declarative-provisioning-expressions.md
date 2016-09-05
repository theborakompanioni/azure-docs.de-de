<properties
	pageTitle="Azure AD Connect-Synchronisierung: Grundlegendes zu Ausdrücken für die deklarative Bereitstellung | Microsoft Azure"
	description="Erläutert die Ausdrücke für die deklarative Bereitstellung."
	services="active-directory"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/23/2016"
	ms.author="markusvi;andkjell"/>


# Azure AD Connect-Synchronisierung: Grundlegendes zu Ausdrücken für die deklarative Bereitstellung
Die Azure AD Connect-Synchronisierung basiert auf der deklarativen Bereitstellung, die erstmals in Forefront Identity Manager 2010 eingeführt wurde. Sie ermöglicht Ihnen die Implementierung Ihrer gesamten Geschäftslogik zur Identitätsintegration, ohne kompilierten Code schreiben zu müssen.

Ein wesentlicher Bestandteil der deklarativen Bereitstellung ist die in den Attributflüssen verwendete Ausdruckssprache. Die verwendete Sprache ist eine Teilmenge von Microsoft ® Visual Basic ® for Applications (VBA). Diese Sprache wird in Microsoft Office verwendet, und Benutzer mit Erfahrungen mit VBScript werden sie wiedererkennen. Die Ausdruckssprache für die deklarative Bereitstellung verwendet nur Funktionen und ist keine strukturierte Sprache. Es gibt keine Methoden oder Anweisungen. Funktionen werden stattdessen geschachtelt, um den Programmablauf auszudrücken.

Weitere Informationen finden Sie unter [Willkommen bei der VBA-Sprachreferenz für Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Die Attribute sind stark typisiert. Eine Funktion akzeptiert nur Attribute des richtigen Typs. Zudem muss die Groß-/Kleinschreibung beachtet werden. Sowohl bei Funktions- als auch Attributnamen muss die Groß-/Kleinschreibung korrekt sein. Andernfalls wird ein Fehler ausgegeben.

## Sprachdefinitionen und Bezeichner

- Funktionen verfügen über einen Namen, gefolgt von Argumenten in Klammern: FunctionName(argument 1,argument N).
- Attribute werden durch eckige Klammern gekennzeichnet: [attributeName].
- Parameter werden durch Prozentzeichen gekennzeichnet: %ParameterName%.
- Zeichenfolgenkonstanten werden in Anführungszeichen eingeschlossen, beispielsweise "Contoso". Hierbei müssen gerade Anführungszeichen ("") verwendet werden, typografische Anführungszeichen („”) sind nicht zulässig.
- Numerische Werte werden ohne Anführungszeichen ausgedrückt und im Dezimalformat vorliegen. Hexadezimalwerten weisen das Präfix "&H" auf. Beispiel: 98052, &HFF.
- Boolesche Werte werden mit Konstanten ausgedrückt: True, False.
- Integrierte Konstanten und Literale werden nur mit ihrem Namen ausgedrückt: NULL, CRLF, IgnoreThisFlow.

### Functions
Bei der deklarativen Bereitstellung werden viele Funktionen verwendet, um das Transformieren von Attributwerten zu ermöglichen. Diese Funktionen können geschachtelt werden, sodass das Ergebnis einer Funktion an eine andere Funktion übergeben wird.

`Function1(Function2(Function3()))`

Die vollständige Liste der Funktionen finden Sie in der [Funktionsreferenz](active-directory-aadconnectsync-functions-reference.md).

### Parameter
Ein Parameter wird entweder durch einen Connector oder einen Administrator unter Verwendung von PowerShell definiert. Parameter enthalten üblicherweise Werte, die sich je nach System unterscheiden, z.B. der Name der Domäne, in der sich der Benutzer befindet. Diese Parameter können in Attributflüssen verwendet werden.

Der Active Directory Connector stellt folgende Parameter für eingehende Synchronisierungsregeln bereit:

| Parametername | Kommentar |
| --- | --- |
| Domain.Netbios | NetBIOS-Format der Domäne, die gerade importiert wird, z.B. „FABRIKAMSALES“ |
| Domain.FQDN | FQDN-Format der Domäne, die gerade importiert wird, z.B. „sales.fabrikam.com“ |
| Domain.LDAP | LDAP-Format der Domäne, die gerade importiert wird, z.B. „DC=sales,DC=fabrikam,DC=com“ |
| Forest.Netbios | NetBIOS-Format des Gesamtstrukturnamens, der gerade importiert wird, z.B. „FABRIKAMCORP“ |
| Forest.FQDN | FQDN-Format des Gesamtstrukturnamens, der gerade importiert wird, z.B. „fabrikam.com“ |
| Forest.LDAP | LDAP-Format des Gesamtstrukturnamens, der gerade importiert wird, z.B. „DC=fabrikam,DC=com“ |

Das System stellt den folgenden Parameter bereit, mit dem der Bezeichner des derzeit ausgeführten Connectors abgerufen wird: `Connector.ID`

Hier sehen Sie ein Beispiel, in dem die Metaverseattributdomäne mit dem NetBIOS-Namen der Domäne aufgefüllt wird, in der sich der Benutzer befindet: `domain` <- `%Domain.Netbios%`

### Operatoren
Folgende Operatoren können verwendet werden:

- **Vergleich**: <, <=, <>, =, >, >=
- **Mathematik**: +, -, *, -
- **Zeichenfolge**: & (concatenate)
- **Logischer Ausdruck**: && (and), || (or)
- **Auswertungsreihenfolge**: ( )

Operatoren werden von links nach rechts ausgewertet und haben bei der Auswertung die gleiche Priorität. Dies bedeutet, dass der Multiplikator (*) nicht vor der Subtraktion (-) ausgewertet wird. „2*(5+3)“ ist nicht dasselbe wie „2*5+3“. Die Klammern werden verwendet, um die Reihenfolge der Auswertung zu ändern, wenn die Auswertungsreihenfolge von links nach rechts nicht geeignet ist.

## Mehrwertige Attribute

### Attributflüsse für mehrwertige Attribute
Die Funktionen können sowohl für einwertige als auch für mehrwertige Attribute verwendet werden. Bei mehrwertigen Attributen wird die Funktion für jeden Wert ausgeführt, und auf alle Werte wird die gleiche Funktion angewendet.

Beispiel: `Trim([proxyAddresses])` – Führt eine Trim-Funktion für jeden Wert im Attribut „proxyAddress“ aus. `Word([proxyAddresses],1,"@") & "@contoso.com"` – Ersetzt für jeden Wert mit dem @-Zeichen die Domäne durch „@contoso.com“. `IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` – Sucht nach der SIP-Adresse und entfernt sie aus den Werten.

### Zusammenführen von Attributwerten
In den Attributflüssen ist eine Einstellung verfügbar, mit der Sie ermitteln können, ob mehrwertige Attribute aus mehreren verschiedenen Connectors zusammengeführt werden sollten. Beim Standardwert **Update** wird die Synchronisierungsregel mit der höchsten Rangfolge angewendet.

![Zusammenführungstypen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions/mergetype.png)

Weitere Einstellungen sind **Merge** und **MergeCaseInsensitive**. Mit diesen Optionen können Sie Werte aus unterschiedlichen Quellen zusammenführen. Sie können beispielsweise das Attribut „member“ oder „proxyAddresses“ aus mehreren unterschiedlichen Gesamtstrukturen zusammenführen. Wenn Sie diese Option verwenden, müssen alle Synchronisierungsregeln im Bereich für ein Objekt denselben Zusammenführungstyp aufweisen. Es ist nicht möglich, den Zusammenführungstyp **Update** für einen Connector und **Merge** für einen anderen festzulegen. Wenn Sie es versuchen, wird eine Fehlermeldung ausgegeben.

Der Unterschied zwischen **Merge** und **MergeCaseInsensitive** ist die Verarbeitung doppelter Attributwerte. Das Synchronisierungsmodul stellt sicher, dass keine doppelten Werte in das Zielattribut eingefügt werden. Bei **MergeCaseInsensitive** werden auch keine doppelten Werte eingefügt, bei denen sich nur die Groß-/Kleinschreibung unterscheidet. Beispielsweise werden nicht sowohl „SMTP:bob@contoso.com“ als auch „smtp:bob@contoso.com“ im Zielattribut vorhanden sein. **Merge** überprüft nur die genauen Werte. Daher ist es möglich, dass mehrere Werte vorhanden sind, bei denen sich nur die Groß-/Kleinschreibung unterscheidet.

Die Option **Replace** entspricht **Update**, wird aber nicht verwendet.

## Weitere Ressourcen

[Azure AD Connect-Synchronisierung: Funktionsreferenz](active-directory-aadconnectsync-functions-reference.md) [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md) [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0824_2016-->