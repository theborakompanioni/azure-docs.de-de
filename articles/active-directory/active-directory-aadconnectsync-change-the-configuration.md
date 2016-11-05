---
title: 'Azure AD Connect-Synchronisierung: Ändern der Standardkonfiguration | Microsoft Docs'
description: Erläutert Schritt für Schritt, wie Sie eine Änderung an der Konfiguration in der Azure AD Connect-Synchronisierung vornehmen.
services: active-directory
documentationcenter: ''
author: andkjell
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2016
ms.author: andkjell

---
# Azure AD Connect-Synchronisierung: Ändern der Standardkonfiguration
In diesem Thema wird Schritt für Schritt erläutert, wie Sie Änderungen an der Standardkonfiguration in der Azure AD Connect-Synchronisierung vornehmen. Es enthält schrittweise Anleitungen für einige allgemeinen Szenarien. Mit diesem Wissen sollten Sie in der Lage sein, basierend auf Ihren eigenen Geschäftsregeln einfache Änderungen an Ihrer eigenen Konfiguration vorzunehmen.

## Synchronisierungsregel-Editor
Der Synchronisierungsregel-Editor dient zum Anzeigen und Ändern der Standardkonfiguration. Sie finden ihn im Startmenü in der Gruppe **Azure AD Connect**. ![Startmenü mit dem Synchronisierungsregel-Editor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Wenn Sie den Editor öffnen, sehen Sie die vordefinierten Standardregeln.

![Synchronisierungsregel-Editor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### Navigation im Editor
Mithilfe der Dropdownmenüs am oberen Rand des Editorfensters können Sie schnell nach einer bestimmten Regel suchen. Wenn Sie z.B. die Regeln mit dem Attribut „proxyAddresses“ anzeigen möchten, ändern Sie die Einstellungen in den Dropdownmenüs wie folgt: ![SRE-Filterung](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png) Drücken Sie zum Zurücksetzen der Filter und Laden einer neuen Konfiguration die Taste **F5** auf der Tastatur.

Oben rechts sehen Sie die Schaltfläche **Neue Regel hinzufügen**. Diese Schaltfläche dient zum Erstellen eigener benutzerdefinierter Regeln.

Unten im Fenster befinden sich Schaltflächen, mit denen Sie Aktionen für eine ausgewählte Synchronisierungsregel ausführen können. Die Schaltflächen **Bearbeiten** und **Löschen** sind selbsterklärend. **Exportieren** erzeugt ein PowerShell-Skript zur erneuten Erstellung der Synchronisierungsregel. Dieses Verfahren ermöglicht es Ihnen, eine Synchronisierungsregel von einem Server auf einen anderen zu verschieben.

## Erstellen Ihrer ersten benutzerdefinierten Regel
Änderungen an den Attributflüssen zählen zu den häufigsten Änderungen. Die Daten in Ihrem Quellverzeichnis weichen möglicherweise von denen in Azure AD ab. Im Beispiel in diesem Abschnitt möchten Sie sicherstellen, dass der Vorname eines Benutzers immer die **korrekte Groß-/Kleinschreibung** aufweist.

### Deaktivieren des Schedulers
Der [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) wird standardmäßig alle 30 Minuten ausgeführt. Sie möchten sicherstellen, dass er nicht gestartet wird, während Sie Änderungen vornehmen und Probleme in Ihren neuen Regeln beheben. Um den Scheduler vorübergehend zu deaktivieren, starten Sie PowerShell, und führen Sie folgenden Befehl aus: `Set-ADSyncScheduler -SyncCycleEnabled $false`

![Deaktivieren des Schedulers](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

### Erstellen der Regel
1. Klicken Sie auf **Neue Regel hinzufügen**.
2. Geben Sie auf der Seite **Beschreibung** Folgendes ein: ![Filterung für die eingehende Regel](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)
   * Name: Geben Sie einen beschreibenden Namen für die Regel ein.
   * Beschreibung: Fügen Sie Informationen hinzu, denen andere Benutzer entnehmen können, wozu die Regel dient.
   * Connected System (Verbundenes System): Das System, in dem sich das Objekt befindet. In diesem Fall wählen wir den Active Directory Connector aus.
   * Connected System Object Type/Metaverse Object Type (Objekttyp „Verbundenes System“/Metaverseobjekttyp): Wählen Sie **Benutzer** bzw. **Person** aus.
   * Verknüpfungstyp: Ändern Sie diesen Wert in **Join**.
   * Precedence (Rangfolge): Geben Sie einen Wert an, der im System eindeutig ist. Ein niedrigerer numerischer Wert steht für eine höhere Rangfolge.
   * Tag: Lassen Sie dieses Feld leer. Dieses Feld sollte nur bei vordefinierten Regeln von Microsoft einen Wert enthalten.
3. Geben Sie auf der Seite **Scoping Filter** (Bereichsfilter) **givenName ISNOTNULL** ein. ![Bereichsfilter für die eingehende Regel](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png) Dieser Abschnitt wird verwendet, um die Objekte zu definieren, auf die die Regel angewendet werden soll. Wenn Sie die Felder leer lassen, gilt die Regel für alle Benutzerobjekte. Sie würde in diesem Fall jedoch Konferenzräume, Dienstkonten und andere nicht personenbezogene Benutzerobjekte enthalten.
4. Lassen die Felder im Abschnitt **Join Rules** (Verknüpfungsregeln) leer.
5. Ändern Sie auf der Seite **Transformations** (Transformationen) den „FlowType“ in **Expression** (Ausdruck). Wählen Sie das Zielattribut **givenName** aus, und geben Sie `PCase([givenName])` als Quelle ein. ![Transformationen für die eingehende Regel](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png) Das Synchronisierungsmodul beachtet sowohl beim Funktionsnamen als auch dem Attributnamen die Groß-/Kleinschreibung. Im Fall einer fehlerhaften Eingabe wird beim Hinzufügen der Regel eine Warnung angezeigt. Da Sie Ihre Einstellungen im Editor speichern und den Vorgang dann fortsetzen können, müssten Sie die Regel erneut öffnen und korrigieren.
6. Klicken Sie auf **Hinzufügen**, um die Regel zu speichern.

Die neue benutzerdefinierte Regel sollte zusammen mit den anderen Synchronisierungsregeln im System angezeigt werden.

### Überprüfen der Änderung
Nachdem Sie diese Änderung vorgenommen haben, möchten Sie sicherstellen, dass die Regel wie erwartet funktioniert und nicht zu Fehlern führt. Hierzu stehen Ihnen abhängig von der Anzahl von Objekten zwei Methoden zur Auswahl.

1. Ausführen einer vollständigen Synchronisierung aller Objekte
2. Ausführen einer Vorschau und einer vollständigen Synchronisierung eines einzelnen Objekts

Starten Sie den **Synchronisierungsdienst** über das Startmenü. Alle Schritte in diesem Abschnitt werden in diesem Tool ausgeführt.

1. **Vollständige Synchronisierung aller Objekte** Wählen Sie oben im Fenster **Connectors** aus. Suchen Sie nach dem Connector, den Sie im vorherigen Abschnitt geändert haben (in diesem Fall Active Directory-Domänendienste), und wählen Sie ihn aus. Klicken Sie im Menü „Aktionen“ auf **Ausführen**, wählen Sie **Full Synchronization** (Vollständige Synchronisierung) aus, und klicken Sie auf **OK**. ![Vollständige Synchronisierung](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png) Die Objekte sind jetzt in der Metaverse aktualisiert. Nun möchten Sie sich das Objekt in der Metaverse ansehen.
2. **Ausführen einer Vorschau und einer vollständigen Synchronisierung eines einzelnen Objekts** Wählen Sie oben im Fenster **Connectors** aus. Suchen Sie nach dem Connector, den Sie im vorherigen Abschnitt geändert haben (in diesem Fall Active Directory-Domänendienste), und wählen Sie ihn aus. Wählen Sie **Search Connector Space** (Connectorbereich durchsuchen) aus. Verwenden Sie den Bereich, um nach einem Objekt zu suchen, das Sie zum Testen der Änderung verwenden möchten. Wählen Sie das Objekt aus, und klicken Sie auf **Vorschau**. Wählen Sie auf dem neuen Bildschirm **Commit Preview** (Commitvorschau) aus. ![Commitvorschau](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png) Die Änderung wird jetzt an die Metaverse übergeben.

**Überprüfen des Objekts in der Metaverse** Jetzt möchten Sie einige Beispielobjekte auswählen, um sicherzustellen, dass der Wert wie erwartet ist und die Regel angewendet wird. Wählen Sie oben im Fenster **Metaverse Search** (Metaversesuche) aus. Fügen Sie beliebige Filter hinzu, die Sie zur Suche nach den entsprechenden Objekten benötigen. Öffnen Sie im Suchergebnis ein Objekt. Sehen Sie sich die Attributwerte an, und überprüfen Sie in der Spalte **Synchronisierungsregeln**, ob die Regel wie erwartet angewendet wurde. ![Metaversesuche](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)

### Deaktivieren des Schedulers
Wenn alles wie erwartet funktioniert, können Sie den Scheduler wieder aktivieren. Führen Sie in PowerShell `Set-ADSyncScheduler -SyncCycleEnabled $true` aus.

## Andere häufige Änderungen am Attributfluss
Im vorherigen Abschnitt wurde beschrieben, wie Sie Änderungen an einem Attributfluss vornehmen. In diesem Abschnitt werden einige weitere Beispiele vorgestellt. Die Schritte zum Erstellen der Synchronisierungsregel werden hier in gekürzter Form wiedergegeben. Die vollständigen Schritte finden Sie bei Bedarf im vorherigen Abschnitt.

### Verwenden eines anderen Attributs als den Standardwert
Bei Fabrikam wird in einer Gesamtstruktur das lokale Alphabet für Vorname, Nachname und Anzeigename verwendet. Die lateinische Zeichenfolgendarstellung dieser Attribute ist in den Erweiterungsattributen zu finden. Beim Erstellen der globalen Adressliste in Azure AD und Office 365 möchte das Unternehmen stattdessen diese verwenden.

Mit einer Standardkonfiguration sieht ein Objekt aus der lokalen Gesamtstruktur wie folgt aus: ![Attributfluss 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Führen Sie folgende Schritte aus, um eine Regel mit anderen Attributflüssen zu erstellen:

* Öffnen Sie den **Synchronisierungsregel-Editor** über das Startmenü.
* Lassen Sie auf der linken Seite **Eingehend** ausgewählt, und klicken Sie auf die Schaltfläche **Neue Regel hinzufügen**.
* Benennen Sie die Regel und fügen Sie eine Beschreibung hinzu. Wählen Sie das lokale Active Directory und die entsprechenden Objekttypen aus. Wählen Sie für **Verknüpfungstyp** die Option **Join** aus. Wählen Sie als Rangfolge eine Zahl, die nicht von einer anderen Regel verwendet wird. Die vordefinierten Regeln beginnen mit 100. In diesem Beispiel kann also der Wert 50 verwendet werden. ![Attributfluss 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Lassen Sie den Bereich leer (d.h. die Regel soll für alle Benutzerobjekte in der Gesamtstruktur gelten).
* Lassen Sie die Verknüpfungsregeln leer (d.h. alle Verknüpfungen sollen von der Standardregel behandelt werden).
* Erstellen Sie unter „Transformations“ (Transformationen) die folgenden Abläufe. ![Attributfluss 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Klicken Sie auf **Hinzufügen**, um die Regel zu speichern.
* Wechseln Sie zu **Synchronization Service Manager**. Wählen Sie unter **Connectors** den Connector aus, für den wir die Regel hinzugefügt haben. Wählen Sie **Ausführen** und dann **Full Synchronization** (Vollständige Synchronisierung) aus. Bei einer vollständigen Synchronisierung werden alle Objekte neu berechnet, die die aktuellen Regeln verwenden.

Dies ist das Ergebnis für dasselbe Objekt mit dieser benutzerdefinierten Regel: ![Attributfluss 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### Länge der Attribute
Zeichenfolgenattribute sind standardmäßig als indizierbar festgelegt, die maximale Länge beträgt 448 Zeichen. Wenn Sie mit Zeichenfolgenattributen arbeiten, die möglicherweise mehr Zeichen enthalten, müssen Sie Folgendes in den Attributfluss einschließen: `attributeName` <- `Left([attributeName],448)`

### Ändern von "userPrincipalSuffix"
Das userPrincipalName-Attribut in Active Directory ist den Benutzern nicht immer bekannt und eignet sich möglicherweise nicht als Anmelde-ID. Der Installationsassistent für die Azure AD Connect-Synchronisierung ermöglicht die Auswahl eines anderen Attributs, z.B. „mail“. In einigen Fällen muss das Attribut jedoch berechnet werden. Das Unternehmen Contoso verfügt beispielsweise über zwei Azure AD-Verzeichnisse, eins für die Produktion und eins für Testzwecke. Benutzer im Testmandanten sollen ein anderes Suffix in der Anmelde-ID verwenden. `userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Verketten Sie in diesem Ausdruck alle Zeichen links des ersten @-Zeichens (Word) mit einer festgelegten Zeichenfolge.

### Konvertieren eines einwertigen Attributs in ein mehrwertiges Attribut
Einige Attribute in Active Directory weisen im Schema mehrere Werte auf, obwohl sie in "Active Directory-Benutzer und -Computer" wie einwertige Attribute aussehen. Das Attribut „description“ ist ein Beispiel hierfür. `description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

Falls das Attribut einen Wert besitzt, wird in diesem Ausdruck das erste Element (Item) im Attribut genommen, die voran- und nachgestellten Leerzeichen (Trim) werden entfernt, und die ersten 448 Zeichen (Left) der Zeichenfolge werden beibehalten.

### Verhindern des „Fließens“ eines Attributs
Hintergrundinformationen zum Szenario in diesem Abschnitt finden Sie unter [Steuern des Attributflussprozesses](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Es gibt zwei Möglichkeiten, wie Sie dafür sorgen, dass ein Attribut nicht „fließt“ (also nicht übertragen wird). Die erste Option befindet sich im Installations-Assistenten und ermöglicht es Ihnen, [ausgewählte Attribute zu entfernen](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Diese Option funktioniert, wenn Sie das Attribut vorher noch nie synchronisiert haben. Falls Sie aber bereits mit der Synchronisierung dieses Attributs begonnen haben und es danach mit diesem Feature entfernen, beendet das Synchronisierungsmodul die Verwaltung des Attributs, und die vorhandenen Werte verbleiben in Azure AD.

Wenn Sie den Wert eines Attributs entfernen und sicherstellen möchten, dass es in Zukunft nicht „fließt“, müssen Sie stattdessen eine benutzerdefinierte Regel erstellen.

Bei Fabrikam haben wir festgestellt, dass einige der Attribute, die wir mit der Cloud synchronisiert haben, nicht vorhanden sein sollten. Wir möchten sicherstellen, dass diese Attribute aus Azure AD entfernt werden. ![Ungültige Erweiterungsattribute](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Erstellen einer neuen Regel für eingehende Synchronisierung und Füllen der Beschreibung ![Beschreibungen](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Erstellen Sie Attributflüsse vom Typ **Ausdruck** und mit der Quelle **AuthoritativeNull**. Das Literal **AuthoritativeNull** gibt an, dass der Wert in MV auch dann leer sein sollte, wenn eine Synchronisierungsregel mit geringerer Vorrangigkeit versucht, den Wert einzufügen. ![Transformation für Erweiterungsattribute](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Speichern Sie die Synchronisierungsregel. Starten Sie **Synchronisierungsdienst**, suchen Sie nach dem Connector, und wählen Sie **Ausführen** und **Vollständige Synchronisierung**. Dadurch werden alle Attributflüsse neu berechnet.
* Stellen Sie sicher, dass die beabsichtigten Änderungen exportiert werden sollen, indem Sie den Connectorbereich durchsuchen. ![Gestaffeltes Löschen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## Nächste Schritte
* Weitere Informationen zum Konfigurationsmodell finden Sie unter [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md) (Grundlegendes zur deklarativen Bereitstellung).
* Weitere Informationen zur Ausdruckssprache finden Sie unter [Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0914_2016-->