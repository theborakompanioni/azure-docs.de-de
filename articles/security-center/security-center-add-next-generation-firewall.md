<properties
   pageTitle="Hinzufügen einer Firewall der nächsten Generation in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Firewall der nächsten Generation hinzufügen“ umsetzen."
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
   ms.date="07/15/2016"
   ms.author="terrylan"/>

# Hinzufügen einer Firewall der nächsten Generation in Azure Security Center

In Azure Security Center wird unter Umständen empfohlen, dass Sie zum Verbessern Ihrer Sicherheitsmaßnahmen eine Firewall der nächsten Generation eines Microsoft-Partners hinzufügen. In diesem Dokument sehen wir uns ein Beispiel zu dieser Vorgehensweise an.

> [AZURE.NOTE] Die Informationen in diesem Dokument gelten für die Vorschauversion von Azure Security Center. Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Firewall der nächsten Generation hinzufügen** aus. ![Hinzufügen einer Firewall der nächsten Generation][1]

2. Wählen Sie auf dem Blatt **Firewall der nächsten Generation hinzufügen** einen Endpunkt aus. ![Auswählen eines Endpunkts][2]

3. Ein zweites Blatt **Firewall der nächsten Generation hinzufügen** wird geöffnet. Sie können eine vorhandene Lösung verwenden, sofern verfügbar, oder eine neue erstellen. Bei diesem Beispiel sind keine vorhandenen Lösungen verfügbar. Daher erstellen wir eine neue Firewall der nächsten Generation. ![Erstellen einer neuen Firewall der nächsten Generation][3]

4. Wählen Sie zum Erstellen einer neuen Firewall der nächsten Generation in der Liste der angezeigten Partner eine Lösung aus. Bei diesem Beispiel wählen wir **Check Point**. ![Auswählen der Firewalllösung der nächsten Generation][4]

5. Auf dem eingeblendeten Blatt **Check Point** werden Informationen zu dieser Partnerlösung angezeigt. Klicken Sie auf dem Informationsblatt auf **Erstellen**. ![Blatt mit Firewallinformationen][5]

6. Das Blatt **Virtuellen Computer erstellen** wird geöffnet. Hier können Sie Informationen eingeben, die erforderlich sind, um den virtuellen Computer einzurichten, auf dem die Firewall der nächsten Generation ausgeführt wird. Führen Sie die Schritte aus, und geben Sie die für die Firewall der nächsten Generation erforderlichen Informationen ein. Klicken Sie zum Übernehmen auf „OK“. ![Erstellen eines virtuellen Computers zum Ausführen der Firewall der nächsten Generation][6]

## Nächste Schritte

In diesem Dokument wurde gezeigt, wie Sie die Security Center-Empfehlung „Firewall der nächsten Generation hinzufügen“ umsetzen. Weitere Informationen zu Firewalls der nächsten Generation und der Partnerlösung von Check Point finden Sie in den folgenden Artikeln:

- [Firewall der nächsten Generation](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitsrichtlinien konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png

<!---HONumber=AcomDC_0720_2016-->