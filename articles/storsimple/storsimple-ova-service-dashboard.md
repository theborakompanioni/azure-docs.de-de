---
title: "Dashboard des StorSimple Manager-Diensts – Virtual Array | Microsoft Docs"
description: "Beschreibt das Dashboard des StorSimple Manager-Diensts und erläutert, wie Sie den Zustand Ihres StorSimple Virtual Array über dieses Dashboard verwalten."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 36ac6eb4-a616-4bb1-8163-6862ac33da63
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 58aa9508e28a00efe69d0a9cbc8adb6837d085c4


---
# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Verwenden des Dashboards des StorSimple Manager-Diensts für das StorSimple Virtual Array
## <a name="overview"></a>Übersicht
Die Dashboardseite des StorSimple Manager-Diensts enthält eine Übersicht für die StorSimple Virtual Arrays (auch als lokale virtuelle StorSimple-Geräte oder virtuelle Geräte bezeichnet), die mit dem StorSimple Manager-Dienst verbunden sind. Die Geräte, die der Aufmerksamkeit eines Systemadministrators bedürfen, werden entsprechend gekennzeichnet. Dieses Tutorial bietet eine Einführung zur Dashboardseite, erläutert die Dashboardinhalte und -funktion und beschreibt die Aufgaben, die Sie auf dieser Seite ausführen können.

![Dienstdashboard](./media/storsimple-ova-service-dashboard/dashboard1.png)

Das Dashboard des StorSimple Manager-Diensts zeigt die folgenden Informationen an:

* Oben auf der Seite im **Diagrammbereich** sehen Sie die relevanten Metriken für Ihre virtuellen Geräte. Sie können den primären Speicher anzeigen, der von allen virtuellen Geräten verwendet wird, sowie den Cloudspeicher, der von den virtuellen Geräten über einen bestimmten Zeitraum verwendet wird. Verwenden Sie die Steuerelemente oben rechts im Diagramm, um die relative oder absolute Nutzung anzugeben und für die Zeitskala einen Zeitraum von einer Woche, einem Monat, drei Monaten oder einem Jahr anzugeben. Verwenden Sie das Aktualisierungssteuerelement ![refresh-control](./media/storsimple-ova-service-dashboard/refresh-control.png), um das Diagramm zu aktualisieren.
* Die **Nutzungsübersicht** zeigt den bereitgestellten primären Speicher, der von allen virtuellen Geräten im Verhältnis zum Gesamtspeicher genutzt wird, der für alle virtuellen Geräte zur Verfügung steht. **Bereitgestellt** bezieht sich auf die Menge des Speicherplatzes, die vorbereitet und zur Verwendung zugeordnet wurde. **Verwendet** bezieht sich auf die Verwendung von Freigaben oder Volumes, wie sie den Initiatoren angezeigt wird, die eine Verbindung mit den virtuellen Geräten hergestellt haben. Unter **Max. Kapazität** wird die maximale Kapazität aller virtuellen Geräte angezeigt.
* Der Bereich **Warnungen** stellt eine Momentaufnahme aller aktiven Warnungen auf sämtlichen virtuellen Geräten bereit, gruppiert nach dem jeweiligen Schweregrad. Durch Klicken auf den Schweregrad wird die Seite mit den **Warnungen** geöffnet, auf der nur die betreffenden Warnungen angezeigt werden. Auf der Seite mit den **Warnungen** können Sie auf einzelne Warnungen klicken, um weitere Details zur jeweiligen Warnung anzuzeigen, einschließlich empfohlener Aktionen. Sie können die Warnung auch löschen, falls das Problem behoben wurde.
* Der Bereich **Aufträge** bietet eine Momentaufnahme der kürzlich ausgeführten Aufträge auf allen virtuellen Geräten, die mit dem Dienst verbunden sind. Es sind Links angegeben, die Sie zum Anzeigen von derzeit in Bearbeitung befindlichen Aufträgen verwenden können, oder von Aufträgen, die innerhalb der letzten 24 Stunden erfolgreich oder nicht erfolgreich abgeschlossen wurden. 
* Der **Schnellansichtsbereich** rechts auf der Seite enthält nützliche Informationen wie den Dienststatus, die Gesamtzahl der mit dem Dienst verbundenen virtuellen Geräte, den Standort des Diensts sowie Details zum Abonnement, das dem Dienst zugeordnet ist. Es gibt auch ein Link zum Vorgangsprotokoll. Klicken Sie auf den Link, um eine Liste aller abgeschlossenen Vorgänge des StorSimple Manager-Diensts anzuzeigen. 

Über das Dashboard des StorSimple Manager-Diensts können Sie die Ausführung der folgenden Aufgaben initiieren:

* Abrufen des Dienstregistrierungsschlüssels.
* Anzeigen der Vorgangsprotokolle

## <a name="get-the-service-registration-key"></a>Abrufen des Dienstregistrierungsschlüssels
Der Dienstregistrierungsschlüssel wird verwendet, um ein virtuelles StorSimple-Gerät beim StorSimple Manager-Dienst zu registrieren, damit das Gerät im klassischen Azure-Portal für weitere Verwaltungsvorgänge angezeigt wird. Der Schlüssel wird auf dem ersten virtuellen Gerät erstellt und für die verbleibenden virtuellen Geräte freigegeben. 

Durch Klicken auf **Registrierungsschlüssel** (unten auf der Seite) wird das Dialogfeld **Dienstregistrierungsschlüssel** geöffnet, über das Sie den aktuellen Dienstregistrierungsschlüssel in die Zwischenablage kopieren oder den Dienstregistrierungsschlüssel neu generieren können.

![Registrierungsschlüssel](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Das erneute Generieren des Schlüssels wirkt sich nicht auf zuvor registrierte virtuelle Geräte aus, sondern nur auf die virtuellen Geräte, die nach der Neugenerierung des Schlüssels mit dem Dienst registriert werden.

Weitere Informationen über das Abrufen des Dienstregistrierungsschlüssels finden Sie unter [Abrufen des Dienstregistrierungsschlüssels](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>Anzeigen der Vorgangsprotokolle
Sie können die Vorgangsprotokolle anzeigen, indem Sie auf den entsprechenden Link im **Schnellansichtsbereich** des Dashboards klicken. Dadurch gelangen Sie zur Verwaltungsdienste-Seite. Dort können Sie die Protokolle filtern und die für den StorSimple Manager-Dienst spezifischen Protokolle anzeigen.

![Vorgangsprotokoll](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie die lokale Webbenutzeroberfläche verwenden, um [Ihr StorSimple Virtual Array zu verwalten](storsimple-ova-web-ui-admin.md).




<!--HONumber=Nov16_HO3-->


