---
title: "Überwachen der Sicherheitsintegrität in Azure Security Center | Microsoft Docs"
description: "Diese Artikel bietet eine Einführung in die Überwachungsfunktionen von Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: a31a1b7909ea7440780c9a7af4cb908c9aa6a449
ms.openlocfilehash: 5852808ad0f5cf02908df7ff969ab2106d9f5abd


---
# <a name="security-health-monitoring-in-azure-security-center"></a>Überwachen der Sicherheitsintegrität in Azure Security Center
Dieser Artikel unterstützt Sie bei der Überwachung der Richtliniencompliance mithilfe der Überwachungsfunktionen von Azure Security Center.

## <a name="what-is-security-health-monitoring"></a>Was ist Überwachen der Sicherheitsintegrität?
Überwachung wird häufig als Beobachten und Warten auf ein Eintreten eines Ereignisses aufgefasst, damit auf diese Situation reagiert werden kann. Sicherheitsüberwachung bezieht sich auf eine proaktive Strategie, bei der Ihre Ressourcen überwacht werden, um Systeme zu erkennen, die nicht den Unternehmensstandards oder bewährten Methoden entsprechen.

## <a name="monitoring-security-health"></a>Überwachung der Sicherheitsintegrität
Nach Aktivierung der [Sicherheitsrichtlinien](security-center-policies.md) für die Ressourcen eines Abonnements analysiert Security Center die Sicherheit Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Informationen zur Netzwerkkonfiguration sind sofort verfügbar. Es kann jedoch eine Stunde oder länger dauern, bis Informationen zur VM-Konfiguration (wie Sicherheitsupdatestatus und Betriebssystemkonfiguration) zur Verfügung stehen. Der Sicherheitsstatus Ihrer Ressourcen sowie ggf. vorliegende Probleme werden auf dem Blatt **Sicherheitsintegrität der Ressource** angezeigt. Eine Liste mit diesen Problemen steht auch auf dem Blatt **Empfehlungen** zur Verfügung.

Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

Auf der Kachel **Sicherheitsintegrität der Ressource** können Sie den Sicherheitsstatus Ihrer Ressourcen überwachen. Im folgenden Beispiel sehen Sie eine Reihe von Problemen mit hohem und mittlerem Schweregrad, die eine Reaktion erfordern. Die Sicherheitsrichtlinien, die aktiviert sind, wirken sich auf die Arten von Sicherheitsmechanismen aus, die überwacht werden.

![Kachel „Sicherheitsintegrität der Ressource“](./media/security-center-monitoring/security-center-monitoring-fig1-new4.png)

Erkennt Security Center ein Sicherheitsrisiko, das behoben werden muss (beispielsweise einen virtuellen Computer mit fehlenden Sicherheitsupdates oder ein Subnetz ohne [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md)), wird dies hier angegeben.

### <a name="monitor-virtual-machines"></a>Überwachen virtueller Maschinen
Wenn Sie auf der Kachel **Sicherheitsintegrität der Ressource** auf **Virtuelle Computer** klicken, wird das Blatt **Virtuelle Computer** geöffnet. Dieses enthält weitere Details zum Onboarding sowie Vorbeugungsschritte und eine Liste mit allen virtuellen Computern, die von Security Center überwacht werden (wie im folgenden Screenshot zu sehen).

![Fehlendes Systemupdate nach virtuellem Computer](./media/security-center-monitoring/security-center-monitoring-fig2-ga.png)

* Schritte zur Integration
* Empfehlungen für virtuelle Computer
* Virtuelle Computer

In jedem Abschnitt können Sie eine einzelne Option auswählen, um weitere Details zu empfohlenen Problembehandlungsschritten anzuzeigen. Diese Bereiche werden in den folgenden Abschnitten ausführlicher erläutert.

#### <a name="monitoring-recommendations"></a>Überwachen der Empfehlungen
In diesem Abschnitt werden die Gesamtanzahl und der aktuelle Status der virtuellen Computer angezeigt, die für die Datensammlung initialisiert wurden. Sobald die Datensammlung für alle virtuellen Computer initialisiert wurde, sind sie bereit für den Empfang der Security Center-Sicherheitsrichtlinien. Wenn Sie auf diesen Eintrag klicken, wird das Blatt **Installationsstatus der Datensammlung** angezeigt. Hier sehen Sie die Namen der virtuellen Computer und den aktuellen Status der Datensammlung in der Spalte **INSTALLATIONSSTATUS**, wie im folgenden Screenshot zu sehen.

![Initialisierungsstatus von virtuellen Computern](./media/security-center-monitoring/security-center-monitoring-fig3-ga.png)

#### <a name="virtual-machine-recommendations"></a>Empfehlungen für virtuelle Computer
Dieser Abschnitt enthält eine Reihe von [Empfehlungen für die einzelnen virtuellen Computer](security-center-virtual-machine-recommendations.md), die von Azure Security Center überwacht werden. Die erste Spalte enthält die Empfehlung. Die zweite Spalte enthält die Gesamtanzahl virtueller Computer, die von dieser Empfehlung betroffen sind. Die dritte Spalte gibt Aufschluss über den Schweregrad des Problems, wie im folgenden Screenshot zu sehen.

![Empfehlungen für virtuelle Computer](./media/security-center-monitoring/security-center-monitoring-fig4-ga.png)

> [!NOTE]
> Auf dem Blatt **Networking Health** (Netzwerkintegrität) werden in der Liste **Netzwerktopologie** nur virtuelle Computer mit mindestens einem öffentlichen Endpunkt angezeigt.
> 
> 

Jeder Empfehlung ist eine Reihe von Aktionen zugeordnet, die ausgeführt werden können, wenn Sie darauf klicken. Wenn Sie beispielsweise auf **Fehlende Systemupdates** klicken, wird das Blatt **Fehlende Systemupdates** geöffnet. Auf dem Blatt sind die virtuellen Computer mit fehlenden Patches sowie der Schweregrad des fehlenden Updates aufgeführt, wie im folgenden Screenshot zu sehen.

![Fehlende Systemupdates für virtuelle Computer](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

Auf dem Blatt **Fehlende Systemupdates** befindet sich eine Tabelle mit folgenden Informationen:

* **VIRTUELLER COMPUTER**: Der Name der virtuellen Maschine, für die Updates fehlen.
* **SYSTEMUPDATES**: Die Anzahl der fehlenden Systemupdates.
* **LETZTE ÜBERPRÜFUNGSZEIT**: Der Zeitpunkt, zu dem der virtuelle Computer zuletzt von Security Center auf Updates überprüft wurde.
* **STATUS**: Der aktuelle Status der Empfehlung:
  * **Offen**: Die Empfehlung wurde noch nicht umgesetzt.
  * **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
  * **Behoben**: Die Empfehlung wurde bereits umgesetzt. (Wenn das Problem behoben wurde, wird der Eintrag abgeblendet dargestellt.)
* **SCHWEREGRAD**: Beschreibt den Schweregrad der jeweiligen Empfehlung:
  * **Hoch**: Ein Sicherheitsrisiko betrifft eine wichtige Ressource (Anwendung, virtueller Computer oder Netzwerksicherheitsgruppe) und erfordert eine Reaktion.
  * **Mittel**: Es liegt ein nicht kritisches Problem vor, oder es sind zusätzliche Schritte erforderlich, um einen Prozess abzuschließen oder eine Sicherheitslücke zu schließen.
  * **Niedrig**: Ein Sicherheitsrisiko, das behandelt werden sollte, jedoch keine unmittelbare Aufmerksamkeit erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)

Um die Details zur Empfehlung anzuzeigen, klicken Sie auf den Namen des jeweiligen virtuellen Computers. Für den virtuellen Computer öffnet sich ein neues Blatt mit einer Updateliste, wie im folgenden Screenshot zu sehen.

![Fehlende Systemupdates für einen bestimmten virtuellen Computer](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> Hier werden die gleichen Sicherheitsempfehlungen aufgeführt wie auf dem Blatt **Empfehlungen**. Weitere Informationen zur Anwendung von Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md). Dies gilt nicht nur für virtuelle Computer, sondern für alle Ressourcen, die auf der Kachel **Ressourcenintegrität** verfügbar sind.
> 
> 

#### <a name="virtual-machines-section"></a>Abschnitt „Virtuelle Computer“
Der Abschnitt „Virtuelle Computer“ enthält eine Übersicht über alle virtuellen Computer und Empfehlungen. Jede Spalte steht für eine Gruppe von Empfehlungen, wie im folgenden Screenshot zu sehen:

![Übersicht über alle virtuellen Computer und Empfehlungen](./media/security-center-monitoring/security-center-monitoring-fig7-ga.png)

Anhand der Symbole, die unter jeder Empfehlung angezeigt werden, können Sie schnell erkennen, bei welchen virtuellen Computern eine Aktion erforderlich ist und um welche Art von Empfehlung es sich handelt.

Im vorherigen Beispiel liegt für einen einzelnen virtuellen Computer eine kritische Empfehlung im Zusammenhang mit dem Endpunktschutz vor. Weitere Informationen erhalten Sie, indem Sie auf den virtuellen Computer klicken. Daraufhin öffnet sich ein neues Blatt für diesen virtuellen Computer, wie im folgenden Screenshot zu sehen.

![Sicherheitsdetails des virtuellen Computers](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Dieses Blatt enthält die Sicherheitsdetails für den virtuellen Computer. Im unteren Bereich des Blatts werden jeweils die empfohlene Aktion und der Schweregrad des jeweiligen Problems angezeigt.

#### <a name="cloud-services-preview-section"></a>Abschnitt „Clouddienste (Vorschau)“
Der Integritätsstatus für Clouddienste ist auf der Kachel **Sicherheitsintegrität** des virtuellen Computers angegeben. Eine Empfehlung wird erstellt, wenn die Betriebssystemversion nicht mehr aktuell ist, wie im folgenden Screenshot zu sehen:

![Integritätsstatus für Clouddienste](./media/security-center-monitoring/security-center-monitoring-fig8-new2.png)

Sie müssen die Schritte in der Empfehlung ausführen, um die Betriebssystemversion zu aktualisieren. Wenn Sie in „WebRole1“ (Ausführung von Windows Server mit automatischer Bereitstellung Ihrer Web-App für IIS) oder „WorkerRole1“ (Ausführung von Windows Server mit automatischer Bereitstellung Ihrer Web-App für IIS) auf die rote Warnung klicken, erscheint ein neues Blatt mit weiteren Details zur Empfehlung, wie im folgenden Screenshot zu sehen:

![Clouddienstdetails](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Eine Erläuterung der Empfehlung erhalten Sie, wenn Sie in der Spalte **BESCHREIBUNG** auf **Betriebssystemversion aktualisieren** klicken. Daraufhin öffnet sich das Blatt **Betriebssystemversion aktualisieren (Vorschau)** mit weiteren Details.

![Clouddienstempfehlungen](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Überwachen virtueller Netzwerke
Wenn Sie auf der Kachel **Sicherheitsintegrität der Ressource** auf **Netzwerk** klicken, wird das Blatt **Netzwerk** mit weiteren Details geöffnet, wie im folgenden Screenshot zu sehen:

![Blatt „Netzwerk“](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Netzwerkempfehlungen
Dieses Blatt enthält (ähnlich wie bei den Ressourcenintegritätsinformationen des virtuellen Computers) im oberen Bereich eine zusammengefasste Liste mit Problemen und im unteren Bereich eine Liste mit den überwachten Netzwerken.

Im Abschnitt für die Statusanalyse der Netzwerke sind potenzielle Sicherheitsprobleme und entsprechende [Empfehlungen](security-center-network-recommendations.md)aufgeführt. Beispiele für Probleme:

* Keine Firewall der nächsten Generation (Next Generation Firewall, NGFW) installiert
* Keine Netzwerksicherheitsgruppen für Subnetze aktiviert
* Keine Netzwerksicherheitsgruppen für virtuelle Computer aktiviert
* Eingeschränkter Zugriff über öffentliche externe Endpunkte
* Fehlerfreie Endpunkte mit Internetzugriff

Wenn Sie auf eine Empfehlung klicken, öffnet sich ein neues Blatt mit weiteren Details, wie im folgenden Beispiel gezeigt.

![Details für eine Empfehlung auf dem Blatt „Netzwerk“](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

In diesem Beispiel befindet sich auf dem Blatt **Fehlende Netzwerksicherheitsgruppen für Subnetze konfigurieren** eine Liste mit Subnetzen und virtuellen Computern, die nicht durch eine Netzwerksicherheitsgruppe geschützt sind. Wenn Sie auf das Subnetz klicken, auf das Sie die Netzwerksicherheitsgruppe anwenden möchten, öffnet sich ein weiteres Blatt.

Auf dem Blatt **Netzwerksicherheitsgruppe auswählen** können Sie eine geeignete Netzwerksicherheitsgruppe für das Subnetz auswählen oder eine neue Netzwerksicherheitsgruppe erstellen.

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

In dieser Hierarchie umfasst die erste Ebene [virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md), [virtuelle Netzwerkgateways](../vpn-gateway/vpn-gateway-site-to-site-create.md) und [virtuelle Netzwerke (klassisch)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Die zweite Ebene umfasst Subnetze und die dritte Ebene die virtuellen Computer dieser Subnetze. Die rechte Spalte enthält den aktuellen Status der Netzwerksicherheitsgruppe für diese Ressourcen, wie im folgenden Beispiel zu sehen:

![Status der Netzwerksicherheitsgruppe im Abschnitt „Netzwerktopologie“](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Im unteren Bereich des Blatts befinden sich die Empfehlungen für diesen virtuellen Computer (ähnlich wie weiter oben beschrieben). Sie können auf eine Empfehlung klicken, um weitere Informationen zu erhalten oder die erforderliche Sicherheitskontrolle/-konfiguration anzuwenden.

### <a name="monitor-data"></a>Überwachen von Daten
Wenn Sie auf der Kachel **Sicherheitsintegrität der Ressource** auf **Daten** klicken, wird das Blatt **SQL** geöffnet. Es enthält Empfehlungen zu Problemen, die mit der Überwachung oder der nicht aktivierten Transparent Data Encryption zusammenhängen. Außerdem werden [Empfehlungen](security-center-sql-service-recommendations.md) zum allgemeinen Integritätsstatus der Datenbank abgegeben.

![SQL-Ressourcenintegrität](./media/security-center-monitoring/security-center-monitoring-fig13-ga.png)

Sie können auf eine Empfehlung klicken und ausführlichere Informationen zu weiteren Aktionen erhalten, um ein Problem zu beheben. Das folgende Beispiel zeigt die Erweiterung der Empfehlung **Datenbanküberwachung nicht aktiviert**.

![Details zu einer SQL-Empfehlung](./media/security-center-monitoring/security-center-monitoring-fig14-ga.png)

Das Blatt **Überwachung für SQL-Datenbanken aktivieren** enthält folgende Informationen:

* Liste der SQL-Datenbanken
* Server, auf dem sich eine Datenbank befindet
* Informationen dazu, ob die Einstellung vom Server geerbt wurde oder in der Datenbank eindeutig ist
* Aktueller Status
* Schweregrad des Problems

Wenn Sie auf die Datenbank klicken, um die Empfehlung umzusetzen, erscheint das Blatt **Überwachung und Bedrohungserkennung**, wie im folgenden Screenshot zu sehen.

![Blatt „Überwachung und Bedrohungserkennung“](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Um die Überwachung zu aktivieren, wählen Sie für **Überwachung** die Option **EIN** aus.

### <a name="monitor-applications"></a>Überwachen von Anwendungen
Wenn Ihre Azure-Workload Anwendungen umfasst, die sich auf [(über Azure Resource Manager erstellten) virtuellen Computern](../azure-resource-manager/resource-manager-deployment-model.md) mit verfügbar gemachten Webports (TCP-Ports 80 und 443) befinden, kann Security Center diese Ports überwachen, um potenzielle Sicherheitsprobleme zu erkennen und Problembehandlungsschritte zu empfehlen. Wenn Sie auf die Kachel **Anwendungen** klicken, wird das Blatt **Anwendungen** mit einer Reihe von Empfehlungen im Abschnitt **Vorbeugungsschritte** geöffnet. Außerdem werden die Anwendungen nach Host/virtueller IP aufgeschlüsselt, wie im folgenden Screenshot zu sehen.

![Sicherheitsintegrität für Anwendungen](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Auch hier können Sie auf eine Empfehlung klicken, um weitere Details zu einem Problem und Informationen zu dessen Behandlung anzuzeigen. Bei dem Beispiel in der folgenden Abbildung handelt es sich um eine Anwendung, die als unsichere Webanwendung identifiziert wurde. Wenn Sie die als unsicher eingestufte Anwendung auswählen, wird ein weiteres Blatt mit der folgenden Option geöffnet:

![Details zu einer unsicheren App](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Dieses Blatt enthält eine Liste mit allen Empfehlungen für diese Anwendung. Wenn Sie auf die Empfehlung **Web Application Firewall hinzufügen** klicken, wird das Blatt **Web Application Firewall hinzufügen** mit Optionen zum Installieren einer WAF (Web Application Firewall) eines Partners geöffnet, wie im folgenden Screenshot zu sehen.

![Dialogfeld „Web Application Firewall hinzufügen“](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Siehe auch
In diesem Artikel haben Sie erfahren, wie Sie die Überwachungsfunktionen in Azure Security Center verwenden können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitseinstellungen in Azure Security Center konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.




<!--HONumber=Nov16_HO4-->


