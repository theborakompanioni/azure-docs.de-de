<properties
   pageTitle="Azure Data Catalog – Entwicklerkonzepte"
   description="Eine Einführung in die wichtigsten Konzepte im konzeptionellen Modell von Azure Data Catalog, die durch die Catalog-REST-API verfügbar gemacht werden."
   services="data-catalog"
   documentationCenter=""
   authors="spelluru"
   manager=""
   editor=""
   tags=""/>
<tags 
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="05/10/2016"
   ms.author="spelluru"/>

# Azure Data Catalog – Entwicklerkonzepte

**Azure Data Catalog** von Microsoft ist ein vollständig verwalteter Clouddienst mit Funktionen für die Datenquellenermittlung und das Crowdsourcing von Datenquellenmetadaten. Entwickler können den Dienst über die REST-APIs nutzen. Eine erfolgreiche Integration von **Azure Data Catalog** setzt voraus, dass die Entwickler mit den implementierten Konzepten vertraut sind.

## Wichtige Begriffe

Das konzeptionelle Modell von **Azure Data Catalog** basiert auf vier zentralen Konzepten: **Katalog**, **Benutzer**, **Assets** und **Anmerkungen**.

![Konzept][1]

*Abbildung 1: Vereinfachte Darstellung des konzeptionellen Modells von Azure Data Catalog*

### Katalog

Ein**Katalog** ist der Container der obersten Ebene und wird für sämtliche von der Organisation gespeicherte Metadaten verwendet. Pro Azure-Konto ist jeweils ein **Katalog** zulässig. Kataloge sind mit einem Azure-Abonnement verknüpft, pro Konto kann aber nur ein einzelner **Katalog** erstellt werden, auch wenn ein Konto über mehrere Abonnements verfügen kann.

Der Katalog enthält **Benutzer** und **Assets**.

### Benutzer

Benutzer sind Sicherheitsprinzipale, die über Berechtigungen zum Ausführen von Aktionen im Katalog verfügen (Durchsuchen des Katalogs, Hinzufügen, Bearbeiten oder Entfernen von Elementen usw.).

Ein Benutzer kann verschiedene Rollen haben. Weitere Informationen zu Rollen finden Sie im Abschnitt „Rollen und Autorisierung“.

Einzelne Benutzer und Sicherheitsgruppen können hinzugefügt werden.

Azure Data Catalog verwendet Azure Active Directory für die Identitäts- und Zugriffsverwaltung. Jeder Katalogbenutzer muss einem Active Directory für das Konto angehören.

### Assets

Ein **Katalog** enthält Datenassets. **Assets** sind die Einheit der vom Katalog verwalteten Granularität.

Die Granularität eines Assets variiert je nach Datenquelle. Bei SQL Server oder Oracle Database kann ein Asset eine Tabelle oder Sicht sein. Bei SQL Server Analysis Services kann ein Asset ein Kennzahlenobjekt, ein Dimensionsobjekt oder ein Key Performance Indicator (KPI) sein. Bei SQL Server Reporting Services ist ein Asset ein Bericht.

Ein **Asset** ist das Element, das Sie einem Katalog hinzufügen oder daraus entfernen. Es ist die Ergebniseinheit, die von **Search** zurückgegeben wird.

Ein **Asset** setzt sich aus dem Namen, dem Speicherort und dem Typ sowie aus Anmerkungen zur genaueren Beschreibung zusammen.

### Anmerkungen

Anmerkungen sind Elemente, die Metadaten für Assets darstellen.

Beispiele für Anmerkungen sind Beschreibungen, Tags, Schemas, Dokumentationen usw. Eine vollständige Liste der Asset- und Anmerkungstypen finden Sie im Abschnitt "Assetobjektmodell".

## Crowdsourcing von Anmerkungen und Benutzerperspektive (Meinungsvielfalt)

Ein zentraler Aspekt von Azure Data Catalog ist die Crowdsourcing-Unterstützung für Metadaten im System. Im Gegensatz zu einem Wiki-Ansatz, bei dem nur eine einzelne Meinung möglich ist und vom letzten Autor bestimmt wird, können beim Azure Data Catalog-Modell gleichzeitig mehrere Meinungen im System vorhanden sein.

Dieser Ansatz spiegelt die Realität von Unternehmensdaten wider, da er für ein bestimmtes Asset verschiedene Perspektiven von unterschiedlichen Benutzern zulässt:

-	Ein Datenbankadministrator kann Informationen zu verfügbaren Vereinbarungen zum Servicelevel oder das verfügbare Verarbeitungszeitfenster für ETL-Massenvorgänge angeben.
-	Ein Data Steward kann Informationen zu den Geschäftsprozessen, bei denen das Asset angewendet wird, oder die Klassifizierungen angeben, die das Unternehmen auf das Asset angewendet hat.
-	Ein Finanzanalyst kann Informationen zur Verwendung der Daten im Rahmen der Berichterstellung am Ende des Zeitraums angeben.

Um dieses Beispiel zu ermöglichen, kann jeder Benutzer (Datenbankadministrator, Data Steward und Analyst) einer einzelnen, im Katalog registrierten Tabelle eine Beschreibung hinzufügen. Alle Beschreibungen werden im System gespeichert und im Azure Data Catalog-Portal angezeigt.

Dieses Muster wird für die meisten Elemente im Objektmodell verwendet. Aus diesem Grund handelt es sich bei Objekttypen in der JSON-Nutzlast häufig um Arrays für Eigenschaften, obwohl Sie eigentlich einen Singleton erwarten würden.

Am Assetstamm findet sich beispielsweise ein Array mit Beschreibungsobjekten. Die Array-Eigenschaft heißt „descriptions“. Eine Beschreibungsobjekt besitzt eine einzige Eigenschaft – die Beschreibung. Für jeden Benutzer, der „description“ eingibt, wird ein Beschreibungsobjekt für den von ihm angegebenen Wert erstellt.

Die UX kann dann entscheiden, wie die Kombination dargestellt wird. Für die Anzeige stehen drei verschiedene Muster zur Verfügung.

-	Das einfachste Muster ist „Alles anzeigen“. Bei diesem Muster werden alle Objekte in einer Art von Listenansicht angezeigt. Diese Variante wird von der UX des Azure Data Catalog-Portals für die Beschreibung verwendet.
-	Ein weiteres Muster ist „Zusammenführen“. Bei diesem Muster werden alle Werte der verschiedenen Benutzer zusammengeführt. Duplikate werden entfernt. Beispiele für dieses Muster in der UX des Azure Data Catalog-Portals sind die Eigenschaften „tags“ und „experts“.
-	Ein drittes Muster ist „Letzter Autor ist ausschlaggebend“. Bei diesem Muster wird nur der zuletzt eingegebene Wert angezeigt. Ein Beispiel für dieses Muster ist „friendlyName“.

## Assetobjektmodell

Wie bereits im Abschnitt mit den wichtigen Begriffen angedeutet, enthält das Objektmodell von **Azure Data Catalog** Elemente, bei denen es sich um Assets oder Anmerkungen handeln kann. Elemente besitzen optionale oder erforderliche Eigenschaften. Einige Eigenschaften gelten für alle Elemente. Einige Eigenschaften gelten für alle Assets. Einige Eigenschaften gelten nur für bestimmte Assettypen.

### Systemeigenschaften

<table><tr><td><b>Eigenschaftenname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr><tr><td>timestamp</td><td>DateTime</td><td>Der Zeitpunkt der letzten Änderung des Elements. Diese Eigenschaft wird vom Server generiert, wenn ein Element eingefügt wird sowie bei jeder Aktualisierung eines Elements. Der Wert dieser Eigenschaft wird bei der Eingabe von Veröffentlichungsvorgängen ignoriert.</td></tr><tr><td>id</td><td>Uri</td><td>Die absolute URL des Elements (schreibgeschützt). Es handelt sich hierbei um den eindeutig adressierbaren URI für das Element. Der Wert dieser Eigenschaft wird bei der Eingabe von Veröffentlichungsvorgängen ignoriert.</td></tr><tr><td>type</td><td>String</td><td>Der Typ des Assets (schreibgeschützt).</td></tr><tr><td>etag</td><td>String</td><td>Eine Zeichenfolge entsprechend der Version des Elements, die beim Ausführen von Vorgängen, die Elemente im Katalog aktualisieren, für die Steuerung optimistischer Nebenläufigkeit verwendet werden kann. Mit „*“ kann die Übereinstimmung mit einem beliebigen Wert angegeben werden.</td></tr></table>

### Allgemeine Eigenschaften

Diese Eigenschaften gelten für alle Stammassettypen und für alle Anmerkungstypen.

<table>
<tr><td><b>Eigenschaftenname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>
<tr><td>fromSourceSystem</td><td>Boolean</td><td>Gibt an, ob Daten des Elements aus einem Quellsystem (z.B. SQL Server-Datenbank, Oracle-Datenbank) abgeleitet oder von einem Benutzer erstellt wurde.</td></tr>
</table>

### Allgemeine Stammeigenschaften
<p>
Diese Eigenschaften gelten für alle Stammassettypen.
<table><tr><td><b>Eigenschaftenname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr><tr><td>name</td><td>String</td><td>Ein Name, der sich aus den Speicherortinformationen der Datenquelle ableitet.</td></tr><tr><td>dsl</td><td>DataSourceLocation</td><td>Beschreibt die Datenquelle eindeutig und ist einer der Bezeichner für das Asset. (Siehe Abschnitt zu dualer Identität.) Die DSL-Struktur variiert je nach Protokoll und Quelltyp.</td></tr><tr><td>dataSource</td><td>DataSourceInfo</td><td>Weitere Informationen zum Typ des Assets.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Beschreibt den Benutzer, der dieses Asset zuletzt registriert hat. Enthält sowohl die eindeutige ID für den Benutzer („Upn“) als auch einen Anzeigenamen („lastName“ und „firstName“).</td></tr><tr><td>containerId</td><td>String</td><td>Die ID des Containerassets für die Datenquelle. Diese Eigenschaft wird für den Containertyp nicht unterstützt.</td></tr></table>

### Allgemeine Nicht-Singleton-Anmerkungseigenschaften

Diese Eigenschaften gelten für alle Nicht-Singleton-Anmerkungstypen (d.h. Anmerkungen, von denen pro Asset mehrere auftreten können).

<table>
<tr><td><b>Eigenschaftenname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>
<tr><td>key</td><td>String</td><td>Ein benutzerdefinierter Schlüssel, der die Anmerkung in der aktuellen Sammlung eindeutig identifiziert. Die Länge des Schlüssels darf 256 Zeichen nicht überschreiten.</td></tr>
</table>

### Stammassettypen

Stammassettypen stellen die verschiedenen Typen von Datenassets dar, die im Katalog registriert werden können. Für jeden Stammtyp ist eine Ansicht definiert, die die Assets und Anmerkungen beschreibt, die in der Ansicht enthalten sind. Der Sichtname sollte bei der Veröffentlichung eines Assets mithilfe der REST-API im entsprechenden {view\_name}-URL-Segment verwendet werden.

<table><tr><td><b>Assettyp (Sichtname)</b></td><td><b>Zusätzliche Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Zulässige Anmerkungen</b></td><td><b>Kommentare</b></td></tr><tr><td>Tabelle („tables“)</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Tag<p>Schema<p>ColumnDescription<p>ColumnTag<p> Experten<p>Vorschau<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>Dokumentation<p></td><td>Eine Tabelle stellt beliebige Tabellendaten dar. Dazu zählen SQL-Tabellen, SQL-Sichten, tabellarische Analysis&#160;Services-Tabellen, mehrdimensionale Analysis&#160;Services-Dimensionen, Oracle-Tabellen usw.   </td></tr><tr><td>Kennzahl („measures“)</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Tag<p>Experten<p>AccessInstruction<p>Dokumentation<p></td><td>Dieser Typ stellt eine Analysis&#160;Services-Kennzahl dar.</td></tr><tr><td></td><td>Kennzahl</td><td>Column</td><td></td><td>Metadaten zur Beschreibung der Kennzahl.</td></tr><tr><td></td><td>isCalculated </td><td>Boolean</td><td></td><td>Gibt an, ob es sich um eine berechnete Kennzahl handelt.</td></tr><tr><td></td><td>measureGroup</td><td>String</td><td></td><td>Der physische Container für die Kennzahl.</td></tr><td>KPI („kpis“)</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Tag<p>Experten<p>AccessInstruction<p>Dokumentation</td><td></td></tr><tr><td></td><td>measureGroup</td><td>String</td><td></td><td>Der physische Container für die Kennzahl.</td></tr><tr><td></td><td>goalExpression</td><td>String</td><td></td><td>Ein numerischer MDX-Ausdruck oder eine Berechnung, die den Zielwert des KPI zurückgibt.</td></tr><tr><td></td><td>valueExpression</td><td>String</td><td></td><td>Ein numerischer MDX-Ausdruck, der den tatsächlichen Wert des KPI zurückgibt.</td></tr><tr><td></td><td>statusExpression</td><td>String</td><td></td><td>Ein MDX-Ausdruck, der den Status des KPI zu einem bestimmten Zeitpunkt darstellt.</td></tr><tr><td></td><td>trendExpression</td><td>String</td><td></td><td>Ein MDX-Ausdruck, der den Wert des KPI im Zeitverlauf auswertet. Der Trend kann ein beliebiges zeitbasiertes Kriterium sein, das in einem bestimmten Geschäftskontext hilfreich ist.</td>
<tr><td>Bericht („reports“)</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Tag<p>Experten<p>AccessInstruction<p>Dokumentation<p></td><td>Dieser Typ stellt einen SQL&#160;Server&#160;Reporting&#160;Services-Bericht dar. </td></tr><tr><td></td><td>assetCreatedDate</td><td>String</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>String</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>String</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>String</td><td></td><td></td></tr><tr><td>Container („containers“)</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Tag<p>Experten<p>AccessInstruction<p>Dokumentation<p></td><td>Dieser Typ stellt einen Container für andere Objekte dar, z.&#160;B. eine SQL-Datenbank, ein Azure-Blobcontainer oder ein Analysis Services-Modell.</td></tr></table>

### Anmerkungstypen

Anmerkungstypen stellen Typen von Metadaten dar, die anderen Typen innerhalb des Katalogs zugewiesen werden können.

<table>
<tr><td><b>Anmerkungstyp (geschachtelter Sichtname)</b></td><td><b>Zusätzliche Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>

<tr><td>Beschreibung („descriptions“)</td><td></td><td></td><td>Enthält eine Beschreibung für ein Asset. Jeder Benutzer des Systems kann eine eigene Beschreibung hinzufügen. Nur dieser Benutzer kann das Beschreibungsobjekt bearbeiten. (Administratoren und Assetbesitzer können das Beschreibungsobjekt löschen, aber nicht bearbeiten.) Diese Elemente werden separat verwaltet. Es gibt also ein Array mit Beschreibungen für die einzelnen Assets (eine für jeden Benutzer, der einen Beitrag zu dem Asset geleistet hat, sowie ggf. eine weitere mit Informationen, die sich aus der Datenquelle ableiten).</td></tr>
<tr><td></td><td>description</td><td>string</td><td>Eine kurze Beschreibung des Assets (zwei bis drei Zeilen)</td></tr>

<tr><td>Tag („tags“)</td><td></td><td></td><td>Enthält ein Tag für ein Asset. Jeder Benutzer des Systems kann mehrere Tags für ein Asset hinzufügen. Nur der Benutzer, der ein Tagobjekt erstellt hat, kann dieses bearbeiten. (Administratoren und Assetbesitzer können das Tagobjekt löschen, aber nicht bearbeiten.) Diese Elemente werden separat verwaltet. Es gibt daher ein Array von Tagobjekten für jedes Asset.</td></tr>
<tr><td></td><td>tag</td><td>string</td><td>Ein Tag mit einer Beschreibung des Assets.</td></tr>

<tr><td>FriendlyName („friendlyName“)</td><td></td><td></td><td>Enthält einen Anzeigenamen für ein Asset. FriendlyName ist eine Singletonanmerkung – nur ein FriendlyName kann einem Asset hinzugefügt werden. Nur der Benutzer, der ein FriendlyName-Objekt erstellt hat, kann dieses bearbeiten. (Administratoren und Assetbesitzer können das FriendlyName-Objekt löschen, aber nicht bearbeiten.) Diese Elemente werden separat verwaltet.</td></tr>
<tr><td></td><td>friendlyName</td><td>string</td><td>Ein Anzeigename des Assets.</td></tr>

<tr><td>Schema („schema“)</td><td></td><td></td><td>Das Schema beschreibt die Struktur der Daten. Es führt die Attributnamen (Spalte, Attribut, Feld usw.), Typen und andere Metadaten auf. Diese Informationen werden alle von der Datenquelle abgeleitet. Das Schema ist eine Singletonanmerkung – nur ein Schema kann für ein Asset hinzugefügt werden.</td></tr>
<tr><td></td><td>columns</td><td>Column[]</td><td>Ein Array mit Spaltenobjekten. Diese beschreiben die Spalte mit aus der Datenquelle abgeleiteten Informationen.</td></tr>

<tr><td>ColumnDescription („columnDescriptions“)</td><td></td><td></td><td>Enthält eine Beschreibung für eine Spalte. Jeder Benutzer des Systems kann seine eigenen Beschreibungen für mehrere Spalten hinzufügen (höchstens eine pro Spalte). Nur der Benutzer, der ein ColumnDescription-Objekt erstellt hat, kann dieses bearbeiten. (Administratoren und Assetbesitzer können das ColumnDescription-Objekt löschen, aber nicht bearbeiten.) Diese Elemente werden separat verwaltet. Es gibt daher ein Array mit ColumnDescription-Objekten für die einzelnen Assets (eins für jeden Benutzer, der einen Beitrag zu der Spalte geleistet hat, sowie ggf. ein weiteres mit Informationen, die aus der Datenquelle abgeleitet wurde). Die ColumnDescription ist lose an das Schema gebunden, daher kann ihre Synchronisierung verloren gehen, d.h., die ColumnDescription kann eine Spalte beschreiben, die nicht mehr im Schema vorhanden ist. Die Synchronisierung ist Sache des Autors. Die Datenquelle kann auch beschreibende Informationen zu Spalten enthalten. Hierbei handelt es sich dann um zusätzliche ColumnDescription-Objekte, die beim Ausführen des Tools erstellt werden.</td></tr>
<tr><td></td><td>columnName</td><td>String</td><td>Der Name der Spalte, auf die sich diese Beschreibung bezieht.</td></tr>
<tr><td></td><td>description</td><td>String</td><td>Eine kurze Beschreibung der Spalte (zwei bis drei Zeilen).</td></tr>

<tr><td>ColumnTag („columnTags“)</td><td></td><td></td><td>Enthält ein Tag für eine Spalte. Jeder Benutzer des Systems kann mehrere Tags für eine bestimmte Spalte sowie Tags für mehrere Spalten hinzufügen. Nur der Benutzer, der ein ColumnTag-Objekt erstellt hat, kann dieses bearbeiten. (Administratoren und Assetbesitzer können das ColumnTag-Objekt löschen, aber nicht bearbeiten.) Diese Elemente werden separat verwaltet. Es gibt daher ein Array von ColumnTag-Objekten für jedes Asset. Das ColumnTag ist lose an das Schema gebunden, daher kann ihre Synchronisierung verloren gehen, d.h., das ColumnTag kann eine Spalte beschreiben, die nicht mehr im Schema vorhanden ist. Die Synchronisierung ist Sache des Autors.</td></tr>
<tr><td></td><td>columnName</td><td>String</td><td>Der Name der Spalte, auf die sich dieses Tag bezieht.</td></tr>
<tr><td></td><td>tag</td><td>String</td><td>Ein Tag mit einer Beschreibung der Spalte.</td></tr>

<tr><td>Experte („experts“)</td><td></td><td></td><td>Enthält einen Benutzer, der als Experte in dem Dataset angesehen wird. Expertenmeinungen (also Beschreibungen) werden in der UX zuerst aufgelistet. Jeder Benutzer kann eigene Experten angeben. Nur dieser Benutzer kann das Expertenobjekt bearbeiten. (Administratoren und Assetbesitzer können das Expertenobjekt löschen, aber nicht bearbeiten.)</td></tr>
<tr><td></td><td>Experte</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Vorschau („previews“)</td><td></td><td></td><td>Die Vorschau enthält eine Momentaufnahme der Top-20-Datenzeilen für das Asset. Sie ist nur für einige Assettypen sinnvoll. (Mit anderen Worten: Eine Vorschau ist sinnvoll für eine Tabelle, aber nicht für eine Kennzahl).</td></tr>
<tr><td></td><td>preview</td><td>object[]</td><td>Ein Array mit Objekten, das eine Spalte darstellt. Jedes Objekt besitzt eine Eigenschaftenzuordnung zu einer Spalte mit einem Spaltenwert für die Zeile.</td></tr>

<tr><td>AccessInstruction („accessInstructions“)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>string</td><td>Der MIME-Typ des Inhalts.</td></tr>
<tr><td></td><td>Inhalt</td><td>string</td><td>Die Anweisungen für den Zugriff auf dieses Datenasset. Dies kann eine URL, eine E-Mail-Adresse oder einen Satz von Anweisungen sein.</td></tr>

<tr><td>TableDataProfile („tableDataProfiles“)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>Die Anzahl der Zeilen im Dataset</td></tr>
<tr><td></td><td>size</td><td>lang</td><td>Die Größe des Datasets in Bytes.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>string</td><td>Der Zeitpunkt der letzten Schemaänderung</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>string</td><td>Der Zeitpunkt der letzten Datasetänderung (Daten wurden hinzugefügt, geändert oder gelöscht)</td></tr>

<tr><td>ColumnsDataProfile („columnsDataProfiles“)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columns</td></td><td>ColumnDataProfile[]</td><td>Ein Array von Spaltendatenprofilen.</td></tr>

<tr><td>Dokumentation („documentation“)</td><td></td><td></td><td>Einem bestimmten Asset kann nur eine Dokumentation zugeordnet sein.</td></tr>
<tr><td></td><td>mimeType</td><td>string</td><td>Der MIME-Typ des Inhalts.</td></tr>
<tr><td></td><td>Inhalt</td><td>string</td><td>Der Inhalt der Dokumentation.</td></tr>

</table>

### Allgemeine Typen

Allgemeine Typen können als Typen für Eigenschaften verwendet werden, sind aber keine Elemente.
<table>
<tr><td><b>Allgemeiner Typ</b></td><td><b>Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sourceType</td><td>string</td><td>Beschreibt den Typ der Datenquelle (SQL&#160;Server, Oracle Database...).  </td></tr>
<tr><td></td><td>objectType</td><td>string</td><td>Beschreibt den Typ des Objekts in der Datenquelle (beispielsweise Tabelle oder Sicht für SQL Server).</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>protocol</td><td>string</td><td>Erforderlich. Beschreibt ein Protokoll zur Kommunikation mit der Datenquelle, d.h. „tds“ für SQL Server, „oracle“ für Oracle usw... Unter [Spezifikation des Datenquellenverweises – DSL-Struktur](data-catalog-dsr.md) finden Sie die Liste der derzeit unterstützten Protokolle.</td></tr>
<tr><td></td><td>address</td><td>Wörterbuch&lt;string, object></td><td>Erforderlich. Dies ist eine Gruppe von Daten, die spezifisch für das Protokoll ist, das zum Identifizieren der Datenquelle verwendet wird, auf die verwiesen wird. Die für ein bestimmtes Protokoll gültigen Adressdaten, d.h. ohne Kenntnis des Protokolls bedeutungslos.</td></tr>
<tr><td></td><td>authentication</td><td>Zeichenfolge</td><td>Optional. Das Authentifizierungsschema, das zur Kommunikation mit der Datenquelle verwendet wird, d.h. windows, oauth usw... </td></tr>
<tr><td></td><td>connectionProperties</td><td>Wörterbuch&lt;string, object></td><td>Optional. Weitere Informationen zum Herstellen einer Verbindung mit einer Datenquelle.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>Beachten Sie, dass das Back-End bei der Veröffentlichung keinerlei Überprüfung der angegebenen Eigenschaften anhand von AAD durchführt.</td></tr>
<tr><td></td><td>upn</td><td>string</td><td>Eindeutige E-Mail-Adresse des Benutzers. Muss angegeben werden, wenn keine Objekt-ID angegeben wurde, oder im Zusammenhang mit der Eigenschaft „lastRegisteredBy“, andernfalls optional.</td></tr>
<tr><td></td><td>objectId</td><td>GUID</td><td>AAD-Identität einer Benutzer- oder Sicherheitsgruppe. Optional. Muss angegeben werden, wenn kein UPN angegeben wurde, andernfalls optional.</td></tr>
<tr><td></td><td>firstName</td><td>string</td><td>Der Vorname des Benutzers (für die Anzeige). Optional. Nur im Kontext der Eigenschaft „lastRegisteredBy“ gültig. Kann bei der Bereitstellung eines Sicherheitsprinzipals für Rollen, Berechtigungen und Experten nicht angegeben werden.</td></tr>
<tr><td></td><td>lastName</td><td>string</td><td>Der Nachname des Benutzers (für die Anzeige). Optional. Nur im Kontext der Eigenschaft „lastRegisteredBy“ gültig. Kann bei der Bereitstellung eines Sicherheitsprinzipals für Rollen, Berechtigungen und Experten nicht angegeben werden.</td></tr>

<tr><td>Column</td><td></td><td></td><td></td></tr>
<tr><td></td><td>name</td><td>string</td><td>Der Name der Spalte oder des Attributs.</td></tr>
<tr><td></td><td>type</td><td>string</td><td>Der Datentyp der Spalte oder des Attributs. Die zulässigen Typen hängen vom Datenquellentyp des Assets ab. Nur eine Teilmenge von Typen wird unterstützt.</td></tr>
<tr><td></td><td>maxLength</td><td>int</td><td>Die maximal zulässige Länge für die Spalte oder das Attribut. Abgeleitet von der Datenquelle. Gilt nur für einige Datenquellentypen.</td></tr>
<tr><td></td><td>Genauigkeit</td><td>Byte</td><td>Die Genauigkeit für die Spalte oder das Attribut. Abgeleitet von der Datenquelle. Gilt nur für einige Datenquellentypen.</td></tr>
<tr><td></td><td>isNullable</td><td>Boolean</td><td>Gibt an, ob die Spalte einen Nullwert enthalten kann. Abgeleitet von der Datenquelle. Gilt nur für einige Datenquellentypen.</td></tr>
<tr><td></td><td>expression</td><td>string</td><td>Wenn es sich bei dem Wert um eine berechnete Spalte handelt, enthält dieses Feld den Ausdruck, der den Wert angibt. Abgeleitet von der Datenquelle. Gilt nur für einige Datenquellentypen.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>string</td><td>Der Name der Spalte</td></tr>
<tr><td></td><td>Typ </td><td>string</td><td>Der Typ der Spalte</td></tr>
<tr><td></td><td>min </td><td>string</td><td>Der Mindestwert im Dataset</td></tr>
<tr><td></td><td>max </td><td>string</td><td>Der Höchstwert im Dataset</td></tr>
<tr><td></td><td>avg </td><td>double</td><td>Der durchschnittliche Wert im Dataset</td></tr>
<tr><td></td><td>stdev </td><td>double</td><td>Die Standardabweichung für das Dataset</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>Die Anzahl der Nullwerte im Dataset</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>Die Anzahl der unterschiedlichen Werte im Dataset</td></tr>


</table>

## Assetidentität
Azure Data Catalog verwendet „protocol“ und Identitätseigenschaften aus dem Eigenschaftenbehälter „address“ der DataSourceLocation-Eigenschaft „dsl“, um die Identität des Assets zu erzeugen, die verwendet wird, um das Asset im Katalog zu adressieren. Beispielsweise hat das Protokoll „tds“ die Identitätseigenschaften „server“, „database“, „schema“ und „object“; die Kombination der Protokoll- und Identitätseigenschaften wird verwendet, um die Identität des SQL Server-Tabellenassets zu generieren. Azure Data Catalog enthält verschiedene integrierte Datenquellenprotokolle, die unter [Von Azure Data Catalog unterstützte Datenquellen](data-catalog-dsr.md) aufgeführt sind. Der Satz der unterstützten Protokolle kann programmgesteuert erweitert werden (nähere Informationen siehe Data Catalog-REST-API-Referenz). Administratoren des Katalogs können benutzerdefinierte Datenquellenprotokolle registrieren. In der folgenden Tabelle werden die Eigenschaften beschrieben, die erforderlich sind, um ein benutzerdefiniertes Protokoll zu registrieren.

### Benutzerdefinierte Datenquellenprotokoll-Spezifikation
<table>
<tr><td><b>Typ</b></td><td><b>Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Namespace</td><td>string</td><td>Der Namespace des Protokolls. Der Namespace muss zwischen 1 und 255 Zeichen lang sein, mindestens ein nicht leeres Teil enthalten, die Teile müssen durch den Punkt (.) getrennt sein. Jeder Teil muss zwischen 1 und 255 Zeichen lang sein, mit einem Buchstaben beginnen und darf nur Buchstaben und Zahlen enthalten.</td></tr>
<tr><td></td><td>Name</td><td>string</td><td>Der Name des Protokolls. Der Name muss zwischen 1 und 255 Zeichen lang sein, mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und den Gedankenstrich (-) enthalten.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty[]</td><td>Liste der Identitätseigenschaften, muss mindestens eine, darf jedoch nicht mehr als 20 Eigenschaften enthalten. Beispiel: „server“, „database“, „schema“, „object“ sind die Identitätseigenschaften des „tds“-Protokolls.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet[]</td><td>Liste der Identitätssätze. Definiert Sätze von Identitätseigenschaften, die die gültige Identität des Assets darstellen. Muss mindestens einen, darf jedoch nicht mehr als 20 Sätze enthalten. Beispiel: {„server“, „database“, „schema“ und „object“} ist ein Identitätssatz für das „tds“-Protokoll, der die Identität des SQL Server-Tabellenassets definiert.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Name</td><td>Zeichenfolge</td><td>Der Name der Eigenschaft. Der Name muss zwischen 1 und 100 Zeichen lang sein, mit einem Buchstaben beginnen und darf nur Buchstaben und Zahlen enthalten.</td></tr>
<tr><td></td><td>Typ</td><td>Zeichenfolge</td><td>Der Typ der Eigenschaft. Unterstützte Werte: „bool“, „boolean“, „byte“, „guid“, „int“, „integer“, „long“, „string“, „url“</td></tr>
<tr><td></td><td>ignoreCase</td><td>bool</td><td>Gibt an, ob bei Verwendung des Eigenschaftswerts die Groß-/Kleinschreibung ignoriert werden soll. Kann nur für Eigenschaften vom Typ „string“ angegeben werden. Der Standardwert ist „false“.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>bool[]</td><td>Gibt an, ob für jedes Segment des URL-Pfads die Groß-/Kleinschreibung ignoriert werden soll. Kann nur für Eigenschaften vom Typ „url“ angegeben werden. Der Standardwert ist [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Name</td><td>string</td><td>Der Name des Identitätssatzes.</td></tr>
<tr><td></td><td>Eigenschaften</td><td>string[]</td><td>Die Liste der in diesem Identitätssatz enthaltenen Identitätseigenschaften. Sie darf keine Duplikate enthalten. Jede Eigenschaft, auf die der Identitätssatz verweist, muss in der Liste der „identityProperties“ des Protokolls definiert werden.</td></tr>

</table>

## Rollen und Autorisierung

Microsoft Azure Data Catalog bietet Autorisierungsfunktionen für asset- und anmerkungsbezogene CRUD-Vorgänge.

## Wichtige Begriffe

Azure Data Catalog verwendet zwei Autorisierungsmechanismen:

- Rollenbasierte Autorisierung
- Berechtigungsbasierte Autorisierung

### Rollen

Drei Rollen stehen zur Verfügung: **Administrator**, **Besitzer** und **Mitwirkender**. Jede Rolle hat ihren eigenen Umfang und ihre eigenen Rechte. Diese werden in der folgenden Tabelle zusammengefasst.

<table><tr><td><b>Rolle</b></td><td><b>Umfang</b></td><td><b>Rechte</b></td></tr><tr><td>Administrator</td><td>Katalog (also alle Assets/Anmerkungen im Katalog)</td><td>Read Delete ViewRoles ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Besitzer</td><td>Jedes Asset (Stammelement)</td><td>Read Delete ViewRoles ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Mitwirkender</td><td>Jedes einzelne Asset und jede einzelne Anmerkung</td><td>Read Update Delete ViewRoles – Hinweis: Wird dem Mitwirkenden die Leseberechtigung für das Objekt entzogen, werden ihm auch die anderen Rechte entzogen.</td></tr></table>

> [AZURE.NOTE] Die Rechte **Read**, **Update**, **Delete** und **ViewRoles** gelten für jedes Element (Asset oder Anmerkung), die Rechte **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility** und **ViewPermissions** gelten dagegen nur für das Stammasset.
>
>Das Recht **Delete** gilt für ein Element sowie für alle dazugehörigen untergeordneten Elemente. Beim Löschen eines Assets werden also beispielsweise auch sämtliche Anmerkungen für dieses Asset gelöscht.

### Berechtigungen

Eine Berechtigung ist eine Liste mit Zugriffssteuerungseinträgen. Jeder Zugriffssteuerungseintrag weist einem Sicherheitsprinzipal eine Gruppe von Rechten zu. Berechtigungen können nur für ein Asset (Stammelement) angegeben werden und gelten für das Asset und alle dazugehörigen untergeordneten Elemente.

In der Vorschauversion von **Azure Data Catalog** wird in der Berechtigungsliste nur das Recht **Read** unterstützt, um die Einschränkung der Sichtbarkeit eines Assets zu ermöglichen.

Standardmäßig ist jeder authentifizierte Benutzer zum Lesen aller Elemente im Katalog berechtigt, es sei denn, die Sichtbarkeit ist auf die Gruppe von Prinzipalen in den Berechtigungen beschränkt.

## REST-API

Mit den Ansichtselementanforderungen **PUT** und **POST** können Sie Rollen und Berechtigungen steuern. Neben der Elementnutzlast können zwei Systemeigenschaften angegeben werden: **roles** und **permissions**.

> [AZURE.NOTE]
>
> **permissions** gilt nur für ein Stammelement.
>
> Die Rolle **Besitzer** gilt nur für ein Stammelement.
>
> Bei der Erstellung eines Elements im Katalog wird standardmäßig der entsprechende **Mitwirkende** als derzeit authentifizierter Benutzer festgelegt. Soll ein Element für jeden Benutzer aktualisierbar sein, muss der **Mitwirkende** in der Eigenschaft **roles** bei der Erstveröffentlichung des Elements auf den speziellen Sicherheitsprinzipal <Everyone> festgelegt werden (siehe Beispiel weiter unten). Der **Mitwirkende** kann nicht geändert werden (auch nicht vom **Administrator** oder **Besitzer**) und bleibt während der gesamten Lebensdauer eines Elements unverändert. Der einzige unterstützte Wert zum expliziten Festlegen des **Mitwirkenden** ist <Everyone>. Der **Mitwirkende** kann also nur ein Benutzer sein, der ein Element erstellt hat, oder <Everyone>.

###Beispiele
**Festlegen des Mitwirkenden auf <Everyone> beim Veröffentlichen eines Elements.** Die Objekt-ID des speziellen Sicherheitsprinzipals <Everyone> lautet „00000000-0000-0000-0000-000000000201“. **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

  > [AZURE.NOTE] Einige HTTP-Clientimplementierungen können Anforderungen als Reaktion auf eine 302-Antwort vom Server neu ausstellen, in der Regel werden die Autorisierungsheader jedoch aus der Anforderung entfernt. Der Autorisierungsheader ist für Anforderungen an ADC erforderlich. Vergewissern Sie sich daher, dass der Autorisierungsheader noch bereitgestellt ist, wenn eine Anforderung an einen von ADC festgelegten Umleitungsstandort neu ausgestellt wird. Der folgende Beispielcode verdeutlicht dies mithilfe des Objekts „.NET HttpWebRequest“.

**Body**

	{
		"roles": [
			{
				"role": "Contributor",
				"members": [
					{
						"objectId": "00000000-0000-0000-0000-000000000201"
					}
				]
			}
		]
	}

  **Zuweisen von Besitzern und Einschränken der Sichtbarkeit für ein vorhandenes Stammelement** **PUT** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30 { "roles": [ { "role": "Owner", "members": [ { "objectId": "c4159539-846a-45af-bdfb-58efd3772b43", "upn": "user1@contoso.com" }, { "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f", "upn": "user2@contoso.com" } ] } ], "permissions": [ { "principal": { "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a", "upn": "user3@contoso.com" }, "rights": [ { "right": "Read" } ] }, { "principal": { "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31", "upn": "user4@contoso.com" }, "rights": [ { "right": "Read" } ] } ] }

> [AZURE.NOTE] Bei „PUT“ muss im Text keine Elementnutzlast angegeben werden: „PUT“ kann zur reinen Aktualisierung von Rollen und/oder Berechtigungen verwendet werden.

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png

<!---HONumber=AcomDC_0518_2016-->