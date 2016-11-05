---
title: Hinzufügen einer Firewall der nächsten Generation in Azure Security Center | Microsoft Docs
description: In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlungen **Firewall der nächsten Generation hinzufügen** und **Datenverkehr nur über NGFW routen** umsetzen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2016
ms.author: terrylan

---
# Hinzufügen einer Firewall der nächsten Generation in Azure Security Center
In Azure Security Center wird unter Umständen empfohlen, dass Sie zum Verbessern Ihrer Sicherheitsmaßnahmen eine Firewall der nächsten Generation (Next Generation Firewall, NGFW) eines Microsoft-Partners hinzufügen. In diesem Dokument sehen wir uns ein Beispiel zu dieser Vorgehensweise an.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.
> 
> 

## Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Firewall der nächsten Generation hinzufügen** aus. ![Hinzufügen einer Firewall der nächsten Generation][1]
2. Wählen Sie auf dem Blatt **Firewall der nächsten Generation hinzufügen** einen Endpunkt aus. ![Auswählen eines Endpunkts][2]
3. Ein zweites Blatt **Firewall der nächsten Generation hinzufügen** wird geöffnet. Sie können eine vorhandene Lösung verwenden, sofern verfügbar, oder eine neue erstellen. Bei diesem Beispiel sind keine vorhandenen Lösungen verfügbar. Daher erstellen wir eine neue Firewall der nächsten Generation. ![Erstellen einer neuen Firewall der nächsten Generation][3]
4. Wählen Sie zum Erstellen einer neuen Firewall der nächsten Generation in der Liste der angezeigten Partner eine Lösung aus. Bei diesem Beispiel wählen wir **Check Point**. ![Auswählen der Firewalllösung der nächsten Generation][4]
5. Auf dem eingeblendeten Blatt **Check Point** werden Informationen zu dieser Partnerlösung angezeigt. Klicken Sie auf dem Informationsblatt auf **Erstellen**. ![Blatt mit Firewallinformationen][5]
6. Das Blatt **Virtuellen Computer erstellen** wird geöffnet. Hier können Sie Informationen eingeben, die zum Einrichten des virtuellen Computers erforderlich sind, auf dem die Firewall der nächsten Generation ausgeführt wird. Führen Sie die Schritte aus, und geben Sie die für die Firewall der nächsten Generation erforderlichen Informationen ein. Klicken Sie zum Übernehmen auf „OK“. ![Erstellen eines virtuellen Computers zum Ausführen der Firewall der nächsten Generation][6]

## Datenverkehr nur durch Firewall der nächsten Generation leiten
Kehren Sie zum Blatt **Empfehlungen** zurück. Nachdem Sie über Security Center eine Firewall der nächsten Generation hinzugefügt haben, wurde ein neuer Eintrag namens **Datenverkehr nur über NGFW routen** generiert. Diese Empfehlung wird nur erstellt, wenn Sie die Firewall der nächsten Generation über Security Center installiert haben. Wenn Sie über Endpunkte mit Internetzugriff verfügen, empfiehlt Security Center die Konfiguration von Regeln für Netzwerksicherheitsgruppen, die erzwingen, dass eingehender Datenverkehr durch Ihre Firewall der nächsten Generation an Ihren virtuellen Computer geleitet wird.

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Datenverkehr nur über NGFW routen**. ![Datenverkehr nur durch Firewall der nächsten Generation leiten][7]
2. Daraufhin wird das Blatt **Datenverkehr nur über NGFW routen** geöffnet, das die virtuellen Computer enthält, an die Sie Datenverkehr weiterleiten können. Wählen Sie einen virtuellen Computer aus der Liste aus. ![Auswählen eines virtuellen Computers][8]
3. Ein Blatt für den ausgewählten virtuellen Computer wird geöffnet, auf dem dazugehörige Eingangsregeln angezeigt werden. In einer Beschreibung erhalten Sie weitere Informationen zu den möglichen nächsten Schritten. Wählen Sie **Eingangsregeln bearbeiten** aus, um mit dem Bearbeiten einer Eingangsregel fortzufahren. Es wird erwartet, dass **Quelle** für die Endpunkte mit Internetzugriff, die mit der Firewall der nächsten Generation verknüpft sind, nicht auf **Beliebig** festgelegt wird. Weitere Informationen zu den Eigenschaften der Eingangsregeln finden Sie unter [NSG-Regeln](../virtual-network/virtual-networks-nsg.md#nsg-rules). ![Konfigurieren von Regeln zum Begrenzen des Zugriffs][9] ![Eingangsregel bearbeiten][10]

## Weitere Informationen
In diesem Dokument wurde gezeigt, wie Sie die Security Center-Empfehlung „Firewall der nächsten Generation hinzufügen“ umsetzen. Weitere Informationen zu Firewalls der nächsten Generation und der Partnerlösung von Check Point finden Sie in den folgenden Artikeln:

* [Firewall der nächsten Generation](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png

<!---HONumber=AcomDC_0727_2016-->