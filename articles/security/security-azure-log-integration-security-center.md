---
title: Azure-Protokollintegration mit Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Security Center-Warnungen abrufen, die mit Protokollintegration arbeiten.
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 03/22/2017
ms.author: barclayn
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: ba97d1a98b72c5afc98bbdf6c81b0ec19f338e74
ms.lasthandoff: 04/13/2017


---

# <a name="how-to-get-your-security-center-alerts-in-azure-log-integration"></a>Abrufen von Azure Security Center-Warnungen bei der Azure-Protokollintegration
Dieser Artikel enthält die erforderlichen Schritte zum Aktivieren des Azure-Protokollintegrationsdiensts, um Sicherheitshinweisinformationen abzurufen, die vom Azure Security Center generiert wurden. Sie müssen die Schritte im Artikel [Erste Schritte](security-azure-log-integration-get-started.md) abgeschlossen haben, bevor Sie die Schritte in diesem Artikel durchführen können.

## <a name="detailed-steps"></a>Ausführliche Schritte
Die folgenden Schritte erstellen den erforderlichen Azure Active Directory-Dienstprinzipal und weisen dem Dienstprinzipal Leseberechtigungen für das Abonnement zu:
1. Öffnen Sie die Eingabeaufforderung, und navigieren Sie zu **c:\Programme\Microsoft Azure Log Integration**.
2. Führen Sie den Befehl ``azlog createazureid`` aus.

    Dabei werden Sie zur Angabe Ihrer Azure-Anmeldeinformationen aufgefordert. Anschließend wird in den Azure AD-Mandanten, die die Azure-Abonnements hosten, in denen der angemeldete Benutzer als Administrator, Co-Administrator oder Besitzer fungiert, ein [Azure Active Directory-Dienstprinzipal](../active-directory/develop/active-directory-application-objects.md) erstellt. Handelt es sich bei dem angemeldeten Benutzer nur um einen Gastbenutzer des Azure AD-Mandanten, tritt bei dem Befehl ein Fehler auf. Die Authentifizierung gegenüber Azure wird über Azure Active Directory (AD) durchgeführt. Beim Erstellen eines Dienstprinzipals für die Azure-Protokollintegration wird die Azure AD-Identität erstellt, die Lesezugriff auf Azure-Abonnements erhält.

2. Als Nächstes führen Sie einen Befehl aus, der dem in Schritt 2 erstellten Dienstprinzipal Lesezugriff auf das Abonnement zuweist. Ohne Angabe einer Abonnement-ID versucht der Befehl, die Dienstprinzipal-Leserrolle für alle Abonnements zuzuweisen, auf die Sie Zugriff haben. </br></br>
``azlog authorize <SubscriptionID>`` </br> für Beispiel </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    Wenn Sie den Befehl „authorize“ unmittelbar nach dem Befehl „createazureid“ ausführen, werden unter Umständen Warnungen angezeigt. Nach der Erstellung des Azure AD-Kontos dauert es etwas, bis das Konto verwendungsbereit ist. Wenn Sie nach dem Ausführen des Befehls „createazureid“ ca. 60 Sekunden warten, um den Befehl „authorize“ auszuführen, sollten diese Warnungen nicht angezeigt werden.

4. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass die JSON-Dateien des Überwachungsprotokolls vorhanden sind:
 * **c:\Users\azlog\AzureResourceManagerJson**
 * **c:\Users\azlog\AzureResourceManagerJsonLD** </br></br>
5. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass darin bereits Security Center-Warnungen vorhanden sind:</br></br>
 * **c:\Users\azlog\AzureSecurityCenterJson**
 * **c:\Users\azlog\AzureSecurityCenterJsonLD** </br></br>

Wenn während der Installation und Konfiguration Probleme auftreten, öffnen Sie eine [Supportanfrage](/azure-supportability/how-to-create-azure-support-request.md), und wählen Sie **Protokollintegration** als den Dienst aus, für den Sie Support anfordern.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-Protokollintegration finden Sie in folgenden Dokumenten:

* [Microsoft Azure Log Integration for Azure logs (Microsoft Azure-Protokoll-Integration für Azure-Protokolle)](https://www.microsoft.com/download/details.aspx?id=53324): Download Center für Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
* [Introduction to Azure log integration](security-azure-log-integration-overview.md) (Einführung in die Azure-Protokollintegration): Dieses Dokument stellt die Azure-Protokollintegration, die wichtigsten Funktionen sowie die Funktionsweise vor.
* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.
* [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
* [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)](../security-center/security-center-integrating-alerts-with-log-integration.md) : In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.
* [New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Neue Features für Azure-Diagnose und Azure-Überwachungsprotokolle): In diesem Blogbeitrag werden Azure-Überwachungsprotokolle und andere Features vorgestellt, mit denen Sie sich einen Einblick in die Vorgänge Ihrer Azure-Ressourcen verschaffen können.

