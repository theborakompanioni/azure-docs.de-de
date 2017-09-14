---
title: Neuerungen in Azure Data Catalog | Microsoft Docs
description: "Dieser Artikel bietet eine Übersicht über die neu hinzugefügten Funktionen von Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 09/06/2017
ms.author: maroche
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 7259505b68202f3741e64e86555d2e3f968b8951
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="whats-new-in-azure-data-catalog"></a>Neuerungen in Azure Data Catalog
Updates für **Azure Data Catalog** werden in regelmäßigen Abständen veröffentlicht. Nicht jede Version umfasst neue Funktionen für Benutzer, da sich einige Versionen auf Back-End-Dienstfunktionen konzentrieren. Auf dieser Seite werden neue Funktionen für Benutzer aufgezeigt, die dem Dienst Azure Data Catalog hinzugefügt wurden.

## <a name="whats-new-for-september-2017"></a>Neuerungen im September 2017 
September 2017 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:

* Unterstützung des Extrahierens von Metadaten von Join-Beziehungen aus DB2-Datenquellen beim Registrieren verknüpfter Tabellen mit dem Registrierungstool für Datenquellen
* Unterstützung für die Registrierung von Datenquellen mit der MongoDB-Version 3.4 mit dem Datenquellen-Registrierungstool
* Unterstützung des Löschens aller Metadaten für die enthaltenen Objekte in einem einzigen Vorgang, wenn eine Datenbank oder ein anderer Container aus Data Catalog entfernt wird
* Unterstützung für die Anzeige von bis zu 1.000 Tags, Begriffe aus dem Unternehmensglossar oder anderen Suchmerkmalen beim Verfeinern einer Suche im Data Catalog-Portal


## <a name="whats-new-for-august-2017"></a>Neuerungen im August 2017 
August 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

*   Ein neues Beispiel für Entwickler ist zum Erstellen und Verwalten von Beziehungsmetadaten mithilfe der REST-API von Data Catalog verfügbar. Das Beispiel *Import relationship information into Data Catalog* steht auf der [Seite mit den Data Catalog-Codebeispielen](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0) zur Verfügung. 
* Unterstützung des Extrahierens von Metadaten von Join-Beziehungen aus Teradata-Datenquellen beim Registrieren verknüpfter Tabellen mit dem Registrierungstool für Datenquellen.
* Unterstützung von SQL Server-Tabellenwertfunktions-Objekten (TVF) beim Registrieren von SQL Server-Datenquellen mit dem Registrierungstool für Datenquellen.
* Mehrere Aktualisierungen und Überarbeitungen, um Leistung und Nutzbarkeit des Data Catalog-Portals zu steigern.

## <a name="whats-new-for-july-2017"></a>Neuerungen im Juli 2017 
Juli 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Unterstützung einer detaillierteren Steuerung zulässiger Vorgänge mit Metadaten:
    - Katalogadministratoren können die Fähigkeit von Benutzern, dem Katalog Tags und zugehörige Metadaten hinzuzufügen, einschränken, indem sie einen schreibgeschützten Zugriff auf den Katalog aktivieren.
    - Katalogadministratoren können die Fähigkeit der Benutzer einschränken, neue Datenquellen im Katalog zu registrieren.
    - Katalogadministratoren können die Fähigkeit der Benutzer einschränken, den Besitz von Metadaten von Datenobjekten im Katalog zu übernehmen.
    - Zur Vereinfachung der Verwaltung von Berechtigungen können Azure Active Directory-Sicherheitsgruppen und -Benutzern Berechtigungen erteilt werden.
* Unterstützung von Beziehungen zwischen registrierten Datenobjekten und der Ermittlung verwandter Datenobjekte im Data Catalog-Portal:
    - Extrahieren von Beziehungsmetadaten aus SQL Server- (einschließlich Azure SQL-Datenbank), Oracle- und MySQL-Datenquellen bei Verwenden des Registrierungstools für Data Catalog-Datenquellen.
    - Ermittlung verwandter Datenobjekte beim Anzeigen von Objektmetadaten im Data Catalog-Portal.
    - Vorgänge zum Definieren, Ermitteln und Verwalten von Beziehungen zwischen Datenobjekten mithilfe der REST-API von Data Catalog.

Weitere Informationen zum Verwalten von Berechtigungen in Data Catalog finden Sie unter [Schützen des Zugriffs auf Data Catalog und Datenobjekte](data-catalog-how-to-secure-catalog.md).
Weitere Informationen zu Beziehungen in Data Catalog finden Sie unter [Anzeigen verknüpfter Datenobjekte in Azure Data Catalog](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Neuerungen im Juni 2017 
Juni 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Unterstützung der Datenquellen Sybase, Apache Cassandra und MongoDB. Benutzer können jetzt Cassandra- und MongoDB-Datenbanken und -Tabellen sowie Sybase-Datenbanken, -Tabellen und -Sichten registrieren und ermitteln.

> [!NOTE]
> Wenn Sie MongoDB- und Cassandra-Tabellen registrieren, die Spalten mit komplexen Datentypen, z.B. Datensätzen und Arrays, enthalten, werden diese Spalten nicht in die Metadaten einbezogen, die Data Catalog hinzugefügt werden.

## <a name="whats-new-for-may-2017"></a>Neuerungen im Mai 2017 
Mai 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Für den Massenimport von Unternehmensglossarbegriffen ist ein neues Beispiel für Entwickler verfügbar. Das Beispiel „Glossary Bulk Import“ (Massenimport von Glossarbegriffen) finden Sie auf der Seite [Data Catalog – Entwicklerbeispiele](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Unterstützung der Bearbeitung von ODBC-Verbindungsinformationen im Azure Data Catalog-Portal. Besitzer von Datenobjekten und Data Catalog-Administratoren können nun die Verbindungsinformationen für registrierte ODBC-Datenquellen bearbeiten, ohne die Datenquellen erneut registrieren zu müssen.
*   Unterstützung für klickbare URLs in Definitionen und Beschreibungen im Unternehmensglossar. Wenn in den Beschreibungs- und Definitionseigenschaften von Unternehmensglossarbegriffen URLs enthalten sind, werden diese URLs im Data Catalog-Portal als Links angezeigt.
*   Unterstützung der Anzeige der Namen von Experten zusätzlich zur E-Mail-Adresse Beim Anzeigen von Experten in den Eigenschaften eines Datenobjekts im Data Catalog-Portal wird in der QuickInfo der vollständige Name des Experten in Azure Active Directory angezeigt.

## <a name="whats-new-for-april-2017"></a>Neuerungen im April 2017 
April 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Unterstützung von ODBC-Datenquellen. Benutzer können jetzt ODBC-Datenbanken, -Tabellen und -Sichten mithilfe des Registrierungstools für Data Catalog-Datenquellen registrieren und ermitteln.

## <a name="whats-new-for-march-2017"></a>Neuerungen im März 2017 
März 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Unterstützung der Verwendung von AAD-Sicherheitsgruppen für Data Catalog-Administratoren.
*   Azure Data Catalog ist jetzt in einer neuen Azure-Region verfügbar. Kunden können Azure Data Catalog nun neben den Regionen „USA, Osten“, „USA, Westen“, „Europa, Westen“ und „Australien, Osten“ auch in den Regionen „Europa, Norden“ und „Asien, Südosten“ in der Region „USA, Westen-Mitte“ bereitstellen. Weitere Informationen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Neuerungen im Februar 2017 
Februar 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Unterstützung erweiterter Einstellungen im Registrierungstool für Data Catalog-Datenquellen. Benutzer können Timeoutwerte angeben, wenn sie große Datenquellen registrieren.
*   Unterstützung der Datenquellen FTP und PostgreSQL Benutzer können jetzt FTP-Dateien und -Verzeichnisse sowie PostgreSQL-Datenbanken, -Tabellen und -Sichten registrieren und ermitteln.

## <a name="whats-new-for-january-2017"></a>Neuerungen im Januar 2017 
Januar 2017 wurden Azure Data Catalog folgende Funktionen hinzugefügt:
*   Azure Data Catalog ist jetzt mit [CSA STAR](https://www.microsoft.com/trustcenter/compliance/csa-star-certification) konform.
*   Integration mit [Abrufen und Transformieren in Excel 2016 und Power Query für Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel-Benutzer können innerhalb von Excel mithilfe von Azure Data Catalog Abfragen freigeben und ermitteln. Diese Funktion ist für Benutzer mit Power BI Pro-Lizenzen verfügbar.

## <a name="whats-new-for-december-2016"></a>Neuerungen im Dezember 2016
Dezember 2016 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:
*   Azure Data Catalog ist jetzt mit [HIPAA](https://www.microsoft.com/trustcenter/Compliance/HIPAA) und [EU-Modellklauseln](https://www.microsoft.com/TrustCenter/Compliance/EU-Model-Clauses) konform.
*   Unterstützung für die Bearbeitung der Verbindungsinformationen für Datenquellen. Besitzer von Datenobjekten und Data Catalog-Administratoren können nun die Verbindungsinformationen für registrierte Datenquellen bearbeiten, ohne die Datenquellen erneut registrieren zu müssen.
*   Unterstützung für Salesforce.com-Datenquellen. Benutzer können nun Salesforce-Objekte registrieren und ermitteln.


## <a name="whats-new-for-november-2016"></a>Neuerungen im November 2016
November 2016 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:
*   Azure Data Catalog ist jetzt mit [ISO/IEC 27001](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001) und [ISO/IEC 27018](https://www.microsoft.com/TrustCenter/Compliance/iso-iec-27018) konform.
*   Unterstützung für die manuelle Registrierung von ODBC-Datenquellen mithilfe von Data Catalog-Portal und REST-API.

## <a name="whats-new-for-september-2016"></a>Neuerungen im September 2016
September 2016 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:

* Unterstützung von IBM DB2-Datenquellen. Benutzer können nun DB2-Datenbanken, -Tabellen und -Sichten registrieren und ermitteln.
* Unterstützung von Azure Cosmos DB-Datenquellen. Benutzer können nun Cosmos DB-Datenbanken und -Sammlungen registrieren und ermitteln.
* Unterstützung der Anpassung des Katalognamens im Data Catalog-Portal. Catalog-Administratoren können nun Text für den Titel des Portals angeben (etwa den Namen der Organisation).

## <a name="whats-new-for-august-2016"></a>Neuerungen im August 2016
August 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Erweiterungen für die Registrierung von SQL Server-MDS-Datenquellen (Master Data Services). Benutzer können nun Vorschauen und Datenprofile einschließen, wenn sie MDS-Entitäten mithilfe des Registrierungstools für Data Catalog-Datenquellen registrieren.
* Unterstützung administratordefinierter gespeicherter Organisationssuchen. Beim Speichern einer Suche im Data Catalog-Portal können Data Catalog-Administratoren nun Suchvorgänge für den persönlichen Gebrauch oder für alle Katalogbenutzer speichern. Gespeicherte Organisationssuchen werden für alle Katalogbenutzer freigegeben und können als standardisierter Ausgangspunkt für die Ermittlung von Datenquellen fungieren.
* Aktualisierung der Eigenschaftenansicht im Data Catalog-Portal. Zur Verbesserung der Konsistenz und Übersichtlichkeit werden nun alle Datenasseteigenschaften in einem einzelnen Bereich mit anpassbarer Größe angezeigt und verwaltet.

## <a name="whats-new-for-july-2016"></a>Neuerungen im Juli 2016
Juli 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Unterstützung von SQL Server-MDS-Datenquellen (Master Data Services). Benutzer können nun MDS-Modelle und -Entitäten registrieren und ermitteln.
* Unterstützung gespeicherter SQL Server-Prozeduren. Benutzer können nun Objekte gespeicherter Prozeduren in SQL Server-Datenquellen registrieren und ermitteln.
* Unterstützung zusätzlicher Sprachen im Azure Data Catalog-Portal und dem Tool zum Registrieren von Datenquellen. Damit werden insgesamt 18 Sprachen unterstützt. Die in Azure Data Catalog verwendete Sprache entspricht den in Windows oder im Webbrowser festgelegten Spracheinstellungen.
* Aktualisierung und Optimierung der Startseite des Data Catalog-Portals (einschließlich Verbesserung der Leistung und Benutzerfreundlichkeit).

## <a name="whats-new-for-june-2016"></a>Neuerungen im Juni 2016
Juni 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Unterstützung der Größenanpassung von Spalten in der Listenansicht beim Ermitteln von Datenassets im Data Catalog-Portal. Benutzer können nun die Größe einzelner Spalten anpassen und dadurch lange Assetmetadaten wie Tags und Beschreibungen einfacher lesen.
* Dem Menü „Öffnen in“ des Data Catalog-Portals wurde Power Query für Excel hinzugefügt. Benutzer können nun unterstützte Datenquellen in Excel 2016 oder in Excel 2010 und Excel 2013 öffnen, wenn das Add-In [Power Query für Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) installiert ist.
* Unterstützung von Azure Table Storage-Datenquellen. Benutzer können nun Tabellenobjekte in Azure Storage-Datenquellen registrieren und ermitteln.

## <a name="whats-new-for-may-2016"></a>Neuerungen im Mai 2016
Mai 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Ein Unternehmensglossar, mit dem Katalogadministratoren Geschäftsbedingungen und Hierarchien definieren können, um ein allgemeines Geschäftsvokabular zu erstellen. Benutzer können registrierte Datenassets mit Glossarbegriffen markieren, um das Erkennen und Verstehen des Kataloginhalts zu vereinfachen. Weitere Informationen finden Sie unter [Einrichten des Unternehmensglossars für gesteuertes Markieren](data-catalog-how-to-business-glossary.md)  
* Erweiterungen im Data Catalog-Unternehmensglossar, die es Benutzern ermöglichen, mehrere Glossarbegriffe in einem einzigen Vorgang zu aktualisieren. Benutzer können mehrere Begriffe auswählen, um folgende Felder zu bearbeiten:
  * Übergeordneter Begriff: Der Benutzer kann einen neuen übergeordneten Begriff auswählen. Alle ausgewählten Begriffe werden so aktualisiert, dass sie untergeordnete Begriffe des ausgewählten übergeordneten Begriffs darstellen. Wenn alle ausgewählten Begriffe über denselben übergeordneten Begriff verfügen, wird dieser übergeordnete Begriff im Textfeld angezeigt. Andernfalls ist das Feld „übergeordneter Begriff“ leer.   
  * Tags und Beteiligte: Benutzer können Tags und Beteiligte für mehrere Glossarbegriffe mithilfe desselben Szenarios wie beim Markieren mehrerer Datenassets hinzufügen und entfernen.

> [!NOTE]
> Das Unternehmensglossar ist nur in der Standard Edition von Azure Data Catalog verfügbar. Die kostenlose Edition bietet keine Funktionen für gesteuertes Markieren bzw. ein Unternehmensglossar.

## <a name="whats-new-for-march-2016"></a>Neuerungen im März 2016
März 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Ein konsolidierter REST-API-Endpunkt für den programmgesteuerten Zugriff auf die Suchfunktionen und Funktionen zur Katalogbestandsverwaltung des Azure Data Catalog-Diensts. Der Such-API-Endpunkt und Katalog-API-Endpunkt waren veraltet und wurden zum 21. März 2016 eingestellt. Es gibt keine Änderungen an der API-Semantik. Nur der Endpunkt-URI wurde geändert. Weitere Informationen finden Sie in der [Azure Data Catalog REST-API-Referenz](https://msdn.microsoft.com/library/azure/mt267595.aspx). API-Beispiele finden Sie unter [Azure Data Catalog – Entwicklerbeispiele](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Neuerungen im Februar 2016
Februar 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Eine neu gestaltete Datenquellenauswahl im Tool zum Registrieren von Datenquellen von Azure Data Catalog. Das Tool zum Registrieren von Datenquellen wurde aktualisiert, um Ihnen das Suchen und Auswählen der von Azure Data Catalog unterstützten Datenquellen zu erleichtern.
* Unterstützung für zehn weitere Sprachen im Azure Data Catalog-Portal und dem Tool zum Registrieren von Datenquellen. Zusätzlich zu Englisch ist Azure Data Catalog jetzt in Deutsch, Spanisch, Französisch, Italienisch, Japanisch, Koreanisch, Portugiesisch (Brasilien), Russisch, Chinesisch (vereinfacht) und Chinesisch (traditionell) verfügbar. Die in Azure Data Catalog verwendete Sprache entspricht den in Windows oder im Webbrowser des Benutzers festgelegten Spracheinstellungen.
* Unterstützung für die Georeplikation von Azure Data Catalog-Daten für die Geschäftskontinuität und Notfallwiederherstellung. Alle Azure Data Catalog-Inhalte, einschließlich Datenquellenmetadaten und per Crowdsourcing gesammelter Kommentare, werden jetzt zwischen zwei Azure-Regionen repliziert, ohne dass für Kunden zusätzliche Kosten anfallen. Die Azure-Regionen sind vorab festgelegt, mindestens 500 Meilen voneinander entfernt und entsprechen der unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](../best-practices-availability-paired-regions.md)beschriebenen Zuordnung.
* Unterstützung der Änderung der von Azure Data Catalog verwendeten Azure-Abonnements. Administratoren von Azure Data Catalog können auf der Seite „Einstellungen“ im Azure Data Catalog-Portal zu Abrechnungszwecken ein anderes Azure-Abonnement auswählen.

## <a name="whats-new-for-january-2016"></a>Neuerungen im Januar 2016
Januar 2016 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Unterstützung für die manuelle Registrierung zusätzlicher Datenquellen. Sie können jetzt im Azure Data Catalog-Portal manuelle Einträge erstellen oder die Azure Data Catalog-REST-API zum Registrieren der folgenden Datenquellen verwenden:
  * OData – Funktion, Entitätenmenge und Entitätencontainer
  * HTTP – Datei, Endpunkt, Bericht und Website
  * Dateisystem – Datei
  * SharePoint – Liste
  * FTP – Dateien und Verzeichnisse
  * Salesforce.com – Objekt
  * DB2 – Tabelle, Ansicht und Datenbank
  * PostgreSQL – Tabelle, Ansicht und Datenbank
* Unterstützung für „Open in SQL Server Data Tools“ für SQL Server-Datenquellen (einschließlich Azure SQL-Datenbank und Azure SQL Data Warehouse).  
* Unterstützung für das Registrieren und Ermitteln von SAP HANA-Ansichten und -Paketen Mit dem Registrierungstool für Datenquellen von Azure Data Catalog können Sie SAP HANA-Datenquellen registrieren, und mit dem Azure Data Catalog-Portal können sie registrierte SAP HANA-Datenquellen mit Anmerkungen versehen und ermitteln.
* Die Möglichkeit zum Anheften und Entfernen von Datenassets im Azure Data Catalog-Portal. Sie können Datenassets anheften, um sie leichter finden und erneut verwenden zu können.
* Neu gestaltete Startseite im Azure Data Catalog-Portal. Die neue Homepage ermöglicht Einblicke in die aktuelle Benutzeraktivität – darunter kürzlich veröffentlichte Assets, angeheftete Assets und gespeicherte Suchvorgänge – sowie einen Einblick in die Aktivität im Katalog als Ganzes.
* Unterstützung für persistente Benutzereinstellungen im Azure Data Catalog-Portal. Benutzeroberflächeneinstellungen – z. B. Raster- oder Kachelansicht, die Anzahl der Ergebnisse pro Seite und Aktivieren bzw. Deaktivieren von Treffermarkierungen – werden sitzungsübergreifend beibehalten.
* Azure Data Catalog ist jetzt in zwei neuen Azure-Regionen verfügbar. Kunden können Azure Data Catalog nun neben den Regionen „USA, Osten“, „USA, Westen“, „Europa, Westen“ und „Australien, Osten“ auch in den Regionen „Europa, Norden“ und „Asien, Südosten“ bereitstellen. Weitere Informationen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

> [!NOTE]
> „Open in SQL Server Data Tools“ erfordert die Installation von Visual Studio 2013 mit Update 4 und SQL Server-Tools. Besuchen Sie [Herunterladen von SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx), um die aktuelle Version von SQL Server Data Tools zu installieren.


## <a name="whats-new-for-december-2015"></a>Neuerungen im Dezember 2015
Dezember 2015 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:

* Unterstützung für Datenprofile für Azure SQL Data Warehouse-Datenquellen. Beim Registrieren von Azure SQL Data Warehouse-Tabellen und -Sichten können Benutzer festlegen, dass in die aus der Datenquelle extrahierten Metadaten auch Datenprofilmetriken eingeschlossen werden sollen.
* Unterstützung für das Registrieren und Ermitteln von MySQL-Objekten und -Datenbanken. Mit dem Registrierungstool für Datenquellen von Azure Data Catalog können die Benutzer MySQL-Datenquellen registrieren, und mit dem Azure Data Catalog-Portal können sie registrierte MySQL-Datenquellen mit Anmerkungen versehen und ermitteln.
* Unterstützung für SPNEGO und Windows-Authentifizierung für Teradata-Datenquellen. Bei der Registrierung von Teradata-Tabellen und -Sichten können Benutzer festlegen, dass die Herstellung der Verbindung mit Teradata mithilfe von SNPEGO und Windows-Authentifizierung und mit LDAP-Authentifizierung und TD2-Authentifizierung erfolgen soll.
* Unterstützung für Datenquellen aus dem Azure Data Lake-Speicher. Benutzer können nun Azure Data Catalog verwenden, um Datenquellen aus dem Azure Data Lake-Speicher zu registrieren und zu ermitteln.
* Unterstützung für das manuelle Festlegen von Netzwerk-Proxyeinstellungen im Registrierungstool für Datenquellen von Azure Data Catalog. Benutzer können auf der Startseite des Tools die Option „Proxyeinstellungen ändern“ auswählen und die Proxyadresse und den Port angeben, der vom Tool verwendet werden soll.

## <a name="whats-new-for-november-2015"></a>Neuerungen im November 2015
November 2015 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:

* Möglichkeit zum Anzeigen und Kopieren von Verbindungszeichenfolgen über das Azure Data Catalog-Portal für SQL Server-Datenquellen (einschließlich Azure SQL-Datenbank) und Oracle-Datenquellen. Benutzer können in den Verbindungsinformationen für eine Tabelle, Ansicht oder Datenbank von SQL Server oder Oracle auf den Link „Verbindungszeichenfolgen anzeigen“ klicken, um die Verbindungszeichenfolgen zum Herstellen der Verbindung mit der Datenquelle anzuzeigen. Für SQL Server-Datenquellen werden ADO.NET-, ODBC-, OLEDB- und JDBC-Verbindungszeichenfolgen bereitgestellt. Für Oracle-Datenquellen werden ODBC- und OLEDB-Verbindungszeichenfolgen bereitgestellt.
* Unterstützung für das Einbinden von Datenprofilen, wenn Teradata-Tabellen und -Ansichten registriert werden.
* Unterstützung für „Open in Power BI Desktop“ für SQL Server-Datenquellen (einschließlich Azure SQL-Datenbank und Azure SQL Data Warehouse), SQL Server Analysis Services-, Azure Storage- und HDFS-Quellen.  
* Unterstützung für die LDAP-Authentifizierung für Teradata-Datenquellen. Bei der Registrierung von Teradata-Tabellen und -Sichten können Benutzer festlegen, dass die Herstellung der Verbindung mit Teradata mithilfe der LDAP-Authentifizierung und der TD2-Authentifizierung erfolgen soll.
* Unterstützung für „In Excel öffnen“ für Teradata-Datenquellen.
* Unterstützung für zuletzt verwendete Suchbegriffe im Azure Data Catalog-Portal. Bei der Suche im Portal können Benutzer die zuletzt verwendeten Suchbegriffe auswählen, um den Ermittlungsvorgang zu beschleunigen.
* Unterstützung für die Vorschau für Teradata-Datenquellen. Beim Registrieren von Teradata-Tabellen und -Sichten können Benutzer festlegen, dass Momentaufnahmedatensätze mit den extrahierten Metadaten aus der Datenquelle eingeschlossen werden sollen.
* Unterstützung für „In Excel öffnen“ für Azure SQL Data Warehouse-Datenquellen
* Unterstützung für die Definition und Bearbeitung von Schemas auf Spaltenebene für manuell registrierte Datenassets. Nach dem manuellen Erstellen eines Datenassets mit dem Azure Data Catalog-Portal können Benutzer in den Eigenschaften der Datenassets Spaltendefinitionen hinzufügen.
* Unterstützung für Abfragen vom Typ „has“ beim Durchsuchen von Azure Data Catalog, um die Ermittlung von registrierten Datenassets mit bestimmten Metadaten zu ermöglichen. Die Azure Data Catalog-Abfragesyntax enthält nun Folgendes:

| Abfragesyntax | Zweck |
| --- | --- |
| `has:previews` |Sucht nach Datenassets, die eine Vorschau beinhalten. |
| `has:documentation` |Sucht nach Datenassets, für die eine Dokumentation bereitgestellt wurde. |
| `has:tableDataProfiles` |Sucht nach Datenassets mit Datenprofilinformationen auf Tabellenebene. |
| `has:columnsDataProfiles` |Sucht nach Datenassets mit Datenprofilinformationen auf Spaltenebene. |


> [!NOTE]
> Für „Open in Power BI Desktop“ muss eine aktuelle Version der Anwendung Power BI Desktop installiert sein. Wenn bei der Verwendung dieser Funktion Probleme oder Fehler auftreten, vergewissern Sie sich, dass Sie die neueste Version von Power BI Desktop von [PowerBI.com](https://powerbi.com) heruntergeladen haben.


## <a name="whats-new-for-october-2015"></a>Neuerungen im Oktober 2015
Oktober 2015 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Unterstützung für die Verschlüsselung ruhender Daten bei der Datenvorschau und in Datenprofilen für registrierte Datenquellen. Azure Data Catalog verschlüsselt alle Datenquellen von Vorschaudatensätzen und Datenprofilen, die im Dienst registriert sind, in transparenter Weise. Schlüsselmanagement durch Catalog-Administratoren ist dafür nicht erforderlich.
* Unterstützung für Teradata-Datenquellen. Benutzer können nun Teradata-Tabellen und -Ansichten registrieren und ermitteln.
* Unterstützung für lokale Hive-Datenquellen. Benutzer können jetzt Hive-Tabellen für Apache Hive in Hadoop in lokalen Datenquellen registrieren und ermitteln.
* Unterstützung für gespeicherte Suchen im Azure Data Catalog-Portal. Benutzer können Suchbegriffe speichern und die Auswahl filtern, um vorherige Suchen leicht wiederholen zu können und hilfreiche Ansichten des Kataloginhalts zu definieren. Außerdem können Benutzer eine gespeicherte Suche als Standardsuche markieren. Wenn ein Benutzer auf der Startseite des Azure Data Catalog-Portals oder auf der Seite mit den ersten Schritten auf das Suchsymbol mit der Lupe klickt, wird der Benutzer direkt zur gespeicherten Suche geleitet, die als Standardsuche markiert ist.
* Unterstützung für Rich Text-Dokumentation für registrierte Datenbestände und Container im Azure Data Catalog-Portal. Benutzer können jetzt Dokumentation für Datenbestände, z. B. Tabellen, Ansichten und Berichte, sowie für Container, z. B. Datenbanken und Modelle, für Szenarien bereitstellen, bei denen Tags und Beschreibungen nicht ausreichen.
* Unterstützung für die manuelle Registrierung bekannter Datenquellentypen. Benutzer können Datenquelleninformationen manuell über das Azure Data Catalog-Portal für alle Datentypen eingeben, die von Azure Data Catalog unterstützt werden.
* Unterstützung für die Autorisierung von Azure Active Directory-Sicherheitsgruppen. Catalog-Administratoren können den Zugriff des Katalogs auf Sicherheitsgruppen und Benutzerkonten ermöglichen, um die Verwaltung des Zugriffs auf Azure Data Catalog zu vereinfachen.
* Unterstützung für das Öffnen von Hive-Datenquellen in Excel über das Azure Data Catalog-Portal.

> [!NOTE]
> Bei der aktuellen Version wird nur die Teradata TD2-Authentifizierung unterstützt. Zusätzliche Authentifizierungsmethoden werden in zukünftigen Releases unterstützt.

> [!NOTE]
> Zum Verwenden des Features „In Excel öffnen“ für Hive-Datenquellen müssen Benutzer den ODBC-Treiber für Hive installieren.

## <a name="whats-new-for-september-2015"></a>Neuerungen im September 2015
September 2015 wurden Azure Data Catalog die folgenden Funktionen hinzugefügt:

* Unterstützung für das Einbinden von Datenprofilen, wenn Hive-Datenquellen registriert werden.
* Unterstützung für programmgesteuertes Ermitteln der Katalog-API, was die Integration von Anwendungen in Azure Data Catalog vereinfacht.
* Eine neue Übersicht über die Möglichkeiten zur Ermittlung von Datenquellen im Azure Data Catalog-Portal. Wenn Benutzer die Seite „Ermitteln“ des Azure Data Catalog-Portals aufrufen, ohne einen Suchbegriff einzugeben, wird eine Übersicht der Kataloginhalte angezeigt, einschließlich der am häufigsten verwendeten Tags, Experten, Datenquellentypen und Objekttypen.
* Unterstützung für das Registrieren und Ermitteln von Azure SQL Data Warehouse-Objekten und -Datenbanken. Weitere Informationen zu Azure SQL Data Warehouse finden Sie unter [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Unterstützung für das Registrieren und Ermitteln von SQL Server Analysis Services-Modellen und SQL Server Reporting Services-Servern als Container. Azure Data Catalog erstellt beim Registrieren von SSAS- und SSRS-Objekten einen Eintrag für das SSAS-Modell und den SSRS-Server sowie für die Berichte und andere Objekte. Die Container können über das Azure Data Catalog-Portal ermittelt und mit Anmerkungen versehen werden. Benutzer können neben den Inhalten des Katalogs auch die Inhalte eines Modells oder Servers durchsuchen und filtern.
* Unterstützung für das Registrieren und die Ermittlung von SQL Server Analysis Services-Objekten über HTTP/HTTPS. Benutzer können jetzt mithilfe einer URL (z.B. https://servername/olap/msmdpump.dll) anstelle eines Servernamens eine Verbindung mit SSAS-Servern herstellen und die Standardauthentifizierung und anonyme Verbindungen zusätzlich zur Windows-Authentifizierung verwenden. Weitere Informationen zu HTTP/HTTPS-Verbindungen mit SSAS finden Sie unter [Konfigurieren des HTTP-Zugriffs auf Analysis Services](https://msdn.microsoft.com/library/gg492140.aspx).
* Unterstützung für die Hive-Datenquellen auf HDInsight. Benutzer können jetzt Hive-Tabellen für Apache Hive in Hadoop in HDInsight-Datenquellen registrieren und ermitteln. Weitere Informationen zu Hive in HDInsight finden Sie im [HDInsight Documentation Center](../hdinsight/hdinsight-use-hive.md).
* Unterstützung für das Registrieren und Ermitteln von Oracle-Datenbanken und HDFS-Clustern als Container Beim Registrieren von Oracle-Tabellen und -Sichten oder HDFS erstellt Azure Data Catalog einen Eintrag für die Datenbank sowie für die Tabellen und Sichten. Die Datenbank kann über das Azure Data Catalog-Portal ermittelt und mit Anmerkungen versehen werden. Benutzer können neben den Inhalten des Katalogs auch die Inhalte einer Datenbank oder eines Clusters durchsuchen und filtern.
* Unterstützung für die manuelle Registrierung unbekannter Datenquellentypen Benutzer können über das Azure Data Catalog-Portal manuell Datenquelleninformationen eingeben, sodass Datenquellen, die nicht explizit vom Quellregistrierungstool unterstützt werden, mit Anmerkungen versehen und ermittelt werden können.
* Unterstützung für das Registrieren und Ermitteln von SQL Server-Datenbanken als Container Beim Registrieren von SQL Server-Tabellen und -Sichten erstellt Azure Data Catalog einen Eintrag für die Datenbank, Tabellen und Sichten. Die Datenbank kann über das Azure Data Catalog-Portal ermittelt und mit Anmerkungen versehen werden. Benutzer können neben den Inhalten des Katalogs auch die Inhalte einer Datenbank durchsuchen und filtern.

> [!NOTE]
> SSAS- und SSRS-Objekte, die vor dem Release vom 18. September registriert wurden, müssen mithilfe des Registrierungstools für Datenquellen erneut registriert werden, bevor der Modell- oder Servereintrag dem Katalog hinzugefügt wird. Das erneute Registrieren einer Datenquelle wirkt sich nicht auf Anmerkungen aus, die von Benutzern im Azure Data Catalog-Portal hinzugefügt wurden.

> [!NOTE]
> Oracle-Tabellen und -Ansichten und HDFS-Dateien und -Verzeichnisse, die vor dem Release vom 11. September registriert wurden, müssen mithilfe des Registrierungstools für Datenquellen erneut registriert werden, bevor der Datenbank- oder Clustereintrag dem Katalog hinzugefügt wird. Das erneute Registrieren einer Datenquelle wirkt sich nicht auf Anmerkungen aus, die von Benutzern im Azure Data Catalog-Portal hinzugefügt wurden.

> [!NOTE]
> SQL Server-Tabellen und -Sichten, die vor dem Release vom 4. September registriert wurden, müssen mithilfe des Registrierungstools für Datenquellen erneut registriert werden, bevor der Datenbankeintrag dem Katalog hinzugefügt wird. Das erneute Registrieren einer Datenquelle wirkt sich nicht auf Anmerkungen aus, die von Benutzern im Azure Data Catalog-Portal hinzugefügt wurden.

## <a name="whats-new-for-august-2015"></a>Neuerungen im August 2015
August 2015 wurden Azure Data Catalog folgende Funktionen hinzugefügt:

* Unterstützung für die Datenprofilerstellung von SQL Server- und Oracle-Datenquellen. Beim Registrieren von SQL Server- und Oracle-Tabellen und -Sichten können Benutzer optional Datenprofilinformationen für die registrierten Objekte einbeziehen. Das Datenprofil umfasst Statistiken auf Objektebene und auf Spaltenebene.
* Unterstützung für Hadoop HDFS-Datenquellen. Benutzer können nun HDFS-Dateien und -Verzeichnisse registrieren und ermitteln.
* Unterstützung zur Bereitstellung von Zugriffsanforderungsinformationen für registrierte Datenquellen. Benutzer können nun zur einfachen Integration mit vorhandenen Tools oder Prozessen für alle registrierten Datenassets Anweisungen für Zugriffsanforderungen angeben, z. B. E-Mail-Links oder URLs.
* QuickInfos für Tags und Experten, sodass leichter ermittelt werden kann, welche Benutzer welche Metadaten für registrierte Datenassets angegeben haben.
* In der oberen Navigationsleiste wurde die neue Schaltfläche "Benutzer" mit dem entsprechenden Menü hinzugefügt. In diesem Menü kann der Benutzer das für die Anmeldung bei Azure Data Catalog verwendete Konto anzeigen und sich bei Bedarf abmelden. In diesem Menü wird zudem der Katalogname angezeigt. Dies ist nützlich für Entwickler, die die Azure Data Catalog-REST-API verwenden.
* Nur Standard Edition: Beim Hinzufügen von Besitzern zu Datenbeständen unterstützt Azure Data Catalog jetzt sowohl Benutzerkonten als auch Sicherheitsgruppen als Besitzer. Um eine Sicherheitsgruppe als Besitzer für ausgewählte Datenasset hinzuzufügen, können Sie entweder den Anzeigenamen der Gruppe oder die UPN-E-Mail-Adresse der Gruppe (sofern vorhanden) eingeben.
* Unterstützung für Datenquellen in Azure Blob Storage. Benutzer können nun Azure Storage-Blobs und -Verzeichnisse registrieren und ermitteln.


