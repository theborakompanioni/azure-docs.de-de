---
title: "Microsoft Threat Modeling Tool – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über alle Funktionen im Threat Modeling Tool"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 621ff305d7e782f85eeaae6c3fb02031673549c6
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="threat-modeling-tool-feature-overview"></a>Übersicht über die Funktionen des Threat Modeling Tools

Wir freuen uns, dass Sie das Threat Modeling Tool für Ihre Anforderungen an die Bedrohungsmodellierung verwenden möchten. Falls noch nicht geschehen, lesen Sie **[Erste Schritte mit dem Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md)**, um sich mit den Grundlagen vertraut zu machen.

> Unser Tool wird häufig aktualisiert, sodass Sie diesen Leitfaden häufig auf unsere neuesten Funktionen und Verbesserungen überprüfen sollten.

Nach Klicken auf die Schaltfläche „Neues Modell erstellen“ wird eine leere Startseite ähnlich wie in der nachstehenden Abbildung geöffnet:

![Leere Startseite](./media/azure-security-threat-modeling-tool/tmtstart.png)

Lassen Sie uns anhand des von unserem Team im Artikel **[Erste Schritte](./azure-security-threat-modeling-tool-getting-started.md)** erstellten Bedrohungsmodells alle Funktionen durchgehen, die das Tool derzeit zu bieten hat.

![Grundlegendes Bedrohungsmodell](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Navigation

Vor dem Einstieg in die integrierten Funktionen wollen wir auf die wichtigsten Komponenten des Tools eingehen.

### <a name="menu-items"></a>Menüelemente

Die Benutzeroberfläche sollte ähnlich wie bei anderen Microsoft-Produkten sein. Zuerst wollen wir uns mit den Menüelementen auf oberster Ebene beschäftigen:

![Menüelemente](./media/azure-security-threat-modeling-tool/menuitems.png)

| Bezeichnung                               | Details      |
| --------------------------------------- | ------------ |
| **Datei** | <ul><li>Dateien öffnen, speichern und schließen</li><li>Bei OneDrive-Konten an-/abmelden</li><li>Links teilen (Anzeigen und Bearbeiten)</li><li>Dateiinformationen anzeigen</li><li>Neue Vorlage auf vorhandene Modelle anwenden</li></ul> |
| **Bearbeiten** | Aktionen rückgängig machen/wiederholen sowie Kopieren, Einfügen und Löschen |
| **Ansicht** | <ul><li>Zwischen den Ansichten **Analyse** und **Entwurf** wechseln</li><li>Geschlossene Fenster öffnen (z.B. Schablonen, Elementeigenschaften und Meldungen)</li><li>Layout auf die Standardeinstellungen zurücksetzen</li></ul> |
| **Diagramm** | Diagramme hinzufügen/löschen und durch die „Registerkarten“ von Diagrammen navigieren |
| **Berichte** | HTML-Berichte zum Teilen mit anderen erstellen |
| **Hilfe** | Für die Nutzung des Tools hilfreiche Anleitungen |

Die Symbole sind Verknüpfungen für die Menüs der obersten Ebene:

| Symbol                               | Details      |
| --------------------------------------- | ------------ |
| **Öffnen** | Öffnet eine neue Datei |
| **Speichern** | Speichert die aktuelle Datei |
| **Entwurf** | Dient zum Wechseln in die Entwurfsansicht, in der Sie Modelle erstellen können |
| **Analysieren** | Zeigt generierte Bedrohungen und deren Eigenschaften |
| **Diagramm hinzufügen** | Fügt ein neues Diagramm hinzu (vergleichbar mit neuen Registerkarten in Excel) |
| **Diagramm löschen** | Löscht das aktuelle Diagramm |
| **Kopieren/Ausschneiden/Einfügen** | Dient zum Kopieren/Ausschneiden/Einfügen von Elementen |
| **Rückgängig/Wiederholen** | Dient zum Rückgängigmachen/Wiederholen von Aktionen |
| **Vergrößern/Verkleinern** | Dient zum Vergrößern und Verkleinern des Diagramms für eine bessere Ansicht |
| **Feedback** | Öffnet das MSDN-Forum |

### <a name="canvas"></a>Zeichenbereich

Der Bereich, in dem Sie gezogene Elemente ablegen. Ziehen und Ablegen ist der schnellste und effizienteste Weg, Modelle zu erstellen. Sie können auch mit der rechten Maustaste klicken und im Menü eine Option wählen, wodurch generische Versionen der Elemente hinzugefügt werden, die Sie verwenden (siehe unten).

#### <a name="dropping-the-stencil-on-the-canvas"></a>Ablegen der Schablone im Zeichenbereich

![Ablegen im Zeichenbereich](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-the-stencil"></a>Klicken auf die Schablone

![Elementeigenschaften](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Schablonen

Die Ihnen zur Verfügung stehenden Schablonen hängen von der ausgewählten Vorlage ab. Wenn Sie die richtigen Elemente nicht finden können, versuchen Sie es mit einer anderen Vorlage, oder ändern Sie eine Ihren Anforderungen entsprechend. Im Allgemeinen sollten Sie eine Kombination von Kategorien wie die folgenden finden können:

| Schablonennamen                               | Details      |
| --------------------------------------- | ------------ |
| **Prozess** | Anwendungen, Browser-Plug-Ins, Threads, virtuelle Computer |
| **Externe Einheit** | Authentifizierungsanbieter, Browser, Benutzer, Webanwendungen |
| **Datenspeicher** | Cache, Speicher, Konfigurationsdateien, Datenbanken, Registrierung |
| **Datenfluss** | Binärdatei, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP |
| **Vertrauensstellungsgrenze** | Unternehmensnetzwerke, Internet, Computer, Sandkasten, Benutzer-/Kernelmodus |

### <a name="notesmessages"></a>Notizen/Meldungen

| Komponente                               | Details      |
| --------------------------------------- | ------------ |
| **Meldungen** | Interne Toollogik zum Benachrichtigen von Benutzern, wenn ein Fehler vorliegt, wie z.B. kein Datenfluss zwischen Elementen |
| **Notizen** | Von Entwicklerteams im gesamten Entwurfs- und Überprüfungsprozess hinzugefügte manuelle Notizen |

### <a name="element-properties"></a>Elementeigenschaften

Diese unterscheiden sich je nach ausgewähltem Element. Abgesehen von Vertrauensstellungsgrenzen enthalten alle anderen Elemente drei allgemeine Optionen:

| Elementeigenschaft                               | Details      |
| --------------------------------------- | ------------ |
| **Name** | Nützlich zum Benennen Ihrer Prozesse, Speicher, Einheiten und Flüsse, damit diese einfach erkannt werden |
| **Außerhalb des gültigen Bereichs** | Falls ausgewählt, wird das Element aus der Bedrohungsgenerierungsmatrix entfernt (nicht empfohlen) |
| **Grund für „Außerhalb des gültigen Bereichs“** | Feld zur Begründung, um Benutzer wissen zu lassen, warum diese Option gewählt wurde |

Eigenschaften werden in jeder Elementkategorie geändert. Klicken Sie auf die einzelnen Elemente, um die verfügbaren Optionen zu untersuchen, oder öffnen Sie die Vorlage, um mehr zu erfahren. Sehen wir uns nun die Funktionen an.

## <a name="welcome-screen"></a>Begrüßungsbildschirm

Der Begrüßungsbildschirm ist das Erste, was Sie sehen, wenn Sie die App öffnen.

### <a name="open-a-model"></a>Öffnen eines Modells

Wenn Sie den Mauszeiger über der Schaltfläche „Modell öffnen“ bewegen, werden zwei versteckte Optionen angezeigt: „Auf diesem Computer öffnen“ und „Auf OneDrive öffnen“. Über die erste wird das Dialogfeld „Datei öffnen“ geöffnet, während Sie bei Wahl der zweiten durch den Anmeldevorgang für OneDrive geleitet werden. Nach erfolgreicher Authentifizierung können Sie Ordner und Dateien auswählen.

![Modell öffnen](./media/azure-security-threat-modeling-tool/openmodel.png)

![Auf Computer oder OneDrive öffnen](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Feedback, Vorschläge und Probleme

Bei Wahl dieser Option gelangen Sie zu den MSDN-Foren für SDL-Tools. Sie können erfahren, was andere Benutzer zum Tool zu sagen haben, sowie Problemumgehungen und neue Ideen finden.

![Feedback](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Entwurfsansicht

Bei jedem Öffnen oder Erstellen eines neuen Modells gelangen Sie zur Entwurfsansicht.

### <a name="adding-elements"></a>Hinzufügen von Elementen

Es gibt zwei Möglichkeiten zum Hinzufügen von Elementen zum Raster:

- **Ziehen und Ablegen**: Ziehen Sie das gewünschte Element in das Raster. Nutzen Sie dann die Elementeigenschaften, um zusätzliche Informationen bereitzustellen.
- **Klicken mit der rechten Maustaste**: Klicken Sie mit der rechten Maustaste auf das Raster, und wählen Sie eine Option im Kontextmenü. Eine allgemeine Darstellung des jeweiligen Elements wird auf dem Bildschirm angezeigt.

### <a name="connecting-elements"></a>Verbinden von Elementen

Es gibt zwei Möglichkeiten zum Verbinden von Elementen im Tool:

- **Ziehen und Ablegen**: Ziehen Sie den gewünschten Datenfluss in das Raster, und verbinden Sie beide Enden mit den entsprechenden Elementen.
- **Klicken bei gedrückter UMSCHALTTASTE**: Klicken Sie auf das erste Element (Daten werden gesendet), halten Sie die UMSCHALTTASTE gedrückt, und wählen Sie dann das zweite Element aus (Daten werden empfangen). Klicken Sie mit der rechten Maustaste, und wählen Sie „Verbinden“. Wenn Sie einen bidirektionalen Datenfluss verwenden, ist die Reihenfolge nicht so wichtig.

### <a name="properties"></a>Eigenschaften

Zeigt alle Eigenschaften, die in den auf dem Diagramm platzierten Schablonen geändert werden können. Um die Eigenschaften anzuzeigen, klicken Sie einfach auf die Schablone. Die Informationen werden entsprechend aufgefüllt. Das folgende Beispiel zeigt den Status vor und nach dem Ziehen der Schablone „Datenbank“ auf das Diagramm:

#### <a name="before"></a>Vorher

![Vorher](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Nachher

![Nachher](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>Meldungen

Wenn Sie ein Bedrohungsmodell erstellen und vergessen, Datenflüsse mit Elementen zu verbinden, werden Sie im Benachrichtigungsfenster zum Agieren aufgefordert. Sie können dies ignorieren oder die Anweisungen zum Beheben des Problems befolgen. 

![Meldungen](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Notizen

Durch Wechseln von der Registerkarte „Meldungen“ zu „Notizen“ können Sie Ihrem Diagramm Notizen hinzufügen, um Ihre Gedanken zu sammeln.

## <a name="analysis-view"></a>Analyseansicht

Sobald Sie mit dem Erstellen Ihres Diagramms fertig sind, wechseln Sie zur Analyseansicht, indem Sie zu den Optionen im obersten Menü navigieren und das Lupensymbol neben der Farbpalette auswählen.

![Analyseansicht](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Generierte Bedrohungsauswahl

Wenn Sie auf eine Bedrohung klicken, können Sie drei besondere Funktionen nutzen:

| Funktion                               | Information      |
| --------------------------------------- | ------------ |
| **Markierung „Gelesen“** | <p>Die Bedrohung ist nun als „Gelesen“ markiert, wodurch Sie einfacher die Elemente nachverfolgen können, mit denen Sie sich bereits beschäftigt haben.</p><p>![Markierung „Gelesen/Nicht gelesen“](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Interaktionsfokus** | <p>Die zu dieser Bedrohung gehörende Interaktion wird hervorgehoben.</p><p>![Interaktionsfokus](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **Bedrohungseigenschaften** | <p>Weitere Informationen zur Bedrohung werden im Fenster mit den „Bedrohungseigenschaften“ hinzugefügt.</p><p>![Bedrohungseigenschaften](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Ändern der Priorität

Bei Änderung der Prioritätsstufe jeder generierten Bedrohung wird auch ihre Farbe geändert, um Bedrohungen mit hoher, mittlerer oder niedriger Priorität einfacher bestimmen zu können.

![Ändern der Priorität](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Editierbare Felder von Bedrohungseigenschaften

Wie Sie in der obigen Abbildung sehen können, können Benutzer die vom Tool generierten Informationen ändern und bestimmten Feldern auch Informationen hinzufügen, z.B. eine Begründung. Diese Felder werden von der Vorlage generiert. Wenn Sie weitere Informationen zu Bedrohungen benötigen, müssen Sie Änderungen vornehmen.

![Bedrohungseigenschaften](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Berichte

Sobald Sie mit dem Ändern von Prioritäten und Aktualisieren des Status der einzelnen generierten Bedrohungen fertig sind, können Sie die Datei speichern und/oder einen Bericht ausgeben, indem Sie zu „Bericht“ navigieren und dann „Vollständigen Bericht erstellen“ auswählen. Sie werden aufgefordert, den Bericht zu benennen. Anschließend sehen Sie einen Bildschirm, der mit der folgenden Abbildung vergleichbar ist:

![Bericht](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie eine Vorlage zur Community beitragen können, finden Sie auf unserer **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**-Seite. **[Laden Sie das Tool heute noch herunter](https://aka.ms/tmtpreview)**, um die ersten Schritte zu unternehmen.

