---
title: Installieren von Endpoint Protection in Azure Security Center | Microsoft Docs
description: "In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung **Endpoint Protection installieren** implementieren."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/23/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: b8a69d89f335c00c5ddc3c201e33a66e1dea1da5
ms.openlocfilehash: 6ccb5367b1c7c6cae7b9d35b6a5471c8edfe6f85


---
# <a name="install-endpoint-protection-in-azure-security-center"></a>Installieren von Endpoint Protection in Azure Security Center
In Azure Security Center wird die Bereitstellung einer Antischadsoftware für Ihre virtuellen Azure-Computer empfohlen, sofern noch keine Antischadsoftware aktiviert ist. Diese Empfehlung gilt nur für virtuelle Windows-Computer. Diese Empfehlung überprüft gegenwärtig das Vorhandensein von Windows Defender oder TrendMicro Deep Security. In Zukunft sollen zusätzliche Endpoint Protection-Lösungen hinzugefügt werden.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Es ist keine schrittweise Anleitung.
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Endpoint Protection installieren** aus.
   ![„Endpoint Protection installieren“ auswählen][1]
2. Das Blatt **Endpoint Protection installieren** wird mit einer Liste virtueller Computer geöffnet, für die keine Antischadsoftware aktiviert ist. Wählen Sie in der Liste die virtuellen Computer aus, auf denen Antischadsoftware installiert werden soll, und klicken Sie auf **Install on VMs**(Auf virtuellen Computern installieren).
   ![Virtuelle Computer auswählen, auf denen Antischadsoftware installiert werden soll][2]
3. Das Blatt **Select Endpoint Protection** (Endpoint Protection auswählen) wird geöffnet, auf dem Sie die Antischadsoftware-Lösung auswählen können, die Sie verwenden möchten. Wählen wir in diesem Beispiel **Microsoft Antimalware**.
   ![Select Endpoint Protection][3]
4. Weitere Informationen zur Antischadsoftware-Lösung werden angezeigt. Klicken Sie auf **Erstellen**.
   ![Erstellen einer Antischadsoftware-Lösung][4]
5. Geben Sie die erforderlichen Konfigurationseinstellungen auf dem Blatt **Erweiterung hinzufügen** ein, und wählen Sie anschließend **OK** aus. Weitere Informationen zu den Konfigurationseinstellungen finden Sie unter [Standardmäßige und benutzerdefinierte Antimalware-Konfiguration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../security/azure-security-antimalware.md) ist jetzt auf den ausgewählten VMs aktiv.

## <a name="see-also"></a>Siehe auch
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Endpoint Protection installieren“ implementieren. Weitere Informationen zum Aktivieren von Antischadsoftware finden Sie in den folgenden Artikeln:

* [Microsoft Antimalware for Cloud Services and Virtual Machine (Microsoft Antischadsoftware für Cloud Services und virtuelle Computer):](../security/azure-security-antimalware.md) Erfahren Sie mehr zur Bereitstellung von Microsoft Antischadsoftware.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitsrichtlinien konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png



<!--HONumber=Nov16_HO4-->


