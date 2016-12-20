---
title: Beheben von Betriebssystem-Sicherheitsrisiken in Azure Security Center | Microsoft Docs
description: "In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Betriebssystem-Sicherheitsrisiken beheben“ umsetzen."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3f38ec79596152d67e3591583d6016e8528caf4c


---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Beheben von Betriebssystem-Sicherheitsrisiken in Azure Security Center
Azure Security Center untersucht täglich das Betriebssystem Ihres virtuellen Computers (VM) auf Konfigurationen, die bewirken können, dass der virtuelle Computer anfälliger für Angriffe wird, und empfiehlt Konfigurationsänderungen, um diese Sicherheitsrisiken zu bereinigen. Weitere Informationen zu den speziellen Konfigurationen, die überwacht werden, finden Sie in der [Liste der empfohlenen Konfigurationsregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center empfiehlt, dass Sie Sicherheitsrisiken beheben, wenn die Betriebssystemkonfiguration Ihres virtuellen Computers nicht den empfohlenen Konfigurationsregeln entspricht.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Es ist keine schrittweise Anleitung.
> 
> 

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Betriebssystem-Sicherheitsrisiken beheben**.
   ![Betriebssystem-Sicherheitsrisiken beheben][1]
   
    Auf dem Blatt **Betriebssystem-Sicherheitsrisiken beheben** sind Ihre virtuellen Computer mit Betriebssystemkonfigurationen aufgeführt, die nicht den empfohlenen Konfigurationsregeln entsprechen.  Für jeden virtuellen Computer wird auf dem Blatt Folgendes angegeben:
   
   * **REGELN MIT FEHLERN** : Die Anzahl der Regeln, bei denen bei der Betriebssystemkonfiguration des virtuellen Computers Fehler aufgetreten sind.
   * **LETZTE ÜBERPRÜFUNGSZEIT** : Datum und Uhrzeit der letzten Überprüfung der Betriebssystemkonfiguration des virtuellen Computers durch Security Center.
   * **STATUS** : Der aktuelle Status des Sicherheitsrisikos:
     
     * Offen: Das Sicherheitsrisiko wurde noch nicht behoben.
     * In Bearbeitung: Die Empfehlung wird derzeit umgesetzt, sodass keine Aktion Ihrerseits erforderlich ist.
     * Gelöst: Das Sicherheitsrisiko wurde bereits behoben. (Wenn das Problem behoben wurde, ist der Eintrag grau hinterlegt.)
   * **SCHWEREGRAD** : Alle Sicherheitsrisiken wurden auf den Schweregrad „Niedrig“ festgelegt, was heißt, dass ein Sicherheitsrisiko behoben werden soll, ohne dass unmittelbar Aufmerksamkeit erforderlich ist.

Wählen Sie einen virtuellen Computer aus. Für diesen virtuellen Computer wird ein Blatt geöffnet, auf dem die fehlgeschlagenen Regeln angezeigt werden.
   ![Konfigurationsregeln mit Fehlern][2]

Wählen Sie eine Regel aus. Lassen Sie uns bei diesem Beispiel **Kennwort muss Komplexitätsvoraussetzungen entsprechen**auswählen. Ein Blatt mit einer Beschreibung der Regel mit dem Fehler und der Auswirkung wird geöffnet. Überprüfen Sie die Details und wie Betriebssystemkonfigurationen angewendet werden.
  ![Beschreibung der Regel mit Fehlern][3]

  Security Center verwendet die Common Configuration Enumeration (CCE), um Konfigurationsregeln eindeutige Bezeichner zuzuweisen. Die folgende Informationen sind auf diesem Blatt angegeben:

* NAME: Name der Regel
* Schweregrad: CCE-Schweregrad: „Kritisch“, „Wichtig“ oder „Warnung“
* CCIED: Eindeutiger CCE-Bezeichner für die Regel
* Beschreibung: Beschreibung der Regel
* SICHERHEITSRISIKO: Erläuterung des Sicherheitsrisikos, wenn die Regel nicht angewendet wird
* AUSWIRKUNG: Geschäftliche Auswirkung, wenn die Regel angewendet wird
* ERWARTETER WERT: Der erwartete Wert, wenn das Security Center die Betriebssystemkonfiguration Ihres virtuellen Computers mit der Regel abgleicht
* REGELVORGANG: Vom Security Center verwendeter Regelvorgang, wenn das Security Center die Betriebssystemkonfiguration Ihres virtuellen Computers mit der Regel abgleicht
* TATSÄCHLICHER Wert: Zurückgegebener Wert, nachdem das Security Center die Betriebssystemkonfiguration Ihres virtuellen Computers mit der Regel abgeglichen hat
* AUSWERTUNGSERGEBNIS: Ergebnis der Analyse: Bestanden oder Fehler

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Betriebssystem-Sicherheitsrisiken beheben“ umsetzen. Sie können den Satz der Konfigurationsregeln [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)überprüfen. Security Center verwendet die Common Configuration Enumeration (CCE), um Konfigurationsregeln eindeutige Bezeichner zuzuweisen. Auf der Website zu [CCE](http://cce.mitre.org) finden Sie weitere Informationen.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png



<!--HONumber=Nov16_HO3-->


