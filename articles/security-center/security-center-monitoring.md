---
title: "Sicherheitsüberwachung in Azure Security Center | Microsoft-Dokumentation"
description: "Diese Artikel bietet eine Einführung in die Überwachungsfunktionen von Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 4752199803f1fab04608e6a8618e3294e85fb38a
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="security-health-monitoring-in-azure-security-center"></a>Überwachen der Sicherheitsintegrität in Azure Security Center
Dieser Artikel unterstützt Sie bei der Überwachung der Richtliniencompliance mithilfe der Überwachungsfunktionen von Azure Security Center.

## <a name="what-is-security-health-monitoring"></a>Was ist Überwachen der Sicherheitsintegrität?
Überwachung wird häufig als Beobachten und Warten auf ein Eintreten eines Ereignisses aufgefasst, damit auf diese Situation reagiert werden kann. Sicherheitsüberwachung bezieht sich auf eine proaktive Strategie, bei der Ihre Ressourcen überwacht werden, um Systeme zu erkennen, die nicht den Unternehmensstandards oder bewährten Methoden entsprechen.

## <a name="monitoring-security-health"></a>Überwachung der Sicherheitsintegrität
Nach Aktivierung der [Sicherheitsrichtlinien](security-center-policies.md) für die Ressourcen eines Abonnements analysiert Security Center die Sicherheit Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Informationen zur Netzwerkkonfiguration sind sofort verfügbar. Abhängig davon, auf wie vielen Ihrer (virtuellen und physischen) Computer der Agent installiert ist, kann es eine Stunde oder länger dauern, bis die Konfigurationsinformationen für die Computer (beispielsweise Sicherheitsupdatestatus und Betriebssystemkonfiguration) gesammelt wurden und verfügbar sind. Der Sicherheitsstatus Ihrer Ressourcen sowie ggf. vorliegende Probleme werden im Abschnitt **Prävention** angezeigt. Eine Liste mit diesen Problemen steht auch auf der Kachel **Empfehlungen** zur Verfügung.

Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

Unter dem Abschnitt **Prävention** können Sie den Sicherheitszustand Ihrer Ressourcen überwachen. Im folgenden Beispiel sehen Sie, dass auf der Kacheln der jeweiligen Ressource (Compute, Netzwerk, Speicher und Daten sowie Anwendung) die Gesamtanzahl der identifizierten Probleme angegeben ist.

![Kachel „Sicherheitsintegrität der Ressource“](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute"></a>Überwachen von Compute
Wenn Sie auf die Kachel **Compute** klicken, werden drei Registerkarten angezeigt:

- **Übersicht:** Überwachung und Empfehlungen.
- **VMs und Computer:** Liste mit allen virtuellen und physischen Computern und dem jeweils aktuellen Sicherheitszustand.
- **Clouddienste:** Liste mit allen von Security Center überwachten Web- und Workerrollen.

![Fehlendes Systemupdate nach virtuellem Computer](./media/security-center-monitoring/security-center-monitoring-fig1-sep2017.png)

Auf den einzelnen Registerkarten können sich jeweils mehrere Abschnitte befinden, und in jedem Abschnitt können Sie eine einzelne Option auswählen, um weitere Details zu den empfohlenen Problembehandlungsschritten für ein bestimmtes Problem anzuzeigen. 

#### <a name="monitoring-recommendations"></a>Überwachen der Empfehlungen
Dieser Abschnitt enthält die Gesamtanzahl und den jeweils aktuellen Status der virtuellen und physischen Computer, die für die automatische Bereitstellung initialisiert wurden. Wenn Sie auf diesen Eintrag klicken, öffnet sich **VM-Agent fehlt oder reagiert nicht**. 

![Fehlendes Systemupdate nach virtuellem Computer](./media/security-center-monitoring/security-center-monitoring-fig1-new003-2017.png)


#### <a name="recommendations"></a>Recommendations
Dieser Abschnitt enthält eine Reihe von [Empfehlungen für die einzelnen virtuellen und physischen Computer](security-center-virtual-machine-recommendations.md), die von Azure Security Center überwacht werden. Die erste Spalte enthält die Empfehlung. Die zweite Spalte enthält die Gesamtanzahl virtueller und physischer Computer, die von dieser Empfehlung betroffen sind. Die dritte Spalte gibt Aufschluss über den Schweregrad des Problems, wie im folgenden Screenshot zu sehen:

![Empfehlungen für virtuelle Computer](./media/security-center-monitoring/security-center-monitoring-fig2-sep2017.png)

> [!NOTE]
> Unter **Networking Health** (Netzwerkintegrität) werden in der Liste **Netzwerktopologie** nur virtuelle Computer mit mindestens einem öffentlichen Endpunkt angezeigt.
>

Jeder Empfehlung ist eine Reihe von Aktionen zugeordnet, die ausgeführt werden können, wenn Sie darauf klicken. Wenn Sie also beispielsweise auf **Fehlende Systemupdates** klicken, erscheint eine Liste mit virtuellen und physischen Computern, auf denen Patches fehlen, und dem Schweregrad des fehlenden Updates, wie im folgenden Screenshot zu sehen:

![Fehlende Systemupdates für virtuelle Computer](./media/security-center-monitoring/security-center-monitoring-fig9-sep2017.png)

Für **Fehlende Systemupdates** werden zwei Diagramme mit einer Zusammenfassung kritischer Updates angezeigt (eins für Windows, eins für Linux). Der zweite Teil enthält eine Tabelle mit folgenden Informationen:

* **NAME**: Der Name des fehlenden Updates.
* **Anzahl von VMs und Computern**: Die Gesamtzahl virtueller und physischer Computer, auf denen dieses Update fehlt.
* **STATUS**: Der aktuelle Status der Empfehlung:
  * **Offen**: Die Empfehlung wurde noch nicht umgesetzt.
  * **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
  * **Behoben**: Die Empfehlung wurde bereits umgesetzt. (Wenn das Problem behoben wurde, wird der Eintrag abgeblendet dargestellt.)
* **SCHWEREGRAD**: Beschreibt den Schweregrad der jeweiligen Empfehlung:
  * **Hoch**: Ein Sicherheitsrisiko betrifft eine wichtige Ressource (Anwendung, virtueller Computer oder Netzwerksicherheitsgruppe) und erfordert eine Reaktion.
  * **Mittel**: Es liegt ein nicht kritisches Problem vor, oder es sind zusätzliche Schritte erforderlich, um einen Prozess abzuschließen oder eine Sicherheitslücke zu schließen.
  * **Niedrig**: Ein Sicherheitsrisiko, das behandelt werden sollte, jedoch keine unmittelbare Aufmerksamkeit erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)

Klicken Sie zum Anzeigen der Empfehlungsdetails in der Liste auf den Namen des fehlenden Updates. 

![Fehlende Systemupdates für einen bestimmten virtuellen Computer](./media/security-center-monitoring/security-center-monitoring-fig4-sep2017.png)

> [!NOTE]
> Hier werden die gleichen Sicherheitsempfehlungen aufgeführt wie unter der Option **Empfehlungen**. Weitere Informationen zur Anwendung von Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md). Das gilt nicht nur für virtuelle und physische Computer, sondern für alle Ressourcen, die auf der Kachel **Ressourcenintegrität** verfügbar sind.
>

#### <a name="vms--computers-section"></a>Abschnitt „VMs und Computer“
Der Abschnitt „VMs und Computer“ enthält eine Übersicht über alle Empfehlungen für virtuelle und physische Computer. Jede Spalte steht für eine Gruppe von Empfehlungen, wie im folgenden Screenshot zu sehen:

![Übersicht über alle virtuellen Computer und Empfehlungen](./media/security-center-monitoring/security-center-monitoring-fig5-sep2017.png)

Die Liste enthält vier Arten von Symbolen:

![icon1](./media/security-center-monitoring/security-center-monitoring-icon1.png) Azure-fremder Computer

![icon2](./media/security-center-monitoring/security-center-monitoring-icon2.png) Virtueller Azure Resource Manager-Computer

![icon3](./media/security-center-monitoring/security-center-monitoring-icon3.png) Klassischer virtueller Azure-Computer

![icon4](./media/security-center-monitoring/security-center-monitoring-icon4.png) Virtuelle Computer, die nur anhand des Arbeitsbereichs identifiziert werden, der dem angezeigten Abonnement angehört. Dazu zählen virtuelle Computer aus anderen Abonnements, die dem Arbeitsbereich in diesem Abonnement unterstellt sind, sowie virtuelle Computer, die mit dem direkten SCOM-Agent installiert wurden und über keine Ressourcen-ID verfügen.

Anhand des Symbols unter der jeweiligen Empfehlung sehen Sie sofort, bei welchem virtuellen oder physischen Computer eine Aktion erforderlich ist und um welche Art von Empfehlung es sich handelt. Mithilfe der Option **Filter** können Sie zudem auswählen, welche Optionen auf diesem Bildschirm angezeigt werden sollen.

![Filter](./media/security-center-monitoring/security-center-monitoring-fig6-sep2017.png)

Im vorherigen Beispiel liegt für einen einzelnen virtuellen Computer eine kritische Empfehlung im Zusammenhang mit dem Endpunktschutz vor. Weitere Informationen erhalten Sie, indem Sie auf den virtuellen Computer klicken:

![Sicherheitsdetails des virtuellen Computers](./media/security-center-monitoring/security-center-monitoring-fig7-sep2017.png)

Hier finden Sie die Sicherheitsdetails für den virtuellen oder physischen Computer. Im unteren Bereich des Blatts werden die empfohlene Aktion und der Schweregrad des jeweiligen Problems angezeigt.

#### <a name="cloud-services-section"></a>Abschnitt „Clouddienste“
Für Clouddienste wird eine Empfehlung erstellt, wenn die Betriebssystemversion nicht mehr aktuell ist, wie im folgenden Screenshot zu sehen:

![Integritätsstatus für Clouddienste](./media/security-center-monitoring/security-center-monitoring-fig1-new006-2017.png)

In einem Szenario, in dem eine Empfehlung vorliegt (was im vorherigen Beispiel nicht der Fall ist), muss die Betriebssystemversion mithilfe der Schritte aus der Empfehlung aktualisiert werden. Ist ein Update verfügbar, erhalten Sie eine Warnung (rot oder orange, je nach Schweregrad des Problems). Wenn Sie in „WebRole1“ (Ausführung von Windows Server mit automatischer Bereitstellung Ihrer Web-App für IIS) oder „WorkerRole1“ (Ausführung von Windows Server mit automatischer Bereitstellung Ihrer Web-App für IIS) auf diese Warnung klicken, werden weitere Details zur Empfehlung angezeigt, wie im folgenden Screenshot zu sehen:

![Clouddienstdetails](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Eine Erläuterung der Empfehlung erhalten Sie, wenn Sie in der Spalte **BESCHREIBUNG** auf **Betriebssystemversion aktualisieren** klicken. 

![Clouddienstempfehlungen](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Überwachen virtueller Netzwerke
Wenn Sie auf die Kachel **Netzwerk** klicken, wird das Blatt **Netzwerk** mit weiteren Details geöffnet, wie im folgenden Screenshot zu sehen:

![Blatt „Netzwerk“](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Netzwerkempfehlungen
Auch hier sehen Sie (ähnlich wie bei den Ressourcenintegritätsinformationen des virtuellen Computers) im oberen Bereich eine zusammengefasste Liste mit Problemen und im unteren Bereich eine Liste mit den überwachten Netzwerken.

Im Abschnitt für die Statusanalyse der Netzwerke sind potenzielle Sicherheitsprobleme und entsprechende [Empfehlungen](security-center-network-recommendations.md)aufgeführt. Beispiele für Probleme:

* Keine Firewall der nächsten Generation (Next Generation Firewall, NGFW) installiert
* Keine Netzwerksicherheitsgruppen für Subnetze aktiviert
* Keine Netzwerksicherheitsgruppen für virtuelle Computer aktiviert
* Eingeschränkter Zugriff über öffentliche externe Endpunkte
* Fehlerfreie Endpunkte mit Internetzugriff

Wenn Sie auf eine Empfehlung klicken, erscheinen weitere Details, wie im folgenden Beispiel gezeigt:

![Details für eine Empfehlung unter „Netzwerk“](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

In diesem Beispiel wird unter **Fehlende Netzwerksicherheitsgruppen für Subnetze konfigurieren** eine Liste mit Subnetzen und virtuellen Computern angezeigt, die nicht durch eine Netzwerksicherheitsgruppe geschützt sind. Wenn Sie auf das Subnetz klicken, auf das Sie die Netzwerksicherheitsgruppe anwenden möchten, erscheint **Netzwerksicherheitsgruppe auswählen**. Hier können Sie eine geeignete Netzwerksicherheitsgruppe für das Subnetz auswählen oder eine neue Netzwerksicherheitsgruppe erstellen.

#### <a name="internet-facing-endpoints-section"></a>Abschnitt „Endpunkte mit Internetanbindung“
Im Abschnitt **Endpunkte mit Internetzugriff** werden die virtuellen Computer, die derzeit mit einem Endpunkt mit Internetzugriff konfiguriert sind, und der jeweilige Status angezeigt.

![Mit einem Endpunkt mit Internetzugriff konfigurierte virtuelle Computer und Status](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Diese Tabelle enthält den Endpunktnamen, der den virtuellen Computer darstellt, die Internet-IP-Adresse und den aktuellen Status der Netzwerksicherheitsgruppe und der NGFW. Die Tabelle ist nach Schweregrad sortiert:

* Rot (oben): Hohe Priorität; sollte umgehend behoben werden.
* Orange: Mittlere Priorität; sollte zeitnah behoben werden.
* Grün (ganz unten): Fehlerfrei.

#### <a name="networking-topology-section"></a>Abschnitt „Netzwerktopologie“
Der Abschnitt **Netzwerktopologie** enthält eine hierarchische Ansicht der Ressourcen, wie im folgenden Screenshot zu sehen:

![Hierarchische Ansicht der Ressourcen im Abschnitt „Netzwerktopologie“](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Diese Tabelle ist nach Schweregrad sortiert (virtuelle Computer und Subnetze):

* Rot (oben): Hohe Priorität; sollte umgehend behoben werden.
* Orange: Mittlere Priorität; sollte zeitnah behoben werden.
* Grün (ganz unten): Fehlerfrei.

In dieser Hierarchie umfasst die erste Ebene [virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md), [virtuelle Netzwerkgateways](/vpn-gateway/vpn-gateway-site-to-site-create.md) und [virtuelle Netzwerke (klassisch)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). Die zweite Ebene umfasst Subnetze und die dritte Ebene die virtuellen Computer dieser Subnetze. Die rechte Spalte enthält den aktuellen Status der Netzwerksicherheitsgruppe für diese Ressourcen, wie im folgenden Beispiel zu sehen:

![Status der Netzwerksicherheitsgruppe im Abschnitt „Netzwerktopologie“](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Im unteren Bereich des Blatts befinden sich die Empfehlungen für diesen virtuellen Computer (ähnlich wie weiter oben beschrieben). Sie können auf eine Empfehlung klicken, um weitere Informationen zu erhalten oder die erforderliche Sicherheitskontrolle/-konfiguration anzuwenden.

### <a name="monitor-storage--data"></a>Überwachen von Speicher und Daten

Wenn Sie im Abschnitt **Prävention** auf **Speicher und Daten** klicken, wird **Datenressourcen** mit Empfehlungen für SQL und Speicher geöffnet. Außerdem werden [Empfehlungen](security-center-sql-service-recommendations.md) zum allgemeinen Integritätsstatus der Datenbank abgegeben. Weitere Informationen zur Speicherverschlüsselung finden Sie unter [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Aktivieren der Verschlüsselung für Azure-Speicherkonten in Azure Security Center).

![Datenressourcen](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

Sie können unter **SQL Recommendations** (SQL-Empfehlungen) auf eine Empfehlung klicken und ausführlichere Informationen zu weiteren Aktionen erhalten, um ein Problem zu beheben. Das folgende Beispiel zeigt die Erweiterung der Empfehlung **Database Auditing & Threat detection on SQL databases** (Datenbanküberwachung und Bedrohungserkennung bei SQL-Datenbanken).

![Details zu einer SQL-Empfehlung](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Unter **Überwachung und Bedrohungserkennung für SQL-Datenbanken aktivieren** werden folgende Informationen angezeigt:

* Liste der SQL-Datenbanken
* Server, auf dem sich eine Datenbank befindet
* Informationen dazu, ob die Einstellung vom Server geerbt wurde oder in der Datenbank eindeutig ist
* Aktueller Status
* Schweregrad des Problems

Wenn Sie auf die Datenbank klicken, um die Empfehlung zu implementieren, erscheint **Überwachung und Bedrohungserkennung**, wie im folgenden Screenshot zu sehen:

![Überwachung und Bedrohungserkennung](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Um die Überwachung zu aktivieren, wählen Sie für **Überwachung** die Option **EIN** aus.

### <a name="monitor-applications"></a>Überwachen von Anwendungen

Wenn Ihre Azure-Workload Anwendungen umfasst, die sich auf [(über Azure Resource Manager erstellten) virtuellen Computern](../azure-resource-manager/resource-manager-deployment-model.md) mit verfügbar gemachten Webports (TCP-Ports 80 und 443) befinden, kann Security Center diese Ports überwachen, um potenzielle Sicherheitsprobleme zu erkennen und Problembehandlungsschritte zu empfehlen. Wenn Sie auf die Kachel **Anwendungen** klicken, erscheint **Anwendungen** mit einer Reihe von Empfehlungen im Abschnitt **Anwendungsempfehlungen**. Darüber hinaus werden eine Anwendungsaufschlüsselung, die IP-Adresse/Domäne und der Installationsstatus einer WAF-Lösung angezeigt:

![Sicherheitsintegrität für Anwendungen](./media/security-center-monitoring/security-center-monitoring-fig8-sep2017.png)

Auch hier können Sie auf die Empfehlung klicken, um weitere Details zu einem Problem und Informationen zu dessen Behandlung anzuzeigen. Bei dem Beispiel in der folgenden Abbildung handelt es sich um eine Anwendung, die als unsichere Webanwendung identifiziert wurde. Wenn Sie die als unsicher eingestufte Anwendung auswählen, steht die folgende Option zur Verfügung:

![Details](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Hier finden Sie eine Liste mit allen Empfehlungen für diese Anwendung. Wenn Sie auf die Empfehlung **Web Application Firewall hinzufügen** klicken, wird **Web Application Firewall hinzufügen** mit Optionen zum Installieren einer WAF (Web Application Firewall) eines Partners geöffnet, wie im folgenden Screenshot zu sehen:

![Dialogfeld „Web Application Firewall hinzufügen“](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Siehe auch
In diesem Artikel haben Sie erfahren, wie Sie die Überwachungsfunktionen in Azure Security Center verwenden können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitseinstellungen in Azure Security Center konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

