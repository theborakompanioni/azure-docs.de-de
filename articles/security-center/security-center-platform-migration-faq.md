---
title: "Häufig gestellte Fragen zur Plattformmigration von Security Center | Microsoft-Dokumentation"
description: In diesem FAQ-Dokument werden Fragen zur Plattformmigration von Azure Security Center beantwortet.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 2ffbaca614d667db565197f3c13b1658fffc2a7c
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="security-center-platform-migration-faq"></a>Häufig gestellte Fragen zur Plattformmigration von Security Center
Seit Anfang Juni 2017 verwendet Azure Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). In diesem FAQ-Dokument werden Fragen zur Plattformmigration beantwortet.

## <a name="data-collection-agents-and-workspaces"></a>Datensammlung, Agents und Arbeitsbereiche

### <a name="how-is-data-collected"></a>Wie werden Daten gesammelt?
Security Center verwendet den Microsoft Monitoring Agent, um Daten von Ihren virtuellen Computern zu sammeln. Die Sicherheitsdaten umfassen Informationen zu Sicherheitskonfigurationen (zur Identifizierung der Sicherheitsrisiken) sowie Informationen zu Sicherheitsereignissen (zur Erkennung von Bedrohungen). Vom Agent gesammelte Daten werden entweder in einem vorhandenen, mit dem virtuellen Computer verbundenen Log Analytics-Arbeitsbereich oder in einem neuen, von Security Center erstellten Arbeitsbereich gespeichert. Bei der Erstellung eines neuen Arbeitsbereichs wird der Standort des virtuellen Computers berücksichtigt.

> [!NOTE]
> Der Microsoft Monitoring Agent ist der gleiche Agent, der auch von der Operations Management Suite (OMS), vom Log Analytics-Dienst und von System Center Operations Manager (SCOM) verwendet wird.
>
>

Wenn die Datensammlung erstmals aktiviert wird oder Ihre Abonnements migriert werden, überprüft Security Center, ob der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf den einzelnen virtuellen Computern installiert ist. Ist der Microsoft Monitoring Agent nicht installiert, geschieht Folgendes:

- Der Microsoft Monitoring Agent wird auf dem virtuellen Computer installiert.
   - Wenn am Standort des virtuellen Computers bereits ein von Security Center erstellter Arbeitsbereich vorhanden ist, wird der Agent mit diesem Arbeitsbereich verbunden.
   - Ist kein Arbeitsbereich vorhanden, erstellt Security Center eine neue Ressourcengruppe und einen Standardarbeitsbereich an diesem Standort und verbindet den Agent mit diesem Arbeitsbereich. Für den Arbeitsbereich und die Ressourcengruppe gilt folgende Namenskonvention:

       Arbeitsbereich: DefaultWorkspace-[Abonnement-ID]-[Region]

       Ressourcengruppe: DefaultResouceGroup-[Region]
- Im Arbeitsbereich wird eine Security Center-Lösung installiert.

Der Standort des Arbeitsbereichs basiert auf dem Standort des virtuellen Computers. Weitere Informationen finden Sie unter [Azure Security Center-Datensicherheit](security-center-data-security.md).

> [!NOTE]
> Vor der Plattformmigration hat Security Center Sicherheitsdaten Ihrer virtuellen Computer mithilfe des Azure Monitoring Agents gesammelt und in Ihrem Speicherkonto gespeichert. Nach der Plattformmigration sammelt und speichert Security Center die gleichen Daten mithilfe des Microsoft Monitoring Agents und des Arbeitsbereichs. Das Speicherkonto kann nach der Migration entfernt werden.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-the-workspaces-created-by-security-center"></a>Werden mir die von Security Center erstellten Arbeitsbereiche im Rahmen von Log Analytics oder OMS in Rechnung gestellt?
Nein. Von Security Center erstellte Arbeitsbereiche sind zwar für die knotenbasierte Abrechnung von OMS konfiguriert, es fallen jedoch keine OMS-Kosten an. Die Abrechnung von Security Center basiert immer auf Ihrer Security Center-Sicherheitsrichtlinie und den installierten Lösungen in einem Arbeitsbereich:

- **Free-Tarif:** Security Center installiert die Lösung „SecurityCenterFree“ im Standardarbeitsbereich. Für den Free-Tarif entstehen keine Kosten.
- **Standard-Tarif:** Security Center installiert die Lösungen „SecurityCenterFree“ und „Security“ im Standardarbeitsbereich.

Weitere Informationen zu Preisen finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/). Auf der Seite mit den Preisinformationen werden Änderungen an der Speicherung von Sicherheitsdaten und die anteilsmäßige Abrechnung ab Juni 2017 berücksichtigt.

> [!NOTE]
> Der OMS-Tarif von Arbeitsbereichen, die von Security Center erstellt wurden, hat keine Auswirkungen auf die Security Center-Abrechnung.
>
>

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Kann ich die von Security Center erstellten Standardarbeitsbereiche löschen?
**Das Löschen des Standardarbeitsbereichs wird nicht empfohlen.** Security Center verwendet die Standardarbeitsbereiche, um Sicherheitsdaten Ihrer virtuellen Computer zu speichern.  Wenn Sie einen Arbeitsbereich löschen, kann Security Center diese Daten nicht erfassen, und einige Sicherheitsempfehlungen und Warnungen sind nicht verfügbar.

Entfernen Sie zur Wiederherstellung den Microsoft Monitoring Agent auf den virtuellen Computern, die mit dem gelöschten Arbeitsbereich verbunden sind. Security Center installiert den Agent erneut und erstellt neue Standardarbeitsbereiche.

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Was passiert, wenn der Microsoft Monitoring Agent bereits als Erweiterung auf dem virtuellen Computer installiert wurde?
Bereits vorhandene Verbindungen mit Benutzerarbeitsbereichen werden von Security Center nicht überschrieben. Security Center speichert Sicherheitsdaten des virtuellen Computers im bereits verbundenen Arbeitsbereich.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Was passiert, wenn auf dem Computer ein Microsoft Monitoring Agent installiert ist, es sich dabei aber nicht um eine Erweiterung handelt?
Wenn der Microsoft Monitoring Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, wird der Microsoft Monitoring Agent nicht installiert, und die Sicherheitsüberwachung ist nur eingeschränkt möglich.

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Was passiert, wenn ich diese Erweiterungen entferne?
Wenn Sie die Microsoft Monitoring-Erweiterung entfernen, kann Security Center keine Sicherheitsdaten des virtuellen Computers erfassen, und einige Sicherheitsempfehlungen und Warnungen sind nicht verfügbar. Innerhalb von 24 Stunden erkennt Security Center, dass die Erweiterung auf dem virtuellen Computer fehlt, und installiert sie erneut.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Wie verhindere ich die automatische Agent-Installation und die Arbeitsbereicherstellung?
Sie können die Datensammlung für Ihre Abonnements in der Sicherheitsrichtlinie deaktivieren, dies wird jedoch nicht empfohlen. Die Deaktivierung der Datensammlung schränkt die Empfehlungen und Warnungen von Security Center ein. Die Datensammlung ist für Abonnements des Standard-Tarifs erforderlich. So deaktivieren Sie die Datensammlung:

1. Wenn Ihr Abonnement für den Standard-Tarif konfiguriert ist, öffnen Sie die Sicherheitsrichtlinie für dieses Abonnement, und wählen Sie den Tarif **Free** aus.

   ![Tarif ][1]

2. Deaktivieren Sie als Nächstes die Datensammlung, indem Sie auf dem Blatt **Sicherheitsrichtlinie – Datensammlung** die Option **Aus** auswählen.

   ![Datensammlung][2]

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Wie entferne ich durch Security Center installierte OMS-Erweiterungen?
Der Microsoft Monitoring Agent kann manuell entfernt werden. Dies wird jedoch nicht empfohlen, da es die Empfehlungen und Warnungen von Security Center einschränkt.

> [!NOTE]
> Bei aktivierter Datensammlung wird der Agent nach dem Entfernen erneut installiert.  Daher müssen Sie vor dem manuellen Entfernen des Agents die Datensammlung deaktivieren. Die Anleitung zum Deaktivieren der Datensammlung finden Sie unter [Wie verhindere ich die automatische Agent-Installation und die Arbeitsbereicherstellung?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?).
>
>

So entfernen Sie den Agent manuell:

1.  Öffnen Sie im Portal **Log Analytics**.
2.  Wählen Sie auf dem Blatt „Log Analytics“ einen Arbeitsbereich aus:
3.  Wählen Sie die virtuellen Computer aus, die Sie nicht überwachen möchten, und wählen Sie anschließend **Trennen** aus.

   ![Entfernen des Agents][3]

> [!NOTE]
> Falls ein virtueller Linux-Computer bereits über einen OMS-Agent verfügt, bei dem es sich nicht um eine Erweiterung handelt, wird der Agent beim Entfernen der Erweiterung ebenfalls entfernt und muss vom Kunden erneut installiert werden.
>
>

## <a name="existing-oms-customers"></a>OMS-Bestandskunden

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Überschreibt Security Center bereits vorhandene Verbindungen zwischen virtuellen Computern und Arbeitsbereichen?
Wenn der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf einem virtuellen Computer installiert ist, wird die vorhandene Arbeitsbereichverbindung nicht überschrieben. Stattdessen verwendet Security Center den vorhandenen Arbeitsbereich.

Eine Security Center-Lösung wird im Arbeitsbereich installiert, falls noch keine vorhanden ist, und die Lösung wird nur auf die relevanten virtuellen Computer angewendet. Wenn Sie eine Lösung hinzufügen, wird sie automatisch standardmäßig für alle mit Ihrem Log Analytics-Arbeitsbereich verbundenen Windows- und Linux-Agents bereitgestellt. Mit der [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) (ein OMS-Feature) können Sie einen Geltungsbereich auf Ihre Lösungen anwenden.

Wenn der Microsoft Monitoring Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, wird der Microsoft Monitoring Agent nicht installiert, und die Sicherheitsüberwachung ist eingeschränkt.

### <a name="what-should-i-do-if-i-suspect-that-the-data-platform-migration-broke-the-connection-between-one-of-my-vms-and-my-workspace"></a>Was kann ich tun, wenn ich den Verdacht habe, dass die Datenplattformmigration die Verbindung zwischen einem virtuellen Computer und meinem Arbeitsbereich unterbrochen hat?
Dieser Fall sollte nicht eintreten. Falls doch, [erstellen Sie eine Azure-Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md), und geben Sie folgende Details an:

- Die Azure-Ressourcen-ID des betroffenen virtuellen Computers
- Die Azure-Ressourcen-ID des Arbeitsbereichs, der vor der Verbindungsunterbrechung für die Erweiterung konfiguriert war
- Den Agent und die zuvor installierte Version

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-the-billing-implications"></a>Installiert Security Center Lösungen in meinen vorhandenen OMS-Arbeitsbereichen? Was bedeutet das für die Abrechnung?
Wenn Security Center erkennt, dass ein virtueller Computer bereits mit einem Arbeitsbereich verbunden ist, den Sie erstellt haben, aktiviert Security Center Lösungen in diesem Arbeitsbereich gemäß Ihres Tarifs. Da die Lösungen mittels [Zielgruppenadressierung für Lösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting) nur auf die relevanten virtuellen Azure-Computer angewendet werden, bleibt die Abrechnung unverändert.

- **Free-Tarif:** Security Center installiert die Lösung „SecurityCenterFree“ im Arbeitsbereich. Für den Free-Tarif entstehen keine Kosten.
- **Standard-Tarif:** Security Center installiert die Lösungen „SecurityCenterFree“ und „Security“ im Arbeitsbereich.

   ![Lösungen im Standardarbeitsbereich][4]

> [!NOTE]
> Die Lösung „Security“ in Log Analytics ist die Lösung „Sicherheit und Überwachung“ in OMS.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>In meiner Umgebung sind bereits Arbeitsbereiche vorhanden. Kann ich diese zum Sammeln von Sicherheitsdaten verwenden?
Wenn der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf einem virtuellen Computer installiert ist, verwendet Security Center den vorhandenen verbundenen Arbeitsbereich. Eine Security Center-Lösung wird im Arbeitsbereich installiert, falls noch keine vorhanden ist, und die Lösung wird mittels [Zielgruppenadressierung für Lösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting) nur auf die relevanten virtuellen Computer angewendet.

Wenn Security Center den Microsoft Monitoring Agent auf virtuellen Computern installiert, werden dabei die von Security Center erstellten Standardarbeitsbereiche verwendet. In Kürze können Kunden die zu verwendenden Arbeitsbereiche konfigurieren.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Die Lösung „Security“ ist bereits in meinen Arbeitsbereichen enthalten. Was bedeutet das für die Abrechnung?
Die Lösung „Sicherheit und Überwachung“ dient zum Aktivieren von Security Center-Features des Standard-Tarifs für virtuelle Azure-Computer. Wenn die Lösung „Sicherheit und Überwachung“ bereits in einem Arbeitsbereich installiert ist, verwendet Security Center die vorhandene Lösung. Die Abrechnung wird dadurch nicht beeinflusst.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Security Center-Plattformmigration finden Sie in den folgenden Artikeln:

- [Azure Security Center – Plattformmigration](security-center-platform-migration.md)
- [Azure Security Center – Handbuch zur Problembehandlung](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png

