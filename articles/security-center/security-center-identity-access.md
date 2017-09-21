---
title: "Überwachen von Identität und Zugriff in Azure Security Center | Microsoft-Dokumentation"
description: "In diesem Dokument wird die Identitäts- und Zugriffsfunktion in Azure Security Center beschrieben, mit der Sie die Zugriffsaktivitäten der Benutzer und identitätsbezogene Probleme überwachen können."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 2e351cd38e7baefc09fa36ceabffec92de22433b
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="monitoring-identity-and-access-in-azure-security-center"></a>Überwachen von Identität und Zugriff in Azure Security Center
In diesem Dokument wird beschrieben, wie Sie Azure Security Center zum Überwachen der Identität und Zugriffsaktivitäten von Benutzern überwachen.

## <a name="why-monitor-identity-and-access"></a>Gründe für das Überwachen der Identität und des Zugriffs
Die Identität sollte die Kontrollebene für Ihr Unternehmen darstellen, und der Schutz der Identität sollte höchste Priorität haben. In der Vergangenheit haben Unternehmen über Grenzen verfügt, bei denen es sich quasi um eine der „Hauptverteidigungslinien“ gehandelt hat. Da heutzutage immer mehr Daten und Apps in die Cloud verlagert werden, ist die Identität zur neuen Grenze geworden.

Indem Sie Ihre Identitätsaktivitäten überwachen, können Sie proaktive Aktionen durchführen, bevor es zu einem Vorfall kommt, bzw. reaktive Aktionen, um einen Angriffsversuch zu stoppen. Das Dashboard „Identität und Zugriff“ enthält eine Übersicht über den Identitätszustand, z.B. die Anzahl von fehlgeschlagenen Anmeldeversuchen, das bei den Versuchen verwendete Benutzerkonto, gesperrte Konten, Konten mit geänderten oder zurückgesetzten Kennwörtern und die Anzahl von derzeit angemeldeten Konten.

## <a name="how-to-monitor-identity-and-access-activities"></a>Vorgehensweise beim Überwachen der Identität und der Zugriffsaktivitäten
Führen Sie die unten angegebenen Schritte aus, um die aktuellen Aktivitäten zu visualisieren, die sich auf die Identität und den Zugriff beziehen. Hierfür müssen Sie auf das Dashboard **Identität und Zugriff** zugreifen:

1.  Öffnen Sie das Dashboard **Security Center**.
2.  Klicken Sie im linken Bereich unter **Prävention** auf **Identität und Zugriff**. Wenn Sie mehrere Arbeitsbereiche verwenden, wird die Arbeitsbereichsauswahl angezeigt.

    ![Arbeitsbereichsauswahl](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Wenn in der letzten Spalte **UPGRADEPLAN** angezeigt wird, liegt dies daran, dass für diesen Arbeitsbereich das kostenlose Abonnement verwendet wird und Sie ein Upgrade auf ein Standard-Abonnement durchführen müssen, um dieses Feature nutzen zu können. Bei REQUIRES UPDATE (UPDATE ERFORDERLICH) müssen Sie [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) aktualisieren, um das Feature nutzen zu können. Weitere Informationen zum Tarif finden Sie unter „Azure Security Center-Preise“. 
    > 
3. Wenn Sie mehr als einen Arbeitsbereich untersuchen möchten, können Sie die Untersuchung gemäß der Spalte **FAILED LOGONS** (FEHLGESCHLAGENE ANMELDUNGEN) priorisieren. In dieser Spalte wird die aktuelle Anzahl von fehlgeschlagenen Anmeldeversuchen für den Arbeitsbereich angezeigt. Wählen Sie den gewünschten Arbeitsbereich aus. Das Dashboard **Identität und Zugriff** wird angezeigt.

    ![Identität und Zugriff](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. Die in diesem Dashboard verfügbaren Informationen dienen Ihnen als direkte Hilfe beim Identifizieren einer potenziellen verdächtigen Aktivität. Dieses Dashboard ist in drei Hauptbereiche unterteilt:
    * **Identitätsstatus**: Enthält eine Zusammenfassung der identitätsbezogenen Aktivitäten, die in diesem Arbeitsbereich durchgeführt werden.
    * **Failed logons** (Fehlgeschlagene Anmeldungen): Dient zum schnellen Identifizieren der Hauptursache für den Fehler beim Anmeldeversuch. Es wird eine Liste mit den zehn Konten angezeigt, für die bei Anmeldeversuchen die meisten Fehler aufgetreten sind.
    * **Logons overtime** (Anmeldungen mit Mehrzeit): Dient zum schnellen Identifizieren des Mehraufwands an Zeit für Anmeldungen, und es wird eine Top 10-Liste mit den Computerkonten mit entsprechenden Anmeldeversuchen angezeigt.
    
Unabhängig von der ausgewählten Kachel basiert das angezeigte Dashboard auf der [Protokollsuche-Abfrage](https://docs.microsoft.com/azure/security-center/security-center-search). Die einzigen Unterschiede sind der Typ der Abfrage und das Ergebnis. Sie können weiterhin ein Element auswählen, z.B. einen Computer, und darauf klicken, um relevante Daten anzuzeigen. 

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wurde beschrieben, wie Sie die Identität und den Zugriff in Azure Security Center überwachen. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln. 
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.


