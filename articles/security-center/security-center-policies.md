<properties
   pageTitle="Festlegen von Sicherheitsrichtlinien in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument enthält Informationen dazu, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren können."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="yurid"/>

# Festlegen von Sicherheitsrichtlinien in Azure Security Center
Dieses Dokument hilft Ihnen dabei, Sicherheitsrichtlinien in Security Center zu konfigurieren. Es werden die erforderlichen Schritte zum Ausführen dieser Aufgabe beschrieben.

## Was sind Sicherheitsrichtlinien?
In einer Sicherheitsrichtlinie wird der Satz von Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement oder der angegebenen Ressourcengruppe zu empfehlen sind. In Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements oder -Ressourcengruppen gemäß den Sicherheitsanforderungen Ihres Unternehmens sowie gemäß der Art von Anwendungen oder der Vertraulichkeit der Daten in den einzelnen Abonnements.

So gelten beispielsweise für Ressourcen, die zu Entwicklungs- oder Testzwecken verwendet werden, unter Umständen andere Sicherheitsanforderungen als für Ressourcen, die für Produktionsanwendungen verwendet werden. Ähnlich kann für Anwendungen mit reglementierten Daten, etwa personenbezogene Informationen (Personally Identifiable Information), eine höhere Sicherheitsstufe erforderlich sein. Die in Azure Security Center aktivierten Sicherheitsrichtlinien dienen zum Steuern der Sicherheitsempfehlungen und der Überwachung, um potenzielle Sicherheitsrisiken identifizieren und Bedrohungen entschärfen zu können. Weitere Informationen dazu, wie Sie die für Sie am besten geeignete Option ermitteln können, finden Sie unter [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md).

## Festlegen von Sicherheitsrichtlinien für Abonnements

Sicherheitsrichtlinien können für jedes Abonnement oder jede Ressourcengruppe konfiguriert werden. Damit Sie eine Sicherheitsrichtlinie ändern können, müssen Sie ein Besitzer oder Mitwirkender dieses Abonnements sein. Öffnen Sie das Azure-Portal, und führen Sie die folgenden Schritte aus, um Sicherheitsrichtlinien in Security Center zu konfigurieren:

1. Klicken Sie auf dem Security Center-Dashboard auf die Kachel **Richtlinie**.

2. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Richtlinie pro Abonnement oder Ressourcengruppe festlegen**, das auf der rechten Seite geöffnet wird, das Abonnement aus, für das Sie die Sicherheitsrichtlinie aktivieren möchten. Wenn Sie die Sicherheitsrichtlinie nicht für das gesamte Abonnement, sondern nur für eine Ressourcengruppe aktivieren möchten, scrollen Sie zum nächsten Abschnitt, in dem das Einrichten von Sicherheitsrichtlinien für Ressourcengruppen erläutert wird.

    ![Definieren von Richtlinien](./media/security-center-policies/security-center-policies-fig1-ga.png)

3. Das Blatt **Sicherheitsrichtlinie** für dieses Abonnement wird geöffnet. Es enthält eine Reihe von Optionen und sieht in etwa wie folgt aus:

    ![Aktivieren von Datensammlung](./media/security-center-policies/security-center-policies-fig2-ga.png)

4. Stellen Sie sicher, dass die Option **Collect data from virtual machines** auf **Ein** festgelegt ist. Diese Option aktiviert automatische Protokollsammlung für vorhandene und neue Ressourcen.

    >[AZURE.NOTE] Es empfiehlt sich, die Datensammlung für alle Ihre Abonnements zu aktivieren, damit die Sicherheitsüberwachung für alle vorhandenen und neuen virtuellen Computer verfügbar ist. Wird Datensammlung aktiviert, wird der Überwachungs-Agent installiert. Wenn Sie Datensammlung momentan von diesem Standort nicht aktivieren möchten, können Sie dies später über die Integritäts- und Empfehlungsansichten tun. Sie können die Datensammlung auch nur für das Abonnement oder für ausgewählte virtuelle Computer aktivieren. Weitere Informationen zu den unterstützten virtuellen Computern finden Sie unter [Azure Security Center – häufig gestellte Fragen](security-center-faq.md).

5. Ist Ihr Speicherkonto noch nicht konfiguriert, wird beim Öffnen der **Sicherheitsrichtlinie** möglicherweise eine Warnung angezeigt, die in etwa wie folgt aussieht:

    ![Speicherauswahl](./media/security-center-policies/security-center-policies-fig2.png)

6. Sollte diese Warnung angezeigt werden, klicken Sie auf die angegebene Option, und wählen Sie die Region aus, wie im folgenden Screenshot zu sehen:

    ![Speicherauswahl](./media/security-center-policies/security-center-policies-fig3-ga.png)

7. Wählen Sie für jede Region, in der Sie virtuelle Computer ausführen, ein Speicherkonto, in dem Daten gespeichert werden, die von diesen virtuellen Computer gesammelt wurden. Dies macht es einfach für Sie, Daten aus Datenschutz- und Datenhoheitszwecken im selben geografischen Gebiet zu speichern. Nachdem Sie entschieden haben, welche Region verwendet werden soll, wählen Sie die Region dann das Speicherkonto aus.

8. Klicken Sie auf dem Blatt **Choose storage accounts** auf **OK**.

    > [AZURE.NOTE] Bei Bedarf können Sie Daten von virtuellen Computern in verschiedenen Regionen in einem zentralen Speicherkonto aggregieren. Weitere Informationen hierzu finden Sie unter [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).

9. Klicken Sie auf dem Blatt **Sicherheitsrichtlinie** auf **Ein**, um die Sicherheitsempfehlungen zu aktivieren, die Sie für dieses Abonnement verwenden möchten. Klicken Sie auf die Option **Prevention policy** (Präventionsrichtlinie). Das Blatt **Sicherheitsrichtlinie** wird geöffnet, wie im folgenden Screenshot zu sehen:

	![Auswählen der Sicherheitsrichtlinien](./media/security-center-policies/security-center-policies-fig4-ga.png)

Die folgende Tabelle gibt Aufschluss über die Funktion der einzelnen Optionen:

| Richtlinie | Zustand Ein |
|----- |-----|
| System Updates | Ruft täglich eine Liste mit den verfügbaren Sicherheitsupdates und wichtigen Updates von Windows Update oder WSUS ab (je nachdem, welcher Dienst für diesen virtuellen Computer konfiguriert ist) und empfiehlt die Anwendung der fehlenden Updates. Führt eine Überprüfung der letzten Updates in Linux-Systemen durch, indem ein per Distribution bereitgestelltes Paketverwaltungssystem verwendet wird. Hiermit wird ermittelt, für welche Pakete Updates verfügbar sind. Außerdem werden Sicherheitsupdates und kritische Updates für virtuelle [Cloud Services](./cloud-services/cloud-services-how-to-configure.md)-Computer ermittelt. |
| Sicherheitsrisiken des Betriebssystems | Analysiert täglich Betriebssystemkonfigurationen, die bewirken können, dass die virtuellen Computer anfälliger für Angriffe werden, und empfiehlt Konfigurationsänderungen, um auf diese Sicherheitsrisiken zu reagieren. Weitere Informationen zu den speziellen Konfigurationen, die überwacht werden, finden Sie in der [Liste der empfohlenen Basisregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) (in englischer Sprache). |
| Endpoint Protection | Empfiehlt die Bereitstellung von Endpoint Protection für alle virtuellen Windows-Computer, um Viren, Spyware und andere Schadsoftware zu erkennen und zu entfernen.|
| Datenträgerverschlüsselung | Empfiehlt die Aktivierung der Datenträgerverschlüsselung auf allen virtuellen Computern, um den Datenschutz im Ruhezustand zu optimieren. 
| Netzwerksicherheitsgruppen | Empfiehlt die Konfiguration von [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) (NSGs), um den eingehenden und ausgehenden Datenverkehr für VMs mit öffentlichen Endpunkten zu steuern. NSGs, die für ein Subnetz konfiguriert sind, werden für alle Netzwerkschnittstellen der virtuellen Computer übernommen, sofern nichts anderes angegeben ist. Zusätzlich zur Überprüfung, ob eine NSG konfiguriert wurde, bewertet diese Option Sicherheitsregeln für eingehende Daten, um Regeln zu identifizieren, die jeglichen eingehenden Datenverkehr zulassen. |
| Web Application Firewall | Empfiehlt die Bereitstellung einer Web Application Firewall auf virtuellen Computern, wenn Folgendes zutrifft: [Öffentliche IP auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) wird verwendet, und die zugehörigen NSG-Sicherheitsregeln für eingehenden Datenverkehr sind so konfiguriert, dass Zugriff auf den Port 80/443 zugelassen wird. Es wird eine IP-Adresse mit Lastenausgleich (VIP) verwendet, und die zugehörigen Lastenausgleichs- und eingehenden NAT-Regeln sind so konfiguriert, dass Zugriff auf den Port 80/443 zugelassen wird (weitere Informationen finden Sie unter [Unterstützung des Azure-Ressourcen-Managers für Load Balancer](../load-balancer/load-balancer-arm.md)). |
| Firewall der nächsten Generation | Erweitert den Schutz von Netzwerken über die in Azure integrierten Netzwerksicherheitsgruppen hinaus. Security Center erkennt Bereitstellungen, für die eine Firewall der nächsten Generation empfohlen wird, und ermöglicht Ihnen die Bereitstellung eines virtuellen Geräts. |
| SQL-Überwachung | Empfiehlt, dass Überwachung des Zugriffs auf Azure SQL-Server und -Datenbanken zu Compliance-, erweiterten Erkennungs- und Untersuchungszwecken aktiviert wird. |
| Transparent Data Encryption für SQL | Empfiehlt, dass Verschlüsselung im Ruhezustand für Ihre Azure SQL-Datenbanken, die zugehörigen Sicherungen und die Transaktionsprotokolldateien aktiviert wird, sodass Ihre Daten selbst dann, wenn unberechtigt auf sie zugegriffen wird, nicht gelesen werden können. |
	
Klicken Sie nach Abschluss der Konfiguration aller Optionen auf dem Blatt **Sicherheitsrichtlinie**, das die Empfehlungen enthält, auf **OK**, und klicken Sie anschließend auf dem Blatt **Sicherheitsrichtlinie**, das die ursprünglichen Einstellungen enthält, auf **Speichern**.

## Festlegen von Sicherheitsrichtlinien für Ressourcengruppen

Wenn Sie Ihre Sicherheitsrichtlinien pro Ressourcengruppe konfigurieren möchten, gehen Sie dazu ähnlich vor wie beim Einrichten von Sicherheitsrichtlinien für Abonnements. Der Hauptunterschied besteht darin, dass Sie den Abonnementnamen erweitern und die Ressourcengruppe auswählen müssen, für die Sie die eindeutige Sicherheitsrichtlinie konfigurieren möchten:

![Auswahl der Ressourcengruppe](./media/security-center-policies/security-center-policies-fig5-ga.png)

Nach dem Auswählen der Ressourcengruppe wird das Blatt **Sicherheitsrichtlinie** geöffnet. Standardmäßig ist die Option **Vererbung** aktiviert. Das bedeutet, dass alle Sicherheitsrichtlinien für diese Ressourcengruppe von der Abonnementebene geerbt werden. Sie können diese Konfiguration ändern, falls Sie eine benutzerdefinierte Sicherheitsrichtlinie pro Ressourcengruppe festlegen möchten. In diesem Fall müssen Sie **Eindeutig** auswählen und entsprechende Änderungen für die Option **Prevention policy** (Präventionsrichtlinie) vornehmen.

![Sicherheitsrichtlinie pro Ressourcengruppe](./media/security-center-policies/security-center-policies-fig6-ga.png)

> [AZURE.NOTE] Bei einem Konflikt zwischen der Richtlinie auf Abonnementebene und der Richtlinie auf Ressourcengruppenebene hat die Richtlinie auf Ressourcenebene Vorrang.


## Siehe auch

In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

- [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md): Erfahren Sie, wie Sie Entwurfsüberlegungen zur Einführung von Azure Security Center planen und umsetzen können.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!---HONumber=AcomDC_0824_2016-->