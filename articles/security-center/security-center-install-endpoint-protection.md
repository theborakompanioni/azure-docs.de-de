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
ms.date: 06/16/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: efb86a0ae362c30a6772c391a499154b7ae2a697
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="install-endpoint-protection-in-azure-security-center"></a>Installieren von Endpoint Protection in Azure Security Center
Azure Security Center empfiehlt die Installation von Endpoint Protection auf Ihren virtuellen Azure-Computern (VMs), sofern noch keine aktiviert ist. Diese Empfehlung gilt nur für virtuelle Windows-Computer.

> [!NOTE]
> In diesem Beispielszenario wird Antischadsoftware von Microsoft verwendet. Unter [Partner Integration in Azure Security Center (Partnerintegration in Azure Security Center)](security-center-partner-integration.md#partners-that-integrate-with-security-center) finden Sie eine Liste der Partner, die in Security Center integriert sind.  
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option** Endpoint Protection installieren **aus.
   ![„Endpoint Protection installieren“ auswählen][1]
2. Das Blatt **Endpoint Protection installieren** zeigt eine Liste virtueller Computer an, die nicht über Endpoint Protection verfügen. Wählen Sie die virtuellen Computer aus der Liste aus, auf denen Endpoint Protection installiert werden soll, und klicken Sie auf **Install on VMs** (Auf virtuellen Computern installieren).
   ![Auswählen virtueller Computer, auf denen Endpoint Protection installiert werden soll][2]
3. Über das Blatt **Endpoint Protection auswählen** können Sie die Lösung zur Endpoint Protection auswählen, die Sie verwenden möchten. Wählen wir in diesem Beispiel **Microsoft Antimalware**.
   ![Select Endpoint Protection][3]
4. Weitere Informationen zur Endpoint Protection-Lösung werden angezeigt. Klicken Sie auf **Erstellen**.
   ![Erstellen einer Antischadsoftware-Lösung][4]
5. Geben Sie die erforderlichen Konfigurationseinstellungen auf dem Blatt **Erweiterung hinzufügen** ein, und wählen Sie anschließend **OK** aus. Weitere Informationen zu den Konfigurationseinstellungen finden Sie unter [Standardmäßige und benutzerdefinierte Antimalware-Konfiguration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../security/azure-security-antimalware.md) ist jetzt auf den ausgewählten VMs aktiv.

## <a name="see-also"></a>Siehe auch
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Endpoint Protection installieren“ implementieren. Weitere Informationen zum Aktivieren von Antischadsoftware von Microsoft finden Sie in den folgenden Dokumenten:

* [Microsoft Antimalware for Cloud Services and Virtual Machine (Antischadsoftware von Microsoft für Cloud Services und virtuelle Computer)](../security/azure-security-antimalware.md): Erfahren Sie mehr zur Bereitstellung von Antischadsoftware von Microsoft.

Weitere Informationen zu Security Center finden Sie in den folgenden Dokumenten:

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

