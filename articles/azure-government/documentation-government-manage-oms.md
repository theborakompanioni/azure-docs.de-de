---
title: "Operations Management Suite für Azure Government | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Log Analytics in der Operations Management Suite für die Behörden und Lösungsanbieter der US-Regierung angewendet werden kann."
services: azure-government
cloud: gov
documentationcenter: 
author: sacha
manager: jobruno
ms.assetid: 
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/05/2016
ms.author: saweld
translationtype: Human Translation
ms.sourcegitcommit: 35cefe7f394e0f332b7b6fd4ec1609ae04f5fc6f
ms.openlocfilehash: 84eabe4bb79262c47f530c59e36119bacc4115d7

---

# <a name="azure-government-cybersecurity-monitoring-and-securing-your-assets-with-operations-management-suite"></a>Azure Government-Internetsicherheit: Überwachen und Sichern Ihrer Assets mit der Operations Management Suite 

## <a name="cybersecurity-in-the-cloud"></a>Internetsicherheit in der Cloud
Beim Wechsel in die Cloud ist es von entscheidender Bedeutung für unsere Kunden, dass sie die Kontrolle über die Assetverwaltung und die Sicherheit der Azure Government-Dienste behalten, die sie in der Cloud bereitgestellt haben. Firewalls der virtuellen Computer müssen ordnungsgemäß konfiguriert werden. Auf die virtuellen Netzwerke müssen die richtigen Netzwerksicherheitsgruppen angewendet werden. Der Zugriff auf die Assets muss zum richtigen Zeitpunkt gesperrt werden. All diese notwendigen Arbeitsschritte müssen geplant, entwickelt und umgesetzt werden, um eine sichere Infrastruktur für Ihre Organisation zu ermöglichen.

Das Einrichten einer solchen Umgebung kann eine Herausforderung darstellen. Das Onboarding Ihrer Serverflotte in einen Überwachungsdienst ist eine ausgesprochen schwierige Aufgabe, und auch die Aktualisierung eines solchen Diensts kann eine Herausforderung sein. Auch die Überwachung der Infrastruktur für verschiedene Cloudanbieter und über die Cloud und die lokalen Standorte hinweg ist schwierig. Und nicht zuletzt müssen Sie die Überwachungsstruktur auf dem neuesten Stand halten und mithilfe von Azure Application Insights dafür sorgen, dass Sicherheitsbedrohungen aus dem Internet erkannt und gemeldet sowie geeignete Maßnahmen gegen diese ergriffen werden. All dies kostet Zeit, Ressourcen und Computeleistung.

## <a name="microsoft-operations-management-suite"></a>Microsoft Operations Management Suite
Die Microsoft Operations Management Suite – jetzt in Azure Government verfügbar – ist ein Satz von Azure-Diensten, mit denen Sie all diese Aufgaben schnell und einfach ausführen können. In diesem Artikel geht es schwerpunktmäßig um die Verwendung von Log Analytics für umfangreiche Protokollsuchen, damit Sie Daten schnell analysieren und Bedrohungen in Ihrer Umgebung schnell erkennen können.

Azure Log Analytics ermöglicht Folgendes:

* Bereitstellen von Agents auf einzelnen virtuellen Computern (unter Linux oder Windows) in Azure, für andere Cloudanbieter und lokal
* Zusammenführen Ihrer vorhandenen Protokolle mit vorhandenen Protokollierungsdaten über ein Azure Government-Speicherkonto oder einen System Center Operations Manager-Endpunkt

Sehen wir uns nun an, wie sich Log Analytics in die Serverflotte integrieren lässt, und untersuchen einige schlüsselfertige Lösungen, mit denen sich die hier beschriebenen Herausforderungen bewältigen lassen.

## <a name="onboarding-servers-to-log-analytics"></a>Onboarding von Servern in Log Analytics
Der erste Schritt bei der Integration Ihrer Cloudassets in Log Analytics besteht darin, den Log Analytics-Agent für alle Protokollquellen zu installieren. Bei virtuellen Computern ist das sehr einfach, weil Sie den Agent manuell aus dem Log Analytics-Portal herunterladen können.

![Abbildung 1: Mit der Operations Management Suite verbundene Windows-Server](./media/documentation-government-oms-figure1.png)
<p align="center">Abbildung 1: Mit Log Analytics verbundene Windows-Server</p>

Sie können virtuelle Azure-Computer direkt über das Azure-Portal mit Log Analytics verbinden. Anweisungen hierzu finden Sie unter [New ways to enable Log Analytics on your Azure VMs](https://blogs.technet.microsoft.com/momteam/2016/02/10/new-ways-to-enable-log-analytics-oms-on-your-azure-vms/) (Neue Möglichkeiten zum Aktivieren von Log Analytics auf Ihren virtuellen Azure-Computern).

Sie können die Computer auch programmgesteuert verbinden oder die VM-Erweiterung für Log Analytics direkt in Ihren Azure Resource Manager-Vorlagen konfigurieren. Anweisungen für Windows-basierte Computer finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents) und für Linux-basierte Computer unter [Verbinden von Linux-Computern mit Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-linux-agents).

## <a name="onboarding-storage-accounts-and-operations-manager-to-log-analytics"></a>Onboarding von Speicherkonten und Operations Manager in Log Analytics
Log Analytics kann auch eine Verbindung mit Ihrem Speicherkonto und/oder vorhandenen Bereitstellungen von System Center Operations Manager herstellen, sodass Sie Operations Management-Funktionen auch in hybriden Szenarien nutzen können (z.B. bei mehreren Cloudanbietern oder in gemischten lokalen/cloudbasierten Infrastrukturen).

![Abbildung 2: Verbinden von Azure Storage und Operations Manager mit Log Analytics](./media/documentation-government-oms-figure2.png)
<p align="center">Abbildung 2: Verbinden von Azure Storage und Operations Manager mit Log Analytics</p>

Log Analytics unterstützt außerdem das Protokollieren von Informationen aus anderen Überwachungsdiensten wie z.B. Chef oder Puppet. Darüber hinaus stehen für Azure-Bereitstellungen auch virtuelle Computer mit Log Analytics-fähigen Azure Resource Manager-Vorlagen bereit. So können Sie die Bereitstellung von Computeressourcen und das Onboarding in Ihrem Log Analytics-Arbeitsbereich gleichzeitig durchführen.

![Abbildung 3: Azure Resource Manager-Vorlagen für virtuelle Azure-Computer mit Log Analytics-Erweiterung](./media/documentation-government-oms-figure3a.png)
![Abbildung 3: Azure Resource Manager-Vorlagen für virtuelle Azure-Computer mit Log Analytics-Erweiterung](./media/documentation-government-oms-figure3b.png)
<p align="center">Abbildung 3: Azure Resource Manager-Vorlagen für virtuelle Azure-Computer mit Log Analytics-Erweiterung</p>

Informationen zum lokalen Einrichten von Log Analytics für Ihre vorhandene Operations Manager-Implementierung finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-om-agents).

## <a name="applying-intelligence-through-operations-management-suite-solution-packs"></a>Anwenden von intelligenten Funktionen über die Operations Management Suite-Lösungspakete
Sie verfügen über eine Vielzahl von Quellen zum Protokollieren von Daten – nun müssen Sie Erkenntnisse und wichtige Informationen aus diesen Daten ziehen.

Log Analytics ist im Kern ein Protokollsuchdienst, mit dem Sie leistungsstarke Abfragen schreiben können, um Tausende oder sogar Millionen von Protokollen schnell zu durchsuchen. Es kann jedoch ziemlich schwierig sein, die Probleme zu erkennen, für die Sie die Abfragen schreiben müssen.

Hier kommen die Operations Management Suite-Lösungen ins Spiel. Es handelt sich hierbei um Abfragepakete, die nativ in Log Analytics integriert sind und Ihnen proaktiv Einblicke in Ihre mit Log Analytics verwalteten Komponenten bieten.

Zum Thema Internetsicherheit erläutere ich kurz drei Szenarien, die Log Analytics ohne weiteren Konfigurationsaufwand für Sie lösen kann.

### <a name="antimalware-assessment"></a>Schadsoftwarebewertung
Schadsoftwarebewertungen bieten einen vorgefertigten Satz an Abfragen, Benachrichtigungen und Überwachungsdashboards, mit denen Sie auf einen Blick erkennen können, wie gut Ihr Bestand gegen Schadsoftware geschützt ist.

Dieses Dashboard bietet vier Arten von Informationen:
* Server mit aktiven und/oder behobenen Bedrohungen.
* Aktuell erkannte Bedrohungen.
* Computer, die nicht ausreichend geschützt sind. Um diese Informationen zu ermitteln, durchsucht Log Analytics die Protokolle Ihrer Computer nach Standorten mit geöffneten Firewalls oder nicht ordnungsgemäß konfigurierten Regeln in gängigen Webbrowsern.
* Eine Analyse des aktuellen Schutzzustands Ihrer Server, z.B. durch den nativen Virenschutz Ihrer Windows-Betriebssysteme oder durch eine Lösung wie System Center Endpoint Protection.

Hier können Sie z.B. sehen, dass die folgende Bedrohung von System Center erkannt und automatisch eingegrenzt wurde:

![Abbildung 4: Operations Management Suite-Lösung für die Schadsoftwarebewertung](./media/documentation-government-oms-figure4.png)
<p align="center">Abbildung 4: Operations Management Suite-Lösung für die Schadsoftwarebewertung</p>

Weitere Informationen zur Schadsoftwarebewertung finden Sie im Artikel [Lösung zur Bewertung von Schadsoftware in Log Analytics](https://azure.microsoft.com/en-us/documentation/articles/log-analytics-malware/).

### <a name="identity-and-access"></a>Identität und Zugriff
In einem weiteren häufigen Szenario der Internetsicherheit in der Cloud geht es um kompromittierte Anmeldeinformationen. Nicht nur Ihr Cloudabonnement ist mit Anmeldeinformationen verknüpft, sondern jedem einzelnen virtuellen Computer ist ein Benutzername und/oder ein Geheimnis (üblicherweise ein Zertifikat oder ein Kennwort) zugeordnet.

Log Analytics strukturiert alle Anmeldeversuche auf den Geräten in Ihrem Bestand und speichert sie nach Typ (remote, lokal, Benutzername usw.). Das folgende Beispiel zeigt eine große Anzahl nicht erfolgreicher Anmeldeversuche mithilfe größtenteils zufälliger Zeichenfolgen als Benutzernamen. Dies weist darauf hin, dass meine Computer höchstwahrscheinlich angreifbar und nicht ordnungsgemäß durch Firewalls und Zugriffssteuerungslisten geschützt sind.

![Abbildung 5: 97,3% fehlerhafte Anmeldeversuche in den letzten 24 Stunden](./media/documentation-government-oms-figure5.png)
<p align="center">Abbildung 5: 97,3% fehlerhafte Anmeldeversuche in den letzten 24 Stunden</p>

### <a name="threat-intelligence"></a>Bedrohungsanalyse
Log Analytics bietet darüber hinaus auch Schutz in Szenarien, bei denen sich der Angreifer innerhalb Ihrer Organisation befindet. Beispielsweise könnte ein böswilliger Benutzer versuchen, Daten aus Ihrer Organisation zu schleusen.

Die Bedrohungsanalysefunktion in Log Analytics untersucht alle Netzwerkprotokolle auf Ihrem Computer und benachrichtigt Sie automatisch, wenn eingehende oder ausgehende Netzwerkverbindungen von bzw. mit bekanntermaßen schädlichen IPs (z.B. IP-Adressen aus dem nicht indizierten Darknet) erkannt werden.

Der folgende Screenshot zeigt sowohl eingehende als auch ausgehende Netzwerkverbindungen mit der Volksrepublik China.

Nach einem Doppelklick auf das Tag für die eingehende Verbindung ist zu erkennen, dass ein über Log Analytics verwalteter virtueller Linux-Computer ausgehende Verbindungen mit einer bekannten Darknet-IP-Adresse in China herstellt.

Sie können auch Benachrichtigungen in Operations Management Suite-Lösungen einrichten, beispielsweise für die Bedrohungsanalyse. Im folgenden Screenshot wurde eine Benachrichtigung eingerichtet, sodass eine E-Mail gesendet wird, wenn Log Analytics mehr als 10 ausgehende Verbindungen mit einer bekannten böswilligen IP-Adresse erkennt. Diese Warnung wurde so konfiguriert, dass ein Azure Automation-Auftrag ausgelöst wird, der den betroffenen virtuellen Computer automatisch herunterfährt.

![Abbildung 6: Operations Management Suite – Benachrichtigungen und Automatisierung](./media/documentation-government-oms-figure6.png)
<p align="center">Abbildung 6: Operations Management Suite – Benachrichtigungen und Automatisierung</p>

Dies ist nur ein Beispiel für eine sofort einsatzbereite Operations Management Suite-Lösung, die auf Ihre Komponenten angewendet werden kann – unabhängig davon, ob diese in Azure, bei einem anderen Clouddienstanbieter oder lokal in Ihrem Standort ausgeführt werden.

Die Operations Management Suite aktualisiert ihre Machine Learning-Funktionen fortlaufend, um automatisch die neuesten Bedrohungen bekämpfen zu können, und es werden regelmäßig neue Lösungen im Azure Marketplace bereitgestellt.

Weitere Informationen zur Operations Management Suite finden Sie auf [unserer Dokumentationsseite](https://azure.microsoft.com/en-us/documentation/articles/documentation-government-overview/).



<!--HONumber=Jan17_HO1-->


