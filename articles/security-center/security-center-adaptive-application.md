---
title: Adaptive Anwendungssteuerungen in Azure Security Center | Microsoft-Dokumentation
description: "In diesem Dokument erfahren Sie, wie Sie Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden, mithilfe der adaptiven Anwendungssteuerung in Azure Security Center einer Whitelist hinzufügen."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 18ae6a970455646b7a25170f5abefa52a98b0ba2
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Adaptive Anwendungssteuerungen in Azure Security Center (Vorschauversion)
In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie die Anwendungssteuerung in Azure Security Center verwenden.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Was sind adaptive Anwendungssteuerungen in Security Center?
Mit adaptiven Anwendungssteuerungen lässt sich steuern, welche Anwendungen auf Ihren virtuellen Computern in Azure ausgeführt werden können. Dadurch können Sie Ihre virtuellen Computer unter anderem besser vor Schadsoftware schützen. Security Center nutzt Machine Learning, um die auf dem virtuellen Computer ausgeführten Prozesse zu analysieren, und unterstützt Sie beim Anwenden von Whitelistregeln, die auf diesen Daten basieren. Dadurch können Sie Anwendungswhitelists deutlich einfacher konfigurieren und verwalten und profitieren außerdem von folgenden Möglichkeiten:

- Blockierung der Ausführung schädlicher Anwendungen oder Ausgabe einer Warnung beim Versuch, eine schädliche Anwendung auszuführen (auch bei Anwendungen, die möglicherweise von Antischadsoftwarelösungen nicht erfasst werden)
- Einhaltung der Sicherheitsrichtlinie Ihrer Organisation, die die ausschließliche Verwendung von lizenzierter Software vorschreibt
- Vermeidung der Verwendung von unerwünschter Software in Ihrer Umgebung
- Vermeidung der Ausführung veralteter oder nicht unterstützter Apps
- Verhinderung der Verwendung bestimmter Softwaretools, die in Ihrer Organisation nicht zulässig sind
- Steuerung des Zugriffs auf sensible Daten im Rahmen der App-Nutzung durch die IT-Abteilung

## <a name="how-to-enable-adaptive-application-controls"></a>Aktivieren adaptiver Anwendungssteuerungen
Mit adaptiven Anwendungssteuerungen können Sie eine Gruppe von Anwendungen definieren, deren Ausführung in konfigurierten Ressourcengruppen zulässig ist. Dieses Feature steht nur für Windows-Computer (alle Versionen; klassisch oder Azure Resource Manager) zur Verfügung. Gehen Sie wie folgt vor, um das Anwendungswhitelisting in Security Center zu konfigurieren:

1.  Öffnen Sie das Dashboard **Security Center**, und klicken Sie auf **Übersicht**.
2.  Unter **Erweiterter Cloudschutz** zeigt die Kachel **Adaptive application controls** (Adaptive Anwendungssteuerungen) die Anzahl virtueller Computer, für die die Steuerung derzeit aktiviert ist (relativ zur Gesamtanzahl virtueller Computer). Auf dieser Kachel sehen Sie auch, wie viele Probleme in der letzten Woche gefunden wurden: 

    ![Adaptive Anwendungssteuerungen](./media/security-center-adaptive-application\security-center-adaptive-application-fig1.png)

3. Klicken Sie auf die Kachel **Adaptive application controls** (Adaptive Anwendungssteuerungen), um weitere Optionen anzuzeigen.

    ![controls](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. Der Abschnitt „Ressourcengruppen“ enthält drei Registerkarten:
    * **Empfohlen**: Liste der Ressourcengruppen, für die Anwendungssteuerung empfohlen wird. Security Center nutzt Machine Learning, um geeignete virtuelle Computer für die Anwendungssteuerung basierend darauf zu ermitteln, ob die virtuellen Computer konsistent die gleichen Anwendungen ausführen.
    * **Konfiguriert**: Liste der Ressourcengruppen mit den virtuellen Computern, für die die Anwendungssteuerung bereits konfiguriert ist. 
    * **No recommendation** (Keine Empfehlung): Liste der Ressourcengruppen mit virtuellen Computern, für die keine Anwendungssteuerungsempfehlungen vorliegen. Ein Beispiel wären etwa virtuelle Computer, auf denen sich die Anwendungen immer wieder ändern und die keinen konsistenten Zustand erreicht haben.

Ausführlichere Informationen zu den einzelnen Optionen und zu ihrer Verwendung finden Sie in den folgenden Abschnitten.

### <a name="configure-a-new-application-control-policy"></a>Konfigurieren einer neuen Anwendungssteuerungsrichtlinie
Klicken Sie auf die Registerkarte **Empfohlen**, um eine Liste mit Ressourcengruppen mit Anwendungssteuerungsempfehlungen anzuzeigen:

![Empfohlen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

Die Liste enthält Folgendes:
- **NAME**: Der Name des Abonnements und der Ressourcengruppe.
- **VMs**: Die Anzahl virtueller Computer in der Ressourcengruppe.
- **ZUSTAND**: Der Zustand der Empfehlungen (in der Regel „Offen“).
- **SCHWEREGRAD**: Der Schweregrad der Empfehlungen.

Wählen Sie eine Ressourcengruppe aus, um die Option **Create application control rules** (Anwendungssteuerungsregeln erstellen) zu öffnen:

![Anwendungssteuerungsregeln](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

Sehen Sie sich unter **VMs auswählen** die Liste mit den empfohlenen virtuellen Computern an, und heben Sie die Auswahl aller virtuellen Computer auf, auf die Sie die Anwendungssteuerung nicht anwenden möchten. Sehen Sie sich unter **Prozesse für Whitelistregeln auswählen** die Liste mit empfohlenen Anwendungen an, und heben Sie die Auswahl aller Anwendungen auf, die Sie nicht anwenden möchten. Die Liste enthält Folgendes:

- **NAME**: Der vollständige Anwendungspfad.
- **PROZESSE**: Die Anzahl von Anwendungen innerhalb des jeweiligen Pfads.
- **ALLGEMEIN**: „TRUE“ gibt an, dass diese Prozesse auf den meisten virtuellen Computern in dieser Ressourcengruppe ausgeführt wurden.
- **SICHERHEITSLÜCKE**: Ein Warnsymbol gibt an, ob die Anwendungen von einem Angreifer zur Umgehung des Anwendungswhitelistings verwendet werden können. Diese Anwendungen sollten vor der Genehmigung unbedingt überprüft werden. 

Klicken Sie nach Abschluss Ihrer Auswahl auf die Schaltfläche **Erstellen**. Die Anwendungssteuerung wird standardmäßig immer im *Überwachungsmodus* aktiviert. Nachdem Sie sich vergewissern haben, dass Ihre Whitelist keine nachteiligen Auswirkungen auf Ihre Workload hat, können Sie in den *Erzwingungsmodus* wechseln.

> [!NOTE]
> Aus Sicherheitsgründen versucht Security Center immer, eine Herausgeberregel für die Anwendungen zu erstellen, die in die Whitelist aufgenommen werden sollen. Eine Pfadregel für den vollständigen Pfad der spezifischen EXE-Datei wird nur erstellt, wenn eine Anwendung über keine Herausgeberinformationen verfügt (also nicht signiert ist).
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Bearbeiten und Überwachen einer mit Anwendungssteuerung konfigurierten Gruppe

Klicken Sie zum Bearbeiten und Überwachen einer mit Anwendungssteuerung konfigurierten Gruppe unter **Ressourcengruppen** auf **KONFIGURIERT**:

![Ressourcengruppen](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

Die Liste enthält Folgendes:

- **NAME**: Der Name des Abonnements und der Ressourcengruppe.
- **VMs**: Die Anzahl virtueller Computer in der Ressourcengruppe.
- **MODUS**: Der Überwachungsmodus protokolliert, wenn versucht wird, eine Anwendung auszuführen, die sich nicht in der Whitelist befindet. Im Blockierungsmodus wird die Ausführung von Anwendungen verhindert, die sich nicht in der Whitelist befinden.
- **SCHWEREGRAD**: Der Schweregrad der Empfehlungen.

Wählen Sie eine Ressourcengruppe aus, um Änderungen auf der Seite **Edit application control policy** (Anwendungssteuerungsrichtlinie bearbeiten) vorzunehmen.

![Schutz](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

Unter **Schutzmodus** stehen folgende Optionen zur Auswahl:
- **Überwachen**: In diesem Modus werden die Regeln von der Anwendungssteuerungslösung nicht erzwungen und nur die Aktivitäten auf den geschützten virtuellen Computern überwacht. Dies empfiehlt sich, wenn Sie zuerst das allgemeine Verhalten untersuchen möchten, bevor Sie die Ausführung einer App auf dem virtuellen Zielcomputer blockieren.
- **Erzwingen**: In diesem Modus werden die Regeln von der Anwendungssteuerungslösung erzwungen, und Anwendungen, deren Ausführung nicht zulässig ist, werden blockiert. 

Wie bereits erwähnt, werden neue Anwendungssteuerungsrichtlinien standardmäßig immer im *Überwachungsmodus* konfiguriert. Unter **Policy extension** (Richtlinienerweiterung) können Sie eigene Anwendungspfade hinzufügen, die in die Whitelist aufgenommen werden sollen. Nach dem Hinzufügen dieser Pfade erstellt Security Center zusätzlich zu den bereits vorhandenen Regeln die entsprechenden Regeln für diese Anwendungen. Im Abschnitt **Probleme** werden alle aktuellen Verstöße aufgeführt.

![Probleme](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Diese Liste enthält:

- **PROBLEME**: Alle protokollierten Verstöße. Mögliche Optionen:
    - **ViolationsBlocked**: Tritt auf, wenn sich die Lösung im Erzwingungsmodus befindet und versucht wird, eine Anwendung auszuführen, die nicht in der Whitelist enthalten ist.
    - **ViolationsAudited**: Tritt auf, wenn sich die Lösung im Überwachungsmodus befindet und eine Anwendung ausgeführt wird, die nicht in der Whitelist enthalten ist.
    - **RulesViolatedManually**: Tritt auf, wenn ein Benutzer versucht hat, Regeln auf den virtuellen Computern manuell (und nicht über das ASC-Verwaltungsportal) zu konfigurieren.
- **NO. OF VMS** (ANZAHL VIRTUELLER COMPUTER): Die Anzahl virtueller Computer mit diesem Problemtyp.

Durch Klicken auf die einzelnen Zeilen gelangen Sie zur Seite [Azure-Aktivitätsprotokoll](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Hier finden Sie Informationen zu allen virtuellen Computern mit dieser Art von Verstoß. Wenn Sie auf die drei Punkte am Zeilenende klicken, können Sie den entsprechenden Eintrag löschen. Der Abschnitt **Configured virtual machines** (Konfigurierte virtuelle Computer) enthält die virtuellen Computer, für die diese Regeln gelten. 

![Konfigurierte virtuelle Computer](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

Unter **Regeln für Herausgeberwhitelists** werden die Anwendungen aufgeführt, für die eine Herausgeberregel erstellt wurde – basierend auf den Zertifikatinformationen, die für die jeweilige Anwendung gefunden wurden. Weitere Informationen finden Sie unter [Understanding the publisher rule condition in AppLocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) (Grundlegendes zur Herausgeberregelbedingung in AppLocker).

![Whitelistregeln](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Wenn Sie auf die drei Punkte am Zeilenende klicken, können Sie die entsprechende Regel löschen. Unter **Regeln für Pfadwhitelists** wird der vollständige Anwendungspfad (einschließlich der ausführbaren Datei) für die Anwendungen aufgeführt, die nicht mit einem digitalen Zertifikat signiert, aber in den Whitelistregeln immer noch aktuell sind. 

> [!NOTE]
> Aus Sicherheitsgründen versucht Security Center standardmäßig, eine Herausgeberregel für die ausführbaren Dateien zu erstellen, die in die Whitelist aufgenommen werden sollen. Eine Pfadregel für den vollständigen Pfad der spezifischen EXE-Datei wird nur erstellt, wenn eine ausführbare Datei über keine Herausgeberinformationen verfügt (also nicht signiert ist).

![Regeln für Pfadwhitelists](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

Die Liste enthält Folgendes:
- **NAME**: Der vollständige Patch der ausführbaren Datei.
- **SICHERHEITSLÜCKE**: „TRUE“ gibt an, dass Anwendungen von einem Angreifer zur Umgehung des Anwendungswhitelistings verwendet werden können.  

Wenn Sie auf die drei Punkte am Zeilenende klicken, können Sie die entsprechende Regel löschen. Nach Abschluss der Änderungen können Sie auf die Schaltfläche **Speichern** klicken, um Ihre Änderungen zu übernehmen, oder auf **Verwerfen**, um Ihre Änderungen zu verwerfen.

### <a name="not-recommended-list"></a>Liste für Elemente ohne Empfehlung

Security Center empfiehlt das Anwendungswhitelisting nur für virtuelle Computer, die einen stabilen Satz von Anwendungen ausführen. Wenn sich die Anwendungen auf den zugeordneten virtuellen Computern immer wieder ändern, werden keine Empfehlungen erstellt. 

![Empfehlung](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Die Liste enthält Folgendes:
- **NAME**: Der Name des Abonnements und der Ressourcengruppe.
- **VMs**: Die Anzahl virtueller Computer in der Ressourcengruppe.

## <a name="preview-registration"></a>Vorschauregistrierung

Adaptive Anwendungssteuerungen stehen für Azure Security Center Standard-Kunden als eingeschränkte öffentliche Vorschauversion zur Verfügung. Wenn Sie die Vorschauversion ausprobieren möchten, senden Sie uns eine [E-Mail](mailto:ASC_appcontrol@microsoft.com) mit Ihren Abonnement-IDs.

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden, mithilfe der adaptiven Anwendungssteuerung in Azure Security Center einer Whitelist hinzufügen. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln. 
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.


