---
title: "Connector – Versionsveröffentlichungsverlauf | Microsoft Docs"
description: "Dieses Thema listet alle Versionen des Connectors für Forefront Identity Manager (FIM) und Microsoft Identity Manager (MIM) auf."
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 313145f4d8e5faa91fb3504cb0fd0ba87ca2e379
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="connector-version-release-history"></a>Connector – Versionsveröffentlichungsverlauf
Die Connectors für Forefront Identity Manager (FIM) und Microsoft Identity Manager (MIM) werden regelmäßig aktualisiert.

> [!NOTE]
> Dieses Thema behandelt nur FIM und MIM. Die Installation dieser Connectors wird für Azure AD Connect nicht unterstützt. Bei einem Upgrade auf den angegebenen Build werden veröffentlichte Connectors in AADConnect vorinstalliert.

In diesem Thema sind alle veröffentlichten Connector-Versionen aufgeführt.

Verwandte Links:

* [Die neuesten Connectors herunterladen](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Generischer LDAP-Connector](active-directory-aadconnectsync-connector-genericldap.md) – Referenzdokumentation
* [Generischer SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md) – Referenzdokumentation
* [Web Services-Connector](http://go.microsoft.com/fwlink/?LinkID=226245) – Referenzdokumentation
* [PowerShell-Connector](active-directory-aadconnectsync-connector-powershell.md) – Referenzdokumentation
* [Lotus Domino-Connector](active-directory-aadconnectsync-connector-domino.md) – Referenzdokumentation


## <a name="116040-aadconnect-pending-release"></a>1.1.604.0 (AADConnect [ausstehendes Release])


### <a name="fixed-issues"></a>Behobene Probleme:

* Webdienste (generisch):
  * Ein Problem, das die Erstellung eines SOAP-Projekts verhindert, wenn zwei oder mehr Endpunkte vorhanden sind, wurde behoben.
* SQL (generisch):
  * Beim Importvorgang wurde die Zeit von GSQL nicht ordnungsgemäß konvertiert, wenn die Speicherung im Connectorbereich erfolgte. Das Standardformat für Datum und Uhrzeit für den Connectorbereich von GSQL wurde von „JJJJ-MM-TT hh:mm:ssZ“ in „JJJJ-MM-TT HH:mm:ssZ“ geändert.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Behobene Probleme:

* Webdienste (generisch):
  * Das Tool „Wsconfig“ hat das JSON-Array nicht korrekt von „Beispielanforderung“ für die REST-Dienstmethode konvertiert. Dadurch wurden Probleme bei der Serialisierung dieses JSON-Arrays für die REST-Anforderung verursacht.
  * Das Konfigurationstool für Webdienstconnectors unterstützt in Namen von JSON-Attributen keine Bereichssymbole. 
    * Der Datei „WSConfigTool.exe.config“ kann ein Ersetzungsmuster hinzugefügt werden, z.B. ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```.

* Lotus Notes:
  * Wenn die Option zum Zulassen benutzerdefinierter Zertifizierer für Organisationen/Organisationseinheiten**** deaktiviert ist, tritt für den Connector ein Fehler beim Export auf. Update: Nach dem Exportfluss werden alle Attribute in Domino exportiert, zum Zeitpunkt des Exports wird jedoch ein KeyNotFoundException-Fehler an Sync zurückgegeben. 
    * Der Grund: Beim Umbenennungsvorgang tritt ein Fehler auf, wenn versucht wird, DN (UserName-Attribut) durch Ändern eines der folgenden Attribute zu ändern:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  * Wenn die Option zum **Zulassen benutzerdefinierter Zertifizierer für Organisationen/Organisationseinheiten** aktiviert ist, die erforderlichen Zertifizierer aber noch leer sind, tritt ein KeyNotFoundException-Fehler auf.

### <a name="enhancements"></a>Verbesserungen:

* SQL (generisch):
  * **Szenario: überarbeitet implementiert:** „*“-Feature
  * **Lösungsbeschreibung:** geänderter Ansatz für die [Behandlung referenzierter, mehrwertiger Attribute](active-directory-aadconnectsync-connector-genericsql.md)


### <a name="fixed-issues"></a>Behobene Probleme:

* Webdienste (generisch):
  * Fehler beim Importieren der Serverkonfiguration, wenn der WebService-Connector vorhanden ist.
  * Der WebService-Connector funktioniert nicht mit mehreren Webdiensten.

* SQL (generisch):
  * Für referenzierte Einzelwertattribute werden keine Objekttypen aufgeführt.
  * Der Deltaimport bei der Strategie der Änderungsnachverfolgung löscht Objekte, wenn der Wert aus der mehrwertigen Tabelle entfernt wird.
  * OverflowException im GSQL-Connector mit DB2 bei AS/400

Lotus:
  * Eine Option zum Aktivieren/Deaktivieren der Suche von Organisationseinheiten vor dem Öffnen der Seite „GlobalParameters“ wurde hinzugefügt.

## <a name="114430"></a>1.1.443.0

Veröffentlicht: März 2017

### <a name="enhancements"></a>Verbesserungen

* SQL (generisch):</br>
  **Symptome des Szenarios:** Für den SQL-Connector gibt es eine bekannte Einschränkung, bei der nur ein Verweis auf einen Objekttyp zulässig und ein Querverweis mit Membern erforderlich ist. </br>
  **Lösungsbeschreibung:** Im Verarbeitungsschritt für Verweise, bei denen die Option „*“ gewählt wird, werden ALLE Kombinationen von Objekttypen an das Synchronisierungsmodul zurückgegeben.

>[!Important]
- Es werden viele Platzhalter erstellt.
- Es muss sichergestellt werden, dass die Benennung objekttypübergreifend eindeutig ist.


* LDAP (generisch):</br>
 **Szenario:** Wenn in einer bestimmten Partition nur einige Container ausgewählt werden, wird die Suche trotzdem in der gesamten Partition durchgeführt. Die bestimmte Partition wird nach dem Synchronisierungsdienst gefiltert, aber nicht nach MA. Dies kann zu einer Beeinträchtigung der Leistung führen. </br>

 **Lösungsbeschreibung:** Der Code des GLDAP-Connectors wurde geändert, um es zu ermöglichen, dass jeweils alle Container und Suchobjekte durchlaufen werden, anstatt die gesamte Partition zu durchsuchen.


* Lotus Domino:

  **Szenario:** Unterstützung für das Löschen von Domino-E-Mails zum Entfernen einer Person während eines Exportvorgangs. </br>
  **Lösung:** Unterstützung eines konfigurierbaren E-Mail-Löschvorgangs zum Entfernen einer Person während eines Exportvorgangs.

### <a name="fixed-issues"></a>Behobene Probleme:
* Webdienste (generisch):
 * Wenn die Dienst-URL in SAP-wsconfig-Standardprojekten mit dem WebService-Konfigurationstool geändert wird, tritt der folgende Fehler auf: „Ein Teil des Pfads konnte nicht gefunden werden.“

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* LDAP (generisch):
 * Für den GLDAP-Connector sind in AD LDS nicht alle Attribute sichtbar
 * Fehler im Assistenten, wenn für das LDAP-Verzeichnisschema keine UPN-Attribute erkannt werden
 * Deltaimporte schlagen mit Ermittlungsfehlern fehl, die während des vollständigen Imports nicht vorhanden sind, wenn das „objectclass“-Attribut nicht ausgewählt ist
 * Auf der Konfigurationsseite zum „Konfigurieren von Partitionen und Hierarchien“ werden keine Objekte angezeigt, deren Typ der Partition für Novel-Server für  
LDAP MA (generisch) entspricht. Es wurden nur Objekte der RootDSE-Partition angezeigt.


* SQL (generisch):
 * Fehler vom Typ „Wasserzeichen für SQL (generisch): Mehrwertiges Attribut für Deltaimport nicht importiert“ behoben
 * Beim Exportieren von gelöschten/hinzugefügten Werten von mehrwertigen Attributen werden diese in der Datenquelle nicht gelöscht/hinzugefügt.  


* Lotus Notes:
 * Ein spezifisches Feld für den „Vollständigen Namen“ wird im Metaverse richtig angezeigt, aber beim Exportieren nach Notes ist der Wert für das Attribut ein Nullwert oder leer.
 * Fehler aufgrund von doppeltem Zertifizierer behoben
 * Wenn das Objekt ohne Daten im Lotus Domino Connector mit anderen Objekten ausgewählt wird, tritt beim vollständigen Import der Ermittlungsfehler auf.
 * Wenn der Deltaimport auf dem Lotus Domino Connector ausgeführt wird, gibt der Dienst „Microsoft.IdentityManagement.MA.LotusDomino.Service.exe“ am Ende der Ausführung ggf. einen Anwendungsfehler aus.
 * Die Gruppenmitgliedschaft funktioniert im Allgemeinen gut und wird beibehalten. Ausnahme: Beim Ausführen des Exports zum Entfernen eines Benutzers aus der Mitgliedschaft wird der Vorgang für ein Update als erfolgreich angezeigt, aber der Benutzer wird nicht tatsächlich aus der Lotus Notes-Mitgliedschaft entfernt.
 * In der Konfigurations-GUI von Lotus MA wurde die Möglichkeit zum Auswählen von „Append Item at bottom“ (Element unten anfügen) hinzugefügt, damit während des Exports für mehrwertige Attribute unten neue Elemente angefügt werden können.
 * Der Connector fügt die erforderliche Logik zum Löschen der Datei aus dem Mailordner und dem ID-Tresor hinzu.
 * Das Löschen der Mitgliedschaft für übergreifende NAB-Member funktioniert nicht.
 * Werte sollten erfolgreich aus dem mehrwertigen Attribut gelöscht werden

## <a name="111170"></a>1.1.117.0
Veröffentlicht März 2016

**Neuer Connector**  
Erste Version des [Generischer SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md)

**Neue Features:**

* Generischer LDAP-Connector:
  * Zusätzliche Unterstützung für den Deltaimport mit Isode.
* Webdienst-Connector:
  * Die csEntryChangeResult- und setImportErrorCode-Aktivität wurde aktualisiert, um das Zurückgeben von Fehlern der Objektebene an das Synchronisierungsmodul zu ermöglichen.
  * Aktualisierte die „SAP6“ und „SAP6User“ Vorlagen dahingehend, das die neue Objektebenenfehler-Funktionalität verwendet wird.
* Lotus Domino-Connector:
  * Für den Export benötigen Sie einen Zertifizierer pro Adressbuch. Sie können jetzt dasselbe Kennwort für alle Zertifizierer verwenden, um die Verwaltung zu vereinfachen.

**Behobene Probleme:**

* Generischer LDAP-Connector:
  * Für IBM Tivoli DS wurden einige Verweisattribute nicht richtig erkannt.
  * Für Open LDAP wurden während eines Deltaimports Leerzeichen am Anfang und Ende der Zeichenfolge abgeschnitten.
  * Fehler beim Export für Novell und NetIQ bei dem ein Objekt zwischen Organisationseinheiten/Containern verschoben und gleichzeitig umbenannt wurde.
* Webdienst-Connector:
  * Wenn der Webdienst mehrere Endpunkte für dieselbe Bindung hat, hat der Connector diese Endpunkte nicht ordnungsgemäß ermittelt.
* Lotus Domino-Connector:
  * Fehler beim Export des Attributs fullName in eine „Mail-in-Datenbank“.
  * Ein Export, der einer Gruppe Elemente sowohl hinzufügte als auch aus ihr entfernte, exportierte nur die hinzugefügten Elemente.
  * Ist ein Notes-Dokument ungültig (weil FALSE für das Attribut „isValid“ festgelegt ist), kann der Connector keine Verbindung herstellen.

## <a name="older-releases"></a>Ältere Versionen
Vor März 2016 wurden die Connectors als Support-Themen veröffentlicht.

**Generisches LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, September 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015 März
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, Januar 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, September 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014 März

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, September 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, September 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, September 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015 März
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, August 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, Februar 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, Oktober 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, August 2013

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

