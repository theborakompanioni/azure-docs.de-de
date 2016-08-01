<properties
   pageTitle="Weiterleiten von Datenverkehr ausschließlich durch eine Firewall der nächsten Generation in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Datenverkehr nur durch Firewall der nächsten Generation leiten“ umsetzen."
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

# Weiterleiten von Datenverkehr ausschließlich durch eine Firewall der nächsten Generation in Azure Security Center

Das Azure Security Center kann erkennen, ob Sie eine Firewall der nächsten Generation bereitgestellt haben. Wenn Sie über Endpunkte mit Internetzugriff verfügen, empfiehlt Azure Security Center die Konfiguration von Regeln für Netzwerksicherheitsgruppen, die erzwingen, dass eingehender Datenverkehr durch Ihre Firewall der nächsten Generation zu Ihrem virtuellen Computer (VM) geleitet wird.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Datenverkehr nur durch Firewall der nächsten Generation weiterleiten**. ![Datenverkehr nur durch Firewall der nächsten Generation leiten][1]

2. Daraufhin wird das Blatt **Datenverkehr nur durch Firewall der nächsten Generation weiterleiten** geöffnet, das VMs enthält, zu denen Sie Datenverkehr weiterleiten können. Wählen Sie in der Liste eine VM aus. ![Auswählen eines virtuellen Computers][2]

3. Ein Blatt für den ausgewählten virtuellen Computer wird geöffnet, auf dem dazugehörige Eingangsregeln angezeigt werden. In einer Beschreibung erhalten Sie weitere Informationen zu den möglichen nächsten Schritten. ![Konfigurieren von Regeln zum Begrenzen des Zugriffs][3]

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
[1]: ./media/security-center-route-traffic-through-ngfw/route-traffic-through-ngfw.png
[2]: ./media/security-center-route-traffic-through-ngfw/select-vm.png
[3]: ./media/security-center-route-traffic-through-ngfw/configure-rules-to-limit-access.png

<!---HONumber=AcomDC_0720_2016-->