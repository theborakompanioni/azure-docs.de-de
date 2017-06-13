---
title: Generischer SQL-Connector | Microsoft Docs
description: Dieser Artikel beschreibt die Konfiguration des generischen SQL-Connectors von Microsoft.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/2/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: b7e99f8a4d7bc1cd30c71ce08ad38c13203f8b69
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="generic-sql-connector-technical-reference"></a>Technische Referenz für den generischen SQL-Connector
Dieser Artikel beschreibt den generischen SQL-Connector. Der Artikel bezieht sich auf folgende Produkte:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Mindestens Hotfix 4.1.3671.0 ( [KB3092178](https://support.microsoft.com/kb/3092178)) erforderlich.

Für MIM2016 und FIM2010R2 steht der Connector im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495)zum Download bereit.

Um diesen Connector in Aktion zu sehen, lesen Sie den Artikel [Schrittweise Anleitung für den generischen SQL-Connector](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Übersicht über den generischen SQL-Connector
Der generische SQL-Connector ermöglicht die Integration des Synchronisierungsdiensts in ein Datenbanksystem mit ODBC-Konnektivität.  

Im Anschluss finden Sie einen allgemeinen Überblick über die von der aktuellen Connectorversion unterstützten Features:

| Feature | Support |
| --- | --- |
| Verbundene Datenquelle |Der Connector wird mit allen 64-Bit-ODBC-Treibern unterstützt. Getestet wurde er mit Folgendem:  <li>Microsoft SQL Server und SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 und 11g</li><li>MySQL 5.x</li> |
| Szenarios |<li>Objektlebenszyklusverwaltung</li><li>Kennwortverwaltung</li> |
| Vorgänge |<li>Vollständiger Import und Deltaimport, Export</li><li>Für Export: Hinzufügen, Löschen, Aktualisieren und Ersetzen</li><li>Kennwort festlegen, Kennwort ändern</li> |
| Schema |<li>Dynamische Ermittlung von Objekten und Attributen</li> |

### <a name="prerequisites"></a>Voraussetzungen
Zur Verwendung des Connectors muss auf dem Synchronisierungsserver Folgendes vorhanden sein:

* Microsoft .NET 4.5.2 Framework oder eine höhere Version
* 64-Bit-ODBC-Clienttreiber

### <a name="permissions-in-connected-data-source"></a>Berechtigungen für die verbundene Datenquelle
Zum Erstellen des generischen SQL-Connectors sowie zum Ausführen der unterstützten Aufgaben benötigen Sie Folgendes:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Ports und Protokolle
Welche Ports für die ordnungsgemäße Verwendung des ODBC-Treibers benötigt werden, können Sie der Dokumentation des Datenbankanbieters entnehmen.

## <a name="create-a-new-connector"></a>Erstellen eines neuen Connectors
Wählen Sie zum Erstellen eines generischen SQL-Connectors im **Synchronisierungsdienst** die Option **Verwaltungs-Agent** und anschließend **Erstellen** aus. Wählen Sie den Connector **Generic SQL (Microsoft)** (Generisch, SQL (Microsoft)) aus.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Konnektivität
Der Connector verwendet eine ODBC-DSN-Datei für die Konnektivität. Erstellen Sie die DSN-Datei mithilfe von **ODBC Data Sources** (ODBC-Datenquellen) (im Startmenü unter **Verwaltungstools**). Erstellen Sie im Verwaltungstool einen **Datei-DSN** , um ihn für den Connector bereitzustellen.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Der Konnektivitätsbildschirm ist der erste, der beim Erstellen eines neuen generischen SQL-Connectors angezeigt wird. Beginnen Sie mit der Angabe folgender Informationen:

* DSN-Dateipfad
* Authentifizierung
  * Benutzername
  * Kennwort

Die Datenbank muss eine der folgenden Authentifizierungsmethoden unterstützen:

* **Windows-Authentifizierung**: Die authentifizierende Datenbank überprüft den Benutzer anhand der Windows-Anmeldeinformationen. Zur Authentifizierung bei der Datenbank werden der angegebene Benutzername und das angegebene Kennwort verwendet. Dieses Konto benötigt Berechtigungen für die Datenbank.
* **SQL-Authentifizierung**: Die authentifizierende Datenbank verwendet zum Herstellen der Datenbankverbindung den im Konnektivitätsbildschirm definierten Benutzernamen und das dazugehörige Kennwort. Wenn Sie Benutzername und Kennwort in der DSN-Datei speichern, haben die im Konnektivitätsbildschirm angegebenen Anmeldeinformationen Vorrang.
* **Azure SQL-Datenbank-Authentifizierung**: Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](../../sql-database/sql-database-aad-authentication.md).

**DN is Anchor**(DN ist Anker): Wenn Sie diese Option aktivieren, wird der DN auch als Ankerattribut verwendet. Die Option kann bei einer einfachen Implementierung verwendet werden, es gelten jedoch folgende Einschränkungen:

* Der Connector unterstützt nur einen einzelnen Objekttyp. Daher müssen alle Verweisattribute auf den gleichen Objekttyp verweisen.

**Export Type: Object Replace**(Exporttyp: Objekt ersetzen): Falls nur einige Attribute geändert wurden, wird beim Exportieren das gesamte Objekt mit allen Attributen exportiert und das vorhandene Objekt ersetzt.

### <a name="schema-1-detect-object-types"></a>Schema 1 (Objekttyperkennung)
Auf dieser Seite wird für den Connector die Suche nach den verschiedenen Objekttypen in der Datenbank konfiguriert.

Jeder Objekttyp wird als Partition dargestellt und unter **Configure Partitions and Hierarchies**(Konfigurieren von Partitionen und Hierarchien) weiter konfiguriert.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Object Type detection method**(Objekttyp-Erkennungsmethode): Der Connector unterstützt folgende Objekttyp-Erkennungsmethoden:

* **Fester Wert**: Geben Sie eine kommagetrennte Liste mit Objekttypen an. Beispiel: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Table/View/Stored Procedure**(Tabelle/Sicht/Gespeicherte Prozedur): Geben Sie den Namen der Tabelle, Sicht oder gespeicherten Prozedur und anschließend den Namen der Spalte mit der Objekttypenliste an. Geben Sie bei Verwendung einer gespeicherten Prozedur außerdem Parameter im Format **[Name]:[Richtung]:[Wert]**an. Geben Sie die Parameter jeweils in einer separaten Zeile an.  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **SQL-Abfrage**: Diese Option ermöglicht die Angabe einer SQL-Abfrage, die eine einzelne Spalte mit Objekttypen zurückgibt (Beispiel: `SELECT [Column Name] FROM TABLENAME`). Die zurückgegebene Spalte muss vom Typ „String“ (varchar) sein.

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (Attributtyperkennung)
Auf dieser Seite wird die Erkennung der Attributnamen und -typen konfiguriert. Die Konfigurationsoptionen werden für jeden auf der vorherigen Seite ermittelten Objekttyp aufgeführt.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Attribute Type detection method**(Attributtyp-Erkennungsmethode): Der Connector unterstützt für jeden im Schema 1-Bildschirm erkannten Objekttyp folgende Attributtyp-Erkennungsmethoden:

* **Table/View/Stored Procedure**(Tabelle/Sicht/Gespeicherte Prozedur): Geben Sie den Namen der Tabelle, Sicht oder gespeicherten Prozedur an, die für die Suche nach den Attributnamen verwendet werden soll. Geben Sie bei Verwendung einer gespeicherten Prozedur außerdem Parameter im Format **[Name]:[Richtung]:[Wert]**an. Geben Sie die Parameter jeweils in einer separaten Zeile an. (Drücken Sie STRG+EINGABETASTE, um eine neue Zeile zu erhalten.) Wenn Sie Attributnamen in einem mehrwertigen Attribut ermitteln möchten, geben Sie eine kommagetrennte Tabellen- oder Sichtenliste an. Mehrwertige Szenarien, bei denen die übergeordnete und die untergeordnete Tabelle die gleichen Spaltennamen enthalten, werden nicht unterstützt.
* **SQL-Abfrage**: Diese Option ermöglicht die Angabe einer SQL-Abfrage, die eine einzelne Spalte mit Attributnamen zurückgibt (Beispiel: `SELECT [Column Name] FROM TABLENAME`). Die zurückgegebene Spalte muss vom Typ „String“ (varchar) sein.

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (Definieren von Anker und DN)
Auf dieser Seite können Sie jeweils das Anker- und DN-Attribut für die erkannten Objekttypen konfigurieren. Sie können mehrere Attribute auswählen, um einen eindeutigen Anker zu erhalten.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Mehrwertige und boolesche Attribute werden nicht aufgeführt.
* Für DN und Anker kann nicht das gleiche Attribut verwendet werden, es sei denn, auf der Konnektivitätsseite wurde die Option **DN is Anchor** (DN ist Anker) aktiviert.
* Ist auf der Konnektivitätsseite die Option **DN is Anchor** (DN ist Anker) aktiviert, wird auf dieser Seite nur das DN-Attribut benötigt. Dieses Attribut wird dann auch als Ankerattribut verwendet.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schema 4 (Definieren von Attributtyp, Verweis und Richtung)
Auf dieser Seite können Sie für die Attribute jeweils den Attributtyp (beispielsweise ganze Zahl, Binärwert oder boolescher Wert) und die Richtung konfigurieren. Hier werden alle Attribute der Seite **Schema 2** aufgeführt (auch mehrwertige Attribute).

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **DataType**: Dient dazu, den Attributtyp den Typen zuzuordnen, die dem Synchronisierungsmodul bekannt sind. Standardmäßig wird der im SQL-Schema erkannte Typ verwendet, „Datum/Uhrzeit“ und „Verweis“ sind jedoch nicht so einfach erkennbar. Hierfür muss **DateTime** oder **Reference** angegeben werden.
* **Direction**: Die Richtung des Attributs kann auf „Import“, „Export“ oder „ImportExport“ festgelegt werden. „ImportExport“ ist die Standardeinstellung.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Hinweise:

* Falls ein Attributtyp nicht vom Connector erkannt werden kann, wird der Datentyp „String“ verwendet.
* **Geschachtelte Tabellen** können als einspaltige Datenbanktabellen betrachtet werden. Oracle speichert die Zeilen einer geschachtelten Tabelle in keiner bestimmten Reihenfolge. Beim Abrufen der geschachtelten Tabelle in eine PL/SQL-Variable werden die Zeilen jedoch mit fortlaufenden tiefgestellten Zeichen (beginnend mit 1) versehen. Dies ermöglicht einen arrayähnlichen Zugriff auf einzelne Zeilen.
* **VARRYS** werden vom Connector nicht unterstützt.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (Definieren der Partition für Verweisattribute)
Auf dieser Seite wird für alle Referenzattribute konfiguriert, auf welche Partition (Objekttyp) das jeweilige Attribut verweist.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Bei Verwendung von **DN is anchor**(DN ist Anker) muss der gleiche Objekttyp verwendet werden, der auch den Ausgangspunkt des Verweises darstellt. Sie können nicht auf einen anderen Objekttyp verweisen.

>[!NOTE]
Seit dem Update vom März 2017 steht nun eine Option für „*“ zur Verfügung. Bei Auswahl dieser Option werden alle möglichen Mitgliedstypen importiert.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Ab Mai 2017 wurde die Option „*“ (**beliebige Option**) geändert, um auch Import- und Exportflows zu unterstützen. Wenn Sie diese Option verwenden möchten, muss Ihre mehrwertige Tabelle/Sicht ein Attribut mit dem Objekttyp enthalten.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> Wenn „*“ ausgewählt wird, muss auch der Name der Spalte mit dem Objekttyp angegeben werden.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

Nach dem Import wird etwas Ähnliches wie in der folgenden Abbildung angezeigt:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globale Parameter
Auf der Seite für globale Parameter können Sie den Deltaimport, das Datums-/Uhrzeitformat sowie die Kennwortmethode konfigurieren.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



Der generische SQL-Connector unterstützt folgende Methoden für den Deltaimport:

* **Trigger**: Weitere Informationen finden Sie unter [Generieren von Deltasichten mithilfe von Triggern](https://technet.microsoft.com/library/cc708665.aspx).
* **Wasserzeichen**: Eine allgemeine Methode, die für jede beliebige Datenbank verwendet werden kann. Die Wasserzeichenabfrage wird auf der Grundlage des Datenbankanbieters vorab aufgefüllt. Jede verwendete Tabelle/Sicht muss über eine Wasserzeichenspalte verfügen. Diese Spalte muss Einfüge- und Aktualisierungsvorgänge für die Tabellen sowie für abhängige (mehrwertige oder untergeordnete) Tabellen nachverfolgen. Die Uhren zwischen Synchronisierungsdienst und Datenbankserver müssen synchronisiert werden. Andernfalls werden unter Umständen einige Einträge im Deltaimport ausgelassen.  
  Einschränkung:
  * Die Wasserzeichenstrategie unterstützt keine gelöschten Objekte.
* **Momentaufnahme**: (Kann nur mit Microsoft SQL Server verwendet werden.) [Generieren von Deltasichten mithilfe von Momentaufnahmen](https://technet.microsoft.com/library/cc720640.aspx)
* **Änderungsnachverfolgung**: (Kann nur mit Microsoft SQL Server verwendet werden.) [About Änderungsnachverfolgung](https://msdn.microsoft.com/library/bb933875.aspx)  
  Einschränkungen:
  * Anker- und DN-Attribut müssen Teil des Primärschlüssels für das ausgewählte Objekt in der Tabelle sein.
  * Die SQL-Abfrage wird beim Importieren und Exportieren mit Änderungsnachverfolgung nicht unterstützt.

**Zusätzliche Parameter**: Geben Sie mithilfe der Zeitzone des Datenbankservers den Standort Ihres Datenbankservers an. Dieser Wert dient zur Unterstützung der verschiedenen Formate von Datums-/Uhrzeitattributen.

Der Connector speichert Datums- und Datums-/Uhrzeitwerte immer im UTC-Format. Zur ordnungsgemäßen Konvertierung von Datums- und Uhrzeitangaben müssen die Zeitzone des Datenbankservers und das verwendete Format angegeben werden. Das Format muss im .NET-Format angegeben werden.

Beim Exportieren muss jedes Datums-/Uhrzeitattribut für den Connector im UTC-Format angegeben werden.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Password Configuration**(Kennwortkonfiguration): Der Connector bietet Kennwortsynchronisierungsfunktionen und unterstützt das Festlegen und Ändern von Kennwörtern.

Der Connector stellt zwei Methoden zur Unterstützung der Kennwortsynchronisierung bereit:

* **Stored Procedure** (Gespeicherte Prozedur): Diese Methode erfordert zwei gespeicherte Prozeduren, um das Festlegen und Ändern von Kennwörtern zu unterstützen. Geben Sie alle Parameter für das Hinzufügen und Ändern von Kennwörtern unter **Set Password SP** (Parameter für die gespeicherte Prozedur zum Festlegen von Kennwörtern) bzw. unter **Change Password SP** (Parameter für die gespeicherte Prozedur zum Ändern von Kennwörtern) ein, wie im folgenden Beispiel zu sehen:
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Password Extension** (Kennworterweiterung): Diese Methode erfordert eine Kennworterweiterungs-DLL. (Geben Sie den Namen der Erweiterungs-DLL an, die die Schnittstelle [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) implementiert.) Die Kennworterweiterungsassembly muss im Erweiterungsordner platziert werden, damit der Connector die DLL zur Laufzeit laden kann.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Außerdem muss auf der Seite **Erweiterung konfigurieren** die Kennwortverwaltung aktiviert werden.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Configure Partitions and Hierarchies
Wählen Sie auf der Seite mit den Partitionen und Hierarchien alle Objekttypen aus. Jeder Objekttyp stellt eine eigene Partition dar.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Darüber hinaus können Sie die auf der **Konnektivitätsseite** oder auf der Seite mit den globalen **Parametern** definierten Werte überschreiben.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfigurieren von Ankern
Diese Seite ist schreibgeschützt, da der Anker bereits definiert wurde. Dem ausgewählten Ankerattribut wird immer der Objekttyp angefügt, um sicherzustellen, dass es objekttypübergreifend eindeutig ist.

![anchors](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurieren des Ausführungsschrittparameters
Diese Schritte werden für die connectorspezifischen Ausführungsprofile konfiguriert. Die Konfigurationen übernehmen das eigentliche Importieren und Exportieren von Daten.

### <a name="full-and-delta-import"></a>Vollständiger Import und Deltaimport
Der generische SQL-Connector unterstützt vollständige Importe und Deltaimporte mit folgenden Methoden:

* Tabelle
* Sicht
* Stored Procedure
* SQL-Abfrage

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Table/View**  
Zum Importieren mehrwertiger Attribute für ein Objekt müssen Sie unter **Name der mehrwertigen Tabelle/Sichten** die kommagetrennten Tabellen-/Sichtnamen und unter **Verknüpfungsbedingung** die entsprechenden Verknüpfungsbedingungen für die übergeordnete Tabelle angeben.

Ein Beispiel: Sie möchten das Employee-Objekt und alle dazugehörigen mehrwertigen Attribute importieren. Es gibt zwei Tabellen: die Haupttabelle „Employee“ und die mehrwertige Tabelle „Department“.
Gehen Sie wie folgt vor:

* Geben Sie unter **Table/View/SP** (Tabelle/Sicht/Gespeicherte Prozedur) die Zeichenfolge **Employee** ein.
* Geben Sie unter **Name of Multi-Valued table/views**(Name der mehrwertigen Tabelle/Sichten) die Zeichenfolge „Department“ ein.
* Geben Sie unter **Verknüpfungsbedingung** die Verknüpfungsbedingung zwischen „Employee“ und „Department“ ein (beispielsweise `Employee.DEPTID=Department.DepartmentID`).
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Gespeicherte Prozeduren**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Bei großen Datenmengen empfiehlt sich die Implementierung der Paginierung für gespeicherte Prozeduren.
* Damit Ihre gespeicherte Prozedur die Paginierung unterstützt, müssen Sie den Start- und den Endindex angeben. Weitere Informationen finden Sie unter [Effiziente Paginierung bei großen Datenmengen](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndex@EndIndex werden zur Ausführungszeit durch entsprechende Seitengrößenwerte ersetzt, die auf der Seite **Configure Step** (Schritt konfigurieren) konfiguriert wurden. Ein Beispiel: Angenommen, der Connector ruft die erste Seite ab, und die Seitengröße ist auf 500 festgelegt. In diesem Fall wird @StartIndex auf 1 und @EndIndex auf 500 festgelegt. Diese Werte werden größer, wenn der Connector weitere Seiten abruft, was die Änderung von @StartIndex und @EndIndex nach sich zieht.
* Geben Sie zum Ausführen einer parametrisierten gespeicherten Prozedur die Parameter im Format `[Name]:[Direction]:[Value]` an. Geben Sie die Parameter jeweils in eine separate Zeile ein. (Drücken Sie STRG+EINGABETASTE, um eine neue Zeile zu erhalten.)
* Der generische SQL-Connector unterstützt auch Importvorgänge aus Verbindungsservern in Microsoft SQL Server. Falls Informationen aus einer Tabelle auf dem Verbindungsserver abgerufen werden sollen, muss die Tabelle im folgenden Format angegeben werden: `[ServerName].[Database].[Schema].[TableName]`
* Der generische SQL-Connector unterstützt nur Objekte mit ähnlicher Struktur (Aliasname und Datentyp) zwischen Ausführungsschrittinformationen und Schemaerkennung. Wenn sich das Schema und die für den Ausführungsschritt angegebenen Informationen bei dem ausgewählten Objekt unterscheiden, unterstützt der SQL-Connector das entsprechende Szenario nicht.

**SQL-Abfrage**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Abfragen mit mehreren Resultsets werden nicht unterstützt.
* Die SQL-Abfrage unterstützt die Paginierung und stellt Start- und Endindex als Variable zur Verfügung.

### <a name="delta-import"></a>Deltaimport
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Im Vergleich zum vollständigen Import erfordert der Deltaimport einige weitere Konfigurationsschritte.

* Bei Verwendung der Nachverfolgung von Deltaänderungen mithilfe der Trigger- oder Momentaufnahmemethode müssen Sie im Feld **History Table or Snapshot database name** (Name der Verlaufstabelle oder Momentaufnahmedatenbank) den Namen der Verlaufstabelle oder der Momentaufnahmedatenbank angeben.
* Darüber hinaus müssen Sie Verknüpfungsbedingungen für die Verlaufstabelle und die übergeordnete Tabelle angeben. Beispiel: `Employee.ID=History.EmployeeID`
* Um die Transaktion der übergeordneten Tabelle anhand der Verlaufstabelle nachverfolgen zu können, müssen Sie den Namen der Spalte mit den Vorgangsinformationen (Hinzufügung/Aktualisierung/Löschung) angeben.
* Wenn Sie Deltaänderungen mithilfe der Wasserzeichenmethode nachverfolgen möchten, müssen Sie den Namen der Spalte mit den Vorgangsinformationen unter **Water Mark Column Name**(Wasserzeichenspaltenname) angeben.
* Die Spalte **change Type attribute** (Änderungstypattribut) wird für den Änderungstyp benötigt. Diese Spalte ordnet eine Änderung aus der primären oder mehrwertigen Tabelle einem Änderungstyp in der Deltasicht zu. Diese Spalte kann für Änderungen auf Attributebene den Änderungstyp „Modify_Attribute“ und für Änderungen auf Objektebene den Änderungstyp „Hinzufügen“, „Ändern“ oder „Löschen“ enthalten. Sollte es sich um einen anderen Wert als „Hinzufügen“, „Ändern“ oder „Löschen“ handeln, können Sie die entsprechenden Werte mithilfe dieser Option definieren.

### <a name="export"></a>Export
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Der generische SQL-Connector unterstützt vier Methoden für den Export:

* Tabelle
* Sicht
* Stored Procedure
* SQL-Abfrage

**Table/View**  
(Tabelle/Sicht) Bei Verwendung der Option „Table/View“ (Tabelle/Sicht) generiert der Connector die entsprechenden Abfragen und führt den Exportvorgang aus.

**Gespeicherte Prozeduren**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Bei Verwendung der Option „Stored Procedure“ (Gespeicherte Prozedur) werden drei verschiedene gespeicherte Prozeduren für Einfüge-/Aktualisierungs-/Löschvorgänge benötigt:

* **Add SP Name**(Name der gespeicherten Prozedur zum Hinzufügen): Diese gespeicherte Prozedur wird ausgeführt, wenn der Connector ein Objekt erhält, das in die entsprechende Tabelle eingefügt werden soll.
* **Update SP Name**(Name der gespeicherten Prozedur zum Aktualisieren): Diese gespeicherte Prozedur wird ausgeführt, wenn der Connector ein Objekt erhält, das in der entsprechenden Tabelle aktualisiert werden soll.
* **Delete SP Name**(Name der gespeicherten Prozedur zum Löschen): Diese gespeicherte Prozedur wird ausgeführt, wenn der Connector ein Objekt erhält, das in der entsprechenden Tabelle gelöscht werden soll.
* Attribut aus dem Schema, das als Parameterwert für die gespeicherte Prozedur verwendet wird. Beispiel: `EmployeeName: INPUT: @EmployeeName` („EmployeeName“ ist im Connectorschema ausgewählt, und der Connector ersetzt den entsprechenden Wert im Zuge des Exportvorgangs.)
* Geben Sie zum Ausführen einer parametrisierten gespeicherten Prozedur Parameter im Format `[Name]:[Direction]:[Value]` an. Geben Sie die Parameter jeweils in eine separate Zeile ein. (Drücken Sie STRG+EINGABETASTE, um eine neue Zeile zu erhalten.)

**SQL query**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Bei Verwendung der Option „SQL-Abfrage“ werden drei verschiedene Abfragen für Einfüge-/Aktualisierungs-/Löschvorgänge benötigt:

* **Insert Query**(Abfrage für Einfügevorgänge): Diese Abfrage wird ausgeführt, wenn der Connector ein Objekt erhält, das in die entsprechende Tabelle eingefügt werden soll.
* **Update Query**(Abfrage für Aktualisierungsvorgänge): Diese Abfrage wird ausgeführt, wenn der Connector ein Objekt erhält, das in der entsprechenden Tabelle aktualisiert werden soll.
* **Delete Query**(Abfrage für Löschvorgänge): Diese Abfrage wird ausgeführt, wenn der Connector ein Objekt erhält, das in der entsprechenden Tabelle gelöscht werden soll.
* Attribut aus dem Schema, das als Parameterwert für die Abfrage verwendet wird. Beispiel: `Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Problembehandlung
* Informationen zum Aktivieren der Protokollierung für die Behandlung von Connectorproblemen finden Sie unter [Vorgehensweise: Aktivieren der ETW-Ablaufverfolgung für Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).

