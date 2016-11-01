<properties
   pageTitle="Aktivieren von Netzwerksicherheitsgruppen in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument erfahren Sie, wie Sie die von Azure Security Center empfohlene Aktivierung von Netzwerksicherheitsgruppen implementieren."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# Aktivieren von Netzwerksicherheitsgruppen in Azure Security Center

Azure Security Center empfiehlt die Aktivierung einer Netzwerksicherheitsgruppe (NSG), sofern noch keine aktiviert ist. NSGs enthalten eine Zugriffssteuerungsliste (Access Control List, ACL) zum Zulassen oder Verweigern von eingehendem Netzwerkdatenverkehr für Ihre VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnetzen oder einzelnen VM-Instanzen innerhalb dieses Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VM-Instanzen in diesem Subnetz. Darüber hinaus kann Datenverkehr zu einer einzelnen virtuellen Maschine weiter beschränkt werden, indem eine NSG direkt diesem virtuellen Computer zugewiesen wird. Weitere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md).

Sollten keine NSGs aktiviert sein, stellt Security Center zwei Empfehlungen bereit: „Netzwerksicherheitsgruppen in Subnetzen aktivieren“ und „Netzwerksicherheitsgruppen auf virtuellen Computern aktivieren“. Sie haben die Wahl, auf welcher Ebene (Subnetz oder virtueller Computer) Sie NSGs anwenden möchten.


> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** aus, ob Netzwerksicherheitsgruppen in Subnetzen oder auf virtuellen Computern aktiviert werden sollen. ![Aktivieren von Netzwerksicherheitsgruppen][1]

2. Daraufhin wird das Blatt zum Konfigurieren fehlender Netzwerksicherheitsgruppen für Subnetze oder virtuelle Computer geöffnet (je nachdem, welche Empfehlung Sie ausgewählt haben). Wählen Sie ein Subnetz oder einen virtuellen Computer aus, um eine NSG dafür zu konfigurieren.

  ![Konfigurieren der NSG (Subnetz)][2]

  ![Konfigurieren der NSG (virtueller Computer)][3]
3. Wählen Sie auf dem Blatt **Netzwerksicherheitsgruppe auswählen** eine vorhandene NSG oder die Option zum Erstellen einer neuen NSG aus.

  ![Auswählen der Netzwerksicherheitsgruppe][4]

Falls Sie eine neue NSG erstellen möchten, führen Sie die Schritte unter [Verwalten von NSGs mithilfe des Azure-Portals](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) aus, um eine NSG zu erstellen und Sicherheitsregeln festzulegen.

## Weitere Informationen

In diesem Artikel haben Sie erfahren, wie Sie die von Security Center empfohlene Aktivierung von Netzwerksicherheitsgruppen für Subnetze oder virtuelle Computer implementieren. Weitere Informationen zu NSGs finden Sie hier:

- [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Verwalten von NSGs mithilfe des Azure-Portals](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md): Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie sicherheitsbezogene Neuigkeiten und Informationen zu Azure.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]: ./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png

<!---HONumber=AcomDC_0803_2016-->