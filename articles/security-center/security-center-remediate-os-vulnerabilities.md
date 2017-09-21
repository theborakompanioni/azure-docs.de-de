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
ms.date: 09/11/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 39879c22278a55f841e294cda5a89bec2bdf6988
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Beheben von Betriebssystem-Sicherheitsrisiken in Azure Security Center
Azure Security Center analysiert täglich das Betriebssystem Ihrer virtuellen Computer (VMs) und physischen Computer auf Konfigurationen, die sie möglicherweise anfälliger für Angriffe machen. Security Center empfiehlt die Behandlung von Sicherheitsrisiken, wenn die Betriebssystemkonfiguration nicht den empfohlenen Konfigurationsregeln entspricht, und empfiehlt Konfigurationsänderungen, um diese Sicherheitsrisiken zu beseitigen.

> [!NOTE]
> Weitere Informationen zu den speziellen Konfigurationen, die überwacht werden, finden Sie in der [Liste der empfohlenen Konfigurationsregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
„Betriebssystem-Sicherheitsrisiken beheben“ wird als Empfehlung in Security Center präsentiert. Diese Empfehlung wird unter **Empfehlungen** und unter **Compute** angezeigt.

In diesem Beispiel betrachten wir die Empfehlung **Betriebssystem-Sicherheitsrisiken beheben (durch Microsoft)** unter **Compute**.
1. Klicken Sie im Hauptmenü von Security Center auf **Compute**.

   ![Betriebssystem-Sicherheitsrisiken beheben][1]

2. Klicken Sie unter **Compute** auf **Betriebssystem-Sicherheitsrisiken beheben (durch Microsoft)**. Das Dashboard **Konflikt durch Sicherheitsrisiken für Betriebssystem (von Microsoft)** wird geöffnet.

   ![Betriebssystem-Sicherheitsrisiken beheben][2]

  Im oberen Bereich des Dashboards befindet sich Folgendes:

  - Die Gesamtanzahl von Regeln (nach Schweregrad), denen die Betriebssystemkonfiguration Ihrer virtuellen und physischen Computer nicht entspricht.
  - Die Gesamtanzahl von Regeln (nach Typ), denen die Betriebssystemkonfiguration Ihrer virtuellen und physischen Computer nicht entspricht.
  - Die Gesamtanzahl von Regelfehlern, aufgeschlüsselt nach Windows-Betriebssystemkonfigurationen und Linux-Betriebssystemkonfigurationen.

  Im unteren Bereich des Dashboards werden alle Regelfehler für Ihre virtuellen und physischen Computer sowie der Schweregrad des fehlenden Updates aufgeführt. Die Liste enthält Folgendes:

  - **CCIED**: Eindeutiger CCE-Bezeichner für die Regel Security Center verwendet die Common Configuration Enumeration (CCE), um Konfigurationsregeln eindeutige Bezeichner zuzuweisen.
  - **NAME**: Name des Regelfehlers
  - **REGELTYP**: Registrierungsschlüssel, Sicherheitsrichtlinie oder Überwachungsrichtlinie
  - **ANZAHL VON VMS UND COMPUTERN**: Die Gesamtzahl virtueller und physischer Computer mit diesem Regelfehler
  - **REGELSCHWEREGRAD**: CCE-Schweregrad: „Kritisch“, „Wichtig“ oder „Warnung“
  - **STATUS**: Der aktuelle Status der Empfehlung:

    - **Offen**: Die Empfehlung wurde noch nicht verarbeitet.
    - **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
    - **Behoben**: Die Empfehlung wurde bereits umgesetzt. (Wenn das Problem behoben wurde, wird der Eintrag abgeblendet dargestellt.)

3. Klicken Sie auf einen Regelfehler in der Liste, um weitere Informationen anzuzeigen.

   ![Konfigurationsregeln mit Fehlern][3]

  Die folgende Informationen sind auf diesem Blatt angegeben:

  - NAME: Name der Regel
  - CCIED: Eindeutiger CCE-Bezeichner für die Regel
  - Betriebssystemversion: Betriebssystemversion des virtuellen oder physischen Computers
  - REGELSCHWEREGRAD: CCE-Schweregrad: „Kritisch“, „Wichtig“ oder „Warnung“
  - VOLLSTÄNDIGE BESCHREIBUNG: Beschreibung der Regel
  - SICHERHEITSRISIKO: Erläuterung des Sicherheitsrisikos, wenn die Regel nicht angewendet wird
  - POTENZIELLE AUSWIRKUNG: Geschäftliche Auswirkung, wenn die Regel angewendet wird
  - GEGENMASSNAHME: Schritte zur Behebung
  - ERWARTETER WERT: Der erwartete Wert, wenn das Security Center die Betriebssystemkonfiguration Ihres virtuellen Computers mit der Regel abgleicht
  - TATSÄCHLICHER Wert: Zurückgegebener Wert, nachdem das Security Center die Betriebssystemkonfiguration Ihres virtuellen Computers mit der Regel abgeglichen hat
  - REGELVORGANG: Vom Security Center verwendeter Regelvorgang, wenn das Security Center die Betriebssystemkonfiguration Ihres virtuellen Computers mit der Regel abgleicht

4. Klicken Sie auf dem oberen Menüband auf das Symbol **Suche**. Die Suche wird mit einer Liste der Arbeitsbereiche geöffnet, die virtuelle und physische Computer mit dem ausgewählten Betriebssystem-Sicherheitsrisiko enthalten. Dieses Blatt für die Arbeitsbereichauswahl wird nur angezeigt, wenn die ausgewählte Regel für mehrere virtuelle Computer gilt, die mit unterschiedlichen Arbeitsbereichen verbunden sind.

  ![Aufgeführte Arbeitsbereiche][4]

5. Wählen Sie einen Arbeitsbereich aus. Eine für den Arbeitsbereich mit dem Betriebssystem-Sicherheitsrisiko gefilterte Log Analytics-Suchabfrage wird geöffnet.

  ![Arbeitsbereich mit Betriebssystem-Sicherheitsrisiko][5]

6. Klicken Sie auf einen Computer in der Liste, um weitere Informationen zu erhalten. Ein weiteres Suchergebnis wird geöffnet, das gefilterte Informationen für diesen speziellen Computer enthält.

  ![Für diesen Computer gefiltert][6]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Betriebssystem-Sicherheitsrisiken beheben“ umsetzen. Sie können den Satz der Konfigurationsregeln [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)überprüfen. Security Center verwendet die Common Configuration Enumeration (CCE), um Konfigurationsregeln eindeutige Bezeichner zuzuweisen. Auf der Website zu [CCE](https://nvd.nist.gov/cce/index.cfm) finden Sie weitere Informationen.

Weitere Informationen zu Security Center finden Sie in den folgenden Ressourcen:

* [Unterstützte Plattformen in Azure Security Center:](security-center-os-coverage.md) Enthält eine Liste mit unterstützten Windows- und Linux-VMs.
* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png

