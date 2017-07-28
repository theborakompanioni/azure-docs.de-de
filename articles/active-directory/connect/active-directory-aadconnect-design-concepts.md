---
title: "Entwurfskonzepte für Azure AD Connect | Microsoft Docs"
description: In diesem Thema werden bestimmte Aspekte des Implementierungsentwurfs beschrieben.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: bfb41f254d74bef843461a058ee5b2ced7fc45ec
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017

---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Designkonzepte
Dieses Themas beschreibt, welche Aspekte bei der Planung der Implementierung von Azure AD Connect berücksichtigt werden müssen. Bestimmte Aspekte werden in diesem Thema sehr gründlich behandelt, und diese Konzepte werden in anderen Themen ebenfalls kurz beschrieben.

## <a name="sourceanchor"></a>sourceAnchor
Das Attribut sourceAnchor ist definiert als *Attribut, das während der Lebensdauer eines Objekts unveränderlich ist*. Es identifiziert ein Objekt eindeutig als in Azure AD und lokal identisch. Das Attribut wird auch als **immutableId** bezeichnet, und die beiden Namen werden austauschbar verwendet.

Das Wort „unveränderlich“ ist in diesem Thema wichtig. Da der Wert dieses Attributs nicht geändert werden kann, nachdem es festgelegt wurde, ist es wichtig, einen Entwurf auszuwählen, der Ihr Szenario unterstützt.

Das Attribut wird für die folgenden Szenarien verwendet:

* Wenn Sie einen neuen Synchronisierungsmodulserver erstellen oder nach einem Notfallwiederherstellungsszenario erneut erstellen, verknüpft dieses Attribut vorhandene Objekte in Azure AD mit lokalen Objekten.
* Wenn Sie von einer ausschließlichen Cloudidentität zu einem synchronisierten Identitätsmodell wechseln, ermöglicht dieses Attribut die genaue Übereinstimmung vorhandener Objekte in Azure AD mit lokalen Objekten.
* Bei Verwendung des Verbunds wird dieses Attribut zusammen mit **userPrincipalName** im Anspruch zur eindeutigen Identifizierung eines Benutzers verwendet.

In diesem Thema wird nur „sourceAnchor“ behandelt, da sich das Element auf Benutzer bezieht. Die gleichen Regeln gelten für alle Objekttypen, doch üblicherweise ist dieses Problem nur für Benutzer von besonderem Belang.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Auswählen eines guten Attributs sourceAnchor
Der Attributwert muss den folgenden Regeln entsprechen:

* Weniger als 60 Zeichen lang
  * Zeichen, bei denen es sich nicht um a-z, A-Z oder 0-9 handelt, werden als drei Zeichen codiert und gezählt.
* Keine Sonderzeichen: &#92; ! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Global eindeutig
* Zeichenfolge, Ganzzahl oder Binärzahl
* Sollte nicht auf einem Benutzernamen beruhen, da dieser geändert werden kann.
* Groß-/Kleinschreibung sollte nicht relevant sein und Werte, die sich nach Groß-/Kleinschreibung unterscheiden, sollten vermieden werden.
* Sollte bei Erstellung des Objekts zugewiesen werden.

Ist das ausgewählte Attribut „sourceAnchor“ nicht vom Typ „Zeichenfolge“, unterzieht Azure AD Connect den Wert des Attributs einem Base64Encode-Prozess, um sicherzustellen, dass keine Sonderzeichen angezeigt werden. Wenn Sie einen anderen Verbundserver als AD FS verwenden, stellen Sie sicher, dass Ihr Server auch in der Lage ist, das Attribut einem Base64Encode-Prozess zu unterziehen.

Beim Attribut „sourceAnchor“ wird die Groß-/Kleinschreibung berücksichtigt. Der Wert "JohnDoe" ist nicht identisch mit "johndoe". Sie sollten nicht zwei verschiedene Objekte besitzen, bei denen sich nur die Groß-/Kleinschreibung unterscheidet.

Wenn Sie eine einzige lokale Gesamtstruktur haben, sollten Sie das Attribut **objectGUID**verwenden. Dieses Attribut wird auch bei der Verwendung von Expresseinstellungen in Azure AD Connect und von DirSync verwendet.

Wenn Sie mehrere Gesamtstrukturen besitzen und keine Benutzer zwischen Gesamtstrukturen und Domänen verschieben, dann ist **objectGUID** ebenfalls ein ideales Attribut.

Wenn Sie Benutzer zwischen Gesamtstrukturen und Domänen verschieben, müssen Sie ein Attribut finden, das nicht geändert wird oder während des Verschiebevorgangs zusammen mit den Benutzern verschoben werden kann. Ein empfohlenes Verfahren ist die Einführung eines synthetischen Attributs. Ein Attribut, das etwas Ähnliches wie eine GUID enthält, wäre geeignet. Beim Erstellen des Objekts wird eine neue GUID erstellt und dem Benutzer zugewiesen. Im Synchronisierungsmodulserver kann eine benutzerdefinierte Synchronisierungsregel erstellt werden, um diesen Wert basierend auf **objectGUID** zu erstellen und das ausgewählte Attribut in ADDS zu aktualisieren. Wenn Sie das Objekt verschieben, stellen Sie sicher, dass Sie auch den Inhalt dieses Werts kopieren.

Eine andere Lösung ist, ein vorhandenes Attribut zu wählen, von dem Sie wissen, dass es nicht geändert wird. Zu den häufig verwendeten Attributen zählt **employeeID**. Wenn Sie ein Attribut in Betracht ziehen, das Buchstaben enthält, stellen Sie sicher, dass keine Möglichkeit besteht, dass sich die Groß-/Kleinschreibung für den Wert des Attributs ändern kann. Zu schlechten Attributen, die nicht verwendet werden sollten, gehören Attribute mit dem Namen des Benutzers. Durch Hochzeit oder Scheidung könnte sich der Name ändern, und dies ist für dieses Attribut nicht zulässig. Dies ist auch ein Grund, warum Attribute wie **userPrincipalName**, **mail** und **targetAddress** im Installations-Assistenten von Azure AD Connect nicht einmal ausgewählt werden können. Diese Attribute enthalten außerdem das „@“-Zeichen, das in „sourceAnchor“ nicht zulässig ist.

### <a name="changing-the-sourceanchor-attribute"></a>Ändern des Attributs sourceAnchor
Der Wert des Attributs sourceAnchor kann nicht geändert werden, nachdem das Objekt in Azure AD erstellt und die Identität synchronisiert wurde.

Aus diesem Grund gelten die folgenden Einschränkungen für Azure AD Connect:

* Das Attribut sourceAnchor kann nur bei der Erstinstallation festgelegt werden. Diese Option ist schreibgeschützt, wenn Sie den Installations-Assistenten erneut ausführen. Wenn Sie diese Einstellung ändern müssen, müssen Sie deinstallieren und neu installieren.
* Wenn Sie einen anderen Azure AD Connect-Server installieren, müssen Sie das gleiche Attribut sourceAnchor wie zuvor auswählen. Wenn Sie zuvor bereits DirSync verwendet haben und zu Azure AD wechseln, müssen Sie **objectGUID** verwenden, da dieses Attribut von DirSync verwendet wird.
* Wenn der Wert für „sourceAnchor“ geändert wird, nachdem das Objekt nach Azure AD exportiert wurde, meldet die Azure AD Connect-Synchronisierung einen Fehler und lässt keine weiteren Änderungen am Objekt zu, bevor das Problem behoben und die Änderung von „sourceAnchor2 im Quellverzeichnis wieder rückgängig gemacht wurde.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>Verwendung von „msDS-ConsistencyGuid“ in „sourceAnchor“
Standardmäßig verwendet Azure AD Connect (Version 1.1.486.0 und älter) „objectGUID“ als „sourceAnchor“-Attribut. „objectGUID“ wird vom System generiert. Sie können den jeweiligen Wert nicht bei der Erstellung lokaler AD-Objekte festlegen. Wie im Abschnitt [sourceAnchor](#sourceanchor) erläutert, gibt es verschiedene Szenarien, in denen Sie den Wert von „sourceAnchor“ festlegen müssen. Wenn die Szenarien auf Sie zutreffen, müssen Sie ein konfigurierbares AD-Attribut (z.B. „msDS-ConsistencyGuid“) als „sourceAnchor“-Attribut verwenden.

Azure AD Connect (Version 1.1.524.0 und höher) ermöglicht nun die Verwendung von „msDS-ConsistencyGuid“ als „sourceAnchor“-Attribut. Bei der Verwendung dieser Funktion konfiguriert Azure AD Connect die Synchronisierungsregeln automatisch wie folgt:

1. „msDS-ConsistencyGuid“ wird als „sourceAnchor“-Attribut für Benutzerobjekte verwendet. „objectGUID“ wird für andere Objekttypen verwendet.

2. Für jedes lokale AD-Benutzerobjekt, dessen „msDS-ConsistencyGuid“-Attribut nicht aufgefüllt ist, schreibt Azure AD Connect den zugehörigen Wert von „objectGUID“ in das Attribut „msDS-ConsistencyGuid“ im lokalen Active Directory zurück. Nachdem das Attribut „msDS-ConsistencyGuid“ aufgefüllt wurde, exportiert Azure AD Connect das Objekt nach Azure AD.

>[!NOTE]
> Sobald ein lokales AD-Objekt in Azure AD Connect importiert (d.h., in den AD-Connectorbereich importiert und in die Metaverse projiziert) wurde, können Sie den zugehörigen Wert von „sourceAnchor“ nicht mehr ändern. Um den Wert von „sourceAnchor“ für ein bestimmtes lokales AD-Objekt festzulegen, konfigurieren Sie das jeweilige „msDS-ConsistencyGuid“-Attribut, bevor es in Azure AD Connect importiert wird.

### <a name="permission-required"></a>Erforderliche Berechtigung
Damit diese Funktion verwendet werden kann, muss dem AD DS-Konto zum Synchronisieren mit dem lokalen Active Directory die Berechtigung zum Schreiben in das Attribut „msDS-ConsistencyGuid“ im lokalen Active Directory gewährt werden.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>Vorgehensweise: Aktivieren der „ConsistencyGuid“-Funktion - Neuinstallation
Sie können die Verwendung von „ConsistencyGuid“ als „sourceAnchor“ bei der Neuinstallation aktivieren. In diesem Abschnitt werden die Express- und die benutzerdefinierte Installation ausführlich behandelt.

  >[!NOTE]
  > Nur neuere Versionen von Azure AD Connect (1.1.524.0 und höher) unterstützen die Verwendung von „ConsistencyGuid“ als „sourceAnchor“ bei der Neuinstallation.

### <a name="how-to-enable-the-consistencyguid-feature"></a>Gewusst wie: Aktivieren der Funktion „ConsistencyGuid“
Die Funktion kann derzeit nur bei der Neuinstallation von Azure AD Connect aktiviert werden.

#### <a name="express-installation"></a>Express-Installation
Bei der Installation von Azure AD Connect mit dem Express-Modus legt der Azure AD Connect-Assistent automatisch das am besten geeignete AD-Attribut fest, das mit folgender Logik als „sourceAnchor“-Attribut verwendet werden soll:

* Zunächst fragt der Azure AD Connect-Assistent Ihren Azure AD-Mandanten ab, um das AD-Attribut abzurufen, das in der vorherigen Installation von Azure AD Connect (sofern vorhanden) als „sourceAnchor“-Attribut verwendet wurde. Sind diese Informationen verfügbar, verwendet Azure AD Connect dasselbe AD-Attribut.

  >[!NOTE]
  > Nur bei neueren Versionen von Azure AD Connect (1.1.524.0 und höher) werden Informationen über das verwendete „sourceAnchor“-Attribut in Ihrem Azure AD-Mandanten gespeichert. Bei älteren Versionen von Azure AD Connect ist dies nicht der Fall.

* Wenn keine Information zum verwendeten „sourceAnchor“-Attribut verfügbar ist, prüft der Assistent den Status des Attributs „msDS-ConsistencyGuid“ in Ihrem lokalen Active Directory. Wenn das Attribut in keinem Objekt im Verzeichnis konfiguriert ist, verwendet der Assistent „msDS-ConsistencyGuid“ als „sourceAnchor“-Attribut. Wenn das Attribut in einem oder in mehreren Objekten im Verzeichnis konfiguriert ist, folgert der Assistent daraus, dass das Attribut von anderen Anwendungen verwendet wird und nicht als „sourceAnchor“-Attribut infrage kommt.

* In diesem Fall greift der Assistent auf „objectGUID“ als „sourceAnchor“-Attribut zurück.

* Nachdem das Attribut „sourceAnchor“ festgelegt wurde, speichert der Assistent die Informationen in Ihrem Azure AD-Mandanten. Die Informationen werden für eine spätere Installation von Azure AD Connect verwendet.

Nach Abschluss der Express-Installation informiert der Assistent Sie darüber, welches Attribut als Quellankerattribut ausgewählt wurde.

![Angabe des für „sourceAnchor“ ausgewählten AD-Attributs im Assistenten](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Benutzerdefinierte Installation
Wenn Sie Azure AD Connect mit benutzerdefiniertem Modus installieren, bietet der Azure AD Connect-Assistent bei der Konfiguration des „sourceAnchor“-Attributs zwei Optionen:

![Benutzerdefinierte Installation – Konfiguration von „sourceAnchor“](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Einstellung | Beschreibung |
| --- | --- |
| Ich möchte den Quellanker durch Azure verwalten lassen | Wählen Sie diese Option aus, wenn Azure AD das Attribut für Sie auswählen soll. Wenn Sie diese Option auswählen, wendet der Azure AD Connect-Assistent dieselbe [Logik wie bei der Auswahl des Attributs „sourceAnchor“ bei der Express-Installation](#express-installation) an. Ähnlich wie bei der Express-Installation informiert der Assistent Sie nach Abschluss der benutzerdefinierten Installation darüber, welches Attribut als Quellankerattribut ausgewählt wurde. |
| Ein bestimmtes Attribut | Wählen Sie diese Option aus, wenn Sie ein vorhandenes AD-Attribut als sourceAnchor-Attribut angeben möchten. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>Vorgehensweise Aktivieren der„ConsistencyGuid“-Funktion - vorhandene Bereitstellung
Haben Sie eine vorhandene Bereitstellung von Azure AD Connect, die „objectGUID“ als das „SourceAnchor“-Attribut verwendet, können Sie auf „ConsistencyGuid“ als „SourceAnchor“-Attribut wechseln.

>[!NOTE]
> Nur neuere Versionen von Azure AD Connect (1.1.552.0 und höher) unterstützen den Wechsel von „objectGUID“ zu „ConsistencyGuid“ als „SourceAnchor“-Attribut.

Von „objectGUID“ zu „ConsistencyGuid“ als „SourceAnchor“-Attribut wechseln:

1. Starten Sie den Azure AD Connect-Assistenten, und klicken Sie auf **Konfigurieren**, um zum Bildschirmbereich Aufgaben zu gelangen.

2. Wählen Sie die **Configure Source Anchor(Quellanker konfigurieren)** Aufgabenoption, und klicken Sie auf **Weiter**.

   ![Aktivieren Sie „ConsistencyGuid“ für die vorhandene Bereitstellung – Schritt 2](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Geben Sie Ihre Azure AD-Administrator-Anmeldeinformationen ein, und klicken Sie auf **Weiter**.

4. Der Assistent überprüft den Status des Attributs „msDS-ConsistencyGuid“ in Ihrem lokalen Active Directory. Ist das Attribut nicht auf irgendein Objekt im Verzeichnis konfiguriert, geht Azure AD Connect davon aus, dass zur Zeit keine andere Anwendung das Attribut nutzt und es daher sicher ist, dieses als das „SourceAnchor“-Attribut zu verwenden. Klicken Sie auf zum Fortfahren auf **Weiter**.

   ![Aktivieren Sie „ConsistencyGuid“ für die vorhandene Bereitstellung – Schritt 4](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. Klicken Sie auf dem **Ready to Configure (Bereit für die Konfiguration)**-Bildschirm auf **Konfigurieren**, um die Konfigurationsänderung vorzunehmen.

   ![Aktivieren Sie „ConsistencyGuid“ für die vorhandene Bereitstellung – Schritt 5](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. Nachdem die Konfiguration abgeschlossen ist, zeigt der Assistent an, dass „MsDS-ConsistencyGuid“ jetzt als das „SourceAnchor“-Attribut verwendet wird.

   ![Aktivieren Sie „ConsistencyGuid“ für die vorhandene Bereitstellung – Schritt 6](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Ist das Attribut während der Analyse (Schritt 4) in einem oder in mehreren Objekten im Verzeichnis konfiguriert, schließt der Assistent daraus, dass das Attribut von einer anderen Anwendung verwendet wird, und gibt die in der nachfolgenden Abbildung dargestellte Fehlermeldung aus. Wenn Sie sicher sind, dass das Attribut nicht von vorhandenen Anwendungen verwendet wird, wenden Sie sich an den Support, um Informationen zum Unterdrücken der Fehlermeldung zu erhalten.

![Aktivieren Sie „ConsistencyGuid“ für die vorhandene Bereitstellung – Fehler](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Auswirkungen auf die AD FS-Konfiguration oder Verbundkonfiguration eines Drittanbieters
Wenn Sie lokale AD FS-Bereitstellungen mit Azure AD Connect verwalten, ändert Azure AD Connect die Anspruchsregeln automatisch dahingehend, dass dasselbe AD-Attribut für „sourceAnchor“ verwendet wird. Dadurch wird sichergestellt, dass der von AD FS generierte Anspruch „ImmutableID“ den „sourceAnchor“-Werten entspricht, die nach Azure AD exportiert werden.

Wenn Sie AD FS außerhalb von Azure AD Connect verwalten oder Drittanbieter-Verbundserver für die Authentifizierung verwenden, müssen Sie die Anspruchsregeln für den Anspruch „ImmutableID“ manuell aktualisieren, sodass er den nach Azure AD exportierten Werten von „sourceAnchor“ entspricht. Dies wird im Abschnitt [Ändern von AD FS-Anspruchsregeln](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims) des Artikels beschrieben. Nach Abschluss der Installation gibt der Assistent folgende Warnung aus:

![Verbundkonfiguration eines Drittanbieters](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Hinzufügen von neuen Verzeichnissen zur vorhandenen Bereitstellung
Nehmen wir an, Sie haben Azure AD Connect mit aktivierter „ConsistencyGuid“-Funktion bereitgestellt und möchten nun ein anderes Verzeichnis zur Bereitstellung hinzufügen. Wenn Sie versuchen, das Verzeichnis hinzuzufügen, überprüft der Azure AD Connect-Assistent den Status des Attributs „msDS-ConsistencyGuid“ im Verzeichnis. Wenn das Attribut in einem oder in mehreren Objekten im Verzeichnis konfiguriert ist, folgert der Assistent daraus, dass das Attribut von anderen Anwendungen verwendet wird und gibt die in der nachfolgenden Abbildung dargestellte Fehlermeldung aus. Wenn Sie sicher sind, dass das Attribut nicht von vorhandenen Anwendungen verwendet wird, wenden Sie sich an den Support, um Informationen zum Unterdrücken der Fehlermeldung zu erhalten.

![Hinzufügen von neuen Verzeichnissen zur vorhandenen Bereitstellung](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Azure AD-Anmeldung
Bei der Integration Ihres lokalen Verzeichnisses in Azure AD ist es wichtig zu wissen, wie sich die Synchronisierungseinstellungen auf die Art und Weise auswirken, in der ein Benutzer sich authentifiziert. Azure AD verwendet einen Benutzerprinzipalname (User Principal Name, UPN, ), um den Benutzer zu authentifizieren. Wenn Sie jedoch Ihre Benutzer synchronisieren, müssen Sie das Attribut, das als Wert für „userPrincipalName“ verwendet werden soll, sehr sorgfältig auswählen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Auswählen des Attributs für userPrincipalName
Bei Auswahl des Attributs, das den in Azure zu verwendenden UPN-Wert bereitstellt, sollten Sie Folgendes sicherstellen:

* Die Attributwerte entsprechen der UPN-Syntax (RFC 822) und müssen somit im Format username@domain
* Das Suffix in den Werten stimmt mit einer der überprüften benutzerdefinierten Domänen in Azure AD überein.

In den Expresseinstellungen wird userPrincipalName für das Attribut angenommen. Falls das userPrincipalName-Attribut nicht den Wert enthält, den Ihre Benutzer zum Anmelden bei Azure verwenden sollen, müssen Sie **Benutzerdefinierte Installation**wählen.

### <a name="custom-domain-state-and-upn"></a>Benutzerdefinierter Domänenstatus und UPN
Sie müssen unbedingt sicherstellen, dass eine überprüfte Domäne für das UPN-Suffix existiert.

John ist ein Benutzer in contoso.com. John soll den lokalen UPN john@contoso.com für die Anmeldung bei Azure verwenden, nachdem Sie die Benutzer mit Ihrem Azure AD-Verzeichnis „contoso.onmicrosoft.com“ synchronisiert haben. Zu diesem Zweck müssen Sie „contoso.com“ als benutzerdefinierte Domäne in Azure AD hinzufügen und überprüfen, bevor Sie mit dem Synchronisieren der Benutzer beginnen können. Wenn das UPN-Suffix von John (beispielsweise „contoso.com“) mit keiner überprüften Domäne in Azure übereinstimmt, ersetzt Azure AD das UPN-Suffix durch „contoso.onmicrosoft.com“.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Nicht routingfähige lokale Domänen und UPN für Azure AD
Einige Unternehmen verfügen über nicht routingfähige Domänen, z.B. „contoso.local“, oder einteilige Domänen, z.B. „contoso“. Sie können eine nicht routingfähige Domäne in Azure AD nicht überprüfen. Azure AD Connect kann Synchronisierungen nur mit einer überprüften Domäne in Azure AD durchführen. Wenn Sie ein Azure AD-Verzeichnis erstellen, wird eine routingfähige Domäne erstellt, die zur Standarddomäne für Ihre Azure AD-Instanz wird, z.B. „contoso.onmicrosoft.com“. Daher ist es notwendig, in einem solchen Szenario alle anderen routingfähigen Domänen zu überprüfen, wenn Sie Synchronisierungen nicht mit der standardmäßigen Domäne „onmicrosoft.com“ durchführen möchten.

Unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](../active-directory-add-domain.md) finden Sie weitere Informationen zum Hinzufügen und Überprüfen von Domänen.

Azure AD Connect erkennt, ob Sie eine nicht routingfähige Domänenumgebung ausführen, und warnt Sie entsprechend davor, nicht mit den Expresseinstellungen fortzufahren. Wenn Sie sich in einer nicht routingfähigen Domäne befinden, ist es wahrscheinlich, dass auch der UPN der Benutzer nicht routingfähige Suffixe aufweist. Wenn Sie beispielsweise „contoso.local“ verwenden, empfiehlt Azure AD Connect die Verwendung von benutzerdefinierten Einstellungen anstatt der Expresseinstellungen. Indem Sie benutzerdefinierte Einstellungen verwenden, können Sie das Attribut angeben, das als UPN für die Anmeldung bei Azure verwendet werden soll, nachdem die Benutzer mit Azure AD synchronisiert wurden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

