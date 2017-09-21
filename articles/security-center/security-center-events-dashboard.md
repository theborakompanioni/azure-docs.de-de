---
title: "Überwachen und Verarbeiten von Sicherheitsereignissen in Azure Security Center | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mit dem Security Center-Dashboard für Ereignisse Sicherheitsereignisse von Ihren virtuellen Azure-Computern und Azure-fremden Computern anzeigen können."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 01ac75244839c0e3c1ac350d4271677feb21a9d7
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>Überwachen und Verarbeiten von Sicherheitsereignissen in Azure Security Center
Das Ereignis-Dashboard enthält eine Übersicht über die Anzahl der im Laufe der Zeit erfassten Sicherheitsereignisse sowie eine Liste relevanter Ereignisse, die unter Umständen Ihre Aufmerksamkeit erfordern.  

> [!NOTE]
> Damit dieses Feature verwendet werden kann, muss für Ihren Arbeitsbereich Log Analytics Version 2 ausgeführt werden. Außerdem muss für den Arbeitsbereich der Standard-Tarif von Security Center festgelegt sein. Weitere Informationen zum Standard-Tarif finden Sie auf der Seite mit den [Preisen für Security Center](security-center-pricing.md).
>
>

## <a name="what-is-a-security-event"></a>Was ist ein Sicherheitsereignis?
Security Center nutzt Microsoft Monitoring Agent für die Erfassung verschiedener sicherheitsbezogener Konfigurationen und Ereignisse auf Ihren Computern und speichert diese Ereignisse in Ihren Arbeitsbereichen. Beispiele für diese Daten sind Betriebssystemprotokolle (Windows-Ereignisprotokolle), ausgeführte Prozesse sowie Ereignisse aus in Security Center integrierten Sicherheitslösungen. Darüber hinaus kopiert der Microsoft Monitoring Agent Absturzabbilddateien in Ihren Arbeitsbereich bzw. in Ihre Arbeitsbereiche.

## <a name="events-processed-dashboard"></a>Dashboard mit verarbeiteten Ereignissen
Sie greifen auf das Dashboard **Ereignisse** über das Hauptmenü von Security Center oder über das Blatt **Übersicht** in Security Center zu.  

![Dashboard mit verarbeiteten Ereignissen][1]

Auf der Kachel **Ereignisse** unter **Security Center** wird die Anzahl von Ereignissen angezeigt, die von ihren virtuellen Azure-Computern und Azure-fremden Computern an Security Center gesendet werden.

Unter **Events dashboard** (Ereignis-Dashboard) wird eine Übersicht über die Anzahl der im Laufe der Zeit verarbeiteten Ereignissen sowie eine Liste der Ereignisse angezeigt.

 ![Dashboard][2]

 Der obere Bereich des Dashboards zeigt den Verlauf aller Ereignisse, die in der letzten Woche verarbeitet wurden. In der unteren Hälfte des Dashboards sind relevante Ereignisse sowie alle Ereignisse nach Typ aufgeführt:

 - Zu relevanten Ereignissen ****zählen von Security Center bereitgestellte Ereignisabfragen sowie von Ihnen erstellte und hinzugefügte Ereignisabfragen. Das Dashboard bietet außerdem eine Kurzübersicht über die Anzahl der einzelnen relevanten Ereignisse.
 - Unter **All events by type** (Alle Ereignisse nach Typ) werden die empfangenen Ereignistypen und die Anzahl der einzelnen Typen angezeigt. Beispiele für Ereignistypen: SecurityEvent, CommonSecurityLog, WindowsFirewall und W3CIISLog.

## <a name="view-processed-event-details"></a>Anzeigen von Details zu verarbeiteten Ereignissen
1. Klicken Sie im Hauptmenü von **Security Center** auf **Events Processed** (Verarbeitete Ereignisse).
2. Unter Umständen wird die Arbeitsbereichsauswahl **Events Processed** (Verarbeitete Ereignisse) geöffnet. Wenn nur ein Arbeitsbereich vorhanden ist, wird diese Arbeitsbereichsauswahl nicht angezeigt. Wenn Sie über mehrere Arbeitsbereiche verfügen, müssen Sie einen Arbeitsbereich auswählen, um seine Details zu verarbeiteten Ereignissen anzuzeigen. Wählen Sie einen Arbeitsbereich aus der Liste aus, wenn mehrere Arbeitsbereiche vorhanden sind.

  ![Liste mit Arbeitsbereichen][3]

3. Das Dashboard **Events Processed** (Verarbeitete Ereignisse) wird geöffnet und zeigt Ereignisdetails für den ausgewählten Arbeitsbereich an. Sie können die relevante Ereignissen und alle Ereignisse nach Typ anzeigen.  In diesem Beispiel wurde **Notable events** (Relevante Ereignisse) ausgewählt.

  ![Relevantes Ereignis][4]

4. Durch Auswahl eines Ereignistyps können Sie weitere Daten eines Arbeitsbereichs abfragen. In diesem Beispiel wurde **SecurityEvent** ausgewählt.

  ![Auswählen eines Ereignistyps][5]

5. **Protokollsuche** wird mit zusätzlichen Details zum Ereignistyp geöffnet.

  ![Protokollsuche][6]

## <a name="add-a-notable-event"></a>Hinzufügen eines relevanten Ereignisses
Security Center stellt vorkonfigurierte relevante Ereignisse bereit. Sie können mithilfe der [Log Analytics-Abfragesprache](../log-analytics/log-analytics-search-reference.md) auf der Grundlage Ihrer eigenen Abfrage relevante Ereignisse hinzufügen. Wir kehren zum Dashboard **Events Processed** (Verarbeitete Ereignisse) zurück, um ein relevantes Ereignis hinzuzufügen.

1. Klicken Sie auf **Add Notable Event** (Relevantes Ereignis hinzufügen).

  ![Hinzufügen eines relevanten Ereignisses][7]

2. **Add custom notable event** (Benutzerdefiniertes relevantes Ereignis hinzufügen) wird geöffnet.  Geben Sie unter **Anzeigename** einen Namen für das relevante Ereignis ein. Geben Sie unter **Suchabfrage** Ihre Abfrage für das Ereignis ein.

  ![Eingeben Ihrer Abfrage][8]

4. Klicken Sie auf **OK**.

## <a name="update-your-workspace-for-events-processing"></a>Aktualisieren des Arbeitsbereichs für die Ereignisverarbeitung
Für Ihren Arbeitsbereich muss Log Analytics Version 2 ausgeführt werden, und für den Arbeitsbereich muss der Standard-Tarif von Security Center festgelegt sein, damit die Ereignisverarbeitung in Security Center verwendet werden kann. In der Arbeitsbereichsauswahl **Ereignisse** sind Arbeitsbereiche angegeben, die diese Anforderungen nicht erfüllen.

![Arbeitsbereich erfüllt Anforderungen nicht.][9]

Gehen Sie wie folgt vor:

- Wenn die Zeile des Arbeitsbereichs **REQUIRES UPDATE** (UPDATE ERFORDERLICH) enthält, müssen Sie den Arbeitsbereich auf Log Analytics Version 2 aktualisieren.
- Wenn in der Zeile des Arbeitsbereichs **UPGRADE PLAN** (TARIFUPGRADE) angezeigt wird, müssen Sie für den Arbeitsbereich ein Upgrade auf den Standard-Tarif von Security Center ausführen.
- Wenn die Zeile des Arbeitsbereichs leer ist, erfüllt der Arbeitsbereich die Anforderungen, und bei Auswahl eines Arbeitsbereichs wird das Dashboard aufgerufen.

> [!NOTE]
> Die Spalte **EREIGNISSE** unter **Ereignisse** enthält die Anzahl von Ereignissen in jedem Arbeitsbereich.  Diese Spalte ist bei einigen Arbeitsbereiche leer, da für diese Arbeitsbereiche der Free-Tarif von Security Center gilt. Im Tarif „Free“ sammelt Security Center Ereignisse, sie werden jedoch nicht in Log Analytics gespeichert und sind nicht auf dem Dashboard verfügbar.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Aktualisieren des Arbeitsbereichs auf Log Analytics Version 2
1. Wählen Sie einen Arbeitsbereich aus, der aktualisiert werden muss (**REQUIRES UPDATE** (UPDATE ERFORDERLICH)).
2. **Upgrade der Suche** wird geöffnet. Wählen Sie **Upgrade jetzt ausführen**.

  ![Upgrade jetzt ausführen][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Upgrade auf den Standard-Tarif von Security Center
1. Wählen Sie einen Arbeitsbereich, für den **UPGRADE PLAN** (TARIFUPGRADE) angezeigt wird.
2. **Events dashboard** (Ereignis-Dashboard) wird geöffnet. Klicken Sie auf **Try the Events Processed dashboard** (Dashboard „Verarbeitete Ereignisse“ ausprobieren).

  ![Dashboard ausprobieren][11]

3. Wählen Sie unter **Integration in erweiterte Sicherheit** den Arbeitsbereich aus, für den Sie ein Upgrade ausführen.
4. Wählen Sie unter **Preise** die Option **Standard**.
5. Wählen Sie **Speichern** aus.

  ![Upgrade auf den Standard-Tarif][12]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie das Dashboard „Events Processed“ (Verarbeitete Ereignisse) von Security Center verwenden. Weitere Informationen zur Funktionsweise des Dashboards und zum Schreiben eigener Ereignisabfragen finden Sie in den folgenden Ressourcen:

- [Was ist Log Analytics?](../log-analytics/log-analytics-overview.md) – Übersicht über Log Analytics
- [Grundlegendes zu Protokollsuchvorgängen in Log Analytics](../log-analytics/log-analytics-log-search-new.md) – In diesem Artikel wird beschrieben, wie Protokollsuchen in Log Analytics verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen einer Protokollsuche bekannt sein sollten.
- [Referenz zur Log Analytics-Suche](../log-analytics/log-analytics-search-reference.md) – Hier erfahren Sie, wie Sie mit der Abfragesprache in Log Analytics eigene Ereignisabfragen schreiben.

Weitere Informationen zu Security Center finden Sie unter folgendem Link:

- [Einführung in Azure Security Center](security-center-intro.md) – Hier werden wichtige Funktionen von Security Center beschrieben.

<!--Image references-->
[1]: ./media/security-center-events-dashboard/events-processed.png
[2]: ./media/security-center-events-dashboard/dashboard.png
[3]: ./media/security-center-events-dashboard/view-processed-event.png
[4]: ./media/security-center-events-dashboard/notable-event.png
[5]: ./media/security-center-events-dashboard/events-by-type.png
[6]: ./media/security-center-events-dashboard/log-search-detail.png
[7]: ./media/security-center-events-dashboard/add-notable-event.png
[8]: ./media/security-center-events-dashboard/create-query.png
[9]: ./media/security-center-events-dashboard/requires-update.png
[10]: ./media/security-center-events-dashboard/search-upgrade.png
[11]: ./media/security-center-events-dashboard/try-dashboard.png
[12]: ./media/security-center-events-dashboard/onboard-workspace.png

