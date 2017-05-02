---
title: Lotus Domino-Connector | Microsoft Docs
description: "Dieser Artikel beschreibt die Konfiguration des Lotus Domino-Connectors von Microsoft."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: aebe0b74c952045375e264bed88d33d936e34b92
ms.lasthandoff: 03/14/2017


---
# <a name="lotus-domino-connector-technical-reference"></a>Technische Referenz für den Lotus Domino-Connector
Dieser Artikel beschreibt den Lotus Domino-Connector. Der Artikel bezieht sich auf folgende Produkte:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Mindestens Hotfix 4.1.3671.0 ( [KB3092178](https://support.microsoft.com/kb/3092178)) erforderlich.

Der Connector steht für MIM2016 und FIM2010R2 im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495)zum Download zur Verfügung.

## <a name="overview-of-the-lotus-domino-connector"></a>Übersicht über den Lotus Domino-Connector
Der Lotus Domino-Connector ermöglicht die Integration des Synchronisierungsdiensts in den Lotus Domino-Server von IBM.

Im Anschluss finden Sie einen allgemeinen Überblick über die von der aktuellen Connector-Version unterstützten Features:

| Feature | Support |
| --- | --- |
| Verbundene Datenquelle |Server:  <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Client:<li>Lotus Domino 8.5.x</li><li>Lotus Notes 9.x</li> |
| Szenarios |<li>Objektlebenszyklusverwaltung</li><li>Gruppenverwaltung</li><li>Kennwortverwaltung</li> |
| Vorgänge |<li>Vollständiger Import und Deltaimport</li><li>Export</li><li>Kennwort auf HTTP-Kennwort festlegen und ändern</li> |
| Schema |<li>Person (Roamingbenutzer, Kontakt (Person ohne Zertifikat))</li><li>Group</li><li>Ressource (Ressource, Raum, Onlinebesprechung)</li><li>Mail-In-Datenbank</li><li>Dynamische Ermittlung von Attributen für unterstützte Objekte</li> |

Der Lotus Domino-Connector verwendet für die Kommunikation mit dem Lotus Domino-Server den Lotus Notes-Client. Aufgrund dieser Abhängigkeit muss auf dem Synchronisierungsserver ein unterstützter Lotus Notes-Client installiert sein. Die Kommunikation zwischen Client und Server wird über die .NET-Interop-Schnittstelle für Lotus Notes (Interop.domino.dll) implementiert. Diese Schnittstelle ermöglicht die Kommunikation zwischen der Microsoft-.NET-Plattform und dem Lotus Notes-Client und unterstützt den Zugriff auf Lotus Domino-Dokumente und -Sichten. Abhängig von der ausgewählten Deltaimportmethode kann für den Deltaimport ggf. auch die native C++-Schnittstelle verwendet werden.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie den Connector verwenden, stellen Sie sicher, dass auf dem Synchronisierungsserver folgende Voraussetzungen erfüllt sind:

* Microsoft .NET 4.5.2 Framework oder eine höhere Version
* Auf dem Synchronisierungsserver muss der Lotus Notes-Client installiert sein.
* Für den Lotus Domino-Connector muss auf dem Domino-Verzeichnisserver die standardmäßige Lotus Domino-LDAP-Schemadatenbank (schema.nsf) vorhanden sein. Sollte sie nicht vorhanden sein, können Sie sie installieren, indem Sie auf dem Domino-Server den LDAP-Dienst ausführen oder neu starten.

### <a name="connected-data-source-permissions"></a>Berechtigungen für die verbundene Datenquelle
Zum Ausführen der unterstützten Vorgänge des Lotus Domino-Connectors müssen Sie folgenden Gruppen angehören:

* Administratoren mit Vollzugriff
* Administratoren
* Datenbankadministratoren

Die folgende Tabelle gibt Aufschluss über die erforderlichen Berechtigungen für die einzelnen Vorgänge:

| Vorgang | Zugriffsrechte |
| --- | --- |
| Importieren |<li>Öffentliche Dokumente lesen</li><li> Administrator mit Vollzugriff (als Administrator mit Vollzugriff haben Sie automatisch effektiven Zugriff in der Zugriffssteuerungsliste.)</li> |
| Exportieren und Kennwort festlegen |Effektiver Zugriff:  <li>Erstellen eines Dokuments</li><li>Dokumente löschen</li><li>Öffentliche Dokumente lesen</li><li>Öffentliche Dokumente schreiben</li><li>Dokumente replizieren oder kopieren</li>Für Exportvorgänge benötigen Sie zudem folgende Rollen: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Direkte Vorgänge und AdminP
Vorgänge werden entweder direkt oder aber über den AdminP-Prozess an das Domino-Verzeichnis weitergeleitet. Die folgenden Tabellen enthalten alle unterstützten Objekte, Vorgänge und (sofern zutreffend) die dazugehörige Implementierungsmethode:

**Primäres Adressbuch**

| Objekt | Erstellen | Aktualisieren | Löschen |
| --- | --- | --- | --- |
| Person |AdminP |Direkt |AdminP |
| Group |AdminP |Direkt |AdminP |
| Mail-In-Datenbank |Direkt |Direkt |Direkt |
| Ressource |AdminP |Direkt |AdminP |

**Sekundäres Adressbuch**

| Objekt | Erstellen | Aktualisieren | Löschen |
| --- | --- | --- | --- |
| Person |– |Direkt |Direkt |
| Group |Direkt |Direkt |Direkt |
| Mail-In-Datenbank |Direkt |Direkt |Direkt |
| Ressource |– |N/V |– |

Beim Erstellen einer Ressource wird ein Notes-Dokument erstellt. Analog dazu wird beim Löschen einer Ressource auch das Notes-Dokument gelöscht.

### <a name="ports-and-protocols"></a>Ports und Protokolle
Der IBM Lotus Notes-Client und die Domino-Server kommunizieren per NRPC (Notes Remote Procedure Call, NRPC). Für NRPC muss TCP/IP verwendet werden. Die Standardportnummer 1352 kann vom Domino-Administrator geändert werden.

### <a name="not-supported"></a>Nicht unterstützt
Die folgenden Vorgänge werden von der aktuellen Version des Lotus Domino-Connectors nicht unterstützt:

* Verschieben des Postfachs zwischen Servern

## <a name="create-a-new-connector"></a>Erstellen eines neuen Connectors
### <a name="client-software-installation-and-configuration"></a>Installieren und Konfigurieren der Clientsoftware
Lotus Notes muss auf dem Server installiert sein, **bevor** der Connector installiert wird.

Führen Sie eine Einzelbenutzerinstallation ****aus. Die standardmäßige Mehrbenutzerinstallation **** kann nicht verwendet werden.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Installieren Sie auf der Featureseite nur die erforderlichen Lotus Notes-Features und die einmalige ****Clientanmeldung. Einmaliges Anmelden ist für die Anmeldung des Connectors beim Domino-Server erforderlich.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Hinweis:** Starten Sie Lotus Notes einmalig mit einem Benutzer, der sich auf demselben Server befindet wie das Konto, das sie als Dienstkonto des Connectors verwenden. Stellen Sie auch sicher, dass der Lotus Notes-Client auf dem Server geschlossen ist. Der Client darf nicht ausgeführt werden, wenn der Connector versucht, eine Verbindung mit dem Domino-Server herzustellen.

### <a name="create-connector"></a>Connectorerstellung
Wählen Sie zum Erstellen eines Lotus Domino-Connectors im **Synchronisierungsdienst** die Option **Verwaltungs-Agent** und anschließend **Erstellen** aus. Wählen Sie den Connector **Lotus Domino (Microsoft)** aus.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Falls Ihre Synchronisierungsdienstversion eine Möglichkeit zum Konfigurieren der **Architektur** bietet, stellen Sie sicher, dass der Connector auf den Standardwert für die Ausführung in **Prozess** festgelegt ist.

### <a name="connectivity"></a>Konnektivität
Auf der Konnektivitätsseite müssen Sie den Namen des Lotus Domino-Servers sowie die Anmeldeinformationen angeben.  
![Konnektivität](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Die Domino-Servereigenschaft unterstützt zwei Formate für den Server:

* Servername
* Servername/Verzeichnisname

Das Format **Servername/Verzeichnisname** wird für dieses Attribut bevorzugt, da es die Reaktionsgeschwindigkeit erhöht, wenn der Connector den Domino-Server kontaktiert.

Die angegebene Benutzer-ID-Datei wird in der Konfigurationsdatenbank des Synchronisierungsdiensts gespeichert.

Für den Deltaimport **** stehen folgende Optionen zur Verfügung:

* **Keine**. Der Connector führt keine Deltaimporte aus.
* **Hinzufügen/Aktualisieren**. Der Connector führt Deltaimporte für Hinzufüge- und Aktualisierungsvorgänge aus. Für Löschvorgänge ist ein vollständiger Import **** erforderlich. Bei diesem Vorgang wird das .NET-Interop verwendet.
* **Hinzufügen/Aktualisieren/Löschen**. Der Connector führt Deltaimporte für Hinzufüge-, Aktualisierungs- und Löschvorgänge aus. Bei diesem Vorgang werden die nativen C++-Schnittstellen verwendet.

Folgende Schemaoptionen **** stehen zur Auswahl:

* **Standardschema**. Der Connector erkennt das Schema vom Domino-Server. Diese Auswahl ist die Standardoption.
* **DSML-Schema**. Wird nur verwendet, wenn der Domino-Server das Schema nicht verfügbar macht. In diesem Fall können Sie eine DSML-Datei mit dem Schema erstellen und das Schema stattdessen importieren. Weitere Informationen zu DSML finden Sie unter [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Wenn Sie auf „Weiter“ klicken, werden die Konfigurationsparameter für Benutzer-ID und Kennwort überprüft.

### <a name="global-parameters"></a>Globale Parameter
Die Seite mit den globalen Parametern dient zum Konfigurieren der Zeitzone und der Option für Import- und Exportvorgänge.  
![Globale Parameter](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Der Parameter **Domino Server-Serverzeitzone** definiert den Standort Ihres Domino-Servers.

Diese Konfigurationsoption wird für Vorgänge vom Typ **Deltaimport** benötigt, da der Synchronisierungsdienst so Veränderungen zwischen den letzten beiden Importen ermitteln kann.

>[!Note]
Seit dem Update vom März 2017 beinhaltet der Bildschirm mit den globalen Parametern die Option zum Löschen der E-Mail-Datenbank des Benutzers während der Löschung des Benutzers.

![Löschen des Postfachs eines Benutzers](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Importeinstellungen, Methode
Für **Vollständigen Import durchführen nach** stehen folgende Optionen zur Verfügung:

* Suche
* Sicht (empfohlen)

**Suche** nutzt die Indizierung in Domino, die Indizes werden jedoch häufig nicht in Echtzeit aktualisiert, und die vom Server zurückgegebenen Daten sind nicht immer korrekt. Für Systeme mit vielen Änderungen eignet sich diese Option in der Regel nicht gut, und in bestimmten Fällen führt sie sogar zu ungewollten Löschvorgängen. **Suche** ist allerdings schneller als **Sicht**.

**Sicht** wird empfohlen, da diese Option den korrekten Zustand der Daten liefert. Sie ist geringfügig langsamer als „Suche“.

#### <a name="creation-of-virtual-contact-objects"></a>Erstellen virtueller Contact-Objekte
Für **Erstellen des\_Contact-Objekts aktivieren** stehen folgende Optionen zur Verfügung:

* Keine
* Werte ohne Verweis
* Verweiswerte und Werte ohne Verweis

In Domino können Verweisattribute viele verschiedene Formate für den Verweis auf andere Objekte enthalten. Zur Darstellung verschiedener Varianten implementiert der Connector \_Contact-Objekte, die auch als **virtuelle Kontakte** (Virtual Contacts, VCs) bezeichnet werden. Diese Objekte werden erstellt, damit sie mit vorhandenen MV-Objekten verknüpft oder als neue Objekte projiziert werden können. Auf diese Weise können Attributverweise beibehalten werden.

Wenn Sie diese Einstellung aktivieren und es sich beim Inhalt eines Verweisattributs nicht um ein DN-Format handelt, wird ein \_Contact-Objekt erstellt. Ein Mitgliedsattribut einer Gruppe kann beispielsweise SMTP-Adressen enthalten. In Verweisattributen können auch „shortName“ und andere Attribute vorhanden sein. Wählen Sie für dieses Szenario die Option **Werte ohne Verweis**aus. Diese Konfiguration ist die am häufigsten verwendete Einstellung für Domino-Implementierungen.

Wenn Lotus Domino für die Verwendung separater Adressbücher mit unterschiedlichen Distinguished Names konfiguriert ist, die jeweils das gleiche Objekt darstellen, können auch \_Contact-Objekte für alle in einem Adressbuch gefundenen Verweiswerte erstellt werden. Wählen Sie in diesem Szenario die Option **Verweiswerte und Werte ohne Verweis** aus.

Wenn das Attribut **FullName** in Domino mehrere Werte besitzt, müssen Sie auch die Erstellung virtueller Kontakte aktivieren, damit Verweise aufgelöst werden können. Dieses Attribut kann nach einer Zusammenführung oder Trennung z.B. mehrere Werte aufweisen. Aktivieren Sie in diesem Szenario das Kontrollkästchen **Erstellen... aktivieren, wenn „FullName“ mehrere Werte besitzt**.

Durch die Verknüpfung mit den richtigen Attributen werden die \_Contact-Objekte mit dem Metaverse-Objekt verknüpft.

Dem DN dieser Objekte wird „VC=\_Contact“ hinzugefügt.

#### <a name="import-settings-conflict-object"></a>Importeinstellungen, Konfliktobjekt
**Konfliktobjekt ausschließen**

In einer großen Domino-Implementierung kann es aufgrund von Replikationsproblemen vorkommen, dass mehrere Objekte den gleichen DN besitzen. In diesen Fällen liegen dem Connector zwei Objekte mit unterschiedlichen UniversalID-Werten, aber gleichem DN vor. Dieser Konflikt hätte die Erstellung eines kurzlebigen Objekts im Connectorbereich zur Folge. Der Connector kann Objekte ignorieren, die in Domino als Replikationsopfer ausgewählt wurden. Es wird empfohlen, dieses Kontrollkästchen nicht zu deaktivieren.

#### <a name="export-settings"></a>Exporteinstellungen
Wenn die Option **AdminP zum Aktualisieren von Verweisen verwenden** deaktiviert ist, werden Verweisattribute wie etwa ein Mitgliedsattribut mit einem direkten Aufruf ohne Verwendung des AdminP-Prozesses exportiert. Verwenden Sie diese Option nur, wenn AdminP nicht konfiguriert wurde, um die referenzielle Integrität zu gewährleisten.

#### <a name="routing-information"></a>Routinginformationen
In Domino können Routinginformationen als DN-Suffix in ein Verweisattribut eingebettet werden. So kann beispielsweise das Mitgliedsattribut in einer Gruppe **CN=example/organization@ABC** enthalten. Das Suffix @ABC ist die Routinginformation. Routinginformationen dienen in Domino zum Senden von E-Mails an das richtige Domino-System. Hierbei kann es sich um ein System in einer anderen Organisation handeln. Im Feld für Routinginformationen können Sie die Routingsuffixe angeben, die innerhalb der Organisation im Connectorbereich verwendet werden. Wird einer dieser Werte als Suffix in einem Verweisattribut gefunden, wird die Routinginformation aus dem Verweis entfernt. Falls das Routingsuffix eines Verweiswerts keiner der angegebenen Optionen entspricht, wird ein \_Contact-Objekt erstellt. Bei der Erstellung dieser \_Contact-Objekte wird **RO=@<RoutingSuffix>** in den DN eingefügt. Außerdem werden für diese \_Contact-Objekte folgende Attribute hinzugefügt, um ggf. die Verknüpfung mit einem echten Objekt zu ermöglichen: „\_routingName“, „\_contactName“, „\_displayName“ und „UniversalID“.

#### <a name="additional-address-books"></a>Zusätzliche Adressbücher
Falls keine **Directory Assistance** zur Angabe des Namens sekundärer Adressbücher installiert ist, können Sie die Adressbücher manuell eingeben.

#### <a name="multivalued-transformation"></a>Mehrwertige Transformation
Viele Attribute in Lotus Domino sind mehrwertig. Die entsprechenden Metaverse-Attribute sind in der Regel einwertig. Durch Konfigurieren der Option für den Import- und Exportvorgang kann der Connector zur erforderlichen Umwandlung der betroffenen Attribute beitragen.

**Export**  
Die Option für den Exportvorgang unterstützt zwei Modi:

* Element anfügen
* Element ersetzen

**Element ersetzen** : Bei Verwendung dieser Option entfernt der Connector immer die aktuellen Werte des Attributs in Domino und ersetzt sie durch die angegebenen Werte. Die angegebenen Werte können ein- oder mehrwertig sein.

Beispiel: Das Assistentenattribut eines Personenobjekts hat folgende Werte:

* CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
* CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

Wird diesem Personenobjekt nun ein neuer Assistent namens **David Alexander** zugewiesen, sieht das Ergebnis wie folgt aus:

* CN=David Alexander/OU= Contoso /O=Americas,NAB=names.nsf

**Element anfügen** : Bei Verwendung dieser Option behält der Connector die vorhandenen Werte des Attributs in Domino bei und fügt am Anfang der Datenliste neue Werte ein.

Beispiel: Das Assistentenattribut eines Personenobjekts hat folgende Werte:

* CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
* CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

Wird diesem Personenobjekt nun ein neuer Assistent namens **David Alexander** zugewiesen, sieht das Ergebnis wie folgt aus:

* CN=David Alexander/OU= Contoso /O=Americas,NAB=names.nsf
* CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
* CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

**Import**  
Die Option für den Importvorgang unterstützt zwei Modi:

* Standard
* Mehrwertig zu einwertig

**Standard** : Bei Verwendung der Option „Standard“ werden sämtliche Werte aller Attribute importiert.

**Mehrwertig zu einwertig** : Bei Verwendung dieser Option wird ein mehrwertiges Attribut in ein einwertiges Attribut konvertiert. Sind mehrere Werte vorhanden, wird der oberste Wert verwendet (dies ist meist auch der neueste Wert).

Beispiel: Das Assistentenattribut eines Personenobjekts hat folgende Werte:

* CN=David Alexander/OU= Contoso /O=Americas,NAB=names.nsf
* CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
* CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

Die neueste Aktualisierung dieses Attributs ist **David Alexander**. Da die Importvorgangsoption auf „Mehrwertig zu einwertig“ festgelegt ist, importiert der Connector nur **David Alexander** in den Connectorbereich.

Die Logik für die Konvertierung mehrwertiger Attribute in einwertige Attribute gilt nicht für das Gruppenmitgliedsattribut und für das Attribut „FullName“ einer Person.

Es ist auch möglich, Import- und Exporttransformationsregeln für mehrwertige Attribute pro Attribut (als Ausnahme für die globale Regel) zu konfigurieren. Geben Sie zum Konfigurieren dieser Option „[Objekttyp].[Attributname]“ in die Textfelder **Attributausschlussliste (Import)** und **Attributausschlussliste (Export)** ein. Wenn Sie also beispielsweise „Person.Assistant“ eingeben und das globale Flag auf den Import aller Werte festgelegt ist, wird für den Assistenten nur der erste Wert importiert.

#### <a name="certifiers"></a>Zertifizierer
Alle Organisationen/Organisationseinheiten werden vom Connector aufgeführt. Zum Exportieren von Personenobjekten in das primäre Adressbuch wird ein Zertifizierer mit dazugehörigem Kennwort benötigt.

Wenn alle Zertifizierer das gleiche Kennwort verwenden, kann die Option **Kennwort für alle Zertifizierer** verwendet werden. In diesem Fall können Sie hier das Kennwort eingeben und nur die Zertifiziererdatei angeben.

Bei reinen Importvorgängen muss kein Zertifizierer angegeben werden.

### <a name="configure-provisioning-hierarchy"></a>Konfigurieren der Bereitstellungshierarchie
Überspringen Sie diese Seite beim Konfigurieren des Lotus Domino-Connectors. Der Lotus Domino-Connector unterstützt hierarchische Bereitstellungen nicht.  
![Bereitstellungshierarchie](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurieren von Partitionen und Hierarchien
Wenn Sie Partitionen und Hierarchien konfigurieren, müssen Sie das primäre Adressbuch namens „NAB=names.nsf“ auswählen. Zusätzlich zum primären Adressbuch können Sie auch sekundäre Adressbücher auswählen (sofern vorhanden).  
![Partitionen](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Auswählen von Attributen
Wenn Sie Ihre Attribute konfigurieren, müssen Sie alle Attribute mit dem Präfix **\_MMS\_** auswählen. Diese Attribute sind erforderlich, wenn Sie neue Objekte für Lotus Domino bereitstellen.

![Attribute](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objektlebenszyklusverwaltung
Dieser Abschnitt enthält eine Übersicht über die verschiedenen Objekte in Domino.

### <a name="person-objects"></a>Personenobjekte
Das Personenobjekt stellt Benutzer in der Organisation und in Organisationseinheiten dar. Zusätzlich zu den Standardattributen kann der Domino-Administrator einem Personenobjekt auch benutzerdefinierte Attribute hinzufügen. Ein Personenobjekt muss mindestens alle erforderlichen Attribute enthalten. Eine vollständige Liste mit erforderlichen Attributen finden Sie unter [Lotus Notes-Eigenschaften](#lotus-notes-properties). Die Registrierung eines Personenobjekts setzt Folgendes voraus:

* Das Adressbuch (names.nsf) muss definiert worden sein, und es muss sich um das primäre Adressbuch handeln.
* Sie müssen über die O-/OE-Zertifizierer-ID und das Kennwort für die Registrierung eines bestimmten Benutzers in der Organisation/Organisationseinheit verfügen.
* Sie müssen einen spezifischen Satz von Lotus Notes-Eigenschaften für ein Personenobjekt definieren. Diese Eigenschaften werden für die Bereitstellung des Personenobjekts verwendet. Weitere Informationen finden Sie weiter unten in diesem Dokument im Abschnitt [Lotus Notes-Eigenschaften](#lotus-notes-properties).
* Das ursprüngliche HTTP-Kennwort für eine Person ist ein Attribut und wird bei der Bereitstellung festgelegt.
* Für Personenobjekte werden folgende Typen unterstützt:
  1. Normaler Benutzer mit einer Mail- und einer Benutzer-ID-Datei
  2. Roamingbenutzer (ein normaler Benutzer mit allen Roamingdatenbankdateien)
  3. Kontakte (Benutzer ohne ID-Datei)

Mit Ausnahme von Kontakten können Personen weiter in US-Benutzer und internationale Benutzer unterteilt werden (gemäß der Definition durch den Wert der Eigenschaft „\_MMS\_IDRegType“). Diese Personen greifen mithilfe des Notes-Clients auf Lotus Domino-Server zu, besitzen eine Notes-ID und verfügen über ein Personendokument. Bei Verwendung von Notes Mail besitzen sie außerdem eine Maildatei. Zur Aktivierung muss der Benutzer registriert werden. Weitere Informationen finden Sie unter:

* [Einrichten von Notes-Benutzern](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Benutzerregistrierung](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Verwalten von Benutzern](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Umbenennen von Benutzern](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Alle diese Vorgänge werden in Lotus Domino ausgeführt und anschließend in den Synchronisierungsdienst importiert.

### <a name="resources-and-rooms"></a>Ressourcen und Räume
Eine Ressource ist eine andere Art von Datenbank in Lotus Domino. Bei Ressourcen kann es sich um Konferenzräume mit unterschiedlichen Arten von Ausstattung (z.B. Projektoren) handeln. Der Lotus Domino-Connector unterstützt Untertypen von Ressourcen, die anhand des Ressourcentypattributs definiert werden:

| Ressourcentyp | Ressourcentypattribut |
| --- | --- |
| Raum |1 |
| Ressource (Sonstiges) |2 |
| Onlinebesprechung |3 |

Für die ordnungsgemäße Verwendung des Ressourcenobjekttyps ist Folgendes erforderlich:

* Die Ressourcenreservierungsdatenbank muss bereits auf dem verbundenen Domino-Server vorhanden sein.
* Der Standort für die Ressource muss bereits definiert sein.

Die Ressourcenreservierungsdatenbank enthält drei Arten von Dokumenten:

* Standortprofil
* Ressource
* Reservierung

Weitere Informationen zum Einrichten der Ressourcenreservierungsdatenbank finden Sie im Thema zum [Einrichten der Ressourcenreservierungsdatenbank](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Erstellen, Aktualisieren und Löschen von Ressourcen**  
Die Erstellungs-, Aktualisierungs- und Löschvorgänge werden vom Lotus Domino-Connector in der Ressourcenreservierungsdatenbank ausgeführt. Ressourcen werden als Dokumente in „Names.nsf“ (dem primären Adressbuch) erstellt. Weitere Informationen zum Bearbeiten und Löschen von Ressourcen finden Sie unter [Bearbeiten und Löschen von Ressourcendokumenten](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importieren und Exportieren von Ressourcen**  
Die Ressourcen können genau wie andere Objekttypen in den Synchronisierungsdienst importiert und daraus exportiert werden. Wählen Sie den Objekttyp während der Konfiguration als Ressource aus. Halten Sie für einen erfolgreichen Exportvorgang Details zu Ressourcentyp, Konferenzdatenbank und Standortname bereit.

### <a name="mail-in-databases"></a>Mail-In-Datenbanken
Eine Mail-In-Datenbank ist eine Datenbank für den Empfang von E-Mails. Diese Datenbank ist ein Lotus Domino-Postfach, das keinem bestimmten Lotus Domino-Benutzerkonto zugeordnet ist (d.h. es besitzt keine eigene ID-Datei und kein eigenes Kennwort). Einer Mail-In-Datenbank sind eine eindeutige Benutzer-ID (Kurzname) und eine eigene E-Mail-Adresse zugeordnet.

Falls ein separates Postfach mit eigener E-Mail-Adresse benötigt wird, die von verschiedenen Benutzern gemeinsam genutzt werden kann (z.B. „group@contoso.com“), wird eine Mail-In-Datenbank erstellt. Der Zugriff auf dieses Postfach wird mithilfe der entsprechenden Zugriffssteuerungsliste (Access Control List, ACL), gesteuert. Diese enthält die Namen der Notes-Benutzer, die zum Öffnen des Postfachs berechtigt sind.

Eine Liste mit den erforderlichen Attributen finden Sie weiter unten in diesem Artikel im Abschnitt [Erforderliche Attribute](#mandatory-attributes) .

Wenn eine Datenbank für den E-Mail-Empfang konzipiert wird, wird in Lotus Domino ein Mail-In-Datenbankdokument erstellt. Dieses Dokument muss im Domino-Verzeichnis jedes Servers vorhanden sein, auf dem eine Kopie der Datenbank gespeichert wird. Eine ausführliche Beschreibung der Erstellung eines Mail-In-Datenbankdokuments finden Sie im Thema zum [Erstellen eines Mail-In-Datenbankdokuments](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Vor dem Erstellen einer Mail-In-Datenbank muss die Datenbank bereits auf dem Domino-Server vorhanden sein (sollte vom Lotus-Administrator erstellt worden sein).

### <a name="group-management"></a>Gruppenverwaltung
Eine ausführliche Übersicht über die Lotus Domino-Gruppenverwaltung finden Sie in den folgenden Ressourcen:

* [Verwenden von Gruppen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Erstellen einer Gruppe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Erstellen und Ändern von Gruppen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Verwalten von Gruppen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Umbenennen einer Gruppe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Kennwortverwaltung
Für einen registrierten Lotus Domino-Benutzer gibt es zwei Arten von Kennwörtern:

1. Benutzerkennwort (gespeichert in der Datei „User.id“)
2. Internet-/HTTP-Kennwort

Der Lotus Domino-Connector unterstützt nur Vorgänge mit HTTP-Kennwort.

Zur Verwaltung von Kennwörtern müssen Sie im Verwaltungs-Agent-Designer die Kennwortverwaltung für den Connector aktivieren. Wählen Sie dazu auf der Dialogseite **Erweiterungen konfigurieren** die Option **Kennwortverwaltung aktivieren** aus.  
![Kennwortverwaltung aktivieren](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Der Lotus Domino-Connector unterstützt folgende Vorgänge für das Internetkennwort:

* Kennwort festlegen: Legt in Domino ein neues HTTP-/Internetkennwort für den Benutzer fest. Das Konto wird außerdem standardmäßig entsperrt. Das Entsperr-Flag wird über die WMI-Schnittstelle des Synchronisierungsmoduls verfügbar gemacht.
* Kennwort ändern: In diesem Szenario möchte ein Benutzer unter Umständen sein Kennwort ändern, oder er wird nach einer bestimmten Zeit zu einer Kennwortänderung aufgefordert. Bei diesem Vorgang muss sowohl das alte als auch das neue Kennwort angegeben werden. Nach erfolgter Änderung wird das neue Kennwort in Lotus Domino aktualisiert.

Weitere Informationen finden Sie unter:

* [Verwenden des Internetsperrfeatures](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Verwalten von Internetkennwörtern](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Verweisinformationen
In diesem Abschnitt werden Attributbeschreibungen und Attributanforderungen für den Lotus Domino-Connector aufgeführt.

### <a name="lotus-notes-properties"></a>Lotus Notes-Eigenschaften
Bei der Bereitstellung von Personenobjekten für Ihr Lotus Domino-Verzeichnis benötigen Ihre Objekte einen spezifischen Satz von Eigenschaften mit bestimmten Werten. Diese Werte sind nur bei Erstellvorgängen erforderlich.

In der folgenden Tabelle werden diese Eigenschaften aufgeführt und beschrieben:

| Eigenschaft | Beschreibung |
| --- | --- |
| \_MMS_AltFullName |Der alternative vollständige Name des Benutzers. |
| \_MMS_AltFullNameLanguage |Die Sprache für die Angabe des alternativen vollständigen Namens des Benutzers. |
| \_MMS_CertDaysToExpire |Die Anzahl von Tagen (ab dem aktuellen Datum) bis zum Ablauf des Zertifikats. Ohne Angabe wird standardmäßig ein Datum verwendet, das vom aktuellen Datum aus zwei Jahre in der Zukunft liegt. |
| \_MMS_Certifier |Eigenschaft, die den Organisationshierarchienamen des Zertifizierers enthält. Beispiel: OU=OrganizationUnit,O=Org,C=Country. |
| \_MMS_IDPath |Ist diese Eigenschaft leer, wird auf dem Synchronisierungsserver keine lokale Benutzer-ID-Datei erstellt. Enthält die Eigenschaft einen Dateinamen, wird eine Benutzer-ID-Datei im Ordner „madata“ erstellt. Die Eigenschaft kann auch einen vollständigen Pfad enthalten. |
| \_MMS_IDRegType |Personen können als Kontakte, US-Benutzer und internationale Benutzer klassifiziert werden. Die folgende Tabelle enthält die möglichen Werte: <li>0 – Kontakt</li><li>1 – US-Benutzer</li><li>2 – Internationaler Benutzer</li> |
| \_MMS_IDStoreType |Erforderliche Eigenschaft für US-Benutzer und internationale Benutzer. Die Eigenschaft enthält einen Ganzzahlwert, der angibt, ob die Benutzer-ID als Anlage im Notes-Adressbuch oder in der Maildatei der Person gespeichert wird. Wenn es sich bei der Benutzer-ID-Datei um eine Anlage im Adressbuch handelt, kann sie optional mit „\_MMS_IDPath“ als Datei erstellt werden. <li>Leer: Die ID-Datei wird im ID-Tresor gespeichert./Es gibt keine ID-Datei (für Kontakte).</li><li> 1: Anlage im Notes-Adressbuch. Für Benutzer-ID-Dateien, bei denen es sich um Anlagen handelt, muss die Eigenschaft „\_MMS_Password“ festgelegt werden.</li><li>2: Speichert die ID in der Maildatei der Person. „\_MMS_UseAdminP“ muss auf „false“ festgelegt werden, damit die Maildatei bei der Registrierung der Person erstellt werden kann. Für Benutzer-ID-Dateien muss die Eigenschaft „\_MMS_Password“ festgelegt werden.</li> |
| \_MMS_MailQuotaSizeLimit |Die für die E-Mail-Dateidatenbank zulässige Anzahl von Megabytes. |
| \_MMS_MailQuotaWarningThreshold |Die Anzahl von Megabytes für die E-Mail-Dateidatenbank, bei deren Überschreitung eine Warnung ausgegeben wird. |
| \_MMS_MailTemplateName |Die E-Mail-Vorlagendatei, die zum Erstellen der E-Mail-Datei des Benutzers verwendet wird. Ist eine Vorlage angegeben, wird die Maildatei unter Verwendung der angegebenen Vorlage erstellt. Ist keine Vorlage angegeben, wird die Datei auf der Grundlage der Standardvorlagendatei erstellt. |
| \_MMS_OU |Optionale Eigenschaft zum Angeben des Namens der Organisationseinheit unter dem Zertifizierer. Bei Kontakten muss diese Eigenschaft leer sein. |
| \_MMS_Password |Erforderliche Eigenschaft für Benutzer. Die Eigenschaft enthält das Kennwort für die ID-Datei des Objekts. |
| \_MMS_UseAdminP |Legen Sie diese Eigenschaft auf „true“ fest, wenn die Maildatei durch den AdminP-Prozess auf dem Domino-Server (und damit asynchron zum Exportvorgang) erstellt werden soll. Ist die Eigenschaft auf „false“ festgelegt, wird die Maildatei mit dem Domino-Benutzer (synchron im Exportvorgang) erstellt. |

Bei einem Benutzer mit zugeordneter ID-Datei muss die Eigenschaft „\_MMS_Password“ einen Wert enthalten. Für den E-Mail-Zugriff über den Lotus Notes-Client müssen die Eigenschaften „MailServer“ und „MailFile“ eines Benutzers einen Wert enthalten.

Für den E-Mail-Zugriff über einen Webbrowser müssen die folgenden Eigenschaften Werte enthalten:

* MailFile: Erforderliche Eigenschaft, die den Pfad auf dem Lotus Domino-Server enthält, an dem die Maildatei gespeichert ist.
* MailServer: Erforderliche Eigenschaft, die den Namen des Lotus Domino-Servers enthält. Bei diesem Wert handelt es sich um den Namen, der beim Erstellen der Lotus-Maildatei auf dem Domino-Server verwendet werden muss.
* HTTPPassword: Optionale Eigenschaft, die das Webzugriffskennwort für das Objekt enthält.

Für den Zugriff auf den Domino-Server ohne E-Mail-Funktion muss die Eigenschaft „HTTPPassword“ einen Wert enthalten. Die Eigenschaften „MailFile“ und „MailServer“ können dagegen leer sein.

Mit „\_MMS_ IDStoreType = 2“ (Speicherung der ID in der Maildatei) wird die Eigenschaft „MailSystem“ von „NotesRegistrationclass“ auf „REG_MAILSYSTEM_INOTES (3)“ festgelegt.

### <a name="mandatory-attributes"></a>Erforderliche Attribute
Der Lotus Domino-Connector unterstützt hauptsächlich die folgenden Arten von Objekten (Dokumenttypen):

* Group
* Mail-In-Datenbank
* Person
* Kontakt (Person ohne Zertifizierer)
* Ressource

In diesem Abschnitt sind die Attribute aufgeführt, die für die einzelnen unterstützten Objekte erforderlich sind, um sie auf einen Domino-Server zu exportieren.

| Objekttyp | Erforderliche Attribute |
| --- | --- |
| Group |<li>ListName</li> |
| Mail-In-Datenbank |<li>FullName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Person |<li>Nachname</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Kontakt (Person ohne Zertifizierer) |<li>\_MMS_IDRegType</li> |
| Ressource |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Website</li><li>displayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Allgemeine Probleme und Fragen
### <a name="schema-detection-does-not-work"></a>Die Schema-Erkennung funktioniert nicht.
Für eine erfolgreiche Schemaerkennung muss die Datei „schema.nsf“ auf dem Domino-Server vorhanden sein. Diese Datei wird nur angezeigt, wenn auf dem Server LDAP installiert ist. Überprüfen Sie Folgendes, falls das Schema nicht erkannt wird:

* Die Datei „schema.nsf“ befindet sich im Stammordner des Domino-Servers.
* Der Benutzer ist zum Anzeigen der Datei „schema.nsf“ berechtigt.
* Erzwingen Sie einen Neustart des LDAP-Servers. Öffnen Sie die **Lotus Domino-Konsole**, und laden Sie das Schema mithilfe des Befehls **Tell LDAP ReloadSchema** neu.

### <a name="not-all-secondary-address-books-are-visible"></a>Es werden nicht alle sekundären Adressbücher angezeigt.
Für die Suche nach sekundären Adressbüchern benötigt der Domino-Connector das Feature **Directory Assistance** . Sollten sekundäre Adressbücher fehlen, überprüfen Sie, ob [Directory Assistance](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) auf dem Domino-Server aktiviert und konfiguriert wurde.

### <a name="custom-attributes-in-domino"></a>Benutzerdefinierte Attribute in Domino
Das Schema kann in Domino auf unterschiedliche Arten erweitert werden, sodass es als benutzerdefiniertes Attribut erscheint, das vom Connector genutzt werden kann.

**Ansatz 1: Erweitern des Lotus Domino-Schemas**

1. Erstellen Sie eine Kopie der Domino-Verzeichnisvorlage {PUBNAMES.NTF}. Die entsprechenden Schritte finden Sie [hier](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html). (Es empfiehlt sich, die standardmäßige IBM Lotus Domino-Verzeichnisvorlage unverändert zu lassen.)
2. Öffnen Sie die Kopie der Domino-Verzeichnisvorlage {CONTOSO.NTF}, die Sie erstellt haben, in Domino Designer, und führen Sie die folgenden Schritte aus:
   * Klicken Sie auf „Freigegebene Elemente“, und erweitern Sie „Unterformulare“.
   * Doppelklicken Sie anschließend auf das Unterformular „${ObjectName}InheritableSchema“ (wobei „{ObjectName}“ der Name der standardmäßigen strukturellen Objektklasse ist, z.B. „Person“).
   * Geben Sie für das Attribut, das Sie dem Schema hinzufügen möchten, den Namen „{MyPersonAtrribute}“ ein. Erstellen Sie ein entsprechendes Feld, indem Sie im Menü **Erstellen** die Option **Feld** auswählen.
   * Legen Sie die Eigenschaften des hinzugefügten Felds fest, indem Sie im Fenster mit den Feldeigenschaften den Typ, das Format, die Größe, die Schriftart und ähnliche Parameter auswählen.
   * Behalten Sie für den Standardwert des Attributs den für das Attribut vergebenen Namen bei. (Wenn der Attributname also beispielsweise „MyPersonAttribute“ lautet, behalten Sie den Standardwert mit dem gleichen Namen bei.)
   * Speichern Sie das Unterformular „${ObjectName}InheritableSchema“ mit den aktualisierten Werten.
3. Ersetzen Sie die Domino-Verzeichnisvorlage „{PUBNAMES.NTF}“ durch die neue benutzerdefinierte Vorlage „{CONTOSO. NTF}“. Die entsprechende Vorgehensweise finden Sie [hier](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Schließen Sie Domino Administrator, und öffnen Sie die Domino-Konsole, um den LDAP-Dienst neu zu starten und das LDAP-Schema neu zu laden:
   * Fügen Sie in der Domino-Konsole den Befehl [Restart Task LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html) als **Domino-Befehl** ein, um den LDAP-Dienst neu zu starten.
   * Verwenden Sie zum Neuladen des LDAP-Schemas den Befehl „Tell LDAP ReloadSchema“.
5. Öffnen Sie Domino Administrator, und rufen Sie die Registerkarte für Personen und Gruppen auf, um sich zu vergewissern, dass das hinzugefügte Attribut beim Hinzufügen einer Person in Domino verfügbar ist.
6. Öffnen Sie auf der Dateiregisterkarte **** die Datei „Schema.nsf“, und vergewissern Sie sich, dass sich das hinzugefügte Attribut in der LDAP-Objektklasse „dominoPerson“ befindet.

**Ansatz 2: Erstellen einer Erweiterungsklasse mit benutzerdefiniertem Attribut und Zuordnen zur Objektklasse**

1. Erstellen Sie eine Kopie der Domino-Verzeichnisvorlage „{PUBNAMES.NTF}“. Die entsprechenden Schritte finden Sie [hier](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html). (Lassen Sie die standardmäßige IBM Lotus Domino-Verzeichnisvorlage unbedingt unverändert.)
2. Öffnen Sie die Kopie der Domino-Verzeichnisvorlage {CONTOSO.NTF}, die Sie erstellt haben, in Domino Designer.
3. Wählen Sie im linken Bereich „Freigegebener Code“ und anschließend „Unterformulare“ aus.
4. Klicken Sie auf „Neues Unterformular“.
5. Führen Sie folgende Schritte aus, um die Eigenschaften für das neue Unterformular anzugeben:
   * Wählen Sie im geöffneten neuen Unterformular „Design“ und anschließend „Unterformulareigenschaften“ aus.
   * Geben Sie neben der Eigenschaft „Name“ einen Namen für die Erweiterungsobjektklasse ein (beispielsweise „TestSubform“).
   * Lassen Sie die Optionseigenschaft „In Einfügedialogfeld für das Unterformular einschließen“ aktiviert.
   * Deaktivieren Sie die Optionseigenschaft „Pass-through-HTML in Notes rendern“.
   * Lassen Sie die anderen Eigenschaften unverändert, und schließen Sie das Eigenschaftendialogfeld für das Unterformular.
   * Speichern und schließen Sie das neue Unterformular.
6. Gehen Sie wie folgt vor, um ein Feld zum Definieren der Erweiterungsobjektklasse hinzuzufügen:
   * Öffnen Sie das von Ihnen erstellte Unterformular.
   * Wählen Sie „Erstellen“ und anschließend „Feld“ aus.
   * Geben Sie neben dem Namen auf der Registerkarte „Grundlagen“ des Felddialogfelds einen beliebigen Namen an (beispielsweise „{MyPersonTestAttribute}“).
   * Legen Sie die Eigenschaften des hinzugefügten Felds fest, indem Sie den Typ, das Format, die Größe, die Schriftart und ähnliche Parameter auswählen.
   * Behalten Sie für den Standardwert des Attributs den für das Attribut vergebenen Namen bei. (Wenn der Attributname also beispielsweise „MyPersonTestAttribute“ lautet, behalten Sie den Standardwert mit dem gleichen Namen bei.)
   * Speichern Sie das Unterformular mit den aktualisierten Werten, und gehen Sie dann wie folgt vor:
     * Wählen Sie im linken Bereich „Freigegebener Code“ und anschließend „Unterformulare“ aus.
     * Wählen Sie das neue Unterformular und anschließend „Design“ > „Designeigenschaften“ aus.
     * Klicken Sie auf die dritte Registerkarte von links, und wählen Sie **Dieses Designänderungsverbot weitergeben**aus.
7. Öffnen Sie das Unterformular „${ObjectName}ExtensibleSchema“ (wobei „{ObjectName}“ der Name der standardmäßigen strukturellen Objektklasse ist, z.B. „Person“).
8. Fügen Sie die Ressource ein, wählen Sie das von Ihnen erstellte Unterformular (z.B. „TestSubform“) aus, und speichern Sie das Unterformular „${ObjectName}ExtensibleSchema“.
9. Ersetzen Sie die Domino-Verzeichnisvorlage „{PUBNAMES.NTF}“ durch die neue benutzerdefinierte Vorlage „{CONTOSO. NTF}“. Die entsprechende Vorgehensweise finden Sie [hier](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Schließen Sie Domino Administrator, und öffnen Sie die Domino-Konsole, um den LDAP-Dienst neu zu starten und das LDAP-Schema neu zu laden:
    * Fügen Sie in der Domino-Konsole den Befehl [Restart Task LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html) als **Domino-Befehl** ein, um den LDAP-Dienst neu zu starten.
    * Verwenden Sie zum Neuladen des LDAP-Schemas den Befehl **Tell LDAP ReloadSchema**.
11. Öffnen Sie Domino Administrator, und rufen Sie die Registerkarte für Personen und Gruppen auf, um sich zu vergewissern, dass das hinzugefügte Attribut beim Hinzufügen einer Person in Domino (auf der Registerkarte „Sonstiges“) verfügbar ist.
12. Öffnen Sie auf der Dateiregisterkarte **** die Datei „Schema.nsf“, und vergewissern Sie sich, dass sich das hinzugefügte Attribut in der LDAP-Erweiterungsobjektklasse „TestSubform“ befindet.

**Methode 3: Hinzufügen des benutzerdefinierten Attributs zur ExtensibleObject-Klasse**

1. Öffnen Sie die im Stammverzeichnis abgelegte Datei „{Schema.nsf}“.
2. Wählen Sie im linken Menü unter **Alle Schemadokumente** LDAP-Objektklassen aus, und klicken Sie anschließend auf die Schaltfläche **Objektklasse hinzufügen**:
3. Geben Sie den LDAP-Namen im Format „{zzzExtensibleSchema}“ ein (wobei „zzz“ der Name der standardmäßigen strukturellen Objektklasse ist, z.B. „Person“). Wenn Sie das Schema also beispielsweise für die Objektklasse „Person“ erweitern möchten, geben Sie den LDAP-Namen „{PersonExtensibleSchema}“ an.
4. Geben Sie den übergeordneten Objektklassennamen an, für den Sie das Schema erweitern möchten. Wenn Sie das Schema also beispielsweise für die Objektklasse „Person“ erweitern möchten, geben Sie den übergeordneten Objektklassennamen „{dominoPerson}“ an.
5. Geben Sie eine gültige, der Objektklasse entsprechende OID an.
6. Wählen Sie unter den Feldern für obligatorische oder optionale Attributtypen nach Bedarf erweiterte/benutzerdefinierte Attribute aus.
7. Klicken Sie auf **Speichern und schließen**, nachdem Sie der erweiterbaren Objektklasse (ExtensibleObjectClass) die erforderlichen Attribute hinzugefügt haben.
8. Für die entsprechende Standardobjektklasse wird eine erweiterte Objektklasse (ExtensibleObjectClass) mit erweiterten Attributen erstellt.

## <a name="troubleshooting"></a>Problembehandlung
* Informationen zum Aktivieren der Protokollierung für die Behandlung von Connectorproblemen finden Sie unter [Vorgehensweise: Aktivieren der ETW-Ablaufverfolgung für Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).

