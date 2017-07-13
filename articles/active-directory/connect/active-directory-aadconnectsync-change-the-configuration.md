---
title: "Azure AD Connect-Synchronisierung: Vornehmen einer Konfigurationsänderung in der Azure AD Connect-Synchronisierung | Microsoft-Dokumentation"
description: "Erläutert Schritt für Schritt, wie Sie eine Änderung an der Konfiguration in der Azure AD Connect-Synchronisierung vornehmen."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 4a88cf56eea3dd562d4d5dcc4fe7364ea226a348
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017

---
<a id="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration" class="xliff"></a>

# Azure AD Connect-Synchronisierung: Ändern der Standardkonfiguration
In diesem Thema wird Schritt für Schritt erläutert, wie Sie Änderungen an der Standardkonfiguration in der Azure AD Connect-Synchronisierung vornehmen. Es enthält Schrittanleitungen für einige allgemeinen Szenarien. Mit diesem Wissen sollten Sie in der Lage sein, basierend auf Ihren eigenen Geschäftsregeln einfache Änderungen an Ihrer eigenen Konfiguration vorzunehmen.

<a id="synchronization-rules-editor" class="xliff"></a>

## Synchronisierungsregel-Editor
Der Synchronisierungsregel-Editor dient zum Anzeigen und Ändern der Standardkonfiguration. Sie finden ihn im Startmenü in der Gruppe **Azure AD Connect** .  
![Startmenü mit dem Synchronisierungsregel-Editor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Wenn Sie den Editor öffnen, sehen Sie die vordefinierten Standardregeln.

![Synchronisierungsregel-Editor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

<a id="navigating-in-the-editor" class="xliff"></a>

### Navigation im Editor
Mithilfe der Dropdownmenüs am oberen Rand des Editorfensters können Sie schnell nach einer bestimmten Regel suchen. Wenn Sie z.B. die Regeln mit dem Attribut „proxyAddresses“ anzeigen möchten, ändern Sie die Einstellungen in den Dropdownmenüs wie folgt:  
![SRE-Filterung](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Drücken Sie zum Filtern und Laden einer neuen Konfiguration **F5** auf der Tastatur.

Oben rechts sehen Sie die Schaltfläche **Neue Regel hinzufügen**. Diese Schaltfläche dient zum Erstellen eigener benutzerdefinierter Regeln.

Unten im Fenster befinden sich Schaltflächen, mit denen Sie Aktionen für eine ausgewählte Synchronisierungsregel ausführen können. Die Schaltflächen **Bearbeiten** und **Löschen** sind selbsterklärend. **Exportieren** erzeugt ein PowerShell-Skript zur erneuten Erstellung der Synchronisierungsregel. Dieses Verfahren ermöglicht es Ihnen, eine Synchronisierungsregel von einem Server auf einen anderen zu verschieben.

<a id="create-your-first-custom-rule" class="xliff"></a>

## Erstellen Ihrer ersten benutzerdefinierten Regel
Änderungen an den Attributflüssen zählen zu den häufigsten Änderungen. Die Daten in Ihrem Quellverzeichnis weichen möglicherweise von denen in Azure AD ab. Im Beispiel in diesem Abschnitt möchten Sie sicherstellen, dass der Vorname eines Benutzers immer die **korrekte Groß-/Kleinschreibung**aufweist.

<a id="disable-the-scheduler" class="xliff"></a>

### Deaktivieren des Schedulers
Der [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) wird standardmäßig alle 30 Minuten ausgeführt. Sie möchten sicherstellen, dass er nicht gestartet wird, während Sie Änderungen vornehmen und Probleme in Ihren neuen Regeln beheben. Um den Scheduler vorübergehend zu deaktivieren, starten Sie PowerShell, und führen Sie folgenden Befehl aus: `Set-ADSyncScheduler -SyncCycleEnabled $false`

![Deaktivieren des Schedulers](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

<a id="create-the-rule" class="xliff"></a>

### Erstellen der Regel
1. Klicken Sie auf **Neue Regel hinzufügen**.
2. Geben Sie auf der Seite **Beschreibung** Folgendes ein:  
   ![Filterung für die eingehende Regel](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Name: Geben Sie einen beschreibenden Namen für die Regel ein.
   * Beschreibung: Fügen Sie Informationen hinzu, denen andere Benutzer entnehmen können, wozu die Regel dient.
   * Verbundenes System: Das System, in dem sich das Objekt befindet. Wählen Sie in diesem Fall den Active Directory Connector aus.
   * Objekttyp im verbundenen System/Metaverse-Objekttyp: Wählen Sie **User** bzw. **Person** aus.
   * Verknüpfungstyp: Ändern Sie diesen Wert in **Join**.
   * Rangfolge: Geben Sie einen Wert an, der im System eindeutig ist. Ein niedrigerer numerischer Wert steht für eine höhere Rangfolge.
   * Tag: Lassen Sie dieses Feld leer. Dieses Feld sollte nur bei vordefinierten Regeln von Microsoft einen Wert enthalten.
3. Geben Sie auf der Seite **Bereichsfilter** den Wert **givenName ISNOTNULL** ein.  
   ![Bereichsfilter für die eingehende Regel](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Dieser Abschnitt wird verwendet, um die Objekte zu definieren, auf die die Regel angewendet werden soll. Wenn Sie die Felder leer lassen, gilt die Regel für alle Benutzerobjekte. Sie würde in diesem Fall jedoch Konferenzräume, Dienstkonten und andere nicht personenbezogene Benutzerobjekte enthalten.
4. Lassen die Felder im Abschnitt **Zusammenführungsregeln**  leer.
5. Ändern Sie auf der Seite **Transformationen** den „FlowType“ in **Ausdruck**. Wählen Sie das Zielattribut **givenName** aus, und geben Sie `PCase([givenName])` als Quelle ein.
   ![Transformationen für die eingehende Regel](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Das Synchronisierungsmodul beachtet sowohl beim Funktionsnamen als auch dem Attributnamen die Groß-/Kleinschreibung. Im Fall einer fehlerhaften Eingabe wird beim Hinzufügen der Regel eine Warnung angezeigt. Da Sie Ihre Einstellungen im Editor speichern und den Vorgang dann fortsetzen können, müssten Sie die Regel erneut öffnen und korrigieren.
6. Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.

Die neue benutzerdefinierte Regel sollte zusammen mit den anderen Synchronisierungsregeln im System angezeigt werden.

<a id="verify-the-change" class="xliff"></a>

### Überprüfen der Änderung
Nachdem Sie diese Änderung vorgenommen haben, möchten Sie sicherstellen, dass die Regel wie erwartet funktioniert und nicht zu Fehlern führt. Hierzu stehen Ihnen abhängig von der Anzahl von Objekten zwei Methoden zur Auswahl.

1. Ausführen einer vollständigen Synchronisierung aller Objekte
2. Ausführen einer Vorschau und einer vollständigen Synchronisierung eines einzelnen Objekts

Starten Sie den **Synchronisierungsdienst** über das Startmenü. Alle Schritte in diesem Abschnitt werden in diesem Tool ausgeführt.

1. **Vollständige Synchronisierung aller Objekte**  
   Klicken Sie im Menü „Aktionen“ auf **Connectors** aus. Suchen Sie nach dem Connector, den Sie im vorherigen Abschnitt geändert haben (in diesem Fall Active Directory Domain Services), und wählen Sie ihn aus. Klicken Sie im Menü „Aktionen“ auf **Ausführen**, wählen Sie **Vollständige Synchronisierung** aus, und klicken Sie auf **OK**.
   ![Vollständige Synchronisierung](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Die Objekte sind jetzt in der Metaverse aktualisiert. Nun möchten Sie sich das Objekt in der Metaverse ansehen.
2. **Vorschau und vollständige Synchronisierung eines einzelnen Objekts**  
   Klicken Sie im Menü „Aktionen“ auf **Connectors** aus. Suchen Sie nach dem Connector, den Sie im vorherigen Abschnitt geändert haben (in diesem Fall die Active Directory Domain Services), und wählen Sie ihn aus. Wählen Sie **Search Connector Space**(Connectorbereich durchsuchen) aus. Verwenden Sie den Bereich, um nach einem Objekt zu suchen, das Sie zum Testen der Änderung verwenden möchten. Wählen Sie das Objekt aus, und klicken Sie auf **Vorschau**. Wählen Sie auf dem neuen Bildschirm **Commit Preview**(Commitvorschau) aus.  
   ![Commit preview](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   Die Änderung wird jetzt an die Metaverse übergeben.

**Überprüfen des Objekts in der Metaverse**  
Jetzt möchten Sie einige Beispielobjekte auswählen, um sicherzustellen, dass der Wert wie erwartet ist und die Regel angewendet wird. Wählen Sie oben im Fenster **Metaverse Search** (Metaversesuche) aus. Fügen Sie beliebige Filter hinzu, die Sie zur Suche nach den entsprechenden Objekten benötigen. Öffnen Sie im Suchergebnis ein Objekt. Sehen Sie sich die Attributwerte an, und überprüfen Sie in der Spalte **Synchronisierungsregeln** , ob die Regel wie erwartet angewendet wurde.  
![Metaverse search](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

<a id="enable-the-scheduler" class="xliff"></a>

### Deaktivieren des Schedulers
Wenn alles wie erwartet funktioniert, können Sie den Scheduler wieder aktivieren. Führen Sie in PowerShell `Set-ADSyncScheduler -SyncCycleEnabled $true`aus.

<a id="other-common-attribute-flow-changes" class="xliff"></a>

## Andere häufige Änderungen am Attributfluss
Im vorherigen Abschnitt wurde beschrieben, wie Sie Änderungen an einem Attributfluss vornehmen. In diesem Abschnitt werden einige weitere Beispiele vorgestellt. Die Schritte zum Erstellen der Synchronisierungsregel werden hier in gekürzter Form wiedergegeben. Die vollständigen Schritte finden Sie bei Bedarf im vorherigen Abschnitt.

<a id="use-another-attribute-than-the-default" class="xliff"></a>

### Verwenden eines anderen Attributs als den Standardwert
Bei Fabrikam wird in einer Gesamtstruktur das lokale Alphabet für Vorname, Nachname und Anzeigename verwendet. Die lateinische Zeichenfolgendarstellung dieser Attribute ist in den Erweiterungsattributen zu finden. Beim Erstellen der globalen Adressliste in Azure AD und Office 365 möchte das Unternehmen stattdessen diese verwenden.

Mit einer Standardkonfiguration sieht ein Objekt aus der lokalen Gesamtstruktur wie folgt aus:   
![Attributfluss 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Führen Sie folgende Schritte aus, um eine Regel mit anderen Attributflüssen zu erstellen:

* Öffnen Sie den **Synchronisierungsregel-Editor** über das Startmenü.
* Lassen Sie auf der linken Seite **Eingehend** ausgewählt, und klicken Sie auf die Schaltfläche **Neue Regel hinzufügen**.
* Benennen Sie die Regel und fügen Sie eine Beschreibung hinzu. Wählen Sie das lokale Active Directory und die entsprechenden Objekttypen aus. Wählen Sie als **Verknüpfungstyp** die Option **Join** aus. Wählen Sie als Rangfolge eine Zahl, die nicht von einer anderen Regel verwendet wird. Die vordefinierten Regeln beginnen mit 100. In diesem Beispiel kann also der Wert 50 verwendet werden.
  ![Attributfluss 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Lassen Sie den Bereich leer (d.h. die Regel soll für alle Benutzerobjekte in der Gesamtstruktur gelten).
* Lassen Sie die Verknüpfungsregeln leer (d.h. alle Verknüpfungen sollen von der Standardregel behandelt werden).
* Erstellen Sie unter „Transformationen“ die folgenden Abläufe:  
  ![Attributfluss 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.
* Wechseln Sie zu **Synchronization Service Manager**. Wählen Sie unter **Connectors**den Connector aus, für den wir die Regel hinzugefügt haben. Wählen Sie **Ausführen** und dann **Vollständige Synchronisierung** aus. Bei einer vollständigen Synchronisierung werden alle Objekte neu berechnet, die die aktuellen Regeln verwenden.

Dies ist das Ergebnis für dasselbe Objekt mit dieser benutzerdefinierten Regel:   
![Attributfluss 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

<a id="length-of-attributes" class="xliff"></a>

### Länge der Attribute
Zeichenfolgenattribute sind standardmäßig als indizierbar festgelegt, die maximale Länge beträgt 448 Zeichen. Wenn Sie mit Zeichenfolgenattributen arbeiten, die möglicherweise mehr Zeichen enthalten, müssen Sie Folgendes in den Attributfluss einschließen:   
`attributeName` <- `Left([attributeName],448)`

<a id="changing-the-userprincipalsuffix" class="xliff"></a>

### Ändern von "userPrincipalSuffix"
Das userPrincipalName-Attribut in Active Directory ist den Benutzern nicht immer bekannt und eignet sich möglicherweise nicht als Anmelde-ID. Der Installationsassistent für die Azure AD Connect-Synchronisierung ermöglicht die Auswahl eines anderen Attributs, z.B. „mail“. In einigen Fällen muss das Attribut jedoch berechnet werden. Das Unternehmen Contoso verfügt beispielsweise über zwei Azure AD-Verzeichnisse, eins für die Produktion und eins für Testzwecke. Benutzer im Testmandanten sollen ein anderes Suffix in der Anmelde-ID verwenden.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Verketten Sie in diesem Ausdruck alle Zeichen links neben dem ersten @-sign (Word) mit einer festgelegten Zeichenfolge.

<a id="convert-a-multi-value-to-a-single-value" class="xliff"></a>

### Konvertieren eines einwertigen Attributs in ein mehrwertiges Attribut
Einige Attribute in Active Directory weisen im Schema mehrere Werte auf, obwohl sie in "Active Directory-Benutzer und -Computer" wie einwertige Attribute aussehen. Das Attribut „description“ ist ein Beispiel hierfür.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

Falls das Attribut einen Wert besitzt, wird in diesem Ausdruck das erste Element (Item) im Attribut genommen, die voran- und nachgestellten Leerzeichen (Trim) werden entfernt, und die ersten 448 Zeichen (Left) der Zeichenfolge werden beibehalten.

<a id="do-not-flow-an-attribute" class="xliff"></a>

### Verhindern des „Fließens“ eines Attributs
Hintergrundinformationen zum Szenario in diesem Abschnitt finden Sie unter [Steuern des Attributflussprozesses](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Es gibt zwei Möglichkeiten, wie Sie dafür sorgen, dass ein Attribut nicht „fließt“ (also nicht übertragen wird). Die erste Option befindet sich im Installations-Assistenten und ermöglicht es Ihnen, [ausgewählte Attribute zu entfernen](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Diese Option funktioniert, wenn Sie das Attribut vorher noch nie synchronisiert haben. Falls Sie aber bereits mit der Synchronisierung dieses Attributs begonnen haben und es danach mit diesem Feature entfernen, beendet das Synchronisierungsmodul die Verwaltung des Attributs, und die vorhandenen Werte verbleiben in Azure AD.

Wenn Sie den Wert eines Attributs entfernen und sicherstellen möchten, dass es in Zukunft nicht „fließt“, müssen Sie stattdessen eine benutzerdefinierte Regel erstellen.

Bei Fabrikam haben wir festgestellt, dass einige der Attribute, die wir mit der Cloud synchronisiert haben, nicht vorhanden sein sollten. Wir möchten sicherstellen, dass diese Attribute aus Azure AD entfernt werden.  
![Ungültige Erweiterungsattribute](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Erstellen Sie eine neue Regel für die eingehende Synchronisierung, und Füllen Sie die ![Beschreibungen](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png).
* Erstellen Sie Attributflüsse vom Typ **Ausdruck** und mit der Quelle **AuthoritativeNull**. Das Literal **AuthoritativeNull** gibt an, dass der Wert in MV auch dann leer sein sollte, wenn eine Synchronisierungsregel mit geringerer Vorrangigkeit versucht, den Wert einzufügen.
  ![Transformation für Erweiterungsattribute](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Speichern Sie die Synchronisierungsregel. Starten Sie den **Synchronisierungsdienst**, suchen Sie nach dem Connector, und wählen Sie **Ausführen** und **Vollständige Synchronisierung**. Dadurch werden alle Attributflüsse neu berechnet.
* Stellen Sie sicher, dass die beabsichtigten Änderungen exportiert werden sollen, indem Sie den Connectorbereich durchsuchen.
  ![Gestaffeltes Löschen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

<a id="create-rules-with-powershell" class="xliff"></a>

## Erstellen von Regeln mit PowerShell
Der Synchronisierungsregel-Editor funktioniert gut, wenn Sie nur wenige Änderungen vornehmen möchten. Wenn mehr Änderungen erforderlich sind, ist PowerShell wahrscheinlich besser geeignet. Einige erweiterte Funktionen sind nur über PowerShell verfügbar.

<a id="get-the-powershell-script-for-an-out-of-box-rule" class="xliff"></a>

### Abrufen des PowerShell-Skripts für eine Standardregel
Um das PowerShell-Skript anzuzeigen, mit dem eine Standardregel erstellt wurde, wählen Sie die Regel im Synchronisierungsregel-Editor aus, und klicken Sie auf **Exportieren**. Damit erhalten Sie das PowerShell-Skript, mit dem die Regel erstellt wurde.

<a id="advanced-precedence" class="xliff"></a>

### Erweiterte Rangfolge
Die standardmäßigen Synchronisierungsregeln beginnen mit dem Rangfolgewert 100. Wenn Sie über viele Gesamtstrukturen verfügen und viele benutzerdefinierte Änderungen vornehmen müssen, sind 99 Synchronisierungsregeln möglicherweise nicht ausreichend.

Sie können das Synchronisierungsmodul anweisen, dass vor den Standardregeln weitere Regeln eingefügt werden sollen. Führen Sie folgende Schritte aus, um dieses Verhalten festzulegen:

1. Markieren Sie im Synchronisierungsregel-Editor die erste Standardregel (hierbei handelt es sich um **In from AD - User Join**), und wählen Sie **Exportieren** aus. Kopieren Sie den SR-ID-Wert.  
![PowerShell vor der Änderung](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Erstellen Sie die neue Synchronisierungsregel. Sie können zur Erstellung den Synchronisierungsregel-Editor verwenden. Exportieren Sie die Regel in ein PowerShell-Skript.
3. Fügen Sie den ID-Wert aus der Standardregel in die Eigenschaft **PrecedenceBefore** ein. Legen Sie die **Rangfolge** auf **0** fest. Stellen Sie sicher, dass das ID-Attribut eindeutig ist und Sie keine GUID aus einer anderen Regel wiederverwenden. Stellen Sie auch sicher, dass die Eigenschaft **ImmutableTag** nicht festgelegt ist. Diese Eigenschaft sollte nur für eine Standardregel festgelegt werden. Speichern Sie das PowerShell-Skript, und führen Sie es aus. Das Ergebnis dieses Vorgehens: Ihrer benutzerdefinierten Regel wird der Rangfolgewert 100 zugewiesen, und alle anderen Standardregeln erhalten schrittweise ansteigende Werte.  
![PowerShell nach der Änderung](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Bei Bedarf können Sie eine Vielzahl von benutzerdefinierten Synchronisierungsregeln einrichten, die den gleichen **PrecedenceBefore**-Wert verwenden.


<a id="enable-synchronization-of-preferreddatalocation" class="xliff"></a>

## Aktivieren der Synchronisierung von „PreferredDataLocation“
Azure AD Connect unterstützt in Version 1.1.524.0 und höher die Synchronisierung des Attributs **PreferredDataLocation** für **Benutzerobjekte**. Genauer gesagt wurden folgende Änderungen eingeführt:

* Das Schema des Objekttyps **Benutzer** im Azure AD-Connector wird um das Attribut „PreferredDataLocation“ erweitert, das vom Typ „Zeichenfolge“ und einwertig ist.

* Das Schema des Objekttyps **Person** in der Metaverse wird um das Attribut „PreferredDataLocation“ erweitert, das vom Typ „Zeichenfolge“ und einwertig ist.

Standardmäßig ist das Attribut „PreferredDataLocation“ nicht für die Synchronisierung aktiviert, da kein entsprechendes „PreferredDataLocation“-Attribut im lokalen Active Directory vorhanden ist. Sie müssen die Synchronisierung manuell aktivieren.

> [!IMPORTANT]
> Derzeit kann in Azure AD das Attribut „PreferredDataLocation“ sowohl in synchronisierten Benutzerobjekten als auch Cloudbenutzerobjekten direkt mithilfe von Azure AD PowerShell konfiguriert werden. Nachdem Sie die Synchronisierung des Attributs „PreferredDataLocation“ aktiviert haben, dürfen Sie das Attribut nicht mehr in **synchronisierten Benutzerobjekten** mit Azure AD PowerShell konfigurieren, da Azure AD Connect diese basierend auf den Quellattributwerten im lokalen Active Directory überschreibt.

> [!IMPORTANT]
> Ab dem 1. September 2017 kann in Azure AD das Attribut „PreferredDataLocation“ in **synchronisierten Benutzerobjekten** nicht mehr direkt mithilfe von Azure AD PowerShell konfiguriert werden. Für die Konfiguration des Attributs „PreferredLocation“ in synchronisierten Benutzerobjekten dürfen Sie nur Azure AD Connect verwenden.

Bevor Sie die Synchronisierung des Attributs „PreferredDataLocation“ aktivieren, müssen Sie folgende Schritte durchführen:

 * Legen Sie zuerst fest, welches lokale Active Directory-Attribut als Quellattribut verwendet werden soll. Dieses muss vom Typ **Zeichenfolge** und **einwertig** sein.

 * Wenn Sie das Attribut „PreferredDataLocation“ mithilfe von Azure AD PowerShell zuvor in vorhandenen synchronisierten Benutzerobjekten in Azure AD konfiguriert haben, müssen Sie ein **Backport** für die Attributwerte auf die entsprechenden Benutzerobjekte im lokalen Active Directory durchführen.
 
    > [!IMPORTANT]
    > Wenn Sie kein Backport für die Attributwerte auf die entsprechenden Benutzerobjekte im lokalen Active Directory durchführen, entfernt Azure AD Connect bei aktivierter Synchronisierung für das Attribut „PreferredDataLocation“ die vorhandenen Attributwerte in Azure AD.

 * Es wird empfohlen, das Quellattribut jetzt in mindestens einigen lokalen AD-Benutzerobjekten zu konfigurieren, die später für die Überprüfung verwendet werden.
 
Die Schritte zum Aktivieren der Synchronisierung des Attributs „PreferredDataLocation“ können wie folgt zusammengefasst werden:

1. Deaktivieren des Synchronisierungsplaners und Sicherstellen, dass derzeit keine Synchronisierung ausgeführt wird

2. Hinzufügen des Quellattributs zum lokalen AD-Connectorschema

3. Hinzufügen von „PreferredDataLocation“ zum Azure AD-Connectorschema

4. Erstellen einer Synchronisierungsregel für eingehende Daten zur Übertragung des Attributwerts aus dem lokalen Active Directory

5. Erstellen einer Synchronisierungsregel für ausgehende Daten zur Übertragung des Attributwerts an Azure AD

6. Ausführen des vollständigen Synchronisierungszyklus

7. Aktivieren des Synchronisierungsplaners

> [!NOTE]
> Im restlichen Teil dieses Abschnitts werden diese Schritte ausführlich erläutert. Sie werden im Kontext einer Azure AD-Bereitstellung mit einer Topologie mit einer einzelnen Gesamtstruktur und ohne benutzerdefinierte Synchronisierungsregeln beschrieben. Wenn Sie über eine Topologie mit mehreren Gesamtstrukturen verfügen, benutzerdefinierte Synchronisierungsregeln konfiguriert haben oder einen Stagingserver verwenden, müssen Sie die Schritte entsprechend anpassen.

<a id="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress" class="xliff"></a>

### Schritt 1: Deaktivieren des Synchronisierungsplaners und Sicherstellen, dass derzeit keine Synchronisierung ausgeführt wird
Stellen Sie sicher, dass keine Synchronisierung ausgeführt wird, während Sie Synchronisierungsregeln aktualisieren. So vermeiden Sie, dass unbeabsichtigte Änderungen nach Azure AD exportiert werden. So deaktivieren Sie den integrierten Synchronisierungsplaner:

 1. Starten Sie eine PowerShell-Sitzung auf dem Azure AD Connect-Server.

 2. Deaktivieren Sie die geplante Synchronisierung durch Ausführung des Cmdlets `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 
 3. Starten Sie **Synchronization Service Manager**, indem Sie zu „START“ > „Synchronization Service“ navigieren.
 
 4. Wechseln Sie zur Registerkarte **Vorgänge**, und vergewissern Sie sich, dass kein Vorgang mit dem Status *In Arbeit* angezeigt wird.

![Synchronization Service Manager – Überprüfen, dass keine Vorgänge durchgeführt werden](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

<a id="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema" class="xliff"></a>

### Schritt 2: Hinzufügen des Quellattributs zum lokalen AD-Connectorschema
Nicht alle AD-Attribute werden in den lokale AD-Connectorbereich importiert. So fügen Sie das Quellattribut zur Liste der importierten Attribute hinzu:

 1. Wechseln Sie in Synchronization Service Manager zur Registerkarte **Connectors**.
 
 2. Klicken Sie mit der rechten Maustaste auf den **lokalen AD-Connector**, und wählen Sie **Eigenschaften** aus.
 
 3. Navigieren Sie im Popupdialogfeld zur Registerkarte **Attribute auswählen**.
 
 4. Stellen Sie sicher, dass das Quellattribut in der Attributliste aktiviert ist.
 
 5. Klicken Sie zum Speichern auf **OK**.

![Hinzufügen des Quellattributs zum lokalen AD-Connectorschema](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

<a id="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema" class="xliff"></a>

### Schritt 3: Hinzufügen von „PreferredDataLocation“ zum Azure AD-Connectorschema
Standardmäßig wird das Attribut „PreferredDataLocation“ nicht in den Azure AD Connect-Bereich importiert. So fügen Sie das Attribut „PreferredDataLocation“ zur Liste der importierten Attribute hinzu:

 1. Wechseln Sie in Synchronization Service Manager zur Registerkarte **Connectors**.

 2. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Eigenschaften** aus.

 3. Navigieren Sie im Popupdialogfeld zur Registerkarte **Attribute auswählen**.

 4. Stellen Sie sicher, dass das Attribut „PreferredDataLocation“ in der Attributliste aktiviert ist.

 5. Klicken Sie zum Speichern auf **OK**.

![Hinzufügen des Quellattributs zum Azure AD-Connectorschema](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

<a id="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory" class="xliff"></a>

### Schritt 4: Erstellen einer Synchronisierungsregel für eingehende Daten zur Übertragung des Attributwerts aus dem lokalen Active Directory
Die Synchronisierungsregel für eingehende Daten ermöglicht die Übertragung des Attributwerts aus dem Quellattribut im lokalen Active Directory auf die Metaverse:

1. Starten Sie den **Synchronisierungsregel-Editor**, indem Sie zu „START“ > „Synchronisierungsregel-Editor“ navigieren.

2. Legen Sie den Suchfilter **Richtung** auf **Eingehend** fest.

3. Klicken Sie auf die Schaltfläche **Neue Regel hinzufügen**, um eine neue Regel für eingehende Daten zu erstellen.

4. Geben Sie auf der Registerkarte **Beschreibung** die folgende Konfiguration an:
 
    | Attribut | Wert | Details |
    | --- | --- | --- |
    | Name | *Geben Sie einen Namen ein.* | Beispiel: *In from AD – User PreferredDataLocation* |
    | Beschreibung | *Geben Sie eine Beschreibung ein.* |  |
    | Verbundenes System | *Wählen Sie den lokalen AD-Connector aus.* |  |
    | Objekttyp des verbundenen Systems | **Benutzer** |  |
    | Metaverse-Objekttyp | **Person** |  |
    | Verknüpfungstyp | **Join** |  |
    | Rangfolge | *Wählen Sie eine Zahl zwischen 1 und 99 aus.* | Die Zahlen zwischen 1 und 99 sind für benutzerdefinierte Synchronisierungsregeln reserviert. Wählen Sie keinen Wert aus, der von einer anderen Synchronisierungsregel verwendet wird. |

5. Navigieren Sie zur Registerkarte **Bereichsfilter**, und fügen Sie eine **einzelne Bereichsfiltergruppe mit folgender Klausel hinzu**:
 
    | Attribut | Operators | Wert |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Benutzer\_ | 
 
    Der Bereichsfilter legt fest, auf welche lokalen AD-Objekte diese Synchronisierungsregel für eingehende Daten angewendet wird. In diesem Beispiel verwenden wir denselben Bereichsfilter wie die OOB-Synchronisierungsregel *In from AD – User Common*, die verhindert, dass die Synchronisierungsregel auf Benutzerobjekte angewendet wird, die über die Funktion zum Rückschreiben von Azure AD-Benutzern erstellt wurden. Möglicherweise müssen Sie den Bereichsfilter entsprechend Ihrer Azure AD Connect-Bereitstellung anpassen.

6. Navigieren Sie zur Registerkarte **Transformation**, und implementieren Sie folgende Transformationsregel:
 
    | Flowtyp | Zielattribut | Quelle | Einmal anwenden | Zusammenführungstyp |
    | --- | --- | --- | --- | --- |
    | Direkt | PreferredDataLocation | Wählen Sie das Quellattribut aus. | Deaktiviert | Aktualisieren |

7. Klicken Sie auf **Hinzufügen**, um die Regel für eingehende Daten zu erstellen.

![Erstellen einer Synchronisierungsregel für eingehende Daten](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

<a id="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad" class="xliff"></a>

### Schritt 5: Erstellen einer Synchronisierungsregel für ausgehende Daten zur Übertragung des Attributwerts an Azure AD
Die Synchronisierungsregel für ausgehende Daten ermöglicht die Übertragung des Attributwerts von der Metaverse auf das Attribut „PreferredDataLocation“ in Azure AD:

1. Navigieren Sie zum **Synchronisierungsregel-Editor**.

2. Legen Sie den Suchfilter **Richtung** auf **Ausgehend** fest.

3. Klicken Sie auf die Schaltfläche **Neue Regel hinzufügen**.

4. Geben Sie auf der Registerkarte **Beschreibung** die folgende Konfiguration an:

    | Attribut | Wert | Details |
    | --- | --- | --- |
    | Name | *Geben Sie einen Namen ein.* | Beispiel: „Out to AAD – User PreferredDataLocation“ |
    | Beschreibung | *Geben Sie eine Beschreibung ein.* |
    | Verbundenes System | *Wählen Sie den AAD-Connector aus.* |
    | Objekttyp des verbundenen Systems | Benutzer ||
    | Metaverse-Objekttyp | **Person** ||
    | Verknüpfungstyp | **Join** ||
    | Rangfolge | *Wählen Sie eine Zahl zwischen 1 und 99 aus.* | Die Zahlen zwischen 1 und 99 sind für benutzerdefinierte Synchronisierungsregeln reserviert. Wählen Sie keinen Wert aus, der von einer anderen Synchronisierungsregel verwendet wird. |

5. Navigieren Sie zur Registerkarte **Bereichsfilter**, und fügen Sie eine **einzelne Bereichsfiltergruppe mit zwei Klauseln hinzu**:
 
    | Attribut | Operators | Wert |
    | --- | --- | --- |
    | sourceObjectType | EQUAL | Benutzer |
    | cloudMastered | NOTEQUAL | True  |

    Der Bereichsfilter legt fest, auf welche Azure AD-Objekte diese Synchronisierungsregel für ausgehende Daten angewendet wird. In diesem Beispiel verwenden wir den Bereichsfilter aus der OOB-Synchronisierungsregel „Out to AD – User Identity“. Er verhindert, dass die Synchronisierungsregel auf Benutzerobjekte angewendet wird, die nicht über das lokale Active Directory synchronisiert wurden. Möglicherweise müssen Sie den Bereichsfilter entsprechend Ihrer Azure AD Connect-Bereitstellung anpassen.
    
6. Navigieren Sie zur Registerkarte **Transformation**, und implementieren Sie folgende Transformationsregel:

    | Flowtyp | Zielattribut | Quelle | Einmal anwenden | Zusammenführungstyp |
    | --- | --- | --- | --- | --- |
    | Direkt | PreferredDataLocation | PreferredDataLocation | Deaktiviert | Aktualisieren |

7. Klicken Sie auf **Hinzufügen**, um die Regel für ausgehende Daten zu erstellen.

![Erstellen einer Synchronisierungsregel für ausgehende Daten](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

<a id="step-6-run-full-synchronization-cycle" class="xliff"></a>

### Schritt 6: Ausführen des vollständigen Synchronisierungszyklus
Der vollständige Synchronisierungszyklus ist im Allgemeinen erforderlich, da wir sowohl zum AD- als auch zum Azure AD-Connectorschema neue Attribute hinzugefügt und benutzerdefinierte Synchronisierungsregeln eingeführt haben. Es wird empfohlen, die Änderungen vor dem Export nach Azure AD zu überprüfen. Anhand der folgenden Schritte können Sie die Änderungen überprüfen, während Sie die Schritte des vollständigen Synchronisierungszyklus manuell ausführen. 

1. Führen Sie im **lokalen AD-Connector** den Schritt **Vollständiger Import** aus:

   1. Wechseln Sie in Synchronization Service Manager zur Registerkarte **Vorgänge**.

   2. Klicken Sie mit der rechten Maustaste auf den **lokalen AD-Connector**, und wählen Sie **Ausführen...** aus.

   3. Wählen Sie im Popupdialogfeld **Vollständiger Import** aus, und klicken Sie auf **OK**.
    
   4. Warten Sie, bis der Vorgang abgeschlossen ist.

    > [!NOTE]
    > Sie können den Schritt „Vollständiger Import“ im lokalen AD-Connector überspringen, wenn das Quellattribut bereits in der Liste der importierten Attribute enthalten ist. Mit anderen Worten: Sie mussten bei [Schritt 2 zum Hinzufügen des Quellattributs zum lokalen AD-Connectorschemas](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema) keine Änderungen vornehmen.

2. Führen Sie im **Azure AD-Connector** den Schritt **Vollständiger Import** aus:

   1. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Ausführen...** aus.

   2. Wählen Sie im Popupdialogfeld **Vollständiger Import** aus, und klicken Sie auf **OK**.
   
   3. Warten Sie, bis der Vorgang abgeschlossen ist.

3. Überprüfen Sie die Änderungen an den Synchronisierungsregeln zu einem vorhandenen Benutzerobjekt:

Das Quellattribut aus dem lokalen Active Directory und „PreferredDataLocation“ aus Azure AD wurden in den jeweiligen Connectorbereich importiert. Bevor Sie mit dem Schritt „Vollständige Synchronisierung“ fortfahren, wird empfohlen, eine **Vorschau** zu einem vorhandenen Benutzerobjekt im lokalen AD-Connectorbereich auszuführen. In dem von Ihnen ausgewählten Objekt muss das Quellattribut aufgefüllt sein. Eine erfolgreiche **Vorschau**, bei dem „PreferredDataLocation“ in der Metaverse aufgefüllt ist, ist ein guter Indikator dafür, dass Sie die Synchronisierungsregeln korrekt konfiguriert haben. Informationen zum Durchführen einer **Vorschau** finden Sie im Abschnitt [Überprüfen der Änderungen](#verify-the-change).

4. Führen Sie im **lokalen AD-Connector** den Schritt **Vollständige Synchronisierung** aus:

   1. Klicken Sie mit der rechten Maustaste auf den **lokalen AD-Connector**, und wählen Sie **Ausführen...** aus.
  
   2. Wählen Sie im Popupdialogfeld **Vollständige Synchronisierung** aus, und klicken Sie auf **OK**.
   
   3. Warten Sie, bis der Vorgang abgeschlossen ist.

5. Stellen Sie sicher, dass **Ausstehende Exporte** in Azure AD festgelegt ist, indem Sie wie folgt vorgehen:

   1. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Connectorbereich durchsuchen** aus.

   2. Gehen Sie im Popupdialogfeld „Connectorbereich durchsuchen“ wie folgt vor:

      1. Legen Sie den **Bereich** auf **Ausstehender Export** fest.
      
      2. Aktivieren Sie alle drei Kontrollkästchen: **Hinzufügen, Ändern und Löschen**.
      
      3. Klicken Sie auf die Schaltfläche **Durchsuchen**, um die Liste von Objekten mit Änderungen, die exportiert werden sollen, abzurufen. Um die Änderungen für ein bestimmtes Objekt zu untersuchen, doppelklicken Sie auf das Objekt.
      
      4. Stellen Sie sicher, dass die Änderungen Ihren Erwartungen entsprechen.

6. Führen Sie im **Azure AD-Connector** den Schritt **Exportieren** aus.
      
   1. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Ausführen...** aus.
   
   2. Wählen Sie im Popupdialogfeld „Connector ausführen“ den Schritt **Exportieren** aus, und klicken Sie auf **OK**.
   
   3. Warten Sie, bis der Export nach Azure AD abgeschlossen ist.

> [!NOTE]
> Sie werden möglicherweise feststellen, dass die Schritte „Vollständige Synchronisierung“ und „Exportieren“ zum Azure AD-Connector nicht enthalten sind. Diese Schritte sind nicht erforderlich, da die Attributwerte lediglich von dem lokalen Active Directory auf Azure AD übertragen werden.

<a id="step-7-re-enable-sync-scheduler" class="xliff"></a>

### Schritt 7: Erneutes Aktivieren des Synchronisierungsplaners
So aktivieren Sie erneut den integrierten Synchronisierungsplaner:

1. Starten Sie eine PowerShell-Sitzung.

2. Aktivieren Sie die geplante Synchronisierung durch Ausführung des Cmdlets `Set-ADSyncScheduler -SyncCycleEnabled $true` wieder.



<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Weitere Informationen zum Konfigurationsmodell finden Sie unter [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md)(Grundlegendes zur deklarativen Bereitstellung).
* Weitere Informationen zur Ausdruckssprache finden Sie unter [Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

