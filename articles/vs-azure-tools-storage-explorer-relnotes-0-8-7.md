---
title: Microsoft Azure-Speicher-Explorer 0.8.7 (Vorschau) | Microsoft-Dokumentation
description: "Versionshinweise für Microsoft Azure-Speicher-Explorer 0.8.7 (Vorschau)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/18/2017
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: bf47274a97342253dfd4ebfa9f1029a7c18b81eb
ms.openlocfilehash: fc3620ca19d4824bc8ffb3bbe89ef47c5127b9d1


---
# <a name="microsoft-azure-storage-explorer-087-preview"></a>Microsoft Azure-Speicher-Explorer 0.8.7 (Vorschau)
## <a name="overview"></a>Übersicht
Dieser Artikel enthält die Versionshinweise für die Vorschauversion von Azure-Speicher-Explorer 0.8.7.

Bei der [Vorschauversion des Microsoft Azure-Speicher-Explorers](./vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine eigenständige App, über die Sie unter Windows, MacOS und Linux komfortabel mit Azure Storage-Daten arbeiten können.

## <a name="azure-storage-explorer-087-preview"></a>Azure-Speicher-Explorer 0.8.7 (Vorschau)
### <a name="download-azure-storage-explorer-087-preview"></a>Herunterladen des Azure-Speicher-Explorers 0.8.7 (Vorschau)
- [Azure-Speicher-Explorer 0.8.7 (Vorschau) für Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [ Azure-Speicher-Explorer 0.8.7 (Vorschau) für Macintosh](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure-Speicher-Explorer 0.8.7 (Vorschau) für Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new-updates"></a>Neue Updates
* Sie können im Fenster **Aktivitäten** wählen, wie Sie Konflikte zu Beginn einer Update-, Download- oder Kopiersitzung lösen.
* Bewegen Sie den Mauszeiger auf eine Registerkarte, um den vollständigen Pfad der Speicherressource anzuzeigen.
* Wenn Sie auf eine Registerkarte klicken, wird sie mit dem Speicherort im Navigationsbereich auf der linken Seite synchronisiert.

### <a name="fixes"></a>Fehlerbehebungen
* Behoben: Speicher-Explorer ist unter MacOS jetzt eine vertrauenswürdige App.
* Behoben: Ubuntu 14.04 wird wieder unterstützt.
* Behoben: Die Option „Konto hinzufügen“ blinkte manchmal beim Laden von Abonnements.
* Behoben: Es kam vor, dass im Navigationsbereich auf der linken Seite nicht alle Speicherressourcen aufgeführt wurden.
* Behoben: Im Aktionsbereich wurden manchmal leere Aktionen angezeigt.
* Behoben: Die Fenstergröße von der letzten geschlossenen Sitzung wird jetzt beibehalten.
* Behoben: Sie können mit dem Kontextmenü mehrere Registerkarten für dieselbe Ressource öffnen.

### <a name="known-issues"></a>Bekannte Probleme
* Der Schnellzugriff funktioniert nur mit abonnementbasierten Elementen. Lokale Ressourcen oder Ressourcen, die per Schlüssel oder SAS-Token angefügt werden, werden in dieser Version nicht unterstützt.
* Beim Schnellzugriff kann das Navigieren zur Zielressource einige Sekunden dauern. Dies hängt davon ab, wie viele Ressourcen Sie verwenden.
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, kann dies zu Fehlern führen.
* Bei der Suche wird auf ca. 50.000 Knoten gesucht. Anschließend kann es zu einer Leistungsbeeinträchtigung oder zu Ausnahmefehlern kommen.
* Wenn Sie den Speicher-Explorer unter MacOS zum ersten Mal verwenden, werden unter Umständen mehrere Eingabeaufforderungen angezeigt, in denen die Berechtigung des Benutzers zum Zugreifen auf den Schlüsselbund abgefragt wird. Wir raten Ihnen, die Option **Immer zulassen** zu wählen, damit die Eingabeaufforderung nicht mehr angezeigt wird.

## <a name="previous-releases"></a>Vorgängerversionen
### <a name="features"></a>Features
#### <a name="main-features"></a>Wichtigste Features
* MacOS-, Linux- und Windows-Versionen
* Melden Sie sich an, um Ihre Speicherkonten gruppiert nach Abonnement anzuzeigen:
    * Verwenden Sie Ihr Organisationskonto, Microsoft-Konto, 2FA usw.
    * Konfigurieren und Verwalten von Proxyeinstellungen
    * Entfernen von Konten per Abmeldung
* Verbindungsherstellung mit Speicherkonten per:
    * Kontoname und -schlüssel
    * Benutzerdefinierte Endpunkte (einschließlich Azure China)
    * SAS-URI für Speicherkonten
* Unterstützung von Azure Resource Manager und klassischem Speicher
* Generieren von SAS-Schlüsseln für Blobs, Blobcontainer, Warteschlangen, Tabellen oder Dateifreigaben
* Herstellen einer Verbindung mit Blobcontainern, Warteschlangen, Tabellen oder Dateifreigaben per SAS-Schlüssel (Shared Access Signatures)
* Verwalten von gespeicherten Zugriffsrichtlinien für Blobcontainer, Warteschlangen, Tabellen oder Dateifreigaben
* Lokaler Entwicklungsspeicher mit Speicheremulator (nur Windows)
* Erstellen und Löschen von Blobcontainern, Warteschlangen oder Tabellen
* Anzeigen von $logs-Blobcontainern und $metrics-Tabellen
* Suchen nach bestimmten Blobs, Warteschlangen, Tabellen oder Dateifreigaben
* Direkte Verknüpfungen mit Speicherkonten oder Containern, Warteschlangen, Tabellen oder Dateifreigaben zum Freigeben Ihrer Ressourcen und für den einfachen Zugriff darauf
* Umbenennen von Blobcontainern, Tabellen, Dateifreigaben
* Möglichkeit zum Verwalten und Konfigurieren von CORS-Regeln
* Einfaches Kopieren von Primär- und Sekundärschlüsseln für Speicherkonten
* MD5-Prüfungen bei Upload und Download auf Datenintegrität und Konsistenz
* Suchen nach Blobcontainern, Tabellen, Warteschlangen, Dateifreigaben oder Speicherkonten über das Suchfeld
* Sie können die am häufigsten verwendeten Dienste jetzt für den Schnellzugriff anheften, um die Navigation zu vereinfachen.
* Sie können jetzt mehrere Editoren auf unterschiedlichen Registerkarten öffnen. Einmal klicken, um eine temporäre Registerkarte zu öffnen, und doppelklicken, um eine dauerhafte Registerkarte zu öffnen. Sie können auch auf die temporäre Registerkarte klicken, um sie zu einer dauerhaften Registerkarte zu machen.
* Merkliche Verbesserungen der Leistung und Stabilität für Uploads und Downloads, vor allem für große Dateien auf schnellen Computern
* Wir führen die erweiterte bereichsbezogene Suche wieder ein und haben das Konzept der Bereichsdefinition hinzugefügt. Geben Sie den Pfad zu einem Knoten ein, indem Sie das Symbol zum Daraufzeigen verwenden, mit der rechten Maustaste auf „Search From Here“ (Ab hier suchen) klicken oder manuell vorgehen, um den Bereich für den Knoten festzulegen. Nach dem Festlegen des Bereichs können Sie am Ende des Pfads einen Suchbegriff hinzufügen, um von diesem Knoten aus eine tiefe Suche zu starten.
* Wir haben verschiedene Designs hinzugefügt: Hell (Standard), Dunkel, Hoher Kontrast (Schwarz) und Hoher Kontrast (Weiß).
* Navigieren Sie zu „Bearbeiten“ > „Designs“, um die Designeinstellung zu ändern.
* Unter Linux ist jetzt ein 64-Bit-Betriebssystem erforderlich.
* Wir haben unser Logo aktualisiert!
#### <a name="blobs"></a>Blobs (in englischer Sprache)
* Anzeigen von Blobs und Navigieren durch Verzeichnisse
* Hochladen, Herunterladen, Löschen und Kopieren von Blobs und Ordnern
* Öffnen und Anzeigen der Inhaltstexte und Bildblobs
* Anzeigen von Blobeigenschaften und Metadaten
* Suchen nach Blobs anhand des Präfix
* Erstellen und Aufheben von Leases für Blobs und Blobcontainer
* Hochladen von Dateien per Drag & Drop
* Umbenennen von Blobs und Ordnern
* Erstellen von leeren „virtuellen“ Ordnern in Blobcontainern ist jetzt möglich
* Sie können die Blob- und Dateieigenschaften ändern
#### <a name="tables"></a>Tabellen
* Anzeigen und Abfragen von Entitäten mit ODATA oder Verwenden des Abfrage-Generators zum Erstellen von komplexen Abfragen
* Hinzufügen, Bearbeiten, Löschen von Entitäten
* Importieren und Exportieren von Tabelleninhalten im CSV-Format (einschließlich des Exportierens von Abfrageergebnissen)
* Anpassen der Spaltenreihenfolge
* Möglichkeit zum Speichern von Abfragen
#### <a name="queues"></a>Warteschlangen
* Anzeigen einer Vorschau für die letzten 32 Nachrichten
* Hinzufügen, Entfernen aus der Warteschlange und Anzeigen von Nachrichten
* Löschen der Warteschlange
* Sie können jetzt entscheiden, ob Sie Ihre Warteschlangennachrichten codieren/decodieren möchten.
#### <a name="file-shares"></a>Dateifreigaben
* Anzeigen von Dateien und Navigieren durch Verzeichnisse
* Hochladen, Herunterladen, Löschen und Kopieren von Dateien und Verzeichnissen
* Anzeigen von Dateieigenschaften
* Umbenennen von Dateien und Verzeichnissen

### <a name="bug-fixes"></a>Fehlerbehebungen
* Behoben: Probleme mit dem Einfrieren des Bildschirms
* Behoben: Erweiterte Sicherheit

### <a name="known-issues"></a>Bekannte Probleme
* Bei der Suche wird auf ca. 50.000 Knoten gesucht. Anschließend kann es zu einer Leistungsbeeinträchtigung kommen, und für die MacOS-Installation können erhöhte Rechte erforderlich sein.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Dateien werden von Azure Stack derzeit nicht unterstützt, und das versuchte Erweitern des Knoten **Dateien** führt zu einem Fehler.
* Für die Installation von Linux 14.04 muss für die gcc-Version ein Update oder ein Upgrade durchgeführt werden. Die folgenden Schritte veranschaulichen, wie Sie ein Upgrade durchführen:

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

### <a name="previous-versions"></a>Vorherige Versionen
#### <a name="october-2016-release-version-085"></a>Version vom Oktober 2016 (Version 0.8.5)
* [Herunterladen für Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Herunterladen für Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Herunterladen für Linux](https://go.microsoft.com/fwlink/?LinkId=809308)



<!--HONumber=Jan17_HO3-->


