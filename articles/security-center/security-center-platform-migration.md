---
title: "Azure Security Center – Plattformmigration | Microsoft-Dokumentation"
description: "Dieses Dokument erläutert einige Änderungen an der Art und Weise, wie Azure Security Center-Daten gesammelt werden."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: 5ddf71dcd9c5a2b03e3b1441d8c9b4d91b6bad12
ms.contentlocale: de-de
ms.lasthandoff: 07/24/2017

---
# <a name="azure-security-center-platform-migration"></a>Azure Security Center – Plattformmigration

Ab Anfang Juni 2017 werden in Azure Security Center grundlegende Änderungen an der Art und Weise vorgenommen, wie sicherheitsrelevante Daten gesammelt und gespeichert werden.  Diese Änderungen bringen auch neue Funktionen, z.B. die Möglichkeit einer einfachen Suche nach Sicherheitsdaten. Außerdem erfolgt eine bessere Anpassung an andere Azure-Dienste zur Verwaltung und Überwachung.

> [!NOTE]
> Die Plattformmigration sollte keine Auswirkungen auf Ihre Produktionsressourcen haben. Es sind auch keine Aktion Ihrerseits erforderlich.


## <a name="whats-happening-during-this-platform-migration"></a>Was geschieht während dieser Plattformmigration?

Bisher hat Security Center den Azure Monitoring Agent zum Sammeln von sicherheitsrelevanten Daten von Ihren virtuellen Computern verwendet. Dazu gehören auch Informationen zu Sicherheitskonfigurationen, mit denen die Sicherheitsrisiken identifiziert werden, sowie zu Sicherheitsereignissen, mit denen Bedrohungen erkannt werden. Diese Daten wurden in Ihren Speicherkonten in Azure gespeichert.

Ab sofort verwendet Security Center den Microsoft Monitoring Agent – dies ist derselbe Agent, den auch die Operations Management Suite und der Log Analytics-Dienst verwenden. Die von diesem Agent gesammelten Daten werden entweder in einem vorhandenen *Log Analytics*-[Arbeitsbereich](../log-analytics/log-analytics-manage-access.md), der mit Ihrem Azure-Abonnement verknüpft ist, oder unter Berücksichtigung des geografischen Standorts des virtuellen Computers in neuen Arbeitsbereichen gespeichert.

## <a name="agent"></a>Agent

Im Rahmen des Übergangs wird Microsoft Monitoring Agent (für [Windows](../log-analytics/log-analytics-windows-agents.md) oder [Linux](../log-analytics/log-analytics-linux-agents.md)) auf allen virtuellen Azure-Computern installiert, von denen zurzeit Daten erfasst werden.  Wenn auf dem virtuellen Computer bereits Microsoft Monitoring Agent installiert wurde, nutzt Security Center den installierten Agent.

Für eine bestimmte Zeit (in der Regel einige Tage) werden beide Agents parallel ausgeführt, um einen reibungslosen Übergang ohne Verlust von Daten sicherzustellen. Dadurch kann Microsoft prüfen, ob die neue Datenpipeline betriebsbereit ist, bevor die Verwendung der aktuellen Pipeline beendet wird. Nach der Überprüfung wird der Azure Monitoring Agent von Ihren virtuellen Computern entfernt. Es ist keine Aktion Ihrerseits erforderlich. Sie werden per E-Mail benachrichtigt, wenn alle Kunden migriert wurden.
 
Es wird nicht empfohlen, den Azure Monitoring Agent während der Migration manuell zu deinstallieren, da dies zu Lücken in den Sicherheitsdaten führen könnte. Wenden Sie sich an den [Microsoft-Kundendienst und -Support](https://support.microsoft.com/contactus/), wenn Sie weitere Hilfe benötigen. 

Der Microsoft Monitoring Agent für Windows verwendet den TCP-Port 443. Weitere Informationen finden Sie unter [Azure Security Center – Handbuch zur Problembehandlung](security-center-troubleshooting-guide.md).


> [!NOTE] 
> Da der Microsoft Monitoring Agent möglicherweise durch andere Azure-Dienste zur Verwaltung und Überwachung verwendet wird, wird der Agent nicht automatisch deinstalliert, wenn Sie die Datensammlung in Security Center deaktivieren. Allerdings können Sie den Agent bei Bedarf manuell deinstallieren.

## <a name="workspace"></a>Arbeitsbereich

Wie bereits erläutert werden die vom Microsoft Monitoring Agent (für Security Center) gesammelten Daten entweder in einem vorhandenen Log Analytics-Arbeitsbereich, der mit Ihrem Azure-Abonnement verknüpft ist, oder unter Berücksichtigung des geografischen Standorts des virtuellen Computers in neuen Arbeitsbereichen gespeichert.

Sie können das Azure-Portal durchsuchen, um eine Liste Ihrer Log Analytics-Arbeitsbereiche (einschließlich aller von Security Center erstellten) anzuzeigen. Für neue Arbeitsbereiche wird eine zugehörige Ressourcengruppe erstellt. Für beide gilt folgende Namenskonvention:

- Arbeitsbereich: *DefaultWorkspace-[Abonnement-ID]-[Region]*
- Ressourcengruppe: *DefaultResouceGroup-[Region]* 
 
Für Arbeitsbereiche, die von Security Center erstellt wurden, werden die Daten für 30 Tage beibehalten. Bei vorhandenen Arbeitsbereichen basiert die Vermerkdauer auf dem Tarif des Arbeitsbereichs.

> [!NOTE]
> Die zuvor von Security Center erfassten Daten verbleiben in Ihren Speicherkonten. Nach Abschluss der Migration können Sie diese Speicherkonten löschen.

### <a name="oms-security-solution"></a>OMS-Sicherheitslösung 

Für bestehende Kunden ohne installierte OMS-Sicherheitslösung erfolgt die Installation in ihrem Arbeitsbereich durch Microsoft, wobei ausschließlich Azure-VMs als Ziel verwendet werden. Deinstallieren Sie diese Lösung nicht, da keine automatische Wiederherstellung möglich ist, wenn dies über die OMS-Verwaltungskonsole ausgeführt wird.


## <a name="other-updates"></a>Andere Updates

In Verbindung mit der Plattformmigration veröffentlichen wir parallel noch einige zusätzliche, weniger wichtige Updates:

- Es werden zusätzliche Betriebssystemversionen unterstützt. Die Liste finden Sie [hier](security-center-faq.md#virtual-machines).
- Die Liste der Sicherheitsrisiken des Betriebssystems wird erweitert. Die Liste finden Sie [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Die [Preise](https://azure.microsoft.com/pricing/details/security-center/) werden stündlich abgerechnet (bisher täglich), was für einige Kunden zu Kosteneinsparungen führt.
- Die Datensammlung ist erforderlich und wird für Kunden im Tarif Standard automatisch aktiviert.
- Azure Security Center beginnt mit der Ermittlung von Antischadsoftware-Lösungen, die nicht über Azure-Erweiterungen bereitgestellt wurden. Die Ermittlung von Symantec Endpoint Protection und Defender für Windows 2016 wird als Erstes verfügbar sein.
- Vorbeugende Richtlinien und Benachrichtigungen können nur auf der *Abonnementebene* konfiguriert werden. Preise können hingegen weiterhin auf der *Ressourcengruppenebene* festgelegt werden.


