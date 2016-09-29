<properties
   pageTitle="Entwurfskonzepte für Azure AD Connect | Microsoft Azure"
   description="In diesem Thema werden bestimmte Aspekte des Implementierungsentwurfs beschrieben."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="andkjell"/>

# Azure AD Connect: Designkonzepte
Dieses Themas beschreibt, welche Aspekte bei der Planung der Implementierung von Azure AD Connect berücksichtigt werden müssen. Bestimmte Aspekte werden in diesem Thema sehr gründlich behandelt, und diese Konzepte werden in anderen Themen ebenfalls kurz beschrieben.

## sourceAnchor
Das Attribut sourceAnchor ist definiert als *Attribut, das während der Lebensdauer eines Objekts unveränderlich ist*. Es identifiziert ein Objekt eindeutig als in Azure AD und lokal identisch. Das Attribut wird auch als **immutableId** bezeichnet, und die beiden Namen werden austauschbar verwendet.

Das Wort „unveränderlich“ ist in diesem Thema wichtig. Da der Wert dieses Attributs nicht geändert werden kann, nachdem es festgelegt wurde, ist es wichtig, einen Entwurf auszuwählen, der Ihr Szenario unterstützt.

Das Attribut wird für die folgenden Szenarien verwendet:

- Wenn Sie einen neuen Synchronisierungsmodulserver erstellen oder nach einem Notfallwiederherstellungsszenario erneut erstellen, verknüpft dieses Attribut vorhandene Objekte in Azure AD mit lokalen Objekten.
- Wenn Sie von einer ausschließlichen Cloudidentität zu einem synchronisierten Identitätsmodell wechseln, ermöglicht dieses Attribut die genaue Übereinstimmung vorhandener Objekte in Azure AD mit lokalen Objekten.
- Bei Verwendung des Verbunds wird dieses Attribut zusammen mit **userPrincipalName** im Anspruch zur eindeutigen Identifizierung eines Benutzers verwendet.

In diesem Thema wird nur „sourceAnchor“ behandelt, da sich das Element auf Benutzer bezieht. Die gleichen Regeln gelten für alle Objekttypen, doch üblicherweise ist dieses Problem nur für Benutzer von besonderem Belang.

### Auswählen eines guten Attributs sourceAnchor
Der Attributwert muss den folgenden Regeln entsprechen:

- Weniger als 60 Zeichen lang
    - Zeichen, bei denen es sich nicht um a-z, A-Z oder 0-9 handelt, werden als drei Zeichen codiert und gezählt.
- Keine Sonderzeichen: &#92; ! # $ % & * + / = ? ^ &#96; { }| ~ < > ( ) ' ; : , [ ] " @ _
- Global eindeutig
- Zeichenfolge, Ganzzahl oder Binärzahl
- Sollte nicht auf einem Benutzernamen beruhen, da dieser geändert werden kann.
- Groß-/Kleinschreibung sollte nicht relevant sein und Werte, die sich nach Groß-/Kleinschreibung unterscheiden, sollten vermieden werden.
- Sollte bei Erstellung des Objekts zugewiesen werden.

Ist das ausgewählte Attribut „sourceAnchor“ nicht vom Typ „Zeichenfolge“, unterzieht Azure AD Connect den Wert des Attributs einem Base64Encode-Prozess, um sicherzustellen, dass keine Sonderzeichen angezeigt werden. Wenn Sie einen anderen Verbundserver als AD FS verwenden, stellen Sie sicher, dass Ihr Server auch in der Lage ist, das Attribut einem Base64Encode-Prozess zu unterziehen.

Beim Attribut „sourceAnchor“ wird die Groß-/Kleinschreibung berücksichtigt. Der Wert "JohnDoe" ist nicht identisch mit "johndoe". Sie sollten nicht zwei verschiedene Objekte besitzen, bei denen sich nur die Groß-/Kleinschreibung unterscheidet.

Wenn Sie eine einzige lokale Gesamtstruktur haben, sollten Sie das Attribut **objectGUID** verwenden. Dieses Attribut wird auch bei der Verwendung von Expresseinstellungen in Azure AD Connect und von DirSync verwendet.

Wenn Sie mehrere Gesamtstrukturen besitzen und keine Benutzer zwischen Gesamtstrukturen und Domänen verschieben, dann ist **objectGUID** ebenfalls ein ideales Attribut.

Wenn Sie Benutzer zwischen Gesamtstrukturen und Domänen verschieben, müssen Sie ein Attribut finden, das nicht geändert wird oder während des Verschiebevorgangs zusammen mit den Benutzern verschoben werden kann. Ein empfohlenes Verfahren ist die Einführung eines synthetischen Attributs. Ein Attribut, das etwas Ähnliches wie eine GUID enthält, wäre geeignet. Beim Erstellen des Objekts wird eine neue GUID erstellt und dem Benutzer zugewiesen. Im Synchronisierungsmodulserver kann eine benutzerdefinierte Synchronisierungsregel erstellt werden, um diesen Wert basierend auf **objectGUID** zu erstellen und das ausgewählte Attribut in ADDS zu aktualisieren. Wenn Sie das Objekt verschieben, stellen Sie sicher, dass Sie auch den Inhalt dieses Werts kopieren.

Eine andere Lösung ist, ein vorhandenes Attribut zu wählen, von dem Sie wissen, dass es nicht geändert wird. Zu den häufig verwendeten Attributen zählt **employeeID**. Wenn Sie ein Attribut in Betracht ziehen, das Buchstaben enthält, stellen Sie sicher, dass keine Möglichkeit besteht, dass sich die Groß-/Kleinschreibung für den Wert des Attributs ändern kann. Zu schlechten Attributen, die nicht verwendet werden sollten, gehören Attribute mit dem Namen des Benutzers. Durch Hochzeit oder Scheidung könnte sich der Name ändern, und dies ist für dieses Attribut nicht zulässig. Dies ist auch ein Grund, warum Attribute wie **userPrincipalName**, **mail** und **targetAddress** im Installations-Assistenten von Azure AD Connect nicht einmal ausgewählt werden können. Diese Attribute enthalten außerdem das @-Zeichen, das in „sourceAnchor“ nicht zulässig ist.

### Ändern des Attributs sourceAnchor
Der Wert des Attributs sourceAnchor kann nicht geändert werden, nachdem das Objekt in Azure AD erstellt und die Identität synchronisiert wurde.

Aus diesem Grund gelten die folgenden Einschränkungen für Azure AD Connect:

- Das Attribut sourceAnchor kann nur bei der Erstinstallation festgelegt werden. Diese Option ist schreibgeschützt, wenn Sie den Installations-Assistenten erneut ausführen. Wenn Sie diese Einstellung ändern müssen, müssen Sie deinstallieren und neu installieren.
- Wenn Sie einen anderen Azure AD Connect-Server installieren, müssen Sie das gleiche Attribut sourceAnchor wie zuvor auswählen. Wenn Sie zuvor bereits DirSync verwendet haben und zu Azure AD wechseln, müssen Sie **objectGUID** verwenden, da dieses Attribut von DirSync verwendet wird.
- Wenn der Wert für „sourceAnchor“ geändert wird, nachdem das Objekt nach Azure AD exportiert wurde, meldet die Azure AD Connect-Synchronisierung einen Fehler und lässt keine weiteren Änderungen am Objekt zu, bevor das Problem behoben und die Änderung von „sourceAnchor2 im Quellverzeichnis wieder rückgängig gemacht wurde.

## Azure AD-Anmeldung
Bei der Integration Ihres lokalen Verzeichnisses in Azure AD ist es wichtig zu wissen, wie sich die Synchronisierungseinstellungen auf die Art und Weise auswirken, in der ein Benutzer sich authentifiziert. Azure AD verwendet einen Benutzerprinzipalname (User Principal Name, UPN, ), um den Benutzer zu authentifizieren. Wenn Sie jedoch Ihre Benutzer synchronisieren, müssen Sie das Attribut, das als Wert für „userPrincipalName“ verwendet werden soll, sehr sorgfältig auswählen.

### Auswählen des Attributs für userPrincipalName
Bei Auswahl des Attributs, das den in Azure zu verwendenden UPN-Wert bereitstellt, sollten Sie Folgendes sicherstellen:

- Die Attributwerte entsprechen der UPN-Syntax (RFC 822) und müssen somit im Format username@domain vorliegen.
- Das Suffix in den Werten stimmt mit einer der überprüften benutzerdefinierten Domänen in Azure AD überein.

In den Expresseinstellungen wird userPrincipalName für das Attribut angenommen. Falls das userPrincipalName-Attribut nicht den Wert enthält, den Ihre Benutzer zum Anmelden bei Azure verwenden sollen, müssen Sie **Benutzerdefinierte Installation** wählen.

### Benutzerdefinierter Domänenstatus und UPN
Sie müssen unbedingt sicherstellen, dass eine überprüfte Domäne für das UPN-Suffix existiert.

John ist ein Benutzer in contoso.com. John soll den lokalen UPN john@contoso.com für die Anmeldung bei Azure verwenden, nachdem Sie die Benutzer mit Ihrem Azure AD-Verzeichnis „contoso.onmicrosoft.com“ synchronisiert haben. Zu diesem Zweck müssen Sie „contoso.com“ als benutzerdefinierte Domäne in Azure AD hinzufügen und überprüfen, bevor Sie mit dem Synchronisieren der Benutzer beginnen können. Wenn das UPN-Suffix von John (beispielsweise „contoso.com“) mit keiner überprüften Domäne in Azure übereinstimmt, ersetzt Azure AD das UPN-Suffix durch „contoso.onmicrosoft.com“.

### Nicht routingfähige lokale Domänen und UPN für Azure AD
Einige Unternehmen verfügen über nicht routingfähige Domänen, z.B. „contoso.local“, oder einteilige Domänen, z.B. „contoso“. Sie können eine nicht routingfähige Domäne in Azure AD nicht überprüfen. Azure AD Connect kann Synchronisierungen nur mit einer überprüften Domäne in Azure AD durchführen. Wenn Sie ein Azure AD-Verzeichnis erstellen, wird eine routingfähige Domäne erstellt, die zur Standarddomäne für Ihre Azure AD-Instanz wird, z.B. „contoso.onmicrosoft.com“. Daher ist es notwendig, in einem solchen Szenario alle anderen routingfähigen Domänen zu überprüfen, wenn Sie Synchronisierungen nicht mit der standardmäßigen Domäne „onmicrosoft.com“ durchführen möchten.

Unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-add-domain.md) finden Sie weitere Informationen zum Hinzufügen und Überprüfen von Domänen.

Azure AD Connect erkennt, ob Sie eine nicht routingfähige Domänenumgebung ausführen, und warnt Sie entsprechend davor, nicht mit den Expresseinstellungen fortzufahren. Wenn Sie sich in einer nicht routingfähigen Domäne befinden, ist es wahrscheinlich, dass auch der UPN der Benutzer nicht routingfähige Suffixe aufweist. Wenn Sie beispielsweise „contoso.local“ verwenden, empfiehlt Azure AD Connect die Verwendung von benutzerdefinierten Einstellungen anstatt der Expresseinstellungen. Indem Sie benutzerdefinierte Einstellungen verwenden, können Sie das Attribut angeben, das als UPN für die Anmeldung bei Azure verwendet werden soll, nachdem die Benutzer mit Azure AD synchronisiert wurden.

## Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0914_2016-->