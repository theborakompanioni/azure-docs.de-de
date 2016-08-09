<properties
   pageTitle="Installieren von Endpoint Protection in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung **Endpoint Protection installieren** implementieren."
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
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# Installieren von Endpoint Protection in Azure Security Center

In Azure Security Center wird die Bereitstellung einer Antischadsoftware für Ihre virtuellen Azure-Computer empfohlen, sofern noch keine Antischadsoftware aktiviert ist. Diese Empfehlung gilt nur für virtuelle Windows-Computer.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Endpoint Protection installieren**. ![„Endpoint Protection installieren“ auswählen][1]

2. Das Blatt **Endpoint Protection installieren** wird mit einer Liste virtueller Computer geöffnet, für die keine Antischadsoftware aktiviert ist. Wählen Sie in der Liste die virtuellen Computer aus, auf denen Antischadsoftware installiert werden soll, und klicken Sie auf **Install on VMs** (Auf virtuellen Computern installieren). ![Virtuelle Computer auswählen, auf denen Antischadsoftware installiert werden soll][2]

3. Das Blatt **Select Endpoint Protection** (Endpoint Protection auswählen) wird geöffnet, auf dem Sie die Antischadsoftware-Lösung auswählen können, die Sie verwenden möchten. Wählen wir in diesem Beispiel **Microsoft Antimalware**. ![Endpoint Protection auswählen][3]

4. Weitere Informationen zur Antischadsoftware-Lösung werden angezeigt. Klicken Sie auf **Erstellen**. ![Erstellen einer Antischadsoftware-Lösung][4]

5. Geben Sie die erforderlichen Konfigurationseinstellungen auf dem Blatt **Erweiterung hinzufügen** ein, und wählen Sie anschließend **OK**. Weitere Informationen zu den Konfigurationseinstellungen finden Sie unter [Standardmäßige und benutzerdefinierte Antimalware-Konfiguration](../azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) ist jetzt auf den ausgewählten virtuellen Computern aktiv.

## Weitere Informationen

In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Endpoint Protection installieren“ implementieren. Weitere Informationen zum Aktivieren von Antischadsoftware finden Sie in den folgenden Artikeln:

- [Microsoft Antimalware für Azure Cloud Services und Virtual Machines:](../azure-security-antimalware.md) Informationen zur Bereitstellung von Microsoft Antimalware.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitsrichtlinien konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]: ./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]: ./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]: ./media/security-center-install-endpoint-protection/create-antimalware-solution.png

<!---HONumber=AcomDC_0727_2016-->