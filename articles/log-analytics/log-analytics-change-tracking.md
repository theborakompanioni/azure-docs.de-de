---
title: "Nachverfolgen von Änderungen mit Azure Log Analytics | Microsoft Docs"
description: "Mit der Change Tracking-Lösung in Log Analytics können Sie Änderungen an Software und Windows-Diensten in Ihrer Umgebung ganz einfach identifizieren."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 7e0fa9a83c3c83145a4813422bf73a0e711d0ecc
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Nachverfolgen von Änderungen an der Software in Ihrer Umgebung mit der Change Tracking-Lösung

![Symbol „Änderungen nachverfolgen“](./media/log-analytics-change-tracking/change-tracking-symbol.png)

Dieser Artikel unterstützt Sie bei der einfachen Erkennung von Änderungen an Ihrer Umgebung mithilfe der Änderungsnachverfolgungslösung in Log Analytics. Die Lösung verfolgt Änderungen an Windows- und Linux-Software, an Windows-Dateien und Registrierungsschlüsseln, an Windows-Diensten und an Linux-Daemons nach. Durch Ermitteln von Konfigurationsänderungen können Sie Betriebsprobleme präzise bestimmen.

Sie installieren die Lösung, um den Typ des installierten Agents zu aktualisieren. Änderungen an installierter Software, Windows-Diensten und Linux-Daemons auf den überwachten Servern werden gelesen, und di Daten werden Verarbeitung an den Log Analytics-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Mithilfe der Informationen im Change Tracking-Dashboard können Sie ganz leicht die Änderungen erkennen, die in Ihrer Serverinfrastruktur vorgenommen wurden.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Sie benötigen einen [Windows](log-analytics-windows-agents.md)-, [Operations Manager](log-analytics-om-agents.md)- oder [Linux](log-analytics-linux-agents.md)-Agent auf jedem Computer, auf dem Änderungen überwacht werden sollen.
* Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Lösung für die Änderungsnachverfolgung hinzu.  Es ist keine weitere Konfiguration erforderlich.

### <a name="configure-linux-files-to-track"></a>Konfigurieren von Linux-Dateien für die Nachverfolgung
Führen Sie zum Konfigurieren der nachzuverfolgenden Dateien auf Linux-Computern die folgenden Schritte aus.

1. Klicken Sie im OMS-Portal auf **Einstellungen** (das Zahnradsymbol).
2. Klicken Sie auf der Seite **Einstellungen** auf **Daten** und dann auf **Nachverfolgung von Linux-Dateien**.
3. Geben Sie unter „Nachverfolgung von Linux-Dateien“ den vollständigen Pfad ein, und zwar einschließlich des Dateinamens der Datei, die Sie nachverfolgen möchten. Klicken Sie dann auf das Symbol **Hinzufügen**. Beispiel: „/etc/*.conf“
4. Klicken Sie auf **Speichern**.  
  
> [!NOTE]
> Beim Nachverfolgen von Linux-Dateien stehen zusätzliche Funktionen zur Verfügung, darunter das Nachverfolgen von Verzeichnissen, das rekursive durchlaufen von Verzeichnissen und das Nachverfolgen von Platzhaltern.

### <a name="configure-windows-files-to-track"></a>Konfigurieren der nachzuverfolgenden Windows-Dateien
Führen Sie zum Konfigurieren der nachzuverfolgenden Dateien auf Windows-Computern die folgenden Schritte aus.

1. Klicken Sie im OMS-Portal auf **Einstellungen** (das Zahnradsymbol).
2. Klicken Sie auf der Seite **Einstellungen** auf **Daten** und dann auf **Nachverfolgung von Windows-Dateien**.
3. Geben Sie unter „Nachverfolgung von Windows-Dateien“ den vollständigen Pfad ein, und zwar einschließlich des Dateinamens der Datei, die Sie nachverfolgen möchten. Klicken Sie dann auf das Symbol **Hinzufügen**. Beispiel: „C:\Program Files (x86)\Internet Explorer\iexplore.exe“ oder „C:\Windows\System32\drivers\etc\hosts“.
4. Klicken Sie auf **Speichern**.  
   ![Änderungsnachverfolgung von Windows-Dateien](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-to-track"></a>Konfigurieren der nachzuverfolgenden Windows-Registrierungsschlüssel
Führen Sie zum Konfigurieren der nachzuverfolgenden Registrierungsschlüssel auf Windows-Computern die folgenden Schritte aus.

1. Klicken Sie im OMS-Portal auf **Einstellungen** (das Zahnradsymbol).
2. Klicken Sie auf der Seite **Einstellungen** auf **Daten** und dann auf **Nachverfolgung für Windows-Registrierung**.
3. Geben Sie unter „Änderungsverfolgung für Windows-Registrierung“ den vollständigen Schlüssel ein, den Sie nachverfolgen möchten. Klicken Sie dann auf das Symbol **Hinzufügen**.
4. Klicken Sie auf **Speichern**.  
   ![Änderungsverfolgung für Windows-Registrierung](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Erläuterung der Eigenschaften der Linux-Dateisammlung
1. **Typ**
   * **Datei** (Metadaten der Berichtsdatei: Größe, Änderungsdatum, Hashwert usw.)
   * **Verzeichnis** (Metadaten des Berichtsverzeichnisses: Größe, Änderungsdatum usw.)
2. **Links** (Behandlung von Symlink-Verweisen von Linux auf andere Dateien oder Verzeichnisse)
   * **Ignorieren** (Ignorieren der Symlinks während Rekursionen, um die Verweisdateien/-verzeichnisse nicht einzuschließen)
   * **Folgen** (Während der Rekursion den Symlinks folgen, um auch die Verweisdateien/-verzeichnisse einzuschließen)
   * **Verwalten** (Den Symlinks folgen und die Verarbeitung des zurückgegebenen Inhalts verändern) 
   
   > [!NOTE]   
   > Die Option „Verwalten“ von Links wird nicht empfohlen, da der Abruf des Dateiinhalts derzeit nicht unterstützt wird.
   
3. **Rekursiv durchlaufen** (Die Dateiebenen rekursiv durchlaufen und alle Dateien nachverfolgen, die die Pfadanweisung erfüllen)
4. **Sudo** (Den Zugriff auf Dateien oder Verzeichnisse aktivieren, für die eine Sudo-Berechtigung benötigt wird)

### <a name="limitations"></a>Einschränkungen
Die Änderungsnachverfolgungslösung unterstützt derzeit Folgendes nicht:

* Ordner (Verzeichnisse) für Windows File Tracking
* Rekursion für Windows File Tracking
* Platzhalter für Windows File Tracking
* Pfadvariablen
* Netzwerkdateisysteme
* Inhalt der Datei

Weitere Einschränkungen:

* Die Spalte **Maximale Dateigröße** und ihre Werte werden in der aktuellen Implementierung nicht genutzt.
* Wenn Sie im 30-minütigen Sammlungszyklus mehr als 2.500 Dateien erfassen, wird die Leistung der Lösung möglicherweise beeinträchtigt.
* Wenn der Netzwerkverkehr hoch ist, kann es bis zu sechs Stunden dauern, bis Änderungsdatensätze angezeigt werden.
* Wenn Sie die Konfiguration ändern, während ein Computer heruntergefahren ist, kann der Computer Dateiänderungen bereitstellen, die zur vorherigen Konfiguration gehören.

## <a name="change-tracking-data-collection-details"></a>Details zur Datensammlung für die Änderungsnachverfolgung
Die Änderungsnachverfolgung sammelt Softwareinventur- und Windows-Dienst-Metadaten mit den Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für die Änderungsnachverfolgung erfasst werden.

| Plattform | Direkt-Agent | SCOM-Agent | Linux-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows und Linux |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) | Zwischen 5 und 50 Minuten (je nach Art der Änderung) Weitere Informationen finden Sie unten. |


Die folgende Tabelle zeigt die Datensammlungshäufigkeit für die Änderungstypen.

| **Änderungstyp** | **frequency** | **Sendet der** **Agent** **gefundene Änderungen?** |
| --- | --- | --- |
| Windows-Registrierung | 50 Minuten | Nein |
| Windows-Datei | 30 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |
| Linux-Datei | 15 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |
| Windows-Dienste | 30 Minuten | Ja, alle 30 Minuten, wenn Änderungen gefunden werden. Alle 24 Stunden wird unabhängig von Änderungen eine Momentaufnahme gesendet. Die Momentaufnahme wird also auch gesendet, wenn es keine Änderungen gibt. |
| Linux-Daemons | 5 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |
| Windows-Software | 30 Minuten | Ja, alle 30 Minuten, wenn Änderungen gefunden werden. Alle 24 Stunden wird unabhängig von Änderungen eine Momentaufnahme gesendet. Die Momentaufnahme wird also auch gesendet, wenn es keine Änderungen gibt. |
| Linux-Software | 5 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |

### <a name="registry-key-change-tracking"></a>Registrierungsschlüssel-Änderungsnachverfolgung

Log Analytics führt Überwachung und Nachverfolgung der Windows-Registrierung mit der Change Tracking-Lösung durch. Die Überwachung von Änderungen der Registrierungsschlüssel dient dem Ermitteln von Erweiterungspunkten, an denen Code von Drittanbietern und Schadsoftware aktiv werden können. Die folgende Liste zeigt die Standardregistrierungsschlüssel, die von der Lösung nachverfolgt werden, und die jeweiligen Gründe für die Nachverfolgung.

- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Überwacht Skripts, die beim Start ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Überwacht Skripts, die beim Herunterfahren ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Überwacht für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden, Schlüssel, die geladen werden, bevor der Benutzer sich bei seinem Windows-Konto anmeldet.
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components
    - Überwacht Änderungen an Anwendungseinstellungen.
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit „Explorer.exe“ ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit „Explorer.exe“ ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit „Explorer.exe“ ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Überwacht die Symboloverlayhandler-Registrierung.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Überwacht die Symboloverlayhandler-Registrierung für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects
    - Überwacht, ob neue Browserhilfsobjekt-Plug-Ins für Internet Explorer vorliegen, die dann für den Zugriff auf das Dokumentobjektmodell (DOM) der aktuellen Seite und zum Steuern der Navigation verwendet werden können.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects
    - Überwacht, ob neue Browserhilfsobjekt-Plug-Ins für Internet Explorer vorliegen, die dann für den Zugriff auf das Dokumentobjektmodell (DOM) der aktuellen Seite und zum Steuern der Navigation für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden, verwendet werden können.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Überwacht, ob neue Internet Explorer-Erweiterungen vorliegen, z.B. benutzerdefinierte Toolmenüs und benutzerdefinierte Symbolleistenschaltflächen.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Überwacht, ob neue Internet Explorer-Erweiterungen vorliegen, z.B. benutzerdefinierte Toolmenüs und benutzerdefinierte Symbolleisten-Schaltflächen für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Überwacht die mit wavemapper zugeordneten 32-Bit-Treiber, wave1 und wave2, msacm.imaadpcm, .msadpcm, .msgsm610 und vidc. Ähnlich dem Abschnitt „[drivers]“ in der SYSTEM. INI-Datei.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Überwacht die mit wavemapper zugeordneten 32-Bit-Treiber, wave1 und wave2, msacm.imaadpcm, .msadpcm, .msgsm610 und vidc für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden. Ähnlich dem Abschnitt „[drivers]“ in der SYSTEM. INI-Datei.
- HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Überwacht die Liste der bekannten oder häufig verwendete System-DLLs. Dieses System verhindert, dass schwache Anwendungsverzeichnisberechtigungen durch Infiltration mit Trojanerversionen von System-DLLs ausgenutzt werden.
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Überwacht die Liste der Pakete, die Ereignisbenachrichtigungen von Winlogon empfangen können, dem interaktiven Anmeldungsunterstützungsmodell für das Windows-Betriebssystem.


## <a name="use-change-tracking"></a>Verwenden von Change Tracking
Nach der Installation können Sie die Zusammenfassung der Änderungen für die überwachten Server in OMS über die Kachel **Änderungsnachverfolgung** auf der Seite **Übersicht** anzeigen.

![Abbildung der Kachel "Change Tracking"](./media/log-analytics-change-tracking/change-tracking-tile.png)

Sie können Änderungen an Ihrer Infrastruktur anzeigen und dann Einzelheiten für die folgenden Kategorien abrufen:

* Änderungen nach Konfigurationstyp für Software und Windows-Dienste
* Softwareänderungen an Anwendungen und Updates für einzelne Server
* Gesamtanzahl von Softwareänderungen für jede Anwendungen
* Linux-Pakete
* Änderungen am Windows-Dienst für einzelne Server
* Änderungen an Linux-Daemons

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>So zeigen Sie Änderungen für Änderungstypen an
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Änderungsnachverfolgung**.
2. Prüfen Sie im Dashboard **Änderungsnachverfolgung** die Zusammenfassungsinformationen in einem der Blätter mit den Änderungstypen, und klicken Sie dann auf einen Eintrag, um detaillierte Informationen auf der Seite **Protokollsuche** anzuzeigen.
3. Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md) , um ausführliche Daten zur Änderungsnachverfolgung anzuzeigen.

