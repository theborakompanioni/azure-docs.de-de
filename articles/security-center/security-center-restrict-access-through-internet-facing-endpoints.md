<properties
   pageTitle="Einschränken des Zugriffs über Endpunkte mit Internetzugriff in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird gezeigt, wie Sie die Security Center-Empfehlung „Zugriff über Endpunkt mit Internetzugriff einschränken“ umsetzen."
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
   ms.date="07/26/2016"
   ms.author="terrylan"/>

# Einschränken des Zugriffs über Endpunkte mit Internetzugriff in Azure Security Center

Azure Security Center empfiehlt, den Zugriff über Endpunkte mit Internetzugriff einzuschränken, wenn für beliebige Ihrer Netzwerksicherheitsgruppen (NSGs) mindestens eine Eingangsregel gilt, die den Zugriff über „alle“ Quell-IP-Adressen zulässt. Durch das Öffnen des Zugriffs für alle Quell-IP-Adressen können Angreifer ggf. auf Ihre Ressourcen zugreifen. Security Center empfiehlt, dass Sie diese Eingangsregeln so ändern, dass der Zugriff auf Quell-IP-Adressen eingeschränkt wird, die tatsächlich Zugriff benötigen.

Diese Empfehlung wird für alle Ports generiert, bei denen es sich nicht um Webports handelt und die „Beliebig“ als Quelle haben.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Zugriff über Endpunkt mit Internetzugriff einschränken** aus. ![Zugriff über Endpunkt mit Internetzugriff einschränken][1]

2. Hierdurch wird das Blatt **Zugriff über Endpunkt mit Internetzugriff einschränken** geöffnet. Dieses Blatt listet die virtuellen Computer (VMs) mit Eingangsregeln auf, die ein potenzielles Sicherheitsproblem darstellen. Wählen Sie einen virtuellen Computer aus. ![Auswählen eines virtuellen Computers][2]

3. Auf dem Blatt **NSG** werden Informationen zur Netzwerksicherheitsgruppe, geltende Eingangsregeln und der zugehörige virtuelle Computer angezeigt. Wählen Sie **Eingangsregeln bearbeiten** aus, um mit dem Bearbeiten einer Eingangsregel fortzufahren. ![Blatt „Netzwerksicherheitsgruppe“][3]

4. Wählen Sie auf dem Blatt **Eingangssicherheitsregeln** die zu bearbeitende Eingangsregel aus. In diesem Beispiel wählen wir **AllowWeb** aus. ![Eingangssicherheitsregeln][4]

  Sie können auch **Standardregeln** auswählen, um die Standardregeln anzuzeigen, die in allen NSGs enthalten sind. Die Standardregeln können zwar nicht gelöscht werden, haben aber eine niedrigere Priorität und können somit durch selbst erstellte Regeln außer Kraft gesetzt werden. Erfahren Sie mehr über [Standardregeln](../virtual-network/ virtual-networks-nsg.md#default-rules). ![Standardregeln][5]

5. Bearbeiten Sie auf dem Blatt **AllowWeb** die Eigenschaften der Eingangsregel, sodass die **Quelle** eine IP-Adresse oder ein Block von IP-Adressen ist. Weitere Informationen zu den Eigenschaften von Eingangsregeln finden Sie unter [NSG-Regeln](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Eingangsregel bearbeiten][6]

## Weitere Informationen

In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Zugriff über Endpunkt mit Internetzugriff einschränken“ umsetzen. Weitere Informationen zu NSGs und Regeln finden Sie in den folgenden Artikeln:

- [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Verwalten von NSGs mithilfe des Azure-Portals](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png

<!---HONumber=AcomDC_0727_2016-->