<properties
   pageTitle="Aktivieren des VM-Agents in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „VM-Agent aktivieren“ umsetzen."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/20/2016"
   ms.author="terrylan"/>

# Aktivieren des VM-Agents in Azure Security Center

Der VM-Agent muss auf virtuellen Maschinen (VMs) installiert werden, um das Überprüfen von Patches und Grundregeln sowie Antischadsoftware bereitzustellen. Im Azure Security Center können Sie prüfen, welche VMs den VM-Agent benötigen. Es wird eine Empfehlung angezeigt, den VM-Agent auf diesen VMs zu aktivieren.

Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Der Artikel [VM-Agent und Erweiterungen – Teil 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) enthält Informationen zum Installieren des VM-Agents.


> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **VM-Agent aktivieren** aus. ![VM-Agent aktivieren][1]

2. Daraufhin wird das Blatt **VM-Agent fehlt oder reagiert nicht** geöffnet. Auf diesem Blatt sind die VMs aufgeführt, die den VM-Agent erfordern. Befolgen Sie die Anweisungen auf dem Blatt, um den VM-Agent zu installieren. ![VM-Agent fehlt][2]

## Weitere Informationen

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png

<!---HONumber=AcomDC_0720_2016-->